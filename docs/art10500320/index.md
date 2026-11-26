---
vid: '10500320'
title: 'Functional calculation 3: Structural Ingredients'
subtitle: '3: Structural Ingredients'
authors:
- Neville Holmes
byline: by Neville Holmes (neville.holmes@utas.edu.au)
volume: '24'
issue: '2'
page: '114'
description: Functional calculation in J, Part 3
abstract: This article is the fourth in a series expounding the joys of functional calculation. Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. The functional notation used as the vehicle in this series is provided by a freely-available calculation tool called J.
source: content/printed/242/holmesfc3.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- education
- mathematics and statistics
---

3: Structural Ingredients
{ .subtitle }

by Neville Holmes (neville.holmes@utas.edu.au)
{ .byline }

This article is the fourth in a series expounding the joys of functional calculation. Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. The functional notation used as the vehicle in this series is provided by a freely-available calculation tool called J.
{ .abstract }

This article reviews the numerical calculation capabilities of J which enable lists and tables to be used in calculations. To understand the functional calculation capabilities proper, structural capabilities – dealing with tables and lists – must first be understood. The capabilities described in this article will be illustrated and explained in detail in the next article.

## Calculation

Calculation is generally reckoned to be the systematic manipulation of numeric values. Previous articles in this series have reviewed the arithmetic functions available in J, the interpreter used here as the vehicle for numerical calculation.

The numerical calculation described so far is merely an elaboration of what might be achieved by a conventional electronic calculator. There is an elaboration of ways numbers can be expressed, and an elaboration of functions that can be applied to any number, but that is all.

However rich the set of arithmetic functions available to the user, they cannot do much more than an ordinary pocket calculator can do, unless those arithmetic functions can be applied to lists of numbers, and to lists of lists. Functions that manipulate lists and tables, functions beyond the merely arithmetical, are essential to support the arithmetic functions, and these functions can be called *structural*.

The next article will illustrate the use of these structural functions. They provide an elaboration still within the realms of numerical calculation, namely the ability to structurally manipulate more than one number at a time. Of course, the arithmetic functions already described can also be applied to lists and tables of numbers.

## Building structures

In the first place, constant lists of numbers can be keyed in simply by separating the elements of the list by blanks. So what ? Well, the scalar functions can be applied to compatible lists, that is, to lists with the same number of elements: `3 4 5*6 4 2` yields `18 16 10`, for instance. And if one of the arguments is a scalar (not a list, but a naked number) then it is coupled to each element of the other argument: `60%3 4 5` yields `20 15 12`, for instance.

But other functions are needed, functions that allow lists to be built easily, that allow lists of lists to be built, that allow lists of different kinds to be built. The following table gives the symbols and names for some such functions.

<table class="bdr center left">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
      <tbody style="vertical-align: bottom">
        <tr>
          <td>$</td>
          <td>shape</td>
          <td>reshape</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>#</td>
          <td>tally</td>
          <td>copy</td>
          <td>#.</td>
          <td>unbits</td>
          <td>undigits</td>
          <td>#:</td>
          <td>bits</td>
          <td>digits</td>
        </tr>
        <tr>
          <td>?</td>
          <td></td>
          <td>deal</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>&lt;</td>
          <td>box</td>
          <td></td>
          <td>+.</td>
          <td>cartesian</td>
          <td></td>
          <td>":</td>
          <td>format</td>
          <td>format</td>
        </tr>
        <tr>
          <td>&gt;</td>
          <td>unbox</td>
          <td></td>
          <td>*.</td>
          <td>polar</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>;</td>
          <td>raze</td>
          <td>link</td>
          <td>i.</td>
          <td>integers</td>
          <td></td>
          <td>;:</td>
          <td>words</td>
          <td></td>
        </tr>
      </tbody>
    </table>

The *reshape* function is the most useful of these, and its explanation covers many important issues about structures.

- The left argument of `$` specifies what shape is to be given to the result, the right argument supplying the item or items to be used in the result, and these items are reused cyclically if necessary. Thus, `5$6` yields `6 6 6 6 6`, and `5$6 7` yields `6 7 6 7 6`.
- Lists of lists may be directly formed, for instance, `2 3$4` yields a two item list, each item of which is itself a list of three `4`s. This is very much like a matrix or table with two rows and three columns. If a table is reshaped then the items that go into the new structure are the rows of the table. Thus the expression `4$3 2$1` will yield a table of four rows and two columns.
- Monadic `$` yields the *shape* of its argument, always a list, so the shape of a shape yields a single item list which gives the number of dimensions of the original argument, its *rank*. The rank of a table is two, of a list, one, and of a scalar, zero.
- A structure can have a shape of `0`, that is, it may hold no elements at all. Such a structure is called an *empty* list, and is of particular interest under calculation. Consider, for example, what the average of an empty list should be. Consider also that a table might be empty because it has no rows, or empty because it has no columns, and not necessarily both.
- A list of one item, where that item is an element (not a list), is not the same thing as a scalar. A list of one elementary item has a shape of `1`, while the shape of a scalar is the empty list.

The other functions in the table have more particular properties, and their explanation also raises some interesting issues.

- The *copy* function is like a reshaping, except the items are reshaped individually. Thus, `2#3 4` yields `3 3 4 4`, and `2j1 1j2#3 4` yields `3 3 0 4 0 0`.
- *Tally* gives a simple count of the items in its argument. Thus the tally of a scalar is the same as the tally of a list with one element, `1`, though their shapes are different. The tally of an empty list will always be `0`, but the tally of an empty table will only be `0` if it has no rows.
- <em>Integer</em>s: monadic `i.` is like a reshape but the argument gives the shape of the result, and the elements being reshaped are the nonnegative integers taken in sequence. *Deal* randomly selects from the non-negative integers smaller than its right argument as many as specified by its left argument. Of course the results of both roll (monadic `?`) and deal are only pseudorandom, and the pseudorandom seed can be both inspected and set.
- *Cartesian* and *polar* split numbers in their arguments into two components, as their names suggest. *Bits* and *digits* split their argument numbers into digits, binary in the monadic case, to the base given in the left argument in the dyadic case. *Unbits* and *undigits* reverse the effect of the previous two functions, and so don’t really build structures.
- Here it should be stressed that a structure must be composed entirely of the same kind of element, and there are three kinds of elements – numbers, characters, and boxes. (In fact J also provides for structures of functions, but these are passed over here.) Boxes are explained just below. A character string is a list of characters. For example, `$'this'` yields `4`. The two *format* functions are used to convert numbers to character strings, while *do* can convert character strings to numbers. Incidentally, there is a special name `a.` which stands for the *alphabet*, the list of all possible characters.
- It is often useful to combine different items into a structure, for example to mix numbers and their names, or to mix structures of different shapes. This can be done by putting them into boxes using *box*, which produces a scalar box, or using *link*, which produces at least two boxes in a list of boxes, or *words*, which when applied to a character string yields a boxed list of the words and punctuation in the string. *Words* is particularly useful to apply to a J expression because it boxes its syntactic components, which can help to understand an expression. Arithmetic cannot be done on characters, nor on boxes, but the *unbox* (or *open*) function can remove the box that holds a structure, and *raze* can remove the boxes from a structure of boxes.

## Rebuilding structures

In contrast to functions that build structures from typically lesser structures, like scalars, there are functions that rearrange structures, or extract lesser structures from greater. Such functions are given in the following table.

<table class="bdr center left">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
      <tbody style="vertical-align: bottom">
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td>|.</td>
          <td>reverse</td>
          <td>rotate</td>
          <td>|:</td>
          <td>transpose</td>
          <td>transpose</td>
        </tr>
        <tr>
          <td>,</td>
          <td>ravel</td>
          <td>append</td>
          <td>,.</td>
          <td>knit</td>
          <td>stitch</td>
          <td>,:</td>
          <td>itemise</td>
          <td>laminate</td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td>/:</td>
          <td></td>
          <td>sort up</td>
        </tr>
        <tr style="border-bottom: 1px solid gray">
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td>\:</td>
          <td></td>
          <td>sort down</td>
        </tr>
        <tr>
          <td>{</td>
          <td></td>
          <td>from</td>
          <td>{.</td>
          <td>head</td>
          <td>take</td>
          <td>{:</td>
          <td>tail</td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td>}.</td>
          <td>behead</td>
          <td>drop</td>
          <td>}:</td>
          <td>curtail</td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td>~.</td>
          <td>nub</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
      </tbody>
    </table>

Functions that merely rearrange their components are those shown in the first four rows of the table.

- *Reverse* reverses the sequence of the items of its argument, while *rotate* cyclically shifts the items of its right argument a number of places depending on its left argument. The shift is to the left for a positive number, to the right for a negative. If the right argument is a table then the left argument can be a list of two numbers, the first saying how far the items are to be rotated, the second how far the elements within each item.
- The *transpose* functions rearrange the axes of their right argument. This can be seen as a rearrangement of the elements of the shape of the right argument, a kind of second order rearrangement. Thus, monadic *transpose* will make the rows of a table become its columns, and vice versa. Dyadic *transpose* can be used simply to rearrange axes of structures of rank greater than two, but it can also be used to run axes together and to pick out diagonals of various kinds.
- *Sort up* and *sort down* are anomalous compared to the functions just described, because they reorder the items of their left argument rather than of their right! The reordering sequence is defined by the lexical sequence of the items of the right argument, using `a.` to define the collating sequence of characters in the case of a character structure for right argument. In particular, `x/:x` will yield a list of the items of `x`  in ascending sequence, and `x\:x` in descending.
- *Ravel* runs all the axes of its argument together, yielding a simple list of the elements of the argument. *Append* runs together the items of its arguments, and will expand a scalar argument if necessary. *Knit* ravels the items of its argument, while *stitch* appends corresponding items of its arguments. Understanding the difference as described between these two pairs of functions depends on appreciating that the items of a structure are the components of its highest level list. Thus, `1 2,3 4` yields a list of four items, while `1 2,.3 4` yields a two by two table.
- *Itemise* yields its argument as a single item list, while *laminate* appends its arguments after itemising each of them. Note that `2 3$4 5` does not yield the same result as `2$,:3$4 5`, though the shape of the results is the same.

The functions given in the lower section of the preceding table typically pick out items from their right argument.

- *Head* yields the first item of its argument, *tail* the last; *behead* all but the first, *curtail* all but the last.
- *Take* with a positive left argument yields that many leading items of its right argument; with a negative, that many trailing items. The *take* function can extend an existing structure, normally with `0`s in the case of a numeric structure; with blanks in the case of a character structure; or with empty boxes in the case of a box structure. Thus, `3{.6` yields `6 0 0`, and `_3{.4 5` yields `0 4 5`. One little trap is that although the result of *head* looks the same as the result of *one take*, their shapes are different.
- *Drop* is the opposite to *take* in the obvious way. *Nub* takes all the distinct items of its argument, dropping duplicates out.
- *From* selects from its right argument the items indexed by its left argument. Here it is important to realise that the indexes start from zero, so that `1{x` will pick out the second item of `x`. Interestingly, `_1{x` will pick out the last element of `x`.

## Structural data

Finally, there are a few interesting functions which, like shape and tally, extract data about their arguments.

<table class="bdr center left">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
      <tbody style="vertical-align: bottom">
        <tr style="border-bottom: 1px solid gray">
          <td></td>
          <td></td>
          <td></td>
          <td>%.</td>
          <td>invert</td>
          <td>project</td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>=</td>
          <td>classify</td>
          <td>           </td>
          <td></td>
          <td></td>
          <td></td>
          <td>-:</td>
          <td></td>
          <td>match</td>
        </tr>
        <tr style="border-bottom: 1px solid gray">
          <td></td>
          <td></td>
          <td></td>
          <td>e.</td>
          <td>raze in</td>
          <td>member</td>
          <td>~:</td>
          <td>sieve</td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td>i.</td>
          <td></td>
          <td>index</td>
          <td>/:</td>
          <td>grade up</td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td>\:</td>
          <td>grade down</td>
          <td></td>
        </tr>
      </tbody>
    </table>

These are something of a mixed bunch, *invert* and *project* being simply a generalisation of matrix inversion and matrix division.

The functions given in the vertically centre section of the table all produce boolean values, that is, values composed only of `0`s and `1`s, like the results of the comparison functions.

- *Match* is a kind of hypercompare, comparing its arguments in their entirety, yielding a `1` if they match in every respect, and a `0` otherwise.
- *Sieve* marks the occurrence of the first instance of each distinct item of its argument. Thus, `(~:x)#x` yields the nub of `x`.
- *Member* works in two ways. If its left argument has the same shape as the items of its right argument, then it looks for a match for its left argument with any item of its right argument. Otherwise it reports whether the elements of its left argument are present anywhere in the right argument.
- *Classify* and *raze in* both produce a boolean table. For classify, the rows of the table correspond to the items of the nub of the argument, and within each row the occurrences of the item of the nub are marked for each item of the argument. For structures of numbers or characters, raze in will mark the occurrence of each of the elements of its argument in its ravel. Thus `e.i.4` will produce the unit matrix of size four, while `$e.i.2 3` will yield `2 3 6`. For structures of boxes, membership of the elements within each box are marked for each element of the raze.

Given that so many results depend on a comparison of numbers, it is important to realise that these comparisons are done with a certain amount of tolerance to allow for the imperfection of traditional computer arithmetic, remembering that, for example, present day computers cannot store the value `1r3` exactly. However, the J interpreter does provide for the comparison tolerance to be respecified, even to zero.

The functions given in the bottom section of the table all yield an index to items of the left or only argument.

- The *index* function is very frequently used, and yields, for the items of its right argument, their location in the left argument. If an item is not present in the left argument then it is given the first invalid location. Thus, `(i.5)i.2 3 3.5 4 5 6` yields `2 3 5 4 5 5`.
- *Grade up* and *grade down* yield the permutation index of their argument that would select its items in the required sequence from its argument. Thus, `(/:x){x` will yield the list of `x`’s items in ascending sequence.

## Summary

This article is like a list of ingredients that can be used for structural calculation using the notation provided by J. These ingredients allow lists of numbers and tables (lists of list) of numbers to be manipulated.

The arithmetic functions described and illustrated in previous articles can also be applied to lists and tables. Together, the arithmetic functions and structural functions provide a very powerful capability for relatively complex numerical calculations. This capability is needed as the basis for functional calculation proper, as will be described in later articles of this series.

However, the next article is devoted to illustrating simple use of the structural functions described above.
