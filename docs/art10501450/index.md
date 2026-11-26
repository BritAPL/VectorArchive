---
vid: '10501450'
title: A Notation for APL array Embedding and Serialization
authors:
- Phil Last (phil.last@4xtra.com)
byline: Phil Last (phil.last@4xtra.com)
volume: '26'
issue: '4'
page: '36'
received: '2014-12-17'
online: '2015-03-01'
description: Most systems include a number of tables or arrays that are referred to frequently but rarely changed. I examine the utility and possibility of making these and other data easily visible, editable and transferable between different systems or parts of a system possibly implemented in different APLs.
abstract: Most systems include a number of tables or arrays that are referred to frequently but rarely changed. I examine the utility and possibility of making these and other data easily visible, editable and transferable between different systems or parts of a system possibly implemented in different APLs.
source: ../../php-site/content/printed/264/naples/naples.htm
converter: vecrescue 0.1.0
issue_tab: n4-march-2016
tags:
- language design
- data and files
---

Phil Last (phil.last@4xtra.com)
{ .byline }

Most systems include a number of tables or arrays that are referred to frequently but rarely changed. I examine the utility and possibility of making these and other data easily visible, editable and transferable between different systems or parts of a system possibly implemented in different APLs.
{ .abstract }

## Introductory

> *And I think ... that it's probably time for us to come up with a notation for constants in the language so that ... you can declare matrices and so on in a nice readable fashion.* Morten Kromberg, Dyalog'14, Eastbourne, [Technical Road Map](http://video.dyalog.com/Dyalog14/?v=rRRyDWaU1fA)

My diaeresis hides Morten's emphasis on *scripts*. Certainly *Dyalog*  APL's ability to store code in and retrieve it from scripts external to the traditional *workspace* leaves a gap where stored arrays are concerned. But there seems to be no good reason to keep the benefits that would accrue with such an array notation to one limited form of code storage which most APLs don't support. At the same time there is the necessity to transport systems via the internet which requires serialization not only of code, however stored, but of data and in a form that is also independent of data storage.

*In what follows all the examples and a model presented use Dyalog APL V14.0 but the proposal is intended to be cross platform within APL. The appendices contain a further proposal to include dictionaries as separate entities in the notation and a short description of a model.*

## Requirement

The requirement for an array notation has existed since the first implementers of APL omitted to allow for the direct definition of multi-dimensional arrays in the syntax *without function application*.

From very early in my APL career, when writing systems requiring persistent, constant, arrays I was unhappy with the facilities offered for defining and maintaining them and the necessity to save them as global *variables* along with the code. Why not code an easily edited representation of the data into the function and extract it from its own `⎕CR` at initialization of the system?

Examples of functions returning their embedded data might be

```
 ∇ r←fText
   r←2 2↓⎕CR'fText'
⍝ Embedded text array to be
⍝ extracted at runtime.
⍝ ...
 ∇
      fText
┌→─────────────────────────┐
↓Embedded text array to be │
│extracted at runtime.     │
│...                       │
└──────────────────────────┘
 fNums←{
     ↑×/↑⎕VFI¨↓¯1↓2 2↓⎕CR'fNums'
⍝ 01  12  23  34  45  56  67
⍝   78  89  90  01 23  45  67
⍝ ...
 }
      fNums''
┌→───────────────────┐
↓ 1 12 23 34 45 56 67│
│78 89 90  1 23 45 67│
│ 0  0  0  0  0  0  0│
└~───────────────────┘
```

This led very soon to a utility function that would extract all trailing comment lines from the `⎕CR` of its caller. I have tried many other variants over the years such as: that all comment lines starting with a particular string are returned; that all contiguous comment lines immediately following the call are returned; that `⎕VFI` (`⎕VI` and `⎕FI`) is called internally so that in many cases no further processing is required on the returned data; and several that included mark-up for multi-dimensional arrays.

All the above have their drawbacks and inconveniences but I find them vastly more appealing than the repeated assignment and catenation that is currently the only alternative, albeit a marginally more efficient one in terms of actual machine time.

But what I really wanted was a notation, native to APL, that permitted me to code the array directly into the function without the comments; without repeated assignment, catenation and reshape; and without having to extract it with another function. In other words: executable code; an extension to vector notation. But the possibility did not really present itself until 1997 with the release of *Dyalog* 8.1 that included *dfns* for the first time. Here we had a new syntax that permitted a pair of braces to span line-ends within a function rather than being restricted to a single line as were brackets and parentheses.

```
┌─────────────┐
│∇ r←f00 w    │
│  ...        │
│  f01←{      │
│     ⍺ ... ⍵ │
│     ...     │
│  }          │
│  r←... f01 w│
│  ...        │
│∇            │
└─────────────┘
```

If we could encompass several lines with a function expression then perhaps we could do the same with a display form of a multi-dimensional array to be evaluated during the tokenization of the containing function. This could make all arrays editable within the function editor and eliminate the need to store global constants along with the code.

## A conforming extension

It happens that no APL expression can start with an opening bracket. In other words an opening bracket cannot immediately follow a left arrow, an opening bracket, brace or parenthesis or a line-end.

Also, at least before the advent of *dfns*, it was not possible to have line-ends within matching brackets or parentheses. The ability to code a multi-line *dfn* between parentheses or index or axis brackets partially lifts that restriction. Still, the line-end cannot be directly between them; it must be between braces as well.

These two facts, or the reversal of the one and the relaxation of the other, make possible a syntax that would be a natural and even familiar notation to all APLers.

*Dyalog*'s experimental interpreter, *APLSharp*, permitted line-ends between parentheses, calling what was between them an *expression* whose value was that of the last expression in the list. What follows might appear similar but here the value of a parenthesised or bracketed expression containing line-ends will be the result of evaluating and joining all of them *in some way* so that all play an equal part in the result. An extension of vector notation, if you will.

The two two-dimensional arrays that display as

```
┌→────┐     ┌→─────────────┐
↓zero │     ↓ 0  1  2  3  4│
│one  │ and │ 5  6  7  8  9│
│two  │     │10 11 12 13 14│
│three│     │15 16 17 18 19│
└─────┘     '~─────────────┘
```

could simply be defined in code as

```
┌───────────────┐     ┌─────────────────────┐
│...            │     │...                  │
│[2] T←['zero'  │     │[6] N←[0  1  2  3  4 │
│[3]    'one'   │ and │[7]    5  6  7  8  9 │
│[4]    'two'   │     │[8]   10 11 12 13 14 │
│[5]    'three']│     │[9]   15 16 17 18 19]│
│...            │     │...                  │
└───────────────┘     └─────────────────────┘
```

Brackets will do a task analogous to parentheses but where the latter are used to group items adding *depth*, the former will add *rank*, with each new row of the representation indicating a new cell in the data. And there is no reason not to extend this such that between brackets further brackets will introduce another dimension in the data. Thus, where

```
┌────────────────────────────────────────────────────────┐
│ d←(('these' 'seven' 'words')('form' 'a text' 'array')) │
└────────────────────────────────────────────────────────┘
```

gives us a depth-three, two-item list of three-item lists of strings,

```
┌───────────────┐
│ r←[['these'   │
│     'seven'   │
│     'words']  │
│    ['form'    │
│     'a text'  │
│     'array']] │
└───────────────┘
```

gives us a simple, two-plane, three-row, six-column, three-dimensional array.

> It is worth mentioning here that there is a significant number of APLers who would happily see index and axis brackets removed from the language. The argument is that a pair of brackets does not denote either a function or an operator but it selects and amends data as if it were one or other of them; it is thus an interloper in the language. The arrival of the *index function* was welcomed because it dispensed with the need for index brackets but it came with the disappointment that yet another use of axis brackets was needed to make it workable. The subsequent addition of the  *rank operator* may finally lay this anomaly to rest. I claim that the introduction of brackets as *notation* is *not* an extension of it but rather restores the bracket to its rightful place along with parentheses, braces and quotes as *punctuation*.

Some use of a bracketed array notation *could* lead to slight if unnecessary confusion with both index and axis specification.

> In expression `a[...]`, the bracketed part is unambiguously an index if `a` is an array and an axis if `a` is a function or operator, that is if axis can *ever* be unambiguous.
>
> In expression `a([...])`, the parenthesis is unambiguously an array specification because parentheses are not permitted around index or axis brackets. The whole expression is a function call if `a` is a function and a two item list if `a` is an array.

The notation extends easily to nested data. One particular common type of static array is the table containing columns of numbers and/or strings. They are the devil to edit. Many *Dyalog* users will have seen the array `DRC.ErrorTable` that contains all the error numbers, codes and descriptions for Conga, *Dyalog*'s remote communicator. The first few rows and a later one look like this

```
┌────────────────────────────────────────────┐
│   0  SUCCESS                               │
│ 100  TIMEOUT                               │
│1000  ERR_LOAD_DLL                          │
│1001  ERR_LENGTH                            │
│1104  ERR_SEND      /* Could not send data*/│
└────────────────────────────────────────────┘
```

display like this

```
┌→────────────────────────────────────────────────┐
↓      ┌→──────┐      ┌⊖┐                         │
│ 0    │SUCCESS│      │ │                         │
│      └───────┘      └─┘                         │
│      ┌→──────┐      ┌⊖┐                         │
│ 100  │TIMEOUT│      │ │                         │
│      └───────┘      └─┘                         │
│      ┌→───────────┐ ┌⊖┐                         │
│ 1000 │ERR_LOAD_DLL│ │ │                         │
│      └────────────┘ └─┘                         │
│      ┌→─────────┐   ┌⊖┐                         │
│ 1001 │ERR_LENGTH│   │ │                         │
│      └──────────┘   └─┘                         │
│      ┌→───────┐     ┌→────────────────────────┐ │
│ 1104 │ERR_SEND│     │/* Could not send data*/'│ │
│      └────────┘     └─────────────────────────┘ │
└∊────────────────────────────────────────────────┘
```

and could be defined simply like this

```
┌──────────────────────────────────────────────────────┐
│ ErrorTable←[0 'SUCCESS' ''                           │
│           100 'TIMEOUT' ''                           │
│          1000 'ERR_LOAD_DLL' ''                      │
│          1001 'ERR_LENGTH' ''                        │
│          1104 'ERR_SEND' '/* Could not send data*/'] │
└──────────────────────────────────────────────────────┘
```

Diamonds' being largely equivalent to line-ends we can imagine each row of our multi-line array definition prefixed with a diamond and the whole thing ravelled to produce a single expression for the data, perhaps with suitable removal of redundant diamonds. This gives us the ability to define a simple linear notation which might also prove to be useful as an array serializer.

## Definition

```
┌→──────────────────────────────────────────────┐
│array      []                                  │
│           [ values ]                          │
│values     value                               │
│           value ...                           │
│           value ⋄ ...                         │
│value      number                              │
│           string                              │
│           array                               │
│           (value)                             │
│           [value]                             │
│string     ''                                  │
│           'chars'                             │
│chars      char                                │
│           char ...                            │
│char       typeable unicode character except # │
│           #xxxx (encodes a unicode character) │
│xxxx       four hex digits (0─9, A─F, a─f)     │
│           #0023 encodes the hash (pound) sign │
└───────────────────────────────────────────────┘
```

Diamonds thus fulfil two roles. At the same level of punctuation-nesting: within brackets they delimit cells; within parentheses they delimit items in a list. Thus `[...⋄...]` is an array of two major cells, while `[(...⋄...)]` is a list of two items.

Within a list the above definition encompasses the full panoply of vector notation but also the restriction such that a vector of any depth or length can be defined, *perhaps excepting one of a single item or a nested empty list*.

Within a multi-dimensional array the major cells can be further delimited by brackets, the rank of the array being one more than the highest rank of any of the cells to which all are implicitly raised.

```
┌→────────────────────────────┐
│ a←[[... ⋄ ...]⋄[... ⋄ ...]] │
└─────────────────────────────┘
```

Note that the definition precludes both function definition and execution. This is deliberate as the notation is intended to be an extension of vector notation which also does not involve function calls. Another reason is the proposed equivalence of the multi-line embedded array definition and its serialized counterpart. Including function calls in serialised data would certainly be considered a security issue.

Within any definition only punctuation `[(⋄)]`, numbers and white-space are allowed unquoted while most typeable characters can be included between quotes (with `'` itself doubled) and an escaping protocol is used for non-typeable characters. Any unicode character can be encoded as the *escape character* followed by four hex digits (`0-9`, `A-F`, `a-f`) that encode the character's code-point. I have chosen to use `#` as it is typeable but perhaps uncommon in data; another could be chosen but would have to be standard across all implementations. The escape character *must* be encoded in this way when it represents the character itself. `#` would be `#0023`, carriage return `#000D`, line feed `#000A` and the White Queen ♕ `#2655`.

In most presently implemented APLs a diamond is equivalent to a line-end so, in a reversal of the conceptual leap earlier, where we went from a multi-line approximation to a linear definition, the above syntax permits the array definition to be spread over a number of lines. And as all lines in an APL function can be commented then so can our array definition when embedded over a number of lines in a function or script.

## Limitations

Normal vector notation provides no facility to produce an enclosed scalar or a zero or one item vector, enclosed or otherwise. This restriction *could* be extended to array notation but equally it could be avoided. Normal APL permits blank lines and contiguous diamonds in functions. They are not executed and produce no results. Contiguous diamonds in array notation should follow this pattern and produce no part of the output. Nevertheless there is no reason *not* to differentiate between `[0 1 2]` and `[⋄0 1 2]`. Although the diamond in the second case is ostensibly redundant it is apparent that whereas the first is intended to be merely a vector the second is clearly expected to produce a two dimensional result. What should its shape be? We have the choice between `1 3` and `3 1`. Again, it is clear that `[0⋄1⋄2]` is expected to produce a one column array albeit that its items are strictly scalar. Allowing this leaves our `[⋄0 1 2]` to represent a one row matrix. Similar arguments can be used to define arrays of other ranks with dimensions of one or zero.

## Conclusion

The need for such a notation and the desirability of its being defined to be cross-platform is unquestionable. If a round-trip is desirable, as I believe it is, then the above limitations need to be overcome. But they will require more than one person's imagination. I believe the nested bracket approach could be the simplest and most versatile for multi-dimensional data, that outlined here possibly forming a basis for discussion. Given the power of vector notation APL needs very little enhancement to make it work. Some of the details here might be questionable and could undoubtedly be bettered.

A collaborative effort should be made to come to an agreed design with an eye on extensibility and forward compatibility such that providers could add their own enhancements.

## Appendix A - dictionaries

In all the above I have been referring to multi-dimensional and nested data. Dictionaries, variously known as associative arrays, objects, maps, key-value pairs, namespaces &c. might be considered worthy of their own notation. At least one supplier has implemented namespaces that can contain a set of named arrays and several have implemented object oriented features in which an instance of a class with a number of fields or properties could qualify.

*Where no special provision is made for them in an implementation then any current use must necessarily be represented as an array so an encoder would naturally encode it as such.*

*JSON objects* use a colon `:` to join and separate the key and value of each pair and a comma `,` to separate the pairs from each other. A natural choice for minor separator in an *APL* implementation would be the left arrow `←` while the pairs would be separated from one another by diamonds. But *JSON's* use of braces to distinguish objects from arrays is almost redundant as the presence of the colon would be sufficient except for the empty object that contains no key-value pair and therefore no colon. An arbitrary decision could be made to include a single left arrow merely to distinguish an empty dictionary `[←]` from any other empty array.

What data structure a *decoder* would generate from the notation would be implementation specific as would the array characteristics which would prompt the *encoder* to recognise candidates for encoding in this way.

The implementing of dictionaries along these lines would require the addition of a few more items to the definition:

```
┌───────────────────────┐
│dictionary [←]         │
│           [ pairs ]   │
│pairs      pair        │
│           pair ⋄ ...  │
│pair       key ← value │
│key        string      │
└───────────────────────┘
```

and we must add one more character to the list of permitted unquoted characters giving `[(⋄←)]`, a total of six.

## Appendix B - an experimental model

For the purposes of the proposal I have implemented a set of methods that simulate the action that the parser itself would undertake in a native implementation.

In present APLs the proposed array syntax would engender a syntax error so we have to *trick* the parser into allowing us to define the array in code without having to quote or comment it. We wrap the array definition in a *dfn* and pass it as operand to an operator that extracts and analyses the definition and returns the array without running the code. It results in an indented display not quite as indicative as that above but in which only the syntax colouring indicates anything in any way abnormal.

## Methods

<code><em>ArrayToCode</em></code> is for data embedding.

Given any APL array, <code><em>ArrayToCode</em></code> returns the derivation of a function call to embed in your own code where it is needed.

```
      ⊢a←'zero' 'one' 'two',⊃∘.,/⍳¨3 2
┌→───────────────────┐
↓ ┌→───┐ ┌→──┐ ┌→──┐ │
│ │zero│ │0 0│ │0 1│ │
│ └────┘ '~──┘ '~──┘ │
│ ┌→──┐  ┌→──┐ ┌→──┐ │
│ │one│  │1 0│ │1 1│ │
│ └───┘  '~──┘ '~──┘ │
│ ┌→──┐  ┌→──┐ ┌→──┐ │
│ │two│  │2 0│ │2 1│ │
│ └───┘  '~──┘ '~──┘ │
'∊───────────────────┘
    #.naples.ArrayToCode a
┌→────────────────────────────┐
│ { ⍝ edit indented rows only │
│     ['zero'(0 0)(0 1)       │
│     'one'(1 0)(1 1)         │
│     'two'(2 0)(2 1)]        │
│ }#.naples.CodeToArray 0     │
└─────────────────────────────┘
```

Once there, it can be edited as a part of the function or script while the operator <code><em>CodeToArray</em></code> will return the edited array the next time you run your code. Perhaps a native implementation, which would contain only the middle three indented lines above, would recreate the array immediately on fixing the edited code.

<code><em>APLToSerial</em></code> & <code><em>SerialToAPL</em></code> are for serialization and de-serialization.

Given any APL array, <code><em>APLToSerial</em></code> returns a simple text string suitable for transmission and independent of data storage implementation considerations while <code><em>SerialToAPL</em></code> will reconstitute the original array at the other end.

```
      ⊢s←#.naples.APLToSerial a
┌→───────────────────────────────────────────────────┐
│ ['zero'(0 0)(0 1)⋄'one'(1 0)(1 1)⋄'two'(2 0)(2 1)] │
└────────────────────────────────────────────────────┘
      a ≡ #.naples.SerialToAPL s
1
```

In a native implementation <code><em>CodeToArray</em></code> and <code><em>SerialToAPL</em></code> would be redundant as the notation would be a part of APL itself and as such, executable code while the format primitive could be enhanced to return either of the forms produced above by <code><em>ArrayToCode</em></code> and <code><em>APLToSerial</em></code>.
