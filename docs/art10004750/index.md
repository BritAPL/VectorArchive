---
vid: '10004750'
title: 'Letter: Non-Syllogistic Mathematical Proof'
authors:
- Gérard Langlet
volume: '12'
issue: '1'
page: '123'
status: not online
issue_tab: n1-july-1995
---

Gérard Langlet
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 123](../12/1/VOL.12-NO.1-JULY-1995.pdf#page=125)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.12 No.1</p>
<p>Non-Syllogistic Mathematical Proof An Answerto Vector 114 p 126 From: Gérard Langlet May 1995</p>
<p>Given any sophisticated function f of any number of variables with the result r=f(x,j,Z,...), We can experiment with as many runs as we wish on anyavailable computer.</p>
<p>First, we shall reada series of values for x, y, 2, ... as well as forinitial r (e.g. set to 0), and store them into the linear memory of the computer (note: we do not imposethat such variables arguments as well as result(s) be numericalscalars; they can be heterogeneousarraysof arrays). Second, we take a snapshot of V, the occupied sequence of the computer memory for such a problem. Of course, this snapshot ignores the input- conversion format routines that were used, and ignores the naming of the variablesas wellas their respective lengths. Y is just a binary sequence of 0s and 1s with length N matching p V. Third, we let the function operate.</p>
<p>Fourth, we take a second snapshot of W, the binary sequence of the modified memory at the samelocations(it has the same length ¥).</p>
<p>Now, we shall repeat the procedure with random values of the argument variables (a Monte Carlo technique), at least W times, let us say P times. So, we get two rectangular arrays AV and AW, each with P rows and N columns in which the columns are the successive Vs and Ws respectively.</p>
<p>Is it always possible for N=P, to find a matrix M so that AW is M. AV (if the period standsfor the modulo2 matrix product, i.e. #.4 in APL)? If the answerto sucha question is YES, we have a proof that function f may be replaced by a matrix product, then a proof that f is a linear function even if it is thought of as nonlinear! (Even if ¥ had to be replaced by N? or by anystill larger butfinite number, this would hold!)</p>
<p>It has to be so, because in modulo2 algebra, nonlinearities do vanish.In order to simplify, let f be a secret nonlinear fn of just one variable, let us say a numeric variable x coded in floating-point arithmetic, e.g. with 64 bits. The result r=f(x) is supposed to be another numeric variable of the same type; so, storing r into anotherlocation than the one used for x would waste memory; oncethe value of x is used byf, it becomesuseless, so we shall store the result at the samelocation; our application has become x=f(x).</p>
<p>Theinitial value for x can be chosen among 2values (supposing our arithmetic has no nan (not-a-number).</p>
<p>123</p>
<p>VECTOR Vol.12 No.1</p>
<p>Whatwesay here for numbers codedin 64 bits also holds for numbers coded in n bits, with n small, e.g. 2. So, for pairs of bits, initial x has to be one of the 4 couples (00) (01) (10) (11). The result r or final x has to be also a memberof the same 4 couples. Function f may transform (00) into (00) or (01) or (11) or (11),ie. 4 possibilities.</p>
<p>Function f may transform (01) into (00) or (01) or (11)or(11), i.e, 4 possibilities again.</p>
<p>Etc...</p>
<p>Altogether, one could think we have a restricted choice between 4x4x4x4 functionsf, ie. 256 functions. In fact, this is much too much...</p>
<p>Functionf has to reduce to an algorithm which:</p>
<p>If the first bit of a couple ofbits is 0, either swapit to 1 or don t (2 possibilities). Then either swap the secondbit or don t (2 more possibilities).</p>
<p>If the first bit of a couple ofbits is 1, either swapit to 0 or don t (2 possibilities). Then either swap the secondbit or don t (2 more possibilities).</p>
<p>Obviously, only 24 i.e. 16 possibilities exist for f, not 256.</p>
<p>Only 24 i.e. 16 possibilities exist for matrix M: 72 2p2 in 0-origin.</p>
<p>Amongthe 16 possible matrices, only 6 are invertible; only 4 of these are self- inverse (modulo 2) which correspondsto a reversible function (among which the do-nothing fn or identity matrix, the mirror-function i.e. ¢ in APL or the anti- identity matrix, and 2-genitons Gh and Gy,responsible for the cognitive and the helix transform respectively).</p>
<p>The same reasoning holdsfor anyfinite value of n.</p>
<p>Without repeating the geniton story, the proof that at least numerically,i.e. with any type of computer which would use any type of representation based on bits, in order to code any type of data all functions coalesce into linear functions, implies that no function may be chaotic, except if one allowsstrange rules to alter precision, such as the ones which are used e.g. to subtract values in floating-pointarithmetic and transform PI either into 0 on the PC and 360-like mainframesor into « on the Macintosh.</p>
<p>124</p>
</details>
