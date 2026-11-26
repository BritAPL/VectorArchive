---
vid: '10009410'
title: Multiprecision Arithmetic – Part III
authors:
- John Sullivan
byline: by John Sullivan
volume: '12'
issue: '3'
page: '70'
source: trad/v123/sullivan123_70.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1996
tags:
- mathematics and statistics
- programming techniques
---

by John Sullivan
{ .byline }

### Preliminary

In the function listings that follow I have assumed that the basic functions as defined in Parts I and II of this series are in a namespace called `mp`. If your APL does not support namespaces the functions will be defined in the workspace in the ordinary way, and therefore you should omit the `mp`. before all references to them: for instance in function `Spsp`, below, line 3 will be

```
[3]    n←Fexec n
```

### Some simple applications

As I mentioned in Part II, it is much easier to handle multiprecision numbers if they are represented by character strings. So the first thing we will do with our basic functions is cover them by functions that process these character strings. (They will also process numeric input, as a result of the behaviour of `Fexec`). There are four functions for addition, subtraction, multiplication and division.

```
     ∇ z←a ADD b
[1]   z←0 mp.Ffmt⊃mp.Fadd/mp.Fexec¨a b
     ∇
```

```
     ∇ z←{a}SUB b
[1]    →(0≠⎕NC'a')/∆1
[2]    a←0 0
[3]   ∆1:z←0 mp.Ffmt⊃mp.Fsub/mp.Fexec¨a b
     ∇
```

```
     ∇ z←a MUL b
[1]    z←0 mp.Ffmt⊃mp.Fmul/mp.Fexec¨a b
     ∇
```

The division function introduces a small addition to the way numbers can be represented. Because the function deals with floating point rather than integers, sometimes we need to add precision to the dividend, in order to get extra precision in the quotient. This is done by using the letter P: in addition to saying 1.234E5 for 1.234×10<sup>5</sup>, we can also say 1.234E5P2 for 1.234×10<sup>5</sup> with 2 extra “digits” of precision, one digit being the size of the current radix of the calculation. This P format is sorted out in `DIV`, rather than by `Fexec`, so it only applies to this function.

```
     ∇ z←a DIV b;p;q;r
[1]    b←mp.Fexec b
[2]    →(^/~a∊⎕AV)/∆3
[3]    p←0 ⋄ →('P'^.≠a)/∆2
[4]    p←(1+r←a⍳'P')↓a ⋄ a←r↑a
[5]    →(0≠⍴p←(p∊⎕D)/p)/∆1
[6]    p←0 ⋄ →∆2
[7]   ∆1:⎕SIGNAL(2<⍴p)/16
[8]    p←⍎p
[9]   ∆2:a←mp.Fexec a
[10]   →(p≤0)/∆3
[11]   a[0]←a[0]-p ⋄ a←a,p⍴0
[18]  ∆3:z←0 mp.Ffmt a mp.Fdiv b
     ∇
```

Examples:

```
      '98765432109876543210' MUL '12345678901234567890'
1219326311370217952237463801111263526900
      '98765432109876543210' DIV '12345678901234567890'
8.000000072900000663390006
```

and the following, which may be compared (sorry, IBM!) with \[3\] *p*.52 (see also Vector, Vol.11, No.4, *p*.124):

```
      B←'222333333344444445555555'
      C←'12345.67890123456789'
      (B MUL C) DIV B
12345.67890123456789
```

When you write applications using the multiprecision suite try to avoid using these functions, because of the overhead of repeatedly translating into and out of character format. However, they are useful when using APL in desk-calculator mode for development and testing work.

### The Strong Pseudoprime Test

This is a development from Fermat’s theorem (also known as Fermat’s little theorem) which says that if *n* is prime then a<sup><i>n</i>−1</sup> ≡ 1 mod *n* for 0 &lt; a &lt; *n*. This also holds true for some composite numbers, which are therefore called *pseudoprimes to base a*. If these composites are pseudoprimes for all *a* then they are called *absolute pseudoprimes* or *Carmichael numbers*. Fermat’s theorem is improved by Euler’s criterion, which states that an odd composite number *n* is called an Euler pseudoprime for base *a* if *a<sup>(<i>n</i>-1)/2</sup>* ≡ (*a/<i>n</i>*) mod *n*, with (*a,<i>n</i>*)=1, where (*a/<i>n</i>*) is Jacobi’s symbol. Riesel\[1\], *p*.98, carries this a little further and arrives at the concept of a *strong pseudoprime*, which is defined as follows:

An odd composite number *n* with <i>n</i>−1 = 2<sup>s</sup><i>d</i>, *d* odd, is called a *strong pseudoprime for base a* if either *a<sup>d</sup>* ≡ 1 mod *n*, or <i>a</i><sup>2<sup>r</sup><i>d</i></sup> ≡ –1 mod *n*, for 0 ≤ `r` &lt; s.

This function is executed in the root namespace of the workspace. Its left argument is the base, which defaults to 2 if it is omitted, and the right argument is the number to be tested, which can be given as a character string or as a multiprecision number in vector format. If the number fails the test it must be composite, so we return with zero. Of course, if the number passes the test it could still be composite, but we return 1 to indicate that it is a pseudoprime.

```
     ∇ z←{a}Spsp n;d;e;i;n1;q;r;s;sm
[1]    ⍎(0=⎕Nc'a')/'a←2'
[2]    n←mp.Fexec n
```

We are going to split <i>n</i>−1 as described above. We strip the powers of 2 from <i>n</i>-1, count them into s, quotient is *d*

```
[3]    n1←d←n mp.Fsub 1 ⋄ s←0
[4]    ⍎((d[0]>0)∨~2|¯1↑d)/'d←⊃d mp.Idiv 2 ⋄ s←s+1 ⋄ →⎕LC'
```

If *a<sup>d</sup>* ≡ 1 mod *n* then exit

```
[5]    →(z←0 1 mp.Fequal e←a mp.Impow d n)/0
```

We now look at the variable `r`. When `r` is 0 we test the number we have just generated.

```
[6]    →(z←n1 mp.Fequal e)/0
[7]    r←1
```

Square *d* as `r` increases. We are testing for −1 modulo *n*, which is equivalent to <i>n</i>−1 modulo *n*, since `Imod` returns positive results only

```
[8]   ∆2:→(z←n1 mp.Fequal e←(e mp.Fmul e)mp.Imod n)/0
[9]    →(s>r←r+1)/∆2
```

You may want to add some progress-report code to this function or the `Impow` function, because for large numbers they take some considerable time to run.

For an application of this theorem let us look at a repunit. As mentioned in my preamble to Part I, the repunits are the numbers of the form (10<sup>n</sup>−1)/9. Obviously these can only be prime if *n* is prime, so let us look at the case <i>n</i>=17.

```
      Spsp 17⍴'1'
```

After about fifteen seconds (on a 486/33) comes the result (0, implying composite), so either we give up on this one, or we start trying to factor it. (The factors are in fact 2071723 and 5363222357, see later on, and also Yates\[2\], *p*.142.)

### A primality test

It is not my intention to regurgitate the contents of chapter 4 of Riesel here, but this function is included as an example of how to write APL code to fit the theorems in the book. This function is based on Theorem 4.6, *p*.109, and uses a relaxed converse of Fermat’s theorem to determine the primality of an integer.

The prerequisites for this test are as follows. If *N* is the number to be tested we must have a partial factorization of *N* − 1, such that if *N* − 1 = *R* *F* (where *F* is the factored part and *R* is the remaining part) then *R* &lt; *F*. The theorem states that if we can find an integer *a* such that GCD(<i>a</i><sup>(<i>N</i>−1)/<i>q</i></sup> − 1,<i>N</i>) = 1 for each divisor *q* of *F*, and a<sup><i>N</i>−1</sup> ≡ 1 mod *N*, then *N* is prime. This last condition implies that *N* is a pseudoprime to base *a*: this is not tested in the function here, because it takes so long, and it is assumed that the first thing that anyone does in testing for primality is a pseudoprime test, so it will have been tested already.

This function takes *a* as its left argument (which defaults to 2 if not given). The right argument is a 2-element vector of *N* and *F* in any format you care to supply (character, scalar or multiprecision number). The result is a return code followed by a helpful message: the return codes are 1 for a prime, −1 for a composite number and 0 if the test is inconclusive, in which case you can repeat it with other values of *a*.

We start off by sorting out our input and preparing to loop through the divisors. Note that we only really need one copy of each divisor in *F*, not all of them. Also, we do not test that each factor of *F* actually divides *N* − 1 because we assume that these details have been sorted out before calling the function.

```
     ∇ Z←{A}Test4_6 X;A;B;F;J;N;N1;Q;R;⎕IO
[1]    ⎕IO←0
[2]    N F←X ⋄ ⍎(0=⎕NC'A')/'A←2'
[3]    N←0 0 mp.Fadd mp.Fexec N ⋄ N1←N mp.Fsub 0 1
[4]    F←0 mp.Fadd¨?mp.Fexec¨F
[5]    J←0
```

Now test `1=GCD((B←¯1+A*N1÷F[J]),N)`. We can shorten the calculation here by noting that GCD(*B*,<i>N</i>) is the same as GCD(*B* mod *N*,<i>N</i>) and the mod bit is done in `Impow`, so if *B* mod *N* is 0 or 1 then the GCD is *N* or 1. The function is verbose here because it takes a fair length of time to run, and we want to know what is going on.

```
[6]   ∆1:Z←'Testing ',(0 mp.Ffmt J⊃F),', exponent is ',0 mp.Ffmt B←⊃N1 mp.Idiv J⊃F
[7]    Z,' residue is ',0 mp.Ffmt B←0 ¯1 mp.Fadd A mp.Impow B N
```

If *B* mod *N* is 0 the test was inconclusive, if it is 1 then this part of the test has succeeded. If it is anything else we can continue to calculate the GCD. If this is 1 then we continue with the next factor of *F*, otherwise we have found a factor of *N*, which must be composite.

```
[8]    →(0 0 mp.Fequal B)/∆3   ⍝ Test inconclusive
[9]    →(0 1 mp.Fequal B)/∆2   ⍝ GCD must be 1, so try next one
[10]   →(0 1 mp.Fequal Q←N mp.Gcd B)/∆2  ⍝ Else get GCD.
[11]   Z←¯1('Composite! ',(0 mp.Ffmt Q),' is a factor.') ⋄ →0
[12]  ∆2:→((⍴F)>J←J+1)/∆1
[13]   Z←'Prime. By Riesel Th. 4.6 with factor',(1<⍴F)/'s'
[14]   Z←dbr Z,,⎕FMT 0 mp.Ffmt¨F ⋄ Z←1 Z ⋄ →0
[15]  ∆3:Z←0('Riesel Th. 4.6 primality test inconclusive with A=',⌷A)
     ∇
```

The GCD function uses Euclid’s algorithm to calculate the Greatest Common Divisor. (It is stored in namespace `mp`.) In order to make things a bit easy when scalars are supplied, there are two paths through the function.

```
     ∇ Z←A Gcd B
[1]    →(1^.=(⍴A),⍴B)/∆2
[2]    ⍎(B Fgt A)/'A B←B A'
[3]   ∆1:→(0 1 Fequal Z←A Imod B)/0
[4]    ⍎(0 0 Fequal Z)/'Z←B ⋄ →0'
[5]    ⍎(0 Fgt Z)/'Z←Z Fadd B'
[6]    A←B ⋄ B←Z ⋄ →∆1
```

The following code replicates the previous code, but for scalar rather than multiprecision input.

```
[7]   ∆2:⍎(B>A)/'A B←B A'
[8]   ∆3:→(1=Z←B|A)/0
[9]    ⍎(0=Z)/'Z←B ⋄ →0'
[10]   ⍎(0>Z)/'Z←Z+B'
[11]   A←B ⋄ B←Z ⋄ →∆3
     ∇
```

For an example we can test *N* = 39 × 2<sup>22</sup> + 1 = 163577857, which can easily be verified a prime by trial division.

```
      2 Test4_6 163577857 2
Testing 2, exponent is 81788928, residue is 0
0  Riesel Th. 4.6 primality test inconclusive with A=2
```

```
      3 Test4_6 163577857 2
Testing 2, exponent is 81788928, residue is 0
0  Riesel Th. 4.6 primality test inconclusive with A=3
```

```
      5 Test4_6 163577857 2
Testing 2, exponent is 81788928, residue is 163577855
1  Prime. By Riesel Th. 4.6 with factor 2
```

To show what can happen, I will give an example with a small Carmichael Number (which, as I said earlier, is pseudoprime for all bases). Note that the strong pseudoprime test is stronger than that used for the Carmichael numbers: as 294409 (= 37 × 73 × 109) is not a strong pseudoprime to base 2 we would not normally have got as far as testing it with this function.

```
      Test4_6 294409 (2 3 29 47)
Testing 2, exponent is 147204, residue is 0
0  Riesel Th. 4.6 primality test inconclusive with A=2
```

```
      3 Test4_6 294409 (2 3 29 47)
Testing 2, exponent is 147204, residue is 0
0  Riesel Th. 4.6 primality test inconclusive with A=3
```

```
      5 Test4_6 294409 (2 3 29 47)
Testing 2, exponent is 147204, residue is 32264
¯1  Composite! 4033 is a factor.
```

### Factorizing

There are several different methods for finding the factors of a number. The obvious first choice for a method is trial division: keep dividing your number by successive primes until you get a zero remainder: divide the number by that prime and start the process again on the quotient with the same prime (it may appear more than once in the factorization) until the next prime is greater than the square root of the remaining number. This works fine for small numbers (with a computer this process is relatively quick) but is no good for numbers like (10<sup>33</sup>−1)/9, where the divisors 67<sup>2</sup>, 271, 397 and 126951 are found very quickly, but the remaining number is a bit more difficult to factorize (it is 213524917 × 68618940391). This trial division can be speeded up in certain cases, for example the primitive prime factors of (*a<sup>n</sup>− b<sup>n</sup>*)/(*a− b*) must be of the form <i>kn</i>+1 (or 2<i>kn</i>+1 if *n* is odd), which eliminates a lot of checking, but there’s still a lot left to do.

The next systematic factorizing method to appear, is due to Fermat. The idea is to write an odd composite number as the difference between two square numbers. If we can write  
*n* = *`x`<sup>2</sup>− y<sup>2</sup> = (`x` − y*)(*`x` + y*) where *`x` − y* ≠ 1,  
we immediately have a factorization. See Riesel\[1\], *p*.153ff, for the details.

We start by making our input numeric, then taking its square root. If the remainder is 0 we have hit on a factorization immediately, so we exit; otherwise we compute an intermediate value `m` = \[√<i>n</i>\] + 1, which is the smallest possible value of *x*.

```
     ∇ r←Fermat n;a;m;sqrem;z
[1]    n←mp.Fexec n
[2]    a sqrem←mp.Isqrt n
[3]    →(~0 0 mp.Fequal sqrem)/∆1
[4]    'Number is the square of ',0 mp.Ffmt r←a ⋄ →0
[5]   ∆1:m←a mp.Fadd 0 1
```

We consider <i><tt class="aplu">z</tt> = <tt class="aplu">x</tt><sup>2</sup> − <i>n</i></i> and check whether this is a square. If it is then we have finished, otherwise we try the next possible `x`, i.e. `m` + 1, and try again.

```
[6]    z←(m mp.Fmul m) mp.Fsub n
[7]    a sqrem←mp.Isqrt z
[8]    →(0 0 mp.Fequal sqrem)/∆3
[9]   ∆2:z←z mp.Fadd 1 mp.Fadd 2 mp.Fmul m
[10]   m←1 mp.Fadd m
[11]   a sqrem←mp.Isqrt z
[12]   →(~0 0 mp.Fequal sqrem)/∆2
[13]  ∆3:r←(m mp.Fsub a)(m mp.Fadd a)
     ∇
```

This function is quick if the two factors are approximately the same size, and it is horribly slow if they are wide apart. It can be made to finish quicker by multiplying the number to be factored by small numbers, but how you decide which small numbers to use is a black art. I think most examples in the reference books are worked backwards from known results (I must confess that my examples were all worked backwards also!).

It can also be made to run much faster for numbers of a certain type. For example, if we know that all the factors of a number have the form 2<i>kn</i> + 1 (which they do for most numbers of the form *a<sup>n</sup>* ± *b<sup>n</sup>* for example) then we can reduce the number of iterations by a factor of 2<i><i>n</i><sup>2</sup></i>. See Riesel, *p*.189, for the details. The function is very similar to the previous one, but supply the value of *n* as the left argument (here called *p* to confuse everybody):

```
     ∇ r←p Fermatx n;a;m;mod;res;sqrem;t;x;z
[1]    n←mp.Fexec n
[2]    a sqrem←mp.Isqrt n
[3]    →(~0 0 mp.Fequal sqrem)/∆1
[4]    'Number is the square of ',0 mp.Ffmt r←a ⋄ →0
```

A bit of number theory comes in here. We calculate the modulus (2<i><i>p</i><sup>2</sup></i>) and residue of (<i>n</i>+1)/2 modulo 2<i><i>p</i><sup>2</sup></i>. The residue must be congruent to 1 modulo *p*, and we have an error if it isn’t.

```
[5]   ∆1:res←(⊃(1 mp.Fadd n)mp.Idiv 2)mp.Imod mod←2×p×p
[6]    ⎕SIGNAL(1≠p|1↓res)/11
[7]    t←⊃(1 mp.Fadd a)mp.Idiv mod
```

The calculations are slightly different here, as we calculate an intermediate variable `x` to assist in the calculation of `z`. In order to avoid repetition of coding, an if-then-else block is provided to generate `z` differently first time round compared with further execution (the first time is identified by `z` = 0). `m` is only calculated if it is required to generate the next version of `z`. When we have finished we use `x` to obtain the factors.

```
[8]    z←0 0
[9]   ∆2:x←res mp.Fadd mod mp.Fmul t←1 mp.Fadd t
[10]   →(0 0 mp.Fequal z)/∆3
[11]   z←z mp.Fadd m ⋄ →∆4
[12]  ∆3:z←(x mp.Fmul x)mp.Fsub n
[13]  ∆4:a sqrem←mp.Isqrt z
[14]   →(0 0 mp.Fequal sqrem)/∆5
[15]   m←(mod×2)mp.Fmul x mp.Fadd mod÷2
[16]   →∆2
[17]  ∆5:r←(x mp.Fsub a)(x mp.Fadd a)
     ∇
```

I tried to factor (10<sup>17</sup>−1)/9, a number that we know is composite, see earlier. I seemed to be getting nowhere, so I tried multiplying it by 34<i>k</i>+1 for some values of *k*. All values seemed to give the same result, i.e. a long wait for nothing, until I tried 2585 (*k* = 76). Then I got the following, in exceedingly quick time:

```
      0 mp.Ffmt¨17 Fermatx 2585 mp.Fmul mp.Fexec 17⍴'1'
5355403955  5363222357
```

From this it is easy to see which factor should be divided by 2585 to obtain the factors of 1111111111111111.

Of course, this is a trivial example: normally the numbers will be much bigger and take much longer to factor. There are many other methods of factorizing large numbers, and the best method may be a combination of several of them running concurrently, the task terminating when a factor is found in any of them.

I hope that this article has been useful in showing how to apply the multiprecision functions to solve problems with primality testing and factorizing.

### References

1. Hans Riesel, *Prime numbers and computer methods for factorization* (Birkhäuser, 1985).
2. Samuel Yates, *Repunits and Repetends* (Star Publishing, Florida, 1982).
3. *The APL Handbook of Techniques* (IBM DP Scientific Marketing, 1988. Order No. S320-5996-0).

### Books on Number Theory

In this article, I have mentioned several items from number theory which there is no room to explain. I would refer the reader to any standard text on number theory for further information, for example:

- G H Hardy and E M Wright, *An introduction to the theory of numbers*, Oxford University Press, 1938 and several later editions. I am sorry to say that the index in this book is atrocious, and is unworthy of a standard textbook.
- W Sierpinski, *Elementary theory of numbers*, North-Holland, 1988.
- D M Burton, *Elementary number theory*, Allyn and Bacon, 1980. This is the Open University set book, and its explanations are very clear.

(webpage generated: 21 November 2007, 19:54)
