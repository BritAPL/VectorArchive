---
vid: '10010000'
title: J-ottings
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '10'
issue: '2'
page: '21'
source: trad/v102/thomson102_21.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- language design
- education
---

by Norman Thomson <span class="affil"></span> <span class="email"></span>
{ .byline }

Anyone attending APL93 and paying any attention at all to the presence of J would be struck by the divide between the experts and the much larger number of people to whom J, even after a reasonably long acquaintance, remains a source of intriguing perplexity. There can be no question that mastery does not come in the same blinding fashion that it did with APL, and yet the challenge does not go away as realisation dawns that the road is indeed harder – after all there is always the interpreter to prove that the more mysterious outcomes are backed by an incontrovertible logic.

Of course the sheer richness of meaning with which the mundane set of ASCII symbols have been imbued is both a source of marvel on the one hand and despair on the other as the task of memorising even a subset of the language is contemplated. As if this were not enough there are profound differences which divide the philosophy of J from that of APL and indeed of any other extant programming language, and it may be that the proponents of J have understated some of the more fundamental areas of understanding which are pre-requisite to a good grounding in J.

The object of this occasional column is to offer some “ground level” thoughts which may help to make J more accessible to readers of this magazine who might have been tempted to dismiss J as either an untidy transliteration of APL on the one hand, or an impossibly abbreviated symbolism on the other.

To start with, APL functions have a property of replaceability, by which is meant that if it is observed that

```
     +÷5
0.2
```

the +÷ can be replaced with a function

```
[0]  Z←FN R
[1]  Z←+÷R
```

to obtain

```
     FN 5
0.2
```

A defined verb in J does not however possess the replaceability property. + % 5 is indeed 0.2 but define fn =. + % and what happens is:

```
     fn 5
5.2
```

The verb combination (+ %) is quite different from the two verbs in succession. A rough parallel in English might be the sentence “Double cross the road” which could be understood as an instruction to cross the road twice, quite different in meaning from the admittedly less likely utterance “Doublecross the road.”

What has happened in defining fn is that + % has become the verb combination (+ %) and it is easily verifiable that (+ %)5 is indeed 5.2. This form of verb combination is what is known as a hook. In the absence of a left argument the right argument doubles up as left argument. The hook is essentially a lop-sided affair in the sense that the rightmost verb is applied to the right argument only, following which the left verb is applied to both. It is imperative that two arguments must be present. Without a hook construction (e.g. as in APL) “I double cross the road” describes my declamation “Cross the road, cross the road!” With a hook “I double cross the road” would mean that I cross the road, and then do so a second time, a sort of no-op! Like all analogies this one should not be pursued too far, however the principle should be clear, namely that replaceability as in APL function definition leads to a simpler but less rich form of language.

Function replaceability can if required be realized in J as sequential function composition which is described by the conjunction @.

We thus have

```
     gn =. + @ %
     gn 5
0.2
```

The difficulty of the mental hurdle over which hook requires us to jump may be reflected by the fact that it is hard to find compound words in English which are made up of two verbs. Others which come to mind are blow-dry and cross-check. Under sequential composition, to blow dry one’s (wife’s) hair would presumably mean making an unwanted disturbance to an already complete coiffeur; add the hyphen and things change a lot! Again cross check (no hyphen) suggests placing a final cross following an audit, which is by no means the same as cross-check! Of course it can be argued that the components in the foregoing word are not being used in a purely verbal way, which merely underlines the difficulty of finding verb compositions in English, and thus of relating hook to normal linguistic experience. I suggest it is good practice to separate uncomposed J verbs with spaces, and to remove these only when the verbs are combined either in verb definition, or with parentheses in immediate execution. This is entirely consistent with separating distinct words in English with spaces, whereas providing a hyphen gives, at least in some instances, a rough equivalent of hook.

If hook is connected, albeit tenuously, with our experience of English words, fork is connected instead to our most basic parsing experiences in English grammar. A fork is essentially a tree structure, something quite foreign to APL, but common enough in the “subject verb object” structure of simple sentences. Take the “world’s favourite one-line” as an example of a fork:

```
     mean =. +/%#
     w =. 1 4 2 2
     mean w
2.25                or equivalently
     (+/%#)w
0.25
```

A fork means take the outside verbs, +/ and #, apply them separately, and then – grand climax! – apply the one in the middle.

Now consider something more complex. First in APL define

```
[0]  Z←L F2 R
[1]  Z←1+R-L
```

a function which should not be too difficult for readers of this magazine to comprehend.

J is unlike APL in that the same verb >: doubles up as “greater than or equal to” in the dyadic case, and “1+,” that is “increment” in the monadic case. So the sentence

```
     4 >: ] - [ 5
```

means (executing the verbs from right to left)

take the left argument, namely 4  
negate it to give \_4  
take the right argument of this, which is still \_4  
is 4 greater than or equal to \_4, answer 1

The sentence above is thus a boolean expression. Now consider two verbs:

```
     f1 =.>: ] - [
     f2 =.>: @ ] - [
```

In the case of f1 the verbs combine from the right, three verbs in a row combining to form a fork for which the outside verbs are “take left argument” (\]) and “take right argument” (\[). These are applied separately, and then the minus applied to their results. Call this verb combination “range”. What is left is a two-verb combination which as we saw above must be a hook. So the question answered by f1 is:

is the left argument greater than or equal to the range?

Contrast this with f2 where there is sequential composition and the monadic meaning of >: has priority over the dyadic. f2 thus means:

increment the range in line with the APL function of the same name.

We have already progressed to J verbs whose definitions involve five ASCII characters – a large leap indeed for one session!

(webpage generated: 8 December 2005, 03:00)
