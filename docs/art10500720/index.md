---
vid: '10500720'
title: 'Eight-way street: handling high-rank arrays'
subtitle: Handling high-ranking arrays
authors:
- Stephen Taylor
byline: Stephen Taylor (sjt@5jt.com)
volume: '25'
issue: '3'
page: '43'
received: '2011-12-27'
online: '2012-01-05'
editor: Stephen Taylor
description: Programming techniques for handling high-rank arrays
abstract: Describes techniques for handling ‘noble’ arrays – of rank much higher than three; also introduces a mnemonic for the left argument of dyadic transpose.
source: content/printed/253/noblearrays.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- programming techniques
---

Handling high-ranking arrays
{ .subtitle }

Stephen Taylor (sjt@5jt.com)
{ .byline }

Describes techniques for handling ‘noble’ arrays – of rank much higher than three; also introduces a mnemonic for the left argument of dyadic transpose.
{ .abstract }

When you sup with the devil, use a long spoon. (Proverb)

One of the features of APL attractive to the learner is how functions work on conformable arrays without loops and indexes. When `A×B` can refer to arrays of arbitrary rank it is clear one has stepped into a world of new abstraction and generality.

Later it can be surprising to find how little use one has made of higher-rank arrays. Interpreter writers track their usage to learn where optimisation would be valuable. Only a tiny fraction of arrays have ranks higher than 3. (Let us call such arrays ‘noble’.)

Somehow this seems disappointing. When I started to write APL, I foresaw working in higher realms of abstraction and generality, free from the mental clutter of loops and indexes. The loops are mostly gone, but noble arrays remain tantalisingly rare. I conducted an informal survey of career APL programmers I know. The highest-rank array reported used in commercial software was 5.

The dreams of youth are notoriously prone to disappointment. But is useful to understand the sources of disappointment.

In contrast to youthful dreams, much of life is quotidian, spent brushing one’s teeth and washing dishes. So with programming. Even with much looping swept away there is much to be done that resists abstraction. Life and programming both are less tractable than our dreams.

Another source I can own to is a combination of my own laziness and others’ pressure for results. It takes time and effort to master techniques of higher abstraction and generality. Suppose one is faced, say, with multiplying a table through a rank-4 array. One has perhaps a hazy idea of a short expression that would do it. But writing two loops solves it faster than working out the unfamiliar expression. Repetition of the problem might prompt one to stop and study. Peer pressure from more experienced colleagues might also do it. Otherwise one is pressed always toward the road more travelled.

Visual Basic is famously easy to start writing in. It is also notoriously quick to mire a writer’s ambition in repetitive code. We might call this the ‘VB effect’. But APL writers are also vulnerable to it, and for similar reasons. Both languages favour the self-taught, enabling domain experts, untrained in the orthodoxies of software development, to produce usable code. Without support from writers more experienced in abstractions, autodidacts are liable to miss available coding abstractions.

Sometimes this has profound consequences. I was recently asked to rewrite a core section of an application developed and maintained in just this way over twenty years. The chief developer, a domain expert, was soon to retire. Faced with changes required by regulatory changes he advised that he could not patch in further changes with any confidence. The algorithms should be rewritten as they would have been had the eventual requirements been known twenty years earlier. After years of ‘cut and paste’ patching, the code base had, as some developers put it, “gone sour”. It could no longer be amended with confidence.

Going sour is terminal. Code goes sour when its developers can no longer navigate its exceptions, tricks and redundant parts. Sour code has to be rewritten.

“Software is a constant battle against complexity.”[\[1\]](#ref1) Developers aim to defer as long as possible the souring of code. Abstraction is a weapon in this battle. When short, abstract code has to be revised, it encourages ‘clean’ reformulation that preserves the rigour of the original. It is less susceptible to the accumulation of ‘cut-&-paste’ patching that sours code.

Reading the soured application code I was able to see the traces of cut–&-paste work. Pages of code that looked something like this:

```
NRRUBBLE ← RATER × NRUBBLE
NSRUBBLE ← RATES × NRUBBLE
NTRUBBLE ← RATET × NRUBBLE
NURUBBLE ← RATEU × NRUBBLE
MRRUBBLE ← RATER × MRUBBLE
MSRUBBLE ← RATES × MRUBBLE
MTRUBBLE ← RATET × MRUBBLE
MURUBBLE ← RATEU × NRUBBLE
```

Tracing execution revealed the `RUBBLE` arrays to be vectors. One could see within the verbose solution a terse, higher-rank solution struggling to emerge.

## Factor arrays

Analysis revealed the bulk of this code allocated a sum of money. The vectors were time series, created by applying interest rates, inflation rates and other time-related factors. The money was split other ways as well. In some cases the money was allocated to one of a choice of categories. In others, it was apportioned between categories.

All of these allocations could be represented by multiplication. To apply an interest rate R across valuation years Y:

```
      fTime ← (1+R)*⍳⍴Y
```

To allocate to one of a list of categories `CATS` according to category `C`:

```
      fCat ← CATS=C
```

To apportion across periods according to dates `D1 D2 D3`:

```
      fPeriod ← apportionPeriod(D1 D2 D3)
```

Since these ‘business rules’ are defined orthogonally, we can use Cartesian (outer) products to construct a rank-3 array that allocates, apportions and revalues:

```
      VAL ← AMT × fTime ∘.× fCat ∘.× fPeriod
```

Or, to remove the repetition:

```
      VAL ← AMT × ⊃ ∘.×/fTime fCat fPeriod
```

The result `VAL` has 3 axes: valuation years, category and period.

Sadly, the problem does not admit of so satisfyingly simple a solution. The outer-product reduction works only because the factors are mutually independent. But some business rules work across multiple axes.

For example, a set of rates apply across time, but differ between categories. The rate table `RT` has `⍴Y` rows, and columns corresponding to the categories of `CATS`. That is to say, `RT` has the time and category axes. We can conveniently insert it into our combination of factors:

```
      VAL ← AMT × (RT × fTime ∘.× fCat) ∘.× fPeriod
```

Where the old code had a plethora of similar names, the new has everything stacked in a rank-3 array, from which we can select what we need by indexing. There turns out to be a good deal of this ahead. It is inconvenient for the reader to remember that, for example, the first index on the period axis denotes pre-1988 and the second post-1988. We shall do better with enumerators:

```
      (iM iN iO iP) ← ,¨⍳4 ⍝ enumerate category axis
      (iPr8 iPo8)   ← ,¨⍳2 ⍝ enumerate period axis
```

Thus we can select the pre-1988 value in 2013 for category N as `VAL[Y⍳2013;iN;iPr8]` or tabulate the post-1988 values for categories O and P as `VAL[;iO,iP;iPo8]`. Note that the index enumerators are vectors, so indexing in this way preserves rank: the result too has rank 3. This is not very valuable for a rank-3 array, but will turn out to be helpful as we add axes to `VAL`.

`VAL` still has three axes. We suspect it will need more before we’re done.

It turns out we need `VAL` calculated both with and without the rates in `RT`. We can accommodate this with another axis. First we extend the rate table to apply and not apply:

```
      fRates ← RT ∘.* 1 0
```

`fRates` has 3 axes: valuation years, categories and a new axis: with and without rates. We want some more enumerators:

```
      (iWth iWto)   ← ,¨⍳2 ⍝ enumerate: with & without rates
```

Folding `fRates` into the expression for `VAL` requires a little care now. We could jam the extra axis onto the `fTime fCat` outer product:

```
      VAL ← AMT × (fRates × (fTime ∘.× fCat) ∘.× 1 1) ∘.× fPeriod
```

Or we could enclose rank-3 `RT` into a table of 2-element vectors. This can then be multiplied by the `fTime fCat` outer product: scalar extension multiplies each element of `fTime∘.×fCat` by a 2-element vector:

```
      VAL ← AMT × (⊃ (fTime ∘.× fCat) × ⊂[3]fRates) ∘.× fPeriod
```

We can think of the enclose on the third axis as ‘hiding’ it, leaving only the time and category axes exposed. Disclosing the result of the multiplication ‘restores’ enclosed axes at the end of the shape – in this case exactly where the enclosed axis had been before.

`VAL` now has four axes: valuation year, category, with/without rates, and period. Still we think there will be more axes before we’re done.

## Switching axes

APL developers often write to explore the problem domain. In working with a noble array one might find oneself adding and removing axes as one’s view of the domain improves. Having index enumerators and axis enumerators (see below) are a great help in revising the code as the shape of `VAL` changes.

There is a particular problem with the approach used above to generate the factor arrays. While exploring the domain, their shapes change. (In the example above, we saw the rate table shifted from two axes to three.) Some of the axes of the factor arrays match some others, some will not.

Combine factor arrays into a single array of factors by successively enclosing along selected axes, multiplying and disclosing – as above. Each time a factor array changes shape, this sequence has to be revised. (Sadly, axis enumerators are no help here, as they refer only to the axes of `VAL`.)

Worse, as the combination of factor arrays gets revised, so the shape of the result alters. Every axis will be there, but not necessarily in the order you want. Each revision of the combination is liable to change the sequence of axes. We know dyadic transpose will re-order the axes of an array. Now is the time to get a grip on its left argument.

Suppose that `VAL` is to be a rank-8 axis. Let’s define eight enumerators for its axes.

```
      (aTIM aCAT aPRD aSEX aRAT aBNS aYRS aPLS)←⍳⍴⍴VAL ⍝ enumerate axes
```

We have combined the various factor arrays we defined from the business rules. The resulting factor array has all eight axes, but not in the order above. How do we get them into the right order? Start by identifying the axes in the combined factor array. Suppose they have emerged from the combination in this order:

```
      aYRS aRAT aTIM aPRD aSEX aBNS aCAT aPLS
```

Very good. Then we use this array as the left argument of our dyadic transpose:

```
      aYRS aRAT aTIM aPRD aSEX aBNS aCAT aPLS⍉
```

puts the axes in the order they are to be in `VAL`.

From using axis enumerators in this way we see that the left argument of dyadic transpose functions as an address list – where the corresponding axes of the right argument are to end up.

## Summing and selecting

In reading values from `VAL` the business logic sometimes needs categories and periods distinct, sometimes summed.

Tabulating with-rate values for all years and categories, summed across period:

```
      +/VAL[;;↑iWth;]
```

The `↑` is required to collapse the third (with/without rates) axis, as the enumerators are all length-1 vectors. (We shall see shortly why this is so.) The sum defaults to the last axis and removes it. The result is a table of valuation years by category.

Tabulating with-rate values for all years and periods:

```
      +/[2]VAL[;;↑iWth;]
```

Here the axis operator applies the summing to the category axis. Reading the expression requires us to remember that the second axis is for categories. Again, we will do better with enumerators:

```
      (aTIM aCAT aRAT aPRD)←⍳4 ⍝ enumerate axes
```

This enables us to write the previous tabulations as

```
      +/[aPRD] VAL[;;iWth;]
      +/[aCAT] VAL[;;iWth;]
```

with some gain in legibility. Now we see the value of using vector index enumerators so that indexing does not reduce rank. It allows us to use the axis enumerators for reduction. (A drawback above is that the rates axis remains, with length 1.)

Nor is this is an approach that scales well to nobler arrays. Suppose our array has grown to 8 axes. We wish to index some of them and sum others. The reader’s primary interest is to see what has been selected and what axes are in the result.

We can use successive summations providing we sum axes in descending order. For example, for axes

```
      (aTIM aCAT aPRD aSEX aRAT aBNS aYRS aPLS)←⍳⍴⍴VAL ⍝ enumerate axes
```

we could get a table with rows and columns corresponding respectively to `aSEX` and `aCAT` something like:

```
      sel ← VAL[iThis;;;;;iThat;;iTher]
      ⍉+/[aTIM]+/[aPRD]+/[aRAT]+/[aBNS]+/[aYRS]+/[aPLS] sel
```

The indexing makes the selection clear enough, but it still takes some work to see that the axes not summed are `aCAT` and `aSEX` and thus the result is a table with rows and columns corresponding to those axes, then transposed. For maintenance one would worry that changes in the order of axes will change the result of the successive summations without necessarily breaking execution.

Better if we could write something like:

```
      aSEX aCAT SUMSOF VAL[iThis;;;;;iThat;;iTher]
```

This it turns out we can do quite neatly:

```
    ∇ Z←resultaxes SUMSOF array
[1]  ⍝ array summed across all axes except resultaxes
[2]  ⍝ result has axes in order of resultaxes
[3]  Z←(⍋,resultaxes)⍉+/¨,¨⊂[(⍳⍴⍴array)~resultaxes] array
    ∇
```

The expression in line 3 encloses all the axes that are to be ‘hidden’, producing an array whose shape contains the axes in `resultaxes`, but in ascending order. Each cell of this is then ravelled and summed. The dyadic transpose then shuffles the axes into the desired order.

While any change to the shape of `VAL` would still require a change to the indexing, provided the `aSEX` and `aCAT` axes persist, they may remain as the left argument of `SUMSOF`.

## Conclusion

Using index enumerators considerably improves readability when selecting from noble arrays. Using axis enumerators makes it easier to put axes of intermediate arrays into a desired order. The left argument of dyadic transpose may be thought of as an ‘address list’ of where you want the corresponding axes of the right argument to be ‘sent’.

The dyadic function `SUMSOF` improves the readability and robustness of what would otherwise be a sequence of sum functions vulnerable to shape changes in `VAL`.

## References

1. <a id="ref1"></a> Eric Evans, *Domain-Driven Design*, Addison Wesley, 2003
