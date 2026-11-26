---
vid: '10500560'
title: 'Letter: The ruler’s edge'
prefix: Letter
authors:
- Norman Thomson
byline: Norman Thomson
volume: '25'
issue: '3'
page: '103'
received: '2008-12-02'
online: '2011-08-06'
editor: Stephen Taylor
description: 'Letter: reply to Ray Polivka on “The ruler’s edge”'
source: content/printed/253/thomson.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- programming techniques
---

Norman Thomson
{ .byline }

It is a pleasure to be able to respond my old partner in education Ray Polivka [\[1\]](#ref1) on the subject of programming rulers. Readers may like to compare the different style of approach which might be taken in J to this problem, which, to remind readers, is to write a program for a ruler of length `y.` with numbers and tick marks at intervals of `x.`.

First pre-define an adverb index

```
   index=.1 : '(i.@$*x.)@]'
```

which identifies elements where a criterion verb such as ‘equals zero’ is met, for example:

```
   t
0 1 0 1
0 0 1 1
1 0 1 1
   (=&0)index t                  NB. give indices of 0s
0 0 2 0
4 5 0 0
0 9 0 0
```

This can be used in conjunction with the adverb *amend* (`}`) to provide a general purpose replacement facility, for example:

```
   99(=&0)index}t                NB. replace 0s with 99
99  1 99 1
99 99  1 1
 1 99  1 1
```

This is used in the final line of the verb `numbers`, which supplies the formatted numeric parts of the ruler:

```
   numbers=.4 : 0
t=.x.*>:i.>.y.%x.                NB. multiples of x.up to y.
t=.|:10 10 #:t                   NB. digitise and transpose
t=.y.{."(1)x.":t                 NB. put in spaced char form
t=.(' '(=&'0')index}{.t),:{:t    NB. blank ldng 0s
)
   3 numbers 17
           1  1
  3  6  9  2  5
```

Adding the tick marks, both horizontally or vertically, is now straightforward :

```
   rulera=.4 : 0
t=.x. numbers y.
t,(y.$((<:x.)#0),1){'-^'         NB. add tick marks
)
   rulerc=.4 : 0
t=.x. numbers y.
(|:t),.(0=x.|>:i.y.){'|+'        NB. add tick marks
)
   3 rulera 17
           1  1
  3  6  9  2  5
--^--^--^--^--^--

   3 rulerc 11
  |
  |
 3+
  |
  |
 6+
  |
  |
 9+
  |
  |
```

## References

1. <a id="ref1"></a> “The ruler’s edge revisited”, Ray Polivka, *Vector* Vol.23, No.4 [http://archive.vector.org.uk/art10012030](../art10012030/)
