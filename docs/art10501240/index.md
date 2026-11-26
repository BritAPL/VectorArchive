---
vid: '10501240'
title: Notation as a tool of proof
authors:
- Robert Pullman
byline: Robert Pullman (rpullman@gmail.com)
volume: '26'
issue: '2'
page: '45'
received: '2013-04-01'
online: '2014-01-21'
description: APL is used to analyze the symmetries of magic squares.
abstract: APL is used to analyze the symmetries of magic squares. ⎕IO of 1 used throughout.
source: content/printed/262/pullman/pullman.htm
converter: vecrescue 0.1.0
issue_tab: n23-september-2014
tags:
- mathematics and statistics
- programming techniques
---

Robert Pullman (rpullman@gmail.com)
{ .byline }

APL is used to analyze the symmetries of magic squares. `⎕IO` of 1 used throughout.
{ .abstract }

## 1. A Primer On Magic Squares

The classic magic square of order N is an arrangement of `⍳N*2` into an N by N matrix A such that the sum of each of the N rows, N columns, and both diagonals equals the same value - `(N­×1+N*2)÷2`. There is no solution for `N=2` so presume `N>2`.

2000 years ago the Chinese knew of magic squares of order 3. There are 8 magic squares out of 9! (362880) possible 3 by 3 squares.

In the 17th century Bernard de Bessy determined that there are 7040 magic squares of order 4.

There are many techniques for constructing magic square but no simple way of counting the number of magic squares of order N. In 1973 an MIT grad student, Richard Schoeppel, solved for N=5, 2202441792 solutions. Schoeppel wrote an Assembler program which took a week to determine the answer. In a private communication he wrote:

> “… The approach was straightforward, filling in the cells in a particular order and backtracking. There was a little bit of hardware advantage: The DEC10 had multi-level indexed indirect addressing, making it easy to add a few numbers in a single instruction. Another hack was the instruction for reversing the bits in a register. This made it fast to determine quickly the possible solutions to X+Y=K, using bit masks of the remaining available numbers.
>
> Two other people independently confirmed the count, using different strategies. One Japanese gentleman sent me his thesis, with a long table of pieces of squares that he then assembled. My counting program would probably run in a few minutes today if converted to C.”

For N>5 the number of solutions is unknown. There are estimates of the values, for example `10*19` for N=6. If this is accurate I wonder how long it would take that C program to solve for N=6.

## 2. Symmetries of magic squares

There is a common definition of ‘ distinct" magic squares, e.g. from “Solving Magic Squares”[\[1\]](#ref1)

> “…there are exactly 880 distinct 4x4 magic squares, not counting rotations and reflections…”

There are 7040 magic squares of order 4. The above claims that for a magic square A there are 8 rotations and reflections, which reduces 7040 to 880 solutions. This factor of 8 is attributed to de Bessy, but de Bessy's paper was published posthumously in 1693.

If A is a magic square then so are the reflections `⌽A`,`⊖A`,and `⊖⌽A`. Since `⌽` and `⊖` are commutative there are just the 4 distinct magic squares. `⍉` applied to each of the 4 doubles the result to 8. 90 degree rotation of A is `⍉⊖A`, 180 is `⊖⌽A`, and 270 is `⍉⌽A`.

Actually there are 32 magic squares which can be derived from a magic square of order 4. There are 220 basic solutions from which all 7040 can be derived.

More generally, for a magic square A of order N (>2) one can derive `(!⌊N÷2)×2*2+⌊N÷2` magic squares. This formula has been known for some time by mathematicians, Schoeppel for one, also Benson and Jacoby. In the following sections a reasonable, if not rigorous, proof of this is offered. APL provides a convenient notation for this proof.

## 3. Definitions & Lemmas

*Isomorphic vectors*. V and W are isomorphic if and only if `V[⍋V]≡W[⍋W]`.

*Lemma 3.1*: If V and W are isomorphic integer vectors, `(+/V)=+/W` by the associative property of addition.

*Isomorphic squares*. A and B are isomorphic if and only if each row of A isomorphic to a row of B, each column of A to some column of B, `1 1⍉A` (main diagonal) with `1 1⍉B`, and `1 1⍉⌽A` (opposite diagonal) with `1 1⍉⌽B`.

*Lemma 3.2*: If A is a magic square and A and B are isomorphic, then B is a magic square. Follows from 3.1 applied to each row, column, and diagonal of B.

*Lemma 3.3*: If A and B are isomorphic and `A[I;K]∊B[J;]` then row I of A is isomorphic with row J of B. Proof: `A[I;K]` can only be in one row of B, so since A and B are isomorphic that row must be isomorphic with `A[I;]`.

*Corollary*: `A[K;I]∊B[;J]` then column I of A if isomorphic with column J of B.

## 4. Symmetric transforms

### 4.1. T1 of a magic square A

For any pair I,J such that `I<J≤⌊N÷2`, apply these row switches:

```
A[I,J,(N+1-I),(N+1-J);]←A[J,I,(N+1-J),(N+1-I);]
```

Rows and columns of the result are isomorphic with rows and columns of A but the diagonals are not.

Then switch columns in the same way:

```
A[;I,J,(N+1-I),(N+1-J)]←A[;J,I,(N+1-J),(N+1-I)]
```

Rows and columns of the result are again isomorphic.

The diagonals of the result are also isomorphic with the same diagonals of A since we have switched 4 pairs of diagonal items on the upper left `(A[I;I] & A[J;J])`, upper right `(A[I;N+1-I]` & `A[J;N+1-J])`, lower left `(A[N+1-J;I]` & `A[N+1-I;J])` and lower right `(A[N+1-I;I]` & `A[N+1-J;J])`. So the result is also a magic square.

Through a series of switches any permutation of the first `⌊N÷2` items on the upper left diagonal can be accomplished.

So `!⌊N÷2` distinct magic squares can be derived from A via T1.

### 4.2 T2 of a magic square A

For any `I≤⌊N÷2`, switch row I with row N+1-I:

```
A[I,(N+1-I);]←A[(N+1-I),I;]
```

Rows and columns of the result are isomorphic with A, diagonals are not, since two items of each diagonal are no longer on the same diagonal.

Then switch column I with column `N+1-I`.

```
A[;I,(N+1-I)]←A[;(N+1-I),I]
```

Rows and columns are again isomorphic.

Diagonals are isomorphic to the same diagonals since the only change is `A[I;I]` has switched with `A[N+1-I;N+1-I]` and `A[N+1-I;I]` has switched with `A[I;N+1-I]`. So the result is a magic square.

Each of first `⌊N÷2` rows can be switched, so there are `2*⌊N÷2` distinct magic squares which can be derived from A via T2.

### 4.3 T1 and T2 are disjoint

Since `(N+1-I)>⌊N÷2` no T2 can satisfy `I<J<⌊N÷2`. So there are `(!⌊N÷2)×2*⌊N÷2` distinct magic squares which can be derived from A via T1 and T2.

### 4.4 Closure Under T1 and T2

If A and B are isomorphic magic squares, B can be derived from A.

If `A[I;J]=B[I;J]` then (by lemma 3.3) row I of A is isomorphic with row I of B, and column J of A with column J of B.

Since the main diagonals are isomorphic we can apply T1 and T2 to obtain C such that `C[I;I]=B[I;I]` for all `I≤⌊N÷2`.

So row I of C are isomorphic with row I of B and column I of C with column I of B.

Since the diagonals are isomorphic it follows that `C[I;N+1-I]=B[I;N+1-I]` and `C[N+1-I;I]=B[N+1-I;I]`.

So row `N+1-I` of C is isomorphic with row `N+1-I` of B and column `N+1-I` of C with column `N+1-I` of B.

If N is even this shows that `C[I;J]=B[I;J]` for all I,J so `C≡B`.

If N is odd there is exception of the middle row and middle column. For `I≠(N+1)÷2`, `N-1` items in row I and column I match, which forces `C[I;(N+1)÷2]=B[I;(N+1)÷2]` and `C[(N+1)÷2;I]=B[(N+1)÷2;I]`. That leaves just the central item `[(N+1)÷2,(N+1)÷2]` which is also forced to match and so `C≡B`.

### 4.5 T3: Reflections

If A is a magic square and `B←⌽A` or `B←⊖A` then B is a magic square.

Under `⊖`, `B[;I]` is isomorphic with `A[;I]` and `B[N+1-I;]≡A[I;]`.

Under `⌽`, `B[I;]` is isomorphic with `A[I;]` and `B[;N+1-I]≡A[;I]`.

In each `1 1⍉B` is isomorphic with `1 1⍉⌽A` and `1 1⍉⌽B` with `1 1⍉A`, so B is a magic square.

However B is not isomorhpic to A since `1 1⍉B` is not isomorphic with `1 1⍉A`.

On the other hand if both are applied, say `B←⌽⊖A` then B is isomorphic with A.

It follows that `⊖A` or `⌽A` cannot be arrived at by T1 and T2 so reflection doubles the number of solutions obtained by T1 and T2.

So the number of solutions via T1, T2, and T3 is `(!⌊N÷2)×2*1+⌊N÷2`

### 4.6 T4: Transpose

For any magic square A, `B←⍉A` is also a magic square with `(1 1⍉A)≡1 1⍉B`.

The rows of A are isomorphic with the columns of B, and the columns of B with the rows of A. So B is certainly not isomorphic with A and, by transitivity, not with any isomorphism of A.

Suppose C is isomorphic with A. Since `(1 1⍉A)≡1 1⍉B` by transitivity `1 1⍉B` is isomorphic with `1 1⍉C`, so `1 1⍉B` cannot be isomorphic with `1 1⍉⌽C` or `1 1⍉⊖C`. So `B≢⌽C` and `B≢⊖C`.

This completes the proof that `(!⌊N÷2)×2*2+⌊N÷2` distinct magic squares which can be derived from any one solution.

## 5. Footnote: Associative Magic Squares

An associative magic square has the property that the sum of any item and its diametric opposite is `1+N*2`. This property is preserved under any of the four transforms.

There are no associative magic squares of order `N if 2=4|N`. This was shown by A.H. Frost in 1878. The proof is too detailed to present here. See “Associative magic square”[\[2\]](#ref2)

### References

1. <a id="ref1"></a> “Solving Magic Squares” [http://mathpages.com/home/kmath295.htm](http://mathpages.com/home/kmath295.htm)
2. <a id="ref2"></a> “Associative magic square” [http://en.wikipedia.org/wiki/Associative\_magic\_square](http://en.wikipedia.org/wiki/Associative_magic_square)
