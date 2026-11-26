---
vid: '10009550'
title: Fast Fibbing
authors:
- Sykes
volume: '3'
issue: '4'
page: '121'
status: not online
issue_tab: n4-april-1987
---

Sykes
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 121](../3/4/VOL.3-NO.4-APRIL-1987.pdf#page=123)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.3 No.4</p>
<p>MoreFibbing by Dr Alan Sykes</p>
<p>John Sullivan s article VECTOR Vol.3, No.1), raises some importantissues concerning APL andits relationship with other languages. APL providesonly ratherinefficient tools (some moreinefficient than others!) for solving those computational problems typified by the Fibonacci sequence. The most natural methodis to consider looping (or function recursion) and the search for alternative methods, whilst intellectually stimulating, often give computer scientists (justified?) cause for concern! Afterall, John Sullivan s solution (FIB2) isn t recursive, nor doesit involve only integer arithmetic. However, to avoid suchcriticisms completely,it would be necessary to write our program in machine-code ~ undoubtedly the best languagefor recursiveinteger arithmetic! Many computer scientists would not readily follow this course, and clearly some compromise between the efficiency of the program execution and theefficiency of the program construction is usually made. What we APLers must do is to show our readiness to construct auxiliary processorsforthose applications which demand them,thereby showing our computer science colleaguesthat we can have our cake and eatit . In a wider context of computing solutionsto difference (or differential) equations, John Sullivan hasa point. Numerical solution ofeven simpledifference equations can be fraught with thelikelihood of numerical instability. Hence, if an exact solution is known, this may lead to anefficient and error-free method of computation. In this context, FIB2 is a good solution. Incidentally, at the risk of invoking my own criticism, may I present an APL2 solution (which I hasten to add is much slower than FIB2). FIB3 calculates the first 2N Fibonacci numbers: VRSPIBS (1) Ae, (te.x\Woc2 26111 2)C,,27</p>
<p>This example is interesting in the context of becoming familiar with useful second- generation APL idioms, Itis inefficient because the powers of the matrix 1 1 1 2 created by +ex\Wpe2 2p 1112 are not created recursively. The scan operation \ operates recursively with some binary operations; for example +\1 10000</p>
<p>is muchfaster than =\t 10000 because + is associative whereas - is not. Although +.x is an associative operation on matrices, the APL2 interpreter does not recognise this and hence the inefficiency.</p>
<p>fam very grateful to John Scholes of Dyadic Systemsfor his helpful comments on FIB3 during APL 86 at Manchester.</p>
<p>121</p>
<p>APL CONSULTANTS LONDON &amp; READING</p>
<p>Account Managers (6 yearst) 25K Senior Consultants (4-6 years} to 21K Consultants (2-4 years) to 17K Junior Consultants (1-2 years) to 13K</p>
<p>Are your APLskills and potential being recognised and rewarded? Cocking &amp; Drury consultants have been implementing successful decision support applications for 10 years, with clients who appreciate the productivity e nefits of APL. In our professional team you will experience a range of APL environments- APL*Plus, VSAPL, APL2 and Unix, on both mainframes and micros. You will also be developing systems which,increasingly, needto interface with non-APLInformation Centre products. Of course as the leading APL consultancy,in a rapidly expanding market, we offer a rewarding career withfirst class benefits - profit sharing, free health insurance, and a non contributory pension.</p>
<p>=3== COCKING&amp;DRURYLID. ee eeOCRAe UN CA aon Are oer</p>
<p>155 Friar Street, Reading, RG] JHE</p>
</details>
