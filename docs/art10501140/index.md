---
vid: '10501140'
title: Boolean Reductions
authors:
- Phil Last
byline: by Phil Last (phil.last@ntlworld.com)
volume: '26'
issue: '1'
page: '62'
received: '2013-09-10'
online: '2013-09-22'
editor: John Jacob
description: A look at the boolean vector reductions - choosing the right one for the job in hand; and some potential speed-ups.
keywords:
- boolean
- reduction
- semantics
abstract: A look at the boolean vector reductions - choosing the right one for the job in hand; and some potential speed-ups.
source: content/printed/261/booleanreductions/booleanreductions.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- programming techniques
- performance
---

by Phil Last (phil.last@ntlworld.com)
{ .byline }

A look at the boolean vector reductions - choosing the right one for the job in hand; and some potential speed-ups.
{ .abstract }

## Some boolean scans

Most experienced APLers could list a number of boolean scans, some more than others, that they can include in algorithms, confident that they will fulfill a particular task. Some of these scans would not necessarily seem intuitive to a newcomer, even one conversant with the primitive itself.

*Less-than* and *not-equals* are possibly the most ubiquitous of these.

The less-than scan of a boolean vector turns off all ones in the vector except the first.

The not-equals scan of a boolean vector, reading from the left, flips the corresponding and subsequent items on or off wherever the argument vector is on.

Once we have learned what these do we tend not to question how they work. Thinking of them in similar terms to the apparent left-to-right processing of *and* and *or* scans, the not-equal scan is fairly clear. Not so less-than. This is because not-equals is, and less-than is not, associative over the boolean domain. We can only understand non-associative scans in terms of the individual reductions that constitute their results. The less-than reduction of a boolean vector is true only if the final element is the one-and-only one. Paradoxical except for the fact that the definition of scan demands it, the less-than scan gives the *first* one precisely because the less-than reduction requires only the *last*.

## Reductions

I don't know how many scans are either particularly useful or easily explicable. But I'd guess that not many of my readers can give natural language descriptions of which boolean vectors will return true for more than a very few of the boolean *reductions*.

It may seem that there should be no good answer to this question. For each function infinitely many boolean vectors resolve to one and infinitely many resolve to zero. Is it reasonable to assume a recognisable pattern to distinguish the two sets? Think of and-reduction as *all* and or-reduction as *any* and you can deduce two trivial examples.

I asked myself this question nearly two decades ago and after much experiment found answers to fourteen of the sixteen candidates.

If the mention of sixteen in the previous statement doesn't surprise you then please skip to the next section - [Preliminary results](#Prelim).

## Sixteen boolean functions?

APL traditionally implements either four, six or ten primitive *boolean* or *logical* functions depending on your point-of-view. The four undisputed are *and*, *or*, *nand* and *nor*. Even two of these now have additional, non-boolean definitions. We can add *equals* and *not-equals* by restricting their domains and *less-than*, *less-than-or-equals*, *greater-than-or-equals* and *greater-than* if we are willing to accept that `p>q` can be read as *p and-not q* instead of, and having a different meaning from, *p greater-than q*. But this still only gives us ten.

A boolean or logical dyad takes two booleans and returns one. A single function must return a single one or zero for each boolean pair, of which there are four: `(0 0)(0 1)(1 0)(1 1)`. Representing each of these these resultant four item booleans as a four-digit binary and there being sixteen of those: `0000 0001 0010 ... 1111`; there must be sixteen functions to produce them.

Here they are in the order dictated by their results as above:

```
    b   n  f
 0000   0
 0001   1  ∧
 0010   2  >
 0011   3
 0100   4  <
 0101   5
 0110   6  ≠
 0111   7  ∨
 1000   8  ⍱
 1001   9  =
 1010  10
 1011  11  ≥
 1100  12
 1101  13  ≤
 1110  14  ⍲
 1111  15
```

The six missing, those not implemented as primitives, could be represented in the notation implemented as direct functions (dfns) in Dyalog APL as: `{0} {⍺} {⍵} {~⍵} {~⍺} {1}`. But these only work for simple scalar arguments as they take no cognizance of the left, the right or, in two cases, either argument. We need to reference both of them to produce an array of the required structure valued as the identity or negation of one boolean function that we apply to the other argument.

```
    b   n  f
 0000   0  {⍺∧0∧⍵}  ⍝ false
 0011   3  {⍺∧1∨⍵}  ⍝ left (left notwithstanding right)
 0101   5  {⍵∧1∨⍺}  ⍝ right (left nevertheless right)
 1010  10  {⍵=0∧⍺}  ⍝ negate right
 1100  12  {⍺=0∧⍵}  ⍝ negate left
 1111  15  {⍺∨1∨⍵}  ⍝ true
```

## Preliminary results { #Prelim }

In the following list the comment describes the essential character of logical vector `⍵` when `1<⍴⍵` and the result of `f/⍵` is true.

```
    b   n     f
 0000   0  {⍺∧0∧⍵}  ⍝ never
 0001   1     ∧     ⍝ all ones
 0010   2     >     ⍝ odd leading ones
 0011   3  {⍺∧1∨⍵}  ⍝ first is one
 0100   4     <     ⍝ last is the only one
 0101   5  {⍵∧1∨⍺}  ⍝ last is one
 0110   6     ≠     ⍝ odd ones
 0111   7     ∨     ⍝ at least one one
 1000   8     ⍱
 1001   9     =     ⍝ even zeros
 1010  10  {⍵=0∧⍺}  ⍝ last is parity of the length
 1011  11     ≥     ⍝ even leading ones
 1100  12  {⍺=0∧⍵}  ⍝ first is zero
 1101  13     ≤     ⍝ last is not the only zero
 1110  14     ⍲
 1111  15  {⍺∨1∨⍵}  ⍝ always
```

Those missing descriptions, *nand* and *nor*, caused this paper to be delayed by a period approaching two decades.

At "Iverson College 2012" at Cambridge we were discussing Roger Hui's presentation "How to write an elegant computer program" and moved on to the subject of the semantics of the boolean functions and how many of them can be used in unconventional ways to increase speed and elegance. The reference to `p>q` being equivalent to `p∧~q` above being one such. And of course Simon Garland's memorable: *To be `≥` to be, that is the question.* being an obvious improvement on the original.

In conversation afterwards about the topic of this paper Roger mentioned that he had recently come up with efficient definitions for the missing pair.

These were in the form of an operator that implemented the four reductions: `≤/ ≥/ ⍱/ ⍲/`. But he had also made an effort to verbalise them. I include only those for my missing pair.

After many false starts my most concise wording of them is:

<dl>
    <dd><code> ⍱/ </code>implies odd leading zeros else the last is the only one.</dd>
    <dd><code> ⍲/ </code>implies even leading ones else the last is the only zero.</dd>
</dl>

They can be summed up in these two rank- and origin-independent direct functions:

```
      ⍱/ ←→ {(+/∧\~⍵){(2|⍺)=⍺<⍵-1}⊢/⍴⍵}
      ⍲/ ←→ {(+/∧\⍵){(2|⍺)≠⍺<⍵-1}⊢/⍴⍵}
```

or first-dimension oriented as:

```
      ⍱⌿⍵ ←→ {(+⌿∧⍀~⍵){(2|⍺)=⍺<⍵-1}⊣/⍴⍵}
      ⍲⌿⍵ ←→ {(+⌿∧⍀⍵){(2|⍺)≠⍺<⍵-1}⊣/⍴⍵}
```

*\[ `⊢/` and `⊢/` (and `⊢⌿` and `⊢⌿`) are idioms for selecting the first and last sub-arrays from an array \]*

see [Appendix: Traditional function equivalents](#trad) if these are incomprehensible to you.

So here they are in context:

```
    b   n     f
 0000   0  {⍺∧0∧⍵}  ⍝ never
 0001   1     ∧     ⍝ all ones
 0010   2     >     ⍝ odd leading ones
 0011   3  {⍺∧1∨⍵}  ⍝ first is one
 0100   4     <     ⍝ last is the only one
 0101   5  {⍵∧1∨⍺}  ⍝ last is one
 0110   6     ≠     ⍝ odd ones
 0111   7     ∨     ⍝ at least one one
 1000   8     ⍱     ⍝ odd leading zeros else the last is the only one
 1001   9     =     ⍝ even zeros
 1010  10  {⍵=0∧⍺}  ⍝ last is parity of the length
 1011  11     ≥     ⍝ even leading ones
 1100  12  {⍺=0∧⍵}  ⍝ first is zero
 1101  13     ≤     ⍝ last is not the only zero
 1110  14     ⍲     ⍝ even leading ones else the last is the only zero
 1111  15  {⍺∨1∨⍵}  ⍝ always
```

All this might seem very esoteric but these can prove useful in very different contexts.

<dl>

<dd>
    APL coders can produce simple expressions to validate the resulting pattern
    of a multiple boolean check.
</dd>

<dd>
    Interpreter writers can convert the description into fast code written in their
    favourite compiled language. Possibly your own interpreter already benefits from this.
</dd>

<dd>
    APL coders can anticipate their vendors efforts and produce fast work-arounds
    for slow implementations.
</dd>

</dl>

## Appendix: Traditional function equivalents { #trad }

for the `⍱/` variants:

```
     ∇ r←norRed w;n
 [1]  ⍝ {(+/∧\~⍵){(2|⍺)=⍺<⍵-1}⊢/⍴⍵}
 [2]   n←+/∧\~w
 [3]   r←(2|n)=n<¯1+0⊥⍴w
     ∇
```

for the missing boolean primitives:

```
     ∇ r←a left w
 [1]  ⍝ {⍺∧⍵=⍵}
 [2]   r←a∧w=w
     ∇
```

The others can easily be inferred as they differ to the same extent as their counterparts.
