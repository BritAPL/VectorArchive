---
vid: '10010580'
title: 'J-ottings 44: So Easy a Child of Ten ...'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '21'
issue: '4'
page: '101'
source: trad/v214/jot214.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- mathematics and statistics
- programming techniques
---

by Norman Thomson
{ .byline }

Perfect shuffles just won’t go away! Following J-ottings 43 on this subject, Eugene MacDonnell, Roger Hui and Jeff Shallit made insightful comments which help cast the problem in a broader context. I shall endeavour to summarise their thoughts here, sauced with generous helpings of J!

To recap, a perfect or ripple shuffle of a deck of cards consists of dividing it into two halves (or as nearly as possible if there is an odd number of cards) and taking one card in turn from each half. A single shuffle of a given number of cards is given by

```
   sh=./:@$&0 1            NB. ripple shuffle of i.y.
```

or for repeated shuffling, make the argument into a list (not necessarily numeric) 

```
   rs=./:0 1&($~)@#        NB. ripple shuffle of y.
```

Assume in what follows that both the word “number”and the letters m, n and k denote “a positive integer”, while the letter p means “a prime number” (including 1). A result called Fermat’s Little Theorem, first formally proved by Euler in 1736, states that if (n,p) are relatively prime, then n<sup>p-1</sup>=1 in modulo p arithmetic.  “Relatively prime”says in words what GCD(m,n)=1 says in maths, or `1=m+.n`  says in J, as in the verb:

```
   rps=.i.#~(e.&1(+.i.))    NB. relative primes of y.
   rps 15
1 2 4 7 8 11 13 14
```

Modulo n arithmetic is what primary school children are familiar with as ‘clock arithmetic’, that is the arithmetic of a finite set of numbers `i.n` equally spaced around the rim of a clock. A J session can be set up to perform modulo n arithmetic by setting the modulus and defining an adverb such as `mod` :

```
   n=.7
   mod=.1 : 'n&|@x.'
   (6+mod 3),(*:mod 9)  NB. (9 mod 7),(9^2 mod 7)
2 4
```

Advancing a little (but only a little!) beyond primary school, every number possesses a ‘totient’, where tot(n) is the number of relatively prime numbers which are less than n.  Thus tot(2) is 1, tot(3) and tot(4) are both 2 (the relatively prime number lists being 1,2 and 1,3 respectively), tot(5)=4 (all lower numbers) and so on.  tot(n) is often written f(n), and called ‘Euler’s phi’, or in J, `#@rps`.  Were this mathematical function just a little more useful, it might well have found a place on calculator keyboards, or indeed as a J primitive, along with factorial, log, sin, etc., and the like.

However, it is not necessary to enumerate relatively prime numbers to find tot(n) since it is given by the closed formula

f(n) = n(1-1/p1)… (1-1/pn) where the p’s are the unique prime factors of n 

Totient can thus be regarded as an extension of `q` which gives the prime factorisation of n: 

```
   tot=.*/@,(-.@%)@(~.@q:)      NB. totient (Euler's phi)
   (tot 10),(tot 51)
4 32
```

Neither set of parentheses is necessary in the above definition of `tot`, but they help to clarify how it works.  `(-.@%)n` is 1 - 1/n, `(~.&.q:)` is the prime factor nub, and the comma makes the hook which multiplies in the factor n.

Euler generalised Fermat’s Little Theorem to non-primes by proving that, provided m and n are relatively prime, m<sup>tot(n)</sup> = 1 (mod n). For primes, *all* preceding numbers are relatively prime, so tot(p) = p-1 and Euler’s and Fermat’s theorems are equivalent in this case. Some other properties of the totient are simple to prove, viz. 

tot(n) is even for all n>2 (this follows from the closed formula)  
tot(2<sup>k</sup>) = 2<sup>k-1</sup> (because every odd number less than 2<sup>k</sup> is relatively prime)  
tot(mn) = tot(m).tot(n) if m,n are relatively prime; and  
              &#61; n.tot(m) when the prime factors of n are a subset of those of m.

In particular tot(n<sup>2</sup>) = n.tot(n). The third of the above properties can be described by saying that tot is a multiplicative function. Generalising the result to prime factor products, if n=(p<sub>1</sub><sup>k1</sup>)(p<sub>2</sub><sup>k2</sup>)…(p<sub>v</sub><sup>kv</sup>) then

<pre class="APLU">tot (n) = */ tot (p<sub>1</sub><sup>k1</sup>), tot (p<sub>2</sub><sup>k2</sup>), ... ,tot (p<sub>v</sub><sup>kv</sup>) </pre>

As an aside, the functions tau(n) and sigma(n) as defined below are also multiplicative functions.

```
   seldivs=.0&=@|~i.       NB. select divisors of y.
   divs=.seldivs~#i.       NB. divisors of y. excl y.
   divs 12
1 2 3 4 6
   tau=.#@,divs            NB. tau=no. of divisors incl y.
   sigma=.+/@,divs         NB. sigma=sum of divisors incl y.
   (tau&>12 13 156);(sigma&>12 13 156)
+------+---------+
|6 2 12|28 14 392|
+------+---------+
```

To illustrate the sort of possible uses for tot(n) and modulo n arithmetic, suppose that the last two digits of 3<sup>256</sup> (which incidentally has 123 digits altogether) are required.  tot(100) = 40 so the problem reduces to that of finding the last two digits of 3<sup>16</sup> by e.g.    
   (3<sup>16</sup>)= (81)<sup>4</sup> = (-19)<sup>4</sup> = (361)<sup>2</sup> = 612 = 3721 = 21

As a further aside, it is not hard to prove that tot(2n) = tot(n) if n is odd and =2.tot(n) if n is even, a result which it is pleasing to have J confirm by comparing matching columns in

```
   (5 6$tot&>>:i.30);5 6$tot&>2*>:i.30
+----------------+-----------------+
| 1  1  2  2  4 2| 1  2  2  4  4  4|
| 6  4  6  4 10 4| 6  8  6  8 10  8|
|12  6  8  8 16 6|12 12  8 16 16 12|
|18  8 12 10 22 8|18 16 12 20 22 16|
|20 12 18 12 28 8|20 24 18 24 28 16|
+----------------+-----------------+
```

As well as *confirming* results, J can also *suggest* results ahead of proof.  For example, the result that tot(3n) = 3tot(n) for multiples of 3, and = 2tot(n) otherwise is forecast with clarity by

```
   (5 6$tot&>>:i.30);5 6$tot&>3*>:i.30
+----------------+-----------------+
| 1  1  2  2  4 2| 2  2  6  4  8  6|
| 6  4  6  4 10 4|12  8 18  8 20 12|
|12  6  8  8 16 6|24 12 24 16 32 18|
|18  8 12 10 22 8|36 16 36 20 44 24|
|20 12 18 12 28 8|40 24 54 24 56 24|
+----------------+-----------------+
```

Returning to the ripple shuffle problem, the number of shuffles required to restore an even numbered deck of n cards to its original order is the number of times 2 must be multiplied in modulo n-1 arithmetic in order to obtain 1. To obtain such a value, one way is simply to carry on multiplying and reducing modulo (n-1) until 1 is reached, an event which Euler’s theorem guarantees is bound to happen. However there may be  an earlier arrival at the target than that predicted by Euler’s Theorem. For example tot(51) = 32, so that 32 shuffles will restore 52 cards to their original order.

However, if 2 is doubled repeatedly (note a good excuse for a gerund!) :

```
   n=.51                          NB. set modulus
   mod=.1 : 'n&|@x.'              NB. redefine mod
   p2=.,$:@(+:mod@{:)`}.@.(1&e.)  NB. powers of 2
   p2 2
2 4 8 16 32 13 26 1
```

It transpires that a mere 8 steps are sufficient. 8 is called the multiplicative order of 2 (mo2 for short) in modulo 51 arithmetic, and Euler’s theorem guarantees that mo2(n) is a divisor of tot(n), which is helpful in manual searches.  mo2 is of course just `#p2` . As an alternative to redefining p2 every time the modulus is reset, write

```
mo2=.3 :0               NB. mult order of 2 for odd modulus y.
r=.2
while.(1~:y.|r)do.r=.x:2*r end. [ 2^.r
)
   (mo2 13),(mo2 51)
12 8
```

Now revisit the ripple shuffle with an even number of cards, for example

```
   sh 10
0 2 4 6 8 1 3 5 7 9
```

It takes only a moment to see that 0 and 9 will remain in place in repeated shuffles, and that the second position will be occupied by successive powers of 2 in modulo 9 arithmetic. The number of shuffles to restore a pack with an even number of cards n is thus mo2(n-1).

Eugene pointed out that another way to regard a shuffle such as `sh 10` is as a permutation of `i.10`, which can be expressed using `C.` as a combination of cycles:   

```
   C. sh 10
+-+---+-----------+-+
|0|6 3|8 7 5 1 2 4|9|
+-+---+-----------+-+
```

and if shuffling is continued until the original order is restored, the cycles emerge in the columns of these lists read as a matrix : 

```
   rs^:(i.6)i.10       NB. all distinct shuffles of 10
0 1 2 3 4 5 6 7 8 9
0 2 4 6 8 1 3 5 7 9
0 4 8 3 7 2 6 1 5 9
0 8 7 6 5 4 3 2 1 9
0 7 5 3 1 8 6 4 2 9
0 5 1 6 2 7 3 8 4 9
```

This demonstrates clearly that if 1 is restored to its original position all the other numbers will obediently follow suit.  Since *all* the cycles must return to the start point, the LCM of the lengths of the individual cycles determines the number of perfect shuffles to restore a deck of n cards :

```
   cyclecnt=.(#&>)@(C.@sh)
   cyclecnt 10
1 2 6 1
   ns=.*./@:cyclecnt   NB. no. of restoring shuffles
   ns 52
8
```

If n is odd, `C.sh n` is the same as `C.sh n+1` only without the final one-element box:

```
   C. sh 9
+-+---+-----------+
|0|6 3|8 7 5 1 2 4|
+-+---+-----------+
```

Thus ns(n) and ns(n-1) are identical in value to mo2(n-1) so that ns(n) is defined for *all* integers. The LCM of the `cyclecnt` of a product mn is the LCM of the `cyclecnts` of m and n separately, subject to GCD(m,n)=1. For example:

```
   cyclecnt&.> 11 13 143
+----+----+-------------+
|1 10|1 12|1 10 12 60 60|
+----+----+-------------+
   ns&>11 13 143          NB. LCM(10,12)=60
10 12 60
```

Generalising the LCM property, <tt class="APLU">mo2(n) = \*./ mo2(p<sub>1</sub><sup>k1</sup>),mo2(p<sub>2</sub><sup>k2</sup>), ... ,mo2(p<sub>v</sub><sup>kv</sup>)</tt> which is identical in form to the analogous expression for tot above, only with `*.` (that is LCM) replacing `*` (multiply). The relationship between the notions of multiply and LCM is emphasised by the closeness of the *notation* in J. `mo2` of course is not a multiplicative function – perhaps it should be called an LCM-ic function!

Multiplicative order is a property of all relatively prime numbers less than the modulus. `mo10(n)`, where `mo10` is defined analogously to `mo2`, gives the period length of the recurrence in the decimal representation of `%n`, for example :

```
   (mo10 13),%13
6 0.076923076923
```

For shuffles where every third card is picked ns3 counts the number of shuffles to restore:

```
   sh3=./:@$&0 1 2            NB. shuffle with every 3rd card
   sh3 10
0 3 6 9 1 4 7 2 5 8
   C.sh3 10
+-+-----+-----------+
|0|7 2 6|9 8 5 4 1 3|
+-+-----+-----------+
   cc3=.(#&>)@C.@sh3
   ns3=.*./@:cc3              NB. #shuffles to restore
   ns3&>10 11 12              NB. .. with 10,11 & 12 cards
6 5 5
```

Analogously with ns, ns3(3n) is identical in value to ns3(3n-1), as shown by :

```
    C.sh3 12
+-+---------+----------+--+
|0|9 5 4 1 3|10 8 2 6 7|11|
+-+---------+----------+--+
   C.sh3 11
+-+---------+----------+
|0|9 5 4 1 3|10 8 2 6 7|
+-+---------+----------+
```

although unlike ns, values of ns3(n) no longer coincide with those of mo3(n).

The above procedure can be extended to shuffles with picking at any regular interval, and all the previous discussion on shuffles can be condensed into

```
   shn=./:@$ i.                      NB.x. cards, pick each y.th
   nsn=.*./@:(#&>)@C.@shn            NB.#shuffles to restore
   (51 nsn 2),(10 nsn 3)
8 6
```

Multiplicative orders are a more general property than shuffle counts. Here is a table of totients and the first three multiplicative orders of the first few integers:

<pre class="APLU">
 n:  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16
--------------------------------------------------

tot: 1  2  2  4  2  6  4  6  4 10  4 12  6  8  8

--------------------------------------------------
mo2     2  _  4  _  3  _  6  _ 10  _ 12  _  4  _
mo3        2  4  _  6  2  _  4  5  _  3  6  _  4
mo5           _  2  6  2  6  _  5  2  4  6  _  4

tot<sup>2</sup> 1  1  1  2  1  2  2<sup>*</sup>  2  2  4  2<sup>*</sup> 4  2  4<sup>*</sup> 4<sup>*</sup>


 n:  17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32
-----------------------------------------------------
tot  16  6 18  8 12 10 22  8 20 12 18 12 28  8 30 16
-----------------------------------------------------
mo2   8  _ 18  _  6  _ 11  _ 20  _ 18  _ 28  _  5  _
mo3  16  _ 18  4  _  5 11  _ 20  3  _  6 28  _ 30  8
mo5  16  6  9  _  6  5 22  2  _  4 18  6 14  _  3  8
-----------------------------------------------------
tot<sup>2</sup> 8  2  6  4<sup>*</sup>  4<sup>*</sup> 4 10  4<sup>*</sup> 8  4  6  4<sup>*</sup> 12  4* 8  8*</pre>

If mo2(n) is equal in value to tot(n) it is called a “primitive root”of n.  Roughly speaking, powers of primitive roots exhaust the full gamut of modulo n integers before repeating. Looking at the second and third rows in the table, 2 is a primitive root of some numbers such as 9 and 13, but not of others such as 7 and 17. The table also shows that 3 and 5 are primitive roots of 7. The final row is the totient of the totient which in the case of primes, is also the number of primitive roots. This is also the case for those non-primes such as 9 which possess primitive roots, other non-primes such as 15 have no primitive roots, and are marked with an asterisk in the final row. There is no general formula for primitive roots, but for small numbers such as those given in the table, they are not hard to find, particularly if a computer with J is at hand.   For example, 2 is a primitive root of 13 from the table, and the other three are to be found to be 6, 7 and 11 by observing that

```
6^mod
divs tot n=.13                         NB. powers of 6 modulo
13
6 10 8 9 12
```

does not contain 1, and similarly for 7 and 11. Alternatively use lists to test all the candidate numbers simultaneously:

```
   (<>:i.12)^mod&>>divs tot n=.13      NB. find pr. roots of 13
1  2 3  4  5  6  7  8 9 10 11 12
1  4 9  3 12 10 10 12 3  9  4  1
1  8 1 12  8  8  5  5 1 12  5 12
1  3 3  9  1  9  9  1 9  3  3  1
1 12 1  1 12 12 12 12 1  1 12  1
```

With a little more code all the primitive roots of primes can be extracted in one go:  

```
   t#~-.1 e."1 |:(<t=.rps n)^mod&>divs tot n=.13
2 6 7 11
   t#~-.1 e."1 |:(<t=.rps n)^mod&>divs tot n=.15
```

(null list)

Although this discussion has led into the beginnings of number theory on the one hand and combinatoric analysis on the other, nevertheless a primary school child with outstanding numerical gifts could well appreciate all the *notions* in this article, if not perhaps the *notation*, and could, with at most the aid of a hand calculator, compute the above table of totients and multiplicative orders. Perhaps it is not a coincidence that the abbreviated form is tot(n)!

Norman Thomson October 2005
