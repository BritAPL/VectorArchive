---
vid: '10010020'
title: 'J-ottings 3: Atop and Agenda'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '11'
issue: '2'
page: '24'
source: trad/v112/thomson112_24.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1994
tags:
- language design
- education
---

by Norman Thomson
{ .byline }

The aim of J-ottings is to identify and articulate some fundamental hurdles which the practised APL user must clear in order to achieve full, or even passing fluency in J. The similarities between APL and J do much to disguise the importance of some of the fundamental differences. It is all too easy to start out in J and then give up, particularly in the absence of commercial pressures to drive the learning process along. To stop thus is to miss savouring the delights of travelling towards the high peaks of Roger Hui’s remarkable interpreter. The foothills are steep, but the view from the top is truly spectacular! Not only does J contain, as primitives, many things which have to be explicitly programmed in APL, but after a while the user begins to sense it having a kind of intelligence of its own, giving meanings through ruthless logic to combinations of symbols which may well have never been part of its author’s visions.

Quite apart from the, at times subtle, differences between APL and J there is much in J itself that is “similar but different” which makes the exercise of focusing on “look-alike pairs” a valuable aid in learning the language. What follows highlight two pairs of conjunctions, namely & and &. (with and under) and @ and @. (atop and agenda).

The first three result in compositions, that is if u and v are verbs (u and v are the standard shorthand for “verbs” in the J literature) then u&v, u&.v, u@v are single verb entities, so that a reference to say a left argument means the left argument of the composition and not one of the components.

In Jottings-2 the “sums-of-squares-about-the-mean” verb was given as

```
   ss=.+/@*:@(-mean)
   mean=.+/%#
```

This uses @, the conjunction which is called atop, or perhaps more expressively “sequence”. By reading its definition from left to right the verb ss is interpreted as “the sum of squares of mean deviations”, which is executed by performing mean deviation (-mean), square, and sum in sequence. This much is just like APL.

It is important to appreciate the need for the parentheses in the definition of ss, particularly since in APL a final parenthesis on the right is necessarily redundant unless vector notation is being used. Parentheses in both J and APL commonly appear to the left due to verbs and functions having long right scope. For example parentheses are required to ensure that an expression like (1/2)+3 means 3.5 rather than 1/2+3 = 0.2. Since the atoms of tacit programming are verbs, and adverbs and conjunctions have long left scope, parentheses tend to appear to the right rather than to the left. Without parentheses the definition of ss becomes

```
   ss1=.+/@*:@-mean     
```

whose meaning is given by the parenthesising     

```
   ss1=.((+/@*:)@-)mean
```

At this point a very important principle called rank inheritance must be introduced if the @ conjunction is to be understood fully. Rank inheritance says that in a composition f@g the verb f inherits the rank of the result of g. Thus in the verb ss1 with a vector argument, the result rank of mean is 0, and this rank is “passed along the line” to the remaining verbs which therefore operate at scalar level.

```
   ss1 1 4 1 2     
```

is executed by calculating the mean (it is 2) which becomes the right argument of the hook (...)mean for which 1 4 1 2 becomes the left argument since no explicit left argument is present. The contents of these parentheses are a sequence (...)@- and so - is applied first between 1 4 1 2 and \_2 to give \_1 2 \_1 0. This becomes the argument of the “sum-the-squares” sequence (composition) within the inner brackets, however rank inheritance decrees that this composition be applied at scalar level to give the final result 1 4 1 0.

The conjunction @: is a variant of @ which allows infinite rank. Thus substituting @: for @ in ss1     

```
   ss2=.+/@*:@:-mean     
```

restores the value of ss2 1 4 1 2 to that of ss 1 4 1 2, namely 6.

The difference between & (with) and &. (under) is highlighted by the most common application of the latter, namely multiplication using logs. In terms of frequency of usage this is to with/under what matrix multiplication (in the mathematical sense) is to inner product.

Define

```
   v0=. +&(10&^.)     
```

so that v0 is the “add-base10-logs” verb and 2 v0 3 is 0.69897 .

Now define     

```
   v1=. +&.(10&^.)     
```

which is the “add-base10-logs-and-take-antilogs” verb so that 2 v1 3 is 6 .

A delightful illustration of the use of “under” occurs in one of Donald McIntyre’s APL93 tutorials. In essence the problem is to calculate the value of the expression sqrt(1 - sqrt(1-x^2)). This can be done correctly by using the verbs \*: (“square”), %: (“square root”), -. (“1 minus”), and constructing correctly but mundanely the verb sequence     

```
   %: @ -. @ %: @ -. @ *:
```

However spotting that the first and last verbs in this sequence are inverses leads to the equivalent composition     

```
   (-. @ %: @ -.) &. *:
```

The parentheses, although syntactically unnecessary, help to indicate the next abbreviation which can be achieved because the first and last verbs in the parenthesised composition are also inverses, since -. is self-inverse. The composition can thus be reduced to

```
   (%: &. -.) &. *:
```

Here is another illustration which requires appreciation of the concept of rank-0 and rank-1 cells of a matrix such as i.2 3. This matrix may be conceived either as 6 rank-0 items or 2 rank-1 items which are what we would ordinarily call rows. The rank-1 items are “box”-ed (for which the verb is &lt;) along rank 1 by the expression &lt;"1 where the " symbol is the rank adverb. l=.&lt;"1 m is thus a pair of three-item lists.

```
-----------------
| 0 1 2 | 3 4 5 |      
-----------------
```

The inverse of “box” is “open” given by >, thus (\*:&>)l is the matrix m squared

```
0 1 4     
9 16 25
```

(\*:&.>)l is the squares of the two three-item lists.

```
------------------
| 0 1 4 | 9 16 25 |      
------------------
```

The relation between the conjunctions @ and @. is not at all like that between the conjunctions & and &. .

@. is strongly associated with the conjunction tie which produces a gerund, a word chosen by J’s inventors to underline the fact that what a tie ties is verbs but the result is a noun. The name gerund may at first appear misleading since the resulting noun bears no relation to gerund in its common usage in English as an “-ing” word. In J programming practice verbs are tied together into gerunds to provide the sort of choice which is rendered in other languages by the case statement, and the corresponding select clause is whatever folfc lows @. . This too is a conjunction with a gerund (that is a string of tied verbs) as left conjunct and the selecting index in origin zero as its right conjunct. Thus + ‘ - ‘ % @.2 selects the verb “divide” (remember zero origin!).

The combination of tie and @. called agenda appears frequently in recursively defined verbs, e.g.

```
   fac=. 1: ∩ (]*fac@<:)@. *
```

Monadic \* is signum so either the verb 1: is chosen if the argument is 0, or " argument times fac (argument following decrement-by-1)" if the argument is greater than 0.

g ‘: 0 means “append” the results of all the individual verbs comprising it, e.g.     

```
   g=.+`*     
   (g `: 0) 4     
4 1     
```

thus providing the function arrays which are absent from APL.

Observe that the colon is attached to the tie, not to the 0. Replacing 0 with 3 gives a variation which provides a neat way of generating n(n+1):     

```
   (g `: 3)4     
20     
```

: `3` means “evoke” the gerund in the sense of inserting the component verbs in turn and expanding the noun in the manner of $ (APL rho) to as many items as required. The expression above thus means that is `4+4*4`. Similarly     

<pre class="j">
   (- ` % `: 3)4     
3     
that is <tt class="j">4-4%4</tt>, and     
   (+ ` % `: 3)4 8     
6     
</pre>

that is `4+8%4`, with the second 4 arising from wrap-around.

(webpage generated: 14 October 2007, 18:37)
