---
vid: '10005290'
title: Step by Step Analysis of Variance
authors:
- Alan Mayer
byline: by Alan Mayer (a.d.mayer@swansea.ac.uk)
volume: '10'
issue: '1'
page: '27'
source: trad/v101/mayer101_27.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- mathematics and statistics
- education
---

*by Alan Mayer*  ([a.d.mayer@swansea.ac.uk](mailto:a.d.mayer@swansea.ac.uk))
{ .byline }

The statistical procedure called Analysis of Variance (or ANOVA for short) was designed by statisticians with the specific purpose of striking terror into the hearts of all who dare to practise the Black Art (hereafter called Stats, or, for our friends over The Pond, Stat) without consulting the gurus. It is full of terms like Error Sum of Squares, Adjustment for the Mean, Degrees of Freedom... Of course, lots of people do their Stats these days with the help of big Statistics Software Packages, which do their best to hide the complicated bits and present neat little tables of results. This is very satisfactory for the Average Person – but we’re not Average Persons, we’re APLers and we want to find out what’s going on inside the packages! And with a little bit of APL that’s exactly what we’ll do.

First let’s have a look at a typical ANOVA problem. Suppose a gardener has four propagators, with six plants in each propagator, and he wants to try out four fertilisers, A, B, C and D. On a certain day, he treats all the plants in the first propagator with fertiliser A, all the plants in the second propagator with fertiliser B, and so on. After a week he measures the growth of each plant in mm. His results may look something like this:

```
                    Growth (mm)
Fertiliser A    8   6   6   5   4   7
Fertiliser B    2   3   3   1   7   5
Fertiliser C    5   3   1   4   4   3
Fertiliser D    7   7  15   7  11   8
```

The sort of question we might want to ask now is “Have the different fertilisers caused different amounts of growth?” Let’s get the data into APL and experiment a bit.

Enter the data into a table:

```
     TAB←4 6⍴8 6 6 5 4 7 2 3 3 1 7 5 5 3 1 4 4 3 7 7 15 7 11 8
     TAB
 8 6  6 5  4 7
 2 3  3 1  7 5
 5 3  1 4  4 3
 7 7 15 7 11 8
```

Are the rows different? Let’s try adding them up:

```
     +/TAB
36 21 20 55
```

Certainly the totals seem to be different – Fertiliser D has produced a total growth of 55 mm, while fertilisers B and C have produced only 21 and 20 respectively. But does this mean that fertiliser D is better than the others? After all, we would be most surprised if the totals were all exactly the same, so some sort of difference is to be expected. How big does the difference have to be for us to consider it meaningful or, as statisticians say, significant?

The answer to this problem depends on the amount of variation in each of the rows of figures. If all the values in the first row (i.e. the plants in the first propagator, treated with fertiliser A) are close to the average value of 36÷6 = 6mm, and all the values in the second row are close to 21÷6 = 3.5mm and so on, then these differences will be significant. On the other hand, if the figures vary a lot, the differences may be put down to “random variation”. This is where the idea of Analysis of Variance comes in.

We need a different approach – one in which we measure how much variation there is in the figures altogether, and then measure how much of that variation can be explained by the differences between the averages of the rows, and how much of it is due to the variation inside the rows themselves.

We will start by working out the Grand Mean (the average of all the figures) and subtracting it from the whole table:

```
       GM←(+/,TAB)÷⍴,TAB
       GM
5.5
```

So the average growth over all the plants is 5.5 mm.

```
     TAB2←TAB-GM
     TAB2
 2.5  0.5  0.5 ¯0.5 ¯1.5  1.5
¯3.5 ¯2.5 ¯2.5 ¯4.5  1.5 ¯0.5
¯0.5 ¯2.5 ¯4.5 ¯1.5 ¯1.5 ¯2.5
 1.5  1.5  9.5  1.5  5.5  2.5
```

With the removal of the mean, we are left with a table representing the total variation about that mean, but what is needed is a single measure.

Let us experiment:

```
     +/,TAB2
0
```

Of course, adding the values together achieves nothing, as the positive numbers cancel out the negative ones.

```
     +/,|TAB2
57
```

Our second attempt, adding together the absolute values of the entries in the table is more hopeful. In fact, for mathematical reasons, a more suitable measure is obtained by summing the squares of the entries:

```
     TOTSS←+/,TAB2*2
     TOTSS
230
```

This value, the Total Sum of Squares, is the quantity which must be apportioned between the two causes previously described (between-row variation and within-row variation).

Next we turn our attention to the means of the four rows:

```
     +/TAB2
3 ¯12 ¯13 22
```

These are the row totals, which now sum to 0. The means may be found by dividing each row total by 6 (i.e. (½TAB2)\[2\]):

```
     ROWMEAN←(+/TAB2)÷(⍴TAB2)[2]
     ROWMEAN
0.5 ¯2 ¯2.166666667 3.666666667
```

To isolate that part of TOTSS which is attributable to the difference between these row means, we must subtract 0.5 from the first row of TAB2, ¯2 from the second row, and so on.

To do this we create a matrix by reshaping ROWMEAN:

```
     ⎕PP←6
     6 4⍴ROWMEAN
  0.5 ¯2 ¯2.16667 3.66667
  0.5 ¯2 ¯2.16667 3.66667
  0.5 ¯2 ¯2.16667 3.66667
  0.5 ¯2 ¯2.16667 3.66667
  0.5 ¯2 ¯2.16667 3.66667
  0.5 ¯2 ¯2.16667 3.66667
```

This first effort is in fact the transpose of the required matrix:

```
     ⍉6 4⍴ROWMEAN
   0.5      0.5      0.5      0.5      0.5      0.5
  ¯2       ¯2       ¯2       ¯2       ¯2       ¯2
  ¯2.16667 ¯2.16667 ¯2.16667 ¯2.16667 ¯2.16667 ¯2.16667
   3.66667  3.66667  3.66667  3.66667  3.66667  3.66667
```

So our third table is obtained by

```
     TAB3←TAB2-⍉6 4⍴ROWMEAN
     TAB3
   2        0         0       ¯1        ¯2         1
  ¯1.5     ¯0.5      ¯0.5     ¯2.5       3.5       1.5
   1.66667 ¯0.333333 ¯2.33333  0.666667  0.666667 ¯0.333333
  ¯2.16667 ¯2.16667   5.83333 ¯2.16667   1.83333  ¯1.16667
```

This rather cumbersome method of creating the matrix of row means can be avoided by use of the more powerful (and perhaps more APL-like) dyadic transpose. Consider the three-dimensional matrix TAB2°.-ROWMEAN. To produce the (i,j)th component of TAB3 we need the (i,j)th component of TAB2 minus the ith component of ROWMEAN. In other words, the (i,j,i)th component of TAB2°.-ROWMEAN. Hence the required left argument for the dyadic transpose is 1 2 1:

```
     1 2 1⍉TAB2∘.-ROWMEAN
   2        0         0       ¯1        ¯2         1
  ¯1.5     ¯0.5      ¯0.5     ¯2.5       3.5       1.5
   1.66667 ¯0.333333 ¯2.33333  0.666667  0.666667 ¯0.333333
  ¯2.16667 ¯2.16667   5.83333 ¯2.16667   1.83333  ¯1.16667
```

TAB3 represents the variation which cannot be explained in terms of the differences between the row means.

The corresponding sum of squares is therefore called the Error Sum of Squares:

```
     ESS←+/,TAB3*2
     ESS
95.6667
```

Of the original Total Sum of Squares (230), we have therefore attributed all but the remaining Error Sum of Squares (95.6667) to the effect of the different fertilisers. This amount of variation explained by the effects of the particular treatments given to the different plants is called the Treatment Sum of Squares:

```
     TSS←(+/,TAB2*2)-+/,TAB3*2
     TSS
134.333
```

Rather more than half of the original variation has been accounted for – but is this enough for us to decide that the differences between the fertilisers is significant? To complete our task, we must now perform a statistical test called an F-test. This involves dividing the Treatment and Error Sums of Squares by magic numbers called Degrees of Freedom, which are very simple to calculate, but much less simple to justify.

If there are R rows (in our case 4) and a grand total of N observations (our N is 24) then the Treatment Sum of Squares will have R-1 degrees of freedom, and the Error Sum of Squares will have N-R. Our values are respectively 3 and 20. Using these values, we work out the Mean Square values by dividing the degrees of freedom into the sums of squares. So the Mean Square for Treatment is

```
     MST←TSS÷3
     MST
44.7778
```

And the Mean Square Error is

```
     MSE←ESS÷20
     MSE
4.78333
```

The F statistic is simply the ratio between these two values:

```
     F←MST÷MSE
     F
9.36121
```

This is the ratio that really matters – not so much the fact that “rather more than half of the original variation has been accounted for”. We now ask whether this is the sort of F-value we might expect if all the variations between the row means were due to chance (not to the effect of different fertilisers). To check this, we can either turn to a book of statistical tables, and look up the relevant critical value (with 3 and 20 degrees of freedom) or we can use APL.

The function FTAIL, listed at the end of this article, calculates the probability that we would observe a value as extreme as F if there were really no differences between the fertilisers. The left argument is a vector containing the two degrees of freedom. If this probability is small (less than 0.05 say) then we can conclude that the fertilisers are really different.

```
     3 20 FTAIL F
0.000452101
```

In this case, we have a probability of about 0.00045. In other words, there is less than one chance in 2000 that these results could happen by chance. It is reasonable to deduce that there are significant differences between at least some of the fertilisers.

This particular version of FTAIL loops until a sufficiently accurate value has been achieved. The algorithm is good, and produces accurate results, but the looping approach is not really in the best traditions of APL. Perhaps some of our readers would like to offer something better. We will publish anything that works!

```
[0]   B←K FTAIL Y;X;P;Q;PQ;PP;QQ;CX;TM;A;S;R;TP;I;G;XX
[1]   X←K[2]÷K[2]+K[1]×Y
[2]   P←K[2]×0.5
[3]   Q←K[1]×0.5
[4]   PQ←P+Q
[5]   →(P<PQ×X)/S1
[6]   XX←X
[7]   PP←P
[8]   QQ←Q
[9]   CX←1-X
[10]  →CT
[11] S1:XX←1-X
[12]  CX←X
[13]  PP←Q
[14]  QQ←P
[15] CT:TM←1
[16]  A←1
[17]  B←1
[18]  S←⌊QQ+CX×PQ
[19]  R←XX÷CX
[20] STO:TP←QQ-A
[21]  →(S≠0)/CT2
[22]  R←XX
[23] CT2:TM←TM×TP×R÷PP+A
[24]  B←B+TM
[25]  TP←|TM
[26]  →((TP≤0.0000001)^(TP≤B×0.0000001))/SEO
[27]  A←A+1
[28]  S←S-1
[29]  →(S≥0)/STO
[30]  TP←PQ
[31]  PQ←PQ+1
[32]  →CT2
[33] SEO:G←LOGGAMMA +/K
[34]  G←G-LOGGAMMA K[1]
[35]  G←G-LOGGAMMA K[2]
[36]  B←B×*(G+(PP×⍟XX)+(QQ-1)×⍟CX)
[37]  B←B÷PP
[38]  →(P<X×P+Q)/CT3
[39]  B←1-B
[40] CT3:B←1-B
```

Note that FTAIL calls the function LOGGAMMA, listed below:

```
[0]   S←LOGGAMMA K;II
[1]   S←0
[2]   I←K
[3]   →(K≤2)/ST
[4]  LB:I←I-2
[5]   S←S+⍟I×0.5
[6]   →(I>2)/LB
[7]  ST:→(0=2|K)/0
[8]   S←S+0.57236494
```

(webpage generated: 5 December 2005, 18:45)
