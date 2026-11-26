---
vid: '10011160'
title: J Solution to Enigma 685
authors:
- Dave Ziemann
byline: by Dave Ziemann
volume: '10'
issue: '2'
page: '119'
source: trad/v102/ziemann102_119.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- competitions and puzzles
- programming techniques
---

by Dave Ziemann <span class="affil"></span> <span class="email"></span>
{ .byline }

### The Problem

The problem, set by Keith Austin, is as follows:

I came upon a clearing in the jungle. At the centre was a circular array of plates, some with food (F) and some empty (E). The array is shown in the diagram with the plates numbered for convenience:

```
                 11   E    F   1

               10   F        F   2

               9   F          E   3

                8   E        E   4

                 7   F     F   5

                        F
                        6
```

My guide explained that the plates were for a food-and-fast ceremony over 11 days. The person fasting would select a food plate and eat its contents on the first day; for the second day, (s)he would move clockwise to the next plate and eat its contents, and so on; the first plate had to be a food plate but on some later days there would be empty plates which would produce fast days. The person fasting had to choose a starting plate so that at the end of each of the 11 days more food plates had been used than empty plates. For example, plate 1 is not a starting place as, at the end of day 4, 2 food and 2 empty plates would have been used. However plate 5 is a starting plate.

### Question 1: Which plates are starting plates?

I then moved to a second clearing which again contained a circular array of plates for a 47-day ceremony. As I went round the plates were:

```
 1  2  3  4  5  6  7  8  9 10 11 12
 F  F  E  E  E  E  F  F  F  F  E  F

13 14 15 16 17 18 19 20 21 22 23 24
 E  F  E  F  E  F  F  F  F  E  E  E

25 26 27 28 29 30 31 32 33 34 35 36
 F  F  E  E  F  F  E  E  F  E  F  F

37 38 39 40 41 42 43 44 45 46 47
 E  F  E  F  F  F  F  E  F  F  E
```

### Question 2: Which plates are starting plates in the second clearing?

I then moved to a third clearing which had a circular array of 45 plates which were the same as plates 1 to 45 in the second clearing. These plates were for a 45-day ceremony.

### Question 3: Which plates are starting plates in the third clearing?

I then moved to a fourth clearing which had a circular array of 150 food plates and 140 empty plates; I forgot the order they were in. These plates were for a 290-day ceremony.

### Question 4: How many starting plates were there in the fourth clearing?

### The Solution

We can start be defining the array of 11 plates as a boolean list in J:

```
      p11=. 1 1 0 0 1 1 1 0 1 1 0
```

We want to be able to consider each plate in this list as a potential starting plate. So we can generate a table where each row contains p11 successively left-shifted by one place further, so that each place in turn acts as a starting plate. The fork as produces all the possible shifts of its argument list:

```
      as=. i.@#|.]            NB. all shifts (monad)
      as
+-------------+
|+------+||.|]|
||i.|@|#||  | |
|+------+|  | |
+-------------+
      as p11
1 1 0 0 1 1 1 0 1 1 0
1 0 0 1 1 1 0 1 1 0 1
0 0 1 1 1 0 1 1 0 1 1
0 1 1 1 0 1 1 0 1 1 0
1 1 1 0 1 1 0 1 1 0 0
1 1 0 1 1 0 1 1 0 0 1
1 0 1 1 0 1 1 0 0 1 1
0 1 1 0 1 1 0 0 1 1 1
1 1 0 1 1 0 0 1 1 1 0
1 0 1 1 0 0 1 1 1 0 1
0 1 1 0 0 1 1 1 0 1 1
```

By default, J performs operations along the leading axis of an array. In what follows it will therefore be convenient to hold plate vectors as columns rather than rows. In this case a transpose is not necessary because the matrix is symmetrical.

A plate is only a starting plate if, for each and every day, the cumulative number of Food plates is greater than the cumulative number of Empty plates. We can build a monad called cumas which performs this cumulation as follows:

```
      cum=. +/\               NB. cumulate (monad)
      cumas=. cum@as          NB. cumulate all shifts

      cumas p11
1 1 0 0 1 1 1 0 1 1 0
2 1 0 1 2 2 1 1 2 1 1
2 1 1 2 3 2 2 2 2 2 2
2 2 2 3 3 3 3 2 3 3 2
3 3 3 3 4 4 3 3 4 3 2
4 4 3 4 5 4 4 4 4 3 3
5 4 4 5 5 5 5 4 4 4 4
5 5 5 5 6 6 5 4 5 5 5
6 6 5 6 7 6 5 5 6 6 5
7 6 6 7 7 6 6 6 7 6 6
7 7 7 7 7 7 7 7 7 7 7
```

Now we have to apply this verb first to the list p11 (which indicates the Food plates), and then to the negation of p11 (which indicates the Empty plates), and then ensure that the former exceeds the latter.

This suggests the use of a fork, as follows:

```
      mfptep=. cumas>cumas@-. NB. more food than empty plates
      mfptep
+--------------------+
|cumas|>|+----------+|
|     | ||cumas|@|-.||
|     | |+----------+|
+--------------------+
      mfptep p11
1 1 0 0 1 1 1 0 1 1 0
1 0 0 0 1 1 0 0 1 0 0
1 0 0 1 1 1 1 1 1 1 1
0 0 0 1 1 1 1 0 1 1 0
1 1 1 1 1 1 1 1 1 1 0
1 1 0 1 1 1 1 1 1 0 0
1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 0 1 1 1
1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1
```

The “and-insert” along the leading axis then reveals if each plate is a starting plate or not:

```
      isasp=. *./@mfptep      NB. is a starting plate?
      isasp
+---------------+
|+----+|@|mfptep|
||*.|/|| |      |
|+----+| |      |
+---------------+

     isasp p11
0 0 0 0 1 1 0 0 1 0 0
```

The numbers of the starting plates can be found by incrementing the zero-origin result of the utility verb where, which produces indices from a boolean list:

```
      where=. #i.@#         NB. indices of boolean list (monad)
      sp=. >:@where@isasp   NB. starting plates
      sp p11
5 6 9
```

And hence Question 1 is answered.

Questions 2 and 3 can now also be answered by application of this verb to similar boolean lists:

```
      p47=. 1 1 0 0 0 0 1 1 1 1 0 1 0 1 0 1 0 1 1 1 1 0 0 0 1 1 0 0 1 1 0 0 1 0 1 1 0 1 0 1 1 1 1 0 1 1 0
      p45=. 45{.p47
      sp p47
7 8 9 18 35 40 41
      sp p45
7 8 9 18 35 40 41
```

Changing the order of the Food and Empty plates in p11 will affect which plates are starting plates, although we may surmise that it will not affect the number of starting plates. We can test this by counting the number of starting plates in a series of random permutations of the list p11, by using the very useful verb A. (atomic permute) as follows:

```
      (i.6) A. 'abc'     NB. atomic permute example
abc
acb
bac
bca
cab
cba

      (?!11) A. p11
1 1 1 0 1 0 1 1 1 0 0
      (?!11) A. p11
1 1 1 0 0 1 1 0 1 0 1
      (?!11) A. p11
1 1 0 1 1 0 1 0 1 1 0
      # sp (?!11) A. p11
3
      # sp (?!11) A. p11
3
      # sp (?!11) A. p11
3
```

It would seem that that the number of starting plates depends only upon the total number of Food and Empty plates in any given food-and-fast ceremony. We can therefore construct what we might call a “typical ceremony” to represent a ceremony of a given size.

The dyad tc takes the number of Food plates and the number of Empty plates as its respective arguments:

```
      tc=. ,#1 0"_            NB. typical ceremony
      +/p11
7
      7 tc 4
1 1 1 1 1 1 1 0 0 0 0
```

The verb is constructed from a fork whose three tines are , (append items), # (copy) and 1 0"\_ (a verb which always derives the constant 1 0). We can count the number of starting plates in a typical ceremony with the verb nsp, which takes similar arguments:

```
      nsp=. #@sp@tc           NB. number of starting plates
      7 nsp 4
3
      +/p47
27
      27 nsp 20
7
      150 nsp 140
10
```

Which is the answer to question 4. The number of starting plates in any ceremony would seem to be equal to the total number of Food plates minus the total number of Empty plates.

### Proof

The following proof of this was given to me by Paul Chapman.

Consider an arbitrary ceremony containing any number of Food and Empty plates. There are three possibilities; all the plates are Empty plates, all the plates are Food plates, or there are both Empty plates and Food plates. In the first case there are no starting plates. In the second case all the plates are starting plates.

In the last case, there must be a pair of plates in the sequence Food, Empty. The Empty plate cannot be a starting plate (no Empty plate ever can). The Food plate also cannot be a starting plate, because by the end of the second day the total number of Food plates will not exceed the total number of Empty plates. As neither plate in this pair can be a starting plate, the sequence does not contribute to the total number of starting plates, and so it can be removed from the ceremony without affecting the result. Eliminating all such pairs will eventually result in a complete set of Empty plates or a complete set of Food plates, from which the number of starting plates is immediately determined.

(webpage generated: 6 December 2005, 19:53)
