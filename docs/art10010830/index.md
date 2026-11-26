---
vid: '10010830'
title: K
authors:
- Arthur Whitney
byline: by Arthur Whitney
volume: '10'
issue: '1'
page: '74'
source: trad/v101/whitney101_74.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- language design
- implementations
---

*by Arthur Whitney*
{ .byline }

### Introduction

K is the executable notation at the heart of a high performance programming platform. It is designed for analyzing massive amounts of real-time and historical data – ideal for financial modelling.

K is fast, comprehensive, accessible and productive. It gets its speed from an incremental compiler and well-designed memory management, data types and primitives. It gets its universality by combining easily with other products and including high-performance built-in spreadsheet, graphics and database management. The productivity and accessibility come from the design of the core language.

This paper gives some examples written in K and a brief summary of the language. This work came about because of my experience in using C and Fortran and my work designing, implementing and using several different programming environments including LISPs, object-oriented systems and APLs. In particular, I learned a lot from designing, implementing and using the very successful A+ at Morgan Stanley. I am convinced that the best possible programming environment, at least for finance, is one that is based on high-performance list processing with APL-like primitives. This gives the best combination of productivity and performance.

### Examples:

K has a small and powerful vocabulary. Every primitive operation is a single ASCII symbol. For example,

```
   p: 1.5 1.6             p gets 1.5 and 1.6
   q: 3   4               q gets 3 and 4
   p*q                    p times q
4.5 6.4
   +/p*q                  plus OVER p times q
10.9
```

The basic atoms of the language are characters, numbers, symbols and lambda-expressions. Lists are made up of items. Each item is a list or an atom. A table is a list of equal length lists. In mathematics instead of atom, list and table we say scalar, vector and matrix. For example,

```
   s: 3          a scalar, i.e. an atom.
   v: 3 4        a vector, i.e. a list of 2 atoms.
   m: (2 3 4     a matrix, i.e. a list of 2 lists of 3 atoms.
       5 6 7)
```

Then,

```
   +/v      plus OVER items of v, i.e. 3 + 4.
7

   +/m      plus OVER items of m, i.e. 2 3 4 + 5 6 7.
7 9 11

   +/'m     plus OVER EACH item of m, i.e. (+/2 3 4;+/5 6 7).
9 18

   +\v      plus SCAN items of v, i.e. (3;3+4).
3 7
```

\+ and \* are binary atomic functions. / \\ and ’ are operators. Operators, which are spelled in capitals in the commentary, take functions as arguments and derive related functions as results. E.g. + is plus and +/ is sum. In general,

```
   f/ x     applies f successively OVER the items of x.
   f\ x     applies f successively SCANning the items
     of x from beginning to end.
   f' x     applies f to EACH of the items of x in
     parallel.
```

Also,

```
   v+'v     v plus EACH v, i.e. (3+3;4+4).
6 8
   v<\v     v less-than EACHLEFT v, i.e. (3<3 4;4<3 4).
0 1
0 0
   v</v     v less-than EACHRIGHT v, (3 4<3;3 4<4).
0 0
1 0
```

The binary cases of the functions derived from / \\ and ’ are all parallel operations. In general,

```
   x f' y     applies f to EACH of the items of x and y ⍲
     pairwise.
   x f\ y     applies f to EACH of the items on the
     LEFT(x) while keeping y fixed.
   x f/ y     applies f to EACH of the items on the
     RIGHT(y) while keeping x fixed.
```

A function f is atomic if and only if f is equivalent to f’. For example,

```
   (v+v) ~ v+'v     v plus v is-equivalent-to v plus EACH v.
```

Note,

```
   v*m     v times m, i.e. (3*2 3 4;4*5 6 7).
  6  9 12
 20 24 28
   +/v*m     plus OVER v times m, i.e. 6 9 12+ 20 24 28.
26 33 40
```

Composition arises from the juxtaposition of functions. For example,

```
   ~=     not equal.
   ~<     not less than, i.e. more than or equal.
   ~>     not more than, i.e. less than or equal.
   +/*     plus OVER times ⍲ generalized dot
     product.
   (+/*)\     (plus OVER times) EACHLEFT ⍲ matrix
     product.
```

In general, for functions f and g:

```
((f g)x) ~ f g x     (f compose g) of x is equivalent to f of g of x.
(x(f g)y) ~ f x g y     x(f compose g)y is equivalent to f of x g y.
((x f)y) ~ x f y     (x with f)of y is equivalent to x f y.
```

For example, from linear algebra we know that:

```
s*     is a linear transform from R to R.
m(+/*)\     is a linear transform from R2 to R3.
```

Of course these lists do not have to be rectangular nor of uniform type. Therefore lists are a simple yet powerful extension to the rectangular homogeneous arrays of the original APL. There is no need for enclose or disclose. Moreover, the parallel operators / \\ and ’ simplify and generalize, and thus obviate, inner and outer products as well as the bracket-axis operator.

Another important generalization is making expressions first-class data objects and allowing them any number of arguments. The application of an expression to its arguments is done with brackets. For example, the fundamental calculation in finance is present value, i.e. price.

Given a list of cashflows c that come at corresponding times t with a prevailing discount rate d, then ...

```
   pv:{[c;t;d]+/c*d^t}   pv gets: sum of cashflow times d to the power t.
```

The discount rate is the reciprocal of 1 plus the interest rate, for example, a discount rate of 0.9 corresponds to approximately 11%, and means that 1 dollar one year from now is worth 90 cents today.

For example, a 3-year bond issued with an annual 10% coupon would have:

```
   c: 0.1 0.1 1.1     cashflow is coupon, coupon and
     principal+coupon
   t: 1   2   3     at years 1 2 and 3.
```

Then,

```
   pv[c;t;0.9]     present value of the bond at 0.9 annual discount
0.9729     is approximately 97 cents.
```

This formula is well-known and widely used. Unfortunately it is also inexact. It assumes a constant interest rate over the life of the bond. The right way to make this calculation is to use the current prices of all relevant financial instruments to derive a discount function d\[t\] (read d of t). d\[t\] is the present value of a dollar at all times t. So now the formula is:

```
   pv:{[c;t;d]+/c*d[t]}     pv gets: sum of cashflow times discounts at
     times t.
   pv[c;t;{[t] 0.9^t}]     call pv with c, t and lambda-expression for d.
0.9729
```

Instead of storing a function as an expression we could store the actual mapping itself. This can be much more efficient, e.g. in this case,

```
   d: 1 0.9 0.81 0.729     d gets discount for years 0,1,2 and 3.
   pv[c;t;d]     ... and once again ...
0.9729
```

Note the pv definition didn’t change. Function call and indexing use the same notation, i.e. if the left argument is a list then index – if it is an atomic lambda-expression then execute. This is a powerful idea. A list v is often a very effective representation of a function from 0 1 2 ... to v\[0\] v\[1\] v\[2\] ... . For example,

```
   v:1 2 3 4 5     define a list v, i.e. the range of v.
   !#v     enumerate count v, i.e. the domain of v.
0 1 2 3 4
   v[3]     v at 3
4
   f:{[x]1+x}     define a functional superset of v.
   f[3]     f at 3
4
```

It is possible to fix some arguments to a function just as it is possible to specify just some axes in indexing. In the following example m and f both implement + – m1 and f1 both implement 1+.

```
   m:n+\n:!4     m gets n plus EACHLEFT n gets enumerate 4,
     i.e. m is defined to be the 4 by 4 addition table.
   m
0 1 2 3
1 2 3 4
2 3 4 5
3 4 5 6
```

```
f:{[x;y]x+y}     f is defined to be addition.
   m[1;2]     m at 1 2
3
   f[1;2]     f at 1 2
3
   m1:m[1]     m1 gets m at row 1.
   m1
1 2 3 4
   f1:f[1]     f1 gets f with x fixed at 1.
   m1[2]     m1 at 2
3
   f1[2]     f1 at 2
3
```

The \[ and \] are for convenience. The underlying primitive is @.

### Summary

K stands for “keys to the kingdom”.

The keys to expressiveness include:

     0\.     simple and general atom and list data types.  
     1\.     simple and general atomic and list primitives.  
     2\.     a single ASCII symbol for each primitive.  
     3\.     the identification of list indexing with function call evaluation.  
     4\.     symbolic indexing (not described here).  
     5\.     lambda-expressions with any number of arguments.  
     6\.     function composition.

The keys to performance include:

     0\.     constant time access, i.e. indexing.  
     1\.     constant time searching, i.e. hashing.  
     2\.     constant time append, i.e. clever memory management.  
     3\.     linear time sorting.  
     4\.     parallel functions and operators.  
     5\.     in general, having primitives that can express the important algorithms.  
     6\.     incremental compiler.

In summary, what is K? It is a powerful programming language. Why use K? For productivity and performance. What is new, different and exciting?

     0\.     The lists of K are a simple superset of the rectangular arrays of APL.  
     1\.     Every primitive is a single ASCII symbol – no non-standard characters.  
     2\.     User-defined functions can have any number of arguments.  
     3\.     Indexing has cross-sectional, scatter and symbolic access.  
     4\.     Lambda-expressions are an atomic data type.  
     5\.     There is no branching – control is through iteration and parallel operators.  
     6\.     Bracket axis, inner product and outer product are all superseded by EACH.

### Conclusion

K can run on any architecture but runs especially well on high-bandwidth RISC and parallel RISC. This is because of its emphasis on arrays and parallel operations on the arrays. The natural tendency in K to write parallel solutions to problems helps make K an ideal language for distributed and parallel processing.

(webpage generated: 5 December 2005, 18:50)
