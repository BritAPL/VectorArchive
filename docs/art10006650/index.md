---
vid: '10006650'
title: Volutes – a Survey of Algorithms
authors:
- Howard Peelle
byline: by Howard Peelle
volume: '14'
issue: '4'
page: '53'
source: trad/v144/peelle144_53.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1998
tags:
- programming techniques
- mathematics and statistics
---

by Howard Peelle <span class="affil"></span> <span class="email"></span>
{ .byline }

An array of numbers arranged in a spiral is known as a “volute”, named after the Latin voluta for spiral shells in nature \[0\]. For example, here is square volute of 25 consecutive integers, beginning with 0 at the centre:

```
                        12 11 10  9 24
                        13  2  1  8 23
                        14  3  0  7 22
                        15  4  5  6 21
                        16 17 18 19 20
```

A volute can spiral inward (called an “involute”) or outward (an “evolute”) and either clockwise or counterclockwise, emanating from any orthogonal direction – for a total of 16 variations in a plane. Since a corresponding involute can be produced easily from an evolute by subtraction, only evolutes will be considered here. Volutes can be readily transformed by reflection and/or rotation into other orientations, denoted here by compass directions of the first leg in each dimension. The example above is a NorthWest (NW) evolute.

Algorithms for producing volutes were coded and compared by Eugene McDonnell in his “At Play with J” column \[1\] and also by Roy Sykes in his APL “Whizbang” column \[2\]. (See Appendix for their programs.) This article offers alternative programs and introduces new algorithms – all written in J – including some which increase speed, decrease space, minimize length of code, and generalize. That is, we will reconsider how to fit numbers around and around in square whole arrays.

The programs to follow are grouped by approach:

Algebraic Approach     \[Graham, Knuth & Patashnik\]  
Rings Approach     \[Peelle\]  
Winding Approach     \[Smillie; McDonnell; Peelle\]  
Sum-Scan Approach     \[Hui\]  
Permutation Approach     \[Tuttle; Sykes\]  
Squiral Approach     \[Peelle\]  
Generalizations     \[Peelle\]

All programs (except generalizations) use a single positive integer (N) input and produce a square evolute of consecutive integers in origin 0 using a convenient orientation. Comparisons of speed and space (based on a sample input of N=55) and length are given progressively throughout, and then relative benchmarks are tabularized for several Ns at the end.

### Algebraic Approach

Graham, Knuth & Patashnik \[3 p.99 and p.512\] compute the *x* and *y* coordinates of integer *n* by the following formulas:

for m = ⌊√n⌋  
x(n) = (-1)<sup>m</sup>((n-m(m+1))•\[ ⌊2√n⌋ is even\] + ⌈½m⌉)  
y(n) = (-1)<sup>m</sup>((n-m(m+1))•\[ ⌊2√n⌋ is odd\] - ⌈½m⌉)

These can be expressed together in J as follows:

```
   m =: <.@%:
   even =: -.@odd
   odd  =:  2&|
```

```
xy =: (_1:^m)*((-m*(m+1:))*(even,.odd)@(<.@+:@%:))+(+,.-)@>.@-:@m
```

xy creates a 2-column table for integers `i.*:N` input. Then (like McDonnell’s `evGKPa` in the Appendix) we grade-up the table and reshape the resulting permutation list into an EN volute:

```
   GKPa =: ,~ $ /:@xy@i.@*:
```

This program is more efficient (about 64% in time and 66% in space) and more concise (62% in length) than `evGKPa`. It can be improved a little more by some J gymnastics:

```
   GKPA =: ,~ $ /:@XY@i.@*:
      XY =: Sign * (,. -)@Square + Leg * (,.~ -.)@Parity
       Sign =: _1: ^ m
          m =: <.@%:
       Square =: >.@-:@m
       Leg =: - (*>:)@m
       Parity =: odd@<.@+:@%:
          odd =: 2&|
```

This runs in 80% time, in the same space, and is 89% as long as my `GKPa` above. Although this closed form is mathematically appealing, it appears difficult to improve its efficiency further without altering the basic algorithm. So, let’s consider an alternative.

Graham, Knuth & Patashnik \[3\] also give a formula for the inverse, that is, to obtain n from (x,y):

```
n = (2k)2 ? (2k + x(n) + y(n))  for k = max(|x(n)|,|y(n)|)
```

where the ? sign is  
(-1)<sup>(x(n)&lt;y(n))</sup>

This can be expressed directly in J as follows:

```
   n =: *:@+:@k + sign * +:@k + +/
      k =: >./@:|
      sign =: _1: ^ </
```

Now we can use catalog (like McDonnell’s `evGKPb`, in Appendix) to generate the (x,y) coordinates in row-major order:

```
   x_y =: [: |: >@,@{@(; -)@axis
      axis =: i. - <.@-:
```

And then construct a WN volute:

```
   GKPb =: ,~ $ n@x_y
```

This runs in about 50% time and 107% space and is 89% the length of `evGKPb` (which does not handle even N input correctly). It can be improved further by using two planes of coordinates to produce a NW volute as follows:

```
   GKPB =: n @ X_Y
      X_Y =: (X ,: Y)@axis
         X =: # # ,:
         Y =: # #"1 ,.@:-
```

This is more efficient than my `GKPb` above (64% time and 89% space) and shorter (96%). And it is clearly superior to my `GKPA` (13% time, 78% space, 87% length). Nevertheless, it is expensive to compute the coordinates. So, let’s look beyond this algebraic approach.

### Rings Approach

Better yet, we can make a WN volute without computing coordinates, by using matrices of ‘rings’ as follows:

```
   HAP =: Rings @ axis
      axis =: i. - <.@-:
      Rings =: Squares + Signs * Doubles + Differences
         Squares =: *:@Doubles
         Signs =: _1: ^ Less
            Less =: </ |.@(+ One)
               One =: 2: | >:@#
         Doubles =: +:@Max
            Max =: >./~@:|
         Differences =: -/~
```

This program is much faster (9% time), smaller (47% space) and shorter (95%) than my `GKPB` above.

### Winding Approach

Keith Smillie \[4\] presented a recursive program which turns 90? and winds new legs onto the side of a matrix (see `evKS` in Appendix). My version of this for a NE (odd N) or SW (even N) volute is:

```
   KWS =: Turn @ (Right ,.~ Top , KS) @ <: ` Empty @. (=0:)
      Turn =: |."1 @ |.
      Right =: + *: + i.@>:
      Top =: *: + i.
      Empty =: 0 0&$
```

This program is far more efficient (14% time and 1% space) and shorter (75% length) than `evKS`. Nevertheless, recursion becomes impractical as N becomes large, so let’s consider iteration.

Eugene McDonnell \[1\] recoded his old APL volute program iteratively in J (see `evEEM` in Appendix). The following alternative program builds a WS (odd) or EN (even) volute faster (42%) using less space (57%) but is much longer (146%) than `evEEM`:

```
   EEM =: Build ^: (]`Empty)
      Empty =: 0 0&$
      Build =: Turn @ Bottom @ Left
         Turn =: |."1 @ |.
         Bottom =: , (+ *: + i.@>:)@#
         Left =: ,.~ (*: + i.)@#
```

Here is another program which builds the same volute – but without rotations – by inserting a gerund of verbs in decreasing-sized pairs of boxed legs:

```
   BOX =: [: > TOP`RIGHT`BOTTOM`LEFT / @ LEGS
      TOP =: ,&.>
      RIGHT =: ,.~&.>
      BOTTOM =: ,~&.>
      LEFT =: ,.&.>
      LEGS =: (CORNERS (+ i.)&.> }:)@REPS
         CORNERS =: 2: */\ |
         REPS =: ,@SIGN@(,.~ >:)@i.@-
            SIGN =: * # $ _1 1"_
```

It is faster (76%) than my `EEM` above but, alas, very space-consuming (221%) and much longer (156%).

### Sum-Scan Approach

As reported by McDonnell \[1\], Roger Hui created a WS volute by sum-scanning a matrix assembled from copies of increments (see `evHUI` in Appendix). My recoding is:

```
   HUI =: [: +/\ head , (drop~ (top,bot)@half)
      head =: i. -~ *:@(- even) - odd
         even =: 2: | >:
         odd =: 2&|
      drop =: }.~ (,-)@even
      top =: (<: ,. |.@:+: ,. ]) #"1 (1: ,. 1: + _8: * |.) ,. _1:
      bot =: (|. ,. <:@+: ,. |.) #"1 (1: ,. _5: + 8: * ]) ,. _1:
      half =: >:@i.@<.@-:
```

This is slightly faster (94%) than `evHUI`, significantly smaller (60%) and shorter (63%).

### Permutation Approach

Joey Tuttle apparently considered inverses of grade-up and sum-scan of the desired result ravelled \[1\]. His program `evJKT` (see Appendix) was the fastest in McDonnell’s comparisons.

My revised program for a NW volute is slightly faster (98%), uses slightly less space (98%), but is one token longer (103%) than `evJKT`:

```
   JKT =: ,~ $ /:@(+/\)@Increments
      Increments =: _1&|. @ (# Cycles) @ Repeats
         Repeats =: ,~ 2: # i.
         Cycles =: # $ (,-)@(,1:)@{:
```

As an alternative, Roy Sykes \[2\] employed APL indexed reassignment and other programming tricks to improve efficiency. Although Sykes reported that his program performed over 6 times faster than `evJKT` in APL+DOS, his `EVOLUTE` translated into J (see Appendix) is 2 times slower (201%) and larger (229%) than `evJKT`. This seems to reveal differences between systems and optimization of primitives.

My recoding improves on `EVOLUTE`’s speed (90%), space (82%), and length (83%) for a WS volute:

```
   RAS =: ,~ $ Indices@]`Items }~
      Items =: i.@*:@]
      Indices =: [: +/\ _1: }. Center , Repeats # Cycls
         Center =: <: * >.@-:
         Repeats =: ,~ 2: # i.
         Cycls =: >:@+: $ 1 0 _1 0"_ + 0 _1 0 1&*
```

### Squiral Approach

The following program produces a NW volute by amending positions of an initial zero matrix one at a time, much like drawing a “squiral” using Logo (see \[5\]):

```
   SQU =: 3 : 0
V =. (,~ y.) $ 0
P =. ,~ <.-:y.
I =. _1 0
N =. 1
while. N < *:y.
   do. P =. P + I
       V =. N (<P)} V
       if. N = (>. * <.) %:N
       do. I =. |.I
           if. 0 = {:I do. I =. -I end.
       end.
       N =. >:N
  end. V
)
```

This program is awfully slow (72514%) but uses less space (88%) than McDonnell’s `evJKT`.

### Generalizations

All programs presented here are already generalized for a positive integer N input – notably including both even and odd cases – in order to produce a square evolute of consecutive integers beginning with 0 in the centre (where the ‘centre’ of an even volute is one of the four central spots). Also note that all of the programs handle the degenerate case of input N=0 (whereas `evHUI` and `evJKT` do not).

Any two-dimensional integer evolute or its corresponding involute can be converted into each other by subtracting it from the sum of its largest and smallest items, using `Inv =: -~ (>./ + <./)@,` .

Generalizing volute items to any arithmetic progression is achieved simply by multiplying an integer volute (V) by a scaling factor (S) and adding a constant (A): A + S \* V. Generalizing to a geometric progression (or other functions) would be similar.

Each program here can be generalized to rectangular volutes with shape N+0 1 or N+1 0. For instance, the following program (modified slightly from my `JKT`) inputs the shape:

```
   EVR =: $ /:@(+/\)@Incs
      Incs =: _1&|.@(# Cycles)@Reps
         Reps =: >:@(+/) {. 2: # i.@>:@{:
         Cycles =: # $ (,-)@(,1:)@{:
```

For example, `EVR 5 5` produces a square NW volute (same as `JKT 5`), and `EVR 6 5` produces a rectangular NW volute with 6 rows and 5 columns. Note that a 5 by 6 NW evolute is impossible although `EVR 5 6` gives a (spurious) result. Benchmarks for this program are slightly slower but slightly slimmer than for JKT.

Volutes may be generalized further to any rectangle, as does Smillie’s `Sp` program (see Appendix). However, we will constrain volutes here to be square or rectangular with only one extra row or column, corresponding to the original squiral function \[3\].

A volute can also be generalized for orientation by using a second input to denote the directions of the first two legs. Thus (modified from `EVR` above):

```
   EVO =: ] $ /:@(+/\)@Inc
      Inc =: _1&|.@(Copy Rep)
         Rep =: >:@(+/) {. 2: # i.@>:@(<./)
         Copy =: ] # Cyc
            Cyc =: #@] $ (,~ -)@[
```

We assign pronouns for the eight different orientations:

```
nw =. _1  0 ,:  0 _1
ne =. _1  0 ,:  0  1
sw =.  1  0 ,:  0 _1
se =.  1  0 ,:  0  1
wn =.  0 _1 ,: _1  0
ws =.  0 _1 ,:  1  0
en =.  0  1 ,: _1  0
es =.  0  1 ,:  1  0
```

Then `nw EVO 5 5` produces a square NW volute. `ne EVO 6 5` is a rectangular NE 6 by 5 volute, and `wn EVO 5 6` is a WN 5 by 6, etc.

Of course, this generalization is not necessary since auxiliary programs could be defined to orient a volute result. Assuming a two-dimensional volute input in NorthWest orientation, the following programs yield all orientations:

```
   NW =: ]
   NE =: |."1
   SW =: |.
   SE =: |."1@|.
   WN =: |:
   WS =: |.@|:
   EN =: |:@|.
   ES =: |.@|:@|.
```

For example, `WN EVR 6 5` is a 5 by 6 WN evolute. Benchmarks comparing (worst case) `ES EVR 55 55` versus `es EVO 55 55` indicate that using auxiliary programs is much more favorable (23% time and 59% space).

We leave for the interested reader to write a program to orient any given volute into a given orientation, as in `es Orient EVR 5 5`.

Lastly, volutes can be generalized to higher dimensions. Here is a generalized program to produce a (D>1)-dimensional rectangular evolute which fills in a blank array by a “right hand rule” – for 3-D that is Up (1), North (1), West (1), Down (2), South (2), East (2), then Up (3), etc.:

```
   EVD =: Indexes onto Blank
      Blank =: $ _:
      onto =: i.@#@[ ` (;/@[) ` ] }
      Indexes =: Mid +"1 0: , +/\@I
         Mid =: <.@-:
         I =: _1: }. R # C
            R =: max {. # # i.@>:@(<./)
               max =: >:@(+/)
            C =: max $ (, -)@id@#
               id =: = @ i.
```

For example:

```
  EVD 3 3
2 1 8
3 0 7
4 5 6
  EVD 3 3 3
3 2  _
_ 1  _
_ _ 11
4 _  _
_ 0  _
_ _ 10
5 _  _
6 _  _
7 8  9
```

Note that this is not array-filling for D>2. Also note that rectangular evolutes are correct only when they are possible to construct, e.g., `EVD 4 3` and `EVD 4 3 3`, but not `EVD 3 4` nor `EVD 3 4 3` and `EVD 3 3 4`.

Finally, all these generalizations can be combined in the following program to produce any dimensional rectangular evolute in any orientation:

```
   EV =: indexes onto blank
      blank =: ] $ _:
      onto =: i.@#@[ ` (;/@[) ` ] }
      indexes =: mid +"1 0: , +/\@increments
         mid =: <.@-:@(- 1: = +/)~
         increments =: _1: }. reps@] # cycles
            reps =: max {. # # i.@>:@(<./)
            cycles =: max@] $ (,~ -)@[
               max =: >:@(+/)
```

For example, `nw EV 3 3` and `unw EV 3 3 3` (for `unw =. _1 0 0 , 0 _1 0 ,: 0 0 _1`) produce the examples above.

This program is, unfortunately, rather inefficient (1755% time and 897% space and 202% length) compared to my `JKT`. After all, that’s going around and around in any whole volute!

### Comparisons

The following tables compare speed, space, and length of my (fixed) programs for several Fibonacci values of input N. Benchmarks were obtained on a Pentium PC running J3.05c under Windows.

```
                                     Speed

            N      5       34        55        144        377
   Program \
      GKPA        8.20    91.74    104.55     102.12     279.14
      GKPB        2.13    11.97     13.40      12.79      16.50
      HAP         1.25     1.23      1.21       1.15       1.44
      KWS         4.22    10.11      9.71      16.85  Limit Error
      EEM         2.20     6.21      7.75      17.97     284.32
      BOX         2.30     4.61      5.92      16.85     659.87
      HUI         2.33     1.88      1.29       1          1
      JKT         1        1         1          1.01       1.48
      RAS         1.80     1.88      1.82       1.63       1.99
      SQU        29.94   430.69    705.10    2224.54   54602.52
      EV          3.42    15.08     18.95      40.39    1110.90
```

The fastest program for each N is denoted with 1, and the others show factors slower.

```
                                     Space

            N      5       34        55        144       377
   Program \
     GKPA         1.55     6.09      6.51      6.62       6.66
     GKPB         1.41     4.20      5.10      4.95       4.71
     HAP          1.05     2.24      2.38      2.41       2.41
     KWS          1        1.63      1.71      1.68   Limit Error
     EEM          1.02     2.05      3.78      4.65       4.33
     BOX          1.31     6.68     13.96     30.55      65.05
     HUI          1.37     1         1         1          1
     JKT          1.16     1.25      1.32      1.33       1.33
     RAS          1.20     1.91      1.99      2.00       2.00
     SQU          2.71     1.22      1.18      1.09       1.08
     EV           3.30     9.67     11.82     11.07      10.36
```

The program requiring the least space is denoted with 1, and the others show factors larger.

```
                                    Length

               Tokens Ratio
      Program \
        GKPA         1.66
        GKPB         1.45
        HAP          1.37
        KWS          1
        EEM          1.08
        BOX          1.68
        HUI          2.58
        JKT          1.08
        RAS          1.32
        SQU          1.97
        EV           2.18
```

The shortest program is denoted with 1, and the others show factors longer. The length of a program is its number of tokens (without unnecessary parentheses).

### Discussion

Based on the tables above, the fastest program is `HUI` after accelerating past `JKT` at about N=144. The most economical in space is `HUI`, except `KWS`, `EEM` and `HAP` for small N and `SQU` which appears increasingly competitive for large N. The shortest program here is `KWS`. But note that McDonnell’s program `evEEM` (in Appendix) reigns as most concise (even containing two unnecessary tokens).

To judge which is the best program overall, we can combine the tables – weighted equally – by averaging across the Ns for speed and space. And, now ... the winner is `JKT`! Second place goes to `HAP`, just ahead of `RAS` and `HUI`. An honorable mention goes to program `EV` for its generality. And an award might be given for elegance. This is left for the reader to judge. Which program do you like best?

### References

- \[0\] Cook, Theodore A. (1903) Spirals in Nature and Art, London: John Murray (Edinburgh Press).
- \[1\] McDonnell, Eugene E. (1996) “At Play with J – Volutes”, Vector (13) 2,pp. 144-153
- \[2\] Sykes, Roy A., Jr. (1997) “Evolution”, APL Quote-Quad (27) 3, pp. 27-31
- \[3\] Graham, Knuth & Patashnik. (1994). Concrete Mathematics (2nd edition). Reading, PA: Addison-Wesley.
- \[4\] Smillie, Keith W. (1994) “Primes, Spirals and Coffee Tables”, Vector (11) 4, pp.104-109
- \[5\] Peelle, Howard A. (1998) “Squiral Volutes in Logo and J”, International Journal of Computers for Mathematical Learning (3) 2, (to appear)
- \[6\] Peelle, Howard A. (1990) “Squiral Coordinates Problem” (EDUC 651 course material, revised March 1995) University of Massachusetts.

### Acknowledgements

During development of this article, feedback was welcomed from Chris Burke, Murray Eisenberg, Roger Hui, Eugene McDonnell, Cliff Reiter, Keith Smillie, Adrian Smith, Roy Sykes, Norman Thomson, and Joey Tuttle.

### Appendix:

Previous Volute Algorithms.

From McDonnell \[1\]:

```
GKPae  =. 0:=2&|                    NB. (GKPae y)=1 if y is even
GKPao  =. 1:=2&|                    NB. (GKPao y)=1 if y is odd
GKPaq  =. <.@+:@%:                  NB. floor double sqrt
GKPam  =. <.@%:                     NB. m is floor sqrt
GKPal  =. >.@-:@GKPam               NB. ceiling half m
GKPar  =. _1:^GKPam                 NB. parity (1 or _1)
GKPat  =. (*>:)@GKPam               NB. m*(1+m)
GPax   =. GKPar*((]-GKPat)*GKPae@GKPaq)+GKPal
GKPay  =. GKPar*((]-GKPat)*GKPao@GKPaq)-GKPal
evGKPa =. ,~ $ /:@(GKPax ,. GKPay)@i.@*:
```

```
GKPb0  =. +:@(>./"1)@:|
GKPb1  =. >@,@{@(;~)@(>.@-:@- + i.)
GKPb2  =. _1: ^ </"1
GKPb3  =. *:@[ + GKPb2@] * [ + +/"1@]
GKPb4  =. (GKPb0 GKPb3 ])@GKPb1
evGKPb =. ,~$GKPb4
```

```
QT=. [ ,"2 |.@|:@]
Wd=. [`(((#@[{.]) QT [) Wd #@[}.])@.(0:<#@])
evKS=. (i.@(0:,0:)) Wd i.@*:@]
Sp=. (i. @ ((-/ @ ]) , 0:)) Wd i.@(*/)@]
```

```
evEEM0=.|:@|. , */@$ + i.@#
evEEM1=.[:i.0 0"_[]                    NB. constant empty table
evEEM=.evEEM0^:(+:`evEEM1)
```

```
even  =: 0: = 2&|
odd   =: 1: = 2&|
line0 =: *:@(-even) - odd + i.
c3    =: 1: ,. ] ,. _1:
top=:odd}."1((|.,.+:,.|.)#"1 c3@(1&+)@(_8&*))@:>:@i.@-:@(-~>:@even)
bot=:-@even}."1((|.,.<:@+:,.|.)#"1 c3@(_5&+)@(8&*))@:>:@i.@-:@(-odd)
evHUI =: [: +/\ line0 , top , bot   NB. @: in top and bot for J3.05
```

```
evJKT =. ,~ $ /: @ (+/\) @ evJKT2
evJKT1=. <:@+: $ _1: , ] , 1: , -
evJKT0=. }:@(2: # >:@i.)
evJKT2=. _1&|.@(evJKT0 # evJKT1)
```

From Sykes \[2\], translated into J:

```
   EVOLUTE =: 3 : 0
Z =. i.A=.y.*y.
B =. (2!y.) + */0 2#:y.
Z =. Z (+/\A{.B,(2#>:i.y.)#(+:y.) $ _1 0 1 0 + 0 1 0 _1 * y.) }Z
(2#y.)$Z
)
```

Howard A. Peelle  
University of Massachusetts  
Amherst, MA 01003, USA

(webpage generated: 18 October 2006, 02:56)
