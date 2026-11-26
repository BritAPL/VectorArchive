---
vid: '10501200'
title: NFL Passer Rating by Brian Becker
authors:
- Brian Becker
byline: Brian Becker
volume: '26'
issue: '1'
page: '75'
received: '2013-09-01'
online: '2013-09-28'
description: Modelling NFL passer statistics in APL.
source: content/printed/261/nflpasser/nflpasser.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- mathematics and statistics
- programming techniques
---

Brian Becker
{ .byline }

I like football, both the American version and the version the rest of the world calls football but the Americans call “soccer”. One of the statistics in American football that’s frequently mentioned is the passer (or quarterback) rating. I've heard sportscasters say the maximum passer rating is 158.3 – a rather odd number if you ask me. This piqued my curiosity and I turned to the font of all knowledge – the internet, where I found an interesting entry in en.wikipedia.org[\[1\]](#ref1) which gave the following formula:

<p class="math" markdown="span"><math xmlns="http://www.w3.org/1998/Math/MathML">
            <mi>a</mi>
            <mo>=</mo>
            <mfenced open="(" close=")">
            <mrow>
                <mfrac>
                    <mi>COMP</mi>
                    <mi>ATT</mi>
                </mfrac>
                <mo>-</mo>
                <mn>0.3</mn>
            </mrow>
           </mfenced>
           <mo>×</mo>
           <mn>5</mn>
    </math></p>

<p class="math" markdown="span"><math xmlns="http://www.w3.org/1998/Math/MathML">
            <mi>b</mi>
            <mo>=</mo>
            <mfenced open="(" close=")">
                <mrow>
                <mfrac>
                    <mi>YARDS</mi>
                    <mi>ATT</mi>
                </mfrac>
                <mo>-</mo>
                <mn>3</mn>
                </mrow>
           </mfenced>
           <mo>×</mo>
           <mn>.25</mn>
    </math></p>

<p class="math" markdown="span"><math xmlns="http://www.w3.org/1998/Math/MathML">
            <mi>c</mi>
            <mo>=</mo>
            <mfenced open="(" close=")">
                <mfrac>
                    <mi>TD</mi>
                    <mi>ATT</mi>
                </mfrac>
           </mfenced>
           <mo>×</mo>
           <mn>20</mn>
    </math></p>

<p class="math" markdown="span"><math xmlns="http://www.w3.org/1998/Math/MathML">
            <mi>d</mi>
            <mo>=</mo>
            <mn>2.375</mn>
            <mo>-</mo>
            <mfenced open="(" close=")">
                <mrow>
                <mfrac>
                    <mi>INT</mi>
                    <mi>ATT</mi>
                </mfrac>
               <mo>×</mo>
               <mn>25</mn>
                </mrow>
           </mfenced>
    </math></p>

Where:
{ .math }

ATT = Number of passing attempts  
COMP = Number of completions  
YARDS = Passing yards  
YARDS = Touchdown passes  
INT = Interceptions
{ .math }

Then, the above calculations are used to complete the passer rating:
{ .math }

<p class="math" markdown="span"><math xmlns="http://www.w3.org/1998/Math/MathML">
        <msub>
        <mi>PasserRating</mi>
        <mi>NFL</mi>
        </msub>
        <mo>=</mo>
        <mfenced open="(" close=")">
            <mfrac>
                <mrow>
                    <mi>mm</mi><mfenced open="(" close=")"> <mi>a</mi> </mfenced> <mo>+</mo>
                    <mi>mm</mi><mfenced open="(" close=")"> <mi>b</mi> </mfenced> <mo>+</mo>
                    <mi>mm</mi><mfenced open="(" close=")"> <mi>c</mi> </mfenced> <mo>+</mo>
                    <mi>mm</mi><mfenced open="(" close=")"> <mi>d</mi> </mfenced>
                </mrow>
                    <mn>6</mn>
                </mfrac>
           </mfenced>
       <mo>×</mo>
       <mn>100</mn>
    </math></p>

Where: <math xmlns="http://www.w3.org/1998/Math/MathML">
        <mi>mm</mi><mfenced open="(" close=")"> <mi>x</mi> </mfenced>
        <mo>=</mo>
        <mo>max</mo>
            <mfenced open="(" close=")">
                <mn>0</mn>
                    <mo>min</mo>
                    <mfenced open="(" close=")">
                        <mi>x</mi>
                        <mn>2.375</mn>
                    </mfenced>
            </mfenced>
    </math>
{ .math }

I thought it might be interesting to model the formula in APL and look at the impact of the various inputs, for instance what happens if a passer completes only 50% of his passes, but every pass he completes is for a touchdown?

It’s fairly straightforward to translate the formula directly into APL:

```
     ∇ rating←att PasserRating(comp yards td int);a;b;c;d;mm
[1]    mm←{0⌈2.375⌊⍵}          ⍝ define the max/min function
[2]    a←((comp÷att)-0.3)×5
[3]    b←((yards÷att)-3)×0.25
[4]    c←(td÷att)×20
[5]    d←2.375-(int÷att)×25
[6]    rating←(((mm a)+(mm b)+(mm c)+(mm d))÷6)×100
     ∇
```

For instance, a quarterback who completes 17 of 20 attempts for 300 yards, 4 touchdowns, and no interceptions would have a “perfect” rating of 158.3.

```
      20 PasserRating 17 300 4 0
158.3333333
```

The above function works just fine, but it’s not particularly “APL-like”. By this I mean that it’s longer than it needs to be and doesn't really make use of APL's array handling capabilities.

One of the first things I noticed was that all of the terms (completions, yards, TDs and interceptions) are treated similarly…

- they’re each divided by the number of attempts
- the result of that operation is adjusted by some number (in some cases the number is 0)
- those results are then multiplied by some number
- then those results are added (or subtracted) from a number (again that number may be 0)
- the mm function is applied to each result

Then to complete the calculation the results are summed, that sum is divided by 6 and then multiplied by 100.

So, if instead of having 4 terms (comp, yards, tds, and int) we have one term “stats” which is a 4 element vector comprised of comp, yards, tds, and int, we can simplify the first operation to:

```
      stats÷att
```

The next step is to adjust each result by some number, -0.3 for completions, -3 for yards, and 0 for each of TDs and interceptions. There are a few ways to do this in APL…

```
      (stats ÷ att) - .3 3 0 0 ⍝ is the most straightforward
      ¯.3 ¯3 0 0 + stats ÷ att ⍝ removes the parentheses
      .3 3 0 0 -⍨ stats ÷ att  ⍝ uses subtraction and the commute operator ⍨
                               ⍝ which commutes (switches) the arguments
```

All of these statements produce equivalent results. So, you can use whichever seems most straightforward to you.

Each of these results is multiplied by a number: 5 for completions, .25 for yards, 20 for TDs, and -25 for interceptions. Why use -25? Because the interception result is subtracted from 2.375 in the subsequent operation.

```
      0 0 0 2.375 + 5 .25 20 ¯25 × .3 3 0 0 -⍨ stats ÷ att
```

Next the mm (max/min) function gets applied; it’s so trivial we can skip writing a separate function…

```
      0 ⌈ 2.375 ⌊ 0 0 0 2.375 + 5 .25 20 ¯25 × .3 3 0 0 -⍨ stats ÷ att
```

Then those results are summed…

```
      +/ 0 ⌈ 2.375 ⌊ 0 0 0 2.375 + 5 .25 20 ¯25 × .3 3 0 0 -⍨ stats ÷ att
```

The sum is the divided by 6 and multiplied by 100, again there are several ways to accomplish this…

```
      100 × (+/ 0 ⌈ 2.375 ⌊ 0 0 0 2.375 + 5 .25 20 ¯25 × .3 3 0 0 -⍨ stats ÷ att) ÷ 6
      (100 ÷ 6) × +/ 0 ⌈ 2.375 ⌊ 0 0 0 2.375 + 5 .25 20 ¯25 × .3 3 0 0 -⍨ stats ÷ att
      100 × 6 ÷⍨ +/ 0 ⌈ 2.375 ⌊ 0 0 0 2.375 + 5 .25 20 ¯25 × .3 3 0 0 -⍨ stats ÷ att
```

Putting it all together, we can rewrite our function as:

```
     ∇ rating←att PasserRating stats
 [1]    rating←100×6÷⍨+/0⌈2.375⌊0 0 0 2.375+5 .25 20 ¯25×.3 3 0 0-⍨stats÷att
     ∇
```

This is known as a trad-fn (traditional function) in Dyalog APL. Dyalog also has d-fns (dynamic functions). PasserRating written as a d-fn would look like:

```
      PasserRating←{100×6÷⍨+/0⌈2.375⌊0 0 0 2.375+5 0.25 20 ¯25×.3 3 0 0-⍨⍵÷⍺}
```

Once you have the PasserRating function, you can see the effect of the different inputs. For instance, what if the passer had the same results (17 completions, 300 yards, 4 TDs, and 0 interceptions) but the number of attempts varied?

```
      {'Attempts' 'Rating'⍪⍵,[1.1](PasserRating∘17 300 4 0)¨⍵}16+⍳16
 Attempts       Rating
       17  158.3333333
       18  158.3333333
       19  158.3333333
       20  158.3333333
       21  158.3333333
       22  158.1439394
       23  155.3442029
       24  152.7777778
       25  148.3333333
       26  144.2307692
       27  140.4320988
       28  136.9047619
       29  133.6206897
       30  130.5555556
       31  127.688172
       32  125
```

Does the passer rating formula make sense? For instance, consider two quarterbacks who each have 20 attempts and pass for 400 yards. The first quarterback has only 10 completions, but each of those is for a touchdown. The second quarterback has 20 completions, but only 1 touchdown. Who’s the higher rated quarterback? According to the formula, they both have the same rating.

```
      20 PasserRating ¨(10 400 10 0)(20 400 1 0)
135.4166667 135.4166667
```

Perhaps “passer“ rating is less interesting than a “quarterback” rating which, in addition, could take into account things like:

- sacks – a quarterback who less aware of his surroundings and is sacked more frequently isn’t as effective
- fumbles – similarly a quarterback who fumbles more often isn’t as effective
- yards gained – most of today’s quarterbacks have to be somewhat mobile

Can passer rating be improved upon? There are lots of possibilities and APL makes it easy, even fun, to explore them.

### References

1. <a id="ref1"></a>Passer rating  [http://en.wikipedia.org/wiki/Passer\_rating](http://en.wikipedia.org/wiki/Passer_rating)
