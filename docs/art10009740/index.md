---
vid: '10009740'
title: Persistence and Shadows
authors:
- Stephen Taylor
byline: 'by Stephen Taylor (email: sjt@lambenttechnology.com)'
volume: '18'
issue: '2'
page: '110'
source: trad/v182/pers182.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2001
tags:
- language design
---

*by Stephen Taylor (email: [sjt@lambenttechnology.com](mailto:sjt@lambenttechnology.com))*
{ .byline }

*\[Ed: This paper uses the APL2741 font, which you can [download](resource/apl2741.zip)\]*.

## Abstract

Two changes are proposed to the definition of functions: that local objects might retain their values when a function leaves the execution stack, and that names might be shadowed by default rather than by declaration. These changes would replicate and extend the facilities offered in Dyalog APL by namespaces, context switching and `⎕PATH`, and so make redundant the extra scoping rules introduced by them. Close similarities are identified between workspaces and functions so defined.

## Introduction

This paper is about workspace logistics: how one organises communications between the different parts of a software system. I use the term *logistics* because I want to draw a military analogy. Military commanders try to keep their lines of communication short. So do good programmers.

Avoiding global variables is an example of this. Reading and setting global variables is a long line of communication when many functions are suspended on the execution stack. Measuring distance in stack levels corresponds to our intuition that global variables are ‘far away’ from the evaluated code line. When communication paths are long it is harder for the human reader to see what is passing through the global variables. In contrast information passed between functions as arguments and results is ‘close to’ the evaluation. Short lines of communication help soldiers and programmers understand and control what is happening.

Logistics suggests a related issue: *mobility*. Software is mobile when it is usable in many environments. Mobile software is easy to isolate and test.

The most mobile functions refer to nothing but their arguments and results. This tempts us to write large and monolithic functions, contrary to our good practice of dividing code into short, independently testable functions. The desire to preserve the mobility of sets of related functions has produced over the years both code management systems and extensions to the language, such as packages (SHARP APL) and namespaces (Dyalog APL).

## Public and Private Objects

Objects in a workspace[<sup>1</sup>](#note1) can be divided into two distinct classes according to how confidently one can predict the contents of the execution stack when the object’s name is resolved.

With some objects one has no idea what the execution stack will look like when reference is made to the object. Utility functions belong to this class. That is the nature of utility functions: one cannot say in advance which other functions will find them useful.

With other objects one can predict very well what the execution stack will look like when reference is made. For example, one function performs part of the work of another, and is called only when the other is already on the stack. We call such a function a *subfunction*, and regard it informally as owned by its calling function. Most commonly, no other object will ever refer to it.

Let us distinguish the first class of objects as *public*, and the second as *private*. We are distinguishing them by the sets of other objects that refer to them.

The issue of mobility occurs differently for public and private objects. Public objects must always be around. They form the environment of the software system. Private objects are subject to management. They can be divided into *modules* individuated by how much of the execution stack they share when called.

The focus of this paper is the mobility of modules. We are thus particularly concerned with private objects.

## Persistence

I want to distinguish two properties of local objects that have so far always been treated as the same property.

The names of local objects are localised by a function on the stack; the objects thus become local *to* that function. When the function leaves the stack, the name-value pairings for these names disappear; the objects can be referred to only when the function is on the execution stack; this is what we mean by their property of *locality*.

Now I want to distinguish from locality the property of *persistence*, which can have two values: *persistent* or *ephemeral*. A local object is ephemeral if, when its function is placed on the stack, the local object is undefined. This is how local objects always have behaved. Local objects have always been ephemeral.

A persistent local object would differ in this respect. When its localising function is placed on the stack, the local object is already defined. The name-value pairing would be the same as when the localising function last left the stack.

What would this buy us?

### Predefined constants, default results

Much function code consists of lines that define variables as constants, look-up tables and so on for use by later lines. We can now save these variables inside the functions that use them. No function lines need to be executed to produce them. Functions become clearer to read and faster to execute.

What would this look like? Suppose that we used in the function header colons instead of semicolons to declare persistent local variables. A function that begins:

```
[0]   left FUNCTION right;a;b;c:LookUpTable
[1]   a ← LookUpTable[right]
[2]   ...
```

would suspend in \[1\] on a value error at the first reference to `LookUpTable`. `LookUpTable` can then be defined and execution resumed. `LookUpTable` will retain its value on subsequent calls of `FUNCTION`.

Many functions examine their arguments for certain conditions in which they will return a default result, otherwise unrelated to the arguments. This default result can now be stored inside the function rather than evaluated each time.

### Organising subfunctions

Persistent local objects allow us to bind private functions to the functions that call them. They then appear only when their parent is on the stack and disappear afterwards. No code-management, path setting or references to namespaces is required. Consider a function `CONTROLLER` whose work is divided between three subfunctions. Define it as follows:

```
[0]   CONTROLLER:SUBFN1:SUBFN2:SUBFN3
[1]   SUBFN1
[2]   SUBFN2
[3]   SUBFN3
```

Call it: it suspends on `[1]` as `SUBFN1` is undefined. Define `SUBFN1`, `SUBFN2` and `SUBFN3`. As soon as `CONTROLLER` leaves the stack, the three subfunctions disappear, to reappear whenever `CONTROLLER` is next executed – exactly and only when they are wanted.

When a function is assigned or otherwise copied, all its private objects come with it by default. No explicit management is required.

### Functions can emulate objects, with their own memories

We considered above using persistent local variables as constants. But nothing precludes assignment during execution. For example:

```
[0]   HOWMANYTIMES:Counter
[1]   'You have now done this',(⍕Counter←Counter+1),' times'
```

Consider a subsystem that prepares a document for printing. Many parameters control the result: e.g. page size, margins, footer and header text. These, and objects like the text to be printed, can be conveniently managed in a collection of variables, or file components, ready for spooling or printing. But this is not an arrangement that lends itself easily to replication. In contrast, imagine the process managed by a function with persistent local objects.

```
[0]   DOCUMENT instruction:PageSize:Margins:Header:Footer:Text
[1]   :Select 1⊃instruction
[2]   :Case 'PAGESIZE'
[3]     PageSize←2⊃instruction
[4]   :Case 'MARGINS'
[5]     Margins←2⊃instruction
[6]   :Case 'HEADER'
[7]     Header←2⊃instruction
[8]   :Case 'FOOTER'
[9]     Footer←2⊃instruction
[10]  :Case 'TEXT'
[11]    Text←2⊃instruction
[12]  :Case 'APPEND'
[13]    Text,←2⊃instruction
[14]  :Case 'INSERT'
[15]    Text←(2⊃instruction),Text
[16]  :Case 'PRINT'
[17]    (PageSize Margins)SPOOL Text Header Footer
[18]  :Case 'EMAIL'
[19]    (2⊃instruction)EMAIL Text Header Footer PageSize
[20]  :EndSelect
```

Now the current state of the document is completely represented by the mobile function `DOCUMENT`. We can trivially make copies at any time. When we do so, everything required comes with the function. Template functions, specialised versions of `DOCUMENT`, can have appropriate initial values for their local variables.[<sup>2</sup>](#note2)

```
        Letter ← A4Letterhead ⍝ take a copy of the template
        Letter 'ADDRESS' Correspondent[1 2 3]
        Letter 'TEXT' standardLetterText
        Envelope ← C4ENVELOPE ⍝ take a blank envelope
        Envelope 'ADDRESS' Correspondent[1 2 3]
        Letter 'PRINT'
        Envelope 'PRINT'
        (Email ← Letter) 'PAGESIZE' 'US Letter'
        Email 'EMAIL' 'mailbox@lambenttechnology.com'
```

These functions behave like objects in languages like Smalltalk-80: they contain both data and the methods needed to manipulate them. Notice how casually multiple documents can be handled and consider how less conveniently the same objects would be represented using files or reserved globals.

## Shadowing by Default

Had APL been designed by a programmer rather than a mathematician, the convention for declaring local names might have been the reverse of what it is.

Conventionally, one declares in a function’s header all the names which are to be local; that is, *shadowed*. Reversing the convention, one would declare the names which are to be external, that is, *not shadowed*. (In terms of the length of communication paths, one would declare which names refer to distant objects.) The function header would thus show all the communication paths in and out of the function: through the arguments and through the named external objects.[<sup>3</sup>](#note3)

Either persistent objects or default shadowing could be implemented without the other. Combining them, however, produces surprising results.

### Namespaces and functions

Namespaces in Dyalog APL already exhibit some of the properties discussed. The objects in a namespace are both *local* to it and *persistent*. They can be referred to only when a function local to the namespace has been placed on the stack. The namespace does not have its own entry on the stack, but once implicitly there (by virtue of one of its functions being executed) it shadows all names.

This makes a namespace a good owner for private functions which do not refer to external objects, and for monolithic public functions (for example, utility functions that refer only to their arguments) ; but a poor owner for public functions that do make external references.

Compare a namespace `NS` containing a function `SUB`, with a function `FN` with the same subfunction defined as a persistent local object.

```
[0]   FN expression:SUB
[1]   ⍎expression
```

Then

```
#.NS.SUB 100
```

is equivalent to

```
FN 'SUB 100'
```

Let the function `FN` and the namespace `NS` shadow all external names by default. `FN` could claim two advantages over `NS`:

- The shadow cast by `NS` is impenetrable: `NS.SUB` can refer only to objects local to `NS`. In contrast, `FN.SUB` can be given selective access to objects outside `FN`; declarations in the headers of `FN` and `SUB` would suffice.
- The selective access above could be made general by use of a wildcard for declaring external references. Thus, in the reversed convention, if `;EXT` in a function header declares `EXT` to refer to an external object, then `;*` would declare *all undeclared names* to refer to external objects. Correspondingly, `:*` would declare all references to be to persistent local objects.

### Scoping rules

The scoping rules used by `FN` are more flexible than those that cover `NS`; they permit more control. Arguably, they are also a narrower extension of classic APL scoping rules.

In Dyalog APL there are three sets of scoping rules, which together determine how names are resolved:

- classic APL scoping rules;
- rules for namespaces of functions called directly from their namespaces;
- rules to cover the effects of `⎕PATH` and `⎕CS`, at least one of which (the use of `↑` in `⎕PATH`) exists to pierce the shadows cast by namespaces.

By executing `FN` and its private function on the stack instead of executing a function in its namespace, we reduce the sets of scoping rules from three to two.

By putting `FN` on the stack instead of setting `⎕PATH`, we collapse the scoping rules to just those for functions, plus the variations proposed here. Instead of listing namespaces in `⎕PATH`, one would invoke corresponding functions like `FN`. The execution stack would grow a trifle deeper, but the maintenance programmer would more easily trace references to objects distant from the evaluation, and use only one set of rules in doing so.

### Functions and workspaces

From here it is easy to see strong similarities between a workspace and a suspended function.

- A workspace is equivalent to a suspended niladic function with names shadowed by default and all local objects persistent.
- The lines of the function correspond to `⎕LX`.
- Where arrangements exist to start a workspace and evaluate an expression in it, this corresponds to invoking a monadic version of the function and evaluating its right argument, as the function `FN` above.
- Exiting the function is equivalent to `)CONTINUE`.

## Conclusion

Two small changes in function definition – allowing local objects to persist, and names to be shadowed by default – combine to replace and extend the facilities offered by namespaces, `⎕PATH` and context switching. Workspaces are seen to be special cases of suspended functions so defined.

<b>Footnotes</b>

<ol>
  <a name="note1"><li>I am using object here indifferently to denote variables, defined functions and operators; not to refer specifically to Dyalog APL namespaces.
  <a name="note2"><li>This fails to take account of the use of shallow reference in Dyalog APL.
  <a name="note3"><li>This applies equally to functions and variables. A function’s header would list all the external objects referred to by it or by its subfunctions.
</li></a></li></a></li></a></ol>

***
