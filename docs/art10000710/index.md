---
vid: '10000710'
title: Inner Products for Range Tests
authors:
- Jonathan Barman
byline: by Jonathan Barman
volume: '10'
issue: '4'
page: '133'
source: trad/v104/barman104_133.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- programming techniques
---

by Jonathan Barman <span class="affil"></span> <span class="email"></span>
{ .byline }

At our working group meeting the other day the topic came up of the rather delightful inner products that one can use to check ranges. It seemed a good idea to remind everyone what they are and how they may be derived.

The basic range test is to check if a scalar `a` lies within the range given by a two element vector `b` where `b[1]` is less than `b[2]`. Spelling this out in full gives `(a≥b[1])^(a≤b[2])`The steps needed to turn this into an inner product are as follows:

```
(a≥b[1])^(a≤b[2])
(~a<b[1])^(a≤b[2])         ⍝ ⍺≥? ←→ ~⍺<?
(a<b[1])<(a≤b[2])          ⍝ (~⍺)^? ←→ ⍺<?
(a<b[1])<(a<b[2]+1)        ⍝ ⍺≤? ←→ ⍺<?+1 Only for integers!
</a<b+ 0 1                 ⍝ scalar extension
a<.<b+ 0 1                 ⍝ definition of inner product
```

This is the one that everyone uses, because it looks so pretty! However, there are a total of four ways in which the inner product can be derived:

```
     a<.<b+ 0 1     a<.≤b- 1 0     a>.>b- 1 0     a>.≥b+ 0 1
```

If you want to check that `a` is outside the range `b` then there are four more inner products, which are:

```
     a≤.≥b+ 0 1     a≤.>b- 1 0     a≥.≤b- 1 0     a≥.<b+ 0 1
```

It is quite unusual to see any of these. I don’t think I have ever used them, but if I did I would use the first one as it is the strict negation of `a<.<b+ 0 1`. The above inner products only work for a scalar integer against a two element range. If you have a vector of integers which you want to check against a number of ranges, then you have to use the outer product `</a∘.<b+(⍴b)⍴ 0 1`. Checking for overlapping ranges is almost exactly the same, only you have to reverse one of the ranges. Briefly going through the derivation again:

```
(a[2]≥b[1])^(a[1]≤b[2])
(~a[2]<b[1])^(a[1]<b[2]+1)
(a[2]<b[1])<(a[1]<b[2]+1)
(⌽a)<.<b+ 0 1
```

I normally switch this round and use `a>.<⌽b+ 0 1` so I don’t have to use parentheses. This form is also convenient when `a` is a two column matrix of ranges and `b` is a two element vector. As one can reverse either the left or the right argument for sets of ranges there are 8 possible inner products:

```
   a>.<⌽b+ 0 1     a>.≤⌽b- 1 0        a<.>⌽b- 1 0        a<.≥⌽b+ 0 1
(⌽a)<.< b+ 0 1  (⌽a)<.≤ b- 1 0     (⌽a)>.> b- 1 0     (⌽a)>.≥ b+ 0 1
```

The inverses, for checking that the ranges do not overlap are:

```
   a≥.≥⌽b+ 0 1     a≥.>⌽b- 1 0     a≤.≤⌽b- 1 0     a≤.<⌽b+ 0 1
(⌽a)≤.≥ b+ 0 1  (⌽a)≤.> b- 1 0  (⌽a)≥.≤ b- 1 0  (⌽a)≥.< b+ 0 1
```

I normally use `a>.<⌽b+ 0 1` and forget about the rest. It is important to remember that all the numbers must be integers for these inner products to work. If you are testing floating point numbers, then the basic tests have to be used. Also, an inner product is not necessarily faster than the expanded test. It all depends on the APL interpreter that you are using.

(webpage generated: 28 March 2006, 06:51)
