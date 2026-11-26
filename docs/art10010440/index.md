---
vid: '10010440'
title: 'J-ottings 30: Jaesthetics'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '18'
issue: '2'
page: '50'
source: trad/v182/jot182.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2001
tags:
- programming techniques
- development practice
---

*by Norman Thomson*
{ .byline }

Many J users take the definition of an adverb `each` as a pseudo-primitive, and find it more comfortable to use

```
   each=.&>.
```

rather than `&>.` which is 25% shorter. However, there are in my view two compelling arguments in favour of `&>.`, viz. independence from any specific natural language, and avoidance of the ambiguity which arises because `each` in some contexts is also used to mean `&>` .

I argue in this article that there are matters of taste involved in writing J, and that J programmers inevitably make choices between pieces of code which are identical as far as effects on data are concerned.

A contributor to the J forum recently made the point that for J aficionados, J phrases can become a more natural medium for expressing ideas than any natural language equivalent. Jim Lucas stated that he didn’t care for expressions with lots of `@`s in them, because `@` is a fundamentally ugly symbol. I also have some reservations about `@`, but for a different reason, namely that `@` and `@:` , imply **sequence**, and pieces of code strung together with `@`s look much like the kind of line-by-line code observed in more mundane computer languages.

Two examples illustrate both of these points. The first is `=@i.@#` which gives an identity matrix of the same shape as a square matrix (see J-ottings 29 for a motivation for defining this). This phrase has a good feeling of symmetry on account of the two `@`s. It is analogous to a word such as “fever” in which consonants and vowels alternate, only now read verbs and conjunctions. As for its meaning, the `@`s say that there are three things which have to be done in sequence, first a *tally*, then an *integers* of that *tally*, and finally a *self-classify of* the result of *integers.*  If I were to assign and name this verb for repeat use I would probably call it something like MIM or MIdMat, standing for Matching Identity Matrix. In this case I am not sure whether MIdMat or `=@i.@#` conveys more on later recall.

Now consider the somewhat similar phrase `#i.@#` :

```
   (#i.@#)0 1 0 1 1
1 3 4
```

which converts a bit string into the indices of the 1s. Again the phrase has an overall shape and feel, in this case that of a word like “trot”. A name for assignment purposes might be something BtoInd which has a much lower mnemonic value than MIdMat, and in this case I am confident that my preference for recall purposes is `#i.@#` .

Another recent exchange on the forum offered two approaches to the problem of obtaining a vector of occurrence numbers. The problem is that of taking a sequence such as

```
   m=.'mississippi'
```

and obtaining the vector 0 0 1 0 1 2 3 0 2 1 0 in which each item of `m` is replaced by its occurrence number. Chris Burke and Cliff Reiter offered two different stylistic approaches. Here is Chris’s:

```
   ocb=.[:((]-{)/:@/:)i.~
```

and here is Cliff’s:

```
   ocr=.;@(<@i.@#/.~)/:[:/:~.i.]
```

If I were completely fluent in J, I would be able to understand and compare the two on sight without recourse to English to define intermediate verbs. Although there is a temptation to break these down into smaller verbs, I ask you, the reader, to make the experiment of pretending at this point that J is the only language you “speak”, and to make sense of these expressions on that basis alone.

Looking at `ocb` first, I have to say that I am not a fan of cap (`[:`) which I usually like to replace with an equivalent `@`, yielding in this case

```
   ocb1=.((]-{)(/:@/:))@ i.~
```

The case made for *cap* when it was first introduced into J was that it enabled trains of indefinite length to be constructed without parentheses. In reading J phrases I find that “breathers” in the form of parentheses and `@`s are positively welcome, and the *cap* argument seems a bit like the case for removing gaps between movements of symphonies on the grounds that it gets you to the end sooner.

An obvious first step in solving the occurrence number problem is to translate the vector into an “order of first appearance” integer vector, that is every ‘i’ in `m` maps into the index of the leftmost ‘i’, namely 1, and similarly for the other characters.

This is just what `i.~m` does:

```
   ]t=.i.~m
0 1 2 2 1 2 2 1 8 8 1
```

`t{m` is just `m`, since it makes no difference whether a selecting index is that of the first or any later occurrence. I can see therefore the role of the rightmost verb in `ocb`, and also note that, without loss of generality, the rest of this discussion can be restricted to vectors of this sort – this is the force of the rightmost @ in this case, and is incidentally a general useful property of @. What remains is clearly a hook, whose right verb is easily recognised as the upward ranking verb `/:@/:`. A significant problem in discussing the *grade* verbs is the promiscuity of the word “rank”, which means one thing in the context of J and another in the context of arithmetic generally. I think it best to use the word “ranking” to describe the latter sense and follow J practice of counting from index origin 0, so that the score of the best golfer has upward ranking 0, that of the fifth best golfer upward ranking 4 and so on. The upward ranking of a vector is the vector of the upward ranks of its items in their original order. The left verb of the hook

```
(]-{) (/:@/:)
```

in `ocb` is a fork in which the effect of *right* is to make the right argument `t` the left argument of *minus*, the right argument of which is `t{gt` where `gt` is the upward ranking vector of `t`.

Now pause awhile and consider what happens when a vector of indexes of first appearances (call this an `ifo` vector for short, and define `ifo` =.`i.`~) selects *from* its own upward ranking vector. Ranking in J is a **progressive** operation in the sense that if a vector contains more than one 0, the second 0 from the left is ranked 1, the third is ranked 2 and so on. However, in selection using an `ifo` vector, duplicate items in `t` repeatedly select the upward ranking of first occurrence making the process non-<b>progressive</b>. This is easier to observe by tracing than to describe in words

```
   t
0 1 2 2 1 2 2 1 8 8 1
   /:/:t        NB. upward ranking of t
0 1 5 6 2 7 8 3 9 10 4
   t{/:/:t      NB. t indexing its own ranking
0 1 5 5 1 5 5 1 9 9 1
```

Subtracting the last two vectors in the above sequence gives the occurrence number vector.

```
   ocb m
0 0 0 1 1 2 3 2 0 1 3
```

All of this can be summarised by saying occurrence number is upward ranking minus index of first appearance. I could express this in mathematical symbols, but I doubt whether this would achieve anything like the clarity of the previous sentence. It is precisely this **derived** consequence of the semantics of *grade-up* and *index* which Chris has exploited delightfully in `ocb`.

The fork in `ocb1` raises a stylistic consideration. The purpose of \] is to achieve the argument reversal needed to obtain

```
   (/:/:t) - t{(/:/:t)
```

I have reservations about using `[` and `]` similar to those I expressed concerning `@`, namely that `[` and `]` are a thinly disguised way of referring directly to the data arguments, and thereby causing reversion to traditional programming language style rather than that of tacit programming.

If `up` is defined as

```
   up=./:@/:
```

then the fork in `ocb1` can be written as

```
   up -({up)
```

and `ocb1` can be written

```
   ocb2=.(up-({up))@ifo
```

Of course I am now taking refuge in words, or at least verb-names as pseudo-words, no doubt thereby laying bare my shortcomings in J fluency. As far as recall is concerned, while `ifo` is quite memorable (although no shorter than `i.~` ), the name `up` is uselessly general. I could of course follow the practice of C++ programmers and similar infidels, and call it something like `upward_ranking` but this would be self-defeating, since `/:@/:` is much shorter and infinitely more expressive.

As a spin-off, the hook `{up` delivers the non-progressive upward ranking in which all equal values have the same ranking. For example

```
   ({up) 0 1 2 0 2 5 0
0 3 4 0 4 6 0
```

Next consider Cliff’s verb:

```
   ocr=.;@(<@i.@#/.~)/:[:/:~.i.]
```

The first thing to observe is the presence of the *key* adverb (`/.`) which is what I imagine many J users, myself included, would latch on to as the germ from which to develop a solution to this problem. Again pretend that J is the only available vehicle for communication. The presence of several `@`s indicates that Cliff has taken an essentially sequencing approach, particularly as *cap* can, as before, be exchanged for `@` to give

```
   ocr1=.;@(<@i.@#/.~) /: /:@( ~. i. ])
```

This reveals that the main structure is a fork with the leftmost `/:` as its central prong. As far as the right prong is concerned, two things happen in sequence. First the phrase within the parentheses is *index* by *nub*, for example

```
   (~.i.])m
0 1 2 2 1 2 2 1 3 3 1
```

which I recognise as another way of describing `ifo`, and incidentally conveniently deals with my distaste for `]` !

Then this is graded upwards

```
   /:ifo m
0 1 4 7 10 2 3 5 6 8 9
```

to give the **positions** of the 0s, followed by the positions of the 1s, followed by the positions of the 2s and so on, in other words it is the vector of positions required to *grade-up* dyadically the sequence of all occurrence numbers ordered by item.

*Key* seems a natural way of obtaining this sequence. We have already seen how `i.@#` works

```
   (i.@#)m
0 1 2 3 4 5 6 7 8 9 10
```

Applying *key* using the items of `m` as keys gives

```
  m(i.@#/.)m
0 0 0 0
0 1 2 3
0 1 2 3
0 1 0 0
```

which is then <i>box</i>ed followed by *raze* - a standard device to remove unwanted fill characters. Putting all of this together and including a *reflex* to keep things monadic, the left prong may be summarised as

```
   ocnobykey=.;@(<@i.@#/.~)
```

and the left prong by

```
     posbykey=./:@ifo
```

leading to a further redefinition of `ocr` as

```
   ocr2=. ocnobykey /: posbykey
```

An objection to this use of pseudo-English is that revisiting `sortbykey` and `posbykey` again poses as great a problem in remembering exactly what was meant by these rather vague verb names as that of interpreting directly the unambiguous J strings. The only significant arguments for decomposition of long verbs into shorter verbs is the “breather” effect on the one hand and clarification of top-down structure on the other.

In summary, three separate dimensions of stylistic choice have evolved in the above discussion which have to be made in J programming. First there is the balance between English and J, (with 100% of the latter not ruled out), then secondly the different problem solving approaches of “derive a new property from old” versus “program using existing resources” (`ocb` style versus `ocr` style), and thirdly, choices between J equivalences such as *cap* versus *atop,* as well as avoidance techniques for `[` and `]`. Of course none of this takes account of the hugely important dimensions associated with implementation – often a longer piece of code will be appreciably more efficient than a more elegant alternative. At this point art and science come to a divide where things begin to get severely practical – most certainly relevant, but not the province of Jaesthetics!

***
