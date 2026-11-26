---
vid: '10500410'
title: 'J-ottings 53: Punctuation and rank'
prefix: J-ottings 53
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '24'
issue: '4'
page: '96'
received: '2010-07-20'
online: '2010-03-20'
description: Grammar for pandas? Norman Thomson eats roots, shoots and leaves.
source: content/printed/244/jot53.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- language design
- programming techniques
---

by Norman Thomson
{ .byline }

Lynne Trusse created a best-selling book on the art of punctuation, famously drawing its title from the story of the panda which, after visiting a restaurant “eats shoots, and leaves”, a plausible option allowing for a touch of anthropomorphism, and one which might not cause too great a disturbance to other diners. If on the other hand the panda “eats, shoots, and leaves” the effect is likely to be very different.

Much has been made of the manner in which the constructs of J were inspired by and derived from the parts of speech of ordinary language grammar, nouns, verbs and so on, despite which little reference is made to punctuation. While analogies should not be pushed too far – e.g. unlike *shoots*, there can never be any verb/noun ambiguity for primitive J objects – the concepts of explicit punctuation as provided in J by parentheses and space, and implicit punctuation through the two verb forms *hook* and *fork*, can be helpful in interpreting expressions.

It can be tempting to think of conjunctions such as `@` (*atop*) as punctuators as in e.g.

```
   ((i.#)t);(i.@#)t=.'abcde'
+-+---------+
|5|0 1 2 3 4|
+-+---------+
```

However the role of `@` in the above is that of a neologiser, that is, it constructs a new compound verb, call it ‘index-tally’, operating in scalar fashion on the items of the object `t` to its right. The operational details of such verbs lead naturally to consideration of the most subtle of all J concepts, namely *rank*.

Consider three compound verbs which differ only in punctuation.

```
v1=.>:@i.@#
v2=.>:@(i.@#)
v3=.(>:@i.)@#
```

The *effect* of all three verbs is the same, that is they are *semantically* equivalent as demonstrated by

```
   (v1 t);(v2 t);(v3 t=.'abcde')
+---------+---------+---------+
|1 2 3 4 5|1 2 3 4 5|1 2 3 4 5|
+---------+---------+---------+
```

More general questions are:

1. For any verbs `a b c`, to which (if either) of the two forms `a@(b@c)` and `(a@b)@c` is `v1` *necessarily* equivalent?
2. Is `v2` equivalent to `v3`, or, in mathematical terms, is the conjunction `@` associative?

Intuitively it should not be, for the same sort of reason that *eats, shoots and leaves* has a different meaning from *eats shoots, and leaves*. The scope rule for conjunctions is that they bind closely on the right, which means that it is `v3` which is equivalent to `v1`, and this of course is guaranteed by the J interpreter. Using conjunctions is equivalent to coining new verb-names from old in English, so that the meaning of `v2` is ‘increment-(index-tally)’ as opposed to `v3` which is ‘(increment-index)-tally’.

In learning J it takes a degree of mental adaptation to grasp the idea of a compound verb such as ‘index-tally’ let alone triple compounds like `v2` and `v3`, and also to appreciate that the meaning of ‘index-tally’ is not *index then tally*. This difference can be demonstrated by:

```
   (|.@*:)t=.1 2 2 3  NB. rotate-square
1 4 4 9

   (|.@:*:)t          NB. rotate-following-square
9 4 4 1
```

which suggests that the J terminologies ‘a atop b’ and ‘a at b’ are rendered more comprehensibly in pseudo-English as ‘a-b’ and ‘a-following-b’. The compound verb ‘rotate-square’ has rank zero because it takes on the rank of its rightmost component, a property which, for obvious reasons, is called *rank inheritance*. However, in ‘rotate-following-square’ the colon in `@`: can be thought of as signalling a pause in which rank is readjusted before the left hand verb is executed.

The nature of the arguments which can be presented to a conjunctionally compounded verb depend on the arguments presented to its rightmost verb, and so returning to `v1`, `v2` and `v3`, all of these require an argument acceptable to *tally*. This can be any J object since all J objects are fundamentally lists and so can be tallied at their topmost level. Also since the three verbs are to be executed in right to left succession it would seem, superficially at least, to make no difference how they are parenthesised as the transformed data is ‘passed down the line’ from right to left. However, as ‘rotate-square’ shows, rank inheritance has to be taken into account in the general case.

## Rank lists

Verbs can be categorised according to their rank properties in a manner comparable to conjugation in classical language grammar (see the appendix). Every verb has a rank list, viz.

monadic rank   left rank   right rank

which can always be explicitly obtained by applying the *basic characteristics* adverb `b.` and using `0` as right argument of the resulting verb. Rank can be infinite, and most verbs of infinite rank are *structural*, meaning that, like *box*, they are designed to operate on their argument or arguments as a whole, that is, they do not ‘penetrate’ the outer shells of objects. *Grade-up* is a useful illustration of the notion of ‘infinite rank’ because however large the rank of its argument, it orders objects at the next lowest rank level, thus

```
   /:i.2 10 20 30 40  NB. gradeup two 4 dimnl objects
0 1
   /:i.5 10 20 30 40  NB. gradeup five 4 dimnl objects
0 1 2 3 4
```

*Infinite* is the default verb rank, which is also the rank of all but the simplest user-defined verbs, since the interpreter could potentially be forced to perform exhaustive and unproductive effort to work out the *de-facto* rank, and so it makes the ‘safe’ assumption of *infinite*. However the rank conjunction allows rank to be used flexibly as in

```
   mean=.+/ % #
   mean0=.(+/%#)"0     NB. scalarised mean
   (mean i.5);(mean0 i.5)
+-+---------+
|2|0 1 2 3 4|
+-+---------+
```

Here are the relevant rank lists :

```
   (mean b.0);(mean0 b.0)
+-----+-----+
|_ _ _|0 0 0|
+-----+-----+
```

## Rank inheritance

Returning to ‘rotate-square’ whose rank list is `0 0 0`, although *rotate* is a rank-1 verb, rank inheritance forces rotation at rank 0 (that is, equivalent to an explicit `"0`) and so it inherits a list of rank-0 objects (scalars) each of which has to be treated as a list, with the result that it does nothing. However, if rank is *not* inherited as with `|.@:*:`, then rotation applies to the list of squares as a single entity of rank 1.

The equivalence of `a@(b@c)` and `(a@b)@c` (i.e. associativity) depends on the rank of the inheriting verb being no greater than that of the giving verb, something which will certainly take place if `a`, `b` and `c` are all rank-0 verbs, but which has to be examined in terms of verb rank properties when this is not the case. Rank inheritance from higher to equal or lower creates no problems as in

```
   (>:@i.)6       NB. rank 0 inherits rank 1
1 2 3 4 5 6
```

However, compare

```
   (/:@>:)7 3 5   NB. rank infinite inherits rank 0
0
0
0
```

in which each of the three incremented values is upgraded separately, with

```
   (>:@/:)7 3 5   NB. rank 0 inherits rank infinite
2 3 1
```

The next two examples involve verbs of equal ranks, again there is no inheritance issue, although changing the order of the verbs gives a different result because the grade-up of a transposed matrix is not the same as the transpose of a grade-up of the original matrix.

```
   (/:@|:)i.2 3   NB. both ranks infinite ..
0 1 2
   (|:@/:)i.2 3   NB. .. but the result is different
0 1
```

## Mood, transitivity and commutativity

J verbs are restricted to the imperative mood apart from the verb ‘to be’ (*copula*). Mood is independent of transitivity, meaning that a verb is either monadic (intransitive) or dyadic (transitive). For transitive verbs the arithmetic commutativity of say `+` means that `2 + 3` is in every respect equal to `3 + 2`. However when a computer does addition it is impossible for both arguments to be fetched simultaneously, and so, analogously with transitive verbs in English for which the subject is in some sense ‘stronger’ than the object, the left argument of dyadic verbs binds more strongly than the right. This becomes apparent when repetition is invoked by the power adverb. Thus `2+^:(2)3` means *add 2 twice to 3* (answer 7), as opposed to *add 3 twice to 2* (answer 8).

## Conjugations

Returning to the conjugations, scalar verbs which have rank 0 are the ‘most penetrating’, meaning that unless otherwise modified by the rank conjunction they operate at the lowest cell levels. The ordinary arithmetic verbs are thus all of rank 0. Second-conjugation verbs are all monadic and operate at the level of lists, such as `i.` (*index generator*) and `#:` (*base 2*). The conjugations range from the most penetrating in the first conjugation through to those of the 4th and 5th conjugations which handle objects at a macro level. In between at the third conjugation are a set of verbs which are the counterpart of irregular verbs in natural-language grammars.

## Pseudo-punctuation

Returning to punctuation, there are three verbs, all of infinite rank, which can be thought of as providing a ‘pseudo-punctuator’ role. These are `,` `;` and `,.`

In each of the examples below the pseudo-punctuator is the middle tine of a fork, and the sum and difference of a list can be ‘punctuated’ in the following ways :

```
   5 4(+,-)2 0      NB. sums, then differences
7 4 3 4

   5 4(+;-)2 0      NB. boxed sums, boxed differences
+---+---+
|7 4|3 4|
+---+---+
    5 4 (+,.-)2 0   NB. sums, diffs as separate dimensions
7 3
4 4
```

The verb `[`, also of infinite rank, provides pseudo-punctuation in the form of a statement separator:

```
   a=.2 [ b=.3
   a,b
2 3
```

which works because the above line is effectively

```
   a=.2[3
7
```

Square brackets can sometimes give rise to what looks orthographically like ‘verb parentheses’ as in

```
   2(*:@[+])3
7
```

although arguably the above phrase would have been written with more clarity as

```
   2((*:@[) + ])3
```

## Redundant punctuation

As the above example shows, redundant parentheses can be invaluable in clarifying the meanings of tacit definitions, although, like all good things, it can be overdone, and too many parentheses can sometimes be just as confusing as too few. Once a string of J symbols exceeds about seven characters even an expert reader’s eyes begin to glaze over. Consider for example:

```
   lens=.<"1 @:,.3&":@:i.&' '"1
```

An example of using `lens` might help to clear the fog :

```
   lens >'Florida';'California';'Alaska'
+-------------+-------------+-------------+
|Florida     7|California 10|Alaska      6|
+-------------+-------------+-------------+
```

Things become clearer still if `lens` is rewritten with some parentheses and an explicit space for the hook :

```
   lens1=.<"1 @: (,. ((3&":)@:(i.&' ')"1))
```

But following the seven-character rule it would have been even better to articulate some of the bits by giving meaningful names to verbs along the following lines :

```
   boxrows=.<"1
   format=.3&":          NB. width = 3 characters
   length=.(i.&' ')"1    NB. gives length of string
   lens2=.boxrows@(,.(format@length))
```

Interestingly, although the above four lines appear at first sight to have only a few primitive symbols, all such symbols in `lens` are faithfully reproduced. Arguably it would have been better to write `lens` this way in the first place as this helps to contrast the `@:`s which define compound verbs, with the implicit punctuation in the hook `,.(format@length)`.

Thoughtful punctuation can often help documentation. As a further example, most readers would find that on first sight the following verb definition conveys little of its purpose:

```
   verb=.(+/@:*:@:-+/%#)%<:@#
```

With some redundant parenthesising and renaming, and use of space to emphasise the fork, things become a little clearer :

```
   sdest=.(+/@:(*:@:(-+/%#))) % (<:@#)
```

and with a little more renaming of the parts

```
   sum=.+/
   mean=.+/%#
   mdev=.-mean       NB. mean deviation
   nminus1=.<:@#     NB. n minus 1
   sdest1=.sum@:(*:@mdev)%nminus1
```

the objective of providing the usual form of standard deviation estimate from a sample should become reasonably apparent.

## Cap

When cap (`[:`) was introduced it was argued that it allowed indefinitely long *trains* of verbs to be written without parentheses, thereby implying that parentheses were inherently undesirable. The analogy in English is to favour long strings of words without punctuation, which may not be to everyone’s reading taste. Forks and hooks work well because the human mind assimilates readily twosomes and threesomes, but thereafter the reverse is true, that is `a b c d e` wrongly suggests *a then b then c then …* whereas `a b(c d e)` gives a natural visual picture of the correct meaning. Continuing in the vein of the previous example `*:-+/%#` does not at first sight reveal its meaning whereas `*:-(+/%#)` says with reasonable clarity *subtract the mean from the squares*.

## Space

A first step in the parser of most compilers and interpreters is to remove redundant spaces which are often highly desirable at the orthographic level, for example to underline the fact that three primitive verbs form a fork. Successive digraphs can lead the reader through an unnecessary initial step of disentanglement as, for example, in verb above which, even without the suggested parenthesising and breaking down into smaller verbs, would be easier to interpret if written

```
   verb=.(+/ @: *: @: – +/%#) % <: @ #
```

that is, *(add following square following mean-adjust) divide by decrement-tally*.

On the other hand spaces are probably best omitted between verbs and their objects, e.g. `i. 5` is probably less clear than `i.5`, although it is best not to be too dogmatic.

## Appendix

### 1st Conjugation, rank vector = 0 0 0

Logicals (monadic)
:   `-.` (dyadic) `= ~: < <: > >: +: *:`

Arithmetics (monadic and dyadic)
:   `+ – * % ^ ^. <. >. | ! %: +. *.`

Arithmetics (monadic)
:   \-.

Algorithmics (monadic)
:   `p:` (ith. prime)

Algorithmics (dyadic)
:   `? j. o. r. q:`

### 2nd Conjugation, monadic, list oriented

`i. { ;: #. ". #: p.` (polynomial)  
`A.` (Anagram Index) `C.` (Cycle)

### 3rd Conjugation, irregular

monadic
:   `%.` (rank 2)

dyadic
:   `#:(1 0) p.(1 0) {(0 _) A.(0 _) %.(_ 2) C.(1 _)`

### 4th Conjugation, dyadics with left rank=1, right rank=infinite

`$ |. |: # {. }. ": {::` (fetch)

### 5th Conjugation, all ranks infinite

monadic
:   `= < ~. ~: {: }: #: $ |. |: # {. }. ":`  
    `L.` (Level) `{::` (match)

dyadic
:   `-. -: i. ". E.`(Member of Interval)

monadic and dyadic
:   `, ,. ,: /: \: ; e. $. $: [ ]`  
    `s:` (symbol) `u:` (unicode) `x:` (extended precision)

constant functions
:   that is `9:`, `_8:`, … , `0:`, `1:`, `2:`, … `9:`, also `_:` (infinity)
