---
vid: '10009400'
title: Multiprecision Arithmetic – Part II
authors:
- John Sullivan
byline: by John Sullivan
volume: '12'
issue: '2'
page: '76'
source: trad/v122/sullivan122_76.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1995
tags:
- mathematics and statistics
- programming techniques
---

by John Sullivan <span class="affil"></span> <span class="email"></span>
{ .byline }

### The Story so Far

In Part I, I explained my motivation for writing a multiprecision arithmetic suite in APL, and I gave the code for the addition, subtraction, multiplication and division functions, together with some other goodies, such as changing the radix of a number and raising a multiprecision number to a small power. I also showed the matrix multiplication way of generating Fibonacci numbers using the inner product operator with two user-defined functions.

Now read on ...

### Input and Output

If we set our radix to 10<sup>8</sup> and we multiply 0 11 by 0 9090910 we get 0 1 10, which is pretty meaningless. We need a function to convert our output to a suitable format so that we can read it. Similarly, we need an input function, because it is a lot easier to input large numbers in character format, with blanks or commas to group the digits in suitable blocks, than it is to input them directly as integers.

The formatting function is called `Ffmt` (Float format). It takes a scalar left argument, which indicates the size of the blocks into which the number is separated. If this is positive the separator is a blank, if negative it is a comma. For example:

```
      ¯3 Ffmt ¯1 12345 67890000
12,345.678,9
      3 Ffmt ¯1 12345 67890000
12 345.678 9
      0 Ffmt ¯1 12345 67890000
12345.6789
```

Since we are preparing human-readable output, the radix of our operations should be a power of 10. For the purposes of this function I am going to change the radix to 10<sup>8</sup> if the radix is not already a power of 10.

```
     ∇ z←{h}Ffmt a;b;x;k;n;p;s
[1]    →(0=1|p←10⍟base)/∆1
[2]    a←(100000000,base)chbase a ⋄ p←8
[3]   ∆1:p←⌊p
```

We now strip off the number of radix places, and drop trailing zeros.

```
[4]    n←a[0] ⋄ a←1↓a
[5]    a←(-s←(0≠⌽a)⍳1)↓a ⋄ n+←s
```

But, if we have an integer with a positive exponent, we put the trailing zeros back, or add new zeros if there weren’t any.

```
[6]    →(n≤0)/∆2
[7]    a,←n⍴0 ⋄ n←0
```

Get the sign of the number. If it is zero, then make a quick dash for the exit.

```
[8]   ∆2:→(0≠s←×1↑a←((a≠0)⍳1)↓a)/∆3
[9]    z←'0' ⋄ →0
```

Having got the sign we now work with the absolute value of the number. If the number is fractional and we need leading zeros then supply them.

```
[10]  ∆3:a←|a
[11]   →(n≥0)/∆4
[12]   a←(n⌊-⍴a)↑a
```

Format each part of the number into blank-separated blocks of numeric characters with leading zeros. Each block is *p* characters long, where *p* is log<sub>10</sub> of the radix. If we have a fraction then put the decimal point in the appropriate place (this is made easy by the blanks between the blocks of numbers.

```
[13]  ∆4:z←,' ',('ZI',⌷p)⎕FMT a
[14]   →(n≥0)/∆5
[15]   z[(⍴z)+n×p+1]←'.'
```

Tidy up. If our number is not completely fractional drop the leading blank (line 16). If the number starts with one or more zeros, drop them (line 17), and replace one zero if this leaves the decimal point in the first character (line 18). If the number is negative put a minus sign at the start (line 19). If we have a decimal point then drop any trailing zeros (lines 20 and 21).

```
[16]  ∆5:z←(' '=z[0])↓z
[17]   z←((z≠'0')⍳1)↓z
[18]   z←(('.'=z[0])/'0'),z
[19]   z←((s<1)/'-'),z
[20]   →(~'.'∊z)/∆6
[21]   z←(-('0'≠⌽z)⍳1)↓z
```

If no left argument (`h`) was supplied, or if its value would result in no change to the appearance of the number as we have it now, then we have finished. If `h` is minus the size of the radix then we can bypass the code that puts the blanks into the right places.

```
[22]  ∆6:→(0=⎕NC'h')/0
[23]   →(h=p×1 ¯1)/0,∆8
```

Get rid of all blanks. If `h` is 0 then we have finished. The base from where the separators (blanks or commas) are measured is the decimal point, so find that. Calculate a boolean vector that can be used to put blanks in the number: line 27 does it before the decimal point and line 29 after. Line 28 handles the case where we have an integer with no decimal point.

```
[24]   z~←' '
[25]   →(0=k←|h)/0
[26]   n←z⍳'.'
[27]   s←⌽(⌊n×b←(k+1)÷k)⍴c←(k⍴1),0
[28]   →(n=⍴z)/∆7
[29]   s←s,1,(⌊((⍴z)-n+1)×b)⍴c
[30]  ∆7:z←s\z
```

Drop the resulting leading and trailing blanks. If there is a blank between the minus sign and the number then remove that also.

```
[31]   z←(' '=z[0])↓z
[32]   z←(-' '=¯1↑z)↓z
[33]   →('- '∨.≠2↑z)/∆8
[34]   z←'-',2↓z
```

If `h` was negative then we want comma separators, and that’s it.

```
[35]  ∆8:→(h≥0)/0
[36]   z[(z=' ')/⍳⍴z]←','
     ∇
```

The input function is called `Fexec` (Float execute). If the number is already numeric we get out quickly. The same comments about the radix for `Ffmt` apply to this function also. Be careful here with the terminology: we have two meanings of the word *exponent*: as the number that comes after the E in a number like 1.234E5, and as the number defining the position of the radix point in the multiprecision array. I have tried to avoid confusion, but I have probably failed!

```
     ∇ x←Fexec x;e;f;i;n;p;p1;p10;q;s
[1]    →(~(1↑x)∊⎕AV)/0
[2]    →(p10←0=1|p←10⍟base)/∆1
[3]    p←8
[4]   ∆1:p1←(p+1)÷p←⌊p
```

It is sometimes convenient to enter numbers in scientific notation, i.`e`. with an E to indicate multiplication by some power of 10. We handle that here. It is an error if there is more than one E (lower-case is also acceptable) in the number. Split the exponent off, drop any leading plus-sign, check that the exponent is all numbers with an optional leading minus, make it numeric (this function will crash here if the exponent is just a minus sign: this is deliberate, to allow the calling function to handle the error). The value of the exponent is stored in `e`, which will be 0 if there was no exponent. (Note: if your APL does not have `⎕D` you can replace it by `'0123456789'`).

```
[5]    →(0=e←+/n←x∊'Ee')/∆2
[6]    ⎕SIGNAL(1≠e)/11
[7]    q←n⍳1
[8]    e←(q+1)↓x ⋄ x←q↑x
[9]    e←('+'=1↑e)↓e
[10]   ⎕SIGNAL(~^/(e∊⎕D)∨(⍴e)↑(1↑e)∊'¯-')/11
[11]   e←⍎e
```

Now we basically repeat on the main part of the number what we did to the exponent, but we also have to check on the decimal point if it is present (there must not be more than one of these), and we split the number into two at the decimal point. In this part of the function `s` is the sign of the number, `i` is the integer part and `f` is the fractional part.

```
[12]  ∆2:x←(x∊⎕D,'.-¯')/x
[13]   x←(s←x[0]∊'-¯')↓x
[14]   →(0=⍴x)/0
[15]   i←(q←x⍳'.')↑x
[16]   →(0=⍴f←(q+1)↓x)/∆3
[17]   ⎕SIGNAL(f∨.='.')/11
```

Set up the result (null vector). If there is an integer part, convert it to numerics by spacing it out according to the size of the radix, catenate this to the result. Do the same with the fractional part, which may have to have added trailing zeros in order to ensure that the last block is the correct length.

```
[18]  ∆3:x←∨
[19]   →(0=⍴i)/∆4
[20]   x←x,⍎(⌽(⌊p1×⍴i)⍴(p+1)↑p⍴1)\i
[21]  ∆4:→(0≠⍴f)/∆5
[22]   f←∨ ⋄ →∆6
[23]  ∆5:f←,⍎((p1×⍴f)⍴(p+1)↑p⍴1)\f←f,(p|-⍴f)⍴'0'
```

Put the number all together, multiply all but the first element (which is the location of the radix point) by the sign of the number. Drop leading and trailing zeros.

```
[24]  ∆6:→(0^.=x←(-⍴f),1 ¯1[s]×x,f)/0
[25]   x←(-f←(0≠⌽x)⍳1)↓x
[26]   f←x[0]+f
[27]   x←f,((x≠0)⍳1)↓x←1↓x
```

If the radix we used in this function is not the same as the global radix then recalculate the number in the global radix.

```
[28]   →p10/∆7
[29]   x←(base,100000000)chbase x
```

Process the exponent if it is non-zero. This is done the lazy way, by generating a character number of the appropriate size and recursively calling this function. (This is the reason why this function is in part 2: we needed to define `Fmul` first.)

```
[30]  ∆7:→(1+×e)⊃∆8,0,∆9
[31]  ∆8:x←x Fmul Fexec'0.',((¯1-e)⍴'0'),'1' ⋄ →∆9
[32]  ∆9:x←x Fmul Fexec'1',e⍴'0'
     ∇
```

### Square Roots

There are several algorithms for extracting square roots, but they all seem to have disadvantages somewhere along the line. I had to make a choice of algorithm here, and the one I have chosen seems to be the fastest function around, but it uses multiprecision floating-point numbers and we have to be careful to prevent the intermediate results blowing up and giving `WS FULL`. IBM\[3\] gives an algorithm using long division that is only slightly slower than this version, but it only works with even radices (because at one point the radix is divided by 2), whereas this version works with all radices as defined in part 1. For yet another algorithm, which is slower than these two, see \[2\].

As with division there are two functions for square roots, one for integer square roots with remainder, and the other for floating square roots, rounded to the nearest value. Again, as with division, the floating function calls the integer function and massages the results.

### Isqrt (Integer square root)

First we make sure our input is multiprecision, then we check that it is not negative. It’s also an error in this function if the input is not an integer.

```
     ∇ z←Isqrt a;b;f;n;q;r
[1]    a←scalar a
[2]    ⎕SIGNAL(0∨.>a)/11
```

If our number is 0 then we can exit at once.

```
[3]    →(0∨.≠1↓a)/∆1
[4]    z←(0 0)(0 0) ⋄ →0
```

Use the full precision of the number.

```
[5]   ∆1:a←fullint a
```

If the square is small we can use APL’s built-in exponentiation primitive, rounding the result, then go to the end for processing of the remainder.

```
[6]    →(3<⍴a)/∆2
[7]    z←0,⌊(base⊥a)*0.5 ⋄ →∆8
```

First, we get an accurate starting value. Rather than just looking at the first pair of “digits”, we try to take as much as we can of the number for the starting value. For example, in base 100, if we try to extract the square root of (10 73 74 18 24)<sub>100</sub>, our starting value is (3 27 68)<sub>100</sub>, rather than the 3<sub>100</sub> we would get from just looking at 10<sub>100</sub>. If we decide to work in base 10 (heaven forbid!) the starting value is the same, i.`e`. (3 2 7 6 8)<sub>10</sub>, which is still a lot better than the 3<sub>10</sub> from 1<sub>10</sub>0<sub>10</sub>. And in both these cases the starting value is the square root, so we can exit straight away having saved ourselves a lot of unnecessary effort. The 0 `Fadd` on line 9 is the quickest way to convert the scalar starting value to a multiprecision number.

```
[8]   ∆2:n←(⍴a)⌊(2|⍴a)+2×1⌈⌊8÷10⍟base
[9]    z←(n←1+⌊0.5×⍴a)↑0 Fadd⌊(base⊥n↑a)*0.5
```

Have we hit upon the square root straight away?

```
[10]   →(0 0≡r←a Fsub z Fmul z)/∆9
```

Now we have the usual Newton-Raphson iteration: *`z`<sub>n+1</sub> = `z`<sub>n</sub> +* (<i>a - <tt class="aplu">f</tt></i>(*`z`<sub>n</sub>*))/<i><tt class="aplu">f</tt>′</i>(*`z`<sub>n</sub>*)

```
[11]  ∆3:b←z Fadd(a Fsub z Fmul z)Fdiv z Fmul 2
```

If we have more than 2 radix places we should ignore the extra (because they are not really needed, they slow down the algorithm, and they can give rise to `WS FULL` as we iterate).

```
[12]   ⍎(b[0]<¯2)/'b←¯2,1↓(2+b[0])↓b'
```

If the integer part of this iteration equals the integer part of the previous iteration we have finished, otherwise iterate again.

```
[13]   →((floor b)Fequal floor z)/∆4
[14]   z←b ⋄ →∆3
```

Drop the fractional part of the result, calculate the square-root remainder, catenate it to the result, and exit.

```
[15]  ∆4:⍎(0>z[0])/'z←floor z'
[16]  ∆8:r←a Fsub z Fmul z
[17]  ∆9:z←z r
     ∇
```

The called function `floor` removes the fractional part of the number (just like ⌊):

```
     ∇ z←floor z
[1]    →(z[0]≥0)/0
[2]    z←0,1↓z[0]↓z
     ∇
```

### Fsqrt (Floating-point square root)

The floating square root function starts off the same way as the integer version.

```
     ∇ z←Fsqrt a;b;d;r
[1]    a←scalar a
[2]    ⎕SIGNAL(0^.>a)/11
[3]    →(0∨.≠1↓a)/∆1
[4]    z←0 0 ⋄ →0
```

We now pretend that our numbers are integers, and perform the integer square root. The number of radix places must be even, so add an extra trailing zero if they are not.

```
[5]   ∆1:→(~2|d←a[0])/∆2
[6]    d←d-1 ⋄ a←a,0
[7]   ∆2:z r←Isqrt 0,1↓a
```

If the remainder is greater than the square root add 1 to the root. Put the radix point in the correct place.

```
[8]    →(~r Fgt z)/∆4
[9]    z←z Fadd 0 1
[10]  ∆4:z[0]←z[0]+⌊0.5×d
     ∇
```

### Raising to a Power

In part 1 I showed how to raise a multiprecision number to a small power. In many algorithms we need to raise a small number to a multiprecision power, taking the result modulo some prime *p* (if we do not take the result modulo *p* we could soon get `WS FULL`).

The following function takes a scalar or multiprecision number `m`, and raises it to the power of `x[0]` modulo `x[1]`. The method is a variant of the small-power function shown in part 1, and is described in Ribenboim\[1\], *p*.38.

First we separate the right argument into power and modulus. Convert the power into a bit string. If this is null then our power was 0 and the result is 1, so we can exit.

```
     ∇ z←m Impow x;i;mod;n;q;r
[1]    x mod←x
[2]    z←1
[3]    →(0=⍴n←binary x)/0
```

If the power is 1 then the result is the number we started with. We do not reduce by the modulus since we assume that the programmer has ensured that the number starts off smaller than the modulus.

```
[4]    z←m
[5]    →(1=⍴n)/0
```

Square `z`. Starting at the left, look at the bits of the power. If the current bit is 1 we also multiply `z` by the number `m`. Then take the residue modulo `mod`. Proceed this way until the right of the power is reached.

Of course, the first bit in the power is always 1, so we can cut some processing, and we get the following:

```
[6]   i←1
[7]  ∆1:z←z Fmul z
[8]   →(~n[i])/∆2
[9]   z←z Fmul m
[10] ∆2:z←z Imod mod
[11]  →((⍴n)>i←i+1)/∆1
     ∇
```

The two functions called by `Impow` are as follows:

`binary` converts a multiprecision integer to bits by changing its base to a power of 2, then converting each element of the resulting number to bits. Note that we do not check that the number is a multiprecision integer: the function will fail with a `DOMAIN ERROR` if it is not.

```
     ∇ z←binary x
[1]    z←(1073741824,base)chbase x
[2]    z←(z⍳1)↓z←,⍉(30⍴2)⊤z←1↓z,z[0]⍴0
     ∇
```

`Imod` returns the residue of one number modulo another. It is, of course, possible to extract this result by using the remainder from `Idiv`, but there is a lot of processing in that function that is wasted when you only want the remainder, so `Imod` has been coded to run much faster.

```
     ∇ a←x Imod b;b1;j;q;qf;s
[1]    x←scalar x ⋄ b←scalar b
[2]    ⎕SIGNAL(0∨.>b[0],x[0])/11
[3]    s←0 ⋄ a←x
[4]    →(2<⍴,b←fullint b)/∆1
[5]    →(b[1]≤bsqr)/∆7
[6]   ∆1:b1←b[1]+(3↑b)[2]÷base
[7]   ∆2:→(~0 0 Fgt a)/∆3
[8]    a←|a ⋄ s←~s
```

If line 9 signals an error then there is a programming error in the function. This hasn’t happened for me yet!

```
[9]   ∆3:⎕SIGNAL(a Fgt|x)/11
[10]   →(~(a Fgt 0 ¯1)^b Fgt a)/∆5
[11]   →(~s^~a Fequal 0 0)/∆4
[12]   a←b Fsub a
[13]  ∆4:→0
[14]  ∆5:q←⌊qf←(a[1]+(3↑a)[2]÷base)÷b1
[15]   j←0
[16]   →(0≠q)/∆6
[17]   q←⌊qf←qf×base ⋄ j←1
[18]  ∆6:a←a Fsub q Fmul(a[0]+(⍴a)-j)↑b
[19]   →∆2
[20]  ∆7:b←b[1] ⋄ x←fullint x
[21]   a←x[1] ⋄ x←2↓x
[22]  ∆8:a←b|a
[23]   →(0=⍴x)/∆9
[24]   a←base⊥a,1↑x
[25]   x←1↓x
[26]   →∆8
[27]  ∆9:→(~s^0≠a)/∆10
[28]   a←b-a
[29]  ∆10:a←0,a
     ∇
```

### A User-defined Operator

I suggested last time that the Fibonacci calculation could go much quicker if we wrote our own scan operator. This is because the primitive scan operator does far too much work for our purposes. As an example, consider the following function:

```
     ∇ z←a plus b
[1]    i←i+1
[2]    z←a+b
     ∇
```

If we perform `plus` reduction on the first 10 numbers, we get the following:

```
      i←0
      a←plus/⍳10
      i
9
```

But scan in the same position is horrifying:

```
      i←0
      a←plus\⍳10
      i
45
```

And this is what is happening in our Fibonacci example. Normally we do not mind the extra processing all that much, because it does not take too much time, but with the multiprecision suite it takes up far too much.

For these circumstances we need a new operator, like this one:

```
     ∇ z←(F scan)x;y
[1]    z←⊂y←⊃x
[2]   ∆1:→(0=⍴x←1↓x)/0
[3]    z,←⊂y←y F⊃x
[4]    →∆1
     ∇
```

This operator only works on vectors, and `F scan 1 2 3 4` is equivalent to

```
      1 (1 F 2) ((1 F 2)F 3) (((1 F 2)F 3)F 4)
```

In particular it does not replace `\` (try `-\1 2 3 4` and `-scan 1 2 3 4`). However, it will speed up the operation we are doing here considerably.

```
      Fadd.Fmul scan 10⍴⊂2 2⍴1 1 1 2
```

### And there’s more ...

You now have all of the “basic” functions: the ”building blocks” of the multiprecision suite. If you have Dyalog APL you could have keyed these into a namespace called `mp` in order to keep them out of the way; if your APL does not support namespaces the functions will just be available in the workspace.

In Part III I shall introduce some applications of these functions.

### References

1. Paulo Ribenboim, *The Book of Prime Number Records* (Springer, 1988)
2. George McCarty, *Calculator Calculus* (EduCALC Publications, 1975)
3. *The APL Handbook of Techniques* (IBM DP Scientific Marketing, 1988. Order No. S320–5996–0)

(webpage generated: 11 November 2006, 22:06)
