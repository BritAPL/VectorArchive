---
vid: '10012090'
title: 'About polynomials: Part 1'
subtitle: Part 1
authors:
- Gianluigi Quario
byline: Gianluigi Quariogiangiquario@yahoo.it
volume: '23'
issue: '4'
page: '132'
description: Vector, the journal of the British APL Association
keywords:
- algebra
- APL
- array
- computation
- computing
- J
- K
- mathematics
- programming
- programming language
- programs
- Q
abstract: 'Abstract This is the first part of an article dedicated to polynomials: some thoughts about polynomials, their evaluation, their functional treatment; how APL helps us to understand the strict ties between polynomial evaluation and division; and a new insight into polynomial division. The second part of the article addresses the problem of finding zeros.'
source: trad/v234/quario.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2008
tags:
- mathematics and statistics
- programming techniques
---

Part 1
{ .subtitle }

Gianluigi Quario  
giangiquario@yahoo.it
{ .byline }

**Abstract** This is the first part of an article dedicated to polynomials: some thoughts about polynomials, their evaluation, their functional treatment; how APL helps us to understand the strict ties between polynomial evaluation and division; and a new insight into polynomial division. The second part of the article addresses the problem of finding zeros.
{ .abstract }

A univariate polynomial p(Y) of degree N is defined to be a formal expression in the CMN (common mathematical notation) of the form:

p(Y) ⇔ c\[N\]×Y<sup>N</sup> + c\[N-1\]×Y<sup>(N-1)</sup> + … + c\[1\]×Y<sup>1</sup> + c\[0\]

Here the expression is in descending order, Y is a formal symbol, and every power of Y is just a placeholder for a coefficient c\[k\].

Suppose that in the APL environment `⎕IO←0`, `c` is a numeric vector, `c[0]` is the constant term in the polynomial and `c[k]` is the coefficient of Y<sup>k</sup>.

One can associate with every polynomial a polynomial function, whose values are obtained everywhere by replacing symbol Y by a numerical value.

The reason that mathematicians distinguish between polynomials and polynomial functions is subtle and not worth investigating here. We shall later consider the polynomials as vectors representing polynomial functions and ponder about their functional status.

Nonetheless we’ll consider the problem of their evaluation.

Some hints were given by J’s dictionary and by suggestions from Phil Last, Stephen Mansour and Stephen Taylor. The APL expressions are written in Dyalog.

## Handy definitions

A polynomial of degree 0 is a *constant polynomial*.

When c\[0\]=0 and N=0 the polynomial is called a *zero polynomial* or *null polynomial*.

An *integer polynomial* is a polynomial where all elements of `c` are integer.

A polynomial, all of whose `c` elements are zeroes with exception of one element, is called a *power polynomial* or *monomial polynomial*.

For example: Y<sup>3</sup> or -5×Y<sup>5</sup> or 3×Y

A polynomial is said to be *primitive* if the greatest common divisor of its coefficients is 1.

For any field F (Integers, Rationals, Reals, Complexes, etc.) the polynomials with coefficients in F form a ring which is denoted by F\{\}.

A polynomial p(Y) in F\{\} is called irreducible over F if it is non-constant and cannot be represented as the product of two or more non-constant polynomials from F\{\}.

This definition depends on the field F.

For example: p(Y) ⇔ Y<sup>2</sup>+1 is irreducible over Real field R but not over Complex field C.

## APL evaluation of a polynomial function

A polynomial function can be evaluated in several ways.

Let `poly` be a numerical vector of coefficients of polynomial p(Y); let `point` be a numerical scalar or array.

Then the value of polynomial p(Y) at `point` is

Ruffini-Horner (a)
:   poly\[0\] + point × poly\[1\] + point × poly\[2\] + … point × poly\[N\]

Sum of power monomials (b)
:   (point∘.\*⍳⍴poly)+.×poly

Base value (c) – internal Ruf-Horn method valid also when poly is a null polynomial
:   (point∘.+,0)⊥⌽poly

Ruffini-Horner (d)
:   ⊃\{⍺+point×⍵\}/poly

Ruffini-Horner (e)
:   ⊃+∘(point∘×)/poly

Ruffini-Horner (f)
:   ⊃point\{⍺+⍺⍺×⍵\}/poly

Chain of forks (g)
:   PolyEval←(poly\[0\]∘+)∘(×∘+∘(poly\[1\]∘+)∘(×∘+∘(poly\[2\]∘+)∘  
               … (×∘+∘(poly\[3\]∘+)∘(poly\[4\]∘×)⍨)⍨)⍨)  
    PolyEval point

Form (a) shows the superiority of Iverson notation to the <acronym title="Common Mathematical Notation">CMN</acronym>:

poly\[0\] + point × (poly\[1\] + point × (poly\[2\] + … point × poly\[N\] ) … ))

The Iverson notation is neat, does not use any parentheses, and shows a strict symmetry between addition and multiplication. If we could couple the primitive *reduce* operator with an array of functions `+ ×` we could write:

(+ ×)/poly\[0\],point,poly\[1\],point,poly\[2\], … ,point,poly\[N\]

The J language does not provide for arrays of functions, but includes the wonderful idea of gerunds.

The polynomial can be evaluated in J by means of conjunction *tie* in this way:

\+'\*/poly\[0\],point,poly\[1\],point,poly\[2\], … ,point,poly\[N\]

Form (f) stresses the prominence and simplicity of Horner’s algorithm and shows a greater semantic clarity than the form (c), which internally uses – but also hides – this algorithm.

Form (g) is an example of another way to define the evaluation function of `poly`.

It is reported here because it is a *direct assignment* function, to which the inverse operator `⍣¯1` can be applied (it could be useful for finding a root of `poly`).

## Functional background of polynomials

The set of all polynomials of degree ≤N, over a field (or a ring) F, is denoted F\{N\}.

With the usual algebraic operations, F\{N\} is a vector space, because it is closed under addition (the sum of any two polynomials of degree ≤N is again a polynomial of degree ≤N) and scalar multiplication (a scalar times a polynomial of degree ≤N is still a polynomial of degree ≤N).

There is a simple isomorphism between F\{N\} and the vector space F<sup>(N+1)</sup>.

The standard basis for F\{N\} is the base \{1, Y, Y<sup>2</sup>, … Y<sup>N</sup>\}.

This basis is also called the *monomial basis* and comes from the standard basis for F<sup>(N+1)</sup>.

If F is the field of Integers/Rationals/Reals, the APL vector `poly` (`⍴poly` ←→ N+1) of coefficients of any polynomial can represent a unique element of the vector space F\{N\} .

Hence we can think of a numerical vector as a generic polynomial: furthermore it is straightforward to imagine an isomorphism between functional programming (over polynomials) and traditional data programming (over APL numerical vectors) … *sometimes Functional Programming can be carried out with no need to be mixed up with functions*!

The vector space of all polynomials with coefficients in F forms a commutative ring denoted F\{\} and is called the ring of polynomials over F. The symbol Y is commonly called the *variable*, and this ring is also called the *ring of polynomials in one variable* over F, to distinguish it from more general rings of polynomials in several variables. This terminology is suggested by the important cases of polynomials with real or complex coefficients, which may be alternatively viewed as real or complex polynomial functions.

However, in general, Y, and its powers Y<sup>k</sup>, are treated as formal symbols, not as elements of the field F. One can think of the ring F\{\} as arising from F by adding one new element Y that is external to F and requiring that Y commute with all elements of F. In order for F\{\} to form a ring, all powers of Y have to be included as well, and this leads to the definition of polynomials as linear combinations of the powers of Y with coefficients in F.

If F is the field of Integers/Rationals/Reals, the set of all APL numerical vectors (any length) can represent the set of the ring F\{\}. Note that a null polynomial can be represented by a zero-length vector or by the vector `,0`.

## Functional management of polynomials

### Sum and product

A ring has two binary operations, addition and multiplication.

In the case of the polynomial ring F\{\}, let `poly1` and `poly2` be the vectors representing two polynomials, then these operations are explicitly given by the following definitions:

```
      polySUM  ← {deg←⊃⌈/⍴∘,¨⍺ ⍵ ⋄ (deg↑⍺)+deg↑⍵ }
      polyPROD ← {+⌿(⎕IO-⍳⍴,⍺)⌽⍺∘.×⍵,1↓0×⍺}
```

where the arguments and results of `polySUM` and `polyPROD` are all vectors.

The identity element for addition is the null polynomial.

The identity element for multiplication is the polynomial `,1`.

Let us use the following function for polynomial evaluation:

```
      PolyEval ← {⊃⍵{⍺+⍺⍺×⍵}/⍺}
```

Then the functional highlight is that – for any point `point` – the following pairs of expressions are equivalents:

```
      (poly1 PolyEval point)+(poly2 PolyEval point)
      polySUM PolyEval point

      poly1 PolyEval poly2 PolyEval point
      polyPROD PolyEval point
```

Note that this definition of the multiplication of two polynomials is assimilable to the `∘` function-composition operator or – in maths speech – the ‘Discrete Convolution’ of the polynomial functions. In fact the following two are also equivalent:

```
      (poly1∘PolyEval)∘(poly2∘PolyEval)
      polyPROD∘PolyEval
```

Note that the product of two primitive polynomials is also primitive.

Over the Complex field, every non-constant polynomial can be unambiguously factored into linear factors. In the CMN:

p(Y) ⇔ poly\[N\] × (Y-z1) × (Y-z2) × … × (Y-zN)

or (in APL), p(Y) is given by:

```
      PolyMult ← {+⌿(⎕IO-⍳⍴,⍺)⌽⍺∘.×⍵,1↓0×⍺}
      poly[N] × ⊃PolyMult/-(z1,z2 … zN),¨¯1
```

where `poly[N]` is the leading coefficient of the polynomial and the `z`s are the zeroes of p(Y).

Hence, over the Complex field, all irreducible polynomials are of degree 1: this is the fundamental theorem of algebra.

So, over the Complex field, the polynomial represented by vector `poly` can also be represented by `poly[N]` jointly with vector `z1, z2 … zN`. The J language has chosen to represent any polynomial either with `poly` or the nested vector:

```
    (poly[N])(z1,z2 … zn)
```

### Division

When the ring F\{\} has the *zero-product* property (this is true for Integers, Rationals, Reals, Complexes), it is possible to define a division between the polynomials of the ring F\{\}.

If *f* and *g* are polynomials and *g* is not the null polynomial, then there exist unique polynomials *q* and *r* such that:

*f* = *qg* + *r*

where the degree of *r* is smaller than the degree of *g*.

This division is called division with remainder. The polynomial *f* is said to be divisible by *g* when remainder *r* is the null polynomial.

At school we learned Ruffini’s ‘simple-division’ rule to obtain the quotient polynomial *q* and the remainder polynomial *r*.

The following function performs this algorithm; it is a transcription of a C-language procedure.

```
     ∇
[0]   Z←u PolyDivi_0 v;n;nv;q;r;k;j;⎕IO    ⍝division of 2 general polynomials
[1]     ⍝ cfr Numerical Recipes in C :the art of scientific computing
[2]     ⍝ u v  ⍝numerator and denominator polynomials
[3]     ⍝ q r  ⍝quotient and remainder
[4]       ⍝ void poldiv(float u[], int n, float v[], int nv, float q[], float r[]
[5]       ⍝ the coefficients of quotient poly are returned in q[0..n] and the coeff
[6]       ⍝ of remainder poly are returned in r[0..n]
[7]       ⍝ the elements r[nv..n] and q[n-nv+1..n] are returned as zero
[8]   ⎕IO←0
[9]   n←¯1+⍴u  ⍝degree of numerator
[10]  nv←¯1+⍴v ⍝degree of denominator
[11]  r←u
[12]  q←(⍴u)⍴0
[13]  :For k :In ⌽⍳1+n-nv
[14]     q[k]←r[nv+k]÷v[nv]
[15]     :For j :In ⌽k+⍳nv
[16]        r[j]-←q[k]×v[j-k]
[17]     :EndFor
[18]  :EndFor
[19]  r[nv+⍳1+n-nv]←0
[20]  Z←q r
     ∇
```

For example

```
      (q r) ← 5 4 6 4 1 PolyDivi_0  1 3 1
      q
2 1 1 0 0
      r
3 ¯3 0 0 0
```

Given a divisor polynomial *g*, every polynomial in F\{\} is associated to two polynomials *q* and *r*, and

(degree *f*) ≥ (degree *q*) + (degree *r*)

But we can define a slightly different polynomial division where, given a divisor polynomial, every polynomial in F\{\} is associated with a single polynomial in F\{\} of the same degree.

For that purpose it is necessary to look at Ruffini’s rule and adapt its algorithm by means of Horner’s algorithm coupled with the ‘Accumulating reduction’ D-operator

```
      acc ← {⊃⍺⍺{(⊂⍺ ⍺⍺⊃⍬⍴⍵),⍵}/1↓{⍵,⊂⍬⍴⍵}¯1⌽⍵}
```

Let us start with the simple case where the divisor *g* is a monomial (1-degree) polynomial.

Then consider the result `s` of:

```
      s ← (-⊃g) {⍺+⍺⍺×⍵} acc f
```

We obtain a vector `s` of the same length as `f`, composed by the catenation or the remainder polynomial `r` and the quotient polynomial `q`.

The first element of `s` is also the value of polynomial `f` evaluated at point `⊃g`.

For example

```
      1 {⍺+⍺⍺×⍵} acc 5 4 6 4 1
20 15 11 5 1
```

The remainder of dividing `5 4 6 4 1` by `¯1 1` is `20` and the quotient is `15 11 5 1`.

By means of the simple expression `{⍺+⍺⍺×⍵}` and operator `acc`, it is possible to implement Ruffini’s ‘simple division’ rule between a polynomial and a monomial: we have neither iterations nor recursions.

Funny! The polynomial division is achieved through sum and product.

Note that the APL notation emphasises that Ruffini’s rule is just a more general form of the polynomial evaluation function:

```
      (-⊃g) {⍺+⍺⍺×⍵} / f
```

In the case where the degree of divisor `g` is greater than one, the polynomial division is usually executed by means of the ‘long division’ algorithm. Yet it is enough to extend the ‘simple division’ rule above; now we can perform it for any divisor:

```
PolyDivi←{
  ⍝ division of polynomial ⍺ by  polynomial ⍵
  ⍝ higher degree synthetic division algorithm
  ⍝ return:  vector (remainder,quotient) polynomial
    ZeroClean←{(-+/∧\⌽⍵=0)↓⍵}   ⍝ purge highest_degree_terms when coefficient is 0
    num den←ZeroClean¨⍺ ⍵
    0∊⍴num:num num                ⍝ NULLpoly ÷ poly or NULLpoly  ←→ NULLpoly
    0∊⍴den:'DENOMINATOR ERROR'⎕SIGNAL 11
    1=⍴den:num÷den
    (⍴num)<⍴den:num
    acc←{                         ⍝ accumulating reduction
        ⊃⍺⍺{(⊂⍺ ⍺⍺⊃⍬⍴⍵),⍵}/1↓{⍵,⊂⍬⍴⍵}¯1⌽⍵}
    extRuffini←{                  ⍝ extended Ruffini-Horner. cfr {⍺+⍺⍺×⍵}
        ((⍴⍺⍺)↑(⊃⍺),⍵)+⍺⍺×¯1↑⍵}
    synt←(-¯1↓den÷¯1↑den)extRuffini acc (⍴1↓den),/num
    (⊃synt),⊃¨⌽¨1↓synt
}
```

For example

```
      5 4 6 4 1 PolyDivi 1 3 1
3 ¯3 2 1 1
```

Into the bargain, polynomial division with a fixed polynomial divisor can be considered as something more than an operation that returns a quotient and a remainder: it can be seen as a one-to-one correspondence between F\{\} and F\{\}.

Given the polynomial *f* with degree N we obtain a polynomial *s* with degree N, thus there is also a one-to-one correspondence between F\{N\} and F\{N\}.

### Derivative and Integral

Inside any ring F\{\} or F\{N\} it is possible to define another correspondence, the *derivative*: this is a many-to-one correspondence.

The following function returns the derivative `s` of any polynomial `f`:

```
      PolyDerivative ← {(1<⍴⍵)↓⍵ ×-⎕IO-+⍳⍴⍵}
```

The degree of `s` &lt; degree `f` (except when `f` is a constant or null polynomial).

For example:

```
      PolyDerivative  5 4 6 4 1
      4 12 12 4
```

We can also have a one-to-many correspondence inside F\{\} : the *integral*.

Since a single polynomial can have an infinity of integral polynomials, it is not possible to define a suitable APL function. But we may anchor any number of the field F and build a function that defines a one-to-one correspondence:

```
      PolyIntegral←{⎕IO←1 ⋄ ⍺,⍵÷⍳⍴⍵}
```

The degree of `s` > degree `f` (except when `f` is a null polynomial).

For example:

```
      33∘PolyIntegral 5 4 6 4 1
33 5 2 2 1 0.2
```

## Perspective

Polynomial evaluation is the first step to the old problem of root finding. The second part of this paper will address the task of defining a stable and wide range function for the extraction of complex roots.

## Further Reading

1. <a id="ref1"></a>[http://en.wikibooks.org/wiki/Abstract\_algebra](http://en.wikibooks.org/wiki/Abstract_algebra)
2. <a id="ref2"></a>[http://mathworld.wolfram.com/topics/Algebra.html](http://mathworld.wolfram.com/topics/Algebra.html)
3. <a id="ref3"></a>[http://www.jsoftware.com/jwiki](http://www.jsoftware.com/jwiki)
4. <a id="ref4"></a>[http://www.dyalog.dk/dfnsdws/n\_contents.htm](http://www.dyalog.dk/dfnsdws/n_contents.htm)
