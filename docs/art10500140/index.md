---
vid: '10500140'
title: Quick calculation of Kendall’s Rank Correlation Distribution
authors:
- Gordon Sutcliffe
byline: Gordon Sutcliffe (gsutcliffe@talk21.com)
volume: '24'
issue: '1'
page: '131'
description: Quick Calculation of Kendall’s Rank Correlation Distribution
abstract: The nub of the paper is the km2 function with krfd as a user-friendly cover function for quick calculation of Kendall’s rank correlation frequency distribution. The remainder is intended to provide a useful, but by no means exhaustive, background.
source: content/printed/241/kendall/kendall.htm
converter: vecrescue 0.1.0
issue_tab: n1-december-2008
tags:
- mathematics and statistics
- programming techniques
---

Gordon Sutcliffe (gsutcliffe@talk21.com)
{ .byline }

The nub of the paper is the `km2` function with `krfd` as a user-friendly cover function for quick calculation of Kendall’s rank correlation frequency distribution. The remainder is intended to provide a useful, but by no means exhaustive, background.
{ .abstract }

## Introduction

Kendall’s Rank Correlation Distribution provides the statistical probability for the assertion that two sets of n ranks are significantly different, rather than just random fluctuations. The cumulative probability used for their solution can be produced from the integer frequencies calculated here.

Currently the probabilities for n>10 ranks are managed by using approximations to the normal distribution. However, it is not easy to determine the degree of approximation.

The problem from the calculation point of view is that Kendall’s original method takes an enormous amount of computer time to calculate when the number of ranks is more than 12. On a PC [\[1\]](#ref1) the frequency distribution for n=13 ranks would take days, with the time increasing as factorial n.

Quick calculation is based on Kendall’s theory, but uses a recurrence relationship which enables the frequency distribution to be calculated using floating point arithmetic in a fraction of a second up to n=170. (Factorial 170 is the limit for standard floating point arithmetic on a PC.)

The integer floating-point frequencies are exact up to n=18, but at higher n, precision drops to about 13 to 14 significant digits [\[2\]](#ref2). J extended-integer arithmetic produces exact frequencies up to n=170 in 2000 seconds, or up to n=300 in 14 hours on a PC.

## Methods of calculation

Two calculation methods are considered. Method 1 is based on first principles, as in Kendall’s work. Runtime usually limits calculation to about n=12 ranks at most. The second method is based on a recurrence relation. It is practical up to n=170 ranks using floating point arithmetic and can provide exact frequencies to over n=300 using J extended-integer arithmetic.

Both methods agree with *Biometrika* Table 45, [\[3\]](#ref3) up to its limit of 10 ranks. Basic properties of the Kendall’s rank correlation frequency distribution against Q for n>0 are:

<table style="font-size: 0.85em">
			<col span="2" class="shaded">
			<col style="border-left: 1px solid gray">
			<tr>
				<td class="left">Distribution symmetry about:</td>
				<td class="left">(max. Q)/2</td>
				<td class="left">Q=0 frequencies:</td>
				<td class="left">1</td>
			</tr>
			<tr>
				<td class="left">Sum of the frequencies:</td>
				<td class="left">!n</td>
				<td class="left">Q=1 frequencies:</td>
				<td class="left">n-1</td>
			</tr>
			<tr>
				<td class="left">Number of frequencies, Q:</td>
				<td class="left">1+n×(n-1)/2</td>
				<td class="left">Q=2 frequencies:</td>
				<td class="left">(-1)+n×(n-1)/2 [for n&gt;1]</td>
			</tr>
		</table>

Q and n are discussed further under Method 1.

### Method 1: First principles

Taking the case for n=3, the 3 ranks have 6 = 3 × 2 × 1 permutations. Assuming that a rank order `a b c` corresponds to perfect correlation, the (minimum) number of neighbour interchanges required to transform each permutation to `a b c` is ascertained:

<table style="border-collapse: collapse; font-size: 0.9em;">
			<col span="1">
			<col span="6" style="text-align: center">
			<thead>
				<tr style="border-bottom: 1px solid #999" class="shaded">
					<td class="left">Permutations for n=3</td>
					<td>a b c</td>
					<td>a c b</td>
					<td>b a c</td>
					<td>b c a</td>
					<td>c a b</td>
					<td>c b a</td>
				</tr>
			</thead>
			<tbody>
				<tr>
					<td class="left">Required neighbour interchanges, Q</td>
					<td class="left">0</td>
					<td class="left">1</td>
					<td class="left">1</td>
					<td class="left">2</td>
					<td class="left">2</td>
					<td class="left">3</td>
				</tr>
				<tr>
					<td class="left">Frequency total for each unique Q value above</td>
					<td class="left">1</td>
					<td class="left">—</td>
					<td class="left">2</td>
					<td class="left">—</td>
					<td class="left">2</td>
					<td class="left">1</td>
				</tr>
			</tbody>
		</table>

The frequencies for each value of Q = 0, 1, 2, 3 are repeated in the table below in the row for n=3.

<table style="border-collapse: collapse; font-size: 0.7em; line-height: 1.4em; text-align: center;">
			<caption style="font-size: 1.2em;line-height: 1.4em;  margin-bottom: 0.3em">
				<span style="font-size: 1.2em">Table of Frequency Distributions for n=1 to 6 Ranks</span><br>
				The italic figures are temporary additions to aid explanation of the recurrence method
			</caption>
			<col style="border-right: 1px solid #999">
			<col span="16" style="padding: 0">
			<col span="2" style="border-left: 1px solid #999">
			<thead style="border-bottom: 1px solid #999; vertical-align: bottom;">
				<tr>
					<th>No.<br>of<br>ranks</th>
					<th colspan="16" class="center">
						Number of neighbour interchanges, Q<br>
						<span style="font-weight: normal">for the frequencies in the column below them</span>
					</th>
					<th>Sum<br>of<br>Freqs.</th>
					<th>Count<br>of<br> Qs</th>
				</tr>
				<tr>
					<th>n</th>
					<th>0</th>
					<th>1</th>
					<th>2</th>
					<th>3</th>
					<th>4</th>
					<th>5</th>
					<th>6</th>
					<th>7</th>
					<th>8</th>
					<th>9</th>
					<th>10</th>
					<th>11</th>
					<th>12</th>
					<th>13</th>
					<th>14</th>
					<th>15</th>
					<th>!n</th>
					<th>1+n×<br>(n-1)/2</th>
				</tr>
			</thead>
			<tbody style="border-bottom: 1px solid #999; vertical-align: top;">
				<tr class="shaded">
					<td class="bold">0</td>
					<td>1</td>
					<td><br><span class="italic">(-1)</span></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td>1</td>
					<td>1</td>
				</tr>
				<tr>
					<td class="bold">1</td>
					<td>1</td>
					<td></td>
					<td><br><span class="italic">(-1)</span></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td>1</td>
					<td>1</td>
				</tr>
				<tr class="shaded">
					<td class="bold">2</td>
					<td>1</td>
					<td>1</td>
					<td></td>
					<td><br><span class="italic">-1</span></td>
					<td><br><span class="italic">(-1)</span></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td>2</td>
					<td>2</td>
				</tr>
				<tr>
					<td class="bold">3</td>
					<td>1</td>
					<td>2</td>
					<td>2</td>
					<td>1</td>
					<td><br><span class="italic">-1</span></td>
					<td><br><span class="italic">-2</span></td>
					<td><br><span class="italic">-2</span></td>
					<td><br><span class="italic">(-1)</span></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td>6</td>
					<td>4</td>
				</tr>
				<tr class="shaded">
					<td class="bold">4</td>
					<td>1</td>
					<td>3</td>
					<td>5</td>
					<td>6</td>
					<td>5</td>
					<td>3<br><span class="italic">-1</span></td>
					<td>1<br><span class="italic">-3</span></td>
					<td><br><span class="italic">-5</span></td>
					<td><br><span class="italic">-6</span></td>
					<td><br><span class="italic">-5</span></td>
					<td><br><span class="italic">-3</span></td>
					<td><br><span class="italic">(-1)</span></td>
					<td></td>
					<td></td>
					<td></td>
					<td></td>
					<td>24</td>
					<td>7</td>
				</tr>
				<tr>
					<td class="bold">5</td>
					<td>1</td>
					<td>4</td>
					<td>9</td>
					<td>15</td>
					<td>20</td>
					<td>22</td>
					<td>20<br><span class="italic">-1</span></td>
					<td>15<br><span class="italic">-4</span></td>
					<td>9<br><span class="italic">-9</span></td>
					<td>4<br><span class="italic">-15</span></td>
					<td>1<br><span class="italic">-20</span></td>
					<td><br><span class="italic">-22</span></td>
					<td><br><span class="italic">-20</span></td>
					<td><br><span class="italic">-15</span></td>
					<td><br><span class="italic">-9</span></td>
					<td><br><span class="italic">-4</span></td>
					<td>120<br><span class="italic">(-1)</span></td>
					<td>11</td>
				</tr>
				<tr class="shaded">
					<td class="bold">6</td>
					<td>1</td>
					<td>5</td>
					<td>14</td>
					<td>29</td>
					<td>49</td>
					<td>71</td>
					<td>90</td>
					<td>101</td>
					<td>101</td>
					<td>90</td>
					<td>71</td>
					<td>49</td>
					<td>29</td>
					<td>14</td>
					<td>5</td>
					<td>1</td>
					<td>720</td>
					<td>16</td>
				</tr>
			</tbody>
		</table>

### Method 2: Recurrence relation

Row r+1 is based [\[4\]](#ref4) on the previous row r. Rows are calculated from row n=1 initially. To calculate the frequencies for the next row, first negate a copy of the current row and insert as shown by the italics at the bottom of the row. The row is then summed progressively from column 0 to the right to produce the next row, stopping before the *(\_1)* column, which would otherwise produce an unwanted zero.

## Notes and references

1. <a id="ref1"></a> PC system specification s/w: Windows XP home ed. 5.1.2600, Service pack 2; J v6.01c; H/W: Processor: x86 Family 15 Model 2 Stepping 9 GenuineIntel ~2605 MHz; RAM: 512 MB.
2. <a id="ref2"></a> More precisely the proportional error is in the range: (1 - 6e-15) to (1 + 4.5e-15).
3. <a id="ref3"></a> E.S. Pearson and H.O. Hartley, ed., *Biometrika Tables for Statisticians* Volume 1, 2nd Edition, 1958, Table 45, 84-5, 85-6, 88-9
4. <a id="ref4"></a> M.G. Kendall & A. Stewart, *The Advanced Theory of Statistics*, Volume 2, 3rd Edition, Inference and Relationship, Robust and Distribution-Free Procedures, Ch. 31.25 Eqn (31.34)

## Appendix: J code

### Summary

Verbs grouped according to their main application: in the last column, `n` is the number of ranks, `Q` the number of interchanges.

<table>
			<tbody style="vertical-align: top;">
				<tr><th colspan="3" class="left">Kendall’s Rank Correlation Distribution</th></tr>
				<tr>
					<td class="code left">km1</td>
					<td class="left">Original method</td>
					<td class="code left">km1 n</td>
				</tr>
				<tr>
					<td class="code left">km2</td>
					<td class="left">Recurrence method</td>
					<td class="code left">km2^:(&lt;:n) 1</td>
				</tr>
				<tr>
					<td class="code left">krfd</td>
					<td class="left">Cover function for <code>km2</code>, Frequency Distribution</td>
					<td class="code left">krfd n</td>
				</tr>
				<tr>
					<td class="left"></td>
					<td class="left">Kendall’s Rank Probability Distribution</td>
					<td class="code left">( % +/) krfd n</td>
				</tr>
				<tr>
					<td class="left"></td>
					<td class="left">
						Kendall’s Rank Cumulative Probabilities;
						used for Biometrika <a href="#ref3">[3]</a> check
					</td>
					<td class="code left">(] +/\@:% +/) krfd n</td>
				</tr>
				<tr><th colspan="3" class="left">Probability of random variation in two sets of ranks</th></tr>
				<tr>
					<td class="code left">r2q</td>
					<td class="left">Ranks to Q values</td>
					<td class="code left">A_ranks r2q B_ranks</td>
				</tr>
				<tr>
					<td class="code left">q2p</td>
					<td class="left">Q values to Probabilities</td>
					<td class="code left">n q2p Q</td>
				</tr>
				<tr>
					<td class="code left">q2t</td>
					<td class="left">Q values to Kendall’s tau</td>
					<td class="code left">n q2t Q</td>
				</tr>
				<tr>
					<td class="left"></td>
					<td class="left">Single- and double-tail comparison of rank sets</td>
					<td class="left">see Listing</td>
				</tr>
				<tr><th colspan="3" class="left">Miscellaneous</th></tr>
				<tr>
					<td class="code left">dsp</td>
					<td class="left">Display of results and inline debugging aid</td>
					<td class="code left">… dsp right_arg …</td>
				</tr>
				<tr>
					<td class="code left">ktcv</td>
					<td class="left">Kendall’s tau Critical Values</td>
					<td class="code">n ktcv nominal_probability</td>
				</tr>
			</tbody>
		</table>

### Listing

```
dsp =: 1!:2&2  NB. Utility monad: display and return argument
n =. 3         NB. n indicates the number of ranks
```

```
NB. === Kendall's Rank Correlation Distribution ====================

  NB. Both km1 (Original method) and km2 (Recurrence method) create
  NB. Kendall's Rank Correlation Frequency Distribution for n ranks.

km1 =: [: +/"1 [: = [: +/"1 (i.@! ({. +/@:> ])\."1@:A. i.)
  NB. Monad Syntax:  km1  n   Note: n>10 may fail on memory limit
```

```
km2 =: 3 : 0  NB. Monad SYNTAX:  km2^:(<:n) y=.1  Note: n>0 ; y=1
        NB. n-1 recurrences update the starting value of y
 n =. +/ _1 x: 2{. y              NB. l is the no. of Q values for
 h =. >.-: l=. >: -: ( * >:) n    NB.  the new distribution (n+1)
 ( , |.`(|.@}:)@.(2|l)) +/\  (h&{. - [: (-h)&{. (0>.h-n+1)&{.) y
)
```

```
krfd =: 3 : 0  NB. Syntax: [initial_distrbn]  krfd  iterations_reqd.
 1 krfd y      NB. Monad default x=1 sets floating point arithmetic.
               NB. x can be a previous result and/or have an 'x'
               NB.  suffix for extended integer arithmetic
:
 km2^:(y-1=#x) x  NB. Reduce effective y by 1 when x=1 (or 1x)
)

(] % +/) krfd n      NB. Probability distribution
( +/\@:% +/) krfd n  NB. Cumulative probability
                     NB. For Biometrika Table 45 Ref [1] check
```

```
NB. === Probability of Random Variations of two sets of ranks ======

r2q =: 3 : 0                      NB. Ranks to Q
NB. SYNTAX:  [A_ranks]  r2q  B_ranks   Monad assumes y pre-ordered
 if.  (] -&# ~.) y do. 'r2q: y ranks indiscrete' return. end.
 Q =. +/ ({. +/@:> ])\. y         NB. Exit with Q value
:
 if.  (] -&# ~.) x do. 'r2q: x ranks indiscrete' return. end.
 r2q (/:x) { y                    NB. Reorder y as ascending x
)
```

```
  NB. Convert Q to probability in single- or double-tail test
q2p =: 4 : 0  NB. SYNTAX:  no._of_ranks  qp2  Q_value
 if. -.y e. i.>: l=.-:(* <:) x do. 'q2p: incompat. args' return. end.
 st =. (!x)%~ +/ (y+1){. d=:krfd x  NB. Single-tail probability
 z =. y <. l - y                    NB. Double-tail: complement y
 dt =. 1 <. +: (!x)%~ +/ (z+1){. d  NB. Double-tail probability
 hd =. '   n       Q  Single-tail Double-tail', LF  NB. Headings
 hd, 4 8 10j6 12j6": x, y, st, dt   NB. Results
)
```

```
  NB. Convert Q to Kendall's tau
q2t =: 1: - 4: * ] % ( * <:)@[  NB. SYNTAX:  n  q2t  Q
```

```
  NB. Rank comparison to single-, double-tail probability
'r0 r1 r2' =: 0 1 2 3 4 5  ;  5 4 3 2 1 0 ; 6?.6  NB. Rank data
6 q2p r0 r2q r0    NB. Both results very significant
6 q2p r0 r2q r1    NB. Double-tail result very significant
6 q2p r0 r2q r2    NB. Neither result significant
```

```
NB. === Miscellaneous ==============================================

  NB. Critical values of Kendall's tau in cumulative probability
ktcv =: 4 : 0  NB. SYNTAX:  n  ktcv  Nominal_s-tail_signif._prob.
 d =. (+/\@krfd % !) x      NB. Cumulative probability
 x q2t (d <: y) i: 1        NB. Critical value of Kendall's tau
)
```
