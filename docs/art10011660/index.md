---
vid: '10011660'
title: Cumulative Normal Distributions, Black-Scholes and Error Functions
subtitle: Black-Scholes and Error Functions
authors:
- Ralph Selfridge
byline: Ralph Selfridgeselfrid@cns.ufl.edu
volume: '23'
issue: '1'
page: '136'
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
source: trad/v231/selfridge.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- mathematics and statistics
- programming techniques
---

Black-Scholes and Error Functions
{ .subtitle }

Ralph Selfridge  
[selfrid@cns.ufl.edu](mailto:selfrid@cns.ufl.edu)
{ .byline }

Recent comments in the J Forum about normal probabilities and distributions have led to a revival of interest in Black-Scholes algorithms. Black-Scholes is a set of equations that provide premiums for ‘call’ and ‘put’ options, and there has been an interest in providing appropriate algorithms in J. That, in turn, opens the question of an algorithm that will run in APL. This study was an attempt to create such an algorithm, which may already exist in some APL library but was not available to this author. We use APL2 as the APL medium. A serendipitous result is also a speed-up in computing cumulative normal distributions and an error function for APL.

We start here with Eugene McDonnell’s article “At Play with J: Beware Scholes” \[[1](ref1)\], which provides Black-Scholes equations in reasonable mathematical form and then considers some algorithms in J. This article has several algorithms created, among others, by Hu Zhe and Oleg Kobchenko. McDonnell uses the `cnd` of Zhe and his own verb `bs` for the algorithm of page 141, which we label A1, followed on page 142 by an algorithm, which we label A2, using a formula from Ewart Shaw. All algorithms, using essentially the same verb `bs`, are below in one of two appendices.

We know that A2 is much faster in execution than A1, but it has the problem that it uses `H.` (an included verb in J) not available in APL2. Thus we must create a replacement for `H.`, or go back to A1 and use the `cnd` verb from Zhe. This relies only on a verb `normalprob` from `statdist` which can be converted so that we have Black-Scholes in APL2.

However if we examine `cnd` in A1 there is the noun `__` (negative infinity), which results in a domain error if we replace `__` with a large negative number for use in APL2. That, in turn, forces examination of `normalprob` to find, and work around, `__`. With that covered we have a working Black-Scholes in APL2. Since fairly simple APL is used these algorithms should all work in any APL.

The conversion from the verb `cnd` (J) to the function `CND` (APL) allows for considerable simplification, and even further if `CND` is only for use in Black-Scholes. We make these changes, and any other improvements we can think of, and obtain `CND` and `BS` in Appendix 2. Now if we consider A2 we have a formula for the error function but that in turn allows us to come back from `CND` to get an easy APL function for the error function of a variable (it is entirely possible that such a function exists in an APL library elsewhere). We add this function to Appendix 2.

Now if we consider the changes for APL2, which shorten `CND` considerably, we can ask if similar changes can be made in `CND` of A1. These changes produce A3 in Appendix 1.

S = Current stock price  
X = Option strike price  
R = Risk-free interest rate  
T = time in years until strike date  
V = volatility, or standard deviation of asset price

All four algorithms are entered with `BS S,X,T,R,V` and return two numbers: the first is the ‘call’ premium, the second the ‘put’ premium, with an example

```
   BS 60 65 0.25 0.08 0.3
2.13337  5.84628
```

We need to compare execution times for the differing algorithms, both for BS and `CND/ERF`. In the case of J we use `(10000)6!:2'BS 60 65 0.25 0.08 0.3'`; for APL we must create a timing function that runs 10000 executions of the desired function. In the following table we set w=1.2 and ww a vector of length 500 of numbers in the range 0 to 5 `((i.500)%100)`. All results are normalized so that A2 values are 100.

|   | BS yc | CND w | CND ww | ERF w | ERF ww |
| --- | --- | --- | --- | --- | --- |
| A1 | 288 | 1490 | 1514 |   |   |
| A2 | 100 | 100 | 100 | 100 | 100 |
| A3 | 87 | 154 | 4.8 | 289 | 13.9 |
| APL | 75 | 130 | 27 | 230 | 69 |

When comparing results numeric output across the algorithms agree to several significant places. Since we must use system clocks, without knowing precisely how they run (nor do we know the resolution of the clocks), we use a repetition of 10000. Even so, timing comparisons should not be taken too seriously.

We find that A2 and A3 are substantially the same for Black-Scholes, and the benefit of avoiding `H.` is debatable. It is also interesting that APL appears faster for Black-Scholes. Both `CND` and `ERF` are faster in A2 for a scalar argument but considerably slower as the argument length grows; the fact that `BS` has an internal 2 by 2 is probably the explanation for speed-ups in A3 and APL. In the case of APL, avoiding `H.` becomes crucial, unless an `H` function is written either in APL (very slow) or some better choice, say Fortran and a DLL. There is little choice in APL, but it would appear A3 is better in J, unless the arguments for `cnd` and `erf` are scalars.

If we create a Fortran subroutine to replace `H.` specifically for Black-Scholes, and hook across to APL2 by way of a DLL the resulting timing for `BS` is slightly slower than the given APL2 version (though much faster than using a Fortran version of `H.` with the attendant use of `H"0`).

### References

1. Eugene McDonnell, “<a id="ref1"></a>[At Play with J: Beware Scholes](trad/v193/gene193.htm)” *Vector* 19.3, pp137-142

## Appendix 1

```
A1
load 'statdist'
cnd=: 3 : 'normalprob 0 1 __, y' "0      NB. Hu Zhe
BS=: 3 : 0                               NB. Eugene McDonnell
'S X T R V'=.y
d=. ((^.S%X) + T*R(+,−)−:*:V)%V*%:T
│(S,X*^−R*T)−/ .*cnd d*/1 _1
)

A2
erf=:1 H. 1.5@*: * 2p_0.5 &* %^@:*:      NB. Ewart Shaw
cnd=: −:@ >:@erf %&(%:2)
BS=: 3 : 0
'S X T R V'=.y
d=. ((^.S%X) + T*R(+,−)−:*:V)%V*%:T
│(S,X*^−R*T)−/ .*cnd d*/1 _1
)

A3  (modified A1)
cnd=: 3 : 0
b=. 0 0.31938153 _0.356563782 1.781477937 _1.821255978 1.330274429
1−│(0.398942* ^_0.5**:y)* b p. %>:y*0.2316419
)
BS=: 3 : 0
'S X T R V'=.y
d=. ((^.S%X) + T*R(+,−)−:*:V)%V*%:T
│(S,X*^−R*T)−/ .*cnd d*/1 _1
)
```

## Appendix 2

`CND` provides for cumulative normal distributions, but only for use in Black-Scholes. It can be modified for any shape argument by simple changes in line 2 (made, of necessity, for timing comparisons). `ERF` returns the error function value for its argument, and must also be modified for timing comparisons (again in line 2) or multiple arguments.

```
    ∇ Z←CND Y
[1]   Z←6 1⍴1.330274 ¯1.821256 1.78148 ¯0.356565 0.31938153 0
[2]   Z←1-|(0.398942×*¯0.5×Y*2)×2 2⍴(4 1⍴÷1+Y×0.2316419)⊥Z
    ∇

    ∇ Z←BS Y;S;X;T;R;V
[1]   (S X T R V)←Y
[2]   Z←|(S,X×*-R×T)-.×CND(((⍟S÷X)+T×R+Z,-Z←0.5×V*2)÷V×T*0.5)∘.×1 ¯1
    ∇

    ∇ Z←ERF Y
[1]   Z←6 1⍴1.330274 ¯1.821256 1.78148 ¯0.356565 0.31938153 0
[2]   Z←1-2×|(0.398942×*-Y*2)×(÷1+Y×0.3275911166)⊥Z
    ∇
```

Readers in Scotland, noted for their economy with code, might prefer the following slightly shorter version in Dyalog, using the direct-definition form implemented by John Scholes. We understand John is widely referred to north of the Border as “The Black Scholes”. *Ed.*
{ .ednote }

```
      CONSTANTS←6 1⍴1.330274 ¯1.821256 1.78148 ¯0.356565 0.31938153 0
      CND←CONSTANTS∘{1-|(0.398942×*¯0.5×⍵*2)×2 2⍴(4 1⍴÷1+⍵×0.2316419)⊥⍺}
      ERF←CONSTANTS∘{1-2×|(0.398942×*-Y*2)×(÷1+⍵×0.3275911166)⊥⍺}

    ∇ Z←BS (S X T R V)
[1]   Z←|(S,X×*-R×T)-.×CND(((⍟S÷X)+T×R+1 ¯1×0.5×V*2)÷V×T*0.5)∘.×1 ¯1
    ∇
```
