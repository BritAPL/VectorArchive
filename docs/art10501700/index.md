---
vid: '10501700'
title: Taming statistics with limited-domain operators
authors:
- Stephen Mansour
byline: Stephen M. Mansour, Ph.D. University of Scranton(stephen.mansour@scranton.edu)
volume: '27'
issue: '1'
received: '2016-04-25'
online: '2016-09-01'
editor: John Jacob
description: A systematic method to organize a core set of statistical operations utilising an understandable syntax.
abstract: Most students of statistics are overwhelmed by the sheer number of functions and procedures necessary to model and analyze data. To make matters worse, there are many inconsistencies in statistical tables and notation. Many statistical software packages have a bewildering array of functions. Excel has at least 87 statistical functions alone; many of these functions are very similar with minor variations.
source: ../../php-site/content/printed/271/tamingstatistics/tamingstatistics.htm
converter: vecrescue 0.1.0
issue_tab: n1-20162022
tags:
- mathematics and statistics
- language design
- education
---

![](featuredImg_taming-statistics-with-limited-domain-operators.jpg){ .lead-image }

Stephen M. Mansour, Ph.D.  
University of Scranton  
(stephen.mansour@scranton.edu)
{ .byline }

Most students of statistics are overwhelmed by the sheer number of functions and procedures necessary to model and analyze data. To make matters worse, there are many inconsistencies in statistical tables and notation. Many statistical software packages have a bewildering array of functions. Excel has at least 87 statistical functions alone; many of these functions are very similar with minor variations.
{ .abstract }

Ken Iverson reduced the number of primitive mathematical functions in APL by introducing operators into the language. He also made function syntax more consistent. He referred to this as "taming mathematics".

We plan to do to statistics what Iverson did to mathematics. In other words our goal is "taming statistics". We describe herein a systematic method to organize a core set of statistical operations consistently while using easily understandable syntax. This includes defining a special type of operator which requires a limited set of operands. We will refer to these operators as limited-domain operators.

## Statistical objects

Most of statistics can be described using arrays, functions and operators. We represent samples and finite populations with numeric vectors for quantitative data. For qualitative data, we may use a vector of character vectors, a matrix or comma delimited vector. Statistical functions fall into four basic categories: summary statistics, probability distributions, relations and logical functions. Operators are used to calculate probabilities, generate random variables, determine confidence intervals and perform hypothesis tests.

### Summary functions

In statistics, a *parameter* is measure of a population and a *statistic* is a similar measure computed from a sample. We define a *summary function* in APL as a monadic function which takes a vector as a right argument and produces a scalar result; it describes data with a single value. Structurally, a summary function is similar to +/ in APL. A parameter is the result of a summary function applied to a population vector; a statistic is the result applied to a sample vector. Summary functions include measures of center, spread, position and shape. Measures of position include the mean, median and mode.

```
     X←2 7 5 1 5 3
     mean X                 ⍝ A summary function
3.8333
     (mean,median,mode) X   ⍝ Measures of center
3.8333 4 5
     (range,iqr,var,sdev) X ⍝ Measures of spread
6 3 4.9667 2.2286
     20 percentile X        ⍝ 20th percentile is 2
2
     5 percentileRank X     ⍝ 5 is the 67th percentile
67
     (skewness,kurtosis) X  ⍝ Measures of shape
0.14756 ¯1.1283
     proportion 1 1 1 0 0 1 ⍝ Boolean proportion
0.66667
```

### Probability distributions

Mathematically, a probability distribution is a function whose result is non-negative for any input. Most distributions are dyadic functions; the right argument is a value or an array of values from that distribution while the left argument is a list of parameters. A distribution function cannot generate a domain error; each invalid input simply produces a value of 0. Some distributions may be described monadically; in this case default values are assigned for commonly-used parameters. There are two types of probability distributions, discrete and continuous.

Discrete distributions are defined by the probability mass function. The explicit result of a discrete distribution is a probability and must be between 0 and 1. Some examples of discrete distributions are `uniform, binomial` and `poisson`.

```
     6 uniform 3       ⍝ Probability of rolling a die and getting a 3 = 1/6
0.16667
     3 0.25 binomial 2 ⍝ Binomial Pr(X=2|n=3,p=0.25)
0.0140625
     5.2 poisson 3     ⍝ Poisson Pr(X=3|lambda=5.2)
0.12928
```

Continuous distributions are defined by the density function. The density function may produce a value greater than 1, but the area under the curve must equal 1. Continuous distributions include `normal, exponential` and `rectangular`, as well as `chiSquare, tDist` and `fDist` used in sampling and inference. If the left argument is omitted, the normal density assumes a mean of 0 and a standard deviation of 1.

```
     normal ¯2 ¯1 0 1 2          ⍝ Standard Normal densities
0.053991 0.24197 0.39894 0.24197 0.053991
     8 3 normal 4 6 8 10 12      ⍝ mean=8, std deviation=3
0.16401 0.31945 0.39894 0.31945 0.16401
```

### Relational and logical functions

Most of these functions are primitives in APL and retain their usual meanings. Relational functions consist of the six dyadic scalars functions: &lt;, ≤, =, >, ≥ and ≠. We also include the non-scalar relational function ∊ and the d-fns between and outside defined below:

```
     between←{¯1=×/×⍺∘.-⍵}
     1 2 3 4 5 6 between 2 5
0 0 1 1 0 0
     outside←{1=×/×⍺∘.-⍵}
     1 2 3 4 5 outside 2 4
1 0 0 0 1 1
```

Logical functions include  `and(^), or(∨), not(~)` and `if`. These are used in conjunction with the rules of probability.

```
      SEX←'MMFMFFMMMMMMFFFMFFMM'
      CLASS←1 2 2 3 3 4 2 3 4 2 2 2 3 4 4 1 1 2 3 3
      proportion SEX='M'
0.6
      proportion CLASS = 2
0.35
      proportion not CLASS = 2
0.65
      proportion (SEX = 'M')and (CLASS = 2)
0.25
      proportion (SEX = 'M') or (CLASS = 2)
0.7
      if←{⍵/⍺}
      proportion (SEX = 'M')if (CLASS = 2)
0.71429
```

## The probability operator

Although we can calculate probabilities for discrete distributions using the probability mass functions described above, it is much more difficult to calculate probabilities for continuous functions because we need to specify an interval rather than a specific value. For example, most normal tables are set up to show either cumulative probabilities or 0-to-z probabilities. For other distributions, such as the Student-t, Chi-Square and F Distribution, the tables are set up to show upper-tail probabilities. This lack of consistency makes it difficult for students to understand what is going on. We define an operator `prob` which takes a distribution as its left operand and a relational function as its right operand and calculates a probability.

```
     normal prob ≤ 2         ⍝ Cumulative probability
0.84134
     normal prob between 0 2 ⍝ 0-to-Z probability
0.34134
     5 chiSquare prob > 7.3  ⍝ Upper-tail prob for chi-Square with df=5
0.19927
```

The operator also works with discrete distributions. What is the probability that a baseball player bats safely in a 9-inning game if his batting average is .206? The average player usually gets 4 at bats during a game and to bat safely he must get at least one hit. This will happen about 60% of the time:

```
     4 0.206 binomial prob ≥ 1
0.60255
```

Suppose we flip a fair coin 5 times and count the number of heads:

```
     5 0.5 binomial prob = 2   ⍝ probability of exactly 2 heads
.3125
     5 0.5 binomial prob ≥ 2   ⍝ probability of at least 2 heads
0.8125
     5 0.5 binomial prob < 2   ⍝ probability of less than 2 heads
0.1875
     5 0.5 binomial prob ∊ 2 4 ⍝ probability of 2 or 4 heads
0.46875
```

## The critical value operator

Sometimes the probability is known and we want to find the corresponding value. In this case we use the `criticalValue` operator whose syntax is similar to the probability operator. The following example illustrates this. Assuming student heights are distributed normally with a mean of 68 inches with a standard deviation of 3 inches, what height represents the 95th percentile?

```
      68 3 normal criticalValue > .95
72.935
```

In most statistics textbooks, while the normal table is set up to obtain the probability associated with a critical value, the Student t, chiSquare and F distributions are set up to obtain the critical value from the upper-tail probability. The criticalValue operator also handles the latter case.

```
    ⍝ One-tail Student-t critical value for 17 degrees of freedom
    17 tDist criticalValue < .05
1.7396
    17 tDist criticalValue ≠ 0.01 ⍝ Two-tail critical value for ⍺ = 0.01
2.5758
    ⍝ ChiSquare critical values for 5 degrees of freedom
    5 chiSquare criticalValue < .10 .05 .025 .01 .005
9.2364 11.07 12.833 15.086 16.75
    ⍝ Critical value of F-distribution with 2 d.f. in numerator ...
    2 4 fDist criticalValue < .05 ⍝ ... and 4 d.f. in denominator
6.9443
    ⍝ Generate an F-table using outer product:
    ∘.{⍵ ⍺ fDist criticalValue < 0.05}⍨1+⍳5
 161.45  199.5   215.71  224.58  230.16
 18.513  19      19.164  19.247  19.296
 10.128  9.5521  9.2766  9.1172  9.0135
 7.7086  6.9443  6.5914  6.3882  6.2561
```

## The random variable operator

The `randomVariable` operator generates independent random variables based on its operand. When the operand is the uniform distribution it is similar to the roll (?) function in APL. The rectangular distribution is based on ?0. The right argument is an integer indicating the sample size.

```
     normal randomVariable 5     ⍝ Generate 5 standard normal r.v.’s
0.58949 0.35082 0.1357 0.88211 ¯1.7619
     3 poisson randomVariable 10 ⍝ Generate 10 poisson r.v.’s (mean=3)
5 4 5 2 3 4 1 2 2 0
```

## Confidence intervals and sample sizes

We introduce two monadic operators `confInt` and `sampleSize`. These operators take a summary function as the left operand. The right argument is a vector of values. The optional left argument is a confidence level which defaults to 0.95. When the operator `confInt` is applied, the result will the appropriate confidence interval for the unknown parameter which the statistic represents. The operator `sampleSize` will determine the minimum sample size needed for a particular margin of error:

```
      HT                       ⍝ Heights of ten students
68 65 72 72 75 64 68 68 63 69
      mean HT                  ⍝ Sample mean
68.4
      mean confInt HT          ⍝ 95% confidence interval (default)
65.677 71.123
      SD←⎕←sdev HT             ⍝ Sample standard deviation
3.8064
      E←1                      ⍝ Desired margin of error
      mean sampleSize E SD     ⍝ Sample size needed
59
      .99 mean confInt HT      ⍝ 99% Confidence Interval is wider
64.488 72.312
      .99 mean sampleSize E SD ⍝ Sample size for 99% Conf Interval
101
      sdev confInt HT          ⍝ Conf. interval for standard deviation
 2.6182  6.9491
```

From a sample of 100 voters, there were 47 Democrats (DEM), 11 Independents (IND) and 42 Republicans (REP). We will construct a 95% confidence interval for the proportion of Republicans:

```
      P←⎕←proportion PARTY eq 'REP'     ⍝ 42% of sample are Republicans
0.42
      proportion confInt PARTY eq 'REP' ⍝ 95% confidence interval
0.32326 0.51674
      ⍝ What sample size do we need for ...
      E←0.03                            ⍝ ... Margin of Error = +/- 3% ?
      proportion sampleSize E           ⍝ Worst case p = 50%
1068
      proportion sampleSize E P         ⍝ Assuming previous estimate
1040
```

## Hypothesis tests

Hypothesis tests typically involve comparing a sample statistic to a population parameter. A dyadic operator `hypothesis` takes a summary function to calculate the statistic as its left operand and a relational function as its right operand to do the comparison. Let us assume that the average height of students is 66 inches. The null hypothesis represents our assumption: H₀: µ=66. The alternative hypothesis is the complement: H₁: µ≠66.

We can run the hypothesis operator assigning mean to the left operand and = to the right operand:

```
     HYP←HT mean hypothesis = 150
```

The result is a namespace `HYP`, which contains various results. Two of the most important are the test statistic and the p-value:

```
      HYP.TestStatistic
1.9939
      HYP.pValue
0.077315
```

However, it is much easier to simply generate a report from the namespace. The `report` function assumes a 0.05 level of significance and generates a two-by-two comparison table which illustrates the critical-value approach and the p-value approach simulataneously. The rows of the table show the functional relationships, while the columns show the comparison values.

```
     report HYP
──────────────────────────────────────────
 _
 X =68.40000
 s =3.80643
 n =10
 Standard Error: 1.20370

 Hypothesis Test

  H₀: µ=66              H₁: µ≠66
 ┌──────────────────┬───────────────────┐
 │Test Statistic:   │P-Value:           │
 │t=1.9939          │p=0.077315         │
 ├──────────────────┼───────────────────┤
 │Critical Value:   │Significance Level:│
 │t(α/2;df=9)=2.2622│α=0.05             │
 └──────────────────┴───────────────────┘
  Conclusion: Fail to reject H₀
──────────────────────────────────────────
```

Suppose we wish to generate an upper tail hypothesis test, and furthermore we prefer to use a 0.01 level of significance. We simply change the right operand of `hypothesis` to `>`, and include a left argument to the `report` function:

```
     .01 report HT mean hypothesis > 65
────────────────────────────────────────
 _
 X =68.40000
 s =3.80643
 n =10
 Standard Error: 1.20370

 Hypothesis Test

  H₀: µ≤65             H₁: µ>65
 ┌────────────────┬───────────────────┐
 │Test Statistic: │P-Value:           │
 │t=2.8246        │p=0.009948         │
 ├────────────────┼───────────────────┤
 │Critical Value: │Significance Level:│
 │t(α;df=9)=2.8214│α=0.01             │
 └────────────────┴───────────────────┘
  Conclusion: Reject H₀
────────────────────────────────────────
```

We can test the hypothesis that at least half of the voters are Republican using a hypothesis involving proportions:

```
     report (PARTY eq 'REP') proportion  hypothesis < .5
────────────────────────────────────────
 ∧
 P =0.42000
 n =100
 Standard Error: 0.05000

 Hypothesis Test

  H₀: p≥0.5            H₁: p<0.5
 ┌────────────────┬───────────────────┐
 │Test Statistic: │P-Value:           │
 │Z=1.6           │p=0.054799         │
 ├────────────────┼───────────────────┤
 │Critical Value: │Significance Level:│
 │Z(α)=1.6449     │α=0.05             │
 └────────────────┴───────────────────┘
  Conclusion: Fail to reject H₀
────────────────────────────────────────
```

To compare two populations, we introduce a sample of heights from another group:

```
     HT2←61 59 63 67 68 60 70 73 66
     .10 report HT mean hypothesis > HT2
────────────────────────────────────────────────
 _                   _
 X₁=68.40000         X₂=65.22222
 s₁=3.80643          s₂=4.79004
 n₁=10               n₂=9
 Standard Error: 1.99957

 Hypothesis Test

  H₀: µ₁≤µ₂            H₁: µ₁>µ₂
 ┌─────────────────┬───────────────────┐
 │Test Statistic:  │P-Value:           │
 │t=1.5892         │p=0.06643          │
 ├─────────────────┼───────────────────┤
 │Critical Value:  │Significance Level:│
 │t(α;df=15)=1.3406│α=0.1              │
 └─────────────────┴───────────────────┘
  Conclusion: Reject H₀
────────────────────────────────────────────────
```

### Conclusion

Many statistical packages have large libraries of functions to handle the endless variety of probability calculations, parameter estimates and statistical tests. Operators are a natural way express many of these statistical concepts and allow us to reduce the number of functions—for example we need only one function for each statistical distribution. The implementation of these functions can be accomplished directly in APL or by a call to R.
