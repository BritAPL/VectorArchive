---
vid: '10005390'
title: 'At Play with J: Tacit Definition'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '10'
issue: '3'
page: '100'
source: trad/v103/mcdonnell103_100.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- language design
- programming techniques
---

by Eugene McDonnell <span class="affil"></span> <span class="email"></span>
{ .byline }

You may be among the readers of Vector whose stomachs start churning or whose eyes glaze over when they read the words “tacit definition” in articles about J. This column is meant for you. It is going to take the approach that you know how to write an APL function, and might not be averse to learning how to write a J function in a similar fashion.

The example I shall use is fairly short – just ten lines – and may possibly even be of use to some of you. It was shown to me by Joey Tuttle many years ago. It’s called “factors” and it factors a positive integer `n`. The result of factors `n` is an ordered list of primes such that `n` is the product over the list, that is, `n = */ factors n`. It was able to find the factors of the 18-digit number `(2^59)- 1` (576460752303423487), which are 179951 and 3203431780337, in 3 minutes and 37.1 seconds on a Macintosh Quadra 700.

Various methods of finding the prime factors of a number are given in Knuth, Seminumerical Algorithms, Section 4.5.4 “Factoring into primes”, pp. 338-360. The method used here is the simplest one, Algorithm A, which divides the number by increasingly larger primes until all factors have been found, but the method has been vectorized so that more than one factor may be found at once.

The argument must be a positive integer not greater than `max`, where `max` is a number derived from the floating-point characteristics of your computer. This discussion assumes that your computer uses IEEE floating point arithmetic, which is the case for PCs, Macintoshes, and most Unix machines. On PCs and Unix machines the maximum is the 16-digit `(2^53)-1` (9007199254740991). For Macintoshes it is the 19-digit `(2^63)-1` (9223372036854775807), or 1024 times as large. These are the largest integers which can be represented to full accuracy on those machines without special programming.

First I’ll show the function definition, then give explanations of its lines, paying special attention to those things that differ from APL.

### Function Definition

Let’s assume for the moment that you created the function text, called “text”, below. It could be in the form of a character matrix, a character string delimited by carriage returns, or a vector of boxed character vectors.

```
0.    f =. i. 0
1.    t =. 2 3 5 7 11 13 17 19 23 29 31 37
2.    o =. +/\ 432 $ 4 2 4 2 4 6 2 6
3. L) m =. (-. * t |!.0 y.) # t
4.    f =. f , m
5.    y. =. y. % */ m
6.    $. =. > (* # m) { $. ; L
7.    t =. o + {: t
8.    $. =. > (y. >: *: {. t) { $. ; L
9.    f =. /:~ f , y. -. 1
```

The line numbers are merely for reference. They are not part of the function.

The first thing to notice is that there is no header line, so it would seem that we can’t tell whether there is an explicit result or not, nor what its name is if there is one, nor what the function name is, nor what its valence is, nor what the argument is named, nor which variables are local.

Here is how each of these questions is resolved:

**Explicit result**. Every J function has an explicit result, and it is the value of the last expression executed.

**Function name**. In J a function is named the same way that a variable is named, by assignment, denoted by (`=.`). However, we can’t just write `factors =. text` because all that this would do would be to create another variable named “factors” with the same value as “text”. We have to use the “definition” operator, symbolized by the colon (`:`), whose result is a function, not a variable. Our function is monadic, so to give it a name we write `factors =. text : ''` and this creates the function named “factors”.

**Valence**. For an ambivalent function, the monadic case is given by the left argument to (`:`), and the dyadic case is given by the right argument. If the function is monadic, as in our case, the right argument is empty; if it is dyadic, the left argument is empty. There is no such thing as a niladic function in J.

**Argument name**. By convention, the argument to a monadic function is (y.). In a dyadic function the left argument is named (`x.`) and the right argument is named (`y.`).

**Local variables**. J makes a distinction between local assignment (`=.`) and global assignment (`=:`). This removes the need for a list of local names. The rule is that on the first assignment of a name using (`=.`) the name is made local.

### Here beginneth the detailed description of the function’s lines:

**Line 0** sets the initial factor list to empty. J’s iota is denoted by (`i.`). By the way, if the argument is 1, this will be the result as well, since 1 is not a prime and has no prime factors. However, it will still be true that `n = */` factors `n`, since the product over an empty vector is `1`.

**Line 1** creates as the initial list of trial divisors the first 12 primes. This enables the function to use just one iteration for all arguments less than 1370 (`37^2` is 1369).

**Line 2** forms the list of offsets to be used in creating a new list of trial divisors, after there are no more values left in the current list that divide the current value of the number being factored. In J, the scan operator is defined differently from the APL scan operator. In APL the scan operator implies the reduction operator. In J the function argument to scan is monadic, not dyadic, so that one has to use “sum” (`+/`) not “add” (`+`) if we want the sum scan.

There are 432 items in this vector: 54 repeats of the eight items `4 2 4 2 4 6 2 6`. The purpose of this vector is to remove multiples of 2 3 5 from consideration as trial divisors, since these can’t be primes. For example,

```
37 + +/\ 4 2 4 2 4 6 2 6 ...
41 43 47 49 53 59 61 67 ...
```

Some of the items in these new trial divisors will not be primes. In the list above 49 is composite. Eliminating multiples of 2 3 5 reduces the number of trial divisors by over 73%.

**Line 3** looks as if it begins with a label, but the label name is followed by a right parenthesis instead of a colon. This is because the colon is used for function definition, as described above, and so J uses the right parenthesis “`)`” to separate the label name from the instruction. A label in J is a vector. The value of label “`L`” is `3 4 5 6 7 8 9`; that is, it is a vector of instruction numbers, beginning with the number of the instruction in which it appears, and followed by each of the subsequent instruction numbers. We’ll see the use of this when we discuss instructions 6 and 8.

This line begins the iteration, and forms the vector `m` of newly-found factors of the argument. The factors are found by using the vector of trial divisors of the argument as the left argument to the residue function. However, the residue function in J is fuzzed, just as it is in some APLs, in order to permit proper-looking results when used with near-integers, and also to permit the use of decimal rational numbers as arguments to residue. We are dealing with exact integer arguments, so in order to extend the domain of the residue function we require that it not be fuzzed. To accomplish this in APL systems, the comparison tolerance system variable, `⎕ct`, is set to 0. In J this is done by explicitly modifying the residue function with the “fit” or “customize” operator (`!.`) using 0 as right argument. Thus, instead of writing `t | y`. We write `t | !. 0 y`. and this allows us to work with a residue that has zero fuzz.

Curiously, the “*Dictionary of J*”, which says that the “fit” operator “modifies certain verbs in ways prescribed in their definitions”, doesn’t describe this use of it with respect to residue. Tsk tsk. The advantage of having the modification of the verb occur in direct connection with its use is obvious: one doesn’t have to remember whether or not `⎕ct` has been modified, nor run into the hazard of not restoring it when it should be. The use is direct and immediate, and applies only to the function in question. Furthermore, anyone reading the function knows immediately that it is unfuzzed.

After finding the residues, their signum (`*`) is taken, yielding a vector of 0s and 1s, and these are negated (`-.`) complementing them to 1s and 0s. This boolean vector is used to select (`#`) the corresponding items from `t`, giving in `m` the new factors to be appended to the result. In APL there has always been a confusion about slash: is it an operator or a function? If we write `+/1 2 3` it acts like an operator, but when we write `1 0 1/1 2 3` it acts like a function. In J the “`#`” function is used for the functional case, as in `1 0 1#1 2 3`.

**Line 4** appends the new factors to the result vector.

**Line 5** factors the argument, by dividing (`%`) it by the product (`*/`) of the new factors, thus diminishing it.

**Line 6** is a branch instruction. There is no branch arrow in J, however. What takes its place is a variable called “suite” and denoted by (`$.`). This variable determines the sequence in which instructions of a defined function are executed. At the beginning of execution of a defined function, suite is set to a vector of the instruction numbers in the defined function. In our factors program, which has ten instructions, it would have the initial value `0 1 2 3 4 5 6 7 8 9`. At the beginning of execution of each instruction, the leading item of (`$.`) is removed. Thus, if a five instruction program were written in which each instruction displayed the current value of (`$.`), the initial value of (`$.`) would be `0 1 2 3 4`, and the display of each instruction would be:

```
Instruction     0      1 2 3 4
     1      2 3 4
     2      3 4
     3      4
     4      (empty)
```

The expression at the end of line 6 (`$. ; L`) forms a two-item vector of boxes, using the link (`;`) function. The head of the vector is the boxed current value of suite, and its tail is the boxed vector `L`. Selection by index (`x { y` means select item `x` of `y`) is used to choose one of them, and the one chosen is opened (`>`). The signum (`*`) of the number of (`#`) factors in `m` is determined. This will be 0 if there are no new factors, and 1 otherwise. If there are no new factors, suite is reassigned to itself – effectively a fall-through. If there are new factors, `L` is assigned to suite, causing a branch to line `L`. Thus, if there were any factors in the current vector of trial divisors, we may not have finished with it, and return to `L` to try again with it. If there were none, we are through with the current vector, and fall through to the next instruction.

**Line 7** forms a new vector of trial divisors, by adding the offset vector to the last (`{:`) item of the current vector.

**Line 8** is another branch instruction. This one causes control to be returned to instruction `L` if the reduced argument is greater than or equal to (`>:`) the square (`*:`) of the first item (`{.`) of the new vector of trial divisors, since in this case there may be more factors. If it is less, this means that there can’t be any new factors in the reduced argument, and thus it is either 1 or a prime.

**Line 9** removes `1` from the argument (the result of `x -. y` is `x` with items equal to `y` removed; `17 -. 1` is 17; `1 -. 1` is empty). This leaves it unaltered if it is a prime, or makes it empty otherwise. It then appends the argument to the list of factors (essentially doing nothing if it had been 1), sorts (`/:~`) the list into ascending order, and terminates. In J the semantics of dyadic upgrade and downgrade have been changed. They no longer have the significance of using the left argument as a collating sequence to produce a grade with reference to it. Instead, they are used to sort the left argument into an order specified by the right argument. The definition of dyadic upgrade (`x /: y`) is `(/:y) { x`; that is, the permutation that puts `y` in ascending order is used to permute the items of `x`.

The most frequent use of these sort verbs is with the left and right arguments identical, in which case the result is the sorted argument, either ascending or descending. The reflexive operator (`~`) applies to a monadic verb to produce a dyadic verb with left argument the same as the right argument. For example, `+~1.2` is 2.4, and `/:~ 2 7 1 8 2 8` is `1 2 2 7 8 8`.

Below are some examples of the use of the factors function with some ten-digit numbers to give you some idea of how it behaves. Notice that the time to factor a number is longest when the argument is a prime, and fairly long also when there are two factors roughly equal to the square root of the argument. The comment symbol in J is (`NB.`).

```
time =. 6!:2 NB. yields seconds to execute its string argument
fmt=.":!.20 NB. formats (":) numbers to 20 places (!.20)

time 'k=.factors 6307059899'
1.11667
fmt k
7 19 47421503

time 'k=.factors 6307059901'
0.85
fmt k
379 16641319

time 'k=.factors 6307059903'
0.983333
fmt k
3 127 3461 4783

time 'k=.factors 6307059907'
0.65
fmt k
1201 5251507

time 'k=.factors 6307059909'
3.51667
fmt k
3 24749 84947

time 'k=.factors 6307059911'
10.0167
fmt k
6307059911
```

(webpage generated: 18 February 2006, 02:17)
