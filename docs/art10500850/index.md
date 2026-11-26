---
vid: '10500850'
title: The “88 Hats” puzzle
authors:
- Roger Hui
byline: by Roger Hui (RogerHui.Canada@gmail.com)
volume: '25'
issue: '4'
page: '84'
received: '2011-12-29'
online: '2012-09-20'
editor: Kai Jaeger
description: Interpreter implementor Roger Hui solves a hat puzzle
keywords:
- APL
- J
- Iverson
- Scholes
- array
- arrays
- function
- index
- infinity
- string
- scalar
- string scalar
source: content/printed/254/88hats.htm
converter: vecrescue 0.1.0
issue_tab: n4-november-2012
tags:
- competitions and puzzles
- mathematics and statistics
---

by Roger Hui (RogerHui.Canada@gmail.com)
{ .byline }

The 88 Hats puzzle was posed by Andrew Nikitin[\[1\]](#ref1) to the J Forum on 5 Jun 2007. The following is a slightly modified version.

## The puzzle

88 people stand in a circle, each having a hat with a number from 0 to 87 written on it. Everyone can see the numbers on other people’s hats but can not see his own number. They simultaneously write a number on a piece of paper and give it to the judge. If at least one of them wrote a number that is on his own hat then everyone wins, otherwise everyone loses. What strategy should they use to guarantee victory?

(Numbers on the hats do not have to be all different. People can not exchange any information during the procedure but can agree on some strategy beforehand.)

The puzzle was solved by John Randall[\[2\]](#ref2) a day later.

## A winning strategy

The strategy works for any number of hats `m` . Beforehand, the people number themselves from `0` to `m-1` . Let `h` be the list of hat numbers, and `s[n]` be the sum of the numbers that person `n` sees. Then `w[n]` , the number that person `n` writes, is `m|n-s[n]` .

```
   m←13
   random←{⎕rl←7*5 ⋄ ?⍵}  ⍝ reproducible random numbers

   ⊢ h←random m⍴m
1 9 5 6 2 0 8 8 12 4 6 10 0

   ⊢ s←(∘.≠⍨⍳m)+.×h
70 62 66 65 69 71 63 63 59 67 65 61 71

   ⊢ w←m|(⍳m)-s
8 4 1 3 0 12 8 9 1 7 10 2 6

   w ,[¯0.5] h
8 4 1 3 0 12 8 9  1 7 10  2 6
1 9 5 6 2  0 8 8 12 4  6 10 0

   +/ w=h
1
```

Here’s why it works. Let `t←m|+/h` be the sum of all the hat numbers, modulo `m` . The following are equal:

```
t
m|s[t]+t-s[t]          ⍝ addition and subtraction
m|s[t]+m|t-s[t]        ⍝ modulo arithmetic
m|s[t]+w[t]            ⍝ definition of w
```

Moreover, since `s[t]` is the sum of all hats excluding `h[t]` , it must be that `t=m|s[t]+h[t]` . Therefore `m|s[t]+w[t]` and `m|s[t]+h[t]` are equal and so `w[t]` and `h[t]` are equal. That is, the written number `w[t]` for person `t` matches his hat number `h[t]` .

```
   ⊢ t←m|+/h
6
   w[6]
8
   h[6]
8
```

It is instructive to examine what the winning strategy does for 2 hats. The strategy calls for person `n` to write `m|n-s[n]` . When `m←2` , this reduces to person 0 writing the hat number of person 1 and person 1 writing the negation of the hat number of person 0. The tabulation of all possible hat numberings and corresponding writings shows that in each scenario there is a written number that equals a hat number.

```
    numbering       writing
    0 0             0 1
    0 1             1 1
    1 0             0 0
    1 1             1 0
```

## An abstraction

Let `G` be an abelian group of size `m` with operation `g` and whose group members are items of `u` . Compute `s←{g/(⍵≠⍳m)/u[h]}¨⍳m` ; `s[n]` is the “sum” of hat numbers that person `n` sees. Then the written number `w[n]` is `u⍳u[n] g gi s[n]` where `gi x` is the inverse of `x` in the group. (In Section 1 `G` is the group of integers under addition modulo `m` ; the group elements `u` are `⍳m` .)

Why does it work? Let `t←u⍳g/u[h]` be the index of the “sum” of all the hat numbers. The following are equal:

```
u[t]
u[t] g s[t] g gi s[t]  ⍝ group inverse and identity
s[t] g u[t] g gi s[t]  ⍝ associative and commutative
s[t] g u[w[t]]         ⍝ definition of w
```

Moreover, since `s[t]` is the sum of all hats excluding `u[h[t]]` and the group is abelian, it follows that `u[t]=s[t] g u[h[t]]` . Therefore `s[t] g u[w[t]]` and `s[t] g u[h[t]]` are equal, and so `u[w[t]]` and `u[h[t]]` are equal and consequently `w[t]` and `h[t]` are equal. That is, the written number `w[t]` for person `t` matches his hat number `h[t]` .

## 88 hats

The original puzzle has `m←88` . Since

```
   I←{⍵/⍳⍴⍵}
   I 88 = {+/1=⍵∨⍳⍵}¨⍳1000
89 115 178 184 230 276
```

There are at least 7 different strategies, derived from addition modulo 88 and multiplication modulo each `b` of `89 115 178 184 230 276` on the numbers co-prime to `b` . Thus:

```
win←{
 ⍝ winning strategy based on group table ⍺ for hat numbering ⍵
    assert ⍵∊⍳m←⍴⍵:
    ⍺←m|∘.+⍨⍳m       ⍝ default is + modulo m
    assert(⍴⍺)≡m,m:
    assert ⍺≡⍉⍺:     ⍝ must be abelian
    G←⍺[0;]⍳⍺        ⍝ standardize group table
    g←{G[⍺;⍵]}¨      ⍝ group operation
    gi←{G[⍵;]⍳0}¨    ⍝ inverses
    (⍳m) g gi (∘.≠⍨⍳m)g.×⍵
}

assert←{⍺←'assertion failure' ⋄ 0∊⍵:⍺ ⎕SIGNAL 8 ⋄ shy←0}
```

The left argument `⍺` of `win` is a group table. It is standardized by doing `⍺[0;]⍳⍺` , whence the group elements are `⍳m` and the identity element is `0` . These properties permit the computation of the “all but” sums to be simplified from `{g/(⍵≠⍳m)/u[h]}¨⍳m` to `(∘.≠⍨⍳m)g.×h` .

```
        mgrp←{⍵|∘.×⍨I 1=⍵∨⍳⍵}  ⍝ group table for × modulo ⍵

   mgrp 7
1 2 3 4 5 6
2 4 6 1 3 5
3 6 2 5 1 4
4 1 5 2 6 3
5 3 1 6 4 2
6 5 4 3 2 1

   mgrp 9
1 2 4 5 7 8
2 4 8 1 5 7
4 8 7 2 1 5
5 1 2 7 8 4
7 5 1 8 4 2
8 7 5 4 2 1
```

`mgrp ⍵` computes the group table for the integers under multiplication modulo `⍵` . The group elements are the numbers co-prime to `⍵` .

```
   m←88
   h←random m⍴m

   I h = win h
58

   I h = (mgrp  89) win h
5
   I h = (mgrp 115) win h
87
   I h = (mgrp 178) win h
27
   I h = (mgrp 184) win h
77
   I h = (mgrp 230) win h
82
   I h = (mgrp 276) win h
58
```

Even though `win h` and `(mgrp 276) win h` result in the same person (`58`) writing his hat number, the lists of written numbers are different:

```
   win h
41 9 72 79 53 39 7 8 32 72 85 26 45 47 2 16 46 80 53 ...
   (mgrp 276) win h
70 39 78 9 47 11 47 52 48 20 70 32 26 60 2 76 46 16 ...

   (win h)[58]
5
   ((mgrp 276) win h)[58]
5
   h[58]
5
```

## Collected definitions

```
assert←{⍺←'assertion failure' ⋄ 0∊⍵:⍺ ⎕SIGNAL 8 ⋄ shy←0}

I←{⍵/⍳⍴⍵}

mgrp←{⍵|∘.×⍨I 1=⍵∨⍳⍵}  ⍝ group table for × modulo ⍵

random←{⎕rl←7*5 ⋄ ?⍵}  ⍝ reproducible random numbers

win←{
 ⍝ winning strategy based on group table ⍺ for hat numbering ⍵
    assert ⍵∊⍳m←⍴⍵:
    ⍺←m|∘.+⍨⍳m       ⍝ default is + modulo m
    assert(⍴⍺)≡m,m:
    assert ⍺≡⍉⍺:     ⍝ must be abelian
    G←⍺[0;]⍳⍺        ⍝ standardize group table
    g←{G[⍺;⍵]}¨      ⍝ group operation
    gi←{G[⍵;]⍳0}¨    ⍝ inverses
    (⍳m) g gi (∘.≠⍨⍳m)g.×⍵
}
```

## References

1. <a id="ref1"></a>  [http://www.jsoftware.com/pipermail/general/2007-June/030272.html](http://www.jsoftware.com/pipermail/general/2007-June/030272.html)
2. <a id="ref2"></a>  [http://www.jsoftware.com/pipermail/chat/2007-June/000514.html](http://www.jsoftware.com/pipermail/chat/2007-June/000514.html)
