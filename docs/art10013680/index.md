---
vid: '10013680'
title: Zark Newsletter Extracts
authors:
- Gary Bergquist
byline: edited by Jonathan Barman
volume: '21'
issue: '4'
page: '108'
source: trad/v214/zark214.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- programming techniques
---

edited by Jonathan Barman
{ .byline }

## Utility Corner: To E Or Not To E

(The purpose of this column is to make you more productive by introducing you to utility functions. Think of utility functions as APL functions that have names instead of symbols. By expanding your function vocabulary, you'll be able to write APL code that’s more concise, more efficient, and more readable.)

In last issue’s **Limbering Up** column, you were asked to define a monadic utility function `ENOFF` (Exponential Notation OFF) that behaves exactly like monadic `⍕` except it never returns it s formatted numbers in exponential notation.

APL has a tendency to use exponential notation when the numbers it’s displaying are very large or very small. For example:

```
      .0000012345
0.0000012345
      .00000012345
1.2345E¯7
      1234500000
1234500000
      12345000000
1.2345E10
```

While exponential notation does succeed at expressing number in fewer characters, it does not necessarily improve the clarity of the numbers being displayed.

Here’s an example (from Jim Weigang) that shows a display with and without exponential notation

```
      M←(10*¯4+⍳8)∘.×1.234 1.235 0 ¯1.235
      ⎕PP←3
      M
1.23E¯3 1.24E¯3 0 ¯1.24E¯3
1.23E¯2 1.24E¯2 0 ¯1.24E¯2
1.23E¯1 1.24E¯1 0 ¯1.24E¯1
1.23E0  1.24E0  0 ¯1.24E0
1.23E1  1.24E1  0 ¯1.24E1
1.23E2  1.24E2  0 ¯1.24E2
1.23E3  1.24E3  0 ¯1.24E3
1.23E4  1.24E4  0 ¯1.24E4

      ENOFF M
    0.00123     0.00124 0     ¯0.00124
    0.0123      0.0124  0     ¯0.0124
    0.123       0.124   0     ¯0.124
    1.23        1.24    0     ¯1.24
   12.3        12.4     0    ¯12.4
  123         124       0   ¯124
 1234        1235       0  ¯1235
12340       12350       0 ¯12350
```

As you can see, the `ENOFF` function can make a numeric display more meaningful.

The `ENOFF` functions we received generally take one of two approaches. In the first, logarithms (`⍟`) are used to determine the relative magnitude of the numbers. From the magnitude and the current setting of `⎕PP` (Print Precision), you can determine the appropriate left argument of dyadic `⍕` that will give the desired result.

The following function illustrates the approach for a scalar (single number) argument.

```
     ∇ R←ENOFF N;D;G;W
[1]   ⍝ Returns ⍕N for scalar N, but never
[2]   ⍝ returns exponential notation.
[3]   ⍝
[4]    G←1+⌊10⍟|N+N=0
[5]   ⍝ |G is the number of digits to the
[6]   ⍝ left of the decimal (if G>0) or
[7]   ⍝ the number of consecutive zeros to
[8]   ⍝ the right of the decimal (G≤0).
[9]   ⍝
[10]  ⍝ Digits to right of decimal
[11]   D←0⌈⎕PP-G
[12]  ⍝
[13]  ⍝ Required field widths...
[14]   W←D+1⌈G ⍝ Total no. digits
[15]  ⍝ Negative sign and decimal point:
[16]   W←W+(N<0)+D≠0
[17]  ⍝
[18]   R←(W,D)⍕N ⍝ Format it
[19]   →D↓0 ⍝ Done if no decimal point
[20]  ⍝
[21]  ⍝ Delete trailing 0s:
[22]   G←+/^\'0'=⌽R
[23]   R←(-G+G=D)↓R ⍝ And solitary point
     ∇
```

For `⎕PP←3`, here are some intermediate values of this function’s local variables. The values are shown for four different settings of the right argument `N`:

```
      N   0.001234  12.34  ¯0.01234  0.1

[4]   G      ¯2       2      ¯1       0
[11]  D       5       1       4       3
[14]  W       6       3       5       4
[16]  W       7       4       7       5
[18]  R   0.00123   12.3   ¯0.0123  0.100
[22]  G       0       0       0       2
[23]  R   0.00123   12.3   ¯0.0123  0.1
```

Notice that lines `[22]` and `[23]` remove any trailing zeros to the right of the decimal point. They remove the decimal point too if the result is an integer.

The submission from Jim Weigang utilises this approach, and has additional logic to handle numeric arrays of any dimensions:

```
     ∇ R←{P}ENOFF N;B;D;E;F;G;S;T;V;W;X;Z;⎕IO
[1]   ⍝ Behaves like monadic ⍕ but never
[2]   ⍝ returns exponential notation.
[3]   ⍝ Like ⍕, it is sensitive to ⎕PP.
[4]   ⍝ Optional left argument is ⎕PP
[5]   ⍝ surrogate
[6]   ⍝
[7]    ⎕IO←1
[8]   ⍝ Set P from ⎕PP if no left arg:
[9]    ⍎(0=⎕NC'P')/'P←⎕PP'
[10]  ⍝ Empty result for empty arg:
[11]   →(0∊S←⍴N)↓L1
[12]   R←S⍴''
[13]   →0
[14]  ⍝ Make numbers a matrix:
[15]  L1:N←((×/¯1↓S),¯1↑1,S)⍴N
[16]   G←1+⌊10⍟|N+N=0
[17]  ⍝ MG is the number of digits to the
[18]  ⍝ left of the decimal (if G>0) or
[19]  ⍝ the number of consecutive zeros to
[20]  ⍝ the right of the decimal (G≤0)
[21]  ⍝
[22]  ⍝ Compute the appropriate Width and
[23]  ⍝ Digits format for each number:
[24]   D←0⌈P-G ⍝ Digits to rt. of decimal
[25]  ⍝ Required field widths...
[26]  ⍝ One blank to left plus all digits:
[27]   W←1+D+1⌈G
[28]  ⍝ Negative sign and decimal point:
[29]   W←W+(N<0)+D≠0
[30]  ⍝ Shift needed to alogn decimals:
[31]   T←((⍴D)⍴⌈⌿D)-D
[32]  ⍝ One more if decimal absent in
[33]  ⍝ column that has some decimals:
[34]   T←T+(D=0)^(⍴D)⍴∨⌿D>0
[35]  ⍝ Increase width for shift:
[36]   W←W+T
[37]  ⍝ Make each col have uniform width:
[38]   W←(⍴W)⍴⌈⌿W
[39]  ⍝ Formatted Matrix shape
[40]   G←(1↑⍴N),+/W[1;]
[41]  ⍝ Adjust field widths to slide the
[42]  ⍝ decimal points into alignment:
[43]   W←W-T-(⍴T)⍴¯1↓0,,T
[44]  ⍝
[45]  ⍝ Format each number:
[46]   R←(,W,[2.5]D)⍕,N
[47]  ⍝ Make it a matrix
[48]   R←G⍴(×/G)↑R
[49]  ⍝
[50]  ⍝ Remove trailing zeros to the
[51]  ⍝ right of the decimal, and delete
[52]  ⍝ excess blank columns:
[53]   V←,⌽R ⍝ Work with reversed vector
[54]  ⍝ A trick to avoid zero partitions:
[55]   V←'. ',V
[56]  ⍝ 1s mark first char of each number:
[57]   F←B>¯1↓0,B←V≠' '
[58]  ⍝ Ignore those without a decimal:
[59]  ⍝ F←F\F pORRED V='.'
[60]   X←V='.'
[61]   Z←(X∨F)/F
[62]   F←F\(Z/1⌽Z)≤F/X
[63]  ⍝ 1s mark leading (nee trailing) 0s:
[64]  ⍝ T←F pANDSCAN V='0'
[65]   X←V='0'
[66]   Z←~(T←X≤F)/X
[67]   T←~\T\Z≠¯1↓0,Z
[68]  ⍝ Undo the trick:
[69]   T←2↓T
[70]   V←2↓V
[71]  ⍝ 1s mark char just past each group
[72]  ⍝ of 0s:
[73]   D←T<¯1↓0,T
[74]  ⍝ Delete adjacent decimal:
[75]   T←T∨D\'.'=D/V
[76]   T←T∨V=' ' ⍝ Delete blanks, too
[77]   D←G⍴T←~T ⍝ 0s mark stuff to delete
[78]  ⍝ When expanding, put 1 blank
[79]  ⍝ between cols:
[80]   E←(B∨¯1↓0,B←∨⌿D)/D
[81]  ⍝ Delete 0s and blanks, insert
[82]  ⍝ minimum blanks:
[83]   V←(⍴E)⍴(,E)\T/V
[84]  ⍝ Strip final blank, undo reversal:
[85]   V←⌽0 ¯1↓V
[86]  ⍝
[87]  ⍝ Restore leading dimensions:
[88]   R←((¯1↓S),¯1↑⍴V)⍴V
     ∇
```

In the second approach, monadic format is immediately applied to the numeric argument, in the hopes that APL may not have chosen to foil us with exponential notation. If there is no exponential notation (no E’s), we’re done. If there is exponential notation, only those numbers using it need to be reworked. Even then, some useful information can be gleaned from the exponential format of the number.

Again, the following function illustrates the approach for a scalar (single number) argument.

```
     ∇ R←ENOFF N;B;D;I;P;⎕IO
[1]   ⍝ Returns ⍕N for scalar N, but never
[2]   ⍝ returns exponential notation.
[3]   ⍝
[4]    →('E'∊R←⍕N)↓⎕IO←0
[5]    I←R⍳'E'
[6]    P←⍎(I+1)↓R ⍝ Power
[7]    B←I↑R ⍝ Base
[8]   ⍝ No. decimal places in base:
[9]    D←(⍴B)-1+(+/'.¯'∊B)+'0'=¯1↑B
[10]   R←1↓(0⌈D-P)⍕N
     ∇
```

Here are some intermediate values of the local variables when ŒPP"3, using the same settings of the right argument N illustrated above:

```
      N  0.001234  12.34   ¯0.01234   0.1

[4]   R  1.23E¯3   1.23E1  ¯1.23E¯2  1.0E¯1
[5]   I     4         4       5        3
[6]   P    ¯3         1      ¯2       ¯1
[7]   B   1.23      1.23   ¯1.23      1.0
[9]   D     2         2       2        0
[10]  R  0.00123    12.3  ¯0.0123    0.1
```

Notice that the base and power portions of the number (e.g. `1.23` and `¯3` from `1.23E¯3`) are analysed to determine the appropriate left argument to dyadic `⍕`. In this simple function, dyadic `⍕` is called with a single number left argument, which always returns a leading blank in its result. In the function below, which was submitted by Bruce Hitchcock, the more typical pairs-of-numbers left argument is used.

```
       ∇ R←ENOFF N;B;C;CD;CM;D;E;EX;I;L;M;P;S;SX;T;U;W;X;Y;⎕IO
[1]   ⍝ Behaves like monadic ⍕ but never
[2]   ⍝ returns exponential notation.
[3]   ⍝ Like ⍕, it is sensitive to ⎕PP
[4]   ⍝
[5]    →('E'∊R←⍕N)↓0
[6]    ⎕IO←1 ⍝ Origin 1 is fine
[7]    R←,R ⍝ Make it a vector
[8]    T←R≠' ' ⍝ Flag non blanks
[9]   ⍝ Inds of starts of no.s:
[10]   S←(T>¯1↓0,T)/⍳⍴T
[11]   E←(T>1↓T,0)/⍳⍴T ⍝ ... and ends
[12]   Y←⍴L←1+E-S ⍝ Lengths of the no.s
[13]  ⍝ Which ones contain E_s:
[14]   T←+\X←R='E'
[15]   I←(T[E]≠T[S])/⍳Y
[16]  ⍝ Numbers to reformat:
[17]   M←(,N)[I]
[18]  ⍝ Indices of the E_s:
[19]   X←X/⍳⍴X
[20]  ⍝ Starts/ends of these no.s:
[21]   SX←S[I]
[22]   EX←E[I]
[23]  ⍝ Power portion of no. (e.g. ¯5 for
[24]  ⍝ 1.234E¯5
[25]   U←(EX-X)+EX<⍴R ⍝ Lengths
[26]  ⍝ U←∊X+⍳¨U:
[27]   U←U/X-¯1↓0,+\U
[28]   U←U+⍳⍴U
[29]   P←⎕FI R[U] ⍝ Use ⎕FI or ⍎
[30]  ⍝ No. decimal places in base (e.g.
[31]  ⍝ 3 for ¯1.234E5 or 0 for ¯1.0E5):
[32]   T←0,+\R∊'.¯'
[33]   D←X-SX+1+(T[X]-T[SX])+R[X-1]='0'
[34]  ⍝ No. decimal places to show:
[35]   D←0⌈D-P
[36]  ⍝ Total width of number:
[37]   W←(R[SX]='¯')+(1-P+1)+(D≠0)+D
[38]  ⍝ Reformat these numbers
[39]   M←(,W,[1.5]D)⍕M
[40]  ⍝ Make room to insert them:
[41]   T←(⍴R)⍴1
[42]   U←EX-SX ⍝ Lengths
[43]  ⍝ U←∊SX+⍳¨U:
[44]   U←U/SX-¯1↓0,+\U
[45]   U←U+⍳⍴U
[46]   T[U]←0
[47]   T[SX]←W
[48]   R←T/R
[49]  ⍝ Update lengths, starts, ends:
[50]   L[I]←W
[51]   E←(+\T)[E]
[52]   S←1+E-L
[53]   SX←S[I]
[54]   EX←E[I]
[55]   U←(EX-X)+EX<⍴R ⍝ Lengths
[56]  ⍝ W←∊(SX-1)+⍳¨W
[57]   W←W/(SX-1)-¯1↓0,+\W
[58]   W←W+⍳⍴W
[59]   R[W]←M ⍝ Insert the new numbers
[60]   →(1≥⍴⍴N)⍴0 ⍝ Exit if vector result
[61]  ⍝
[62]  ⍝ Need at least one blank before and
[63]  ⍝ after each no. (before is fine):
[64]   R←R,' '
[65]  ⍝ Find index of decimal point within
[66]  ⍝ each no. (or 1 beyound end):
[67]   T←+\X←R='.'
[68]   T←T[E]=T[S]
[69]   P←(~T)\X/⍳⍴X
[70]   P[T/⍳Y]←1+T/E
[71]  ⍝ No. digits to left of point:
[72]   M←P-S
[73]  ⍝ ... and to right, including point:
[74]   D←0⌈E-P
[75]   D←D+×D
[76]  ⍝ Largest of each by column:
[77]   T←Y÷C←¯1↑⍴N
[78]   CM←1+⌈⌿(T,C)⍴M ⍝ Plus leading blank
[79]   CD←⌈⌿(T,C)⍴D
[80]  ⍝ Replication vector for expanding:
[81]   T←R≠' '
[82]   T[S-1]←(Y⍴CM)-M
[83]   T[E+1]←T[E+1]+(Y⍴CD)-D
[84]   R←((¯1↓⍴N),+/CM+CD)⍴T/R
     ∇
```

From timings we performed on the two functions above, the second approach seems to be quicker, running in 50% to 75% the time required by the first approach. Since these timings depend on the rank and nature of the numbers, we recommend you perform your own timings if speed is critical to your application.

*Ed: There was a small problem verifying that the last two functions (which use monadic format) worked correctly, as the formatting rules in different flavours of APL vary slightly. The original article was evidently developed with APL\*PLUS, but I used Dyalog APL 8.2 to reproduce and test the functions. There is no leading space for the first column of a formatted matrix in Dyalog APL, so line*  `[7]`<i>in the fourth </i>  `ENOFF` *function had to be amended to*  `R←,' ',R`<i>. Also, Dyalog is much less willing to display exponential format for numbers in a simple vector, so extreme measures were necessary to reproduce the intermediate variables when running the simplified (scalar) example function. I never managed to get </i>  `0.1` *to display as*  `1.0E¯1` *and had to put up with*  `1E¯1` *or*  `1.00E¯1` *instead.*

## Limbering Up: Accumulations

(The purpose of this column is to work some flab off your APL midsection. Like muscles, your APL skills can atrophy if not exercised with adequate frequency and variety. This column presents a task for you to perform. Set aside a few minutes from your busy schedule and work the task. Mail in your solution and stay tuned for the results.)

A classic: suppose you have two numeric vectors whose elements are in one-to-one correspondence. The first ( `ACCT`) is a vector of account numbers; the second ( `AMT`) is a vector of dollar amounts. The account numbers in `ACCT` are not distinct; they repeat. What are the distinct account numbers? How many times does each account number occur? What is the sum of the numbers in `AMT` for each distinct account number? What is the percent of each number in `AMT` relative to the total for its account?

The efficient APL algorithms for these problems are well known and will be discussed in the next issue. Your task is to design the syntax of one or more utility functions that make the solutions to such problems convenient and intuitive.

Send your solution to:  
Vector Production  
Brook House  
Gilling East  
York YO62 4JJ  
UK  
email [gill@apl385.com@compuserve.com](mailto:gill@apl385.com)

The notable functions and their authors’ names will be printed in the next issue of *Vector*. Good luck and happy limbering.
