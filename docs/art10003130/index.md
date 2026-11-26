---
vid: '10003130'
title: A Note on Primes
authors:
- Ted Emms
byline: by Ted Emms
volume: '10'
issue: '4'
page: '30'
source: trad/v104/emms104_30.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- mathematics and statistics
- programming techniques
- education
---

by Ted Emms <span class="affil"></span> <span class="email"></span>
{ .byline }

*“My barmie noddle’s working prime.”  
Robert Burns (1759-1796)*

A prime number is an integer that is only divisible by 1 and itself. There are several clever APL “one-liners” which give the first N prime numbers. For example

```
     ⎕←(2=+/[1]0=A∘.|A)/A←⍳N             F1
```

The idea here is to divide the range 1-N by all the numbers in the range and then identify those numbers which are only divisible by two numbers (1 and the number itself). If one is only interested in the start of the series of prime numbers then this formula will suffice. For example N=120 gives the series 2 3 5 ... 109 113 but N=121 gives (I-APL on my computer) the dreaded WS FULL error message. Another “one-liner” is

```
     ⎕←(~A∊A∘.×A)/A←1↓⍳N                 F2
```

and this works well up to N=120 but fails for N=121.

We can take advantage of the fact that the only prime which is even is 2. Thus we can have the formula deal with odd values of N and then prefix 2 on the list of primes produced. Formula F1 becomes

```
     ⎕←2,(1=+⌿0=A∘.|A)/A←1+2×⍳N          F3
```

and F2 becomes

```
     ⎕←2,(~A∊A∘.×A)/A←1+2×⍳N             F4
```

and these are a great improvement. For example with N=115 formula F3 gives  
2 3 5 7 ..... 227 229 but N=116 fails. Similar remarks apply to F4.

If we want the series of primes to be longer we must avoid the memory-gobbling behaviour of the outer product (using a two-dimensional array). The first approach is to create a vector V of the first N integers and drop the first. The first term is now 2 and we enter this in an empty vector T. Then we cancel all the terms in V divisible by 2. The first term in V is now 3 and we add this to T and cancel all terms in V divisible by 3. We continue in this way until the vector V is empty. T then contains the series of primes. This program is a straightforward application of the “Sieve of Eratosthenes”.

```
[0]   T←F5 N;V;B
[1]   V←1↓⍳N
[2]   T←0⍴0
[3]  LP:B←V[1]
[4]   T←T,B
[5]   V←(~0=B|V)/V
[6]   →(0<⍴V)/LP
```

You might try this routine with F5 500. It gives the primes up to 500 but is very slow. We can speed it up by only dealing with the odd numbers and prefixing the list with the prime 2.

```
[0]   T←F6 N;V;B
[1]   V←1+2×⍳⌊(N-1)÷2
[2]   T←2
[3]  LP:B←V[1]
[4]   T←T,B
[5]   V←(~0=B|V)/V
[6]   →(0<⍴V)/LP
```

This is faster but we can do better. Rather than terminate the loop when the V vector is empty we can terminate when the length of the T vector equals or exceeds the length of the V vector. Then we obtain the final list by appending the V vector to the T vector.

```
[0]   T←F7 N;V;B
[1]   V←1+2×⍳⌊(N-1)÷2
[2]   T←2
[3]  LP:B←V[1]
[4]   T←T,B
[5]   V←(~0=B|V)/V
[6]   →((⍴T)<⍴V)/LP
[7]   T←T,V
```

But we can still do better. The largest divisor of the biggest prime cannot exceed the square root of that prime. Therefore we need only carry on with the loop until the leading term of T equals or exceeds that square root. This small but significant change now gives the fastest solution.

```
[0]   T←F8 N;V;B;K
[1]   K←⌊N*0.5
[2]   V←1+2×⍳⌊(N-1)÷2
[3]   T←2
[4]  LP:B←V[1]
[5]   T←T,B
[6]   V←(~0=B|V)/V
[7]   →(B<K)/LP
[8]   T←T,V
```

So we have arrived at a routine which will give a long list of primes and yet is reasonably fast. Once having reassured yourself that the routine works (e.g. with F8 500) you can move to F8 10000. This takes a few minutes but the reward is the list of primes up to and including 9973. You may care to save this vector as VP (vector of primes) for later use.

For example if you want a list (P3) of all three digit primes we can first eliminate the primes that are too big by `P3←(VP<1000)/VP` and then the primes that are too small by `P3←(P3>99)/P3` We can use VP to find out whether individual numbers larger than 10000 are prime or not. Say we wish to find if N is prime. We first find the square root of N and then the next largest integer:

```
     K←⌈N*0.5
```

We then create a reduced list of primes P by

```
     P←(VP<K)/VP
```

and use this to determine if any of these are divisors of N by `+/P|N` If this is greater than 0 then N is composite. We may put these ideas together to form the heavily-contrived direct definition:

```
AA:(2 9⍴'COMPOSITEPRIME     ')[1+0=+/0=((VP<⌈A*0.5)/VP)|A←?;]
```

I’m not sure that this qualifies for a one-liner since it does require the presence of VP in the workspace. However the ideas contained may be employed in solving the following problems.

1. Take all the integers 0,1,...,9 and use them to form two 5-figure primes.
2. Take the 9 integers 1,2,...,9 and use them to form three primes, one of 2 digits, one of 3 digits and one of 4 digits.
3. Take the 9 integers 1,2,...,9 and form three 3-digit primes. The primes must be the smallest “anagrams”. The numbers 137, 173 and 317 are anagrams (rearrangements of the same digits) and so only the smallest (137) is available for choice.

\[Ted Emms has provided solutions to these problems, which I shall publish next issue – together with any other contributions readers may like to submit – Ed\]

(webpage generated: 28 March 2006, 06:40)
