---
vid: '10501520'
title: Reading new code
authors:
- Dan Baronet
byline: Dan Baronet
volume: '26'
issue: '4'
page: '46'
received: 2014-05=17
online: '2015-08-13'
editor: Callum Floume
description: Operators may interfere with reading APL code right-to-left. But now function trains are in play its time to revisit an explanation of how to parse an expression.
source: ../../php-site/content/printed/264/readingcode/readingcode.htm
converter: vecrescue 0.1.0
issue_tab: n4-march-2016
tags:
- programming techniques
- language design
---

Dan Baronet
{ .byline }

## Introduction

One of the requirements to writing and debugging code is to understand how to read the language in the first place. This applies to any language including human languages. If you can't read English the following will be hard to understand.

### Case Study

I was recently reading a document written by Roger Hui and saw the expression

```
    ↑{(0∘,+,∘0)⍣⍵,1}¨∘⍳
```

that made me wonder how to explain how to parse an expression like this. It is supposed to be read right-to-left but operators interfere with this technique and now with trains we need to revisit this.

This expression is a 2-train (called an *Atop*), a function made of 2 functions juxtaposed.

```
    ┌───┬──────────────────────┐
    │ ↑ │ {(0∘,+,∘0)⍣⍵,1}¨ ∘ ⍳ │
    └───┴──────────────────────┘
```

Dyalog V.14 introduced trains.

A train is a series of functions lined up one after the other IN ISOLATION as in `(+÷×!-)` , a train of 5 functions. The functions can be primitives, derived, composed, Dfns, other trains and even user defined traditional functions but they must be in isolation. In *isolation* means the functions are isolated from data. In `(+÷×!-)5` the 5-function train is isolated from the 5, whereas in `+÷×!- 5` it is not.

A two-train function is called an *Atop*. A three-train function is called a *Fork*. More than three function trains are reduced to two or three by considering the three rightmost functions (a Fork) as a function.

Let's see why this expression is a train

Reading from right to left:

1. The first symbol is *Iota*, the Index generator function.
2. The second symbol is *Jot*, the *Compose* operator. *Compose* accepts two operands, each may be a function or an array.
3. The third symbol is *Dieresis*, another operator named *Each*, which means the left operand to *Compose* is not an array. It is a derived function, made of another function which must be bound to *Each* for *Compose* to work.
4. The fourth symbol is right brace `}` which marks the end of a D-fn. Scanning left until we reach its matching `{` we get the function `{(0∘,+,∘0)⍣⍵,1}¨∘⍳`
5. The next symbol is a function, `↑`, and it is not bound to `{` and the entire line, except for the leftmost character *Up Arrow*, is the rightmost function of the Atop.

The only symbol left, *Up Arrow* `↑`, is the other function and this line constitutes the definition of a two-train function, an Atop. This can be seen as

```
    P1← ↑
    P2← {(0∘,+,∘0)⍣⍵,1}¨ ∘ ⍳
    P ←  P1  P2
```

OK, but what does it do?

Let's have a better look at `P2`. This is a Dfn bound to *Each* as left operand to *Compose* and with *Iota* as the right operand. `P2`'s argument (`n`) is given to *Iota*, generating a series of `n` integers each of which is fed to the Dfn one by one (because of *Each*) making effectively `n` separate calls to the Dfn.

Let's have a look at that Dfn in more details:

Reading from the right inside the Dfn ‘`,`’ is the first function encountered but the object to its left is bound to the operator *Power* (`⍣`) so ‘`,`’ is monadic, it is the *Ravel* function. It turns the scalar 1 to its right into a 1-element vector containing the number 1.

*Power* only accepts functions as left operand so what's in parentheses to its left better be a function or we'll get a SYNTAX ERROR.

What's within parentheses and omega constitute the function applied to `,1` . In the Dfn `{(0∘,+,∘0)⍣⍵ ,1}` the function `(0∘,+,∘0)` is applied *omega* times to `,1` . We could rewrite `P2` like this:

```
    F← (0∘,+,∘0)  ⋄  P2← { F⍣⍵ ,1}¨ ∘ ⍳
```

Let's have a look at `F` more closely. It's a *Fork*. This is the first function:

<pre>
          <strong>┌───┐</strong>
    0∘, + <strong>│,∘0│</strong>
          <strong>└───┘</strong>
</pre>

it is ‘`,`’ bound to 0 via the *Compose* operator. It is a function that concatenates 0 **after** its argument.

Operators bind their arguments **before** the functions can make up trains. This is a rule which did not exist before because trains of functions did not exist then.

It is preceded by another function *Plus* which is not bound to anything:

<pre>
        <strong>┌─┐ </strong>
    0∘, <strong>│+│ </strong>,∘0
        <strong>└─┘ </strong>
</pre>

This in turn is preceded by another function made of 0 bound to ‘`,`’ via the *Compose* operator. It is a function that concatenates 0 **before** its argument.

<pre>
    <strong>┌───┐</strong>
    <strong>│0∘,│</strong> + ,∘0 
    <strong>└───┘</strong>
</pre>

The function is a three-function train. In this case it is a *Fork*.

```
      ┌───┬─┬───┐
    F←│0∘,│+│,∘0│
      └───┴─┴───┘
```

Applying this function once to `,1`

```
    (0∘, + ,∘0 ) ,1
1 1
```

Is the same as

```
    (0∘, ,1) + (,∘0 ,1)
1 1
```

or

```
    0 1 + 1 0
1 1
```

Applying this function twice to `,1`

```
    F F ,1    ⍝ or F⍣2 ,1
```

Is the same as `F 1 1,` or the same as

```
    (0∘, 1 1) + (,∘0) 1 1
```

or

```
    0 1 1 + 1 1 0
1 2 1
```

Applying this function thrice to `,1` is

```
    F F F ,1  ⍝  or F⍣3 ,1
```

Is the same as `F 1 2 1,` or the same as

```
    (0∘, 1 2 1) + (,∘0) 1 2 1
```

or

```
    0 1 2 1 + 1 2 1 0
1 3 3 1
```

Going back to the original function:

```
    P ← ↑  {(0∘,+,∘0)⍣⍵,1}¨ ∘ ⍳
```

This is a train of two functions. Which again can be seen as

```
    P ← P1 P2
```

P2 applied to an integer e.g. 4, **in origin 0,** is

```
    {(0∘,+,∘0)⍣⍵,1}¨ ∘ ⍳4
```

or

```
    {(0∘,+,∘0)⍣⍵,1}¨ 0 1 2 3
```

or

```
    (,1) (1 1) (1 2 1) (1 3 3 1)
```

Applying `P1(↑)`, **with ⎕ML 1**, to this array results in

```
1 0 0 0
1 1 0 0
1 2 1 0
1 3 3 1
```

And going back to the original function:

```
    P← ↑ {(0∘,+,∘0)⍣⍵,1}¨ ∘ ⍳
    P  4
1 0 0 0
1 1 0 0
1 2 1 0
1 3 3 1
```

It generates the Pascal Triangle.

## Conclusion

This kind of code will appear more and more often in the future. Even if we do not intend to use all the new features yet we need to be able to read them and understand what it means. APL is evolving all the time, just like any other language. English, Russian, Mandarin weren't spoken the same way 300 years ago and they won't be the same 300 years hence. APL wasn't the same 30 years ago; it WILL be different in the future. We need to adapt.

## Note

You can "see" this article in a video here: [youtu.be/kt4lMZbn-so](http://youtu.be/kt4lMZbn-so) or search for APLtrainer in YouTube to find it.
