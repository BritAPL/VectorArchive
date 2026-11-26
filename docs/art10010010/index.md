---
vid: '10010010'
title: J-ottings 2
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '10'
issue: '3'
page: '29'
source: trad/v103/thomson103_29.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- language design
- education
---

by Norman Thomson <span class="affil"></span> <span class="email"></span>
{ .byline }

First, a recantation! In the previous J-ottings I fell into a trap (rapidly pointed out to me by Donald McIntyre) of confusing the nouns R and L in the APL expression 1+R-L with the J verbs \[ and \] which mean in their dyadic form take the left and right arguments respectively, and monadically same, that is take the argument unchanged. The sentence 4>:\]-\[5 is interpreted exactly as in APL, that is three monadic verbs are executed in succession right to left followed by a dyadic one >: meaning is greater than or equal. The intermediate stages of evaluation are therefore

```
4>:]-5
4>:]_5
4>:_5
1
```

This leads me to the point where the previous J-ottings left off, namely with the thesis that difficulties in learning J arise from the need to understand some profoundly important concepts at a fundamental level. One of the most important is that of verb trains. Hook and fork are the simplest examples of verb trains and embody rules to which there is nothing directly comparable in APL. A hook is a pair of successive verbs, unsymmetrical in the sense that the right verb is necessarily monadic and the left necessarily dyadic. If no left argument is present, the right argument doubles as left argument.

In the case of fork the outer verbs are executed before the inner one which means that a fork is conceptually an elementary tree structure.

The inclusion of parentheses in the sentence above, that is

```
4(>:]-[)5
```

defines a train of four verbs. As in APL, the parentheses cause the evaluation of their contents prior to involvement with their arguments. The functions on their own are combined - a hook or a fork is produced from a train and becomes a single verb. In this case they first (working from right to left) form a fork \]-\[ and then the verb >: combines with the fork-verb to form a hook.

Dealing first with the fork, this has a right argument only, and so the left argument (note that a fork must have a left argument) is supplied by repeating the right argument, and so the verb sequence \]-\[ evaluates to (right argument - right argument), that is zeros in the shape of the right argument. Thus the sentence above is interpreted (executing the verbs from right to left)

apply the fork to give 0,  
then apply the hook which asks the question:  
is 4 greater than or equal to 0?,  
answer 1

So the expression ignores everything about the right argument (5) except its shape. The same effect is achieved by defining the explicit verb

```
f1=.>:]-[
```

which thus returns the answer to the question “is the left argument positive or 0?” As Donald points out, this is more expeditiously answered with the defined hook

```
f1=.>:0:
```

thereby introducing the verb 0: which delivers the value 0.

J also allows the sequential composition of verbs which is explicitly denoted by the conjunction @ (atop). Again there is no directly comparable primitive form in APL. If sequential composition is introduced prior to the fork, viz:

```
f2=.>:@]-[
```

Since conjunctions are evaluated before hooks and forks, this is equivalent to

```
f2=.(>:@])-[
```

The above may give some insight into the richness of the J language and of the subtleties of the interpretation of J expressions.

J has the facility for tacit definition. This is an extra in the way that some APLs have Direct Definition built in. With tacit definition you may define a verb train, give it a name and its right and (possibly) left argument are not shown. The arguments to the (possibly) left and right of the defined name are then inserted to the left and right of the defined expression. So you can define the mean of a vector as

```
mean =.+/%#
```

then if, for example, w=.1 4 2 2, both mean w and (+/%#)w have the same value namely 2.25. On the other hand if the parentheses are not present the result of +/%#w is the same as +/÷½W namely 0.25. A very convenient and appealing hook in this context is -mean:

```
(-mean)w
_1.25 1.75 _0.25 _0.25
```

which, applying the hook rule, means take the mean away from every item of vector w. (Donald McIntyre in “AMENDMENT – A Change for the Better” (Vector 9.3) describes how this can be extended elegantly to deal with nouns of higher rank.)

Forks can also be used in tacit as well as explicit form. Mean itself qualifies in this regard; so does (>./-&lt;./) for range of a vector.

(A gentle test for the reader at this stage: what does the monadic hook (--) mean?)

I consider it is helpful to develop an understanding of J by distinguishing between similar sentences, for example consider the result of applying the following tacit verbs:

```
(+/-!)0 1 2 3 4
(+/@-!)0 1 2 3 4
(+/@(-!))0 1 2 3 4
```

Round brackets on the whole aid understanding, so consider the final expression first which is “+ insert” atop in sequence the hook (-!). Since there is no right argument the value of the hook is each of the five numbers minus its factorial, that is \_1 0 0 \_3 \_20. Adding these gives the value \_24.

In the second expression the removal of the inner parentheses changes the nature of the verb to a hook whose right component is ! and whose left component is the verb sequence “+/ following -.” A composition like this, that is two verbs joined by a conjunction, behaves as a single verb entity. Since the rank of the rightmost verb is 0 (in APL terms it is a scalar function) the composed verb also assumes rank 0, that is it is applied on an item by item basis. This is an instance of the rule of “Inherited Rank” (see “Mastering J” by Donald McIntyre in APL91 Proceedings).

Finally remove the @ as in the first sentence and what remains is a fork in which the outer verbs are evaluated first so that the sentence is equivalent to (+/0 1 2 3 4) - !0 1 2 3 4 that is 9 9 8 4 \_14

Now try repeating this replacing ! with % - this should at the least acquaint you with the symbol \_ \_ standing for minus infinity!

In conclusion consider the definition of a verb ss which describes how to obtain the sum of squares of a vector about its mean. First use the hook -mean, then in sequence “square” and “add insert”:

```
ss=.+/@*:@(-mean)
ss w
4.75
```

This is indeed what is wanted, but it is a good idea to test understanding by considering the meanings of some different but similar forms:

```
ss1=.(+/@*:)@(-mean)
ss1 w
4.75
```

This means apply a doubly composed verb “(sum atop square) atop mean deviation” which gives the same result as ss. Now consider

```
ss2=.+/@*:@-mean
1.5625 3.0625 0.0625 0.0625
```

The priority rule for conjunctions means that ss2 is the hook ((+/@\*:)@-)mean. mean acts monadically and delivers its result to the doubly composed verb ((+/@\*:)@-) which acts dyadically with the original argument as left argument and the mean as right argument. Following the Inheritance Rule, the verb (+/@\*:) inherits rank 0 from the minus and so is equivalent to \*:. ss2 is thus a rank 0 verb and its result of ss2 is the squares of the negation of the mean deviations.

The parentheses around (-mean) are thus critical for a correct “sums of squares” definition in order that a rank of 1 is inherited by the later stages of square and sum. More generally the fundamental nature of verb rank gives rise to one of the most important differences between J and APL, and deserves the strongest emphasis.

*Ed’s comment*: The answer to Norman’s gentle test is that the expression (--), if monadic, means subtract from the right argument the negation of the right argument, so the effect is to double each item in the right argument as in the two examples below.

```
(--) i.5
0 2 4 6 8
(--) 2 3$ i.5
0 2 4
6 8 0
```

(webpage generated: 18 February 2006, 02:16)
