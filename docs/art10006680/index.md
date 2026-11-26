---
vid: '10006680'
title: Solving the “Now I Know” Problem in J
authors:
- Howard Peelle
byline: by Howard A. Peelle (hapeelle@educ.umass.edu)
volume: '20'
issue: '2'
page: '92'
source: trad/v202/peell202.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2003
tags:
- competitions and puzzles
- programming techniques
---

*by Howard A. Peelle (hapeelle@educ.umass.edu)*
{ .byline }

A problem presented in the previous issue, Vector 20.1, is analysed here using J.

## Problem

“Now I Know”

> There are two positive integers, each greater than 1 and less than 100. Pete knows their product. Serena knows their sum. First, Pete says: “I don’t know what they are.” Then Serena says: “I know you don’t know.” Then Pete says: “Now I know.” And Serena says: “So do I.” What are the two integers?

(The allusion is to Pete Sampras and Serena Williams, tennis champions who both won the U.S. Open in 2002.)

## Analysis

Imagine the following thinking by Pete (P) and Serena (S) using J:

P: I’ll start with a list of possible integers:

```
n =: 2 + i.98                    n is 2 plus integers 0 to 97
n                                Show n
  2 3 4 5 6 7 8 9 ... 99         List of 98 integers
```

Serena’s sum could be any integer in n plus any integer in n. Here is a table of all possible sums:

```
s =: n +/ n                           s is each n plus each n
s                                     Show s (a 98 by 98 table)
   4   5   6   7   8   9  10  11 ... 101
   5   6   7   8   9  10  11  12 ... 102
   6   7   8   9  10  11  12  13 ... 103
   7   8   9  10  11  12  13  14 ... 104
   8   9  10  11  12  13  14  15 ... 105
   9  10  11  12  13  14  15  16 ... 106
  10  11  12  13  14  15  16  17 ... 107
  11  12  13  14  15  16  17  18 ... 108
   .                                  .
   .                                  .
   .                                  .
 101 102 103 104 105 106 107 108 ... 198
```

I know Serena’s sum is one of these sums.

> Note: Table s is symmetrical about the main diagonal due to the commutativity of addition, so there are duplicate sums in the bottom left half of the table. Repeats appear along reverse diagonals.

S: Pete’s product could be any integer in n times any integer in n. Here is a table of all possible products:

```
p =: n */ n                           p is each n times each n
p                                     Show p (a 98 by 98 table)
   4   6   8  10  12  14  16  18 ... 198
   6   9  12  15  18  21  24  27 ... 297
   8  12  16  20  24  28  32  36 ... 396
  10  15  20  25  30  35  40  45 ... 495
  12  18  24  30  36  42  48  54 ... 594
  14  21  28  35  42  49  56  63 ... 693
  16  24  32  40  48  56  64  72 ... 792
  18  27  36  45  54  63  72  81 ... 891
   .                                  .
   .                                  .
   .                                  .
 198 297 396 495 594 693 792 891 ...9801
```

I know Pete’s product is one of these products.

> Note: Table `p` is symmetrical about the main diagonal due to the commutativity of multiplication, so there are duplicate products in reverse diagonals.

S: My goal is to eliminate products in `p` corresponding to repeats of my sum (which appear on a certain diagonal of `s`), hopefully leaving only one possible product (and its duplicate). Then, once I know Pete’s product, I can determine the two addends of my sum.

P: My goal is to eliminate sums in s corresponding to repeats of my product which appear in different diagonals in `p`, hopefully leaving only one possible sum (and its duplicate). Then, once I know Serena’s sum, I can determine the two factors of my product.

P: Unfortunately, I can’t announce the answers right away because I know my product cannot be factored into two unique factors. Indeed, it appears repeatedly in `p`. Since I can’t determine Serena’s sum now, I must say “I don’t know”.

S: If Pete doesn’t know the answers, his product must have three or more prime factors. So I can eliminate from `p` all possible products of two primes:

```
primes =: p: i.25                List of primes < 100
primes
  2 3 5 7 11 13 17 19 23 29 31 ... 97
pp =: primes */ primes           Table of products of primes
pp                               (25 by 25 table)
    4   6  10  14  22  26 ...  194
    6   9  15  21  33  39 ...  291
   10  15  25  35  55  65 ...  485
   14  21  35  49  77  91 ...  679
   22  33  55  77 121 143 ... 1067
   26  39  65  91 143 169 ... 1261
    .                           .
    .                           .
    .                           .
 194 291 485 679 1067 1261 ...9409
zero =: -. p e. ,pp                   0 where p in pp, else 1
zero                                  (98 by 98 table)
   0   0   1   0   1   0   1   1 ... 1
   0   0   1   0   1   0   1   1 ... 1
   1   1   1   1   1   1   1   1 ... 1
   0   0   1   0   1   0   1   1 ... 1
   1   1   1   1   1   1   1   1 ... 1
   0   0   1   0   1   0   1   1 ... 1
   1   1   1   1   1   1   1   1 ... 1
   1   1   1   1   1   1   1   1 ... 1
   .                                 .
   .                                 .
   .                                 .
   1   1   1   1   1   1   1   1 ... 1
```

Alternatively: `zero =: 2 < #@q: p`

```
p =: p * zero                  p times zero (item by item)
p                                 Updated p
   0   0   8   0  12   0  16  18 ... 198
   0   0  12   0  18   0  24  27 ... 297
   8  12  16  20  24  28  32  36 ... 396
   0   0  20   0  30   0  40  45 ... 495
  12  18  24  30  36  42  48  54 ... 594
   0   0  28   0  42   0  56  63 ... 693
  16  24  32  40  48  56  64  72 ... 792
  18  27  36  45  54  63  72  81 ... 891
   .                                  .
   .                                  .
   .                                  .
 198 297 396 495 594 693 792 891 ...9801
```

This zeros out all products of two primes.

> Note: Cubes of primes have two unique factors and could be eliminated similarly. (See [Appendix](#app))

P: Following Serena’s thinking (above), I can also zero out the corresponding possible sums in my table s:

```
s =: s * zero                       s is s times zero
s                                 Updated s
   0   0   6   0   8   0  10  11 ... 101
   0   0   7   0   9   0  11  12 ... 102
   6   7   8   9  10  11  12  13 ... 103
   0   0   9   0  11   0  13  14 ... 104
   8   9  10  11  12  13  14  15 ... 105
   0   0  11   0  13   0  15  16 ... 106
  10  11  12  13  14  15  16  17 ... 107
  11  12  13  14  15  16  17  18 ... 108
   .                                  .
   .                                  .
   .                                  .
 101 102 103 104 105 106 107 108 ... 198
```

P: Now, is there just one sum corresponding to my product? Unfortunately, no. I know there are several possible sums remaining. So I cannot announce the answers.

> Note: If Pete does not know the answers at this point, we could look for unique sums (such as 6) in `s` and eliminate them. Similarly, if Serena does not know the answers, we could look for unique products (such as 8) in p and eliminate them. This could be repeated until there is no change in `p` and `s`. (See [Appendix](#app)) But this may not be necessary.

S: Is there just one product corresponding to my sum? No. I know there are several possible products remaining in `p` along the corresponding diagonal in `s` with my sum. So I cannot announce the answers. Actually, the diagonal in `p` corresponding to my sum has *all* possible products remaining. And there are several of these “full”diagonals (a few are shown here with dots as background):

```
   .  .  .  .  .  .  . 18  .  .  .  .  . 30  .  .  .  .  . 42  .  .  . 50  .  .  .
   .  .  .  .  .  . 24  .  .  .  .  . 42  .  .  .  .  . 60  .  .  . 72
   .  .  .  .  . 28  .  .  .  .  . 52  .  .  .  .  . 76  .  .  . 92
   .  .  .  . 30  .  .  .  .  . 60  .  .  .  .  . 90  .  .  . 110
   .  .  . 30  .  .  .  .  . 66  .  .  .  .  . 102 .  .  . 126
   .  . 28  .  .  .  .  . 70  .  .  .  .  . 112 .  .  . 140
   . 24  .  .  .  .  . 72  .  .  .  .  . 120 .  .  . 152
  18  .  .  .  .  . 72  .  .  .  .  . 126 .  .  . 162
   .  .  .  .  . 70  .  .  .  .  . 130 .  .  . 170
   .  .  .  . 66  .  .  .  .  . 132 .  .  . 176
   .  .  . 60  .  .  .  .  . 132 .  .  . 180
   .  . 52  .  .  .  .  . 130 .  .  . 182
   . 42  .  .  .  .  . 126 .  .  . 182 .
  30  .  .  .  .  . 120 .  .  . 180       .
   .  .  .  .  . 112 .  .  . 176             .
   .  .  .  . 102 .  .  . 170
   .  .  . 90  .  .  . 162
   .  . 76  .  .  . 152
   . 60  .  .  . 140
  42  .  .  . 126
   .  .  . 110
   .  . 92
   . 72
   50
    .
    .
    .
```

So I can say “I know you don’t know".

P: If Serena doesn’t know the answers, from looking at her p, all possible products in the corresponding diagonal in s containing her sum must be factorable into primes in different ways. So, I can eliminate all diagonals containing a 0. Now I know that her sum is one with a corresponding “full”diagonal in s: 11, 17, 23, 27, 29, 35, 37, 41, 47... . (The first few are shown here with dots as background.)

```
   .  .  .  .  .  .  . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .  .  .
   .  .  .  .  .  . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  .  .  .  . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  .  .  . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  .  . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   . 11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
  11  .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  .  . 17  .  .  .  .  . 23  .  .  . 27  .
   .  . 17  .  .  .  .  . 23  .  .  . 27  .
   . 17  .  .  .  .  . 23  .  .  . 27  .
  17  .  .  .  .  . 23  .  .  . 27  .  .
   .  .  .  .  . 23  .  .  . 27  .        .
   .  .  .  . 23  .  .  . 27  .              .
   .  .  . 23  .  .  . 27  .
   .  . 23  .  .  . 27  .
   . 23  .  .  . 27  .
  23  .  .  . 27  .
   .  .  . 27  .
   .  . 27  .
   . 27  .
  27  .
   .
   .
   .
```

Actually, my product has two prime factors which sum to only one of these sums in `s` So, I can say “Now I know".

S: If Pete knows the answers, his product cannot have two different prime factorings. His product must appear on only one full diagonal in `p`. So I can eliminate multi-factorable products which appear in more than one full diagonal: 30, 42, 60, 66, 70, 72, etc. Finally, that leaves only one product with my sum alone on a diagonal of `p`:

```
   .  .  .  .  .  .  . 18  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 50 .
   .  .  .  .  .  . 24  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
   .  .  .  .  . 28  .  .  .  .  . 52  .  .  .  .  . 76  .  .  . 92  .
   .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
   .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
   .  . 28  .  .  .  .  .  .  .  .  .  .  . 112 .  .  .  .  .
   . 24  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
  18  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
   .  .  .  .  .  .  .  .  .  .  . 130 .  .  .  .  .
   .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
   .  .  .  .  .  .  .  .  .  .  .  .  .  .  .
   .  . 52  .  .  .  .  . 130 .  .  .  .  .
   .  .  .  .  .  .  .  .  .  .  .  .  .
   .  .  .  .  .  .  .  .  .  .  .  .     .
   .  .  .  .  . 112 .  .  .  .  .           .
   .  .  .  .  .  .  .  .  .  .                 .
   .  .  .  .  .  .  .  .  .
   .  . 76  .  .  .  .  .
   .  .  .  .  .  .  .
   .  .  .  .  .  .
   .  .  .  .  .
   .  . 92  .
   .  .  .
  50  .
   .
```

Now I can say “So do I".

P & S: Pete’s product is 52. Serena’s sum is 17. The answers are 4 and 13.

***

## Review

With the answers in mind, let’s review the problem from the beginning, where Pete knows only his product and Serena knows only her sum:

Pete’s product (52) factors into 2\*26 or 4\*13. Possible corresponding sums are 28 or 17. So Pete must say “I don’t know".

Serena’s sum (17) partitions into 2+15, 3+14, 4+13, 5+12, 6+11, 7+10, or 8+9. Possible corresponding products are 30, 42, 52, 60, 66, 70, or 72. So Serena doesn’t know which is Pete’s product. But she realizes that each of these products can be factored in at least two different ways – that is, they are not unique products of two primes (nor cubes of a prime). So, she says “I know you don’t know".

Pete then reasons that if Serena’s sum were 28, it can be partitioned into two primes 5+23 and she would not know for sure that he didn’t know the answers. In other words, if Pete’s product were 5\*23, he would know the answers. So Pete concludes that Serena ’s sum is not 28 and says “Now I know".

Finally, Serena concludes that if Pete knows the answers, his product can’t have more than two different factorings. Since the possible products 30, 42, 60, 66, 70, and 72 each have three or more prime factors, his product must be the only other one – 52 – which has two factorings: 2\*26 and 4\*13. With her corresponding sum of 17, she knows the answers are 4 and 13 and says “So do I".

## Extensions

Readers may wish to write a J program to solve this “Now I Know Problem”generally and to consider the following questions:

1. Is it necessary to remove products of two primes in order to solve this problem?
2. Is it necessary to remove the cubes of primes in order to solve this problem? (See Note 1 in [Appendix](#app).)
3. Is it necessary to eliminate unique sums and unique products successively in order to solve this problem? (See Note 2 in the [Appendix](#app).)
4. What is the smallest upper limit (&lt;100) for which this problem can be solved?
5. What is the largest upper limit (>100) for which this problem can be solved?

## Reference

\[1\] [www.mathematik.uni-bielefeld.de/~sillke/PUZZLES/logic\_sum\_product](http://www.mathematik.uni-bielefeld.de/~sillke/PUZZLES/logic_sum_product)

***

## <a id="app"></a>Appendix

Note 1: Cubes of primes also have two unique factors and could be eliminated:

```
p3 =: primes ^ 3                        Cubes of primes
zero =: -. p e. p3              0 where p is in p3
p =: p * zero                   Update p
s =: s * zero                   Update s
```

Note 2: Pete and Serena could look in s and p and eliminate any possible unique sums and products, respectively:

```
                             A utility program to calculate
Freq =: (+/) @ (+/) @ (=/~)  frequencies of each item
zero =: (Freq s) > 2            0 where s frequencies not > 2
s =: s * zero
p =: p * zero
zero =: (Freq p) > 2            0 where p frequencies not > 2
p =: p * zero
s =: s * zero
```

This elimination procedure could be repeated (as necessary) until there is no change in `p` and `s`.

***
