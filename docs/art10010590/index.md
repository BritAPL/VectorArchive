---
vid: '10010590'
title: 'J-ottings 45: A bit of OUTReach (or do I mean outrEACH?)'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '22'
issue: '1'
page: '102'
source: trad/v221/jot45.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- language design
- programming techniques
---

by Norman Thomson
{ .byline }

In APL2 there was just one primitive symbol (double dot) for “each” – the “pepper” effect is well imprinted on some of us with long memories, although, as Stephen Taylor hinted in his last editorial, APL just doesn’t look like that any more! I find it interesting that in spite of J being considerably richer in primitives than APL, it does not include “each” as a primitive. Nevertheless it is customary for many users to define an adverb “each”, even although it requires more characters to type than the J which defines it! Unfortunately, depending on the writer, the definition is sometimes <span class="APLU">&amp;&gt;</span> , sometimes <span class="APLU">&amp;.&gt;</span>, and occasionally <span class="APLU">"0</span> . I do not recommend this last practice, since a verb-noun combination is effectively one major case of a primitive operation, and in my view is best recognised as such. I therefore confine discussion of adverbial “each” to the other two possibilities, defined as

```
   each=.&>         NB. uses bond
   EACH=.&.>        NB. uses compose
```

The definitions of the conjunctions <span class="APLU">u&amp;v</span> and <span class="APLU">u&amp;.v</span> in mathematical terms as (uv) and (v<sup>-1</sup>uv) show that the rule

```
   (f each) -: (>f EACH)
```

is completely general, and so only one of <span class="APLU">each</span> and <span class="APLU">EACH</span> is logically necessary. <span class="APLU">EACH</span> has the general merit of greater compactness through generating what APL2 users would recognise as ragged arrays. On the other hand <span class="APLU">each</span> has the merit of avoiding, or at least reducing, boxing on display, but often at the expense of requiring greater numbers of fill characters to achieve global homogeneity. In writing J it can be very desirable to be have a ‘rule of thumb’ appreciation of how <span class="APLU">each</span> and <span class="APLU">EACH</span> work without having to consider the semantic details of “box” and “open” at every point of use.

Thinking in terms of lists is helpful in using <span class="APLU">each</span> and <span class="APLU">EACH</span> effectively, as is sensitivity to the ways in which argument rank works. For example, given

```
   h=.i.2 3        NB. two 3-lists, rank = 2
```

compare the following

```
   h,each 6 7      NB. result=one 2-list, items=three 2-lists, rank=3
0 6
1 6
2 6

3 7
4 7
5 7

   h,EACH 6 7      NB. result=two 3-lists, items=2-lists, rank=2
+---+---+---+
|0 6|1 6|2 6|
+---+---+---+
|3 7|4 7|5 7|
+---+---+---+
```

Applying the general rule above (<span class="APLU">h,each 6 7</span>) is identical to <span class="APLU">&gt;h,EACH 6 7</span>, or to put it in another way, the two displays above show the same six 2-lists, in one case individually boxed, in the other integrated into a structure of higher rank.

Using either <span class="APLU">EACH</span> or <span class="APLU">each</span> with a dyadic verb often requires the user to box one of the arguments to achieve a desired result through bringing about correct argument matching. Often this consists of a simple boxing of one of the arguments as in

```
   x=.6 7;8;9 10 11 12
   (<h),EACH x     NB. left rank 0, right rank 1, result rank 1
+-----+-----+----------+
|0 1 2|0 1 2|0  1  2  0|
|3 4 5|3 4 5|3  4  5  0|
|6 7 0|8 8 8|9 10 11 12|
+-----+-----+----------+
```

The result is a 3-list of scalars (and so of rank 1) each made from the same scalar (<span class="APLU">&lt;h</span>) joined to each item of a 3-list, with everything opened and boxed following the (v<sup>-1</sup>uv) rule which defines <span class="APLU">&amp;. </span>(under). There is a local fill character in the first box, a local scalar expansion in the second, and the third containing two fill characters in the final column has a different shape (<span class="APLU">$</span>) from the other two. Compare this with

```
   (<h),each x    NB. left rank 0, right rank 1, result rank 3
0   1   2   0
3   4   5   0
6   7   0   0

0   1   2   0
3   4   5   0
8   8   8   0

0   1   2   0
3   4   5   0
9  10  11  12
```

where there is no v<sup>-1</sup> boxing. Again the result is a 3-list, but now not of scalars but of three filled lists, each of which is a 4-list of scalars after filling. APL2 talked about “ragged” and “heterogeneous” arrays – a more appropriate differentiation in J is between locally- and globally-filled lists, corresponding to <span class="APLU">EACH</span> and <span class="APLU">each</span> and respectively. Using <span class="APLU">"0</span> as an alternative gives a result rank between those of <span class="APLU">EACH</span> and <span class="APLU">each</span> :

```
  (<h),"0 x      NB. left rank 0, right rank 1, result rank 2
+-----+----------+
|0 1 2|6 7       |
|3 4 5|          |
+-----+----------+
|0 1 2|8         |
|3 4 5|          |
+-----+----------+
|0 1 2|9 10 11 12|
|3 4 5|          |
+-----+----------+
```

Here the scalar left argument gives rise to scalar replication, and the result is a 3-list of non-homogeneous 2-lists without fill characters, that is, it is only the *display* which is rectangular not the result object itself, as is made explicit by

```
   $EACH (<h),"0 x
+---+-+
|2 3|2|
+---+-+
|2 3| |
+---+-+
|2 3|4|
+---+-+
```

In order to avoid monotonous repetition, subsequent examples will use just one of <span class="APLU">EACH</span> or <span class="APLU">each</span>, the other case being covered by the general rule <span class="APLU">(f each) -: (&gt;f EACH) </span>.Consideration of rank is almost always a preliminary to well-considered usage of <span class="APLU">EACH</span>. There are no overall rank rules because these depend on the semantics of each individual <span class="APLU">f</span> . In the next set of examples three figures in square brackets in a comment give the left, right and result ranks of the expression being commented. Consider first the basic case of the verb “shift” with a rank 1 argument on the left and a rank 2 argument on the right :

```
   0 1|.h           NB. 0-shift on rows ,: 1-shift on cols     [1 2 2]
1 2 0
4 5 3
```

Applying <span class="APLU">EACH</span> with “box” on either right or left changes the ranks :

```
   0 1 |.EACH<h     NB. 0-shift on h ; 1-shift on h            [1 0 1]
+-----+-----+
|0 1 2|3 4 5|
|3 4 5|0 1 2|
+-----+-----+

   (<0 1)|.EACH h   NB. (<0 1)-shift applied to items of h     [0 2 2]
+-+-+-+
|0|1|2|
+-+-+-+
|3|4|5|
+-+-+-+
```

Between these extremes is the possibly more useful case of “left rank 1, right rank 1”:

<pre class="APLU">   0 1|.EACH&lt;"1 h   NB. 0-shift to 1st row ; 1-shift to 2<sup>nd</sup>     [1 1 1] 
+-----+-----+
|0 1 2|4 5 3|
+-----+-----+</pre>

Next; given the laminated rank 3 object :

```
   ]hlam=.h,:h+10   NB. hlam is h laminated with h+10
 0  1  2
 3  4  5
10 11 12
13 14 15
```

here are the effects of four rather similar expressions, along with annotated descriptions which attempt to explain the differences between them. What is the best medium for such descriptions? Why J, of course, hence the comments which follow each of the four executable lines supply matching equivalent statements

```
   (<0 1)|.EACH <"2 hlam     NB. (0 1|.h);(0 1|.h+10)          [0 1 1]
+-----+--------+
|1 2 0|11 12 10|
|4 5 3|14 15 13|
+-----+--------+

   0 1|.EACH <"2 hlam        NB. (0|.h);(1|.h+10)              [1 1 1]
+-----+--------+
|0 1 2|13 14 15|
|3 4 5|10 11 12|
+-----+--------+

   0 1|.EACH <"1 hlam        NB. (0|.EACH<"1 h),:(1|.EACH<"1 h+10)
+--------+--------+
|0 1 2   |3 4 5   |          NB.                               [1 2 2]
+--------+--------+
|11 12 10|14 15 13|
+--------+--------+
```

This example is also equivalent to <span class="APLU">2 2$0 0 1 1|.EACH ,&lt;"1 hlam</span> , since <span class="APLU">,&lt;"1
hlam</span> is a 4-list whose items are 3-lists such as <span class="APLU">0 1 2</span>. The next example illustrates how <span class="APLU">"0</span> means apply a shift *at the scalar level* which is necessarily a “do nothing” operation.

```
   (0 1)|."0<"2 hlam         NB. equivalent to <"2 hlam        [1 1 1]
+-----+--------+
|0 1 2|10 11 12|
|3 4 5|13 14 15|
+-----+--------+
```

A further set of examples illustrates what happens with characters. <span class="APLU">g</span> is a pair of 2-lists and thus has the same shape as <span class="APLU">h</span>, only its items are characters lists rather numbers.

```
   ]g=.2 3$'ant';'bee';'cat';'dog';'elk';'frog'
+---+---+----+
|ant|bee|cat |
+---+---+----+
|dog|elk|frog|
+---+---+----+
```

Where items are characters lists, there are necessarily possible ambiguities between genuine space characters and space fill characters, something which is clarified in

```
   1 |.EACH g                NB. 1-shift at item level         [0 2 2]
 +---+---+----+
 |nta|eeb|atc |
 +---+---+----+
 |ogd|lke|rogf|
 +---+---+----+
```

The next example shows simultaneous opening of both arguments followed by v<sup>-1</sup> boxing :

```
   (<0 1)|.EACH <g           NB. <0 1|.g                       [0 0 0]
+--------------+
|+---+----+---+|
||bee|cat |ant||
|+---+----+---+|
||elk|frog|dog||
|+---+----+---+|
+--------------+
```

The next example is one in which the result rank is less than an argument rank :

```
   0 1|.EACH <g              NB. (<0|.g),(<1|.g)               [1 0 0]
+--------------+--------------+
|+---+---+----+|+---+---+----+|
||ant|bee|cat |||dog|elk|frog||
|+---+---+----+|+---+---+----+|
||dog|elk|frog|||ant|bee|cat ||
|+---+---+----+|+---+---+----+|
+--------------+--------------+
```

In the final example in this set the shapes of the items within the two outer boxes are not identical, thus resolving an ambiguity between fill and genuine space characters which each would not have shown :

```
   0 1|.EACH<"1 g            NB. (<0|.0{g),(<1|.1{g)           [1 1 1]
+-------------+--------------+
|+---+---+---+|+---+----+---+|
||ant|bee|cat|||elk|frog|dog||
|+---+---+---+|+---+----+---+|
+-------------+--------------+
```

However, given that each by definition reduces one level of boxing in every case, it often gives less fussy looking results than <span class="APLU">EACH</span> where character lists are involved.

The examples so far have been chosen to illustrate the principles of <span class="APLU">each</span> and <span class="APLU">EACH</span>. An example of a practical problem involving each is that of evaluating linear expressions such as x + 2y - z over separate ranges of values of their variables. The results in this simple example are easy to check.

```
   t3=.1 2;3 4 5;6NB. x={1,2} y={3 4 5} z={6}
   {t3                      NB. catalog generates all combinations
+-----+-----+-----+
|1 3 6|1 4 6|1 5 6|
+-----+-----+-----+
|2 3 6|2 4 6|2 5 6|
+-----+-----+-----+

   ip=.+/ .* each           NB. inner product each
   ({t3)ip <1 2 _1          NB. 1 2 _1 are a set of linear coefficients
1 3 5
2 4 6
```

The advent of “each” and ragged arrays in APL2 breathed new life into the language. J deals with such matters more subtly by forcing choices between two possibilities! Broadly speaking <span class="APLU">EACH</span> makes objects more compact by allowing raggedness and reducing homogeneity to a local level, <span class="APLU">each</span> reduces boxing at the cost of imposing greater homogeneity through globally applied fill characters.
