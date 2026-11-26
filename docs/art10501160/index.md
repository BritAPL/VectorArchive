---
vid: '10501160'
title: Compiling APL to JavaScript
authors:
- Nick Nickolov
byline: Nick Nickolov (nick.nickolov@gmail.com)
volume: '26'
issue: '1'
page: '54'
received: '2013-09-18'
online: '2013-09-26'
editor: John Jacob
description: Nickolov describes ngnAPL giving an overview of the project and its dialect of APL
abstract: This article is about github.com/ngn/apl[1], an APL to JavaScript compiler written in CoffeeScript. It gives an overview of the project and its dialect of APL, then proceeds to describe three major implementational obstacles and the design decisions taken to overcome them.
source: content/printed/261/ngnapl/ngnapl.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- implementations
- language design
---

Nick Nickolov (nick.nickolov@gmail.com)
{ .byline }

This article is about github.com/ngn/apl[\[1\]](#ref1), an APL to JavaScript compiler written in CoffeeScript. It gives an overview of the project and its dialect of APL, then proceeds to describe three major implementational obstacles and the design decisions taken to overcome them.
{ .abstract }

## Introduction

The list of languages that compile to JavaScript is growing steadily for a good reason: almost every modern consumer device can run a browser with a JavaScript interpreter in it. And for the server there’s NodeJS. As a language, JavaScript has its subset of good parts and if you restrict your coding to them, it’s actually a very decent language. The execution model is reminiscent of Lisp with its lambdas and lexical closures, despite the superficially C-like syntax. As a matter of fact, getting rid of those curly braces, semicolons, and other noise is easy thanks to CoffeeScript, which gives a comfortable layer of syntactic sugar over raw JavaScript.

I started an open source project for an APL compiler targeting JavaScript. Though incomplete and of poor performance, it’s good enough for experimenting with short programs, such as the Game of Life or the N Queens problem.

I made a conscious decision to deviate from the APL tradition in several ways.

### Function definition

Lambdas are supported (function literals enclosed in curly braces, a.k.a. dfns) but the del (`∇`) syntax for function definition is not. So, instead of

```
    ∇R←A f B
        R←...
    ∇
```

one is forced to write

```
    f←{...}
```

### Variable scoping

Scoping is always lexical. This means that the two occurrences of `a` below

```
    f←{a←123}
    g←{a←456}
    h←{...}
```

are different variables and neither of them is accessible from within `h`, regardless of the order and nesting of `f`, `g`, `h` invocations.

A variable is considered to belong to the outermost ancestor scope where it is mentioned. So, the first two occurrences of `a` below are the same variable and the third `a` is distinct from them:

```
    f←{
        a←123
        g←{a←456}
    }
    h←{a←789}
```

Note that assignment doesn’t necessarily create a local variable. If the variable is already present in any enclosing scope, assignment will set *that* variable.

A variable must not be used before it is assigned a value. The compiler can detect most violations of that:

```
    ⎕←⍳ a      ⍝ compiler error
    a←5
```

### Phrasal forms

Expressions consisting of a sequence of two or three functions are said to form a hook or a fork respectively and the result is a new function defined as follows:

```
     (fg)⍵    ←→   ⍵fg⍵
    ⍺(fg)⍵    ←→   ⍺fg⍵
     (fgh)⍵   ←→   (f⍵)g(h⍵)
    ⍺(fgh)⍵   ←→   (⍺f⍵)g(⍺h⍵)
```

Examples usages of phrasal forms are the arithmetic mean written as `+/ ÷ ⍴` and continued fraction evaluation as `(+÷)\`.

### Non-privileged primitives

Primitives are just like any other variable, only as if implicitly defined in the root scope. As a side effect, they can be overridden:

```
    3⍟4          ⍝ returns 1.2618595071429148
    ⍟←{⍺+2×⍵}
    3⍟4          ⍝ returns 11
```

One use of this fact might be to introduce comparison tolerance, which isn’t supported by default:

```
    ⎕CT←1e¯13
    =←{(|⍺-⍵)≤⎕CT×(|⍺)⌈|⍵}
```

It’s okay to use single non-alphabetic characters as variable names, so the language can be augmented as one sees fit:

```
    π←○1
    √←{⍵*.5}
    ∑←+/
    ½←{⍵÷2}
    ±←+,-
    €←{exchangeRate×⍵}
    ≈←{(|⍺-⍵)≤⎕CT×(|⍺)⌈|⍵}
```

### Index origin fixed to 0

I took a side in this Holy War.

```
    ⍳3      ⍝ returns 0 1 2
    ⎕IO←0   ⍝ ok, no effect
    ⎕IO←1   ⍝ error
```

### Line terminator ambiguity

A line terminator is treated as a statement separator if it occurs within `{}` or at the topmost level, but is considered whitespace if it’s inside `()` or `[]`.

Example:

```
    glider←{
        a←3 3⍴(
            0 1 0
            0 0 1
            1 1 1
        )
        ⍵ ⍵↑a
    }
```

### Absence of control structures

Control structures such as `:If ... :Else ... :EndIf` are not supported but can be emulated using lambdas, guards, and APL primitives. For instance

```
    :If x
        y
    :Else
        z
    :EndIf
```

can be replaced with

```
    {x:y⋄z}0
```

and

```
    :While x
        y
    :EndWhile
```

with

```
    {~x:1⋄y⋄0}⍣⊢ 0
```

The rest of this article describes three challenging problems which I came across during the implementation of APL. Two of them I solved and the third one is yet to be taken care of.

## Parsing APL expressions

Consider the APL expression

```
    a b c
```

Depending on context, it could mean different things:

```
    a←0  ⋄ b←{} ⋄ c←0  ⋄ a b c    ⍝ a dyadic application of b
    a←{} ⋄ b←{} ⋄ c←0  ⋄ a b c    ⍝ two nested monadic applications
    a←0  ⋄ b←0  ⋄ c←{} ⋄ a b c    ⍝ compiler error
    a←0  ⋄ b←0  ⋄ c←0  ⋄ a b c    ⍝ construction of a new vector
    a←{} ⋄ b←{} ⋄ c←{} ⋄ a b c    ⍝ a fork
```

So, it seems the semantics depend on what `a`, `b`, and `c` represent at runtime, verbs or nouns, and building a complete AST is impossible before we have that knowledge.

Fortunately, there exists a way around this at the cost of imposing a small restriction on variable usage. The restriction is as follows:

- Once we assign a noun to a variable, we are only allowed to assign nouns to it later.
- Once we assign a verb to a variable, we are only allowed to assign verbs to it later.

In other words, variables must preserve their lexical category.

Example:

```
    x ← 0
    x ← 1     ⍝ OK
    x ← {}    ⍝ Compiler error

    f ← {}
    f ← +/    ⍝ OK
    f ← 2 3   ⍝ Compiler error
```

This restriction separates the worlds of nouns and verbs at compile time. Every expression can then be inferred one of the two lexical categories so the compiler know what JavaScript code to render for it.

## Representation of n-dimensional arrays

During the evolution of the project I went through a couple of different implementations of the basic APL data structure.

### Nested representation

My first take on this was to use n nested levels of JavaScript arrays to represent an n-dimensional APL array. For instance:

```
    APL: 2 3⍴⍳6
    JS: [[0, 1, 2], [3, 4, 5]]
```

Thus, subscripting maps nicely between the two languages:

```
    APL: a[i;j;k]
    JS: a[i][j][k]
```

but many of the primitives’ implementations had to involve recursion or multiple loops, which apart from being inefficient was complicating matters unnecessarily, so I transitioned to a flat representation.

### Flat representation

A flat representation is a single JavaScript array which contains all items of the APL array in lexicographic order of their indices (a.k.a. ravel order). Additional meta-information about the shape must be stored, too.

```
    APL: 2 3⍴⍳6
    JS:
        var a = [0, 1, 2, 3, 4, 5];
        a.shape = [2, 3];
```

This way, indexing is not as plain as before:

```
    APL: a[i;j;k]
    JS: a[
            i * a.shape[0] * a.shape[1] +
            j * a.shape[0] +
            k
        ]
```

but it’s still worth it, as the implementation of most primitives gets simpler and more efficient.

There turns out to be an even better data structure which I was lucky to come across.

### Strided representation

In a strided representation, an APL array is a record of:

<dl>
    <dd>
        <code>data</code>: the elements, not necessarily in ravel order
    </dd>
    <dd>
        <code>shape</code>: as usual, an integer array
    </dd>
    <dd>
        <code>stride</code>: an integer for each axis, indicating how many items of <code>data</code> we
        have to skip over in order to move to an adjacent cell along that axis.
        Strides can be positive, negative, or zero.
    </dd>
    <dd>
        <code>offset</code>: the position in <code>data</code> where element <code>[0;0;...;0]</code> is located
    </dd>
</dl>

Example:

```
    APL: 2 3⍴⍳6
    JS:
        {
            data:   [0, 1, 2, 3, 4, 5],
            shape:  [2, 3],
            stride: [3, 1],
            offset: 0
        }
```

and this is another representation of the same APL array

```
    JS:
        {
            data:   [2, 1, 0, 5, 4, 3],
            shape:  [2, 3],
            stride: [3, -1],
            offset: 2
        }
```

Indexing is relatively straightforward:

```
    APL: a[i;j;k]
    JS:
        data[
            offset
            + i * stride[0]
            + j * stride[1]
            + k * stride[2]
        ]
```

The advantage of this strided representation becomes apparent when working with large volumes of data. Functions like transpose (`⍉⍵`), reverse (`⌽⍵`), or drop (`⍺↓⍵`) can reuse the `data` array and only care to give a new `shape`, `stride`, and `offset` to their result. A reshaped scalar, e.g. `1000000⍴0`, can only occupy a constant amount of memory, exploiting the fact that strides can be 0.

## Continuation-passing style

The third problem, one which I haven’t solved in code yet, arose from the way NodeJS does I/O. While in some cases NodeJS allows you to do

```
    var content = fs.readFileSync('a.txt');
```

it strongly encourages the so-called continuation-passing style (CPS), demanding a callback:

```
    fs.readFile('a.txt', function (content) {
        // ... Invoked later, after content becomes available
    });
```

It’s not only that. In a browser environment, if we want to be properly mimicking I/O, we again need CPS. Of course, we can use the blocking functions `prompt()` and `alert()`, but most users find those annoying because they prevent them from using other parts of the UI while entering text.

This is what happens when reading from `⎕`, for instance:

1. APL code tries to get the value of variable `⎕`.
2. The user is prompted to type something in an `<input/>`.
3. APL execution is suspended while the user is typing, looking up help information, using the on-screen keyboard, etc.
4. The user presses enter.
5. APL execution is resumed and the value obtained from `⎕` is processed.

The trouble is in step 3. How do we suspend execution to resume it later? We might as well try to make the compiler output CPS JavaScript code, but that appears get complicated rather quickly.

If only JavaScript had a `call-with-current-continuation` primitive like Scheme, we would be able to do just this:

```
    var content = callcc(fs.readFile.bind(fs, 'a.txt'));
```

but alas, it doesn’t. We can amend that through implementing a virtual machine (VM) in JavaScript with a `callcc` instruction that suspends execution, captures the current state with all its stack and variable bindings and presents it as a callable *resume* function. The compiler can then produce some intermediate bytecode for the VM instead of pure JavaScript.

What if we also expose `callcc` as an APL primitive? This would be so powerful that it could be used to implement an exception mechanism, break/continue, coroutines, generators, Prolog-style backtracking, etc in APL itself. Therefore, adding a VM looks like a lucrative prospect for ngn/apl.

1. <a id="ref1"></a>ngnAPL [https://github.com/ngn/apl](http://www.7-zip.org/)
