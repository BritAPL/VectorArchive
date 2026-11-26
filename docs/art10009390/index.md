---
vid: '10009390'
title: Multiprecision Arithmetic – Part I
authors:
- John Sullivan
byline: by John Sullivan
volume: '12'
issue: '1'
page: '74'
source: trad/v121/sullivan121_74.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1995
tags:
- mathematics and statistics
- programming techniques
---

by John Sullivan <span class="affil"></span> <span class="email"></span>
{ .byline }

### Introduction

When I was a lot younger I became interested in prime numbers. Not, you will understand, run-of-the-mill primes like 2, 3, 5, 7 or even 2357, but decent primes that you can get your teeth into, such as 391581×2<sup>216193</sup>−1, which was discovered in August 1989\[1\]. Later, my interest expanded to the repunits (numbers consisting entirely of 1s) and their factors. These numbers are of the form (10<sup>n</sup>−1)/9, which is similar to the form of the Mersenne numbers, (2<sup>n</sup>−1)/1, which can be considered to be repunits in radix 2. It was but a short step from these to what I have the temerity to call the *generalized repunit* which has the form (*a<sup>n</sup>− b<sup>n</sup>*)/(*a − b*). In order to study the factors of these numbers I needed some way to perform arithmetic operations on large numbers quickly, as pencil and paper long division soon became tiresome, and my pocket calculator could only handle small numbers.

I hate programming. I do it because I have to (it provides the daily crust), but like most lazy people I do as little as necessary to do the job in hand. The wonderful thing about APL is that you do not have to do all those nasty things that are necessary with Fortran or C, like writing nice tidy routines to store your data in a file where you can get at it again for future use. In APL you can store all of your junk in the workspace. You can key in a long line of instructions at the six-blank prompt and let APL do the parsing for you, leaving you to write the underlying functions. What I am trying to say is I don’t like writing things in Fortran or C, where you have to finish the whole shebang before you can start testing it, whereas with APL you can write bits now and add more later, with very little extra effort at the start, because the APL workspace concept handles the fiddly bits for you.

So, when I came to start writing my multiprecision suite, the obvious choice of language was APL. To be sure, I’ll recode it in C when it’s eventually finished, except that by that time I’ll probably be too busy with other things, like playing the harp and singing in the choir.

Laziness has also meant that I have drawn on several source materials in writing this suite. Firstly, I suppose, I got the idea from IBM, who published their own version of a multiprecision suite in 1977\[2\]. Pretty nice it looked, until you tried to do anything with it, and then you discovered the bugs! There is a whole section on multiple-precision arithmetic in Donald Knuth’s Art of Computer Programming\[3\], which explains the algorithms very well. Hans Riesel\[4\] has “A Complete Package for Multiple-Precision Arithmetic” written in Pascal, which unfortunately contains a number of errors, but gives a lot of good algorithms for primality testing and factorization, as does Paulo Ribenboim\[5\]. Finally, a good source of information on repunits is Samuel Yates\[6\].

In this document the following abbreviations and terms are used:

*Maxint*     1+the maximum integer value the computer will handle. For 4-byte integers this is 2147483648.

scalar     A number which is represented by an ordinary integer, as opposed to a multiprecision number. This is allowed by all the functions as long as it is not too big.

### Preliminaries

Multiprecision numbers can be considered to be objects in a class, so it seemed natural to make use of the Namespace facility in Dyalog APL, where the gory details of some of the functions could be hidden from immediate view. This also allows me to set the index-origin in the namespace to zero, and allow the user to have any other index origin in the root namespace. The multiprecision namespace is called mp and is set up as follows:

```
      )ns mp
#.mp
      )cs mp
#.mp
      ⎕io←0
```

Of course, you don’t have to do this if you don’t want to. Although these functions are written in Dyalog APL, I have tried to write them in such a way that they will work with any APL, although you may need to make some minor changes, especially with `⎕SIGNAL`.

### Internal Representation of a Number

Each number is represented as a vector of numbers, preceded by a number, called the *exponent*, which defines the position of the radix-point with respect to the right-hand end of the number. A zero exponent indicates an integer, a positive exponent implies that number of zeros catenated at the end of the number, and a negative exponent implies that the radix point is moved to the left that number of places. It’s a bit like E-notation (e.g. 1E4 is really 10000).

All non-exponent items of the number have the same sign. Leading zeros are eliminated.

### The Radix

Normal arithmetic performed mentally, or with a pencil and paper, is done in radix 10. In a computer environment, however, radix-10 arithmetic is somewhat inefficient, and it is possible to do much better. What are the considerations for determining the optimal radix for the multiprecision suite?

The radix must be a positive integer. It is too complicated to use negative, imaginary or complex radices.

For addition the radix must be less than or equal to half of *Maxint* and for multiplication it must be less than or equal to the square root of *Maxint*, in order to avoid conversions to floating point numbers in the calculations.

The radix for input and output of numbers must be a power of 10. However, the exponentiation algorithm requires the binary representation of the exponent, and so a radix change to a power of 2 has to be made at some time during exponentiation. In all other cases the radix is immaterial, since the functions will work with any radix, but a power of 10 allows easy inspection of intermediate results when debugging.

When deciding which radix to use a balance is struck between the extra processing required and the benefits. For example, with a large radix (greater than the square root of *Maxint*) a radix-change to a smaller one is made when multiplying numbers, but the numbers will be stored in smaller-sized arrays, requiring less storage and less processing in the addition and division functions. With a small radix there is no need to change radix when multiplying, but numbers require more storage and more processing for addition and division.

In this suite there are two radix variables, which are used as follows. The main radix variable is called `base` and it must less than *Maxint* and it must be a square if it is not equal to the other variable. The other variable is called `bsqr`, and it is either the square root of `base` (in which case a radix change is made in the multiplication routine) or it is equal to `base` (so a radix change is not made in the multiplication function). In all cases `bsqr` must be less than the square root of *Maxint*. It is an error if `base` and `bsqr` are not as described here, and the results of your processing will be unpredictable.

In the examples provided in this series of articles, unless stated to the contrary the variable `base` is set to 10<sup>8</sup> and `bsqr` is 10<sup>4</sup>.

The function to change the radix of a number is as follows. The left argument is a 2-element vector containing the new radix and the existing radix in that order. The right argument is the number in the existing radix and the result is the number in the new radix. Obviously (line 2) we do nothing if the radix does not change.

```
     ∇ z←b chbase z;base;bsqr
[1]    'Invalid radix specification'⎕SIGNAL(2≠⍴b)/8
[2]    →(=/b)/0
```

If the new `base` is less than 32768 then we can make `base` and `bsqr` the same

```
[3]    →(b[0]≥32768)/∆1
[4]    bsqr←base←⌊b[0]
[5]    →∆2
```

Otherwise the new radix must be a square. In the process we generate the square root of the radix, and make absolutely sure that this square root is stored as an integer

```
[6] ∆1:'Radix not a square'⎕SIGNAL(0≠1|bsqr←(base←b[0]) *0.5)/8
[7]  bsqr←⌊bsqr
```

Having made certain our radix variables are correct, we now change the radix of the number.

```
[8] ∆2:z←b[1]frombase z
     ∇
```

Radix conversion is done by a separate function. If the radix is unchanged, we exit with no change to the number.

```
     ∇ z←b frombase y;a;f;q
[1]    →(b≠base)/∆1
[2]    z←y ⋄ →0
```

Set the result variable to zero, then express the old radix in terms of the new radix

```
[3]   ∆1:z←0 0
[4]    →(base≥b)/∆2
[5]    b←(0,base)⊤b
[6]   ∆2:b←0,b
```

If we have a floating-point number we will not always be able to give an exact representation in the new radix. However, we can always represent integers exactly in the new radix. Therefore we multiply appropriately in order to make our number an integer, carefully noting the amount we multiplied by (stored in `f` as a power of the old radix):

```
[7]    f←0⌈-y[0] ⋄ y[0]←0⌈y[0]
```

Pad the number with trailing blanks if necessary, and drop the radix-point indicator:

```
[8]    y←1↓fullint y
```

If the next digit is greater than the new radix then split it (equivalent to 45<sub>100</sub> = 4<sub>10</sub>5<sub>10</sub>):

```
[9]   ∆4:→(base≥q←y[0])/∆5
[10]   q←,(0,base)⊤q
```

Multiply the existing result by the radix and add the new digit. If we have any more left then repeat the process:

```
[11]  ∆5:z←(0,q)Fadd b Fmul z
[12]   →(0≠⍴y←1↓y)/∆4
```

If we started with a fraction then we must return a fraction, so divide by the appropriate power of the original radix. Notice that we may lose data here.

```
[13]   →(f=0)/0
[14]   z←z Fdiv b Fspow f
     ∇
```

The function `fullint` converts integers which have had their trailing zeros removed to “full” integers including their trailing zeros. It is also used in other functions, such as the division function.

```
     ∇ z←fullint z
[1]    →(0>1↑z)/0
[3]    z←0,(1↓z),(1↑z)⍴0
     ∇
```

### The Basic Functions

The basic functions are Add, Subtract, Multiply, Divide and Scalar power. There are two functions for Divide: one for an integer result with remainder, and one for a floating-point result which is rounded to the appropriate precision. All function names start with F if they can be used for floating-point operations, and I if they are for integers only.

Before describing the functions I should say that everywhere a multiprecision number is acceptable so also is a scalar. However, we must first convert these scalars to multiprecision numbers, and the following function does just that.

```
     ∇ a←scalar a
[1]    a←((1=⍴a)/0),a←,a
     ∇
```

### Add

The Add function is simple:

```
    ∇ z←a Fadd b;d;m;s
[1]   a←scalar a ⋄ b←scalar b
```

Make the two numbers align on the right by adding trailing zeros if necessary. Save the position of the radix point, which is the lower of the two original positions. We can then drop the radix point position, and add leading zeros to make both numbers the same length (implied only, by variable `m`):

```
[2]   a←a,(0⌈a[0]-b[0])⍴0
[3]   b←b,(0⌈b[0]-a[0])⍴0
[4]   d←a[0]⌊b[0]
[5]   a←1↓a ⋄ b←1↓b ⋄ m←-(⍴a)⌈⍴b
```

Add the two numbers together, making a note of the sign of the most significant part of the result. If the result is 0 then exit here.

```
[6]   →(0≠s←×1↑(z≠0)/z←(m↑a)+m↑b)/∆1
[7]   z←0 0 ⋄ →0
```

Tidy up the result. Carrying is dealt with in lines 9 and 10, leading zeros are dropped in 11, and all is put together in 12.

```
[8]  ∆1:z←s×z
[9]  ∆2:→(0^.=(z←(0,base)⊤z)[0;])/∆3
[10]  z←(z[0;],0)+0,z[1;] ⋄ →∆2
[11] ∆3:z←s×z[1;] ⋄ z←((z≠0)⍳1)↓z
[12]  z←(d+m),(-m←(0≠⌽z)⍳1)↓z
     ∇
```

### Subtract

This is even easier. We just multiply the right argument by −1 and call `Fadd`.

```
     ∇ z←a Fsub b
[1]   z←a Fadd(1↑b),-1↓b←scalar b
     ∇
```

There are two boolean comparison functions that make use of `Fsub`. `Fequal` is the multiprecision equivalent of `=`

```
     ∇ z←x Fequal y
[1]    z←0^.=x Fsub y
     ∇
```

`Fgt` is the multiprecision ordering function. We only need one function, because, given variables X and Y:

For `X>Y` use `X Fgt Y`  
For `X<Y` use `Y Fgt X`  
For `X≤Y` use `~X Fgt Y`  
For `X≥Y` use `~Y Fgt X`

```
     ∇ z←x Fgt y
[1]    z←0∨.>1↓y Fsub x
     ∇
```

### Multiply

Start by storing the number of radix places and the signs of the arguments, then calculate the sign of the result, exiting quickly if the result is zero.

```
     ∇ z←a Fmul b;da;db;noconv;s;sa;sb
[1]    a←scalar a ⋄ b←scalar b
[2]    da←a[0] ⋄ db←b[0]
[3]    sa←×1↑a←((a≠0)⍳1)↓a←1↓a
[4]    sb←×1↑b←((b≠0)⍳1)↓b←1↓b
[5]    →(0≠s←sa×sb)/∆1
[6]    z←0 0 ⋄ →0
```

We work on positive numbers only. Because of the way the algorithm is written it is better that `a` should be the larger of the two numbers, so swap them if necessary.

```
[7]   ∆1:a←|a ⋄ b←|b
[8]    →((⍴b)≥⍴a)/∆2
[9]    a b←b a
```

Because of the danger of overflowing and converting into floating-point numbers the arguments are converted to a smaller radix here (unless, of course, we have specifically requested the function not to do so by making the two radix variables equal). We use variable `noconv` later when we want to convert back again.

```
[10]  ∆2:→(noconv←base=bsqr)/∆3
[11]   a←,⍉(0,bsqr)⊤a
[12]   b←,⍉(0,bsqr)⊤b
```

Calculate the raw result, and refine it by carrying. We cannot avoid a loop here: in radix 10, if we add 1 to 499999 we have 5 carries, one on each iteration of the loop. The same applies here.

```
[13]  ∆3:z←+⌿(-1+⍳1↑⍴z)⌽z,(2⍴1↑⍴z←a∘.×b)⍴0 ⍝ Raw result
[14]  ∆4:→((a←(0,bsqr)⊤z)[0;]^.=0)/∆5
[15]   z←(a[0;],0)+0,a[1;]
[16]   →∆4
```

Convert back from the smaller to the larger radix if necessary:

```
[17]  ∆5:→noconv/∆6
[18]   z←((2|⍴z)⍴0),z
[19]   z←(0,bsqr)⊥⍉((0.5×⍴z),2)⍴z
```

Apply the sign to the result, drop leading zeros, set the number of radix places, drop trailing zeros and that’s it.

```
[20]  ∆6:z←s×((z≠0)⍳1)↓z
[21]   da←da+db
[22]   z←(-db←(0≠⌽z)⍳1)↓z
[23]   z←(da+db),z
     ∇
```

### Divide

This function is complicated because of the way long division is performed. The result of the integer division function is a nested vector of two numbers, the quotient and the remainder. The floating-point version rounds the result to the appropriate precision. The integer division function gives a domain error if floating-point input is supplied.

First the integer division function. It is a domain error if either the divisor or dividend is non-integer, or if the divisor is zero. If the dividend is zero we can quit early and avoid some processing.

```
     ∇ c←a Idiv b;af;bf;j;q;qf;q1;r;r1;s;t
[1]   a←scalar a ⋄ b←scalar b
[2]    ⎕SIGNAL(0∨.>a[0],b[0])/11
[3]    ⎕SIGNAL(0^.=1↓b)/11
[4]    →(0∨.≠1↓a)/∆1
[5]    c←(0 0)(0 0) ⋄ →0
```

Calculate the sign of the result, and work from now on in positive numbers. Make sure we have trailing zeros if necessary:

```
[6]   ∆1:s←1 ¯1[(a∨.<0)≠b∨.<0]
[7]    a←fullint|a ⋄ b←fullint|b
[8]    →(2≠⍴b)/∆3
```

There is no point in using the full algorithm if the divisor is small (i.e. less than the radix), so we divide into two parts here. This is the small divisor part:

```
[9]    →c←(2×(b←1↓b)=1↑r←0,1↑a)⍴0
[10]  ∆2:c←c,q←⌊(t←base⊥r,1↑a)÷b
[11]   r←0,t-q×b ⋄ →(0<⍴a←1↓a)/∆2
[12]   c←c Fadd 0 ⋄ →∆99
```

And this is the path for multiprecision divisors. Start by creating the remainder, and make a floating-point divisor from the first two digits of the divisor.

```
[13]  ∆3:r←a ⋄ c←0 0
[14]   bf←b[1]+b[2]÷base
```

Loop here. If `b>r` then we are done, otherwise we make a trial division to find the next part of the result. `qf` is the provisional quotient (line 19), which is refined in line 21 if it equals 1. The algorithm here is quite complicated, see Riesel\[4\], p.338 for more details.

```
[15]  ∆4:→(b Fgt r)/∆99
[16]  ∆5:af←r[1]
[17]   →(2≥⍴r)/∆6
[18]   af←af+r[2]÷base
[19]  ∆6:q←⌊qf←af÷bf ⋄ j←0
[20]   →(1≠qf)/∆7
[21]   q←r Fgt(⍴r)↑b
[22]  ∆7:→(0≠q)/∆8
[23]   q←⌊qf←qf×base ⋄ j←1
[24]  ∆8:r1←r Fsub b Fmul q1←(2+(⍴r)-j+⍴b)↑q1←0,⌊q
[25]   →(~0 Fgt r1)/∆9
[26]   q←q-1 ⋄ →∆8
[27]  ∆9:c←c Fadd q1 ⋄ r←0,(1↓r1),r1[0]⍴0
```

It may be that our truncation of the provisional quotient has resulted in `r=b`, so

```
[28]   →(~b Fequal r)/∆10
[29]   r←0 0 ⋄ c←c Fadd 1
[30]  ∆10:→∆4
```

And here we tidy up, and return the quotient and remainder:

```
[31]  ∆99:c←(c×1,(¯1+⍴c)⍴s)(r×1,(¯1+⍴r)⍴s)
     ∇
```

The floating-point division function plays around with our numbers to make integers of them, calls the integer version, then tidies up:

```
     ∇ a←a Fdiv b;d;r
[1]    a←scalar a ⋄ b←scalar b
[2]    ⎕SIGNAL(0^.=1↓b)/11
[3]    →(0^.=1↓a)/0
```

Increase the precision of the dividend by the number of digits in the divisor:

```
[4]    a←(a[0]+1-⍴b),1↓a,(¯1+⍴b)⍴0
```

Pretend that our numbers are integers, and perform the integer division:

```
[5]    d←a[0],b[0]
[6]    a←0,1↓a ⋄ b←0,1↓b
[7]    a r←a Idiv b
```

If the remainder is greater than or equal to half the divisor add 1 to the quotient. Maintain the precision of the quotient by padding with unnecessary trailing zeros, finally positioning the radix point in the correct place.

```
[8]    →(b Fgt r Fmul 0 2)/∆1
[9]    a←a Fadd 0 1
[10]  ∆1:a←fullint a
[11]   a[0]←-/d
     ∇
```

### Raising to a scalar power

In this section, I will call the number *b* in the expression *a<sup>b</sup>* the power, rather than the exponent, to avoid confusion with the exponent of the multiprecision number.

The algorithm for this little gem comes from Ribenboim\[5\], p.38. The function raises a multiprecision number to the power of another number which must not be too big (for workspace considerations). We allow a small multiprecision number for the power, as well as a scalar. Anything larger than the radix will trigger a domain error, as will a negative power.

```
      ∇ z←m Fspow x;i;n;q;rem
[1]    →(2≠⍴,x)/∆1
[2]    →(0≠1↑x)/∆1
[3]    x←x[1]
[4]   ∆1:⎕SIGNAL((^/x∊⎕AV)∨1≠⍴,x)/11
[5]    ⎕SIGNAL(x≤0)/11
```

Write the power in radix 2. Start with `z`=1, and at the left of the binary representation of the power. Square `z`. If the current bit in the power is 1 we also multiply `z` by the number `m`. Proceed this way until the right of the power is reached. Of course, the first bit in the power is always 1, so we can cut some processing, and we get the following:

```
[6]    z←m
[7]    →(1=⍴n←((⌈2⍟1+x)⍴2)⊤x)/0
[8]    i←1
[9]   ∆2:z←z Fmul z
[10]   →(~n[i])/∆3
[11]   z←z Fmul m
[12]  ∆3:→((⍴n)>i←i+1)/∆2
     ∇
```

### And there’s more ...

If you have got this far you might like to try the following, assuming that your APL will let you do it. It is based on the well-known matrix multiplication way of generating the Fibonacci sequence, and it illustrates the use of the inner product operator with two user-defined functions.

```
      Fadd.Fmul\4⍴⊂2 2⍴1 1 1 2
```

Of course, it doesn’t run very fast (this can run faster if you write your own scan operator), and the output looks a bit strange, with numbers all over the place, but I will show how to speed and tidy this up in future articles.

In Part 2 I shall show the input and output routines that will enable you to make some sense of the numbers you get out of these functions. I shall also show you how to extract the square root of a random 100-digit number in less time than it takes to key the number in.

### References

1. Geoffrey V Wood, *Large Primes* (Dept. of Mathematics & Computer Science, University College of Swansea, 1988, later amended).
2. *The APL Handbook of Techniques* (IBM DP Scientific Marketing, 1988. Order No. S320−5996−0).
3. Donald E Knuth, *The Art of Computer Programming*, vol. 2: seminumerical algorithms (Addison-Wesley, 1989), Section 4.3.
4. Hans Riesel, *Prime numbers and computer methods for factorization* (Birkhäuser, 1985).
5. Paulo Ribenboim, *The Book of Prime Number Records* (Springer, 1988).
6. Samuel Yates, *Repunits and Repetends* (Star Publishing, Florida, 1982).

(webpage generated: 9 November 2006, 23:59)
