---
vid: '10501070'
title: 'Backgammon Tools in J: Ace-point Bearoffs'
subtitle: '4: Ace-point Bearoffs'
authors:
- Howard A. Peelle
byline: Howard A. Peelle (hapeelle@educ.umass.edu)
volume: '26'
issue: '1'
page: '115'
received: '2013-02-02'
online: '2013-04-09'
editor: John Jacob
description: J programs are presented as analytical tools for expert backgammon to compute the probability of winning a two-sided bearoff with stacks of pieces on both ace points, to generate binary probability trees, and to solve a related problem.
abstract: J programs are presented as analytical tools for expert backgammon to compute the probability of winning a two-sided bearoff with stacks of pieces on both ace points, to generate binary probability trees, and to solve a related problem.
source: content/printed/261/peelle/peelle.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- mathematics and statistics
- programming techniques
---

4: Ace-point Bearoffs
{ .subtitle }

Howard A. Peelle (hapeelle@educ.umass.edu)
{ .byline }

J programs are presented as analytical tools for expert backgammon to compute the probability of winning a two-sided bearoff with stacks of pieces on both ace points, to generate binary probability trees, and to solve a related problem.
{ .abstract }

Utility programs:

```
    ELSE =: `
    WHEN =: @.
    X =: [
    O =: ]
```

The probability of the first player (X) winning is 1 minus the probability for the second player (O) -- which is the sum of probabilities for rolling double dice and single dice -- or else the default 1 when there are no pieces left:

```
Ace =: (1: - Double + Single) ELSE 1: WHEN (X<1:)
```

The probability of double dice is `1%6` times the result of `Ace` for the second player (now first, due to switching turns) with the same number of pieces to bear off and four less for the first player (now second). The probability of single dice is similar, but two less:

```
    Double =: (1:%6:) * O Ace X-4:
    Single =: (5:%6:) * O Ace X-2:
```

An alternative definition:

```
Ace =: (1: - Double + Single)~ ELSE 1: WHEN (<1:)

    Double =: (1:%6:) * (Ace (-4:))
    Single =: (5:%6:) * (Ace (-2:))
```

For example, the winning probability for the first player with 8 pieces vs. 6 pieces is:

```
    8 Ace 6
0.301055
```

Here is a table of such probabilities for all odd-numbered stacks:

```
    5.2 ": 1 3 5 7 9 11 13 15 Ace"0/ 1 3 5 7 9 11 13 15
    1.00 1.00 1.00 1.00 1.00 1.00 1.00 1.00
    0.17 0.86 1.00 1.00 1.00 1.00 1.00 1.00
    0.00 0.25 0.79 0.98 1.00 1.00 1.00 1.00
    0.00 0.02 0.30 0.75 0.96 1.00 1.00 1.00
    0.00 0.00 0.05 0.33 0.72 0.93 0.99 1.00
    0.00 0.00 0.00 0.08 0.35 0.70 0.91 0.99
    0.00 0.00 0.00 0.01 0.10 0.36 0.68 0.90
    0.00 0.00 0.00 0.00 0.02 0.12 0.37 0.67
```

Results for even numbers one larger (`Ace"0/~ +:>:i.8`) are the same. These results can be confirmed by program `Pwin` in [\[3\]](#ref3) using probability distributions. For example, `8 0 0 0 0 0 Pwin 6 0 0 0 0 0 is 0.301055`.

Now generalize these programs to generate a binary tree for any input probability `P` (and its complement) to a given level `L`:

```
    P =: [
    L =: ]

BTree =: (Left,Right) ELSE 1: WHEN (L<1:)

    Left  =:   P * P BTree L-1:
    Right =: -.@P * P BTree L-1:
```

This can be used to generate probabilities for n-roll bearoffs. For example, to bear off 3 pairs of pieces:

```
    1r6 BTree 3
1r216 5r216 5r216 25r216 5r216 25r216 25r216 125r216
```

The sum of any `p BTREE n` is 1, and the previous level `p BTREE n-1` can be reconstructed by `_2 +/\ p BTree n`. The whole tree is:

```
    1r6 BTree"0 i.4
    1     0     0      0     0      0      0       0
  1r6   5r6     0      0     0      0      0       0
 1r36  5r36  5r36  25r36     0      0      0       0
1r216 5r216 5r216 25r216 5r216 25r216 25r216 125r216
```

### Problem

Here is a related conundrum to solve: Given a stack of pieces on the ace-point, what is the probability of bearing off the final pieces with a doubles on the last roll? \[This problem was posed by Walter Trice, well-known backgammon author and world-class player.\] See end of article for answer.

It is easy to modify `BTree` to produce an asymmetric binary tree by truncating a branch:

```
BTreeA =: (LeftA , RightA) ELSE 1: WHEN (L<1:)

    LeftA =: P * P BTreeA L-2:
    RightA =: -.@P * (P BTreeA L-1:) ELSE 0: WHEN (L<2:)
```

Use it to explore the problem:

```
   +/ 1r6 BTreeA 8       or       +/ (1%6) BTreeA 8
479891r1679616                 0.285715
```

Or, use a variant of `Ace` (above):

```
DoubleOffAce =: (D + S) ELSE 1: WHEN (<1:)

    D =: (1:%6:) * DoubleOffAce@(-4:)

    S =: (5:%6:) * DoubleOffAce@(-2:) ELSE 0: WHEN (<3:)
```

For instance, `DoubleOffAce 15` is 0.285715.

### Answer

The probability of bearing off a stack on the ace-point with doubles is `2%7`.  
`+/"1 (1r6 BTreeA)"0 i.10` shows oscillating convergence, or `DoubleOffAce"0 i.20` (in pairs).

## References

1. <a id="ref1"></a> Peelle, Howard A. “Backgammon Tools in J (Part 1) Bearoff Expected Rolls”, Vector, Vol. 24, No. 2&3
2. <a id="ref2"></a> Peelle, Howard A. “Backgammon Tools in J (Part 2) Wastage”, Vector, Vol. 24, No. 4
3. <a id="ref3"></a> Peelle, Howard A. “Backgammon Tools in J (Part 3) Two-sided Bearoff Probabilities”, Vector, Vol. 25, No. 4
