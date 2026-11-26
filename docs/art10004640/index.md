---
vid: '10004640'
title: The Steam Hammer and the Fly
authors:
- Gérard Langlet
byline: Gérard Langlet
volume: '7'
issue: '4'
page: '138'
source: trad/v074/langlet74_138.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1991
tags:
- programming techniques
- performance
- development practice
---

Gérard Langlet
{ .byline }

A French proverb says: “Never use a steam-hammer to smash a fly”. It (or its English counterpart) should be repeated on every page of any book or tutorial devoted to APL.

Vigorously attacked in Vector 7.1 \[1\], I shall answer in a way that may not fulfil Bob Bykerk’s criticisms completely, but as much as I can in a few pages; indeed a whole book would be better - that may come in the future.

First, I have been programming in APL for almost 20 years, writing sophisticated applications for research and industry, i.e. some important subset of the “real world”. Quite often, I also have to translate my algorithms into other compiled languages for various reasons; execution speed, compatibility with other existing programs or systems, absence of APL implementations, hatred or incompetence of colleagues about APL, or simply the still high cost of good APL interpreters on some hardware.

The real world has many many constraints of all kinds, so, when programming in APL, which still is the best language ever designed to test a new idea and build a prototype, it does not seem to me superfluous to think of what will come next. The “RISC” programming style is the fruit of hard experiments. It may not correspond to what is taught in manuals about data structures and/or the usage of APL notation. But I am nothing of a masochist, and I would gently conform to any other so-called “healthy coding style” if I were convinced of its real superiority. APL should not remain like an ivory tower, the real world is indeed a multi-language environment, full of traps e.g. ambiguous and fuzzy (sometimes erroneous) data.

Many people often believe that the beauty and the expressive power of the APL notation on paper, especially in extended implementations, will lead them to write easily nice and efficient code. Just try `R←∊⍳¨V` with, for example, `V←10000⍴3 4 5 0 2 3` on some APL2 compatible interpreter and compare the CPU time with `R←V/V-+/V ⋄ R←R+⍳⍴R`.

Let us now talk about data structures; one is taught to represent sales data in 3 dimensions - salesman `×` month `×` year. Why not, if you just want to use the power of +/ on any of the major axes? But this is in fact a simplistic case; one might want to extract much more sophisticated information from this array - see below. The ideal data structure has not been discovered yet. It is certainly not an array. It may be a “fuzzy fractal”. But some APL implementations (e.g. APL.68000, APL90, and to a certain extent, Dyalog-APL and APL\*PLUS), offer fast reversible matrix to vector conversion tools such as `⎕BOX`.

In general, a text page is shorter when kept as a vector. This is also true for numeric data with heterogeneous length, but `⎕BOX` accepts them only in the first two implementations. Most of my data are kept in vectors, and if and only if it is necessary to handle them in matrices, or in vectors of vectors, or in vectors of matrices, they are TEMPORARILY, rapidly and internally reshaped with automatic mechanisms which accept ANY of the possible data structures as input.

Another item of my short letter to Vector \[2\] has been misunderstood. I never wrote not to use locals; I think that locals should be avoided when they are unnecessary, except e.g. for voluntary didactic purposes. I would appreciate an extended APL implementation in which all one character names would be automatically localised; this would save typing time and space. Since I deprecate the use of parentheses, APL expressions are shorter and do not lead so frequently to the WS FULL message; then, I do collect the intermediate results within the minimum of locals. With “RISC” programming style, I don’t burn my bridges. My programs are easier to debug than before. Nobody is obliged to believe me; just try, if you wish, and criticise only in six months from now.

Try to write ex nihilo with branches, the PERM function listed in \[3\]. It is not an easy way of doing it. Then, try to write a screen manager using some combinations of the 3 buttons of a mouse as well as the key-pad, in order to handle several recursive pop-up menu-windows with lifts, travelators, automatic clipping, shadows and help files. I did that once, in APL\*PLUS PC, and I doubt that I could have succeeded with branches and labels \[4\].

The result is a short general-purpose RISC-APL function which respects several other rules of my anathematised programming habits; the display of a function should not exceed the screen size, so that you never have to scroll up or down to see what it does; every procedure (simply a character string or vector) is defined before the one which uses it; the resulting code executes quickly in APL and can be translated easily i.e. by hand or with another small APL program into any of the important programming languages in the real world outside APL (yes! it does exist).

### Are Salesmen Fly-smashers?

Suppose we have 20 salesmen who have worked for 10 years from 1981 to 1990. Y is the vector of the number of days in each year, and N its sum:

```
      1 20×N←+/⎕←Y←10⍴3 1/365 366
365 365 365 366 365 365 365 366 365 365
3652 73040
```

Let us introduce the following two functions. PSUM which produces the partial sums of B according to A, and EXECR which EXECutes `⍙` with Rank `∆`

```
      ∇R←A PSUM B;D;N;⎕IO
[1]  ⎕IO←1 ⋄ D←¯1↑⍴B←+\B ⋄ N←⌈D÷1⌈+/A ⋄ R←⍳N←|N×⍴,A ⋄ A←D⌊+\N⍴A ⋄ N←⍴⍴B
[2]  N EXECR 'B←0,-B[A]⋄R←B[R]-B[R+1]'
     ∇

      ∇∆ EXECR ⍙                    ⍝ APL.68000 Level II
[1]  ∆←'[',1↓∆⍴';' ⋄ ⍎⎕SS ⍙ '[' ∆   ⍝ ⎕SS accepts strand notation: A B C
     ∇

      ∇∆ EXECR ⍙                    ⍝ APL.68000 Level I
[1]  ⍎⎕SS(⍙;'[';∆←'[',1↓∆⍴';')      ⍝ ⎕SS has 3 arguments (A;B;C)
     ∇
```

Note: `⎕SS` substitutes every occurrence of B by C in character vector A.

Function PSUM is programmed “thinking of vectors”, but it also works, reducing any rank array along the last dimension, due to EXECR which is a general purpose tool (JUST LOCK IT IF YOU DISLIKE IT), or adapt it to your own APL implementation if you have no `⎕SS`. Rub it if you never use arrays.

Now, let us suppose that you want the results per week, per month, and per semester. What will you do if you have organised your data in an array and if you cannot use nested arrays? With vectors, heterogeneous groupings are easy. The number of full weeks in these 10 years is `W←W⌊N÷7`, i.e. 521 and the number of extra days in the same period is `E←7|N` i.e. 5. You may try: `RW←7 PSUM 20 3652⍴T` if you just want to start “as the data are” and love reshaping, but what happens if you want full weeks starting on Sundays - in the English way - or on Mondays - in the French one? Incidentally, January 1st, 1981 was Thursday, so that the first group of days is 3, and the last one is E-3 when the week starts on Sunday.

```
      K←20×⍴V←1 521 1/3 7,E-3
      G←K⍴V
      RSW←G PSUM T ⍝ for "Sunday weeks"
```

Now let D be the day/month-vector in a normal year, reshaped for 10 years:

```
D←120⍴31 28 31 30 31 30 31 31 30 31 30 31 ⋄ I←120⍴12↑0 1
M←2400⍴D+I^12/Y=366 ⋄ S←G PSUM M ⋄ RM←M PSUM T ⋄ RS←S PSUM T
```

RM is the result per Month, and RS the result per Semester.

Note: “Replicate” is frequently used here. Although absent from The ISO-Standard APL it will be in the next standard and is available in most present implementations. If you have APL2, all this will also work; then, try to measure CPU time using enclosed arrays instead... Will the fly survive?

### References

1. Bykerk, B. The Dangers of APL RISC programming, Vector, 7.1, 112.
2. Langlet, G.A. APL RISC Programming Style, Vector, 6.2, 23-24.
3. Langlet, G.A. The Travelling Salesman Problem revisited with APL, APL90 Conference, Copenhagen. APL Quote Quad, 20.4, 228-232 (July 1990).
4. Langlet, G.A. Presentation of GLOS, all-purpose software integrator on PC, in "Modelling of Molecular Structures and Properties", Elsevier Science Publishers, ISBN 0-444-88714-8, 767 (1990).

Ed: The above is a shortened version of Gerard’s original article, which was too long for the space available in Vector.

(webpage generated: 19 December 2006, 01:38)
