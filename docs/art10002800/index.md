---
vid: '10002800'
title: The MAGIC Goes Away – Opening the Boxes
authors:
- Mike Day
volume: '10'
issue: '1'
page: '125'
source: trad/v101/day101_25.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- competitions and puzzles
- mathematics and statistics
- education
---

### Introduction

A year ago Bill McLean brought the game of MAGIC to the pages of Education Vector, Vector Vol.9, No.1 (pp.25-29). In the next issue, 9.2, Dave Ziemann discussed the game, and presented a method for solution and a set of APL\*PLUS II functions to do the job. It was a quite brilliant treatment, but as Dave said, “I am left with the feeling that there’s probably a very simple way of knowing what key to press from any given state, but it eludes me for the moment.” Here is one way of doing just that. I want to share it with you because it throws light on some aspects of A-level-ish Mathematics. The APL is quite neat as well.

In case you don’t remember, in MAGIC you are given a starting 3×3 matrix or “box” of random 0s and 1s. You are asked to pick one or more of nine given standard boxes – I’ll call them `MagicBoxes` – to complement the starting box to achieve the target box, which we call `win`.

Bill’s article described a program to interact with the player, presenting the random starting box and showing the current position. Dave responded to his invitation to readers to provide solution algorithms.

Like Dave Ziemann, I’ve developed my solution mostly in terms of direct definition, but using first generation APL\*PLUS version 8. I’ve also developed an analogous solution in J(4.1) rather to my surprise.

### Representation

As in Dave’s treatment, I take a global zero index origin. I don’t want to rewrite all those parts of Dave’s article that apply here as well. He showed that it’s useful to be able to enumerate the 512 different possible states in a natural way as either decimal numbers 0 to 511 or as binary numbers 0 to 111111111 using some simple functions.

I’ve cribbed his functions DB and BD :

```
     DB:1=(9⍴2)⊤ ⍵
     BD:2⊥ ⍵

     BD  0 0 1 1 1 1 0 1 1
123

     DB  123
0 0 1 1 1 1 0 1 1
```

The functions `SHOWBool` and `SHOWDec` show states as 3×3 boxes:

```
     sym ← '01' ⋄ blk ← ' '

     SHOWBool: 3 0 ↑blk:0≠×/⍴ ⍵ :sym[3 3 ⍴⍉ ⍵ ],blk,SHOWBool 0 1 ↓ ⍵
     SHOWDec:SHOWBool DB, ⍵

     SHOWBool 9 1 ⍴ 0 0 1 1 1 1 0 1 1 ⍝ FN expects a matrix argument
001
111
011

     SHOWDec  123
001
111
011
```

But I’m more interested in the method of solution than in presentation.

### Why use McLean’s MagicBoxes?

The specified set of building blocks or `MagicBoxes` is

```
     SHOWBool MagicBoxes
110 111 011 100 010 001 000 000 000
110 000 011 100 111 001 110 000 011
000 000 000 100 010 001 110 111 011
```

Dave Ziemann presents a “short MAGIC game”. Firstly, the function `SetVars` sets the scene, including choosing which boxes to play with – in this case `MagicBoxes`.

```
SetVars: key←BD⍎boxes←⍵ ⋄ sym←'01',blk←' '⋄ win←495
          ⋄ ⍵←0↑⎕EX'BaseToKey'

     SetVars 'MagicBoxes' ⋄ 32 SHOWPATH win

     0   5   6

    110 001 000
    110 001 110
    000 001 110

000 110 111 111
100 010 011 101
000 000 001 111
```

### BaseBoxes

But suppose we had a different set of boxes; one I call `BaseBoxes`:

```
     SHOWBool BaseBoxes
100 010 001 000 000 000 000 000 000
000 000 000 100 010 001 000 000 000
000 000 000 000 000 000 100 010 001
```

We can easily see that the same problem can be solved – in more steps in this case:

```
     SetVars 'BaseBoxes' ⋄ 32 SHOWPATH win

     0   1   2   5   6   7   8

    100 010 001 000 000 000 000
    000 000 000 001 000 000 000
    000 000 000 000 100 010 001

000 100 110 111 111 111 111 111
100 100 100 100 101 101 101 101
000 000 000 000 000 100 110 111
```

It is pretty obvious with this set of building blocks that we never need use more than nine moves. You can probably see quite easily that there is only one starting state for which nine is the minimum number of boxes, namely the complement of `win`,

```
     SetVars 'BaseBoxes' ⋄ 16 SHOWPATH win

  0   1   2   3   4   5   6   7   8

 100 010 001 000 000 000 000 000 000
 000 000 000 100 010 001 000 000 000
 000 000 000 000 000 000 100 010 001

000 100 110 111 111 111 111 111 111 111
010 010 010 010 110 100 101 101 101 101
000 000 000 000 000 000 000 100 110 111
```

We can use more than the minimum number of moves for any solution, but only by adding some box 2 or 4 or 6 or any positive even number of times more than necessary.

### Solution with BaseBoxes

The solution is intuitively obvious when we use `BaseBoxes` to provide the building blocks. We can also express it mathematically.

Given:

```
     BD START ← 0 0 0 1 0 0 0 0 0
32
     BD WIN ← 1 1 1 1 0 1 1 1 1
495

     +BD ⎕← DIFF ← ADD START, [0.5] WIN
1 1 1 0 0 1 1 1 1
463
```

where

```
     ADD: ≠ / ⍵
```

So we can easily solve a MAGIC puzzle using the unofficial set called `BaseBoxes`. But how do we solve a real puzzle with Bill McLean’s set, `MagicBoxes`?

### Plus and Times for Boolean Numbers

We can think of `≠` for “Booleans” as equivalent to `+` (and `⍲` as well) for ordinary decimal numbers, since (for `<=>` read ‘is equivalent to’):

```
          x ≠ y <=>  2 | x + y
                <=>  2 | x - y
```

The dyadic operation `≠` is the “sum” for the “Vector Space” to which the Binary representations of puzzle states belong.

It is obvious with either `BaseBoxes` or the original `MagicBoxes` that you can add them together any number of times, but that all even numbers are equivalent to doing nothing, while all odd numbers are just the same as only adding once. So we are discovering a system for “scaling” Boolean vectors `x` with integer numbers `k` such that

```
          x + k y <=> x      if 0 = 2 | k
                  <=> x ≠ y  if 1 = 2 | k
```

Suppose we know `x` and `k` and that

```
          x <=> k z
```

What is `z`?

If `k` is odd, i.e. `1 <=> 2 | k`, then

```
          z <=> x
```

However, if `k` is even, both `x` and `z` must be null vectors. `k` cannot be even for non-zero `x`. This is not quite the same as when

          `X = 0 Z` implies that `Z` is infinite unless `X = 0`.

We can in fact extend our system of scalar multipliers to include “rational numbers” of the form (p/q) where p and q are integers. If p/q is reduced to its lowest terms, p/q &lt;=> 1 if both p and q are odd, &lt;=> infinity if either is even.

### Using BaseBoxes as a Basis

Let’s now return to our two sets of boxes. When we don’t use the `SHOWBool` function they appear as two 9×9 Boolean matrices:

```
     (⍕MagicBoxes),(9 10⍴' '),⍕BaseBoxes
 1 1 0 1 0 0 0 0 0          1 0 0 0 0 0 0 0 0
 1 1 1 0 1 0 0 0 0          0 1 0 0 0 0 0 0 0
 0 1 1 0 0 1 0 0 0          0 0 1 0 0 0 0 0 0
 1 0 0 1 1 0 1 0 0          0 0 0 1 0 0 0 0 0
 1 0 1 0 1 0 1 0 1          0 0 0 0 1 0 0 0 0
 0 0 1 0 1 1 0 0 1          0 0 0 0 0 1 0 0 0
 0 0 0 1 0 0 1 1 0          0 0 0 0 0 0 1 0 0
 0 0 0 0 1 0 1 1 1          0 0 0 0 0 0 0 1 0
 0 0 0 0 0 1 0 1 1          0 0 0 0 0 0 0 0 1
```

Consider the third column of `MagicBoxes`

```
     SHOWBool 9 1⍴ ⎕ ← MagicBoxes[;2]
0 1 1 0 1 1 0 0 0
011
011
000
```

This Boolean vector may be seen as signifying that the third Magic Box consists of the following elements of `BaseBoxes`:

```
     0 1 1 0 1 1 0 0 0 / ⍳ 9
1 2 4 5

     SHOWBool  9 1⍴ ≠ / BaseBoxes [; 1 2 4 5]
011
011
000
```

So we now know – trivially really – how to express `MagicBoxes` in terms of sums of `BaseBoxes`. And if we have a number of `MagicBoxes` – e.g. `0 5 6` as in Dave’s first example game – we can take their sum

```
     ≠ / MagicBoxes [; 0 5 6]
1 1 1 0 0 1 1 1 1
```

It is equivalent to the sum of `BaseBoxes` numbers 0 1 2 5 6 7 8 as we saw earlier:

```
     ≠ / BaseBoxes [; 0 1 2 5 6 7 8]
1 1 1 0 0 1 1 1 1
```

### Using MagicBoxes as a Basis

We now need to be able to INVERT the process. Instead of expressing combinations of `MagicBoxes` in terms of `BaseBoxes`, we need to express `BaseBoxes` in terms of `MagicBoxes`. For example, starting from the knowledge that the set `0 1 2 5 6 7 8` of `BaseBoxes` solves a problem, how can we deduce that the set `0 5 6` of `MagicBoxes` does the same job?

Well, the 9×9 matrix `MagicBoxes` is really a transformation matrix from a representation with respect to the basis `MagicBoxes` to one in terms of `BaseBoxes`. For example, the vector

```
     m ← 1 0 0 0 0 1 1 0 0 ⍝  <=> take MagicBoxes  0 5 6
```

is transformed to the vector

```
     b ← 1 1 1 0 0 1 1 1 1 ⍝  <=> take BaseBoxes 0 1 2 5 6 7 8
```

using a Boolean Matrix multiplication function MULT

```
     MagicBoxes MULT m
1 1 1 0 0 1 1 1 1
```

We can define MULT either by

```
     MULT: ⍺ ≠ . ^ ⍵ ⍝ using Boolean operations
```

or by     

```
     MULT: 2 | ⍺ + . × ⍵ ⍝ using numeric add and multiply
```

### Boolean Matrix Inversion

So let’s find a Matrix Inverse for the array MagicBoxes – the usual APL `⌹` almost does the trick, but not quite.

The second form of definition for `MULT` suggests that if

     `x = M y` , for Boolean `x` and `y` and using Boolean matrix multiplication

then     `X = M Y` , for numeric `X,Y, 2 ≡ X <=> x, 2 ≡ Y <=> y` and ordinary matrix multiplication, `(+.×)`.

If there is a Boolean inverse IM for M, then Y = IM X

so     y = 2 ¦ (IM M)(y + 2W) for some integer W

i.e.     y + 2 Z = (IM M)(y + 2W) for some integer Z.

But if `IM = ⌹ M` then this condition is met as (IM M) is an identity matrix. However `IM` can contain non-integer rationals:

```
     +IM ← ⌹ M ← MagicBoxes
 ¯0.2  0.4 ¯0.2  0.4  0.4 ¯0.6 ¯0.2 ¯0.6  0.8
  0.6 ¯0.2  0.6 ¯0.2 ¯0.2 ¯0.2 ¯0.4  0.8 ¯0.4
 ¯0.2  0.4 ¯0.2 ¯0.6  0.4  0.4  0.8 ¯0.6 ¯0.2
  0.6 ¯0.2 ¯0.4 ¯0.2 ¯0.2  0.8  0.6 ¯0.2 ¯0.4
 ¯0.2  0.4 ¯0.2  0.4 ¯0.6  0.4 ¯0.2  0.4 ¯0.2
 ¯0.4 ¯0.2  0.6  0.8 ¯0.2 ¯0.2 ¯0.4 ¯0.2  0.6
 ¯0.2 ¯0.6  0.8  0.4  0.4 ¯0.6 ¯0.2  0.4 ¯0.2
 ¯0.4  0.8 ¯0.4 ¯0.2 ¯0.2 ¯0.2  0.6 ¯0.2  0.6
  0.8 ¯0.6 ¯0.2 ¯0.6  0.4  0.4 ¯0.2  0.4 ¯0.2
```

We now recall that any odd scalar multiple of a Boolean vector is equivalent to the vector itself in our nine-dimensional vector space. The same applies to matrices:

Let:

```
     +IM1 ← ⌹ MagicBoxes ÷ 5
 ¯1  2 ¯1  2  2 ¯3 ¯1 ¯3  4
  3 ¯1  3 ¯1 ¯1 ¯1 ¯2  4 ¯2
 ¯1  2 ¯1 ¯3  2  2  4 ¯3 ¯1
  3 ¯1 ¯2 ¯1 ¯1  4  3 ¯1 ¯2
 ¯1  2 ¯1  2 ¯3  2 ¯1  2 ¯1
 ¯2 ¯1  3  4 ¯1 ¯1 ¯2 ¯1  3
 ¯1 ¯3  4  2  2 ¯3 ¯1  2 ¯1
 ¯2  4 ¯2 ¯1 ¯1 ¯1  3 ¯1  3
  4 ¯3 ¯1 ¯3  2  2 ¯1  2 ¯1
```

The divisor 5 is in fact the determinant of the matrix `MagicBoxes`!

```
     b
1 1 1 0 0 1 1 1 1

     2 | IM1 +.× b ⍝ which just happens to be the same as
1 0 0 0 0 1 1 0 0

     m ⍝ !
1 0 0 0 0 1 1 0 0
```

Unfortunately I’ve cheated. While we could in general take the ordinary inverse and try different odd integer scalars until we light on the right one, we strictly ought to calculate either the determinant of `MagicBoxes`:

```
     DET MagicBoxes
5
```

(It is interesting that the J 4.1 determinant `(-/ . *)` threatens to take over four hours on a portable PC (Toshiba 1850) for this problem.)

... or to gain perhaps even more insight by using a special-purpose function

```
     BINV MagicBoxes
 1 0 1 0 0 1 1 1 0
 1 1 1 1 1 1 0 0 0
 1 0 1 1 0 0 0 1 1
 1 1 0 1 1 0 1 1 0
 1 0 1 0 1 0 1 0 1
 0 1 1 0 1 1 0 1 1
 1 1 0 0 0 1 1 0 1
 0 0 0 1 1 1 1 1 1
 0 1 1 1 0 0 1 0 1
```

I cobbled `BINV` together from Keith Smillie’s `INV` function in STATPACK2

```
     ∇BINV[⎕]∇
[0]  b←BINV a;k;s;p;i;is;t;u;⎕IO
[1]  ⍝  INVERT a SQUARE BINARY MATRIX... AFTER KW SMILLIE's STATPACK2
[2]  ⍝  NB REAL ADD/MINUS BOTH BECOME ≠
[3]  ⍝ REAL TIMES BECOMES ^
[4]  ⍝ REAL DIVIDE SHOULDN'T BE NECESSARY AS DIVISOR is 0 OR 1
[5]  ⍝
[6]  ⍝ AN ALTERNATIVE METHOD is TO TAKE 2 | (DETERMINANT a) × ⌹ a
[7]  ⍝ BUT THAT NEEDS a DETERMINANT FUNCTION OF SIMILAR COMPLEXITY
[8]  ⍝
[9]   ⎕IO←1 ⋄ →((×/×/b=a←2|b←a)^(2=⍴⍴a)^=/⍴a)/OK ⍝ CHECK a is OK
[10]
[11] NOINV:'NO INVERSE!' ⋄ →~b←1
[12]
[13] OK:p←is←⍳s←k←⌊/⍴a ⋄ a←a[;p,1]
[14]
[15] LOOP:a[;1+s]←2>is ⋄ i←t⍳⌈/t←a[⍳k;1] ⍝  FIND NEXT ROW
[16]  p[t←1,i]←p[u←i,1] ⋄ a[t;is]←a[u;is] ⍝ SWAP ROWS
[17]  →(0=a[1;1])/NOINV ⍝ SINGULAR MATRIX
[18] ⍝ DONT NEED ... a[1;]←a[1;]÷a[1;1] DIVISOR ALWAYS UNITY IF REACH HERE
[19]  a←a≠(a[;1]^1<is)∆.^a[1;] ⍝ WAS  a - (a[;1] × 1 < is)∆.× a[1;]
[20]  a←a[t←1+s|is;1+is,0] ⋄ p←p[t] ⍝ SHIFT ROWS AND COLUMNS
[21]
[22]  →(0<k←k-1)/LOOP ⍝
[23]  b←a[;p⍳is] ⍝ REORDER FOR ANSWER
```

It is of course very similar to Smillie’s original, just replacing all ‘`×`’ by ‘`^`’ and both ‘`+`’ and ‘`-`’ by ‘`≠`’. Note that I disabled the line involving division by the pivot cell, since we have seen that Boolean “division” yields either no change or an infinity – and we expect the argument to be non-singular as we are dealing with basis representations.

### Solution

It just remains to fit the various elements together. The function `PATH` does the job.

```
     PATH:(KEY ADD DB ⍵ , ⍺ )/⍳⍴key
```

The arguments `⍺` and `⍵` are the decimal representations of the start and target boxes. `ADD DB ⍵ , ⍺` finds the solution in terms of `BaseBoxes`.

```
     ADD DB win, 32
1 1 1 0 0 1 1 1 1
```

The function `KEY` multiplies this by the Boolean Inverse of `MagicBoxes`, `BaseToKey`, calculating it in the function `BaseRepToKeyRep` if it does not exist. (We see now why `SetVars` expunges `BaseToKey` – it is in case a new set of `MagicBoxes` is invoked as the building blocks.)

```
     BaseRepToKeyRep:BaseToKey:0=⎕NC'BaseToKey':BaseToKey←BINV DB key

     KEY:BaseRepToKeyRep MULT ⍵

     SetVars 'MagicBoxes' ⋄ +m ← KEY ADD DB win, 32
1 0 0 0 0 1 1 0 0
```

Finally, the resulting Boolean vector is used to select the `MagicBoxes`,

```
     m/⍳⍴key
0 5 6
     32 PATH win
0 5 6
```

The remaining functions are just utilities or display routines.

### Conclusions

The game of MAGIC as presented confuses the issue (probably intentionally!) in being apparently a two-dimensional problem with 512 states. However, as Dave Ziemann showed, we really have a 9-dimensional set of Boolean vectors. The decimal representation is helpful to my mind only to make the notation more concise. It does not add to our understanding of the transitions as Dave found. I suspect that instructive analogies might be drawn with other systems.

For example, theoretical physicists describe representations of microscopic space-time in terms of multiple (11?) dimensional strings embedded in 4 “real” dimensions. A Boolean number is a numerical realisation of the smallest possible non-trivial commutative “group” of order 2, so that our MAGIC game problem is a little like a simple physics with 9 different quantum properties, each with 2 alternative states. I gather that elementary particle theory teems with interacting “symmetry” groups, mostly built up from simple subgroups of order 2 or 3.

Casting aside speculation, we have found a concise, quick method of solution for MAGIC puzzles. There are symmetries which I have not pursued – the two-dimensional view may after all be useful! – e.g. the solutions for decimal 1, 4, 64 and 256 are all identical apart from rotation. (There are four corner boxes and four edge boxes and one middle. Various symmetry groups – order four and order two – relate corners together and edges together.) It’s possible that consideration of these symmetries might lead to an improvement in performance of Dave’s algorithm, but my method is so quick and simple that I haven’t followed up this particular line.

Duncan Pearson wondered if a similar vector space treatment involving inversion of a transformation matrix could be applied to the N-Queens problem. I don’t think so – at least I haven’t found it – the reason probably being that you can’t realise a presumably N-dimensional basis corresponding to the operation of “adding” a Queen.

However, the ubiquitous not-equal scan does make an appearance, even if not providing as much of the solution as Gérard Langlet’s views on the power of this construction would lead us to believe (e.g. Vector 9, No 3).

### APL Functions

Here is a listing of all the APL functions involved. It’s only those few listed under “SolutionFunctions” that actually find the answer. The others are to do with setting up the problem (`SetVars`) and displaying one or more states (`SHOWBool` and `SHOWDec`) or the whole problem (`SHOWPATH`).

My version of Direct Definition – it’s not part of APL\*PLUS as supplied – allows local variables, `x`, `y` and `z`, in addition to the arguments `⍵` (and `⍺` if the function is dyadic). The direct definition interpreter expects its functions to produce results.

### Utility Functions

```
LAM: ⍺ ,[⎕IO-0.5] ⍵ ⍝ LAMINATE IN NEW 1ST DIMENSION
SetVars: key←BD ⍎ boxes←⍵ ⋄ sym←'01',blk←' ' ⋄ win←495 ⋄ ⍵ ←0↑⎕EX 'BaseToKey'
```

### SolutionFunctions

```
ADD:≠/ ⍵
BaseRepToKeyRep:BaseToKey:0=⎕NC 'BaseToKey':BaseToKey←BINV DB key

BD:2⊥ ⍵
DB:1=(9⍴2)⊤ ⍵

KEY:BaseRepToKeyRep MULT ⍵
MULT: ⍺ ≠.^ ⍵
PATH:(KEY ADD DB ⍵ , ⍺ )/⍳⍴key
```

### Display Functions

```
KEYPath:(0, ⍵ )LAM ⍺ ,key[⍵ ← ⍺ PATH ⍵ ]
SHOWBool: 3 0 ↑blk:0≠×/⍴ ⍵ :sym[3 3 ⍴⍉ ⍵ ],blk,SHOWBool 0 1 ↓ ⍵
SHOWDec:SHOWBool DB, ⍵
SHOWPATH:(SHP0 ⍵ [1 1 ⍴⎕IO;])⍪SHP1(⍵ ← ⍺ KEYPath ⍵ )[1+⎕IO;]
SHP0:1⌽[1+⎕IO]2⌽(- 0 2 4 +⍴ ⍵ )↑ ⍵ ← 4 0 ⍕ 0 0 1 ↓ ⍵

SHP1:((3 4 ⍴blk),SHOWBool DB,1↓ ⍵ )LAM SHOWBool scanADD DB ⍵
scanADD:≠\ ⍵


     ∇DET[⎕]∇
[0]  z←DET m;i;j;n;a;b;⎕IO
[1]  ⍝ FIND THE DETERMINANT OF m
[2]  ⍝ A METHOD THAT WORKS ... NOT OPTIMAL BUT >> THAN RECURSION!
[3]  ⍝ DET m EQUIV DET m' WHERE m'[i;] = m[i;] - k × m[j;]
[4]  ⍝ TRIES TO GET ZERO'S IN LOWER TRIANGLE (OFF DIAGONAL)
[5]
[6]  ⍝ DET OF A TRIANGULAR MATRIX IS JUST THE PRODUCT OF THE
[7]  ⍝ DIAGONAL ELEMENTS
[8]
[9] →(^/^/m=m+.×⍉m)/ENDI,⎕IO←1
[10]  i←(0,n←¯1↑⍴m)[1]
[11] LOOPI:→(n≤i←i+1)/ENDI
[12]  j←i↓⍳n[1]
[13]  →(1E¯20≥|m[i;i])/ZERO
[14]  m[j;]←m[j;]-m[a;]×m[j;b]÷m[a←i↓b;b←n[1]⍴i]
[15]  →LOOPI
[16] ZERO:→(^/b←0=i↓m[;i])/LOOPI
[17]  m[i;]←m[i;]+,(<\~b)/[1](i,0)↓m
[18]  i←i-1
[19]  →LOOPI
[20] ENDI:
[21] ⍝ m SHOULD BE IN UPPER TRIANGULAR FORM
[22]  z←×/ 1 1 ⍉m
```

***
