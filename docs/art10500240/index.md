---
vid: '10500240'
title: 'J-ottings 52: All but one'
prefix: J-ottings 52
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '24'
issue: '2'
page: '93'
description: Article submitted to vector.org.uk
abstract: A frequent requirement in applied mathematics and statistics is to evaluate sums and products omitting just one variable or dimension. The notion of ‘all but one’ can be interpreted in two ways, depending whether the ‘one’ is to be systematically omitted, or obtained by a merge with an existing dimension, that is by reduction.
source: content/printed/242/jot52.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- programming techniques
- mathematics and statistics
---

by Norman Thomson
{ .byline }

A frequent requirement in applied mathematics and statistics is to evaluate sums and products omitting just one variable or dimension. The notion of ‘all but one’ can be interpreted in two ways, depending whether the ‘one’ is to be systematically omitted, or obtained by a merge with an existing dimension, that is by reduction.
{ .abstract }

## Retaining all but one

As an example of the first case, adding or multiplying ‘all but one’ items in a list progressively can be done by using the hook `invf~f/` which means `(f/x)invf x`, where `invf` is the inverse verb of `f`, for example:

```
   i.5
0 1 2 3 4
   (-~+/)i.5          NB. sum five items omitting one at a time
10 9 8 7 6
   (%~*/)1+i.5        NB. multiply five items omitting one at a time
120 60 40 30 24
```

This extends readily to objects of higher rank:

```
    ]b=.i.3 4
0 1  2  3
4 5  6  7
8 9 10 11
   (-"1~+/)b          NB. sums of all rows but one
12 14 16 18
 8 10 12 14
 4  6  8 10
   (-"2~+/"1)b        NB. sums of all columns but one
 6  5  4  3
18 17 16 15
30 29 28 27
```

The rule is that the rank operand for the *inverse* verb should be one greater than that of the inserted verb.

## Generalising ‘retaining all but one’

Another tool which deals with ‘retaining all but one’ is the *suffix* adverb which eliminates n items from a list in all possible ways:

```
   (1+\.i.5);(2+\.i.5);(3+\.i.5)
+-------+-----+---+
|1 2 3 4|2 3 4|3 4|
|0 2 3 4|0 3 4|0 4|
|0 1 3 4|0 1 4|0 1|
|0 1 2 4|0 1 2|   |
|0 1 2 3|     |   |
+-------+-----+---+
```

In the J line above `+` is monadic, which for real numbers is a ‘do nothing’ verb; that is, the left arguments 1, 2 and 3 are not added to elements of `i.5` but are arguments of the derived verb `+\.` that indicate how many items are to be dropped, progressively working from the left. The first case with 1 as left argument is thus the ‘all but one’ case.

An application of this is the calculation of minors of a determinant. Consider the rank-2 object `z`:

```
   z
3 6 7
9 3 2
9 7 7
   (1&(+\.))"2 z      NB. select 'all but' one rows
9 3 2
9 7 7

3 6 7
9 7 7

3 6 7
9 3 2
```

Now combine the structural verb *transpose* with the adverb *suffix* to switch rows and columns for all but one row at a time:

```
   transuff=.1&(|:\.)"2
   <"2 transuff z
+---+---+---+
|9 9|3 9|3 9|
|3 7|6 7|6 3|
|2 7|7 7|7 2|
+---+---+---+
```

and the result is a list of the transposed planes of `(1&(+\.))"2 z`.

Do this twice using the *power* adverb to generate three boxes within each of the above boxes; the row/column switch is fortuitously reversed and the minors of `z` obtained:

```
   <"2 transuff^:2 z
+---+---+---+
|3 2|9 2|9 3|
|7 7|9 7|9 7|
+---+---+---+
|6 7|3 7|3 6|
|7 7|9 7|9 7|
+---+---+---+
|6 7|3 7|3 6|
|3 2|9 2|9 3|
+---+---+---+
```

Define

```
   minors=.transuff^:2  NB. minors unboxed
   det=.-/ .*           NB. determinant
```

The determinants of the minors are given by

```
   each=.&>
   ]dz=.det each <"2 minors z
 7  45  36
_7 _42 _33
_9 _57 _45
```

This is verified by using the verb `det` dyadically:

```
    (det z);dz det |:z
+-+------+
|3|3  0 0|
| |0 _3 0|
| |0  0 3|
+-+------+
```

It is often convenient to use cofactors, that is the signed determinants of minors. This requires multiplication by a matching matrix whose diagonals are alternately +1 and –1. One way of obtaining this matrix is:

```
   signs=.monad : '-_1+2*2|+/~i.#y.'
```

so that

```
   cof=.signs * det each @<"2@minors
   cof z
 7 _45  36
 7 _42  33
_9  57 _45
```

To summarise this section:

```
   transuff=.1&(|:\.)"2             NB. transpose with suffix
   minors=.transuff^:2              NB. minors unboxed
   det=.-/ .*                       NB. determinant
   each=.&>
   signs=.monad :'-_1+2*2|+/~i.#y.' NB. alternate 1,_1
   cof=.signs * det each@<"2@minors NB. cofactors
```

## Reducing all but one

Rank again is at the heart of the matter, especially as typical experimental data is structured so that dimensions correspond to variables. However, J inherently structures higher-rank data as lists, then lists of lists, lists of lists of lists and so on, which implies a nesting within dimensions which is not usually reflected in the real world variables to which the dimensions correspond. For definiteness use `q` to demonstrate:

```
   ]q=.i.2 3 4
 0  1  2  3
 4  5  6  7
 8  9 10 11

12 13 14 15
16 17 18 19
20 21 22 23
```

The standard definition of rank is:

```
   rk=.#@$   NB. rank
```

`+/` ‘inserts’ `+` between items of the list at the topmost level, thereby reducing rank by one, that is merging planes:

```
   +/q      NB. equivalent to +/"n q for n>2
12 14 16 18
20 22 24 26
28 30 32 34
```

Explicit rank conjunctions allows such reduction to take place at any level in the rank hierarchy:

```
   (+/"1 q);(+/"2 q)  NB. merge cols ; merge rows
+--------+-----------+
| 6 22 38|12 15 18 21|
|54 70 86|48 51 54 57|
+--------+-----------+
```

Progressive reduction to the level of a rank-1 object is obtained using a recursive verb:

```
   sum=.sum@(+/)` ] @.(=&1@rk)  NB. sum down to rank 1
   sum q
60 66 72 78
```

The above values are readily verifiable as the column sums of `q`. To find other such sums, say row sums, transpose the data so as to bring the corresponding dimension to the top level. This suggests a general verb which takes the list `i.n` and performs the a set of `n` possible shifts necessary to bring each item in turn into the leading position:

```
   EACH=.&.>
   shifts=.|.EACH< NB. all distinct shifts of a list
   shifts i.rk q
+-----+-----+-----+
|0 1 2|1 2 0|2 0 1|
+-----+-----+-----+
```

If the argument to `shifts` is a list generated by `i.`, the result is left arguments to *transpose* which provide all the restructured forms of `q` to which `sum` can be applied. This in turn is determined by the rank of the data matrix, so define

```
   targs=.shifts@(i.@rk)  NB. arguments for |:
   targs q
+-----+-----+-----+
|0 1 2|1 2 0|2 0 1|
+-----+-----+-----+
```

The full set of transpositions to supply all possible sums by dimension is then

```
   transposes=.targs |:EACH <     NB. reqd. transposes
   transposes q
+-----------+-----+--------+
| 0  1  2  3| 0 12| 0  4  8|
| 4  5  6  7| 1 13|12 16 20|
| 8  9 10 11| 2 14|        |
|           | 3 15| 1  5  9|
|12 13 14 15|     |13 17 21|
|16 17 18 19| 4 16|        |
|20 21 22 23| 5 17| 2  6 10|
|           | 6 18|14 18 22|
|           | 7 19|        |
|           |     | 3  7 11|
|           | 8 20|15 19 23|
|           | 9 21|        |
|           |10 22|        |
|           |11 23|        |
+-----------+-----+--------+
```

These values are readily verifiable by summing the columns in the boxes above:

```
   sum EACH@transposes q
+-----------+------+---------+
|60 66 72 78|66 210|60 92 124|
+-----------+------+---------+
```

To give these sums in dimension order, that is so that the `$EACH` of the result matches the shape of `q`, write

```
   allsums=.1&|.@(sum EACH@transposes)
   allsums q
+------+---------+-----------+
|66 210|60 92 124|60 66 72 78|
+------+---------+-----------+
```

To summarise this section:

```
   rk=.#@$                             NB. rank
   sum=.sum@(+/)`] @.(=&1@rk)          NB. sum down to rank 1
   shifts=.|.EACH <                    NB. all shifts of i.n
   targs=.shifts@(i.@rk)               NB. arguments for |:
   transposes=.targs |:EACH <          NB. reqd. transpositions
   allsums=.1&|.@(sum EACH@transposes)
```

For comparison, here is a one-liner picked from the J Software forum some years ago which performs the same function by using the *power* adverb `^:` to apply `+/` the requisite number of times with transpositions required between steps as in the version above:

```
   mt=.(<@(+/^:(<:@rk)@:|:)"0 _~i.@rk)
   mt q
+------+---------+-----------+
|66 210|60 92 124|60 66 72 78|
+------+---------+-----------+
```

## Subtotaling

It can be useful to be able to append such reductions to the original data as in:

```
  total=.,+/ NB. append totals for leading dimension
  sub=.3 :0
i=.0 [ r=.total y.
while.(i<<:rk y.)do.r=.total"i r [ i=.i+1 end.
)
   sub q
 0  1  2  3   6
 4  5  6  7  22
 8  9 10 11  38
12 15 18 21  66

12 13 14 15  54
16 17 18 19  70
20 21 22 23  86
48 51 54 57 210

12 14 16 18  60
20 22 24 26  92
28 30 32 34 124
60 66 72 78 276
```

Multi-statement lines using `[` as a separator as in `sub` allow something approaching the succinctness of tacit definition. However the individual statements are executed from the right since `[` itself is just another verb. It is easy to remember that it is `[` rather than `]` which is the separator, since, for example, it is 0 to the left which is assigned to `i` in the first line of `sub`. As far as the J interpreter is concerned it is really only one line which is executed; multiple lines are essentially just an orthographic device.
