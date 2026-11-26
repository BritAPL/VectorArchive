---
vid: '10004420'
title: APL-Defined Functions for the Calculation of Determinants
authors:
- Joseph De Kerf
byline: by Joseph De Kerf
volume: '10'
issue: '3'
page: '21'
source: trad/v103/de103_21.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- mathematics and statistics
- programming techniques
---

by Joseph De Kerf <span class="affil"></span> <span class="email"></span>
{ .byline }

In a lot of scientific and statistical problems, we need the solution of a system of linear equations. In APL such systems may be solved directly with the dyadic form of the domino. In addition, if there are more equations than unknowns, the method of least squares is applied.

In some applications however, we need to calculate a determinant, for instance as a byproduct of the solution of such a system of linear equations. In SHARP APL and in J, the dyadic form of the inner product is extended to its monadic form `f.gA`, such that for instance `+.×A` and `-.×A` return the permanent and the determinant of the right argument `A` respectively. However, this extension was not implemented in any other current dialect of APL. In the meantime, most APL users who need the value of the determinant of a matrix are referred to a defined function.

Consulting the literature to find an appropriate defined function for the calculation of the value of the determinant of a numeric square matrix, we imposed *a priori* the following conditions:

1. The function should work for every numeric square matrix. The result for an empty numeric zero-by-zero matrix should be 1.
2. The function should be index origin independent, working not only for index origin 1, but also for index origin 0.
3. The function should return a scalar, not a one-element vector, neither a one-by-one matrix.
4. If the argument is not a numeric square matrix, no value may be returned as explicit result. Eventually, either an error report is signalled, or an appropriate message is displayed.

More than twenty defined functions published in the literature were examined. None of the investigated functions satisfied the stated requirements. As such, we propose two new defined functions which satisfy those requirements, as far as tested at least. Both functions conform to ISO APL. It is supposed that, either the APL implementation used does not support nested nor heterogeneous arrays or, if the implementation supports nested and heterogeneous arrays, then the arguments are simple and homogeneous.

```
[0] D←DET1 M;N;I;⎕IO
[1] →((2≠⍴⍴M)∨(1≠=/⍴M)∨(1↑,M)=⍕1↑,M)/0
[2] →(2>N)/0,D←(+/1↑,M)+0=N←1↑⍴M
[3] D←N⍴I←⎕IO←1
[4] LAB:D[I]←DET1 M[1↓⍳N;(I≠⍳N)/⍳N]
[5] →(N≥I←I+1)/LAB
[6] D←-/M[1;]×D
```

`DET1` calculates the determinant by recursion, based on minor expansion. It is proposed to be used in the classroom, as a demonstration of such minor expansion. In addition, it is a very illustrative example of the mechanism and possibilities offered by recursion.

```
[0] D←DET2 M;N;I;⎕IO
[1] →((2≠⍴⍴M)∨(1≠=/⍴M)∨(1↑,M)=⍕1↑,M)/0
[2] →(1=1↑⍴M)/0,D←+/1↑,M
[3] →(0=1↑⍴M)/0,D←⎕IO←1
[4] LAB:M[I,1;]←M[1,I←(⌈/|M)⍳⌈/⌈/|M;]
[5] M[;J,1]←M[;1,J←(|M[1;])⍳⌈/|M[1;]]
[6] D←D×M[1;1]×¯1*+/1≠I,J
[7] M←(1 1 ↓M)-((1↓M[;1])∘.×1↓M[1;])÷M[1;1]
[8] →(1≤1↑⍴M)/LAB
```

`DET2` calculates the determinant by elimination, with as pivot in each cycle the maximum of the magnitude of the elements of the current active matrix. It is proposed, not only to be used in the classroom as a demonstration of the elimination method, but also as a subfunction for practical applications in which the value of a determinant is needed.

Finally, if the APL implementation used supports nested and heterogeneous arrays, and we do not restrict the arguments to be simple and homogeneous, for APL2-line implementations e.g., line 1 in both functions could be replaced by:

```
[1]  →((2≤=M)∨(2≠⍴⍴M)∨(1≠=/⍴M)∨∨/((∊M),1↑∊M)∊⎕AV)/0
```

A detailed report of this investigation, including details about the behaviour of the defined functions from the literature examined, has been published in the APL-CAM Journal, Vol.14, No.3, 16 July 1992, pp 417-488. Copies of this report are available at the address of the author:

Joseph De Kerf, Rooienberg 72,  
B-2570 Duffel, BELGIUM.  
Tel.(32) 15-314742.

(webpage generated: 18 February 2006, 02:15)
