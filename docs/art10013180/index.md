---
vid: '10013180'
title: 'At Play with J: Jacobi''s Method'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '11'
issue: '3'
page: '111'
source: trad/v113/mcdonnell113_111.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- mathematics and statistics
- programming techniques
---

by Eugene McDonnell
{ .byline }

### Parallel Jacobi

Warning: this column contains material which may either put you to sleep or turn you against applied mathematics altogether. To take some of the sting away I have added a problem which may give you some pleasure in trying to solve. If you completely distrust your ability to read descriptions of programs, no matter how well-written, I advise you to go at once to the section headed “Problem” and avoid the preliminary exposition, or the material following, valuable as it is.

### Background

Recently I had need of a program to perform eigenanalyses of square symmetric matrices, and went to Vector 9 3 for January 1993, which had Donald McIntyre’s article “Jacobi’s Method for Eigenvalues: an Illustration of J”. I refer you to that article for McIntyre’s lucid explanation of what the method is. In the course of transcribing his 11-line jacobi program, along with its sixteen subprograms and its seven utility verbs, I thought I saw the possibility of speeding it up significantly by taking advantage of some of the parallelism inherent in the problem. I have communicated with McIntyre concerning this, and he tells me that he has used this method for many years, beginning with a Fortran program which he obtained from someone many years ago, transcribing it into APL and recently, as his article shows, into J.

If you look at his program, you will see that at the heart of it are the lines

```
   r=. ((cos,-sin),sin,cos) (ia R)} I
   Q=. q ip |:r [ R=. r ip R ip |:r
```

The first line amends an identity matrix conforming to the argument matrix by replacing two of its diagonal elements and the two corresponding off-diagonal elements with a 2-by-2 rotation matrix. The elements amended are chosen by finding the off-diagonal element of maximum magnitude, say at row-column indices p,q, and inserting the 2-by-2 matrix items at locations (p,p), (p,q), (q,p) and (q,q). This amended identity matrix r is then used with two matrix products involving R, the original argument, and Q, originally an identity matrix. Those involving R have the effect of zeroing out elements (p,q) and (q,p) of R, while leaving the eigenvalues of R unaltered. When this operation has been performed a sufficient number of times, one finds that all of the off-diagonal elements are essentially zero, and that the diagonal elements are the eigenvalues of the argument matrix . Those involving Q produce the eigenvectors of the argument matrix.

The valuable book “Matrix Computations” by Golub and Van Loan describes this method (section 8.5), but because the search for (p,q) is O(n^2), goes on to suggest that it might be more efficient to select p and q in a more rigid way. For the case of a 4-by-4 argument, they suggest that p and q be selected in the following order:

```
   p   q
   0   1
   0   2
   0   3
   1   2
   1   3
   2   3
```

and go back to the beginning, repeating until a sufficiently good solution appears. Golub and Van Loan go on to point out that the rows of the (p,q) table can be arranged in a disjoint, or non-conflicting fashion:

```
     a           b           c
   0   1       0   2       0   3
   2   3       1   3       1   2
```

and that, in a parallel machine, separate processors can be assigned to perform the individual matrix product operations. For example, in the 4-by-4 case, two processors are needed, so that in step A one processor could do the (0,1) case and the other processor could do the (2,3) case; in step B one processor could do the (0,2) case and the other processor could do the (1,3) case; and similarly for step C. They point out that this method works only for even-order matrices, but that the odd case can be handled by bordering the argument matrix on the right and at the bottom with zeros, and then dropping these excess columns at the end. Thus the rotation matrices needed would look like this:

```
        step A         |        step B     |            step C
     c01  s01   0    0 | c02   0   s02   0 | c03   0    0   s03
    -s01  c01   0    0 |  0    0    0    0 |  0    0    0    0
proc1 0    0    0    0 |-s02   0   c02   0 |  0    0    0    0
      0    0    0    0 |  0    0    0    0 |-s03   0    0   c03
      0    0    0    0 |  0    0    0    0 |  0    0    0    0
      0    0    0    0 |  0   c13   0   s13|  0   c12  s12   0
proc2 0    0   c23  s23|  0    0    0    0 |  0  -s12  c12   0
      0    0  -s23  c23|  0  -s13   0   c13|  0    0    0    0
```

My contribution enters here. I realized that one doesn’t need a parallel machine to obtain the benefits of this parallel Jacobi method. One can combine the rotation matrices, since they are disjunct, as follows:

```
   step A                  step B                  step C
 c01  s01   0    0     c02   0   s02   0     c03   0    0   s03
-s01  c01   0    0      0   c13   0   s13     0   c12  s12   0
  0    0   c23  s23   -s02   0   c02   0      0  -s12  c12   0
  0    0  -s23  c23     0  -s13   0   c13   -s03   0   0  c03
```

This technique reduces the number of matrix products required for a matrix of size n by a factor of n%2. Thus the larger the matrix, the greater the savings. A 10-by-10 problem can be reduced by a factor of 5; a 100-by-100 problem by a factor of 50, and so forth.

### The Problem

Now we come to the playful part. As you can see, the row-column pairs to be included at each step must somehow be derived. In the case of a 4-by-4 matrix, we see that step A uses the pairs (0 1) and (2 3); step B uses (0 2) and (1 3); and step C uses (0 3) and (1 2). The problem is to determine a permutation z that produces the desired result. For example, for n=4 any of the following permutations will do:

```
  0 2 3 1
  0 3 1 2
  1 2 0 3
  1 3 2 0
  2 0 1 3
  2 1 3 0
  3 0 2 1
  3 1 0 2
```

If we set z=.0 3 1 2, we can experiment as follows:

```
  ]a=.(z&{)^:(i. <:#z) i. #z NB. all of the possible permutations
 0 1 2 3
 0 3 1 2
 0 2 3 1

  ]b=.((2!#z),2)$,a NB. exhibit all the pairs of items
 0 1
 2 3
 0 3
 1 2
 0 2
 3 1
```

```
   ]c=.(>/"1)b  NB. mask shows where lead item is greater than trail
 0 0 0 0 0 1

   ]d=.c |."_1 b NB. pairs with leading smaller item
 0 1
 2 3
 0 3
 1 2
 0 2
 1 3

   ]e=./:~d NB. pairs in ascending order
 0 1
 0 2
 0 3
 1 2
 1 3
 2 3
```

Problem 1: Define a verb which takes as argument a positive even integer n and yields a permutation which, repeatedly applied to a conforming identity permutation, produces, in successive pairs of items, all possible choices of 2 items from n, with no duplications.

Problem 2: How many of the !n permutations of even order n are solutions to problem 1?

Solutions to this problem may be sent by email to eem@ipsaint.ipsa.reuter.com or by ordinary mail to Eugene McDonnell / 1509 Portola Ave. / Palo Alto, CA 94306 / U. S. A.

### Principal verbs

The verbs described below were written for J8. If you are using an earlier version of J you may wish to get your system upgraded. Here are the verbs making up my solution to the parallel Jacobi problem. The two verbs CEA and CEAI produce identical results, but CEA is written using the rhetorical control structures which have been added to J recently (see my last article) and CEAI uses the algebraic control structures which have been in J from the beginning.

Each main verb CEA and CEAI (Complete EigenAnalysis) takes as argument a square symmetric matrix A and returns two conforming matrices, the first with the eigenvalues along the diagonal, and zeros elsewhere, and the second whose columns are the eigenvectors for the corresponding eigenvalues. They each test the parity of the number of rows of A. If this is even they laminate to A a conforming identity matrix, using the utility verb IM, and then apply the subverb PJ to this initial argument. If it is odd, the action is to border A on the right and the bottom with a column and row of zeros, using the utility verb bz, and then to apply CEA (or CEAI) to this, and at the end removing the bottom row and rightmost column of each matrix of the result with the utility verb ub.

```
CEA =. 3 : 'if. (2|#y.) do. ub"2 CEA bz y. else. PJ y.,:IM y. end.'
CEAI=.(PJ@(,:IM))`(ub"2@(CEAI@bz))@.(2:|#)
```

The subverb PJ (parallel Jacobi) takes as argument an array of two square matrices. It prepares four global variables for use by hsjr: a quantity eps as the product of a globally defined tolerance tol and the Frobenius norm of the first matrix, yielded by the utility verb NF; a quantity s, the number of rows in the first square matrix; a list k, the integers from 0 to s-1; and a list p, a permutation which will be used to alter the arrangement of the atoms of k, using the utility verb mxp. It then employs the verb hsjr (half of s Jacobi rotations) to the limit. At the limit, it yields the desired complete eigenanalysis of the original argument.

```
 PJ=. 3 : 0
 eps=:tol*NF {. y.
 s=:# {. y.
 k =: i. s
 p=:mxp s
 hsjr ^:_ y.
 )
```

The subverb hsjr (half of s Jacobi rotations) takes as argument an array of two square matrices. It begins by making a rotation matrix rm, using the verb RM. This rotation matrix is used with the first matrix of the argument to develop PJ0, the next stage of the eigenvalue matrix, one which has a smaller off-diagonal norm than the previous one, and setting to zero any of its elements which are less than or equal to the quantity eps, using the utility verb clean. Next, it uses the same rotation matrix rm with the last matrix of the argument, to develop PJ1, the next stage in the eigenvector matrix. The two matrices are laminated to give the result array.

```
 hsjr=.3 : 0
 rm=.(k=:p{k) RM {.y.
 PJ0=.((|:rm)+/ .*({.y.)+/ .*rm) clean eps
 PJ1=.({:y.)+/ .*rm
 PJ0,:PJ1
 )
```

The subverb RM (rotation matrix) builds a parallel Jacobi rotation matrix.

It takes as left argument a particular permutation of the integers from 0 through sP1. It fashions this into a two-column table t, then reverses those rows of t in which the first atom is greater than the second atom. An array cs of 2-by-2 cosine-sine matrices, one for each row of t, is formed, using the verb csm. These will be used to amend a matrix of zeros in locations specified by a conforming array of 2-by-2 boxes ix, whose atoms are each a 2-atom list derived from the corresponding row of t, formed using the utility verb CP (Cartesian product). For example, if a row of t is 2 3, the 2-by-2 boxes corresponding to it will be:

```
    +---+---+
    |2 2|2 3|
    +---+---+
    |3 2|3 3|
    +---+---+
```

Finally, a matrix of zeros is formed, conforming to the right argument y., and the positions in this corresponding to positions given by the matrices of ix will be amended with the corresponding matrices of cs, yielding the desired parallel Jacobi rotation matrix.

```
   RM=.3 : 0
 :
 t=.((-:s),2)$x.
 t=.(>/"1 t)|."0 1 t
 cs=.y. csm"2 1 t
 ix=.CP t
 cs ix}0:"0 y.
 )
```

The subverb csm (cosine-sine matrix) takes as left argument a square matrix and as right argument a 2-element list of indices for that matrix, the first element giving a row number and the second element giving a column number, with the row number less than the column number. If the entry in the matrix at that row-column position is zero, the result will be a 2-by-2 identity matrix. If it is nonzero the result will be a 2-by-2 Jacobi rotation matrix, using the verb makecs.

```
csm=.makecs`(=@(i.@2:))@.(0:=<@]{[)
```

The subverb makecs (make cosine-sine table) takes as left argument a square matrix and as right argument a 2-element list of indices for that matrix, the first element giving a row number and the second element giving a column number, with the row number less than the column number. It yields a 2-by-2 Jacobi rotation matrix.

```
makecs=. 3 : 0
 :
 tau=.(((<2#}. y.){x.)-(<2#{. y.){x.)%+:(<y.){x.
 t=.(*tau)%(|tau)+4 o. tau
 c=.%4 o. t
 s=.t*c
 (c,s),:(-s),c
 )
```

The subverb mxp (make index permutation) takes a positive even integer as argument and yields a list which is a permutation of the integers from 0 through one less than the argument. The permutation is such that when applied repeatedly to a conforming list, none of the successive pairs in the lists are equal.

```
mxp=.[: C. 0: ; <: , (,~ >:@|.)@>:@+:@i.@<:v
```

### Utility verbs

The utility verb CP takes a list as argument and returns the Cartesian product of the items of the list.

```
CP=.    {@;"1~
```

The utility verb IM takes as argument a matrix and yields an identity matrix having the same number of rows.

```
IM=.    [: = [: i. #
```

The utility verb NF takes a matrix argument and yields its Frobenius norm as result.

```
NF=.    [: %: [: +/ [: , *:
```

The utility verb clean takes a numeric array as left argument and a positive atom as right argument. It yields a conforming array as result, wherein each element of the left argument with magnitude less than the right argument is replaced by zero.

```
clean=. [ * ] < [: | [
```

The utility verb bz takes a matrix argument and yields a similar matrix bordered on the right and below by a new column and row of zeros.

```
bz=.    >:@$ {. ]
```

The utility verb ub takes a matrix argument and yields a similar matrix with the rightmost column and bottom row removed.

```
ub=.    _1 _1&}.
```

### Test Information

Alter the following value as desired to control accuracy and speed:

```
tol=.1e_6 NB. value should be in the range 1e_2 to 1e_17

 NB. Test matrices

   ]A=.1 1 1 1,1 2 3 4,1 3 6 10,:1 4 10 20
 1 1  1  1
 1 2  3  4
 1 3  6 10
 1 4 10 20

   ]m=.1.5 _1 _0.5,_1 2 _1,:_0.5 _1 1.5
   1.5 _1 _0.5
   _1  2   _1
 _0.5 _1  1.5

    ]r=.1 1 0.5,1 1 0.25,:0.5 0.25 2
   1    1  0.5
   1    1 0.25
 0.5 0.25    2

NB. test results, using tol as specified above (executed on a Macintosh)

   CEA A
  0.453835         0         0         0
         0  0.038016         0         0
         0         0   2.20345         0
         0         0         0   26.3047
  0.787275 _0.308686  0.530366 0.0601868
 _0.163234  0.723091  0.640331  0.201173
 _0.532107  _0.59455  0.391833  0.458082
  0.265358  0.168411 _0.393897  0.863752

   CEA m
           2         0       0
           0         3       0
           0         0       0
    0.707107 _0.408248 0.57735
 _9.8829e_10  0.816497 0.57735
   _0.707107 _0.408248 0.57735
    CEA r
 _0.0166473         0        0
          0
1.48012        0
          0         0  2.53653
   0.721208   0.44428 0.531483
  _0.686348   0.56211 0.461473
  _0.093729 _0.697601 0.710329
```

(webpage generated: 29 October 2007, 15:48)
