---
vid: '10501090'
title: 2^64
authors:
- Roger Hui
byline: Roger K.W. Hui
volume: '26'
issue: '1'
page: '25'
received: '2012-12-17'
online: '2013-04-10'
description: Hui explores how big is 2^64
source: content/printed/261/2x64/2x64.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- mathematics and statistics
- programming techniques
---

Roger K.W. Hui
{ .byline }

How big is `2^64`?

### Basics

```
   2^64
1.84467e19

   2^64x
18446744073709551616

   'c0.0' 8!:2 ]2^64
18,446,744,073,709,551,616
```

And, using the verb us from [\[1\]](#ref1),

```
   us 2^64x
eighteen quintillion, four hundred forty-six quadrillion,
      seven hundred forty-four trillion,
      seventy-three billion, seven hundred nine million,
      five hundred fifty-one thousand, six hundred sixteen
```

### Grains on a Chessboard

One grain of rice is placed on the first square of an 8 by 8 chessboard, two grains on the next square, four grains on the next, and so on, doubling on each square. The total is of course `(2^64)-1` grains. How deep would that amount of rice cover the earth?

Answer in [\[2\]](#ref2).

### Particles in the Universe

Is `2^64` larger than the number of particles in the universe? Not even close [\[3, 4\]](#ref3).

### Avogadro Constant

The Avogadro constant[\[5\]](#ref5) has value 6.022141e23 .

```
   6.022141e23 % 2^64
32646.1
```

That the Avogardo constant is the number of atoms in twelve grams of carbon-12 makes evident the enormity of the error of estimation in the previous section.

### Age of the Universe

The age of the universe[\[6\]](#ref6) is estimated to be about 14 billion years; its age in milliseconds is:

```
   */ 14e9 365.2425 24 60 60 1000
4.41797e20
```

### CPU Cycles

Assume the average modern PC is rated at 2 GHz. The number of CPU cycles in a year is therefore:

```
   */ 2e9 365.2425 24 60 60
6.31139e16
```

The total of CPU cycles in a year for the computers found in a residential neighborhood would exceed `2^64` . (The required number of computers is `292.277 = (2^64) % 6.31e16` ).

### Supertanker Bytes

The largest tanker ever built, the Knock Nevis[\[7\]](#ref7), has a deadweight of 564,763 tonnes (tonne = 1000 kg) and measures 1504 feet by 226 feet with a draft of 81 feet. A run-of-the-mill disk drive has a capacity of 200 GB, and `9e7` drives would have a total capacity of `2^64` bytes. Unless each drive exceeds 6 kg the tanker would be able to carry them.

Might the tanker be constrained by volume? Its volume exceeds `27532224 = */ 1504 226 81`  cubic feet which would readily accommodate `9e7` disk drives (0.3 cubic foot per drive).

We used to play a parlour game of wondering, “What’s the fastest way to send data across the Atlantic?” Adapted for the current paper and for current technology, the question we may ask is, “What is the fastest time to send `2^64` bytes across the Atlantic?” (A supertanker full of disks/flash drives/DRAMs? An A380 full of same? Transmission by a 100 Gbps submarine cable?) A rule of this game is that you must do the calculations in your head.

### Leaves on Trees

You stand on a mountain top in the North American Pacific Northwest with trees in every direction. Are there `2^64` leaves on the trees within your sight? Estimate as follows:

- you can see 100 miles[\[8\]](#ref8)in every direction
- there is a tree every 5 feet

Therefore, the number of trees within your sight is:

```
   NB. square feet within your sight
   o. *: 100 * 5280
8.75826e11

   NB. # trees within your sight
   (*:5) %~ o. *: 100 * 5280
3.5033e10

   NB. required # leaves on a tree
   (2^64) % (*:5) %~ o. *: 100 * 5280
5.26553e8
```

Is it plausible for there to be `5.27e8` leaves on a tree? There probably aren’t that many leaves on an average deciduous tree. However, trees in the Pacific Northwest are evergreen. `5.27e8` needles on an evergreen tree seem possible (`22956.5 = %: 5.27e8` ; 23 thousand branches each having 23 thousand needles).

### Compound Interest

How many years does it take to reach `2^64` dollars for $1 invested at interest rate `r` ? The equation for semi-annual compounding is:

```
(2^64) = (1+r%2)^2*y
```

Taking logarithms on both sides, we get  `y = -: (1+r%2) ^. 2^64`

```
   ] r=: 0.01 * 1+i.10
0.01 0.02 0.03 0.04 0.05 0.06 0.07 0.08 0.09 0.1

   -: (1+r%2) ^. 2^64
4447.22 2229.14 1489.78 1120.09 898.273 750.393
      644.761 565.536 503.914 454.614

   r ,. >. -: (1+r%2) ^. 2^64
0.01 4448
0.02 2230
0.03 1490
0.04 1121
0.05  899
0.06  751
0.07  645
0.08  566
0.09  504
 0.1  455
```

### Fibonacci’s Rabbits

Fibonacci studied the population growth of (idealized) rabbits where:

- in the first month there is 1 newborn pair of rabbits
- a new-born pair becomes fertile from the 2nd month on
- each month every fertile pair begets a new pair
- rabbits never die

How many months are required for the number of rabbits to reach `2^64` ?

Let `F n` be the number of pairs of rabbits after `n` months. Only the `F n-2` rabbits that are alive at `n-2` months produce a pair, and these are added to the existing population `F n-1` . Thus `(F n) = (F n-1) + (F n-2)` .  `F` is of course the Fibonacci sequence [\[9\]](#ref9).

It can be shown that `(F n) = <. 0.5 + (%:5) %~ phi^n` where phi is the golden ratio `-:1+%:5` . The equation to be solved is:

```
    (2^64) = 2 * (%:5) %~ phi^n
```

and the solution is:

```
   phi=: -:1+%:5
   phi ^. -: (%:5) * 2^64
92.4187
```

Less than 8 years.

### Factorial

The number of ways of arranging n distinct objects is `!n` . What is the smallest `n` for which this exceeds `2^64` ?

```
    !^:_1 ]2^64
20.6671
```

### Partitions

A partition of `n` is a sorted list `x` of positive integers such that `n=+/x` . For example, the following is the sorted list of all the partitions of 5:

```
┌─┬───┬───┬─────┬─────┬───────┬─────────┐
│5│4 1│3 2│3 1 1│2 2 1│2 1 1 1│1 1 1 1 1│
└─┴───┴───┴─────┴─────┴───────┴─────────┘
```

What is the smallest `n` for which the number of partitions of `n` exceeds `2^64` ?

The verb `pnv` is from [\[10\]](#ref10) where `pnv n` are the number of partitions for `i.1+n` .

```
   p=: pnv 500
   $ p
501
   5 10 $ p
    1     1     2     3     5     7     11     15     22     30
   42    56    77   101   135   176    231    297    385    490
  627   792  1002  1255  1575  1958   2436   3010   3718   4565
 5604  6842  8349 10143 12310 14883  17977  21637  26015  31185
37338 44583 53174 63261 75175 89134 105558 124754 147273 173525

   p (>i.1:) 2^64
417
   ,. 416 417{p
17873792969689876004
18987964267331664557
   2^64x
18446744073709551616
```

### Katana

To create a katana[\[11\]](#ref11) (samurai sword) a billet of steel is heated and hammered, split and folded back upon itself many times. If the number of foldings is greater than 64 then the number of layers exceeds `2^64` .

### E=m\*c^2

With total conversion, how many kilograms of mass are required to obtain 2^64 joules of energy?

```
   (2^64) % *:3e8
204.964
```

### Square Inches

What is the radius in miles of a sphere whose surface area is `2^64` square inches? The surface area of a sphere with radius r is `o.4**:r` . Thus:

```
   (*/ 12 5280) %~ %: (2^64) % o.4
19122.3
```

Such a sphere is a little larger than Uranus.

### Cubic Inches

What is the radius in miles of a sphere whose volume is `2^64` cubic inches? The volume of a sphere with radius r is `o.(4%3)*r^3` . Thus:

```
   (*/ 12 5280) %~ 3 %: (2^64) % o.4%3
25.8699
```

### Hilbert Matrix

The Hilbert matrix [\[12\]](#ref12) is a square matrix whose (i,j)-th entry is `%1+i+j` . It is famously ill-conditioned with a very small magnitude determinant.

```
   H=: % @: >: @: (+/~) @: i.

   H 5x
  1 1r2 1r3 1r4 1r5
1r2 1r3 1r4 1r5 1r6
1r3 1r4 1r5 1r6 1r7
1r4 1r5 1r6 1r7 1r8
1r5 1r6 1r7 1r8 1r9

   det=: -/ .*

   det H 5x
1r266716800000

   %. H 5x
   25   _300    1050   _1400    630
 _300   4800  _18900   26880 _12600
 1050 _18900   79380 _117600  56700
_1400  26880 _117600  179200 _88200
  630 _12600   56700  _88200  44100
```

The inverse Hilbert matrix has all integer entries, whose (integer) determinant is very large.

```
   >./ | , %. H 15x
114708987924290760000
   >./ | , %. H 14x
3521767173114190000

   % det H 7x
2067909047925770649600000
   % det H 6x
186313420339200000

   perm=: +/ .*

   perm %. H 5x
4855173934730716800000
   perm %. H 4x
5314794912000
```

The smallest inverse Hilbert matrix with an entry that exceeds `2^64` in absolute value is the one of order 15 ; with a determinant that exceeds `2^64` , order 7 ; with a permanent that exceeds `2^64` , order 5 .

### Making $$$

In U.S. dollars the units in common circulation are:

- bills: 100 50 20 10 5 1
- coins: 0.25 0.10 0.05 0.01

A dollar can be “made” in a number of ways:

```
 1.00  0.25  0.10  0.05  0.01

   0     0     0     0    100
   0     0     0     1     95
   0     0     0     2     90
     …
   0     3     2     1      0
   0     4     0     0      0
   1     0     0     0      0
```

In fact, a dollar can be made in 243 ways. What is the smallest multiple of $100 that can be made in greater than `2^64` ways?

```
h=: 4 : 0
 m=. # s=. +/\ y
 if. 2.5=x do. (m$5{.1)#m($,)+/\_2]\s else. (m$x{.1)#s end.
)

chm=: 3 : '+/ 2 h 2.5 h 2 h 5 h 4 h 2.5 h (*y)$~1+20*y' " 0
```

If n is a multiple of 100 then chm n is the number of ways of making n dollars.

```
   chm 100*>:i.3 5
4.88209e10 4.35246e12 7.62895e13 6.46316e14 3.58401e15
1.50147e16   5.149e16 1.51912e17 3.98556e17 9.51655e17
2.10326e18 4.35756e18 8.54636e18 1.59902e19 2.87178e19
   chm 1400 1500
1.59902e19 2.87178e19
```

$1500 can be made in 2.87e19 ways. The exact number is:

```
   chm 1500x
28717791430084742056
```

Suppose the more rarely circulated $2 bill and 50 cent coin are included. Then:

```
chn=: 3 : '+/ 2 h 2.5 h 2 h 2.5 h 2 h 2 h 2.5 h
      (*y)$~1+20*y' " 0

   chn 100*>:i.3 5
9.82355e12    2.78e15 9.69549e16 1.34924e18 1.10638e19
6.40915e19 2.90001e20 1.09038e21 3.54917e21 1.02915e22
2.71434e22 6.61402e22 1.50698e23 3.24114e23 6.63033e23

   chn 500 600
1.10638e19 6.40915e19

   chn 600x
64091464225604008941
```

$600 can be made in 6.41e19 ways, and is the smallest multiple of $100 than can be made in greater than `2^64` ways.

### References

1. <a id="ref1"></a>Hui, Roger K.W., Number in Words, Jwiki Essay, 2007-07-12.
2. <a id="ref2"></a>Hui, Roger K.W., Ken Iverson Quotations and Anecdotes, 2005-09-30.  [http://keiapl.org/anec/#rice](http://keiapl.org/anec/#rice)
3. <a id="ref3"></a>Bernecky, Robert, comp.lang.apl post, 1996-03-31.  [https://groups.google.com](https://groups.google.com/forum/?hl=en&fromgroups=#!search/fewer$20than$202$20power$2060$20particles$20in$20the$20universe%22$2Fcomp.lang.apl$2FOExihuC0O8k$2F-XnWggPRDIcJ/comp.lang.apl/FQqn7-Vz7vc/58wHGpyMy-YJ) search for groups or messages: “fewer than 2 power 60 particles in the universe”
4. <a id="ref4"></a>Hui, Roger K.W., comp.lang.apl post, 1996-04-01.  
    (as above)
5. <a id="ref5"></a>Avogadro constant  [http://en.wikipedia.org/wiki/Avogadro\_constant](http://en.wikipedia.org/wiki/Avogadro_constant)
6. <a id="ref6"></a>Age of the universe  [http://en.wikipedia.org/wiki/Age\_of\_universe](http://en.wikipedia.org/wiki/Age_of_universe)
7. <a id="ref7"></a>Knock Nevis  [http://en.wikipedia.org/wiki/Knock\_Nevis](http://en.wikipedia.org/wiki/Knock_Nevis)
8. <a id="ref8"></a>You can see 100 miles  [http://en.wikipedia.org/wiki/Horizon](http://en.wikipedia.org/wiki/Horizon)
9. <a id="ref9"></a>Hui, Roger K.W., Fibonacci Sequence, Jwiki Essay, 2005-09-26.
10. <a id="ref10"></a>Hui, Roger K.W., Partitions, Jwiki Essay, 2005-11-18.
11. <a id="ref11"></a>Katana  [http://en.wikipedia.org/wiki/Katana](http://en.wikipedia.org/wiki/Katana)
12. <a id="ref12"></a>Hui, Roger K.W., Hilbert Matrix, Jwiki Essay, 2005-09-29.

An earlier version of this paper appeared as a an essay in the  
Jwiki(  [www.jsoftware.com/jwiki/Essays/2^64](http://www.jsoftware.com/jwiki/Essays/2^64)) on 2007-12-06.
