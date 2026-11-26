---
vid: '10500120'
title: 'About polynomials: Part 2'
subtitle: Part 2
authors:
- Gianluigi Quario
byline: Gianluigi Quario (giangiquario@yahoo.it)
volume: '24'
issue: '1'
page: '121'
description: About polynomials, Part 2/2
abstract: This is the second and concluding part of an article dedicated to polynomials. Evaluation of polynomials. Construction of functions for evaluation. Zero finding of a polynomial. A stable and accurate function for finding complex zeroes in polynomials of higher degree.
source: content/printed/241/polynomials.htm
converter: vecrescue 0.1.0
issue_tab: n1-december-2008
tags:
- mathematics and statistics
- programming techniques
---

Part 2
{ .subtitle }

Gianluigi Quario (giangiquario@yahoo.it)
{ .byline }

This is the second and concluding part of an article dedicated to polynomials. Evaluation of polynomials. Construction of functions for evaluation. Zero finding of a polynomial. A stable and accurate function for finding complex zeroes in polynomials of higher degree.
{ .abstract }

## Introduction

The classical problem of solving an Nth-degree polynomial equation

c\[N\]×Y<sup>N</sup> + c\[N-1\]×Y<sup>(N-1)</sup> + … + c\[1\]×Y<sup>1</sup> + c\[0\] = 0

has substantially influenced the development of mathematics throughout the centuries and still has several important applications to the theory and practice of present-day computing. The name *Fundamental Theorem of Algebra* itself evidences that.

The theorem was demonstrated by Gauss in 1799 and says that every polynomial equation of degree N has exactly N solutions in the complex field. The early mathematicians tried to express the solutions by means of general formulas including the coefficients of the polynomial and using just arithmetical operations and radicals. The cases of degree 3 and 4 were solved by Italian thinkers during 16th century. Then for more than two centuries mathematicians tried in vain to find analogous formulas for degree 5.

At the beginning of 19th century Ruffini & Abel demonstrated that it is not possible to have such general formulas from degree 5 up.

The following interest in the solution of polynomial equations was concerned with iterative methods to obtain approximate numerical values.

Many algorithms were proposed and realized. Wolfram Mathworld [\[1\]](#ref1) enumerates the following:

> Bailey’s Method, Bairstow’s Method, Bernoulli’s Method, Bisection, Brent’s Method, Crout’s Method, False Position Method, Graeffe’s Method, Halley’s Irrational Formula, Halley’s Method, Halley’s Rational Formula, Horner’s Method, Householder’s Method, Hutton’s Method, Inverse Quadratic Interpolation, Jenkins-Traub Method, Laguerre’s Method, Lambert’s Method, Lehmer-Schur Method, Lin’s Method, Maehly’s Procedure, Muller’s Method, Newton’s Method, Ridders’ Method, Schröder’s Method, Secant Method, Tangent Hyperbolas Method

There are state-of-art root-finding packages available using multiprecision, such as MPSolve implemented by Bini *et al.* [\[2\]](#ref2) and Eigensolve by Fortune [\[3\]](#ref3).

Nevertheless this subject of research is still a current topic.

Certainly one reason is that none of these algorithms can give adequate results in all realistic circumstances. The inevitable rounding errors generated during the calculation sometimes obstruct the convergence of the iterations, even when using extended-precision arithmetic.

The challenge for the present-day research is to develop algorithms of numerical solutions guaranteed and with reasonable computing time.

I’ll present a Dyalog APL implementation of Aberth’s method, following a Fortran 77 program written by D. Bini [\[4\]](#ref4).

I wish to thank Prof. D.A. Bini sincerely for his agreement to disclose current work.

## Complex numbers

When dealing with polynomials it is convenient to adopt complex numbers.

Complex number c corresponds to a point in a 2-dimensional plane. It can be represented with a pair of real coordinates (a, b) with the orthogonal real and imaginary axes forming a basis.

As this work is based upon Dyalog APL, that does not support computation with complex numbers, we will define a complex number to be a simple 2-element numeric vector. Each complex number is considered to be a scalar. For example:

```
      c ← ⊂1 1
```

is a complex scalar. And

```
      c← (1 1)(0 2)(1 0)
```

is a 3-element complex vector.

## Evaluation of real and complex polynomials

The following function returns the real values of a real polynomial using the Ruffini-Horner method:

```
ZrPoly←{
    ⍝ values at ⍵ of real polynomial with coefficients ⍺
    ⍝ e.g. : 1 3 3 1 ZrPoly ⍳9
     ⎕IO←0 ⋄ coe pts←⍺ ⍵
     ruffini←{⍺+⍺⍺×⍵}
     ⊃pts ruffini/coe
}
```

And this returns the complex values of a real or complex polynomial:

```
ZcPoly←{
    ⍝ values at ⍵ of complex polynomial with coefficients ⍺
    ⍝ e.g. : 1 3 3 1 ZcPoly ⍳9
    ⍝      : (1 0)(3 0)(3 0)(1 0) ZcPoly (1 0)(2 0)(3 0)(1 ¯1)(2 2)
     ⎕IO←0 ⋄ coe pts←Zr2c¨⍺ ⍵
     ⍝         ⍺  + ⍺⍺       ×         enclose if simple ⍵
     ruffini←{(⊂⍺)+¨⍺⍺{(⍺-.×⍵),⍺+.×⌽⍵}¨(,∘⊂∘,⍣(1=≡,⍵))⍵}
     ⊃pts ruffini/coe
}
```

where `Zr2c` transforms a real into a complex number

```
Zr2c←{
    ⍝ array of complex from simple array of real numbers
    ⍝ ⍵ is a simple array (or scalar) of real numbers
     2=|≡⍵:⍵  ⍝already complex (the depth of a complex is 2)
     ⍵,¨0
}
```

## Construction of functions for evaluation of polynomials

Let `poly` be a numerical vector of coefficients of polynomial p(Y),

p(Y) ←→ poly\[N\]×Y<sup>N</sup> + poly\[N-1\]×Y<sup>(N-1)</sup> + … + poly\[1\]×Y<sup>1</sup> + poly\[0\]

In the first part we have seen that the algebrists tend to study the polynomial functions as if they were not functions but generic objects. We continue to represent polynomials by their coefficient vector `poly` (ascending order). Vector `poly` is either a real or a complex vector.

Given `poly` it is natural to define the associated functions for its evaluation `poly∘ZrPoly` or `poly∘ZcPoly`.

When `poly` is real it is possible to define a direct function using only the primitives `+` and `x`. Let us start with a trinomial and consider that it can be evaluated by means of Ruffini-Horner method:

```
poly[0] + Y × (poly[1] + Y × poly[2])
```

The expression `poly[1] + Y × poly[2]` gives us the function

```
{(poly[1] ∘+) ∘ (poly[2] ∘× ) ⍵}
```

and the expression `Y × (poly[1] + Y × poly[2])`

```
{poly[0] ∘+ ⍵} × {(poly[1] ∘+) ∘ (poly[2] ∘× ) ⍵}
```

This is a monadic fork of three functions `{f ⍵} × {g ⍵}` and is equivalent to

```
×∘+∘(poly[1]∘+)∘(×∘+∘(poly[2]∘×)⍨
```

Now the final evaluation function of the trinomial is

```
PolyEval←(poly[0]∘+)∘(×∘+∘(poly[1]∘+)∘(×∘+∘(poly[2]∘×)⍨)
```

In a recursive way we can obtain for a polynomial of degree 3

```
PolyEval←(poly[0]∘+)∘(×∘+∘(poly[1]∘+)∘(×∘+∘(poly[2]∘+)∘(poly[3]∘×)⍨)⍨)
```

and similar direct functions for higher degree.

## Zero finding of a polynomial

From a functional point of view the zero-finding of polynomial `poly` is a straightforward problem. We have the function for polynomial evaluation and the *inverse* operator `⍣¯1` [\[5\]](#ref5).

Zero finding should be reached in this way:

```
      (poly∘ZcPoly⍣¯1) 0
```

or

```
      (PolyEval⍣¯1) 0
```

provided that the inverse operator is applied to a primitive or an expression of primitive functions combined with primitive operators. We can follow only the second way.

Let `poly` be the trinomial `2 3 1`. Then we have

```
      PolyEval←(2∘+)∘(×∘+∘(3∘+)∘(×∘+∘(1∘×)⍨)
```

and

```
      (PolyEval⍣¯1) 0
¯1
```

This is a genuine root of the trinomial.

I built this kind of function for a degree-99 polynomial. (It’s a very long string: you can find it in [\[6\]](#ref6).)

```
      poly ← 100⍴⌽⍳10
```

and the returned result was

```
¯1.338591185893
```

I cannot imagine the hard work done by the interpreter, but I feel admiration for John Scholes and its implementation. This is also a genuine root of the polynomial… even if the result has forgotten many companions.

Unfortunately the inverse operator is not so cute as to perceive that the function `PolyEval` is not invertible: the inverse operator is able to tell the truth but not the whole truth. We cannot follow the functional route.

## A stable and accurate function for finding complex zeroes in polynomials of higher degree

J has the primitive monadic function `p.` [\[7\]](#ref7) which finds roots of a polynomial by means of a internal iterative algorithm. I think that also the APL programmer needs such a tool.

Dyalog APL lacks extended-precision numbers and accordingly it is more difficult to obtain stability (avoid that errors introduced at one time step cannot grow unboundedly at later times) even with methods equipped with the property of convergence.

I found a Fortran program [\[3\]](#ref3) implementing the Aberth method in standard floating-point arithmetic.

The Aberth method is a root-finding algorithm for simultaneous approximation of all the complex roots of a univariate real or complex polynomial. It derives from Newton’s method, but is less susceptible to a failure of convergence.

This algorithm has the advantage of an upper limit to the number of iterations and that, besides the approximated roots, the output contains the corresponding error bounds.

An excerpt from its abstract:

```
X*************************************************************************
X*    NUMERICAL COMPUTATION OF THE ROOTS OF A POLYNOMIAL HAVING          *
X*        COMPLEX COEFFICIENTS, BASED ON ABERTH'S METHOD.                *
X*                    Version 1.4,  June   1996                          *
X*    (D. Bini, Dipartimento di Matematica, Universita' di Pisa)         *
X*                       (bini@dm.unipi.it)                              *
X*************************************************************************
An algorithm for computing polynomial zeros, based on Aberth's method, is
presented. The starting approximations are chosen by means of a suitable
application of Rouché's theorem. More precisely, an integer q  >=1 and a set
of annuli A i for i=1,...,q, in the complex plane, are determined together with
the number k i of zeros of the polynomial contained in each annulus A i. As
starting approximations we choose k i complex numbers lying on a suitable circle
contained in the annulus A i for i=1,...,q. The computation of Newton's
correction is performed in such a way that overflow situations are removed. A
suitable stop condition, based on a rigorous backward rounding error analysis,
guarantees that the computed approximations are the exact zeros of a “nearby”
polynomial. This implies the backward stability of our algorithm. We provide a
Fortran 77 implementation of the algorithm which is robust against overflow and
allows us to deal with polynomials of any degree, not necessarily monic, whose
zeros and coefficients are representable as floating point numbers. In all the
tests performed with more than 1000 polynomials having degrees from 10 up to
25,600 and randomly generated coefficients, the Fortran 77 implementation of our
algorithm computed approximations to all the zeros within the relative precision
allowed by the classical conditioning theorems with 11.1 average iterations. In
the worst case the number of iterations needed has been at most 17.
```

I translated it in a dynamic function (Dyalog APL v.11), taking some liberties with respect to iterations/recursions and the calculation of starting values. The function is named `p_` to evoke J’s primitive `p.`

```
⍝#########################################################################
⍝              //////////  A P L  documentation  \\\\\\\\\\\
⍝#########################################################################
⍝
⍝   The right_argument of this APL function is a vector that contains
⍝       the real or complex coefficients of a polynomial.
⍝   The left_argument ⍺:
⍝       when ⍺ is numeric, value of polynomial is calculated at ⍺.
⍝       when ⍺ is NOT numeric:
⍝         * case ⍺ is missing or 0-length vector: default case
⍝           The function returns a 2 elements vector:
⍝         [1]  hiCoe          scalar    is the highest_degree coefficient;
⍝         [2]  valueRoots     vector    of the computed approximations to
⍝              roots
⍝         * case ⍺='x' :  extended results
⍝         The function returns a 7 elements vector:
⍝         [1]  hiCoe          scalar    is the highest_degree coefficient;
⍝         [2]  valueRoots     vector    of the computed approximations to
⍝              roots
⍝         [3]  boolRoots      vector    is 1 for successful approx. of
⍝              the i-th root.
⍝              More specifically, the disk of center valueRoots[i] and
⍝              radius radiusRoots[i] ,in the complex plane, contains a root
⍝              of p(x)  for i=1,...,n.
⍝              It contains informations about the computed approximations:
⍝              = 1 if the approximation of the i-th root has  been
⍝              carried out successfully, i.e., the computed approximation
⍝              can be viewed as the exact root of a slightly perturbed
⍝              polynomial.
⍝              = 0  if more iterations are needed for the i-th  root
⍝              or if the corresponding root cannot be represented as
⍝              floating point due to overflow or underflow
⍝         [4]  radiusRoots    vector    is the absolute error bound
⍝              If there exist roots which cannot be represented without
⍝              overflow or underflow, then the corresponding components
⍝              of RadiusRoots are set to -1
⍝         [5]  radiusRoots÷Modulus(valueRoots)
⍝                             vector    is the relative error bound
⍝         [6]  moduRoots      vector    moduli of computed approximations
⍝         [7]  iterNo         scalar    is the number of iterations
⍝              needed by the algorithm.
⍝         [8]  msgs           vector    segmented string of warning msgs
⍝                                       (maybe empty)
⍝         * case ⍺='i' :  iteration runtime values
⍝         The function returns a 8 elements vector of case 'x' and:
⍝         [9]  valueInit      vector    2 elements:
⍝                                       - vector of complex init guesses
⍝                                       - bool vector of warnings over guesses
⍝         [10] valueIter      matrix    rows of guesses at each step
⍝         * case ⍺≡'rootcenter' :  the function
⍝         returns the root-center(centroid)of the (not computed) roots
⍝         * else :  default case

⍝   The right_argument of this APL function can be a 2 elements nested vector
⍝    that contains
⍝         [1]  hiCoe          scalar    is the highest_degree coefficient;
⍝         [2]  valueRoots     vector    the N roots of a N-degree polynomial
⍝         hiCoe and valueRoots can be real or complex
⍝    Then the function returns a (N+1)-elements vector  that contains
⍝       the complex coefficients of the polynomial.
⍝
```

Note that all the roots are approximated roots and that they are never rounded. The roots are always returned as complex numbers. Sometimes a real root is represented as having a imaginary part, but that is due to approximation of the real root over the complex plane.

For example the real roots ( -1 and -2) of real polynomial

```
      p_ 2 3 1     ⍝ short result
```

are returned as

```
(1 0)  (( ¯1 ¯1.058791184E¯22)  (¯2 1.009741959E¯28))
```

where `(1 0)` is the highest degree coefficient.

If we write

```
      'x' p_ 2 3 1 ⍝ extended result
```

we get

```
(1 0) ((¯1 ¯1.058791184E¯22)(¯2 1.009741959E¯28)) (1 1)
… (7.68274333E¯15 1.110223025E¯14)
… (7.68274333E¯15 5.551115123E¯15)  (1 2) 5 ('')
```

where we can see that the distance of the computed roots from the actual roots is less than

```
(7.68274333E¯15 1.110223025E¯14)
```

and that 5 iterations were made.

The function `p_` is tailored in order to solve polynomials of degree until 500 and 1Mb of available workspace is more than sufficient. It is possible to raise the maximum-allowed degree: you can change the value assigned to local variable `MAXDEG` inside `p_`. I made some rewarding tests with degree 2000.

`p_` is self-supported and contains some local functions for complex arithmetics.

When the argument of function `p_` is a 2-element nested vector, where the first element is a scalar and the second is a simple vector, the polynomial is returned whose roots are the elements of second vector.

For example `p_ 1(¯1 ¯2)` is the polynomial `2 3 1`

The function `p_` and its companion `p_Display` are stored in file `PolyRoot.DWS` [\[6\]](#ref6).

```
      p_Display 'x' p_ ¯1 9 8 7 6 5 4 3 2 1      ⍝ formatted display of extended result
1) valid=Y   8.757084243466E¯1 ¯8.910997943917E¯1  R= 2.E¯14 RR= 2.E¯14 modulus= 1.25E0
2) valid=Y   8.757084243466E¯1  8.910997943917E¯1  R= 4.E¯14 RR= 3.E¯14 modulus= 1.25E0
3) valid=Y   1.277725973022E¯1  1.319267183775E0   R= 4.E¯14 RR= 3.E¯14 modulus= 1.33E0
4) valid=Y   1.277725973022E¯1 ¯1.319267183775E0   R= 4.E¯14 RR= 3.E¯14 modulus= 1.33E0
5) valid=Y   1.011379823900E¯1  9.573914554732E¯19 R= 7.E¯16 RR= 7.E¯15 modulus= 1.01E¯1
6) valid=Y  ¯7.418904085081E¯1 ¯1.149403115215E0   R= 6.E¯14 RR= 4.E¯14 modulus= 1.37E0
7) valid=Y  ¯7.418904085081E¯1  1.149403115215E0   R= 6.E¯14 RR= 4.E¯14 modulus= 1.37E0
8) valid=Y  ¯1.312159604336E0   4.525676361664E¯1  R= 7.E¯14 RR= 5.E¯14 modulus= 1.39E0
9) valid=Y  ¯1.312159604336E0  ¯4.525676361664E¯1  R= 9.E¯14 RR= 7.E¯14 modulus= 1.39E0
  Centroid= ¯2.222222222222E¯1  4.440892098501E¯16
```

The workspace `PolyRoot.DWS` contains the variable `Describe` with some test polynomials.

<a id="references"></a>

## References

1. <a id="ref1"></a> [mathworld.wolfram.com/PolynomialRoots.html](http://mathworld.wolfram.com/PolynomialRoots.html)
2. <a id="ref2"></a> D.A. Bini and G. Fiorentino, *MPSolve: Numerical computation of polynomial roots* v. 2.0, FRISCO report (1998)
3. <a id="ref3"></a> S. Fortune, “An iterated eigenvalue algorithm for approximating roots of univariate polynomials”, *JSC* (2002)
4. <a id="ref4"></a> D.A. Bini “Numerical computation of polynomial zeros by means of Aberth’s method”, *Numerical Algorithms*, 13 (1996)
5. <a id="ref5"></a> [www.dyalog.com/help](http://www.dyalog.com/help/)
6. <a id="ref6"></a> `PolyRoot.dws` at [tech.groups.yahoo.com/group/dyalogusers/files/Software](http://tech.groups.yahoo.com/group/dyalogusers/files/Software)
7. <a id="ref7"></a> [www.jsoftware.com/jwiki](http://www.jsoftware.com/jwiki)
