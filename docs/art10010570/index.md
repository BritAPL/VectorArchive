---
vid: '10010570'
title: 'J-ottings 43: A Rippling Good Yarn'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '21'
issue: '3'
page: '30'
source: trad/v213/jot213.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- mathematics and statistics
- programming techniques
---

by Norman Thomson
{ .byline }

A “ripple shuffle” of a deck of cards consists of dividing it into two halves (or as nearly as possible if there is an odd number of cards) and taking one card in turn from each half. How many shuffles will it take for a deck of, say, 52 cards to return to its original state?

Suppose that the cards are numbered consecutively from zero and that the total number of cards is even. Then if all the even-numbered cards are placed together in order on top of all the odd-numbered ones, a ripple shuffle would exactly restore the original order. To put this in another way, a ripple shuffle is the inverse of “all evens first” sort. The dyadic form of the primitive verb *grade-up* is very convenient for the purpose of describing the “all evens first” sort, or equivalently the inverse ripple shuffle. To quote from the J help file, `x/:y` is defined as `(/:y){x` , that is `x` is sorted to an order specified by `y` . Thus if `y` is set up as a vector of alternating 0s and 1s the effect is to obtain all the items of one parity followed by all the items of the other. The hook `$~#` applied as `0($~#)v` is convenient for generating a vector of zeros equal in length to an arbitrary vector, and can readily be adapted to generate alternating 0s and 1s, leading to the following inverse ripple shuffle verb:

```
   irs=./: 0 1&($~)@#           NB. inverse ripple shuffle
   irs i.10
0 2 4 6 8 1 3 5 7 9
   irs 'abcdef'
acebdf
```

The power adverb makes repeated application of this process a trivial matter:

```
    irs^:(i.7)i.10                      NB. 7 inv. ripple shuffles
0 1 2 3 4 5 6 7 8 9
0 2 4 6 8 1 3 5 7 9
0 4 8 3 7 2 6 1 5 9
0 8 7 6 5 4 3 2 1 9
0 7 5 3 1 8 6 4 2 9
0 5 1 6 2 7 3 8 4 9
0 1 2 3 4 5 6 7 8 9
```

Once the original order is restored, a succession of ripple shuffles is obtained by reading the rows of the above table from bottom to top. To count the number of shuffles required to restore the original order, the direction in which the table is read is immaterial, so define a straightforward iterative counting verb:

```
countrs=.3 :0   NB. no of ripples to restore original order
r=.y. [ i=.1
while. -.y.-:irs r do. r=.irs r [ i=.i+1 end. i
)
   countrs i.10
6
```

For a deck of 52 cards the number of ripple shuffles required to restore the original order is

```
   countrs i.52
8
```

A range of even card numbers can be explored, and in some cases the results may be a little surprising at first sight:

```
   EACH=.&.>
   w,:>countrs EACH i. EACH w=.2+2*i.20
2 4 6 8 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40
1 2 4 3  6 10 12  4  8 18  6 11 20 18 28  5 10 12 36 12

   w,:>countrs EACH i. EACH w=.22+2*i.20
22 24 26 28 30 32 34 36 38 40 42 44 46 48 50 52 54 56 58 60
 6 11 20 18 28  5 10 12 36 12 20 14 12 23 21  8 52 20 18 58

   w,:>countrs EACH i. EACH w=.64+4*i.15
64 68 72 76 80 84 88 92 96 100 104 108 112 116 120
 6 66 35 20 39 82 28 12 36  30  51 106  36  44  24
```

Intuitively it might be expected that numbers which are relatively rich in factors would have relatively short run-lengths, however this is not in general the case. For example, compare 60 with 92.

A variation on the ripple shuffle is to do a final exchange of the two half-decks. This is achieved by simply changing `0 1` to `1 0` in the verb `irs` :

```
   irss=./: 1 0&($~)@#
```

After defining `countrss` by changing `irs` to `irss` in `countrs`, some corresponding results are:

```
   w,:>countrss EACH i. EACH w=.2+2*i.20
2 4 6 8 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40
2 4 3 6 10 12  4  8 18  6 11 20 18 28  5 10 12 36 12 20
```

The bottom row of this table is, not surprisingly, the same as that of the first part of the previous table, only shifted one place to the left. Thus if rs(n) is the run length for a pack of n cards without final half-deck exchange, and rss(n) is the run length with exchange, then rss(n)=rs(n+2). Thus overall run-length patterns to restore do not change as a result of the final half-deck exchange.

Apart from the powers of two for which rs(n) is simply log2n, the pattern of rs(n) is an irregular one with some abrupt changes. In general numbers with low values in the rs table have low values in the rss one. However, this is not always the case, and in particular rs(54) = rss(52) = 52, that is the effect of the seemingly innocent final switch of the two half-decks is that for a 52-card deck, 52 rather than 8 runs are needed to restore the original state.

Another way in which the problem of order-restoring shuffle run lengths can be addressed is to observe the position of, say, the number 1 in successive ripple shuffles in the table of shuffles above (remember to read the table upwards!). Its successive positions are 1, 2 4, 8, then it wraps around to position 7 which = 16 in modulo 9 arithmetic. The next position is 5 which = 32 in modulo 9 arithmetic and so on. Then look at the number originally in position 2. After one shuffle it moves to position 4, then to 8, 7 and 5, the last two being equivalent to 16 and 32 in modulo 9 arithmetic. Thus the number 2 will be restored in the same number of shuffles as it takes to restore 1, and similarly for all the other numbers. The problem of counting the number of shuffles needed to restore the original order is equivalent to that of obtaining a second 1 in the sequence `2^i.10` in modulo 9 arithmetic, thus

```
   9|2^i.13
1 2 4 8 7 5 1 2 4 8 7 5 1
```

shows the 6-cycle which has already been observed with 10 cards. Since the run-length to restore cannot exceed the number of cards the number of shuffles to restore in general comes from observing 1s in `(n-1)| 2^i.(n+1)`, leading to

```
   (&<:|2&^@&>:@i.)10
2 4 8 7 5 1 2 4 8 7
```

following which the number of shuffles for various cases is given by

```
   count=.>:@i.&1@(<:|2&^@>:@i.)
   count 10
6
   w,:>count EACH w=.12+2*i.15
12 14 16 18 20 22 24 26 28 30 32 34 36 38 40
10 12  4  8 18  6 11 20 18 28  5 10 12 36 12
   w,:>count EACH w=.44+4*i.15
44 48 52 56 60 64 68 72 76 80 84 88 92 96 100
14 23  8 20 61  6 69 35 20 39 85 28 12 36  30
```

Sharp-eyed readers may have noticed a few values such as that for 60 which are inconsistent with the previous table. This is because 2<sup>60</sup> is rather a large number, which requires extended precision for exact integer comparison. Thus in the event of discrepancy it is `countrs` which delivers the correct values.

To deal with the variation in which the two half-decks are exchanged at the end of the shuffle, simply replace the `<:` in the middle with `>:`

```
   counts=.>:@i.&1@(>:|2&^@>:@i.)
   counts 10
10
```

But what does the function rs *look* like? Here is a graph of rs(n) from n=2 to 120:

![graph](jotgraph.png){ width="500" }

As noted earlier, the powers of two are easy to deal with – they simply form the series log<sub>2</sub>n. But why do 22, 52, 74 and 92, for example have short cycles, whereas 20, 30, 50, 60 have long ones? And why are there regions of relatively short run-lengths such as

```
   w,:>countrs EACH i. EACH w=.82+2*i.10
82 84 86 88 90 92 94 96 98 100
54 82  8 28 11 12 10 36 48  30
```

I simply don’t know. Perhaps numbers have hidden personality streaks that I never realised, in which case the phrase “well, he’s a good sort” takes on a whole new meaning!
