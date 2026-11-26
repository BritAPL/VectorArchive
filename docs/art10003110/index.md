---
vid: '10003110'
title: Backtracking, Queens and Permutations
authors:
- Ted Emms
byline: by Ted Emms
volume: '10'
issue: '1'
page: '34'
source: trad/v101/emms101_34.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- programming techniques
- competitions and puzzles
- education
---

*by Ted Emms*
{ .byline }

We are all familiar with the concept of backtracking in everyday life. When presented with a choice we make a selection, and pursue that until we are successful in our aim, or we reach an impasse. If we ‘get stuck’ we retrace our chosen path and select one of the other alternatives. If this process is carried out in a methodical fashion, we will (if it is possible) achieve our aim.

A classic recreational problem is to place 8 queens on an 8×8 chessboard so that no queen attacks any other queen. A queen attacks any piece which is in the same row or column or on one of the diagonals through its square. Backtracking provides a methodical way of solving such a problem. We will choose the simpler problem of placing 4 queens on a 4×4 board to explain the method.

In Figure 1a we have placed a queen on the first row (R=1) and the first column (C=1). We cannot place another queen in that row so we consider row 2. We consider column 1 (C=1) but a queen at that square would violate the conditions. Similarly for C=2, but we can place a queen at C=3 (Figure 1b). We now move to row 3. We try C=1 through 4 but in every case the conditions are violated. We are ‘stuck’. So we backtrack. We return to row 2 to find the queen at C=3. We try the square to the right (C=4) and this is acceptable (Figure 1c). We now move once again to R=3 and this time we find that it is possible to place a queen at C=2 (Figure 1d). On to row 4, but once again trying C=1,2,3,4 we get stuck.

Backtrack to row 3 and try moving the queen at C=2 to the right. It is not possible, so we continue to backtrack to row 2 (Figure 1e). We cannot move the queen at C=4 to the right, so we backtrack to R=1 (Figure 1f). At last! We can move the queen at C=1 to C=2 (Figure 1g). We can fit in a queen at C4 in row 2 (Figure 1h), then a queen at C=1 in R=3 (Figure 1i), and finally a queen at C=3 in row 4. Success!

We have a solution (Figure 1j). But we haven’t finished, because we can backtrack again to find further solutions. Figure 1k through to Figure 1o take us to a further solution at Figure 1o. If we again carry out the backtracking routine we find eventually that we are trying to place a queen in row 0 (R=0) which does not exist, so we must have found all solutions (2 in this case).

Figure 1

```
         (a)               (b)               (c)               (d)
R  ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___
4 |___|___|___|___| |___|___|___|___| |___|___|___|___| |___|___|___|___|
3 |___|___|___|___| |___|___|___|___| |___|___|___|___| |___|_Q_|___|___|
2 |___|___|___|___| |___|___|_Q_|___| |___|___|___|_Q_| |___|___|___|_Q_|
1 |_Q_|___|___|___| |_Q_|___|___|___| |_Q_|___|___|___| |_Q_|___|___|___|
 C  1   2   3   4     1   2   3   4     1   2   3   4     1   2   3   4
         (e)               (f)               (g)               (h)
R  ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___
1 |___|___|___|___| |___|___|___|___| |___|___|___|___| |___|___|___|___|
2 |___|___|___|___| |___|___|___|___| |___|___|___|___| |___|___|___|___|
3 |___|___|___|_Q_| |___|___|___|___| |___|___|___|___| |___|___|___|_Q_|
4 |_Q_|___|___|___| |_Q_|___|___|___| |___|_Q_|___|___| |___|_Q_|___|___|
 C  1   2   3   4     1   2   3   4     1   2   3   4     1   2   3   4
         (i)               (j)               (k)               (l)
R  ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___
1 |___|___|___|___| |___|___|_Q_|___| |___|___|___|___| |___|___|___|___|
2 |_Q_|___|___|___| |_Q_|___|___|___| |_Q_|___|___|___| |___|___|___|___|
3 |___|___|___|_Q_| |___|___|___|_Q_| |___|___|___|_Q_| |___|___|___|_Q_|
4 |___|_Q_|___|___| |___|_Q_|___|___| |___|_Q_|___|___| |___|_Q_|___|___|
 C  1   2   3   4     1   2   3   4     1   2   3   4     1   2   3   4
         (m)               (n)               (o)
R  ___ ___ ___ ___   ___ ___ ___ ___   ___ ___ ___ ___
1 |___|___|___|___| |___|___|___|___| |___|_Q_|___|___|
2 |___|___|___|___| |___|___|___|___| |___|___|___|_Q_|
3 |___|___|___|___| |_Q_|___|___|___| |_Q_|___|___|___|
4 |___|___|_Q_|___| |___|___|_Q_|___| |___|___|_Q_|___|
 C  1   2   3   4     1   2   3   4     1   2   3   4
```

We now have to use this methodology in writing a program in I-APL to find solutions. Experience suggests that we can break the problem up into four routines:

1. A routine for placing queens moving up the board (R increasing). This we shall call QUP.
2. A routine for removing queens and moving down the board (R decreasing). This we shall call QDN.
3. A routine which acts as the master, calling QUP and QDN as necessary. This routine can also contain any initialization routines. We shall call it QMAIN.
4. A routine which prints out the results in a desired form. This is called QPR.

We can represent the state of the board by a vector A which describes how the queens are placed. Thus `A←2 4 1 3` would represent the position in Figure 1j. An empty row may have a zero so that the position in Figure 1n would be `A←3 1 0 0`.

```
[0]   QMAIN N;A;B;Z;R;C;SS
[1]   A←N⍴0
[2]   B←(2×N)⍴' :'
[3]   'Solutions as vectors (0) or as a board  (1)'
[4]   Z←⎕
[5]   R←1
[6]   C←0
[7]   SS←0
[8]  LOOP:QUP
[9]   →(R=0)/OVER
[10]  ⍎(R=(N+1))/'QPR'
[11]  QDN
[12]  →LOOP
[13] OVER:'ALL DONE'
[14]  'Total Solutions = ',⍕SS
```

The size of the board is N. B is a vector used in QPR. Z records the choice of display. SS is the score of solutions. The main loop is contained in lines \[9\]-\[12\]. This repeatedly calls QUP and QDN. If a solution is reached line \[10\] recognises this and calls the print routine QPR. Line \[9\] detects when all solutions have been obtained.

```
[0]   QUP;Y;L;M
[1]  LP0:C←C+1
[2]   →(C>N)/OUTUP
[3]   Y←0
[4]  LP1:Y←Y+1
[5]   M←|(C-A[Y])
[6]   L←R-Y
[7]   →(M=0)/LP0
[8]   →(M=L)/LP0
[9]   →(Y<N)/LP1
[10]  A[R]←C
[11]  C←0
[12]  R←R+1
[13]  →LP0
[14] OUTUP:
```

Line \[2\] detects if we are ‘stuck’. Lines \[4\]-\[9\] form a loop which tests whether placing a queen at C,R violates the conditions. This test is carried out for each queen already placed. M is the difference in column numbers. If the absolute value of M equals R-Y then the two queens being examined are on the same diagonal. If the conditions are violated, lines \[7\] or \[8\] return us to LPO and we try the next column. If the conditions are not violated a queen is placed (line \[10\]) and we move to the next row in order to try to place another queen.

```
[0]   QDN
[1]   R←R-1
[2]   →(R=0)/OUTDN
[3]   C←A[R]
[4]   A[R]←0
[5]  OUTDN:
```

In this routine we go back a row. Line \[2\] detects whether R has got to zero. Otherwise the column number of the last queen place is recovered in line \[3\] and the queen is removed by line \[4\].

```
[0]   QPR;D;E;V
[1]   SS←SS+1
[2]   →(Z=1)/ON1
[3]   'Soln. No: ',(⍕SS),'.....',⍕A
[4]   →OUTPR
[5]  ON1:V←N+1
[6]  LP2:V←V-1
[7]   D←B
[8]   E←A[V]
[9]   D[2×E]←'Q'
[10]  D
[11]  →(V>1)/LP2
[12]  (2×N)⍴'*'
[13] OUTPR:
```

The printout routine uses the value of Z to give either a vector output (line \[3\]) or a board (lines \[6\]-\[11\]). The routine inserts a ‘Q’ in the row vector B set up in QMAIN.

The program is set in motion by the call QMAIN N where N is an integer representing the number of queens to be placed. I suggest you keep N reasonably small until you are sure you have the program working.

The relationship between backtracking and recursion is very close. Some would say that backtracking is just one form of recursion. The standard way for a program to find factorial n, for example, by recursion is for the program to call itself repeatedly whilst n is decremented down to a value where the result is known (`!1`) and then retrace its path (backtrack?) upwards till the value of `n!` is calculated.

Let us now consider the problem of placing 5 different articles (we will call them A, B, C, D and E) in 5 ordered boxes. We could impose conditions (such as A must not be adjacent to B) but we will not do so. The first solution will be ABCDE. Using our backtrack technique, we will get ABCED for the next solution. It is rather surprising to realise that we have a method for getting all the permutations of the 5 items! Writing the program in four routines following the ideas already given we have PAMAIN, PAUP, PADN and PAPR.

It is slightly simpler than the Queens program as it is not necessary to have an RR array. The rest follows the same pattern as the Queens program. The program is set in motion by the call PAMAIN N where N is the number of items to be permuted. Again, try it with small N to begin with.

```
[0]   PAMAIN N;L;U;A;X;R;SS
[1]   L←65↓(65+N)↑⎕AV
[2]   U←N⍴0
[3]   A←0/'0'
[4]   X←0
[5]   R←1
[6]   SS←0
[7]  LOOP:PAUP
[8]   →(R=0)/OVER
[9]   ⍎(R>N)/'PAPR'
[10]  PADN
[11]  →LOOP
[12] OVER:'ALL DONE.'
[13]  'Number of permutations = ',⍕SS
```

```
[0]   PAUP
[1]  LP0:X←X+1
[2]   →(X>N)/OUTUP
[3]   →(U[X]=1)/LP0
[4]   U[X]←1
[5]   A←A,L[X]
[6]   R←R+1
[7]   X←0
[8]   →LP0
[9]  OUTUP:
```

```
[0]   PADN;Q
[1]   R←R-1
[2]   →(R=0)/OUTDN
[3]   Q←A[R]
[4]   X←L⍳Q
[5]   U[X]←0
[6]   A←(¯1+⍴A)⍴A
[7]  OUTDN:


[0]   PAPR
[1]   SS←SS+1
[2]   A
```

It is an interesting challenge to modify the program so that it will permute a number of articles where some are identical. The permutations of the word APPLE would be less in number than the permutations of the word AMPLE.

\[Ted Emms has provided a solution to this challenge, to be printed next issue. Any other offers? - Ed.\]

(webpage generated: 5 December 2005, 18:45)
