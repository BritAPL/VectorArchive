---
vid: '10500630'
title: 'Functional Calculation 5: Operations'
prefix: Functional Calculation
authors:
- Neville Holmes
byline: by Neville Holmes (neville.holmes@utas.edu.au)
volume: '25'
issue: '1'
page: '95'
received: '2010-11-16'
online: '2011-05-02'
editor: Stephen Taylor
description: This article is the sixth in a series on functional calculation. It introduces the use of certain operators which can be applied to functions and values to produce new functions.
abstract: This article is the sixth in a series expounding the joys of functional calculation. Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. The functional notation used as the vehicle in this series is provided by a freely available calculation tool called J. This article makes a start to introducing those functional calculation capabilities, in particular the use of certain operators which can be applied to functions and values to produce new functions.
source: content/printed/251/fc05.htm
converter: vecrescue 0.1.0
issue_tab: n1-june-2011
tags:
- mathematics and statistics
- education
- language design
---

by Neville Holmes (neville.holmes@utas.edu.au)
{ .byline }

This article is the sixth in a series expounding the joys of functional calculation. Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. The functional notation used as the vehicle in this series is provided by a freely available calculation tool called J. This article makes a start to introducing those functional calculation capabilities, in particular the use of certain operators which can be applied to functions and values to produce new functions.
{ .abstract }

## Functional calculation

The description so far has been of numerical calculation, that is, of functions which can be applied to numbers to produce other numbers, though there has been some consideration of structures of characters and boxes. Much is possible using the J notation through such simple numerical calculation, because the notation provides a rich variety of primitive functions, that is, of functions that have symbols like `+` and `<.` and `%:` instead of names given by the user, names like `x` and `foo` and `Herbert`.

What remains to be described is how functional calculation can be built, uniformly and consistently, upon the numerical calculation provided by J.

There are two ways in which functional expressions can be built up – by juxtaposing functions in *trains*, and by applying *operations* to functions and values. Of course the two methods may be combined. Here operations are reviewed while the use of trains is deferred.

In J the definition of functions is exactly the same as the definition of results – it’s simply a matter of naming. Naming is done using the `=.` or `=:` copulas, of which the latter is more general in providing a definition which holds globally. Thus the square root of 999 is a numerical result and is named by

```
   sr =: %: 999
```

while the natural log of the difference[\[1\]](#ref1) is a function and is named by

```
   ld =: ^. @ -
```

where the `^.` and the `-` are functions, and the `@` is an operation, as is about to be explained.

## Operations

Operations are to functions what functions are to values. Primitive operations are given special symbols, and can be monadic or dyadic, but not both. In this they differ from primitive functions which can be used both monadically or dyadically.

The point about operations is that they are applied to produce functions, whereas functions are applied to produce values, that is, numbers or characters or boxes. As higher level entities, operations apply themselves to their operands more strongly than functions apply themselves to their arguments. Two primitive operations have been briefly considered in previous articles.

The symbol `˜` stands for a primitive monadic operation, and its operand (the function to its left) is always applied dyadically. Thus, the argument of the function it produces when used monadically is used both as the left argument and the right argument. Otherwise, the arguments of the function it produces when used dyadically are reversed, or *commuted*, the right argument being used as the left and the left argument being used as the right.

The symbol `/` stands for a primitive monadic operation, and the function it produces when used monadically applies its operand dyadically between all the items of its argument. So `+/` applied monadically to a list of numbers will add them up.

There are two dyadic operations which can be used to combine functions to make new ones, as illustrated in the following diagram.

<table class="left">
		<tr>
			<td class="code">  f&amp;g y</td>
			<td class="code">x f&amp;g y</td>
			<td class="code">           </td>
			<td class="code">  f@g y</td>
			<td class="code">x f@g y</td>
		</tr>
		<tr>
			<td>
<pre>
f
 ↖
   g
    ↖
      y
</pre>
			</td>
			<td>
<pre>
   f
 ↗ ↖
g     g
 ↖    ↖
   x     y
</pre>
			</td>
			<td></td>
			<td>
<pre>
f
 ↖
   g
    ↖
      y
</pre>
			</td>
			<td>
<pre>
f
 ↖
   g
 ↗  ↖
x     y
</pre>
			</td>
		</tr>
		<tr>
			<td class="caption">Monadic After</td>
			<td class="caption">Dyadic After</td>
			<td></td>
			<td class="caption">Monadic Of</td>
			<td class="caption">Dyadic Of</td>
		</tr>
	</table>

Syntactically, what holds for primitive functions also holds for composed functions. Where a primitive function can be used so also can a function produced by an operation. A function, primitive or composed, can be used monadically or dyadically, and this is independent of whether any component operation is monadic or dyadic.

Note particularly that an expression like

```
   x f@g y
```

can be keyed in directly for evaluation, but that the expression

```
   x f g y
```

does *not* have the same meaning, even though it might give the same result.

In some of the literature, dyadic operations are called *conjunctions*, while monadic operations are called *adverbs*, by analogy with the conventional names for parts of speech in natural language. This is a dubious analogy, but the names are useful and will be adopted here.

## Adverbs

The simpler operations are the adverbs. They only have one operand, to their left; conjunctions have two. Most primitive adverbs are structural, that is, their operand is a function, and the adverb controls how the operand is applied amongst the items of the composed function’s argument or arguments. The table lists the adverbs discussed in the following.

<table class="bdr left">
	  <tbody style="vertical-align: bottom">
		<tr>
		  <td class="code">˜</td>
		  <td>both</td>
		  <td>swap</td>
		  <td></td>
		  <td></td>
		  <td></td>
		</tr>
		<tr>
		  <td class="code">/</td>
		  <td>across</td>
		  <td>between</td>
		  <td class="code">/.</td>
		  <td>diagonals</td>
		  <td>sequester</td>
		</tr>
		<tr>
		  <td class="code">\</td>
		  <td>prefixes</td>
		  <td>infixes</td>
		  <td class="code">\.</td>
		  <td>suffixes</td>
		  <td>exfixes</td>
		</tr>
		<tr>
		  <td class="code">}</td>
		  <td>extract</td>
		  <td>amend</td>
		  <td class="code">b.</td>
		  <td>basic</td>
		  <td></td>
		</tr>
		<tr>
		  <td></td>
		  <td></td>
		  <td></td>
		  <td class="code">f.</td>
		  <td>fix</td>
		  <td>fix</td>
		</tr>
	  </tbody>
	</table>

### Moving arguments

The simplest primitive adverb has `˜` for its symbol. It always uses its operand as a dyadic function, but the function it produces may of course be used monadically or dyadically.

If its result, say `f˜`, is used monadically, then the argument is used as both arguments of the operand function. The expression `f˜x` is equivalent to `x f x`. For example, `+˜x` will double `x`, while `*˜x` will square it.

If `f˜` is used dyadically, then the arguments are swapped for the operand function. The expression `x f˜y` is equivalent to `y f x`. For example, `x-˜y` will subtract `x` from `y`, not `y` from `x`, while `x%˜y` will divide `x` *into* `y`, not *by* it.

The dyadic use of this adverb is convenient to streamline thought by removing parentheses. Thus (expression) `f x` may be rewritten `x f˜` expression.

### Inserting functions

The primitive adverb with symbol `/` is structural at a lower level, causing its operand to be inserted between the items of the argument or arguments of the function it produces.

If its result, say `f/`, is used monadically, then it is as though `f` is inserted between the items of the argument of `f/` however many items there might be. In this case `/` is often pronounced *across* or *insert*. For example, `+/x` will add up the items of `x`, `*/x` will multiply them up, and `;/x` will put each in a box.

If `f/` is used dyadically, then it is as though `f` were inserted between the items of the left argument of `f/` and the items of its right argument, each and every one of them at least for scalar functions. In this case `/` is often pronounced *between* or *table*. For example, `x+/y` will, if `x` and `y` are lists, make a table of the sums of the items of `x` and of `y`, while `x*/y` will make a table of their products. Conveniently, `*/˜>:i.12` will produce a 12×12 multiplication table, and, using functions other than `*` as operand, other tables may be similarly produced.

The function `f/` is often spoken of as *the f reduction* when used monadically because its effect is normally to reduce the rank of its argument, at least when `f` is a scalar function. The aspect of most interest here is that a list is reduced to a scalar, so that means an empty list like `i.0` or `$9` must reduce to a scalar. That scalar must be the identity value for the reducing function, so that `+/i.0` will yield `0`, while `*/$7` will yield `1`.

More complex ways of inserting functions use symbols that look like the `/` symbol. However, in all cases that follow, the operand is applied monadically by the adverb, unlike the operand of `/` which is applied dyadically.

- Used monadically, `f\` will apply its operand to successive prefixes of its argument. For example, `<\` will show those successive prefixes boxed, and `+/\` will yield progressive sums.
- Used dyadically, `f\` will apply its operand to successive subsequences of its right argument of the size specified by its left argument. When its left argument is negative, the subsequences are consecutive within their argument, if positive their heads are consecutive. Thus `2+/\y` will yield the sums of distinct pairs of `y`, giving a result with half as many items as `y`, but `2-˜/\y` will yield the first differences, that is, it will subtract each item except the last from its immediately following item giving a result with one item fewer than y.
- The adverb `\.` is just like `\` except the subsequences included by `\` are excluded by `\.` so that `#\i.3` yields `1 2 3` while `#\.i.3` yields `3 2 1`.
- Monadic `/.` applies its operand to diagonals of its argument, while dyadic `/.` applies its operand to subsequences of the right argument selected according to the key given by the left argument.

### Other adverbs

The *amend* adverb with symbol `}` behaves in a more complex way. In the first place, its operand may be a function or it may be a value. In the second place, the `}` adverb is closely associated with the function `{`, an unlikely association.

Superficially, monadic `x}` looks like monadic `x&{` when their argument is of rank one. Thus, `4}i.7` yields the same as `4&{i.7` but their behaviour diverges when more complex arguments and operands are used.

Dyadically, after `w=:x z} y` is carried out, `z{w` will yield `x` in simple cases. The basic idea of dyadic amendment is that, using the example just given, the operand `z` specifies which elements of `y` the elements of `x` are to replace.

The *amend* adverb is too complex to explain further here, except that, where the operand is a function, that function is applied to the overall function’s argument or arguments to give a result that becomes the effective operand as already described.

A couple of housekeeping adverbs are *basic* and *fix*, spelt `b.` and `f.` respectively. The basic adverb produces a monadic function which, for an argument of `_1` yields a character string showing the obverse of `b.`’s operand, for an argument of `0` a numeric list of the ranks of the operand, and for an argument of `1` a character string showing the identity function of the operand. The obverse is needed for the `˜:` and `&.` conjunctions, and can be defined by the `:.` conjunction. The *fix* adverb yields its operand function, but redefined entirely in terms of primitives. All contained definitions are eliminated, so once a function is fixed it can no longer be changed by changing other definitions.

## Conjunctions

The more complex operations are the conjunctions, more complex because they have two operands. Some primitive conjunctions are strictly compositional, their operands being functions, and the conjunction controlling how the operands are applied to the composed function’s argument or arguments. Other primitive conjunctions are structural, that is, they have one operation that is a function, and one that is a value which modifies how the other operand, the function, is applied to the conjunction’s argument or arguments.

Here is a table of the simpler conjunctions.

<table class="bdr left">
	  <tbody style="vertical-align: bottom">
		<tr>
		  <td></td>
		  <td></td>
		  <td></td>
		  <td class="code">;.</td>
		  <td>cut</td>
		  <td>cut</td>
		  <td class="code">^:</td>
		  <td>power</td>
		  <td>power</td>
		</tr>
		<tr>
		  <td></td>
		  <td></td>
		  <td></td>
		  <td class="code">!.</td>
		  <td>fit</td>
		  <td>fit</td>
		  <td class="code">!:</td>
		  <td>foreign</td>
		  <td>foreign</td>
		</tr>
		<tr>
		  <td class="code">"</td>
		  <td>rank</td>
		  <td>rank</td>
		  <td></td>
		  <td></td>
		  <td></td>
		  <td class="code">L:</td>
		  <td>level</td>
		  <td>level</td>
		</tr>
		<tr>
		  <td class="code">@</td>
		  <td>of</td>
		  <td>of</td>
		  <td></td>
		  <td></td>
		  <td></td>
		  <td class="code">@:</td>
		  <td>of</td>
		  <td>of</td>
		</tr>
		<tr>
		  <td class="code">&amp;</td>
		  <td>after</td>
		  <td>after</td>
		  <td class="code">&amp;.</td>
		  <td>dual</td>
		  <td>dual</td>
		  <td>&amp;:</td>
		  <td>after</td>
		  <td>after</td>
		</tr>
	  </tbody>
	</table>

But, before discussing more general primitive conjunctions, it’s useful to review one of the simplest of them, *value bonding*, because it is perhaps the most versatile.

### Value bonding

The symbol for bonding is `&`, the ampersand. In value bonding, one of the operands is a function, and one is a value.

For example, used as a monadic function `3&+` will add 3 to its argument, while `%&5` will divide its sole argument by 5. The value operand doesn’t have to be a scalar, nor does it have to be numeric.

On the other hand, used as a dyadic function `a 3&+` will add 3 to its right argument `a` times, while `a %&5` will divide its right argument by 5 `a` times. If `a` is zero, nothing will happen, but if `a` is negative, the inverse function will be carried out `-a` times.

### Compositions

Bonding can also be used with two functional operands. For a function composed in this way, the right operand is always used monadically, being applied to each argument when there are two, and the left operand being applied to the result or results from the right operand. Thus `^.&%:y` is the same as `^.%:y` (the *ln* of the square root of `y`), while `x^.&%:y` is the same as `(^.x)%:(^.y)` (in which the second pair of parentheses are included for aesthetic reasons), the *ln* `x` root of the *ln* of `y`.

The other simple function-combining primitive conjunction uses the `@` as its symbol, and applies its left operand to the result of its right operand, which is supplied with whatever argument or arguments the composed function is given. Thus `^.@%:y` is the same as `^.&%:y`, but `x^.@%:y` is the same as `^.(x%:y)` the *ln* of the `x` root of `y`.

The following diagram shows the nature of these two most common conjunctions.

<table class="left">
		<tr>
			<td class="code">  f&amp;g y</td>
			<td class="code">x f&amp;g y</td>
			<td class="code">           </td>
			<td class="code">  f@g y</td>
			<td class="code">x f@g y</td>
		</tr>
		<tr>
			<td>
<pre>
f
 ↖
   g
    ↖
      y
</pre>
			</td>
			<td>
<pre>
   f
 ↗ ↖
g     g
 ↖    ↖
   x     y
</pre>
			</td>
			<td></td>
			<td>
<pre>
f
 ↖
   g
    ↖
      y
</pre>
			</td>
			<td>
<pre>
f
 ↖
   g
 ↗  ↖
x     y
</pre>
			</td>
		</tr>
		<tr>
			<td class="caption">Monadic After</td>
			<td class="caption">Dyadic After</td>
			<td></td>
			<td class="caption">Monadic Of</td>
			<td class="caption">Dyadic Of</td>
		</tr>
	</table>

The conjunctions which use the `&` and `@` are the most commonly used, but there are several related ones that are of interest.

- The *dual* or *under* conjunction uses the symbol `&.` and applies the obverse (which is usually its inverse) of its right operand to the result of the bond of its operands. This is very useful with a `>` (*unbox*) right operand to allow the contents of boxes to be worked on then put back into boxes.
- For primitive functions the obverse is usually the inverse, but the `:.` conjunction yields its left operand with its obverse defined as its right operand. The obverse is used by the `^:` conjunction described below, as well as by `&.` as just described.
- The symbol `::` stands for the *adverse* conjunction, very like obverse but using its right operand for the replacement upon error of its left operand rather than for its obverse.
- The symbols `..` and `.:` stand for the even and odd conjunctions, though the other uses of the `.` symbol means that usually a blank character must precede these conjunctions to make their meaning unambiguous and plain. In brief, `f ..g` is `-:@(f+f&g)` while `f .:g` is `-:@(f-f&g).` The names describe the effect when `g=.-` since they then yield the even and odd parts of the function `f`.

### Modifying functions

The primitive conjunctions that modify functions typically use the value of their right operand to change the application of their functional left operand to the arguments of the function produced.

Perhaps the most interesting, and at the same time the most complex, of modifying primitive conjunctions is the *rank* conjunction, expressed by the ditto (`"`) symbol. When its left operand is a function, its right operand specifies the rank to be used for the items of the modified function’s argument or arguments.

## Summary

This article is like a list of ingredients that can be used for combining functions using notation provided by J. At their simplest, these ingredients can be used to define functions that are more complex than the primitive functions but which combine those primitive functions in a variety of ways.

The operations described here, together with those not yet considered, provide the basic means of functional programming. Yet to be considered are trains of various kinds, which will soon be described.

However, the next article in this series will illustrate how operations can be used with functions as a preliminary to treatment of trains.

## Postscript

Some explanation of the brevity and content of this article seems needed for new readers of the series and for anyone thinking of using the series for teaching.

The articles were written over a decade ago and were each handed out to students attending a two hour lecture that used the J interpreter’s workings displayed on a large screen to show in more detail the meaning of what is gone over in the article handed out at the beginning of the lecture. Any queries by students were answered on the spot by use of the interpreter.

At the end of the lecture students were given an article such as the previous one published here, that is, an article giving many examples of the use of the matter of the lecture. While the exemplary articles were based on using the digits of a year to generate the first hundred integers, the students were required to mimic the examples but using the digits of their student identification number. The code used to mark their work would, they were told, give credit for the variety of expressions and for their brevity.

## Notes

1. <a id="ref1"></a> Properly speaking, the *difference* is the magnitude of the subtraction, but English doesn’t have a convenient unambiguous word for the result of a subtraction, so *difference* has its loose meaning here.
