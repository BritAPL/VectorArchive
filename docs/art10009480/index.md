---
vid: '10009480'
title: 'Letter: Adverb/Conjunction Combinations in J'
authors:
- Gordon Sutcliffe
byline: by Gordon Sutcliffe
volume: '11'
issue: '1'
page: '134'
source: trad/v111/sutcliffe111_134.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- language design
---

by Gordon Sutcliffe
{ .byline }

I hope the following problem is of interest to J enthusiasts and look forward to a solution.

The explanation of the combination of adverbs and conjunctions in J, is only partially covered in Ref 1. Consider, for example, four interesting dyadic cases of the conjunctions & and @:

```
1.     x u & v & w  y     u-v-w-x     (Dyadic u)
                     \v-w-y
2.     x u & v @ w  y     u-v-w-x     (Dyadic w)
                         \y
3.     x u @ v & w  y     u-v-w-x     (Dyadic v)
                       \w-y
4.     x u @ v @ w  y     u-v-w-x     (Dyadic w)
                         \y
```

where u, v, w are verbs and x, y are the arguments of the dyadic verb resulting from the execution of the conjunctions. The empirical results are expressed in terms of the original verbs.

The problem posed is this: How can these results be derived analytically? As J has a simple structure \[1,2\], there should be a simple explanation of how multiple adverbs/conjunctions combine to modify their verb arguments. As a start, consider an explanation for long verb trains:

Long Verb Train Execution: In a long (four or more) verb train \[3\], the right verb of the initial fork or hook is substituted by the fork to the right. In the case of an initial fork, the right verb of the fork has the same valence as the train, and hence, the substituted fork(s) have the same valence as the train. In the case of an initial hook, the right verb of the hook is defined always as monadic, and hence, the substituted fork(s) are monadic regardless of the train valence.

A similar explanation might explain the action of the conjunction examples above. That is, the initial dyadic environment being modified as the conjunctions are executed from left to right, and determining the valence of the substituted verbs, when they are subsequently executed from right to left. However, this is not how J works. While case 1, 3 and 4 would be explained; case 2 would have the same result as case 1.

So, back to the beginning, the original unsolved problem. What are the principles of J \[4\] required for synthesis of an explanation of the combined action of adverbs and conjunctions? What is the simple explanation for the conjunction examples?

### References

Ken Iverson, J Introduction & Dictionary Version 7, ISBN 1-895721-06-7

1. p1, Introduction, first paragraph: \`Although it \[J\] has a simple structure, ...'
2. p1, Introduction: \`5. The systematic use of adverbs and conjunctions to modify verbs, ...'
3. p69, F. Trains
4. p10, Vocabulary: \`C) The appended Dictionary of J provides a complete and concise dictionary and grammar.'

(webpage generated: 14 October 2007, 18:06)
