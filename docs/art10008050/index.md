---
vid: '10008050'
title: 'Review: “Calculus” by Ken Iverson'
authors:
- Keith Smillie
volume: '10'
issue: '4'
page: '39'
status: not online
issue_tab: n4-april-1994
---

Keith Smillie
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 39](../10/4/VOL.10-NO.4-APRIL-1994.pdf#page=41)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.4</p>
<p>Calculus by Kenneth E Iverson reviewed by Keith Smillie</p>
<p>Iverson, Kenneth E., 1993. Calculus. verson Software Inc., 33 Major Street, Toronto M5S 2K9, Canada. 130 pp., $25.00 + 7% GST. (US Dollars)</p>
<p>Anyone visiting a university bookstore cannot help but notice how large, expensive and even pretentious many of the mathematics texts, and indeed texts in a variety of other disciplines, have become. At the University of Alberta, for example, the main text in one of the introductory calculus courses has almost 1100 pages and costs $71.75, Since there is also a 400-page solutions manual costing $27.65 and a 150-page laboratory manualcosting $8.65, a student would have to spend over one hundred dollars for more than sixteen hundred pages of material for this one course which would be for many students only one offive courses being taken in a term. Whether the present-day student receives a fair return on sucha large investment of money, reading time and the physical effort in handlingthe texts is of course another matter. (By contrast this reviewer's first calculus text was an unassuming, even drab,little book of some four hundred pages costing less than four dollars although these were immediate postwar prices.)</p>
<p>The above thoughts have been prompted by an examination of Calculus, Kenneth Iverson'slatest monograph from Iverson SoftwareInc., which becauseofits size, scope andprice stands in sharp contrast to the present-daytexts just mentioned. Its content might best be judged by the following paragraphin the Preface: The scopeis broaderthanis usualin an introduction, embracing notonly the differential andintegral calculus, but also the difference calculus so useful in approximations, andthepartial derivatives andthe fractional calculus usually metonly in advanced courses. Such breadthis achievable in small compass not only becauseof the adoption ofinformality, but also because of the executable notation employed. In particular, the array characterof the notation makes possible an elementary treatmentofpartial derivatives in the mannerused in tensor analysis.</p>
<p>The Preface concludes with the statement that Thetextis paced for a reader familiar with polynomials, matrix products, linear functions, and othernotions of elementary algebra; nevertheless, full definitions of such matters are also provided. Thetext, then, consists of the following eight chapters: Introduction, Differential calculus, Vector calculus, Difference calculus, Fractional calculus, Properties of functions, Interpretations &amp; applications, and Analysis.</p>
<p>39</p>
<p>VECTOR Vol.10 No.4</p>
<p>Thereis one pageofreferences, and a one-page Language Summarygiven on the last page of the text rather than more conveniently on the outside back coverasis usual with most IS] publications.</p>
<p>The author has written Calculus in his characteristic terse style whichis described very well by the following sentence beginning at the bottom of the first page of Chapter1:</p>
<p>To avoid distractions from the central topic of the calculus, we will introduce the necessary notation with a minimum of comment, assumingthe reader can grasp the meaning of new notation from context, from simple experiments on the computer, from the language summary in Appendix A, from the Introduction and Dictionary, or from the study of more elementary texts such as Arithmetic.</p>
<p>Thisis followed by two pagesof simple examplesillustrating someof the main features of J and a further two pagesof exercises to reinforce these ideas. The remainder of the chapter consists of an overview of some ofthe topics to be developed in the remainder of the text, together with the introduction of a numberof verbsfor handlingscriptfiles, constructing graphs, and defining some basic mathematical concepts such as circular and hyperbolic functions, matrix products, polynomials and complex numbers.</p>
<p>As would be expected the text makes considerable use of the recently introduced general derivative adverb D. where u D n is the nth derivative of u, and the scalar derivative adverb D=. ("0)(D.1) such that the expression f D x gives the derivative of the function f evaluated at x. Iverson also uses the integral or anti-derivative adverb 1 such that £ I x gives the area under the graph of f from 0 to x. Whereas the adverb D is a primitive in Version of J, the adverb is defined in terms of a sequence of verbs which, for the parameters used for the examples and exercises, is equivalent to approximating the function by four groups of a polynomial of order ten. Persons wishing to obtain someidea of the style and presentation mightread the discussion of these two adverbsin thefirst eight pages of Chapter 2. As an exampleof the useof D and I, consider the verb</p>
<p>N=. (%:@0.@2:) (%~) a@-@-:@«:</p>
<p>for the probability density function of the standard normal distribution. Then N D 1 which has the value _0.241971 is the value of the first derivative of the probability density function evaluated one standard unit from the origin, and N 1 1 which is 0.341345 is the area under the standard normaldistributi on from the origin out to one standard unit. Also the expression 5.4 8.5 8.5 ": (],.N,"0 (N I}) x</p>
<p>40</p>
<p>VECTOR Vol.10 No.4</p>
<p>gives a formatted table of ordinates and areas of the standard normal distribution for any appropriatelist x.</p>
<p>As another example, the verb</p>
<p>Chisqe. ((Ja(-:@&lt;:@&lt;:@[)})*(a@(-@-:@J))) % ((2: a -:@f) » (1@(&lt;:@-:@f)))</p>
<p>is the probability density function for the chi-square distribution, and the expression 408ChiSq I 18.3, where 18.3 is the 5%critical value for the chi- square distribution with ten degrees of freedom, is 0.949891.</p>
<p>Atthe end of Chapter4, Difference Calculus, Iverson introduces the verb</p>
<p>we, 188 * (i. } i. - 1)@&gt;:"0</p>
<p>for finding the weights required for calculating successive differences, and, for example, w 3 is equal to_1 3 _3 1, the weights for third differences. Then we may define the verb Diff=. 't ; t(&gt;:x.) mp&amp;(w x.) \ y.t</p>
<p>where mp=. +/ . * is the matrix product, which gives differences of an arbitrary order, and, for example, the expression 2 Diff 83 i. 8 is equal to 6 12 18 24 30 36, the second differences of the cubic function. Finally we may define the verb DifeTable=.(1,.(],.{.,18diffj@«:)</p>
<p>whichgives a three-column table with argumentvalues in thefirst column, their squares in the second column, andthefirst differences of the squaresin the third. (We might note that one of the programs demonstrated at a conference held at the University Mathematical Laboratory, Cambridge, in June 1949 to mark the inauguration of the EDSAC tabulated the positive integers from 1 to 32, and the squares andfirst differences. Persons interested in the evolution of programming languagessince that time might compare the flow diagrams and program listings for the EDSAC program given on pages 395 to 401 of Randell (1975) with the expression Dif fTable&gt;: 1.32 for the same calculationinJ.)</p>
<p>The origin of someof the ideas in the present monograph may be foundin the last chapter of the author's earlier Arithmetic. Indeed, Iverson states in the Preface to Calculus that the subject is introduced in the same informal manner as arithmetic was in the earlier work. However, in this reviewer's opinion, Calculus is a much more difficult, even formidable, work which may well deter many readers. Indeed, Iverson s quite valid comment in the Preface that introductory</p>
<p>41</p>
<p>VECTOR Vol.10 No.4</p>
<p>courses often succeed only in turning students away from mathematics, and from the many subjects in which the calculus plays a major role may well apply to the present monograph. At the APL93 conference Iverson gave a tutorial Teaching Calculus in which he introduced the ideas and manyofthe details given in Calculus. The diskette distributed at that time contained a number of scripts used during the presentation. A careful study of these scripts could be a most helpful introduction to the present work. After a further study of Calculus, the reader could then, as the present reviewer has done, have the pleasure of approaching some old problems in a more general and satisfying manner than has been previously possible.</p>
<p>References Iverson, K. E., 1991. Arithmetic. lverson SoftwareInc., Toronto.</p>
<p>Iverson, K. E., 1993. J Introduction &amp; Dictionary. Iverson Software Inc., Toronto.</p>
<p>Randell, Brian, 1975. The Origins of Digital Computers, Selected Papers. Second edition. Springer-Verlag, New York.</p>
<p>Standard Mathematical Tables. Nineteenth Edition, 1971. The Chemical Rubber Company, Cleveland, Ohio.</p>
<p>42</p>
<p>VECTOR Vol.10 No.4</p>
<p>RECENT MEETINGS This section of Vector documents all British APL Association meetings, and any other events of interest to the APL world. If you have recently attended any gathering which you feel would be interesting to Vector readers, please let the Editor have a brief note, and wewill include it here.</p>
<p>In this issue, we document Donald McIntyre s J Workshop, Gary Littlejohn svisit to the APL centres in Russia, and the February British APL Association meeting.</p>
<p>43</p>
</details>
