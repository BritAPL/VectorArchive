---
vid: '10005600'
title: 'At Play with J: An Open and Shut Case'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '16'
issue: '4'
page: '99'
source: trad/v164/gene164.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2000
tags:
- mathematics and statistics
- programming techniques
- education
---

*by Eugene McDonnell*
{ .byline }

My 11-year old granddaughter, Amy Powers, brought home a problem the other day which her mathematics teacher had given to her 6th-year class. I thought it pedagogically fruitful, and advanced beyond any mathematics I had learned at her age. It demonstrates how much teaching has changed for the better in the more than sixty years since I was in the 6th year. A broader variety of subject matter beyond the elementary algebra I was taught and a better method of teaching mathematical topics have made the subject more interesting and, I am convinced, easier to learn.

Some background: in many schools in the United States, lockers are provided in which the students may store their textbooks and other belongings. These are usually placed in long rows, and some inspired teacher was inspired to create the *Locker Problem* to give a concrete base on which to help teach some facts about divisors.

## The Locker Problem

In a high school (years 9 to 12) there are 1,000 lockers placed next to each other along a hall. During winter break, the custodians at the school clean the lockers and paint fresh numbers on each locker door. The lockers are numbered from 1 to 1,000. When the 1,000 students arrive back from their vacation, they decide to celebrate returning to school by working off some energy. Here is what they do: Student 1 runs down the row of lockers and inverts every door. (To invert a door means to open it if it was closed, and to close it if it was open.) Student 2 runs down the row of lockers and inverts doors 2, 4, 6, 8, and so on to the end of the line. Next student 3 inverts the doors of lockers 3, 6, 9, 12, and so on to the end of the line. This pattern continues until each of the 1,000 students has had a turn to run down the hall.

When the students are finished, which doors are open?

Amy set about answering this question by making a table:

<pre>
                     1 1 1 1 1 1 <b>1</b> 1
   <b>1</b> 2 3 <b>4</b> 5 6 7 8 <b>9</b> 0 1 2 3 4 5 <b>6</b> 7
 <b>1</b> <b>O</b> O O O O O O O O O O O O O O O O
 2   C   C   C   C   C   C   C   C
 3     C     O     C     O     C
 <b>4</b>       <b>O</b>       O       C       O
 5         C         O         O
 6           C           O
 7             C             O
 8               C               C
 <b>9</b>                 <b>O</b>
10                   C
11                     C
12                       C
13                         C
14                           C
15                             C
<b>16</b>                               <b>O</b>
17                                 C
</pre>

The first row of all Os shows that after student 1 has finished, all the doors are open. The next row, for student 2, shows that the even numbered doors are closed. The third row, for student 3, shows that every third door has changed its state: if it had been open, it is now closed, and if closed, is now open. I’ve added row and column stubs, and have darkened the Os along the diagonal, and the corresponding row and column numbers. Because doors 1, 4, 9, and 16 were open, Amy guessed that the doors that were open were those of students whose numbers were squares. Was she right?

The next step might be difficult for a sixth-grade student to have arrived at. It is to count the number of students who inverted each door. Each of the Os and Cs in Amy’s table is a divisor of the number it lies beneath. For example, the Os and Cs under 12 are inversions by the six students numbered 1, 2, 3, 4, 6, and 12; thus, the number of divisors of 12 is six. The number of divisors of a number varies irregularly. Counting the divisors of our seventeen numbers gives the table:

<pre>
<b>1</b> 2 3 <b>4</b> 5 6 7 8 <b>9</b> 10 11 12 13 14 15 <b>16</b> 17
<b>1</b> 2 2 <b>3</b> 2 4 2 4 <b>3</b>  4  2  6  2  4  4  <b>5</b>  2
</pre>

At first glance, there doesn’t seem to be any pattern to the number of divisors, except that there is a tendency for the number to increase; however, I’ve put in bold type those integers and their divisor counts where the count is odd, since only those doors will be open that have been inverted by an odd number of students. In Amy’s chart the doors that are open are numbers 1, 4, 9, and 16. Her conjecture was that the doors that are open are those whose numbers are squares. I made the further conjecture that squares and only squares have an odd number of divisors. Both conjectures are true. If a number is a square, it has an odd number of divisors; conversely, if a number has an odd number of divisors, it is a square. How can we show this?

Just studying the number of divisors doesn’t reveal any pattern. I shall make a great leap here, and go immediately to a description of the Prime Factors Exponent Numbers (PFENs). Kenneth Iverson’s book *Algebra, an Algorithmic Treatment* (Addison-Wesley, Menlo Park, California, 1972), gives a description of this system in section 16.2. In this number system a positive integer is represented by a list of non-negative integers, where the integer in column i represents the exponent to which prime i is to be raised, going from left to right. The primes corresponding to indices 0 1 2 3 4 5 are 2 3 5 7 11 13. For example, if, in the PFEN for a number, column 3 has the value 5, the third prime, 7, is to be raised to the 5th power, and so 0 0 0 5 represents 16807; the decimal integer corresponding to the PFEN is the product of the results of raising each prime to the corresponding power. The PFEN forms for the first 17 positive integers are:

```
 n   PFEN n
 1
 2   1
 3   0 1
 3   2
 5   0 0 1
 6   1 1
 7   0 0 0 1
 8   3
 9   0 2
10   1 0 1
11   0 0 0 0 1
12   2 1
13   0 0 0 0 0 1
14   1 0 0 1
15   0 1 1
16   4
17   0 0 0 0 0 0 1
```

The PFEN for `12` is `2 1`, signifying that the decimal number corresponding can be obtained by `*/2 3^2 1`, that is, `*/4 3`, or `12`. The PFEN for 1 is the empty list, since 1 has no prime components; its value is the product over the empty list, or 1. A J verb for producing the PFEN corresponding to a decimal number is given by:

```
   pfd =: _&q:    NB. PFEN from decimal
   pfd 300
2 1 2
   pfd 16807
0 0 0 5
```

The first example shows a number having prime factors 2, 3, and 5 with exponents 2, 1, and 2. The second example shows a number having prime factors 2, 3, 5, and 7 with exponents 0, 0, 0, and 5. Since any integer to the zero power is 1, any number of zero exponents do not alter the value of the product.

I have to be reminded from time to time that J has built-in inverses for a great many verbs; I was plodding through defining dfp, the verb inverse to pfd, the hard way late one night, and woke up the next morning to the realization that all I needed was the inverse adverb (`^:_1`).

```
   dfp =: pfd^:_1   NB. decimal from PFEN
   dfp 2 1 2
300
   dfp 0 0 0 5
16807
```

There’s no need for the user to know how an inverse works – it is enough merely to accept the presence of an inverse as a blessing. If you *do* want to know, however, you can see what the inverse looks like in detail by using the *basic* conjunction `b.` as follows:

```
   pfd b. _1
(p:@i.@# */ .^ ])"1 :.(_&q:)
```

Studying this shows that the inverse function works by taking the inner product, with product (\*/) as the left verb and power (^) as the right verb, that is (\*/ . ^), applied between a list of primes (p:@i.@#) on the left and a conforming list of exponents (\])on the right.

The PFEN numbers make the calculation of the product of two numbers easy. To multiply two numbers, add their PFENs. Thus:

```
   72 * 90
6480
   pfd 72 90
3 2 0
1 2 1
   3 2 0 + 1 2 1
4 4 1
   dfp 4 4 1
6480
```

A verb to multiply PFENs can be defined:

```
   multp=:+/@,:
   3 2 0 multp 1 2 1
4 4 1
```

We’re almost at the point of being able to show why squares have an odd number of divisors. One more detail is wanting, and that is, how to determine the number of divisors of a number. We could count the number of integers not greater than a given integer that have a give a residue of zero for that integer. To find that there are four divisors of eight, for example, one could write:

```
   i =: >:@i.
   i 8
1 2 3 4 5 6 7 8
   (i 8)|8
0 0 2 0 3 2 1 0
   0=(i 8)|8
1 1 0 1 0 0 0 1
   +/0=(i 8)|8
4
```

This method becomes unwieldy for large integers. A more compact method would be welcome.

If a number n has prime factors with exponents e, any number which is a divisor of n will have the same prime factors, with exponents which are less than or equal to e. Here I use `q:` with negative infinity as left argument. This gives another representation of an integer, where only the primes which have an exponent greater than zero are given, together with their positive exponents:

```
   pfd2 =: __&q:
   pfd2 666
2 3 37
1 2  1
```

This signifies that 666 is composed of `(2^1)*(3^2)*(37^1)`, or `2*9*37`.

The inverse is given by:

```
   dfp2 =: pfd2^:_1
   dfp2 pfd2 666
666
```

We can enumerate the divisors of 666 by taking all combinations of `1 2` with `1 3 9` with `1 37`. A divisor will thus be one of the numbers generated as follows:

```
   (2^i.2)*/(3^i.3)*/(37^i.2)
 1  37
 3 111
 9 333
 2  74
 6 222
18 666
```

This array shows all the divisors. There are twelve altogether, and the twelve comes from the product over one plus the exponent of each prime: `*/ 1 + 1 2 1`, or `*/2 3 2`, or `12`. Thus we can compute the number of divisors of a number by taking its PFEN, adding one to it, and taking the product:

```
   */ >: 1 2 1
12
```

All the powers that each prime in the composition of the number can take will be given by a table such as the one below, essentially all the numbers in the radix given by `1 + PFEN n` :

```
   pfx =: ] #: [: i. */
   pfx 1 + 1 2 1
0 0 0
0 0 1
0 1 0
0 1 1
0 2 0
0 2 1
1 0 0
1 0 1
1 1 0
1 1 1
1 2 0
1 2 1
   2 3 37 ^"1 pfx 1 + 1 2 1
1 1  1
1 1 37
1 3  1
1 3 37
1 9  1
1 9 37
2 1  1
2 1 37
2 3  1
2 3 37
2 9  1
2 9 37
   */ |: 2 3 37 ^"1 pfx 1 + 1 2 1
1 37 3 111 9 333 2 74 6 222 18 666
```

But we’ve gone a step too far; we don’t need or want the values of the divisors, merely how many divisors there are. In reaching this point, however, we’ve found out how to arrive at this number: take the product over one plus the PFEN. We define the square verb `squrp2` and the number of divisors verb `nd2`:

```
   squrp2 =: 1 2 * ]
   pfd2 666
2 3 37
1 2  1
   squrp2 pfd2 666
2 3 37
2 4  2
   dfp2 squrp2 pfd2 666
443556
   *: 666
443556
   nd2 =: 13 : '*/ >: {: y.'
   nd2
[: */ [: >: {:
   nd2 pfd2 666
12
```

We see that 666 has 12 divisors. How many does its square have?

```
   nd2 squrp2 pfd2 666
45
```

So the square of 666 (443556) has an odd number of divisors. Perhaps now we can see why. If we take any number, having any arbitrary PFENa, consisting of some mixture of odd and even numbers (zero is even), and add it to itself, (thus producing a square) we’ll obtain a PFENb in which *all* the numbers are even, since even plus even is even (2n+2n is 4n, an even number), and odd plus odd is even (2n+1 + 2n+1 is 4n+2, an even number), and it is the square of the number given by PFENa. If we take the PFEN of 443556, or 2 4 2, and add 1 to each exponent, giving 3 5 3, we have a list of all odd numbers; taking the product of this, to yield the number of divisors of the square number PFENb, gives 45, an odd result (the only way a product can be odd is if *all* of the multiplicands are odd – any even multiplicand means the product will be even), so that the square PFENb has an odd number of divisors. This is a completely general result, and means that all square numbers, and only square numbers, have an odd number of divisors.

Amy only tested the first dozen or so numbers. I wrote a set of J verbs to allow testing all thousand locker numbers:

```
   i =: integers      =: [: >: i. NB. positive integers thru y.
   z =: gauge         =: = i NB. gauge 4 is 0 0 0 1, etc.
   c =: cycles        =: $ z NB. recycle gauge y. times
   t =: table         =: c"0 i  NB. make square table
   tk=:table 1000
   $tk
1000 1000
   m19=: +/{.\: NB. locate 1s in Boolean list
   dc=:+/tk
   $dc
1000
   17{.dc
1 2 2 3 2 4 2 4 3 4 2 6 2 4 4 5 2
   p1=:>: m19 2|dc  NB. lockers open; should all be squares
   $p1
31
   p1
1 4 9 16 25 36 49 64 81 100 121 144 169 196 225 256 289 324 361 400 441 484 529 576 625 676 729 784 841 900 961
   sqdc=: nd2"2 pfd2"0 p1  NB. squares divisor count; all should be odd
   sqdc
1 3 3 5 3 9 3 7 5 9 3 15 3 9 9 9 3 15 3 15 9 9 3 21 5 9 7 15 3 27 3
```

Here are the rest of the questions Amy had to answer. See if you can answer them.

<ol type="A">
	<li>What do you notice about the lockers that were touched by exactly two students? (Try <tt>m19 &gt;: 2=dc</tt>)
	</li><li>What do you notice about the lockers that were touched by exactly three students?
	</li><li>What do you notice about the lockers that were touched by exactly four students?
	</li><li>What was the first locker touched by both student 6 and student 8?
	</li><li>What do you notice about the student numbers of the students that touched both locker 24 and locker 36?
	</li><li>Which students touched both locker 100 and 120? What do you notice about their student numbers?
</li></ol>

***
