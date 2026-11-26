---
vid: '10501460'
title: J-ottings 58 – Internal Rate of Return and related topics
subtitle: J-ottings 58
authors:
- Norman Thomson
byline: Norman Thomson
volume: '26'
issue: '4'
page: '93'
received: '2014-11-10'
online: '2015-04-30'
editor: Stephen Taylor
description: A common way of expressing the desirability or otherwise of an investment is through a rate of return. There are several ways in which ‘rate of return’ can be calculated. Comparisons between numbers are only meaningful if the processes producing the numbers are clearly and unambiguously understood. This is where J can be of great help in sorting pathways through a terminological jungle.
abstract: A common way of expressing the desirability or otherwise of an investment is through a rate of return. There are several ways in which ‘rate of return’ can be calculated. Comparisons between numbers are only meaningful if the processes producing the numbers are clearly and unambiguously understood. This is where J can be of great help in sorting pathways through a terminological jungle.
source: ../../php-site/content/printed/264/jot58/jot58.htm
converter: vecrescue 0.1.0
issue_tab: n4-march-2016
tags:
- mathematics and statistics
- applications
---

![](featuredImg_j-ottings-58-internal-rate-of-return-and-related-topics.jpg){ .lead-image }

J-ottings 58
{ .subtitle }

Norman Thomson
{ .byline }

A common way of expressing the desirability or otherwise of an investment is through a rate of return. There are several ways in which ‘rate of return’ can be calculated. Comparisons between numbers are only meaningful if the *processes* producing the numbers are clearly and unambiguously understood. This is where J can be of great help in sorting pathways through a terminological jungle.
{ .abstract }

Internal Rate of Return (IRR) is a commonly used measure. To explain how it is obtained, consider that say 5% growth in any commodity can be calculated by applying a multiplier 1.05, and compound growth by successive applications of this multiplier. In a similar way a list of monetary values can be adjusted for 5% inflation by successive *divisions* by this multiplier. The *opposite* of 5% growth is 5% *decay* (that is growth of -5%), and successive multiplications of 0.95 result in *exponential decay*. However the *inverse* of growth is *not* exponential decay, but rather the process determined by the multiplier (1.05)<sup>-1</sup>=0.9524, meaning that a post-growth value is 95.24% of its original. In the case of decay of, say 20%, the corresponding inverse multiplier is (0.8)<sup>-1</sup>= 1.25 corresponding to 25% . For example under a 20% tax regime, a pre-tax value is obtained from a net value by adding 25%, a process frequently referred to as ‘grossing up’.

The above discussion is consolidated in the following two verbs :

```
   ptom=.%@>:@(0.01&*)  NB. %age growth to inverse multiplier
   mtop=.100&*@<:@%     NB. inverse multiplier to %age growth
   ptom 5 _20
0.952381 1.25
   mtop 0.952381 1.25
4.99999 _20
```

In everyday terms this says that under an assumption of 5% inflation a rational person would be indifferent regarding offers of £95.24 today and a promise of £100 next year. £95.24 is said to be the *net present value* (often abbreviated to NPV) of £100 delayed until next year.

An *income stream* is a list such as

```
        is=._100 20 50 70 80
```

which describes, say, an initial investment of 100 which produces subsequent returns of 20, 50, 70 and 80. The verb `npv` provides compounded net present values for a decay rate (*discount*) given by the left argument :

```
   npv=.]*(ptom@[)^(i.@#@])     NB. net present value
   5 npv is                     NB. 5% decay
_100 19.0476 45.3515 60.4686 65.8162
   10 npv is                    NB. 10% decay
_100 18.1818 41.3223 52.592 54.6411
```

The total of such lists is called the *net benefit*, and the absolute sum of the negative items is the *cost*. The net benefit of is before discounting is thus 120, and the cost 100. After discounting by 5% and 10% the net benefits reduce to

```
   each=.&>
   +/"(1) 5 10 npv each <is
90.6839 66.7372
```

Because exponential decay is not the inverse of growth the NPV discounted streams of `is` are different from the result of repeated compound reduction :

```
   is*0.95^i.5
_100 19 45.125 60.0162 65.1605
   is*0.9^i.5
_100 18 40.5 51.03 52.488
```

Internal Rate of Return answers the question what NPV discount rate would reduce net benefit to zero. In algebraic terms, this amounts to finding a positive real-valued multiplier which satisfies the equation

0 = –100 + 20m + 50m<sup>2</sup> + 70m<sup>3</sup> + 80m<sup>4</sup>

This can be obtained using the root finding verb `p.` which solves polynomials whose argument is coefficients in ascending power order. In the verb `irr` this is followed by a conversion to a percentage:

```
   irr=.mtop@pos@real@roots NB. internal rate of return
      roots=.>@{:@p.        NB. list of roots from result of p.
      real=.#~ (= +)        NB. select real values
      pos=.#~ >&0           NB. select positive values
```

```
   irr is
32.3121
   is%1.323121^i.5
_100 15.1158 28.5608 30.2203 26.1031
   +/_100 15.1158 28.5608 30.2203 26.1031
0                           NB. confirms net benefit is zero
```

In practical terms this means that under an inflation rate of 32.3% the underlying project would finish with no *residual* financial worth.

(Practical note: If `irr` fails through a limit error in `p.` use a standard root finding technique such as the following:

```
   Newton=.adverb :']-x%x D.1'(^:_)("0)  NB. Newton-Raphson
   poly=.|.is                            NB. define a polynomial
   mtop (#.&poly) Newton 0.9
32.3121                                                           )
```

Applying NPV before computing `irr` allows prior assumptions to be made about inflation, e.g.

```
   irr 10 npv is  NB. IRR assuming 10% inflation
20.28
   irr 25 npv is  NB. IRR assuming 25% inflation
5.85
```

For the reason given earlier these values are not identical with the `irr` values which arise on an *a priori* exponential decay assumption:

```
   irr is*0.9^i.5       NB. assuming 10% exponential decay
19.08
   irr is*0.75^i.5      NB. assuming 25% exponential decay
_0.7659
```

If all the inflows are delayed until the final period, the `irr` value is simply the rate of compound interest which for `is` would turn 100 into 220 in the course of four periods:

```
   irr _100 0 0 0 220
21.7883
         100*1.217883^4
220
```

## Average Growth Rate

A reasonable question to ask is why bother with the relative complexity of IRR when a rate of return can be found by simply calculating (inflows–outflows)÷outflows and averaging over the number of periods. To address this, the numerator (inflows–outflows) is just another way of expressing net benefit, while the positive sum of the outflows are given by the verb `costs`. This is illustrated with two input streams which differ only in the order of their items:

```
   costs=./@:(0&>.@-)   NB. outflows (i.e. sum of -ve values)

   is1=._100 20 45 60 _10 55 70 20 10 10
   is2=._100 20 45 60 _10 10 10 20 70 55
   costs each is1;is2
110 110
```

The *benefit-cost* ratios associated with these streams are

```
   (+/ % costs)each is1;is2
1.636 1.636
```

However the stream in which high returns come later has a lower IRR :

```
   ,irr each is1;is2
30.19 25.22
```

showing that averaging loses some of the information implicit in ordering.

Where the term ‘benefit-cost ratio’ is used it is worth checking that the numerator reflects *net* benefits – effectively that is it is inherently a profit ratio. If only inflows appear in the numerator it is a ‘selling price/cost price’ ratio and thus related to mark-up rather than return. The verb `bcr` first incorporates net present value discounting into the averaging process with the discount level set as the left argument:

```
   bcr=.(+/%costs)@:npv         NB. benefit/cost ratio with discounting
   10 bcr each is1;is2
0.80926 0.67133
```

Since the lengths of `is1` and `is2` are both 10 these ratios can be converted to an average compound growth rate by taking the 9th root of `1.80926 1.67133`

```
   9%:1+0.80926 0.67133
1.0681 1.0587
```

that is the average growth rates are roughly 7% and 6% given a discount rate of 10%. This procedure can be generalised as

```
   ktop=.(100&*@<:)             NB. transform (1+k) to 100k%
   root=.<:@#
   agr=.ktop@((root@])%:(>:@bcr))
   10 agr each is1;is2
6.81 5.873
```

If discounting is applied at around the `irr` value then the net average growth rate is more or less zero:

```
   30 agr is1
0.0527249                       NB. growth rate is virtually zero (0.05%)
```

## Using IRR

Where values are money, choices between alternative projects can be guided by IRRs. IRRs can be recalculated dynamically, although they should not be used as a sole criterion since, provided inflows are positive, IRRs carry on increasing as the input stream lengthens, and so *increases* in the IRR tailing off can be a signal suggesting that other statistics should be investigated as well. In the case of `is` successive income streams are given by

```
   }.<\is
+-------+----------+-------------+----------------+
|_100 20|_100 20 50|_100 20 50 70|_100 20 50 70 80|
+-------+----------+-------------+----------------+
```

and successive IRRs by

```
   ,irr each }.<\is
_80 _18.5857 15.6152 32.3121
```

leading to

```
   irrseq=.,@:(irr each)@}.@(<\)
   irrseq is
_80 _18.5857 15.6152 32.3121
```

Post-initial negative items in an income stream as in `is1` might reflect costs incurred in developing a second version of a product. Since polynomials of even order must have an even number of positive roots, using `p.` means that spurious values of IRR will necessarily arise, as for example in

```
   irr _100 20 45 60 _10
_85.04 6.67385
```

where the massive compound 85% decay is clearly inadmissible. To deal with such circumstances it makes sense to insert a filter at the `mtop` level to exclude multipliers of greater than, say 3, or, at the other end of the scale, less than 0.5, hence

```
   filter=.#~ (<&3)*.(>&0.5)
   irr=.mtop@filter@:pos@real@roots
   irrseq is1
0 _22.18 10.22 6.674 19.96 27.96 29.33 29.82 30.19
```

The initial value of `_80` following the first positive flow has been filtered to 0. Following the third inflow of 60 the IRR becomes positive at 10.2%, following which there is a dip to 6.67 due to the negative inflow of 10. Towards the end of the series subsequent increases in IRR tail off as a result of smaller inflows, a signal that benefit is tailing off.

## Annuities and Personal Finance

An *annuity* is a special case of an input stream where the inflows are regular. Annuities in general are a large field within actuarial science, although in the simplest case the calculation has a closed form in which the multiplier \{1-(1+p)<sup>-n</sup>\}/p converts repayments as a percentage of the initial cost into a multiple of present value assuming repayments continue for n years. In May 2014 this factor for a single whole-of-life level annuity at age 65 was around 17. Compare this with an annuity paid for 21 periods at 2.6% whose multiplier is given by

```
   ptok=.>:@*&0.01                      NB. convert %age to (1+p/100)
   am=.4 :'100*(1-(ptok x)^-y)%x'       NB. annuity multiplier
   2.6 am 21                            NB  multiplier for ..
16.026                                  NB. .. 21 repayments at 2.6%
   irr _16.026,21#1                     NB. confirmed by IRR
2.6001
```

Apply a 5% inflation rate and the annuity doesn’t look such a great bargain!

```
   irr _16.026,5 npv 21#1
_1.8065
```

In the case of personal investment, IRR can give a rough guide to the true worth of an investment *on completion*. Consider a share purchase for £33.50 which attracts nine annual dividends of £2, £2, £2, £2.50, £3, £4, £4.45, £5.25 and £4.55, after the last of which it is sold a year later for £65.88.

```
   is3=._33.5 2 2 2 2.5 3 4 4.45 5.25 4.55 65.88
   irr is3
13.46
```

This indicates that over the period the investor has experienced a discount rate of about 13.5% but has no final asset – the inflows account for all benefit. However the average growth rates for 0 and 5% inflation are:

```
   0 5 agr each <is3
11.06 6.508
```

indicating that under 5% inflation `is3` has an average profit-growth based return of 6.5%.

There are streams for which growth can be consistently negative as with

```
   is4=._57.4 1.5 1.5 1.6 1.7 1.8 1.8 1.95 2 27.9
   ;(irr;5&agr)is4
_4.051 _7.289
```

Nevertheless because only the first item is negative, IRR is still an increasing quantity:

```
   ;irrseq is4
_65.84 _52.32 _41.93 _34.25 _28.03 _23.22 _4.051
```

## Future Value Analysis

Future Value Analysis (FVA) is a variation of net present value which, for an income stream with a single initial outflow, expresses benefits valued at *today’s* money :

```
   ratio=.{:@] % {:@npv     NB. find the scale-up factor which ...
   nfv=.ratio*]@npv         NB. ...leaves final npv value unchanged
   5 nfv is
_121.6 23.15 55.13 73.5 80
```

Since ratios of successive terms are unchanged there is no change either to IRRs :

```
   ,irr each (5 nfv is);5 npv is
26.01 26.01
```

For `is` and `is3` the net benefits valued at the end of the investment period assuming 5% inflation are

```
   (+/5 nfv is),(+/5 nfv is3)
110.2 47.94
```

to be compared with values at time of investment of `+/each is;is3` which are 120 and 62.13 respectively. For `is3`, the 47.94 is made up of 36.63 in dividends and 11.31 profit on sale, all at today’s values, representing a gain of 47.94/54.57 or 87.85%. The geometrical average of this is `9 %:1.8785` which equals 7.3%. Three values for growth of this series have been obtained, 13.6% from `irr`, 6.5% from `agr`, and now 7.3% from `nfv`. The need to understand measurement criteria should be clear!

In conclusion, the techniques described here barely begin to scrape the surface both of practical detail and of jargon. Legitimate further questions might concern how the initial outflow was acquired – if by borrowing this leads to further necessary refinement of the `irr` process. The important message is the usefulness of J in penetrating terminology and in understanding precisely the meanings of quantities which may be subtler than their names suggest.
