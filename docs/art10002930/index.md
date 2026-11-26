---
vid: '10002930'
title: Polynomial Curve Fitting
authors:
- J.B. Douglas
volume: '3'
issue: '4'
page: '117'
status: not online
issue_tab: n4-april-1987
---

J.B. Douglas
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 117](../3/4/VOL.3-NO.4-APRIL-1987.pdf#page=119)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.3 No.4</p>
<p>Polynomial CurveFitting by J B Douglas</p>
<p>[refer to Dick Bowman s and the Editor s comments (VECTOR Vol.3, No.1, pp 99-100) on the different responses of different APL systems to a dyadic use of domino for polynomial curvefitting. Dick s remark, thatit is interesting when the same code applied to the same data gives differentresults, is certainly valid. But without downplaying the importance of having good algorithmic and numerical procedures( the code ), [ should like to emphasize the other aspect, the data, of whatis a perpetual problem. The perpetual problem is to devise methodssufficiently robust for appropriate aralysis of any data; it is perpetual because whatever method maybedevisedit is always possible to invent data for which the method will misbehave. Whatis often soughtis a . . . fitted curve . . . to look nice and smooth . . . So let s graph Dick s data (see Figure 1). The curious behaviourat odd and even years is obvious, and no meaningful smooth curve seems plausible in particular, a cubic cannot possibly reproduce the markedoscillation of the Y-valuesat the later years X. To obviatereadily removable DOMAIN or LIMIT ERROR difficulties, which may arise simply because large numberslike 1995areraised to high powersin thecalculations, subtract 1984 from X. This gives numericallyreliable regression coefficients and a smoothfitted curve (see Figure 2) whichis basically nonsense when thoughtof as representing the data. (APL*PLUSPCis capable of giving an equivalentto this result without havingto subtract the 1984.) A moredetailed analysis shows that of the four regression coefficients, only thefirst, associated with the constantterm,is large (comparedwithits standard error) andlikely to be meaningful, and that the mean square aboutthefitted cubic really isn t much less than that abouta constantfitted value. The numericalresults are as follows, with the datafirst: X: 1234567 8 91011 Y: 37 36 39 38 41 39 43 38 43 35 42 +B+YRA+Xe.*#0 1 2 3 returnsthe regression coefficients 34.212 2.040 -0.225 0.00777 for which the standarderrorsare: 5.1 3.5 0.67 0.037; fitted values are calculated from a+.x3 Inspectingthe data, the graph suggests that two distinct quadratics would represent them wellat the years recorded: one quadratic for the odd, and anotherfor the even, years. This fitting can be achieved by replacing the matrix A by the matrix C; both are shown below.</p>
<p>M17</p>
<p>Vol.3 No.4 VECTOR</p>
<p>A c »o oto. 1 1 1 0 0 1 2 4 8 1 0 0 2 4 1 39 2 1 3 9 0 0 1 4 16 64 1 0 0 4 16 Tay 12d 1331 T dW i 0. 0 Then executing: YBC thefit returnsfive coefficients all of which are large compared with their standarderrors, being displayed in Figure 3. (Thecoefficients and their standard errors are:</p>
<p>34.141 2.082 -0.1234 1.632 -0.1507 0.72 0.29 0.024 0.31 0.029)</p>
<p>So in somesense this smoothly, and quite closely, reproducesthedata. It leaves uncertain what meaning should beattached to interpolated values but so do the data. J believe the principal messageto be that one should always look at the data and whatany procedure predicts for the data, to see thatthe question and the answerare reasonable. The misbehaviour of domino is minor compared with the mis-specification of a cubic for the representation ofthe data. Figure 1. Bowman s original data 45.0] | | ° ° 1 42.51] | ° + 5 ¥ ! ° | v 40.04 A | L | ° ° u ! | ° ° E s 37.5] I ° | t °o | | | I 1 | ! 1 ' ! ° t 35.04 1994 1996 1984 1986 1988 1990 1992 YEARS - X VALUES &gt;</p>
<p>118</p>
<p>VECTOR Vol.3 No.4</p>
<p>Figure 2. Original data witha fitted cubic 45.0] | | 1 t ° ° 42.54 + | ° | Yy | ° t v 40.01 * * * * * * * A t * LB [ ° ° u | * EB | ° ° § 37.51 * | ° | t * ° | 35.0 | | | t t 1 | | | ° ' | 1984 1986 1988 1990 1992 1994 1996 YEARS - X VALUES + O OBSERVED * PITTED CURIC</p>
<p>Figure 3. Original data with a fitted pair of quadratics 45.01 | | | | ° * ! 42.5] * i + I | * * y ' ° { v 40,0[ A | * L ( ° ° u | * * E | ° Q Ss 37.5| * ! ° * 1 | * 0 [ * 35.0] t | | | | | | ° t | 19984 1986 1988 1990 19932 1994 1996 YEARS - X VALUES + Oo OBSERVED * PITTED QUADRATICS</p>
<p>119</p>
</details>
