---
vid: '10500220'
title: Generating combinations in J efficiently
authors:
- R.E. Boss
byline: by R.E. Boss
volume: '24'
issue: '2'
page: '75'
description: Generating combinations in J efficiently
abstract: The x combinations of y are the unordered x-sets of i.y and their number is x!y. For years the generation of combinations in J was done most efficiently by the verb comb on the for. page of the dictionary [1]. In this article a few ways of generating combinations in J will be considered and the performances are compared. A new and more efficient algorithm is presented.
source: content/printed/242/boss.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- programming techniques
- performance
---

by R.E. Boss
{ .byline }

The x combinations of y are the unordered x-sets of `i.y` and their number is `x!y`. For years the generation of combinations in J was done most efficiently by the verb `comb` on the `for.` page of the dictionary [\[1\]](#ref1). In this article a few ways of generating combinations in J will be considered and the performances are compared. A new and more efficient algorithm is presented.
{ .abstract }

## The original, comb

See also [\[2\]](#ref2) for the definition.

```
comb=: 4 : 0
 k=. i.>:d=.y-x
 z=. (d$<i.0 0),<i.1 0
 for. i.x do. z=. k ,.&.> ,&.>/\. >:&.> z end.
 ; z
)
```

At some point in the `for.`-loop, `z` represents the combinations boxed according to the starting number, like in the next figure (where `k = 0 1 2`).

```
+---+---+---+
|0 1|1 2|2 3|
|0 2|1 3|   |
|0 3|   |   |
+---+---+---+
```

Then the expression `k ,.&.> ,&.>/\. >:&.> z` first increases all numbers by one,

```
    >:&.> z
+---+---+---+
|1 2|2 3|3 4|
|1 3|2 4|   |
|1 4|   |   |
+---+---+---+
```

then rearranges the boxes by

```
    ,&.>/\. >:&.> z
+---+---+---+
|1 2|2 3|3 4|
|1 3|2 4|   |
|1 4|3 4|   |
|2 3|   |   |
|2 4|   |   |
|3 4|   |   |
+---+---+---+
```

and finally stitches `k` to it:

```
    k ,.&.> ,&.>/\. >:&.> z
+-----+-----+-----+
|0 1 2|1 2 3|2 3 4|
|0 1 3|1 2 4|     |
|0 1 4|1 3 4|     |
|0 2 3|     |     |
|0 2 4|     |     |
|0 3 4|     |     |
+-----+-----+-----+
```

As we can see, a lot of boxing is going on. A few remarks on that. We could get rid of the `>:&.> z` which would lead to

```
    k ,.&.> ,&.>/\. z
+-----+-----+-----+
|0 0 0|1 1 1|2 2 2|
|0 0 1|1 1 2|     |
|0 0 2|1 2 2|     |
|0 1 1|     |     |
|0 1 2|     |     |
|0 2 2|     |     |
+-----+-----+-----+
```

and replace the `; z` at the end by `(i.x)+"0 ; z` However this would worsen the efficiency more than a bit.

On the other hand, one could combine the `,` and `>:` by `>:@,`

This would lead to `k ,.&.> >:@,&.>/\. z` and, although this is somewhat leaner, it is also slower.

## ComB1, the first improvement

We can do better than `comb` if we look at the numbers in the different columns. The next verb uses this. Its is a slight modification of the `comb1` in [\[3\]](#ref3).

```
comB1=: 4 : 0
 d=. x-~y
 k=. |.(>:d),\i.y
 z=. (d$<i.0 0),<i.1 0
 for_j. k  do. z=. j ,.&.> ,&.>/\. z end.
 ; z
)
```

For `'x y' =: 3 5` we get

```
   k=.|.(>:2),\i.5
2 3 4
1 2 3
0 1 2
```

and these rows are stitched subsequently to the prefixed boxes of `z` as is shown below. After the first two rows of `k` are used, we get `z` equal to

```
+---+---+---+
|1 2|2 3|3 4|
|1 3|2 4|   |
|1 4|   |   |
+---+---+---+
```

And just like the original, the next row is stitched by `j ,.&.>,&.>/\. z`

This new method at least made the boxing `>:&.> z` superfluous. So it is no surprise that it was faster and leaner by some 10% to 30%:

```
    rnk2 5&ts&>'x comb y'; 'x comB1 y'['x y'=: 8 24
1   1.07 1.13     0.172 7.915e7
0   1.00 1.00     0.161 6.998e7
```

```
    rnk2 5&ts&>'x comb y'; 'x comB1 y'['x y'=: 12 24
1   1.20 1.17     1.276 4.513e8
0   1.00 1.00     1.067 3.844e8
```

```
    rnk2 5&ts&>'x comb y'; 'x comB1 y'['x y'=: 16 24
1   1.28 1.30     0.644 2.069e8
0   1.00 1.00     0.504 1.594e8
```

The first column of the performance matrix gives the ranking, the 2nd column gives the relative execution time, the 3rd column the relative execution space and the last two columns give the real time and space [\[4\]](#ref4).

## ComB2, second improvement

In [\[5\]](#ref5) – where it was called `comb2` – a further improvement on the generation of combinations is given, now even by a complete tacit and rather elegant verb.

```
   comB2=: [:; [:(,.&.><@;\.)/ >:@-~ [\i.@]
```

This verb starts with `>:@-~[\i.@]` by which a matrix is created in which each row contains the numbers which are used in the corresponding column. The matrix

```
      3(>:@-~ [\i.@]) 5
0 1 2
1 2 3
2 3 4
```

is used for the combinations

```
   3 comB2 5
0 1 2
0 1 3
0 1 4
0 2 3
0 2 4
0 3 4
1 2 3
1 2 4
1 3 4
2 3 4
```

The first row of the first matrix gives the numbers used in the first column of the combinations matrix, and likewise for the second and third row and column. From

```
   (,.&.><@;\.)/_2{. 3(>:@-~ [\i.@]) 5
+---+---+---+
|1 2|2 3|3 4|
|1 3|2 4|   |
|1 4|   |   |
+---+---+---+
```

and

```
   (,.&.><@;\.)/ 3(>:@-~ [\i.@]) 5
+-----+-----+-----+
|0 1 2|1 2 3|2 3 4|
|0 1 3|1 2 4|     |
|0 1 4|1 3 4|     |
|0 2 3|     |     |
|0 2 4|     |     |
|0 3 4|     |     |
+-----+-----+-----+
```

it is obvious what is happening. This verb `comB2` could be regarded as finishing the first improvement, which introduced the `>:@-~ [\i.@]` phrase.

Notice also that the `,&.>/\.` from the first two verbs is replaced by the `<@;\.` phrase. Although this alternative is slower for large numbers, it is of no influence for the relatively small numbers used in combinations.

If the performances are compared, we get

```
    rnk2 5&ts&>'x comb y'; 'x comB1 y'; 'x comB2 y'['x y'=: 8 24
2   1.00 1.13     0.156 7.915e7
0   1.01 1.00     0.158 6.998e7
1   1.02 1.00     0.159 6.997e7
```

```
    rnk2 5&ts&>'x comb y'; 'x comB1 y'; 'x comB2 y'['x y'=: 12 24
2   1.19 1.17     1.277 4.513e8
0   1.00 1.00     1.071 3.844e8
1   1.00 1.00     1.073 3.844e8
```

```
    rnk2 5&ts&>'x comb y'; 'x comB1 y'; 'x comB2 y'['x y'=: 16 24
2   1.25 1.30     0.642 2.069e8
1   1.06 1.00     0.543 1.594e8
0   1.00 1.00     0.513 1.594e8
```

which makes `comB2` the overall champion, be it close to `comB1`, and almost 50% more efficient as the original.

A remark must be made that for the trivial combinations with `x e. 0 1` the answer of `comB2` is incorrect. For that the verb should be adjusted to

```
   comB2`((!,[)$ i.@])@.(0 1 e.~[)
```

which hardly influences the performance. The conclusion is that the performances of `comB1` and `comB2` are more or less equal.

## New improvement: combinations combined

What is really annoying is the fact that although `k comb n` and `(n-k) comb n` are mutually complementary, for relatively small `k` the calculation of `k comb n` is much more efficient than that of `(n-k) comb n`. See the next figures.

```
   rnk2 5&ts&>'8 comB2 24'; '16 comB2 24'
0   1.00 1.00     0.141 6.997e7
1   3.57 2.28     0.503 1.594e8
```

Also taking the complement is of no use, performance wise:

```
   rnk 5&ts&>'8 comB2 24'; '(i.24)-."1 [8 comB2 24'
0 1.00 1.00
1 5.66 1.44
```

So when writing this article an old idea came up again to reconstruct a combination from two (or more) simpler combinations.

The questions than are: which combinations can be combined and how to combine them? To answer the first question, let’s have a look at splitting a given combination.

```
   4 comb 6
0 1 2 3
0 1 2 4
0 1 2 5
0 1 3 4
0 1 3 5
0 1 4 5
0 2 3 4
0 2 3 5
0 2 4 5
0 3 4 5
1 2 3 4
1 2 3 5
1 2 4 5
1 3 4 5
2 3 4 5
```

If we subtract 2 from the last two columns we get

```
   0 0 2 2 -~"1[4 comb 6
0 1 0 1
0 1 0 2
0 1 0 3
0 1 1 2
0 1 1 3
0 1 2 3
0 2 1 2
0 2 1 3
0 2 2 3
0 3 2 3
1 2 1 2
1 2 1 3
1 2 2 3
1 3 2 3
2 3 2 3
```

Boxing according to the first two columns produces

```
   A =: (2&{."1</.])0 0 2 2 -~"1[4 comb 6
   A
+-------+-------+-------+-------+-------+-------+
|0 1 0 1|0 2 1 2|0 3 2 3|1 2 1 2|1 3 2 3|2 3 2 3|
|0 1 0 2|0 2 1 3|       |1 2 1 3|       |       |
|0 1 0 3|0 2 2 3|       |1 2 2 3|       |       |
|0 1 1 2|       |       |       |       |       |
|0 1 1 3|       |       |       |       |       |
|0 1 2 3|       |       |       |       |       |
+-------+-------+-------+-------+-------+-------+
```

As we can see `4 comb 6` appears to be built from two times `2 comb 4`. First of all, the last 2 columns of the boxes of `A` appear to be

```
   <@;\.({."1</.]) 2 comb 4
+---+---+---+
|0 1|1 2|2 3|
|0 2|1 3|   |
|0 3|2 3|   |
|1 2|   |   |
|1 3|   |   |
|2 3|   |   |
+---+---+---+
```

whereas the first columns in the boxes of `A` are the rank-1 boxes of `2 comb 4`

```
   <"1[2 comb 4
+---+---+---+---+---+---+
|0 1|0 2|0 3|1 2|1 3|2 3|
+---+---+---+---+---+---+
```

So `A` can be constructed by

```
(<"1[2 comb 4),”1&.><@;\.({."1</.]) 2 comb 4
```

Generalizing this idea we come to the verb

```
comB3=: 4 : 0
  if. x e. 0 1 do. z=.<((x!y),x)$ i.y
  else. t=. |.(<.@-:)^:(i.<. 2^.x)x
    z=.({.t) ([:(,.&.><@;\.)/ >:@-~[\i.@]) ({.t)+y-x
    for_j. 2[\t do.
z=.([ ;@:(<"1@[ (,"1 ({.j)+])&.> ])&.> <@;\.({&.><)~ (1+({.j)-~{:"1)&.>) z
      if. 2|{:j do. z=.(i.1+y-x)(,.>:)&.> <@;\.z end.
    end.
  end.
  ;z
)
```

We will illustrate the verb in calculating `5 comB3 7`. We start with a check for `x` equal to `0` or `1`. For other `x` we floor and halve (`<.@-:`) until `2` or `3` is reached, for `x = 5` this gives `t = 2 5`. Then the combinations `2 comb 4` are determined, such that they are boxed according to the first number:

```
   2 ([:(,.&.><@;\.)/ >:@-~[\i.@]) 4
+---+---+---+
|0 1|1 2|2 3|
|0 2|1 3|   |
|0 3|   |   |
+---+---+---+
```

This property will be held invariant for all `z` to be computed.

Given such a `z`, we will combine it with itself as described before, which is written in J as given in the long line of the verb.

This long line consists of three parts, the left part (`[`) being `z` itself. The right-hand side `<@;\.({&.><)~ (1+({.j)-~{:"1)&.>` is a bit complicated. Let’s split it first according to

```
   (<@;\. ,&< (1+({.j)-~{:"1)&.>) z
+-------------+-------------+
|+---+---+---+|+-----+---+-+|
||0 1|1 2|2 3|||0 1 2|1 2|2||
||0 2|1 3|   ||+-----+---+-+|
||0 3|2 3|   ||             |
||1 2|   |   ||             |
||1 3|   |   ||             |
||2 3|   |   ||             |
|+---+---+---+|             |
+-------------+-------------+
```

So we see at the left the boxed outfix of `z`, and at the right, the last columns of the boxes of `z`, diminished by `1`. The last are used to select from the first delivering

```
   B =: (<@;\. ({&.><)~ (1+({.j)-~{:"1)&.>) z
   B
+-------------+---------+-----+
|+---+---+---+|+---+---+|+---+|
||0 1|1 2|2 3|||1 2|2 3|||2 3||
||0 2|1 3|   |||1 3|   ||+---+|
||0 3|2 3|   |||2 3|   ||     |
||1 2|   |   ||+---+---+|     |
||1 3|   |   ||         |     |
||2 3|   |   ||         |     |
|+---+---+---+|         |     |
+-------------+---------+-----+
```

Now we come to the central part of the long line

```
   ;@:(<"1@[(,"1 ({.j)+])&.> ])&.>
```

Each box of `z` is combined with each box of `B`, by concatenating a row in a box left with a sub-box in a box of `B`. The last one has to be increased by `2 = ({,j)` and the end result should be ravelled. Then the new `z` is again boxed according to its first column.

```
   ([ ;@:(<"1@[ (,"1 ({.j)+])&.> ])&.> <@;\.({&.><)~ (1+({.j)-~{:"1)&.>) z
+-------+-------+-------+
|0 1 2 3|1 2 3 4|2 3 4 5|
|0 1 2 4|1 2 3 5|       |
|0 1 2 5|1 2 4 5|       |
|0 1 3 4|1 3 4 5|       |
|0 1 3 5|       |       |
|0 1 4 5|       |       |
|0 2 3 4|       |       |
|0 2 3 5|       |       |
|0 2 4 5|       |       |
|0 3 4 5|       |       |
+-------+-------+-------+
```

Since `2|{:j` because `5={:j` an extra column has to be added as in the other verbs. And this is also the final result, be it in an unravelled way.

Now, how about the performances?

```
    rnk2 5&ts&>'x comb y'; 'x comB2 y'; 'x comB3 y'['x y'=: 8 24
2   1.14 1.24     0.157 7.915e7
1   1.15 1.09     0.158 6.997e7
0   1.00 1.00     0.138 6.396e7
```

```
    rnk2 5&ts&>'x comb y'; 'x comB2 y'; 'x comB3 y'['x y'=: 12 24
2   1.76 1.42     1.277 4.513e8
1   1.48 1.21     1.074 3.844e8
0   1.00 1.00     0.726 3.182e8
```

```
    rnk2 5&ts&>'x comb y'; 'x comB2 y'; 'x comB3 y'['x y'=: 16 24
2   2.24 1.68     0.644 2.069e8
1   1.75 1.30     0.504 1.594e8
0   1.00 1.00     0.287 1.230e8
```

which is a major difference, even with the last record holder. And as it should be, the difference is bigger in the cases where `x` is close(r) to `y`.

Some other figures are:

```
   rnk 5&ts&>'x comb y'; 'x comB2 y'; 'x comB3 y'['x y'=: 20 26
2 2.88 2.64
1 2.10 1.98
0 1.00 1.00
```

```
   rnk 5&ts&>'x comb y'; 'x comB2 y'; 'x comB3 y'['x y'=: 20 28
|out of memory: comb
|   z=.k    ,.&.>,&.>/\.>:&.>z
```

```
   rnk 5&ts&> 'x comB2 y'; 'x comB3 y'['x y'=: 20 28
1 1.94 1.55
0 1.00 1.00
```

So finally a new way of generating combinations in J efficiently is found and described.

## Epilogue

One could ask the use of spending so much time speeding up a process that takes seconds or less? Even twice as fast – who cares?

There are other reasons to look for efficiency.

First of all, it is fun to do. Just as trying to break a world record, it costs you sweat, time, pain and money, and is a uniquely rewarding experience when you succeed. “Why did you climb that mountain?” a mountaineer was asked. “Because it was there.” Was the answer. So performances have to be improved, just like records.

The second and by no means less important answer is that if you design an algorithm which is faster and leaner than the existing ones, you have understood the problem and its structure deeply. And often that algorithm reveals that structure in using it. This is very rewarding, to see patterns unseen by others.

Apart from that, J is a language that makes it easy to examine most of the structures you discover. So, as long as there are nice problems for which performance can be improved, I’ll keep on doing it.

## Notes

1. <a id="ref1"></a> [www.jsoftware.com/help/dictionary/cfor.htm](http://www.jsoftware.com/help/dictionary/cfor.htm)
2. <a id="ref2"></a> [www.jsoftware.com/jwiki/Essays/Combinations](http://www.jsoftware.com/jwiki/Essays/Combinations)
3. <a id="ref3"></a> [www.jsoftware.com/pipermail/programming/2007-August/007819.html](http://www.jsoftware.com/pipermail/programming/2007-August/007819.html)
4. <a id="ref4"></a> <code>rnk2=: 1 7j2 5j2 10j3 8j\_3 ": <br>(,.~\[: (/:@/:@:(\{."1),. \}."1) (%"1 &lt;./)@:(,.~ </code>\*/"1))
5. <a id="ref5"></a> [www.jsoftware.com/pipermail/programming/2008-January/009488.html](http://www.jsoftware.com/pipermail/programming/2008-January/009488.html)
