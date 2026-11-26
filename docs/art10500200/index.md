---
vid: '10500200'
title: Inner product – an old/new problem
authors:
- Roger Hui
byline: by Roger Hui (rhui000@shaw.ca)
volume: '24'
issue: '2'
page: '58'
description: Article submitted to vector.org.uk
source: content/printed/242/hui.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- performance
- implementations
- programming techniques
---

by Roger Hui (rhui000@shaw.ca)
{ .byline }

## <a id="abs"></a>Abstract

Exploitation of an old algorithm for inner product leads to a greater than 10-fold improvement in speed to `+.×` in Dyalog v12.1. In the process dfns and dops were found to be excellent tools of thought for thinking about the problem. We also demonstrate the product of some large sparse matrices in J.

## <a id="1"></a>1. Row-by-column

The conventional method for inner product produces its result one element at a time, operating on one row of the left argument against one column of the right argument. That is, if `z ← x f.g y`, then

```
   z[i;j] = f/ x[i;] g y[;j]
```

Question: why is inner product defined in this computationally inconvenient way?

`A` is a linear transformation from `n` (dimensional) space to `m` space, where `m n←⍴A`, and `B` is a linear transformation from `p` space to `n` space (`n p←⍴B`). The transformations are effected by `A+.×y` and `B+.×y1`. For example:

```
   A←¯3+?3 3⍴10
   B←¯3+?3 3⍴10
   y←¯3+?3⍴10

   B+.×y
17 12 ¯12
   A+.×(B+.×y)
2 58 15
```

The last transformation is the result of transforming `y` first by `B` and then by `A`. The inner product `A+.×B` computes the matrix for this transformation, `A` composed with `B` .

```
   (A+.×B) +.× y
2 58 15

   A+.×B
¯9 ¯19 ¯2
¯6  16  8
 9 ¯21 21
```

So the answer to the question is, inner product is defined this way to effect the composition of two linear transformations.

## <a id="2"></a>2. Row-at-a-time

It turns out that there is an algorithm more efficient than ‘row-by-column’, namely ‘row-at-a-time’. For `z←x f.g y`:

```
   z[i;] = f⌿ x[i;] g[0] y
```

The phrase `x[i;] g[0] y` applies `g` to an element of `x[i;]` against the corresponding row of `y`. For example:

```
   10 100 1000 ×[0] 3 4⍴⍳12
   0   10    20    30
 400  500   600   700
8000 9000 10000 11000
```

In practice, the cumulation `f⌿` is composed and interleaved with the dyadic function `g[0]` so that no additional temporary space is needed. The result for row-at-a-time is mathematically and numerically identical to that for the conventional row-by-column method.

The above description is stated in terms of matrices, but the arguments are applicable to arguments of any rank.

## <a id="3"></a>3. Advantages

Row-at-a-time has some efficiency advantages over row-by-column:

- APL arrays are stored in row major order, so that in a column `y[;j]` successive elements are separated from each other and `y[0;j]` can be quite far from `y[n-1;j]`. On modern CPUs with on-chip caches, on large matrices, it is possible for row-by-column to miss the cache on every column, that is, on every element of the inner product result. In contrast, row-at-a-time operates on adjacent memory locations and is ‘cache friendly’.

    On 600-by-600 matrices, row-at-a-time is faster than row-by-column by a factor of 10.

    I believe that there is an additional advantage if the right argument is boolean (bits), as indexing a column `y[;j]` of boolean `y` is not trivial.

- Row-at-time makes it practical to exploit zeros in the left argument. (‘Zero’ in the general sense of the zero in a field with operations `f` and `g` .) In row-at-a-time, in `x[i;] g[0] y` , if `x[i;j]` is zero, there is no need to do `x[i;j] g y[j;]` . In row-by-column, in `x[i;] g y[;j]` , if `x[i;k]` is zero, only `x[i;k] g y[k;j]` can be elided.

    What it means is that you can not afford to spend the time to check `x[i;k]` for zero, because it would slow down the general case. Checking `x[i;j]` for zero in row-at-a-time also slows down the general case, but the cost is amortized over the entire row `y[j;]` .

    On 600-by-600 matrices with a density of 0.5 (i.e. half zeros), row-at-a-time with zero exploitation is faster than row-by-column by a factor of 24.

- Item `j` of the phrase `x[i;] g[0] y` is `x[i;j] g y[j;]`. If the left argument is boolean, this can have only two possible results: `0 g y[j;]` or `1 g y[j;]`. Implementation of this idea in J provides a factor of 5 improvement for boolean inner products.

## <a id="4"></a>4. Models

The current row-by-column implementation in Dyalog v12.0 starts by transposing the right argument to move the leading axis to the back, followed by an outer product of the rows. The process can be modelled as follows:

```
   dotc ←{↑ (↓⍺) ∘.(⍺⍺{⍺⍺/⍺ ⍵⍵ ⍵}⍵⍵) ↓(¯1⌽⍳⍴⍴⍵)⍉⍵}

   x←¯500+?13 19 11⍴1000
   y←¯500+?11 23⍴1000

   (x+.×y) ≡ x +dotc× y
1
   (x-.≥y) ≡ x -dotc≥ y
1
```

The expression can be simplified and shortened with judicious use of components:

```
   dotc   ← {↑ (↓⍺) ∘.(⍺⍺{⍺⍺/⍺ ⍵⍵ ⍵}⍵⍵) ↓(¯1⌽⍳⍴⍴⍵)⍉⍵}
   dotc1  ← {↑ (↓⍺) ∘.(⍺⍺{⍺⍺/⍺ ⍵⍵ ⍵}⍵⍵) ↓flip ⍵}
   dotc2  ← {↑ (↓⍺) ∘.(⍺⍺/at ⍵⍵) ↓flip ⍵}
   dotc3  ← {↑ ⍺ ∘.(⍺⍺/at ⍵⍵)compose↓ flip ⍵}
   dotc4  ← {⍺ ∘.(⍺⍺/at ⍵⍵)under↓ flip ⍵}   ⍝ nonce error
   dotc5  ← {⍺ ∘.(⍺⍺/at ⍵⍵)under↓∘flip ⍵}
   dotc6  ← {∘.(⍺⍺/at ⍵⍵)under↓∘flip}       ⍝ nonce error

   flip   ← {(¯1⌽⍳⍴⍴⍵)⍉⍵}
   at     ← {⍺⍺(⍺ ⍵⍵ ⍵)}
   compose← {(⍵⍵ ⍺)⍺⍺(⍵⍵ ⍵)}
   under  ← {(⍵⍵⍣¯1)(⍵⍵ ⍺)⍺⍺(⍵⍵ ⍵)}
```

The transformation from `dotc3` to `dotc4` and from `dotc5` to `dotc6` do not work, but should. The later definitions are shorter than the earlier ones if the metric is word count; alternatively, they are shorter if the compositions were denoted by symbols such as `⍥` and `⍤` instead of `at` and `compose`.

Row-at-a-time can be modelled as follows:

```
   dotr  ← {↑ (↓⍺) ⍺⍺{⍺⍺⌿⍺ ⍵⍵[0] ⍵}⍵⍵¨ ⊂⍵}
   dotr1 ← {↑ (↓⍺) ⍺⍺⌿at(⍵⍵[0])¨ ⊂⍵}
```

In a C implementation of the algorithm, the functions `↑ ↓ ⊂` and the operator `¨` would not performed explicitly. Rather, they would be embodied as `for` loops and other control structures to mediate access to the arguments `x` and `y`.

In J, the `dotc` (row-by-column) and `dotr` (row-at-a-time) operators can be modelled as follows:

```
   flip  =: 0&|:
   dotcj =: 2 : 'u/@:v"1/ flip'

   lr    =: 1 : '1{u b. 0'  NB. left rank
   dotrj =: 2 : 'u/@(v"(1+(v lr),_))'
```

In the important special case of rank-0 (scalar) functions, where the left rank is 0, `dotrj` simplifies to:

```
   dotrj0 =: 2 : 'u/@(v"1 _)'
   dotr1  ← {↑ (↓⍺) ⍺⍺⌿at(⍵⍵[0])¨ ⊂⍵}
```

`dotr1` is repeated from above. The expression `(↓⍺) f¨ ⊂⍵` means that rows of `⍺` are applied against `⍵` *in toto*, and that is what rank `1 _` does in J. The leading mix `↑` is not needed in J because, not having done the explicit split `↓` , there is no need to undo it.

## <a id="5"></a>5. Inner products on sparse arrays

The sparse datatypes in J represent an array by the indices and values of non-‘zero’ entries. For example:

```
   ] x=: (?.3 4$10) * ?.3 4$2
0 5 9 0
0 9 0 7
0 0 0 0
   ] y=: (?.4 5$10) * ?.4 5$2
0 5 9 0 0
9 0 7 0 0
0 0 3 0 1
0 0 0 0 0

   ] xs=: $. x  NB. convert to sparse
0 1 | 5
0 2 | 9
1 1 | 9
1 3 | 7
   ] ys=: $. y
0 1 | 5
0 2 | 9
1 0 | 9
1 2 | 7
2 2 | 3
2 4 | 1

   x -: xs      NB. match
1
   y -: ys
1
```

Functions apply to sparse arrays, including inner product.

```
   x +/ .* y
45 0 62 0 9
81 0 63 0 0
 0 0  0 0 0

   xs +/ .* ys
0 0 | 45
0 2 | 62
0 4 |  9
1 0 | 81
1 2 | 63

   (x +/ .* y) -: xs +/ .* ys
1
```

Sparse arrays make possible large inner products which are impractical in the dense domain. In J7.01 beta:

```
   load '\ijs\sparsemm.ijs'

   NB. d sa s - random sparse array with density d and shape s

   x=: 0.0001 sa 2$1e5
   y=: 0.0001 sa 2$1e5

   $ x             NB. shape of x
100000 100000
   */ $ x          NB. # of elements in x
1e10

   z=: x +/ .*y    NB. inner product of x and y
   $ z             NB. shape of z
100000 100000

   +/ +./"1 x~:0   NB. # of nonzero rows    in x
99995
   +/ +./   y~:0   NB. # of nonzero columns in y
99995
   +/@, z~:0       NB. # of nonzero entries in z
9989822
```

The preceding expressions show that the conventional row-by-column approach would be problematic. There are 99995 non-zero rows in `x` and 99995 non-zero columns in `y`. Consequently, there are roughly 1e10 row-by-column combinations (of which only 1e7 end up being non-zero). No matter how fast each combination is, there are still a lot of them.

Sparse inner product in J uses a row-at-a-time algorithm. The 1e5 by 1e5 inner product above took 3.6 seconds.
