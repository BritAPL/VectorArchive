---
vid: '10500800'
title: What is it about infinity
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '25'
issue: '3'
page: '25'
online: '2012-03-24'
editor: Kai Jaeger
description: Thomson regarding some paradoxes of infinity, helped by J notation.
abstract: Norman Thomson wrote this letter in response to Sylvia's article "What is it about infinity?" in Vector 25-1
source: content/printed/253/infinity_1.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- mathematics and statistics
---

by Norman Thomson
{ .byline }

Norman Thomson wrote this letter in response to Sylvia's article "What is it about infinity?" in Vector 25-1
{ .abstract }

Dear Sylv,

Following your article “What is it about infinity” in Vector vol. 25 No. 1, may I suggest that taking a slightly different viewpoint may help to clarify and simplify your thoughts concerning rearrangements of terms in the infinite series `ln2 = 1-1/2+1/3-1/4+?` to infinity. First the term absolute convergence means that the sum of the absolute values (i.e. moduli) of all the terms in a series converges to a finite value. The `ln2` series is not absolutely convergent since the sum of the reciprocals of the natural numbers is divergent. The issue of different sums arising from different arrangements of the terms is only a consideration for non-absolutely convergent series, in which case some of the terms must be positive and some negative. The sum for any finite number of terms will be the sum of say `p` positive terms and `n` negative terms. Rearrangement of terms within either of these groups will make no difference to the overall sum once `p` and `n` are chosen. The `ln2` series in standard ordering up to `y` terms is defined in J by

```
   ra=.(_1&^ * %@>:)@i. NB. reciprocals alternating in sign
   ra 6
1 _0.5 0.333333 _0.25 0.2 _0.166667
```

and so two typical consecutive such sums in this somewhat slowly converging series are

```
   (+/ra 1000),+/ra 1001
0.692647 0.693646
```

The positive and negative terms can be separated by

```
   odds=.#~$&1 0@#
   evens=.#~$&0 1@#
```

Any possible rearrangement of terms of the series can be accomplished by taking `p` of its positive terms and `n` of its negative terms. For the unrearranged series `p = n`:

```
   s0=.odds ar 10000            NB. positives
   s1=.evens ar 10000           NB. negatives
   (+/s0)+(+/s1)                NB. sum of first 10,000 terma
0.693097
```

If `p` and `n` are chosen differently, a different result is obtained, for example :

```
   s0=.odds ra 10000            NB. p=10000
   s1=.evens ra 5000            NB. n=5000
   (+/s0)+(+/s1)                NB. new sum
1.03962
```

Riemann proved that the sum of a non-absolutely convergent series can be made to have any arbitrary value by a suitable rearrangement of its terms; it can even be made oscillatory or divergent. Further it can be shown that in the case of the `ln2` series this sum is `½ln(4p/n)`, so that a rearrangement can always be made to achieve any given sum `S` by choosing the ratio `p/n` to be `¼exp(2S)`. For example, suppose `S` is required to be 1.5 , `¼exp(3) = 5.02`, so take about 5 times as many positive terms as negative :

```
   s0=.odds ra 5020
   s1=.evens ra 1000
   (+/s0)+(+/s1)
1.49936
```

Choosing `S=0` requires `p/n` to be `¼`:

```
   s0=.odds ra 10000
   s1=.evens ra 40000
   (+/s0,s1),(+/s0),+/s1
_1.24991e_5 5.24035 _5.24036
```

which helps in resolving the algebraic paradox :

```
      ln2  = 1-1/2+1/3-1/4+? to infinity
        = 1+1/2+1/3+1/4+?,-2(1/2+1/4+?)
              =  1+1/2+1/3+1/4+?,-(1+1/2+?)
       = X – X    where X= the sum of the reciprocals
                =  0
```

Riemann’s theorem may seem extraordinary at first sight, but should be considered in the light of the defining property of a divergent series, namely that the modulus of its sum can be made to exceed any arbitrary value simply by taking enough terms. In an informal sense the Riemann theorem describes a ‘half-way house’ property between absolutely convergence and divergence.

Yours sincerely,  
Norman Thomson
