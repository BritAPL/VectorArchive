---
vid: '10005870'
title: 'J: A Second Lesson'
authors:
- Donald McIntyre
byline: by Donald B. McIntyre
volume: '11'
issue: '1'
page: '36'
source: trad/v111/mcintyre111_36.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- education
- language design
- mathematics and statistics
---

by Donald B. McIntyre
{ .byline }

A First Lesson (Education Vector, Vol.10 No.4, April 1994, pp.18-32) gave examples of J’s parts of speech (nouns, verbs, adverbs, and conjunctions), verb trains (hooks and forks), and the important concepts of items and the rank of verbs and nouns. These were illustrated as they applied to the computation of a similarity table, which is the basis of cluster analysis.

In this Second Lesson we illustrate (a) some ways of entering tables of data, (b) selecting and re-ordering data, and (c) the use of inner products (i.e. matrix multiplication).

Consider first two small tables, t1 and t2, of data on dairy products. The rows (items) are samples analysed for Energy (calories) and Calcium (mg).

```
             t1                t2
          165 285            165 285
          330 570            330 259
           30  52             30  15
           50  86             50  12
```

t1 can be entered in any of the following ways, and t2 similarly:

```
     t1=. > 165 285; 330 570; 30 52; 50 86
     x=. 165 330 30 50  [   y=. 285 570 52 86
     x,:y              NB. Laminate
  165 330 30 50
  285 570 52 86
     t1-: x,.y         NB. Append.  Matches t1
  1
     t1-: |: 2 4$x,y   NB. Transpose after reshape
  1
```

The verb $ reshapes its right argument as specified by the left argument. The verb ¦: transposes a table; i.e. it interchanges rows and columns, turning the table on its side.

With so small a table as t1 you might notice that a simple relationship is evident when you divide each item by the first item. Use "1 to specify that the division is to apply to the rank-1 cells of both arguments:

```
     t1 %"1 {.t1        NB. {. is head.  % is divide
       1        1
       2        2
0.181818 0.182456
 0.30303 0.301754
```

Define a verb as either a hook or a fork to produce the same result:

```
     h=. %"1 {.
     f=. [ %"1 {.      NB. [ returns its left argument
```

The rows are all multiples of any one row. Noting that the rows are not independent, a mathematician would say that t1 has a rank of 1. It is important to note that the word rank can have two meanings: it may mean the span, or number of linearly independent rows or columns; or it may mean (as in J) the number of items in the shape of a noun, the last k axes of an array determining its rank-k cells or k-cells. The context should make it clear which meaning is intended.

The rows of t1 are not independent because the first row corresponds to 1 cup of milk and the other rows to varying quantities of the same milk. Given the composition of a cup of milk, the table can be completed by knowing the volumes of the other items. The table-builder ( \*/ ) is the mathematician’s outer product:

```
     cup=. 165 285
     vol=. 1 2 0.182 0.303
     vol */ cup
     165     285
     330     570
      30.03   51.87
      49.995  86.355
```

Table t2 is very different, because the second row is clearly not a multiple of the first. The first row of t2 again corresponds to a cup of milk. The second row represents something with as many calories as in 2 cups of milk but with only about half the calcium. We cannot obtain it merely by taking a different quantity of milk.

No row of t2 is a simple multiple of any other; every row is a blend of two others. There is more information in t2 than in t1. A mathematician would say that t1 has a span of 1 and t2 has a span of 2. Taking rows 0 and 2 (J uses zero origin) as end-members we can use the matrix divide (%.) verb to solve two sets of linear equations and find the proportions required to blend rows 0 and 2 to produce rows 1 and 3. For example, to get row 2 we blend rows 0 and 2 in the proportions 0.4642 8.447, found by solving the equations:

```
         165 p  +  30 q    =  330
         285 p  +  15 q    =  259
     m=. 0 2{t2
     y=. 1 3{t2
```

The verb from \{ takes items from the right argument as specified by the left argument. Because both m and y must be transposed for use in the equations, and the result must be transposed to match t2, the verb we use is

matrix divide under transpose

```
         %.    &.    |:
```

This is analogous to doing multiplication by the addition of logarithms; first take logarithms of both arguments; then add the logarithms; and finally take the antilog.

plus under log

```
     3 +&.^. 4
  12

     ]w=. y %.&.|: m
  0.464198  8.44691
 _0.0641975 2.01975
```

We can show that these results are correct by an inner product (matrix multiplication); i.e. multiply the rows of w by the columns of m and sum the products.

```
                              cal Ca
                             ┌───┬───┐
                          M  │165│285│   Milk
                             ├───┼───┤
                          L  │30 │15 │   Light cream
                             └───┴───┘
                                 m
    ┌──────────┬───────┐     ┌───┬───┐
    │0.464198  │8.44691│  H  │330│259│   Half-and-half
    ├──────────┼───────┤     ├───┼───┤
    │_0.0641975│2.01975│  C  │50 │12 │   Heavy cream
    └──────────┴───────┘     └───┴───┘
               w                 y
```

In J we use the dot product conjunction (.) to define the inner product. However, because /. would be a J digraph, a space must separate the two symbols:

```
     ip=. +/ .*
     w ip m
   330 259
    30  15
```

The two verbs +/ and \* are composed by the dot-product conjunction to give the new verb ip. The syntax ( f=. g .h ) allows us to extend the pattern of the mathematician’s matrix multiplication by replacing g and h by verbs (either primitive or defined) other than +/ and \*.

w gives the weights required for blending the items of m to produce the items of y. The items of m are the end-members (Milk and Light Cream), which when blended in the proportions \{.w give what in California corresponds to the legal definition of Half-and-half. Because the minimum percentages of milkfat allowed are 3.5 for whole milk, 11.7 for half-and-half, and 20.0 for light cream, we can legally make half-and-half by blending equal quantities of milk and light cream, each of minimum standard. In t2, however, the units of volume are 1 tablespoon for cream but 1 cup for milk and half-and-half; so that, there being 16 tablespoons in a cup, we expect the proportions to be 0.5 cup of milk and 8 tablespoons of light cream. The proportions computed differ slightly from these expectations because energy and calcium are not perfectly correlated with milkfat, and the values in t2 may correspond to milk products above the minimum quality prescribed by law.

In J: A First Lesson (The Education Vector, Vol.10, No. 4, April 1994, pp.17-29) we saw how to compute a similarity table by determining the distances between each of six points in 4-dimensional space. We did this in order to introduce some notions, such as rank of nouns and verbs, with wide applicability in programming. Having now introduced the more specialised pattern of the inner product, we have an alternative way of computing a similarity table.

First define a verb that computes the squares of differences between items (rows of a table are its items). The conjunction atop (@) composes this verb, sqd, from the verbs square (\*:) and minus (- ):

```
    sqd=. *:@-
```

Then use sqd to form a new kind of inner product, ssd, that follows the pattern of matrix multiplication (rows of the left argument combine with columns of the right argument) to give the sums of squares of the differences:

```
    ssd=. +/ .sqd
```

The right argument of the verb ssd will be the transpose of the left argument. Now complete a train of either three verbs (a fork) or two verbs (a hook):

```
  fork=. [ ssd |:
  hook=. ssd |:
```

These are equivalent to the following:

```
       fork                    hook
  ┌─┬─────────────┬──┐   ┌─────────────┬──┐
  │[│┌─────┬─┬───┐│|:│   │┌─────┬─┬───┐│|:│
  │ ││┌─┬─┐│.│sqd││  │   ││┌─┬─┐│.│sqd││  │
  │ │││+│/││ │   ││  │   │││+│/││ │   ││  │
  │ ││└─┴─┘│ │   ││  │   ││└─┴─┘│ │   ││  │
  │ │└─────┴─┴───┘│  │   │└─────┴─┴───┘│  │
  └─┴─────────────┴──┘   └─────────────┴──┘
```

The table (J: A First Lesson) defining six points in 4-dimensional space (Herbert Hellerman’s data) is:

```
]hh=. >1 4 3 1;2 1 3 5;1 1 4 1;3 8 2 4;2 3 3 2;0 2 4 2
  1 4 3 1
  2 1 3 5
  1 1 4 1
  3 8 2 4
  2 3 3 2
  0 2 4 2
```

The squares of the distances are then computed by either the hook or the fork:

```
       hook hh

    0 26 10 30  3  7
   26  0 18 52 13 15
   10 18  0 66  7  3
   30 52 66  0 31 53
    3 13  7 31  0  6
    7 15  3 53  6  0

      (fork -: hook) hh
    1
```

Returning to the milk example, consider some ways in which we might rearrange and otherwise modify the contents of a table. Begin by appending the end-member items to the computed blends:

```
        ]n=. m,w ip m
     165 285
      30  15
     330 259
      50  12
```

Recover t2 by permuting the items of n as defined by a permutation vector; i.e. interchange rows 1 and 2:

```
        p=. 0 2 1 3
        p{n
     165 285
     330 259
      30  15
      50  12
```

The standard cycle representation of the permutation is distinguished by boxed data, which in this case show that items 2 and 1 are interchanged while items 0 and 3 remain unchanged:

```
        C. p
     ┌─┬───┬─┐
     │0│2 1│3│
     └─┴───┴─┘
        (p{n) -: (<2 1) C. n
     1
```

Consider a different permutation:

```
        p=. 0 2 3 1
        p { n
     165 285
     330 259
      50  12
      30  15
       C. p
     ┌─┬─────┐
     │0│3 1 2│
     └─┴─────┘
```

In this case item 0 remains in position 0; while items 3 1 2 cycle so that item 1 goes to position 3, item 2 goes to position 1, and item 3 goes to position 2.

```
      (p{n) -: (<3 1 2) C. n
```

To understand standard cycle representation (C.) consult the J Dictionary (Version 7, pp.93-94) and experiment with the following boxed table. In the definition of f note that when two verbs, say u and v, are composed as u&v, v modifies both arguments before passing them to u, i.e.

```
      x u&v y   is       (v x) u (v y)
```

&lt;"0 boxes rank-0 cells; &lt;"1 boxes rank-1 cells (rows of a table). Ravel items (,.) gives the table formed by ravelling each item, and therefore makes a table out of a list.

```
   y=. 6 3$'abcdefghijklmnopqr'

   f=. ,"1&(<"1)
                                p=. 4 5 2 1 0 3
   ]x=. (,.i.6) f y             p{x
┌─┬───┐                       ┌─┬───┐
│0│abc│                       │4│mno│
├─┼───┤                       ├─┼───┤
│1│def│                       │5│pqr│
├─┼───┤                       ├─┼───┤
│2│ghi│                       │2│ghi│
├─┼───┤                       ├─┼───┤
│3│jkl│                       │1│def│
├─┼───┤                       ├─┼───┤
│4│mno│                       │0│abc│
├─┼───┤                       ├─┼───┤
│5│pqr│                       │3│jkl│
└─┴───┘                       └─┴───┘

   ]c=. C. p
┌─┬───┬─────┐
│2│4 0│5 3 1│
└─┴───┴─────┘
```

Item 2 is unchanged; items 4 and 0 are interchanged; and items 5, 3 and 1 are cycled.

```
  (c C. x)-: p C. x
1
```

To standardise the cycle representation, “each boxed cycle is rotated to begin with its largest element; and the boxed cycles are put in ascending order on their leading elements.”

```
  (<0 4) C. x     (<1 5 3) C. x  ]z=. (1 5 3; 0 4) C.x
┌─┬───┐            ┌─┬───┐            ┌─┬───┐
│4│mno│            │0│abc│            │4│mno│
├─┼───┤            ├─┼───┤            ├─┼───┤
│1│def│            │5│pqr│            │5│pqr│
├─┼───┤            ├─┼───┤            ├─┼───┤
│2│ghi│            │2│ghi│            │2│ghi│
├─┼───┤            ├─┼───┤            ├─┼───┤
│3│jkl│            │1│def│            │1│def│
├─┼───┤            ├─┼───┤            ├─┼───┤
│0│abc│            │4│mno│            │0│abc│
├─┼───┤            ├─┼───┤            ├─┼───┤
│5│pqr│            │3│jkl│            │3│jkl│
└─┴───┘            └─┴───┘            └─┴───┘
  z-: (1 5 3; 0 4; 2) C.x
1
  z-: (1 5 3; 2; 0 4) C.x
1
  z-: (2; 4 0; 5 3 1) C.x
1
```

As we have already seen, the verb from \{ selects items (rows) from a table. This verb also allows scattered indexing:

```
       ]y=. 4 6$'abcdefghijklmnopqrstuvwx'
    abcdef
    ghijkl
    mnopqr
    stuvwx
      (<2 3){y
    p
      (3 1; 1 2; 2 0; 0 4){y
    time
```

One verb is sufficient for selection; the right argument specifies the table and the left argument the cells.

Tables larger than those used in the examples can be read from a DOS file created by another program or entered by a word-processor. The verb readmat makes input simple. The verb can be entered and used simply as a tool. I hope, however, that after working through these first two Lessons readers will be able to understand the definitions with the help of on-line experiments and the following comments.

First read the DOS file and remove null, new line (carriage-return), and end-of-file characters. Then cut at the line-feeds (removing the line-feeds in the process) and execute. The result is a numeric table:

```
   read=. 1!:1@<
   k=. 0 13 26{a.
   g=. #~ [: -. e.&k
   f=. ".;._2
   readmat=. [: f [: g read
```

For 1!:1 see J Dictionary (Version 7, Appendix E).

     a.     is the atomic vector of all 256 possible characters.      e.     is membership, symbolised in logic by the Greek letter epsilon.      ;.     is cut, which in this case applies ". to the delimited segments. The \_2 shows that the delimiter is the last item and that the delimiters are excluded from the result.      ".     Do; i.e. execute the character object.

So that the names f g k can be used again without affecting the verb readmat, we fix the definition and erase the named objects. f. is an adverb

```
   readmat=. readmat f.
   erase=. 4!:55@;:
   erase 'f g k'
```

The argument for readmat is the DOS file identification, including the path if necessary, given within single quotation marks; e.g.

```
   table=. readmat 'c:\mydir\mydata.dat'
```

### Footnotes

The span, in the sense of the number of linearly independent rows or columns, cannot exceed the number of rows or the number of columns, whichever is smaller. Moreover the span of a matrix product cannot exceed the span of the tables that produced it. The matrix product of a table with its own transpose is always a square symmetric table (i.e. a table identical to its transpose), and therefore Jacobi’s method can be used to find its eigenvalues. The span is then the number of non-zero eigenvalues. For further information see my: Jacobi’s Method for Eigenvalues: An illustration of J. Vector Vol.9, No.3, January 1993, pp.125-133. Also D.C. Murdoch Linear Algebra for Undergraduates, John Wiley, 1957, pp.42, 46, 70; B. Noble Applied Linear Algebra, Prentice-Hall, 1969, pp.89, 127.

The examples of blending milk and cream are taken from my Introduction to the Study of Data Matrices, in: Models of Geologic Processes, An Introduction to Mathematical Geology, American Geological Institute, 1969, in which the computations were done in APL.

(webpage generated: 2 October 2007, 05:02)
