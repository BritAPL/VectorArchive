---
vid: '10500790'
title: 'Backgammon tools in J: Two-sided bearoff probabilities'
subtitle: '3: Two-sided bearoff probabilities'
authors:
- Howard A. Peelle
byline: by Howard A. Peelle (hapeelle@educ.umass.edu)
volume: '25'
issue: '4'
page: '104'
online: '2012-03-24'
editor: Kai Jaeger
description: J programs are presented as analytical tools for expert backgammon. Part 3 here develops a program to compute probability distributions for two-sided bearoff (without contact) and uses it to determine the probability of winning.
abstract: J programs are presented as analytical tools for expert backgammon [1]. Part 3 here develops a program to compute probability distributions for two-sided bearoff (without contact) and uses it to determine the probability of winning.
source: content/printed/254/backgammon3.htm
converter: vecrescue 0.1.0
issue_tab: n4-november-2012
tags:
- mathematics and statistics
- applications
---

3: Two-sided bearoff probabilities
{ .subtitle }

by Howard A. Peelle (hapeelle@educ.umass.edu)
{ .byline }

J programs are presented as analytical tools for expert backgammon [\[1\]](#ref1). Part 3 here develops a program to compute probability distributions for two-sided bearoff (without contact) and uses it to determine the probability of winning.
{ .abstract }

The inner board is represented as a list of six integers. For example, one piece on the 5-point and one piece on the 6-point:

```
        board =: 0 0 0 0 1 1
```

Utility programs:

```
ELSE =: `
WHEN =: @.
```

Master program `PD` produces a list of probabilities for bearing off in increasing numbers of rolls. Defined much like `Rolls` in [\[1\]](#ref1), it calls `PDBearoff` or else returns 1 when all pieces are off the board:

```
PD =: PDBearoff ELSE 1: WHEN AllOff
                AllOff =: All@(Board = 0:)
                Board =: ]
                All=: And/
                And =: *.
PDBearoff =: 0: , Average@AllPDs
                Average =: +/ % #
                AllPDs =: PD"1@BestMoves

BestMoves =: PDDoubles , 2: # Better@PDSingles

doubles =: > 1 1 ; 2 2 ; 3 3 ; 4 4 ; 5 5 ; 6 6
singles =: > 1 2 ; 1 3 ; 1 4 ; 1 5 ; 1 6 ; 2 3 ; 2 4 ; 2 5 ; 2 6 ; 3 4 ;
 3 5 ; 3 6 ; 4 5 ; 4 6 ; 5 6
singles =: singles ,: |."1 singles

        PDDoubles =: doubles Best@AllMoves"1 Board
        PDSingles =: singles Best@AllMoves"1 Board
        Better =: Best@,:"1/
                Best =: {~ MinIn@:(N"1)
                MinIn =: i. <./
```

Note that program N (from [\[1\]](#ref1)) is used in `Best` to evaluate boards.

`AllMoves` and its subprograms are the same as in [\[1\]](#ref1):

```
AllMoves =: (Die2 Moves Die1 Moves Board) ELSE ¬
(Die1 Moves ^:4 Board) WHEN (Die1=Die2)
        Die1 =: First =: {.@Dice
        Die2 =: Last =: {:@Dice
        Dice =: [
        EACH =: &.>
Moves =: ~.@;@(LegalMoves EACH <"1)
        LegalMoves =: (Possibles Move"1 Board) ELSE Board WHEN AllOff
                Possibles =: FromTo Where FromTo OK"1 Board
                        FromTo =: On ,. On-Die1
                        Where =: #~
                                On =: Points Where Board > 0:
                        OK =: Off Or Inside Or Highest
                                Or =: +.
                                Inside =: Last > 0:
                                Off =: Last = 0:
                                Highest =: First = Max@On
                                        Max =: >./
                Move =: Board + To - From
                                From =: Points e. First
                                To =: Points e. Last
                                Points =: 1: + i.@6:
```

For example, probabilities of bearing off the board above in 0, 1, 2, 3, and 4 rolls:

```
   PD 0 0 0 0 1 1
0 0.166667 0.707562 0.124829 0.000943073
```

Such a probability distribution is related to expected number of rolls (N):

```
   +/ pd * i.#pd =. PD (board)
1.96005
   N (board)
1.96005
```

Further, `PD` can be used to determine the probability of the first player winning:

```
Pwin =: X/@amp;,:&PD
   X =: +/ . * +/\.
```

`Pwin` computes the `PD` of both boards, then sums products of player 1’s probabilities times reverse cumulative sums of player 2’s probabilities – that is, the sum of probabilities of 1 bearing off times the respective probabilities of 2 not bearing off.

The result is the probability of player 1 winning the bearoff.

Example:

```
   0 0 0 0 1 1 Pwin 0 0 0 0 2 0
0.766415
```

The first player is about 77% likely to win.

The programs above are extremely inefficient, so `PD` should be re-defined to utilize a pre-established database of probability distributions for fast look up (as done for `N` in Part 1).

Upon request, the author will provide a script with efficient programs.

## References

1. <a id="ref1"></a> Peelle, Howard A. “Backgammon Tools in J (Part 1) Bearoff Expected Rolls”, Vector, Vol. 24, No. 2&3.
2. <a id="ref2"></a> Peelle, Howard A. “Backgammon Tools in J (Part 2) Wastage”, Vector, Vol. 24, No. 4.
