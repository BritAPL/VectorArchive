---
vid: '10012490'
title: An Improvement to LOGGAMMA
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '10'
issue: '2'
page: '999'
source: trad/v102/thomson102_24.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- mathematics and statistics
- programming techniques
---

by Norman Thomson <span class="affil"></span> <span class="email"></span>
{ .byline }

(Step by Step Analysis of Variance, Education Vector, July 1993)

In the first place the name of the function is misleading since what LOGGAMMA delivers is log of gamma of half the argument. An excellent approximation for true log gamma can be obtained from Feller’s extension to Stirling’s formula for log factorial, namely

```
                                        1     1
ln n! = .5(ln 2n) + (n+.5)(ln n) - n + --- - ---n-3
                                       12n   360
```

(see An Introduction to Probability, William Feller). Here it is in APL:

```
[0]  Z←LOGFACT N
[1]  →0 IF 0=Z←N
[2]  Z←((0.5+0,N),÷¯1,12,¯360)+.×(⍟(○2),N),N,÷¨N,N*3
```

A final trivial adjustment must be made to allow for the fact that `⌈(n)=(n-1)!`

For the purposes of FTAIL the above routine is an unnecessary refinement. However there are circumstances where it is invaluable, for example in calculating binomial probabilities with large parameter values, or in applying the Fisher Exact Test.

(webpage generated: 8 December 2005, 05:10)
