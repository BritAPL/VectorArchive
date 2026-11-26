---
vid: '10000770'
title: SHARP APL Operators
authors:
- Dan Baronet
volume: '16'
issue: '3'
page: '85'
source: trad/v163/dan163.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2000
tags:
- language design
- implementations
---

Note that this article contains quite a bit of APL code - this is in APL2741 font, [downloadable](resource/apl2741.zip) from this site (32K). There is a new problem in showing the \{match\} symbol in IE5 (it is treated as an optional hyphen), so it has now been duplicated in the font at position ch(166) which will paste into APL\*PLUS as the split-stile character.

## SHARP APL Operators

*by Dan Baronet ([danb@dsuper.net](mailto:danb@dsuper.net))*

## Introduction

Following Soliton’s announcement to unleash their APL into the Linux community I decided to review the differences between SHARP APL<sup>®</sup> and APL2 **style** APLs (IBM’s APL2, Dyalog, APL+Win). I have divided the report into several articles, each with a different topic.

This article is the second of the series. In the first one I showed the differences between the languages and enclosed arrays handling. This one is specific to the operators in SHARP APL.

Not having a current version of *IBM’s* APL2 and *APL+Win* on hand I have been making my observations based on *Dyalog* APL using `⎕`ml=2.

When reading this text keep in mind the date it was written (September 1999) as languages change and may render this information incorrect in the future.

## Conventions

In the following text *SAPL* refers to SHARP APL, including *SAM* which is SAPL under MVS and *SAX* which is SAPL under Unix. SAX is the one also running under Linux and compared to APL2 here.

Variables and workspace names are enclosed in ‘quotes’. Functions and files are enclosed in &lt;angle brackets>.

*Italicized* words usually precede their definition.

The term *atom* refers to a numeric or character scalar only. The term *item* refers to any scalar.

Whenever possible I will show examples in SAX and APL2 side by side.

**To better understand this article you should first read the preceding article titled “SHARP APL vs. APL2 coding style”** (see Vector 16:2 p.87).

Because I will use many examples involving monadic and dyadic functions I will use &lt;m<b>x</b>> to denote a function used in a monadic way (where ‘x’ may be a number as in &lt;m1>, &lt;m2>, etc.) and &lt;d<b>x</b>> to denote a dyadic function as in **m3 x *d2* y.**

I use the terms ‘list’ and ‘vector’ in the same way. Same with the terms ‘table’ and ‘matrix’.

## A Bit of History

All of the composition operators described here were introduced in the late 70s. Unfortunately they were never used to their full potential because of implementation restrictions in SAM. With the arrival of SAX those barriers were removed.

An important distinction between SAPL and APL2 is that is it not possible to create your own operators in SAPL.

## Types of Operators

There are several kinds of operators. They all use a mixture of data and functions as arguments.

The main category discussed here covers the composition operators using functions only as arguments. Before I discuss these, I will introduce another operator which I will use in examples.

### *With* with data arguments

There is a way to *glue* constants to a dyadic function to change it into a monadic function. For example, consider the function “catenate 1 2 3 before each item”:

**SAX                                    APL2**

```
1 2 3¨,¨>x            1 2 3∘,¨x
```

Here `¨` is first used with a numeric (data=1 2 3) left argument to create such a function. Same with `∘` for *Dyalog*. An equivalent statement would have been

```
(<1 2 3),¨>x          (⊂1 2 3),¨x
```

It is also possible to use the data to the right, as in “divide by 3 and 5”:

```
÷¨3 5¨>x              ÷∘3 5¨x
```

for which an equivalent statement could be

```
(<3 5)÷⊂¨>x           (⊂3 5)÷⍨¨x
```

or, without *swap* (the SAX `⊂` operator),

```
x÷¨><3 5              x÷¨⊂3 5
```

When a data argument is used with `¨` (or `∘`) the entire data list is used. Notice what happens next:

```
4 = ⍴÷¨3 6¨>⍳4        4=⍴÷∘3 6¨⍳4
```

SAX applied the function “divide by the vector 3 6” and returned 4 results (of 2 elements each). *Dyalog* applied the same function to *each* one of `⍳`4 yielding 4 enclosures of 2 results each also. If we remove the `¨>` in SAX the 4 results are reassembled into a 4 by 2 table (matrix):

```
4 2 ≡ ⍴ ÷¨3 6 ⍳4
```

Notice how the ‘3 6’ is part of the `÷¨` and the interpreter does not attempt to do 3 6`⍳`4 instead. SAX also “knows” that *divide* is done on scalars whereas in *Dyalog* the `¨` has to be specified in order for the function to be applied on *each* item.

### *Swap*, (`⊂`) monadic use

*Swap*, is used to swap arguments to a function. Its monadic use implies the argument is the same to the left. For example, if `b⊥b` returns the number of contiguous 1s to the right of boolean ‘`b`’ then so does `⊥⊂b`. To remove trailing spaces in a series of enclosed words we can try

```
(-⊥⊂⍤>' '=¨>w)↓¨>w
```

### Composition operators

There are 3 composition operators in SAPL: *with[<sup>1</sup>](#fn1)* (`¨`), *on* (`⍤`) and *upon* (`⍥`). Their syntax is always x **f`¨`g y** and they are ambivalent. As described in the preceding article, *with* differs from *on* only in applying the **inverse of g** after **f** and **g** and requires knowing the inverse of  **g**. Ex:

```
⍴¨>x                   ⍴¨x
```

Here, for each cell, we first *disclose* the cell, apply *shape* then apply the inverse of *disclose*, namely *enclose,* to the result. In APL2 the *disclose/enclose* is implicit and never specified. Nor can any other function be used with the `¨` operator.

To get the shapes of all elements together in a simple array in the preceding example one must either *disclose* the final result:

```
>⍴¨>x          ⊃⍴¨x
```

or, in SAPL, use *on* instead:

```
⍴⍤>x
```

Those 2 operators (*with* and *on*) assume **g** is monadic and **f** is ambivalent. Like in APL2 where **f** is ambivalent.

The individual results are assembled the same way, whether they are disclosed together using ><b>f</b>`¨`> or as a result of doing <b>f</b>`⍤`>.

The ability to specify a function different from *disclose* in SAPL leads to interesting cases. For ex[<sup>2</sup>](#fn2):

```
⍪¨⍉x           (¯2↑1 1,⍴x)⍴x
```

Another is

```
∨\¨⌽b          ⌽∨\⌽b
```

to scan backwards.

### Upon (`⍥`)

This operator differs from *on* in the sense that this time **g** is ambivalent and **f** is monadic. On monadic calls, it doesn’t matter whether you use *on* or *upon*. That is `⍴⍥>y` and `⍴⍤>y` are the same.

What really is interesting is when you have a sequence of them. For example, assume we want to apply three functions in sequence to each cell of an array.

We can do

<table width="80%"><tr>
<td><tt>m1¨&gt;m2¨&gt;m3¨&gt;y</tt></td>
<td align="right"><tt>m1¨m2¨m3¨y</tt></td></tr><tr>
<td>or we can do</td>
<td align="right">(<i>Dyalog</i> only down here)</td></tr><tr>
<td><tt>m1⍥m2⍥m3¨&gt;y</tt></td>
<td align="right"><tt>m1∘m2∘m3¨y</tt></td></tr>
</table>

The end result will likely be different. In the first case &lt;`m3`> is called `⍴y` times, then &lt;`m2`> is called `⍴y` times then &lt;`m1`> is called `⍴y` times. In the second case the *sequence* &lt;`m3,m2,m1`> is called `⍴y` times.

In *Dyalog,* `∘` is used to *glue* functions together, having the same effect in this **monadic** case only. I will often refer to *Dyalog* for examples in the following text as IBM’s APL and APL+Win do not have this feature.

### Chaining functions

Chaining functions using the operators seen so far is not that obvious at first. We have to remember the meaning of `¨`, `⍤` and `⍥` and keep in mind they have, like any other operator, full left scope unless modified by pairs of parentheses.

The monadic case is the same for both SAX and *Dyalog*. Each function is called in sequence.

In the dyadic case chaining functions is interpreted quite differently.

#### *first Dyalog*

In *Dyalog* all but the leftmost function are assumed to be monadic. The monadic functions are called sequentially until the last one which MAY be called dyadically. For example

```
x d1∘m1∘m2∘m3¨y
```

is the same as doing, for each corresponding cell

```
x[i] d1 m1 m2 m3 y[i]
```

As seen earlier, using constants with dyadic functions makes them monadic and is allowed. For example:

```
x d1∘m1∘(2 3∘d2)∘(d3∘'ds')∘m2¨y
```

#### *then SAX*

In SAX the monadic functions are applied **to both corresponding cells** until the dyadic function is reached. The use of *on* and *upon* or parentheses around the functions may achieve the same or different result. Ex:

```
x  d1⍤m1⍤m2⍤m3¨> y
x  d1⍤(m1⍥m2⍥m3)¨> y
```

is the same and equivalent to

```
<(m1 m2 m3 >x[j]) d1 (m1 m2 m3 >y[j])
```

for each corresponding cell.

*upon* (`⍥`) should be put just BEFORE the (only) dyadic function if it’s not the leftmost function, as in

```
x  m1⍥m2⍥d1⍤m3⍤m4¨>y
```

which is, for each corresponding cell,

```
<m1 m2 (m3 m4 >x[j]) d1 (m3 m4 >y[j])
```

Of course clarity may suffer a bit. By the way, this is all in accord with what J does.<sup><a href="#fn3">3</a></sup>

#### *Interpretation of this last example*

Remember operators bind “naturally” to the left. Reading from right to left we see that

- `¨` tells us to apply the entire sequence to its left to each corresponding *disclosed* element of ‘x’ and ‘y’
- the rightmost `⍤` tells us to apply &lt;m4> monadically to >x\[j\] AND >y\[j\] and pass these two results as arguments to dyadic function &lt;m1`⍥`...m3> whose
- rightmost `⍤` tells us to apply &lt;m3> monadically to each argument and pass these two results as arguments to dyadic function &lt;m1`⍥`...d1> whose
- rightmost `⍥` tells us to use &lt;d1> **dyadically** and pass its result as argument to monadic function &lt;m1`⍥`m2> whose
- `⍥` tells us to use monadic &lt;m2>[<sup>4</sup>](#fn4) and pass its result as argument to monadic function &lt;m1>
- which returns its result to `¨`
- which returns the result of the entire sequence after *enclosing* it

Personally I’d recommend breaking down the sequence *if possible.* Or write another function that does the same and call only *that* function, as in `x myfn¨>y`.

*Dyalog’s* dynamic functions can achieve the same result by doing

```
x {m1 m2 (m3 m4 ⍺) d1 m3 m4 ⍵}¨y
```

### Examples

1: to perform integer division (*floor* of *divide*) of two lists of numbers we can code

```
x ⌊⍥÷ y            ⌊x÷y
```

The difference here, is that the former is done cell by cell whereas the latter is done function by function (it cannot be done cell by cell in APL2).

2: let’s put a few features together. In this example we want to take the first “n” rows of each table in a list and then transpose them:

```
n ⍉⍥↑¨>m            ⍉¨n↑[1]¨m
```

Here we have *upon* with short *take with disclose*. ‘n’ being a series of scalars (vector of integers).

3: find the rank of each cell:

```
⍴⍥⍴¨>x              ⍴∘⍴¨x
```

This is more likely to be the kind of code you will encounter.

### Scalar functions

In APL2, the use of the *each* operator is assumed for scalar functions (like +). There is no need to specify it[<sup>5</sup>](#fn5). Ex:

```
x+¨>y               x+y
+¨>/x               +/x
+/¨>x               +/¨x
```

### Rank

Often functions apply to cells of rank 0 (scalar functions like + or -) or of rank infinite (like &lt;). I only know of one other function, *matrix divide*, which is of a different rank, namely 2.

Normally, *matrix divide* applies to matrices. If you try to use it on a 3d array APL2 complains (RANK error). In SAX it won’t. Try

```
4 3 5 ≡ ⌹4 5 3⍴i∘.+i←⍳4
```

Each 5 by 3 matrix (4 of them) was inverted to give four 3 by 5 matrices. `⌹` is said to be of rank 2. Each “cell” to which `⌹` was applied was of rank 2. All were 5 by 3 matrices. There were four of them. A 4-element vector of 5 by 3 matrices.

In SAPL this concept of “frame” (the vector in this case) and “cell” (the matrices) is of utmost importance. It breaks up data into piecemeal sections to be fed to functions.

We can view this 4 by 5 by 3 array as a scalar (the frame) containing a 4 by 5 by 3 matrix (the cell) OR as a four-element vector (the frame) of 5 by 3 matrices (the cells) OR as a 4 by 5 matrix of three-element vectors OR as a 4 by 5 by 3 matrix of scalars.

Most functions have a default rank. The function rank determines the rank of the argument cells. For monadic `⌹` it is 2, a matrix. For + is 0, a scalar. For &lt; it is infinite. Any structure will do.

For *dyadic* functions two ranks exist: the left rank and the right rank. Function `∊` has two ranks (it doesn’t have a monadic rank): a left hand rank of 0 (for each item) and a right hand rank of infinite (membership is checked against all of it).

SAX allows you to modify the rank of primitives and to specify the rank of YOUR functions *when you use them*.

### More examples

We’ve all been facing this problem: we need to add a vector of numbers, either to each line of a as in

<table width="80%"><tr>
<td><tt>m+(⍴m)⍴v</tt></td>
<td align="right">(example 4)
</td></tr></table>

or add a different *scalar* to each *line* of ‘m’ as in

<table width="80%"><tr>
<td><tt>m+⍉(⌽⍴m)⍴v</tt></td>
<td align="right">(example 5)</td>
</tr></table>

These are fine but there are sometimes situations that require more thinking than you’d like to do. If we ponder this we see that in the expression ‘m+v’ the *add* function is trying to add numbers 1 by 1 (scalar by scalar) and is running out of numbers on one side resulting in a length error of some sort.

We can change that in SAPL. We can re-specify its behaviour by saying: “break down your work into a vector (*frame* of shape 5) of vectors (*cell* of shape 3)” (assuming ‘m’ is a 5 by 3 and ‘v’ is a 3 number list).

For example 4 this becomes:

<table width="80%"><tr>
<td width="30%"><tt>m +⍤1 v</tt></td>
<td width="30%"><tt>m+[2]v</tt></td>
<td>(as in APL+Win)</td></tr>
</table>

We modified + to do 5 additions of vectors of 3 elements each. We end up with 5 results of 3 elements each, a 5 by 3 matrix result.

Here the `⍤` with a numeric right argument is called the *rank* operator and modifies its left argument (here +). The 1 to its right specified how many trailing axes (1 here, vectors) of the arguments were to be part of the cell sections. This made a left hand frame shape of (`¯1↓⍴m`) cells, or 5, and a left hand cell shape of (`¯1↑⍴m`), or 3. The same ‘`⍤`1’ also made a right hand frame shape of (`¯1↓⍴v`), or `⍳`0 (a scalar), and a right hand cell shape of (`¯1↑⍴v`) or 3. Cell sizes are equal (3) and frame shapes are acceptable (5 elements + scalar). All is well.

In example 4 we divided the 5 by 3 matrix into 5 vectors of three-element vectors adding them to \[a scalar containing\] a three-element vector resulting in 5 results of three-elements each.

In example 5 we needed to add each vector in ‘m’ to each scalar in ‘v’. We needed to modify &lt;+> to take vectors on one side and scalars on the other:

<table width="80%"><tr>
<td width="30%"><tt>m + ⍤1 0 v</tt></td>
<td width="30%"><tt>m+[1]v</tt></td>
<td>(APL+Win)</td></tr>
</table>

Here we are saying: “add each vector in ‘m’ (the 1 on the same side as ‘m’) to each scalar in ‘v’ (the 0 on its side)”

There are two numbers to the right of the rank op to specify the rank of each argument. When they are the same, or if the function is monadic, one number may be used as in the first example.

Up to three numbers may be specified to denote monadic, left and right dyadic ranks but since these cannot be stored with a user’s function definition it’s unlikely you’ll ever see code with all three.

6: ravel each plane of a 3d array

```
,⍤(2)7 3 4 ⍴ 5 6           ,[2 3]7 3 4⍴5 6
```

The parentheses are used to separated the 2 from the ‘7 3 4’ here. Or you could surround the right argument with parentheses:

```
7 12 ≡⍴,⍤2 (7 3 4 ⍴ 5 6)
```

or you could use the *pass* function (or any function that does not alter the argument):

```
,⍤2⊢7 3 4 ⍴ 5 6
```

otherwise ‘2 7 3 4’ would be the argument to `⍤` and ,`⍤`2 7 3 4 would be applied to `⍴`5 6 instead. Not the same thing.[<sup>6</sup>](#fn6)

APL2 allows *ravel* to be applied to other consecutive dimensions as well. SAPL does not.

```
⍉,⍤2⊢2 3 1⍉7 3 4⍴x          ,[1 2]7 3 4⍴x
```

or

```
,⍤2¨⍉2 1 3⍉7 3 4⍴x
```

7: enclose each line of a table

```
<⍤1 table
```

Going back to example 4, we could re-code it as

```
(<⍤1 m)+⍤><v                ⊃(⊂[2]m)+⊂v
```

and example 5 as

```
(<⍤1 m)+⍤>v                 ⊃(⊂[2]m)+v
```

If the frame size needs to be specified (as opposed to the cell size) simply use negative (complementary) rank. For example, monadic function *table* (`⍪`) is defined as

```
,⍤¯1⊢1/x
```

meaning “ravel all but the first axis of the argument”, making sure ‘x’ is at least a vector (1/x). For a 3d array all planes (rank 2) are raveled, for a matrix all vectors (rank 1) are raveled and for vectors all scalars (rank 0) are raveled.

8: create an array from the shape of another.

Here we wish to create a table of ‘abc’s with the frame of another:

```
'abc'⊣⍤1 x                  ((¯1↓⍴x),3)⍴'abc'
```

9: apply rank to your own functions.

Here we have a function, &lt;R2fn> which takes a matrix (and nothing else) as argument and returns ten numbers. To use it on higher rank arrays we tell APL to use matrices as **cells** (`x←2 3 4 5⍴⍳99`):

```
2 3 10≡⍴R2fn⍤2 x            ⊃R2fn¨⊂[¯2↑⍳⍴⍴x]x
```

### Cut (n`⍤`)

This is a different *function*. It uses `⍤` again but this time with a *left* numeric argument.

This is the same as *Dyalog’s* dyadic `⊂` with variants. APL+Win uses `⎕`penclose instead. Both of these work along the *last* axis unless brackets are used. SAX works along the *first* axis.

In *Dyalog,* b`⊂`x, with ‘b’ boolean, cuts pieces of ‘x’, using 1s in ‘b’ as delimiter to indicate the start of a partition, and encloses them.

In SAX, **b n`⍤`&lt;x** does the same with n=1. ‘b’ specifies where to cut and n=1 means the start cuts where b=1. Variants exists and n=2 means that each cut ENDS on a 1. Ex: b is 0 1 0 1 0 0 1 0, `⎕`io is 1

```
     b 1⍤<⍳8                b⊂⍳8
(2 3⊃4 5 6⊃7 8)             (←answer)
     b 2⍤<⍳8                (¯1↓(∨/b),b)⊂⍳8
(1 2⊃3 4⊃5 6 7)
```

The monadic case assumes the first cell is a delimiter. For ex, with `s←',az,bc,def'`:

```
     1⍤<s                   (s∊1↑s)⊂s
(',az'⊃',bc'⊃',def')        (←answer)
```

To exclude the delimiter use a negative left argument:

```
     ¯1⍤<s                  1↓¨(s∊1↑s)⊂s
('az'⊃'bc'⊃'def')           (←answer)
```

#### *Function &lt;vtom> revisited:*

This function has to be the function most rewritten in APL. The &lt; used so far can be changed to any monadic function. If we use *pass* we get

```
     ¯1⍤⊢s                  ⊃1↓¨(s∊1↑s)⊂s
az
bc
def
```

As usual the pieces are reassembled to fit the largest.

*Cut* also works on higher rank arrays but always on the first dimension. Ex. b`←`1 0 1 0 0 and m`←`5 3`⍴⍳`21:

```
b 1⍤(+⌿)m                   ⊃+⌿¨b⊂[⎕io]m
```

To cut on another axis use rank (here the last one):

```
b 1⍤(+/)⍤1 m                ⊃+/¨b⊂m
```

### More examples

10: suppose we are given a list of series (a vector of vectors) of resistors and wish to find the overall resistance of each series (formula `÷+/÷`) we could use

```
÷⍥(+/)⍥(÷⍤1)⍥>list          ÷∘(+/)∘÷¨list
```

We use `÷⍤`1 because `÷` is a scalar (rank 0) function and composition operators propagate the rank of the first function. Had we not done so `+/` would also apply to each scalar and we would not get the result expected.

Because the inverse propriety of `÷⍤`1 is not defined we cannot do

```
+/¨(÷⍤1)⍤>list
```

11: compare two matrices of the same width, line by line. This is the same as v1`∘`.=v2 for matrices:

```
m1∘.(≡⍤1) m2                (⊂[⎕io]m1)∘.≡⊂[⎕io]m2
```

12: this is the same problem as 2 except that ‘m’ is of various higher ranks arrays. Were it not for the rank operator the solution would be harder to come by.

<table width="80%"><tr>
<td><tt>n ⍉⍥(↑⍤2)¨&gt;m</tt></td>
<td align="right">(solution too complex in APL2)</td></tr>
</table>

### Dot (.)

This operator behaves as in other APLs except that it can also be used monadically. Its definition is fairly complex for the general case but it should be enough to know that `-.×` returns the determinant of a matrix. Ex:

```
100 = -.× 3 3 ⍴ 3 4 7 6
```

### Select (`}`)

This operator takes a selection function, like 1 0/ or 2 3`{`, applicable to the right argument and merges the left argument within it. In APL2 this merging occurs within a variable. In SAX a result is returned instead:

```
x←9 (2¨{) }⍳3               x←⍳3 ⋄ (3⊃x)←9
```

Here, 2`¨{` selects the 3rd element from `⍳`3 which is replaced by 9.

We can do specific items or whole lines. Here the selector function is “`b/`”:

```
x←8 b/}2 3⍴⍳6               x←2 3⍴⍳6⋄(b/x)←8
```

## Conclusion

SAX is definitely different from APL2 when it comes to using operators. Sometimes it is easier to use, sometimes APL2 is easier. J programmers will be at (better) ease with this material as most operators are already defined this way in J.

I understand both worlds. I wish we could define our own operators though. Later maybe.

For questions and remarks I can be reached at [danb@dsuper.net](mailto:danb@dsuper.net).

Dan Baronet  
Montreal  
Canada.

***

1. <a id="fn1"></a> in SAX the term used is *under* but I will stick with *with* here.
2. <a id="fn2"></a> only valid for rank 2 or less objects.
3. <a id="fn3"></a> in J, `¨` is `&.`, `⍤` is `&` and `⍥` is for function arguments.
4. <a id="fn4"></a> it doesn’t matter which operator (on or upon) is used from then on since functions can only be called monadically.
5. <a id="fn5"></a> this is consistent with the definition that all scalar functions apply *disclose* BEFORE the operation and *enclose* AFTER, with the fact that scalars CANNOT be *enclosed*.
6. <a id="fn6"></a> actually it wouldn’t work as `⍤` would issue a length error.

***
