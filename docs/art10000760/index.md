---
vid: '10000760'
title: SHARP APL vs APL2 Coding Style
authors:
- Dan Baronet
volume: '16'
issue: '2'
page: '87'
source: trad/v162/dan.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1999
tags:
- language design
- implementations
---

Note that this article contains quite a bit of APL code - this is in APL2741 font, [downloadable](resource/apl2741.zip) from this site (32K). There is a new problem in showing the \{match\} symbol in IE5 (it is treated as an optional hyphen), so it has now been duplicated in the font at position ch(166) which will paste into APL\*PLUS as the split-stile character. If you already have the font, please check the build date (double-click the font in your fonts folder) and re-install it if you don't have the October 29th 1999 version.

## SHARP APL vs. APL2 coding style

*by Dan Baronet ([danb@dsuper.net](mailto:danb@dsuper.net))*

## Introduction

Following Soliton’s announcement to unleash their APL into the Linux community I decided to review the differences between SHARP APL and APL2 style APLs (IBM’s APL2, Dyalog, APL+Win). I have divided the report into several articles, each with a different topic.

This article is the first of the series. It should be read before any of the others.

Not having a current version of IBM’s APL2 on hand I have been making my observations based on Dyalog’s APL using  `⎕ml=2`, which, like APL+Win, follows APL2 closely.

When reading this text keep in mind the date it was written (September 1999) as languages change and may render this information incorrect in the future.

## Conventions

In the text *italicized* words have a special meaning and often precede their definition.

*SAPL* refers to SHARP APL, including *SAM* which is SAPL under MVS and *SAX* which is SAPL under Unix. SAX is the one also running under Linux and compared to APL2 here.

Variables and workspace names are enclosed in ‘quotes’. Functions and files are enclosed in &lt;angle brackets>.

The term *atom* refers to a numeric or character scalar only. The term *item* refers to any scalar.

Whenever possible I will show examples in SAPL and APL2 side by side.

## A bit of history

SHARP APL (SAPL) used to be a mainframe product only until a PC version appeared in 1984 (release V17 of SAM and rel V20 later on). It remained pretty much so as the PC version never took off. The PC version ran under DOS and lacked many features. Because it was emulating the S/370 it was also very slow.

SAPL really is a product as opposed to a language. There are various components including a batch scheduler, a file system, many shared variable processors, sorting utilities, etc. More importantly, SAPL tasks may spawn and **control** other APL tasks with system functions, something no other APL has ever achieved. Very large secure systems can be (and have been) written in SAPL.

By contrast, in IBM’s APL2<sup><a href="#footnote1">1</a></sup>, everything but the language is external and only available through auxiliary processors (APs). There are no quad functions, for example, to perform file operations.

There are pros and cons for using each APL and the purpose of this article is not to take sides but rather to show the differences between them as objectively as possible.

SAX came to life in the early 90’s and is a more flexible version of SAM. It is based on “the Dictionary of APL” by Ken Iverson. It extends SAM V21. It more closely resembles J. (Think of J on APL char set.) The Linux version is the same as the Unix version.

At some point there was a shift in philosophy using the first versus the last axis in function operations (ex: `/` vs. `⌿` ) and some newer functions were defined in terms of the latter. APL2 has remained pretty much a “last axis favoured” language.

## Enclosed Arrays

In SAPL the term “enclosed array” is used. In APL2 the term used is “nested array”. They are, practically, the same thing. The term “boxed array” (SAX) is also sometimes seen but won’t be used here.

Let’s assume we have the following variables on hand:

```
a←'text'⋄ b←1 2 3 ⋄ c←2 3⍴a ⋄ d←2 4 3⍴⍳33
```

The simplest way to create an enclosed item is to use the expression

<table width="60%"> <th>SAX APL2</th><th>APL2</th><tr> <td align="center"><code>x←&lt;anyvalue</code></td> <td align="center"><code>x←⊂anyvalue</code>
</td></tr></table>

In SAPL there is no maximum level (depth) of enclosure. Thus `<<<a` is ‘a’ enclosed 3 times. In IBM’s APL2 there is a limit.

Items must be *opened* before applying a function to them. If `x←<2 3 4` then

```
  +/>x                  +/⊃x
```

will result in the sum of the values in ‘x’.

To create a list (vector) of enclosed items we can use the usual *catenate* function (`,` ):

```
   x←(<a),(<b),<c       x←(⊂a),(⊂b),⊂c
```

This will ALWAYS work in all interpreters I know. But there is an easier (incompatible) way to do this:

```
   x←a⊃b⊃c              x← a b c
```

Which brings us to the way things are presented to the interpreter.

### Notation

APL2 uses what is known as *strand* notation. Using this notation, arrays are created by putting items together side by side, without using catenate (‘`,`’) as in the example above. Sometimes it is handier than using ‘`,`’. SAX does not support that notation and requires the use of functions ‘`,`’ to catenate items together or ‘`⊃`’ to *link* them if they are to be enclosed.

### Link

*Link* is a specific SAPL function to ‘enclose and catenate’ with a twist. To understand its use let’s look at the monadic case first:

#### Monadic ‘link’

This is another function specific to SAPL. The formal definition is: ‘*enclose* the argument IF it does NOT contain already enclosed item(s)’. Sort of a conditional enclose. In fact *conditional* *enclose* is its real name. See its usage<sup><a href="#footnote2">2</a></sup>:

```
   x←⊃var           ⍎(1<|≡x←var)/'x←⊂var'
```

#### Determining if an array is enclosed

An application of this is to determine whether we are dealing with an enclosed array or not:

```
   x≡⊃x              1<|≡x
```

will return 1 if ‘x’ is enclosed. There is no *depth* function in SAPL.

#### Dyadic ‘link’

`a⊃b` will enclose ‘a’ and catenate it to the enclosure of ‘b’ **if ‘b’ does NOT contain enclosed elements**. Formally,

`a⊃b` is the same as  `(<a),⊃b`

Why not use `(<a),<b` ? The reason is to avoid *enclosing* a list repeatedly. Imagine what would happen if it were to always enclose the right argument in the expression  `a⊃b⊃c`. This would result in `(<a)` ,`<(<b)`,`<c` , a two-element list. This is not what we want. A consequence of this is that one must explicitly code the enclosure of the last item if the last value MUST be enclosed, as in `x←a⊃b⊃<m` (with `m←2 3⍴<a`), to get a three-element list.

On the other hand, if we want to, say, catenate  `<a` before each row of ‘m’ then `a⊃m` will do whereas `(⊂a),m` is required in APL2.

### Heterogeneous arrays

Both APLs support arrays containing a mixture of items of different data types, namely characters, numbers and enclosures although they must be explicitly catenated in SAX:

```
   x←1,'s'                   x←1 's'
   x←1,'s',<'sa'             x←1 's' 'sa'
   x←1,'s','sa'⊃2 3          x←1 's' 'sa'(2 3)
```

### Disclosing multiple items at a time

Entire arrays can be disclosed in one fell swoop with only space and maximum rank (127) restrictions:

```
   2 3 ≡ ⍴>1⊃'dsa'    2 3 ≡ ⍴⊃1'dsa'
```

In Dyalog there is a “same rank” restriction<sup><a href="#footnote3">3</a></sup>. Not in SAX:

<table width="80%" cellpadding="6"><td><code>4 2 4 4 ≡⍴&gt;a⊃b⊃c⊃d</code></td>
<td align="right">(RANK ERROR in Dyalog and APL+PLUS II)</td>
</table>

Disclosing an array results in another array of the shape of the array disclosed, followed by the largest of the shapes of ALL items disclosed with proper fill elements to fill where necessary.

### Atom enclosure/disclosure

A major difference between SAPL and APL2 is in the handling of atoms (simple scalars) upon enclosing them. In SAPL enclosing ANYTHING adds a level of enclosure. Not so in APL2 where an atom can NEVER be enclosed. Thus:

```
   ~2≡<2         2≡⊂2
```

This has several implications as we shall see in a later article.

The disclosure of an atom is permissible in both APLs. Thus

```
   2≡>2          2≡⊃2
```

## Operations on arrays

SAPL offers a few operators to work with (enclosed) arrays. I will discuss only the basics in this article. The next article will cover the subject in more depth (no pun intended).

### *With, Under, Each* (`¨`)

Although *Under* is the official name under SAX, people often refer to it as *With*. APL2 uses the term *Each.*

With ‘`/` ’ this is the most used operator in ALL APLs. It is probably best explained starting with an example:

```
   ⍴¨>x          ⍴¨x
```

Here we wish to return the *shape* of each item by first disclosing (each) cell of ‘x’, getting its *shape* and re-enclosing it, ending with a result of the same shape as the original argument.

In SAPL `¨` is dyadic, allowing for a different right function. The people at APL2 probably felt that since *disclose/enclose* is used most of the time it was better to make `¨` monadic and include these functions implicitly whereas at I.P.Sharp (where SAPL came from) people tried to be more general.

Some interesting combinations can be made with 2 functions and `¨` although 99% of the time *disclose* (`>` ) is used.

An example is multiplication:

`A×B` is `*(⍟A)+(⍟B)` or `A+¨⍟B`

The real APL2 definition is: “for each cell:

- disclose the cell
- apply the (ambivalent) function of ¨
- enclose the cell ”

The real SAPL definition is: “for each cell:

- apply the right function of ¨ (usually >)
- apply the left (ambivalent) function of `¨`
- apply the inverse of the right function of `¨` (usually `<`)”

Or, given **f**  **`¨`g y**, for each cell: “**g<sup>-1</sup> f g y\[j\]**”.

This, of course, implies that someone in the interpreter department goes over all the suitable right hand argument functions and determines an inverse for each one of them. And it was done. For example, `<` and `>` are inverses of each other.  `⍉` is its own inverse.

### Dyadic *With* reviewed

This is the same operator except that **f** (the left function) is used dyadically<sup><a href="#footnote4">4</a></sup>. In more complicated terms, <b>x f </b> **`¨` g y** is, for each corresponding cell:

“**g<sup>-1</sup> (g x\[j\]) f (g y\[j\])**”.

Where ‘x’ is the left argument.

Examples:

`   p ⍴¨> q      p ⍴¨ q`    (reshape each)  
`   t1,¨>t2      t1,¨ t2`  (catenate each)

### Scalar functions

In APL2, the use of the *each* operator is assumed for scalar functions (like + ) and derived functions, enclosed or not. There is no need to specify it. Ex:

```
   x+¨>y        x+y
```

Moreover, the function is applied recursively so long as the lengths are respected at each (nested) level. This is not possible in SAX.

The reason is that since *disclose* is implied and that an *atom* can NEVER be enclosed in APL2 then the *each* specification can be omitted. Structural functions like *take* must use *each* to remove ambiguities.

This is also why  `+/x` and `+/¨x` mean two different things in APL2. In SAX those would be coded as `+¨>/x` and `+/¨>x` respectively.

### *On* (`⍤`)

This operator is similar to *with* except that the result of each cell is not modified by the inverse of **g.**

`¨` and `⍤` are called composition operators. The only difference between them is in the absence of inverse for the right argument function of `⍤`. For example, using `f⍤ > y` is the same as doing

```
   >f¨> y       ⊃f¨ y
```

This may seem trivial but I will show in a subsequent article, along with another operator, the implications of using this operator.

## Other differences

SAPL supports the use of alpha (`⍺`) and omega (`⍵`) as APL identifiers for variables, functions and labels. These were introduced when the `⍺/⍵` “direct definition” notation was popular (late seventies). The wisdom of their usage in code is debatable but they are available. Also SAPL literature often refers to the left argument as alpha and the right argument as omega. Something to keep in mind.

### Complex numbers

These are not available in SAX yet.

### Control structures

These are not APL2 but are worth a mention anyway. They do not exist in SAX. In a subsequent article I will show how to emulate them in a control version system that Soliton provides.

### Defined operators

SAPL does not allow you to define your own operators.

### Error trapping

Similar to Dyalog APL. Even the event numbers are almost the same. A notable difference: the ability to trap value errors “in line” and resume after solving the error. This allows you to easily do “demand paging” of objects.

### File operations

In SAPL those are more like APL+Win. System functions don’t start with an ‘F’ ( `⎕read` , not <code>⎕<b>
f</b>read</code> ). All file operations return a result, that is `val ⎕replace tc` returns `0 0⍴0` . This allows someone to use them with operators.

### Format by example

SAX allows the usage of a character left argument to the format primitive `⍕` . This is the same as IBM’s APL2 (not found in Dyalog or APL+Win).

### Multiple assignments

It is possible, in both APLs, to “assign with disclose” several values to several variables by using quotes around the variables<sup><a href="#footnote5">5</a></sup>:

```
   'a b'←2 5⊃'cxz'         (a b)←(2 5)'czx'
```

There is a peculiarity using this technique where the number of variables is 1:

```
   'a b c'←1⊃1 2⊃1 2 3     ('c' is 1 2 3)
   'b c'←1 2⊃1 2 3         (again, 'c' is 1 2 3)
   'c'←⊃1 2 3              (now 'c' is <1 2 3, NOT 1 2 3)
```

This behaviour is found on ALL APLs (also in J!).

Personally, I think this is inconsistent. If there are no quotes (or parentheses) it should be a direct assignment. If there are some it should ALWAYS remove one level of nesting before assigning the value(s).

### From

*From* is an alternative to bracket indexing. The beauty with *from* is that since it IS a function it does not suffer bracket syntax problems and can be used with operators.

*From* selects elements along the first dimension. Used with list arguments it selects individual items:

```
   n{list                 n⊃¨⊂list
```

*Pick* (dyadic `⊃` in APL2) and *First* (monadic `↑` ) are not available under SAX. You can still use `>` with brackets \[x\] or use *from* (`{`):

```
   x←>atom{list           x←atom⊃list
```

Note that *from* is  `⎕io` independent. It uses an *offset* instead of an origin dependent position (index). *from* also allows you to select items from the end of the array by supplying negative offsets. Ex:

```
   't'=3{a                't'=(3+⎕io)⊃a
   't'^.=0 3{a            't'^.=a[1 4] (in ⎕io 1)
   'x'=¯2{a               'x'=a[⎕io+(⍴a)+¯2]
```

### Prototyping

In APL2 prototypes are used to fill holes created by `\` or ↑ . Ex: `2 2≡⍴↑0⍴⊂2 2⍴5` . SAPL has no prototype. If ‘x’ is an *item*, the expression ‘`0\x` ’ always returns either 0 for a number, blank for a character or `<⍳0` for an enclosed object.

### Reduce N-wise

IBM’s APL2 allows you to reduce along an axis in sections of length N (N <b>f</b>/ x). Not in SAPL.

### Stop, Trace

Those are set using  `⎕stop`  and `⎕trace` system functions as in Dyalog APL. IBM’s APL2 uses ‘`t∆` ’ and ‘`s∆` ’ special name prefixes to do the same.

### Take and drop

In SAX these functions work on the leading axis of an array without having to specify them all. Ex (`?=⍴⍴ m`):

```
   2↑m              (2,1↓⍴m)↑m
   2↓m              ((1⌈⍴⍴m)↑2)↓m
```

IBM’s APL2 allows you to use *take* with **one** axis as in

```
   3↓m              3↓[1]m
```

### Scattered indexing

When the argument has a rank higher than 1 then indices should be enclosed:

```
   (<1 2){d         (⊂1 2)⊃ d
   (1 2⊃0 1){d      (1 2)(0 1)⊃d (⎕io 0)
```

But you can pick an entire row (or first dimension elements) by supplying the offset only:

```
   2 3 ≡ ⍴ 1 3 { 4 3 ⍴ ⍳12
```

### Swap

This operator merely swaps the arguments to a function. Dyalog uses `⍨` for that. For ex:

```
   2=7-⊂9
```

## Unique features in SAX

Here is a list of features not found in any other APL.

### Index (iota underbar)

This function returns the index of the first occurrence using epsilon underbar ( `⍷`)as primary function:

```
   2 3≡(4 5⍴⍳20)⊣2 2 ⍴ 8 9 13 14 (⎕io 1)
```

### Jot

`∘` is a constant whose value is `<⍳0` . This is the constant used for overtake and `\` with enclosed or heterogeneous arrays.

### Nub (monadic take)

This one is not unique to SAX (Dyalog’s equivalent is `∪`) but it handles higher rank arrays:

```
   5 2 ≡ ↑ 9 ⍴ 5 2
   (↑m[7⍴⍳3;] ) ≡ m←3 4 ⍴ 'johnmarypaul'
```

### Nubin (monadic `=`) , nubsieve (monadic `≠` )

`=x` is the equivalent of `(↑x)∘.=x` for vectors

`≠` returns a boolean indicating where each element is first encountered. This is the equivalent of `((v⍳v)=⍳⍴v)` for vectors. Works on matrices too such that

```
   1 0 1 0 1 0 ≡ ≠ 2⌿ m
```

### Output redirection

It is possible to send all input/output to a file using `⎕out 0,tie` .

### Packages

Packages are unique to SAPL and are very useful. A series of system functions can be used with them, all of which start with `⎕p` as in `⎕pack` , `⎕pdef` , etc. For example, to define variable ‘x’ with objects (variables, functions) ‘a’ and &lt;f> (for example to be stored on file) you would do

```
   x←⎕pack 'a f'
```

Think of them as static namespaces (found only in Dyalog). You cannot “enter“ a package and run something from within it. For example, if you wish to use its contents in a local environment you must first localize the names. For example, this function &lt;execin> will execute an expression using a package’s contents:

```
      ∇r←exp execin pack;f
   [1]⍎⎕fx>('f',,';',⎕pnames pack)⊃'⎕pdef pack ⋄ r←',exp
      ∇
```

### Pass, Stop

These two functions are probably the simplest ever conceived yet very useful.

*Stop*:

```
   ⊣⎕dl 5                  0 0⍴⎕dl 5
   x←2⊣y←5                 y←5 ⋄ x←2
```

*Pass*:

```
   ⊢x←1 2 3                ⎕←x←1 2 3
```

This last function is more useful than it looks. For example, if space is important and you do not want the interpreter to keep a copy of the argument to a function you could do

```
   myfn (⎕ex 'x')⊢x
```

This effectively erases the variable AFTER passing it as an argument to &lt;myfn>. The interpreter is then able to free up the memory space, should the need arise later on (for example after you reassign the argument within &lt;myfn>). I don’t know of any other **general** way to do this with any other interpreter.

### I-beam functions

These are a leftover from the 60’s. If you think you know what they are you probably weren’t there. They are kept for compatibility reasons with SAM (many are no-ops) and will not be explained here.

### Less

This one is not unique to SAPL (most APLs handle the vector case) but it accepts higher rank arrays:

```
   12 7 ≡1 7 12 5 ~ ⍳6
   'john' ≡ ,m~m[2 3;]
```

### Raze

This is an interesting monadic function (in terms of raison d’être). Its formal definition is

```
   ⍪¨>⌿
```

In other words, “working from the first dimension: catenate on the first dimension all items.”

My guess is that this came from a requirement to create in SAPL a function reminiscent of APL2’s monadic `∊` which puts everything in ravel order regardless of structural depth<sup><a href="#footnote6">6</a></sup>. Before *raze* programmers would often need to get text (or numbers) from enclosed lists together, sort of  `>,¨>/` but SAM wouldn’t allow it resulting in expressions like “`, ⍕x`” (plus `⎕fi` if numeric) being used resulting in high CPU costs. Hence *raze* was created. Plus it can be useful.

### Shared variables

`⎕svs` returns the status of a variable

`⎕sc` is used to wait on ANY shared variable event. It can also be assigned a “maximum wait time”.

`⎕svn` is used to uniquely identify your task when multi-tasking.

### System commands

SAX commands are similar to other APLs.

`)copy` allows the copying of system variables.

`)load` and `)clear` allow you to specify initial  `⎕wa`. Ex:

`)clear 200000000` will give you a workspace of 200M if you have the room!

### System functions

#### Workspace reporting

Various workspace elements (name, stack) can be found using `⎕ws` .

#### Function definition

`⎕fd` can be used to manipulate function representations (similar to `⎕def` , `⎕vr` in APL+Win).

Of course ISO standard functions like `⎕cr` and `⎕fx` also exist.

### System variables

`⎕ps` controls the formatting of enclosed arrays. It is a four-elements integer list of

1. where to position each item in its own box (`¯ 1`=left, `0`=centre, `1`=right)
2. where to position each item in its own box (`¯ 1`=up, `0`=centre, `1`=down)
3. horizontal spacing between items
4. vertical spacing between items

The default `¯1 ¯1 0 1` fits most cases.

`⎕sp` is used to store values. It stays the same between `)loads` . Probably one of the most valuable system variables ever created.

`10{⎕av` is the character used as line delimiter. Not  `13{⎕av`. One should exercise caution with this.

### Table (monadic comma bar,  `⍪`)

This function restructures any structure into a matrix, something like

```
   (2↑(⍴t),1 1)⍴t
```

*Table* turns a scalar into a 1 by 1, a vector into a 1 column table, anything else into a table.

### Task spawning

This is possible in SAX and will be the subject of another article.

## Conclusion

SAPL and APL2 are quite different in many respects but each (!) has its own merits. For anyone wishing to delve into the APL/Linux world there is, right now, little choice but to examine SAPL. Personally I am comfortable with both. I found SAPL to be fast, reliable and, with the tools Soliton is providing, a serious product.

The next article will deal with special SAPL operators (hoof, rank, cut, numeric with, \}). Another one will deal with multi-task operations. If time permits, another one will talk about intrinsic functions and one of Soliton’s products, *logos*.

### Footnotes

<ol>
  <li><a name="footnote1">Dyalog and APL+Win do include system functions for APL files operations
  <li><a name="footnote2">The | is necessary only in Dyalog where ≡ can be negative if the structure 
doesn’t have a constant depth.
  <li><a name="footnote3">I do not know what IBM’s APL2 and APL+Win do here.
  <li><a name="footnote4">This adverb can’t be found in the English dictionary. Yet.
  <li><a name="footnote5">In Dyalog the parentheses are not necessary around the variables leading 
to potential conflicts.
  <li><a name="footnote6">The design of raze had nothing to do with monadic epsilon in APL2. It is 
much superior, as it preserves lower-level structure. Some consider the 
design of monadic epsilon to be largely useless in real applications.
</a></li></a></li></a></li></a></li></a></li></a></li></ol>

## References

1. Bernecky, R: An Introduction to Function Rank in APL88 Conference Proceedings, ACM SIGAPL Quote Quad 18:2, 1987, pp. 39–43
2. Bernecky, R and Iverson, K E: Operators and Enclosed Arrays, (APL) Users Meeting 1980, I.P. Sharp Associates Limited

Please direct questions/remarks to [danb@dsuper.net](mailto:).

Dan Baronet  
Montreal  
Canada

***
