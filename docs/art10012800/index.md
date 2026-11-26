---
vid: '10012800'
title: 'Review: "Calculus" by Ken Iverson'
authors:
- Keith Smillie
byline: by Keith Smillie
volume: '10'
issue: '4'
page: '39'
source: trad/v104/smillie104_39.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- reviews
- mathematics and statistics
- education
---

by Keith Smillie <span class="affil"></span> <span class="email"></span>
{ .byline }

Iverson, Kenneth E., 1993. Calculus. Iverson Software Inc., 33 Major Street, Toronto M5S 2K9, Canada. 130 pp., $25.00 + 7% GST. (US Dollars)

Anyone visiting a university bookstore cannot help but notice how large, expensive and even pretentious many of the mathematics texts, and indeed texts in a variety of other disciplines, have become. At the University of Alberta, for example, the main text in one of the introductory calculus courses has almost 1100 pages and costs $71.75. Since there is also a 400-page solutions manual costing $27.65 and a 150-page laboratory manual costing $8.65, a student would have to spend over one hundred dollars for more than sixteen hundred pages of material for this one course which would be for many students only one of five courses being taken in a term. Whether the present-day student receives a fair return on such a large investment of money, reading time and the physical effort in handling the texts is of course another matter. (By contrast this reviewer’s first calculus text was an unassuming, even drab, little book of some four hundred pages costing less than four dollars although these were immediate postwar prices.)

The above thoughts have been prompted by an examination of Calculus, Kenneth Iverson’s latest monograph from Iverson Software Inc., which because of its size, scope and price stands in sharp contrast to the present-day texts just mentioned. Its content might best be judged by the following paragraph in the Preface:

The scope is broader than is usual in an introduction, embracing not only the differential and integral calculus, but also the difference calculus so useful in approximations, and the partial derivatives and the fractional calculus usually met only in advanced courses. Such breadth is achievable in small compass not only because of the adoption of informality, but also because of the executable notation employed. In particular, the array character of the notation makes possible an elementary treatment of partial derivatives in the manner used in tensor analysis.

The Preface concludes with the statement that

The text is paced for a reader familiar with polynomials, matrix products, linear functions, and other notions of elementary algebra; nevertheless, full definitions of such matters are also provided. The text, then, consists of the following eight chapters: Introduction, Differential calculus, Vector calculus, Difference calculus, Fractional calculus, Properties of functions, Interpretations & applications, and Analysis.

There is one page of references, and a one-page Language Summary given on the last page of the text rather than more conveniently on the outside back cover as is usual with most ISI publications.

The author has written Calculus in his characteristic terse style which is described very well by the following sentence beginning at the bottom of the first page of Chapter 1:

To avoid distractions from the central topic of the calculus, we will introduce the necessary notation with a minimum of comment, assuming the reader can grasp the meaning of new notation from context, from simple experiments on the computer, from the language summary in Appendix A, from the Introduction and Dictionary, or from the study of more elementary texts such as Arithmetic.

This is followed by two pages of simple examples illustrating some of the main features of J and a further two pages of exercises to reinforce these ideas. The remainder of the chapter consists of an overview of some of the topics to be developed in the remainder of the text, together with the introduction of a number of verbs for handling script files, constructing graphs, and defining some basic mathematical concepts such as circular and hyperbolic functions, matrix products, polynomials and complex numbers.

As would be expected the text makes considerable use of the recently introduced general derivative adverb D. where u D n is the nth derivative of u, and the scalar derivative adverb D=. ("0)(D.1) such that the expression f D x gives the derivative of the function f evaluated at x. Iverson also uses the integral or anti-derivative adverb I such that f I x gives the area under the graph of f from 0 to x. Whereas the adverb D is a primitive in Version 7 of J, the adverb I is defined in terms of a sequence of verbs which, for the parameters used for the examples and exercises, is equivalent to approximating the function by four groups of a polynomial of order ten. Persons wishing to obtain some idea of the style and presentation might read the discussion of these two adverbs in the first eight pages of Chapter 2. As an example of the use of D and I, consider the verb

```
 N=. (%:@o.@2:) (%~) ^@-@-:@*:
```

for the probability density function of the standard normal distribution. Then N D 1 which has the value \_0.241971 is the value of the first derivative of the probability density function evaluated one standard unit from the origin, and N I 1 which is 0.341345 is the area under the standard normal distribution from the origin out to one standard unit. Also the expression

```
 5.1 8.5 8.5 ": (],.N,"0 (N I)) x
```

gives a formatted table of ordinates and areas of the standard normal distribution for any appropriate list x.

As another example, the verb

```
ChiSq=. ((]^(-:@<:@<:@[))*(^@(-@-:@]))) % ((2: ^ -:@[) * (!@(<:@-:@[)))
```

is the probability density function for the chi-square distribution, and the expression 10&ChiSq I 18.3, where 18.3 is the 5% critical value for the chi-square distribution with ten degrees of freedom, is 0.949891.

At the end of Chapter 4, Difference Calculus, Iverson introduces the verb

```
 w=. _1&^ * (i. ! i. - ])@>:"0
```

for finding the weights required for calculating successive differences, and, for example, w 3 is equal to \_1 3 \_3 1, the weights for third differences. Then we may define the verb

```
 Diff=. '' : '(>:x.) mp&(w x.) \ y.'
```

where mp=. +/ . \* is the matrix product, which gives differences of an arbitrary order, and, for example, the expression 2 Diff ^&3 i. 8 is equal to 6 12 18 24 30 36, the second differences of the cubic function. Finally we may define the verb

```
 DiffTable=.(],.(],.{.,1&diff)@*:)
```

which gives a three-column table with argument values in the first column, their squares in the second column, and the first differences of the squares in the third. (We might note that one of the programs demonstrated at a conference held at the University Mathematical Laboratory, Cambridge, in June 1949 to mark the inauguration of the EDSAC tabulated the positive integers from 1 to 32, and the squares and first differences. Persons interested in the evolution of programming languages since that time might compare the flow diagrams and program listings for the EDSAC program given on pages 395 to 401 of Randell (1975) with the expression DiffTable>:i.32 for the same calculation in J.)

The origin of some of the ideas in the present monograph may be found in the last chapter of the author’s earlier Arithmetic. Indeed, Iverson states in the Preface to Calculus that the subject is introduced in the same informal manner as arithmetic was in the earlier work. However, in this reviewer’s opinion, Calculus is a much more difficult, even formidable, work which may well deter many readers. Indeed, Iverson’s quite valid comment in the Preface that “introductory courses often succeed only in turning students away from mathematics, and from the many subjects in which the calculus plays a major role” may well apply to the present monograph. At the APL93 conference Iverson gave a tutorial “Teaching Calculus” in which he introduced the ideas and many of the details given in Calculus. The diskette distributed at that time contained a number of scripts used during the presentation. A careful study of these scripts could be a most helpful introduction to the present work. After a further study of Calculus, the reader could then, as the present reviewer has done, have the pleasure of approaching some old problems in a more general and satisfying manner than has been previously possible.

### References

1. Iverson, K. E., 1991. Arithmetic. Iverson Software Inc., Toronto.
2. Iverson, K. E., 1993. J Introduction & Dictionary. Iverson Software Inc., Toronto.
3. Randell, Brian, 1975. The Origins of Digital Computers. Selected Papers. Second edition. Springer-Verlag, New York.
4. Standard Mathematical Tables. Nineteenth Edition, 1971. The Chemical Rubber Company, Cleveland, Ohio.

(webpage generated: 4 April 2006, 18:47)
