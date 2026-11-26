---
vid: '10011830'
title: 'J-ottings 49: Are You Thinking What I''m Thinking?'
prefix: J-ottings 49
authors:
- Norman Thomson
byline: Norman Thomson
volume: '23'
issue: '3'
page: '84'
description: Vector, the journal of the British APL Association
keywords:
- algebra
- APL
- array
- computation
- computing
- J
- K
- mathematics
- programming
- programming language
- programs
- Q
source: trad/v233/jot49.htm
converter: vecrescue 0.1.0
issue_tab: n3-june-2008
tags:
- APL in perspective
- language design
---

Norman Thomson
{ .byline }

“APL as a Tool of Thought” was the title of a long-running series of seminars held in New York, which, by a tiny change could be made into the claim “APL is a tool of thought”. Generalise this into the statement “Some language is necessary as a tool of thought” and the resulting statement is what is known to linguists as the [Sapir-Whorf Hypothesis](http://en.wikipedia.org/wiki/Sapir%E2%80%93Whorf_hypothesis "Wikipedia article") (SWH). In short, you cannot think unless you have a language.

Whether this is true or not is open to question. Is it possible for an individual to think thoughts which are totally beyond his or her powers of articulation? Could there have been a caveman possessing all the scientific and cosmological vision of an Einstein or a Feynman, but who, having no language, was never able to communicate the fact? Proponents of SWH say no.

“APL is a tool of thought” can be paraphrased “APL is a labour-saving device for thinkers” in the same way that a Dyson is a labour-saving device for cleaners. Given that serious thinking is a hard, brain-taxing activity, “APL is a labour-saving device for thinkers” is probably not in dispute, and indeed the same could be said of any computer language. So what about the proposition “J is a better labour-saving device for thinkers”, or more controversially still “APL (and J) enable people to think thoughts which were previously unattainable to them”.

Computer languages in general differ from natural languages in the privacy of dialogue. Spoken human language competence is tested by a continual stream of reactions from receivers; if the latter respond in a puzzled or incomprehensible fashion, the speaker can often repeatedly retry, thereby learning by failure and feedback, one of the richest means of increasing language competence. By contrast, the response to failure with computer languages is normally an error message, impassive, and emotionally neutral.

One of the ways in which J is sharply differentiated from other computer languages, even APL, is in the relative infrequency of show-stopping error messages. Whereas erroneous input in other languages is likely to cause suspension of execution, as often as not J continues, but delivers an output which is often quite different from that expected. Put in another way, whereas a faulty expression in other languages leads to non-understanding, in J it is just as likely to lead to misunderstanding. A possible response to an unexpected J output might be to shift a parenthesis a place or two, or to switch the odd conjunction, perhaps with the dialogue and resubmissions becoming more rapid as wishful optimism begins to overtake well-considered thought.

Any faulty input which did not deliver an error message is nevertheless the expression of a valid thought, failure to explore which is to forego a valuable opportunity for learning. Thus as well as working towards a correct expression, there is at least as much learning value in ascertaining what thought or intention would have resulted in the faulty input, which is after all the thought which would have been communicated to a more linguistically competent J receiver.

To be specific, consider a J dialogue in which I attempted to find all the combinations of `x` integers from `i.y`. My strategy, at first vaguely conceived in natural language, was first to obtain bit string patterns of all the natural numbers from `0` to `2y-1`, and then select those which contain exactly `x` bits, with the final step being to convert these bit strings into indices.

The first step is provided by a single J symbol. For the sake of definiteness, think in terms of combinations of `2` items out of `3`.

```
   ]t=.#:i.8       NB. 8 is 2^3
0 0 0
0 0 1
0 1 0
0 1 1
1 0 0
1 0 1
1 1 0
1 1 1
```

or more generally

```
   b=.#:@i.@(2&^)  NB. 0,.._1+2^y as binary numbers
   t=.b 3          NB. same t as before
```

There is no problem in calculating the number of bits in each of the `2y` lists

```
    (+/"1)t        NB. sum all the lists in t
0 1 1 2 1 2 2 3
```

My next thought was to select those lists whose bit-sum equals `2`:

```
   ((2&=)@+/"1)t   NB. mark lists whose sum is 2
0 0 0 0 0 0 0 1
```

Something wrong! – what I should have thought was

```
   (2&=@(+/"1))t   NB. right this time?
0 0 0 1 0 1 1 0
```

Following a well-established route for selection using `#~`, the above thinking leads to

```
   (#~2&=@(+/"1))t NB. (#~f)y means f(y) # y
0 1 1
1 0 1
1 1 0
```

From there the route to the summit is clear, but before proceeding, what about the wrong thought? Following the advice given earlier, I should, as a conscientious J learner, ask what it was which I conveyed to the hypothetical perfect J speaker/reader when I said `((2&=)@+/"1)t`? In the first place such a reader would have mentally inserted a right parenthesis following the first verb to the left of `@` so that what was inserted in the J sense was the verb `v=.(2&=)@+`. What does this convey? Any verb to the left of `@` must be monadic, in this case *compare-with-2*. The fact that the insert adverb (`/`) follows `v` means that the `+` in `v` must be dyadic, and so `v` is *compare-with-2 addition*. Next since the inserted verb is defined as operating on lists (`"1`), the insertions are between individual items. Given that each list is a bit-list, the only possible results of adding neighbours are `0`, `1` and `2`. The last occurs only when both inputs are `1`, or equivalently compare with `2` is true, and so in the present context `v` is equivalent to `*` with bits, that is `*.` (logical *and*). Here is confirmation:

```
   v/"1 t
0 0 0 0 0 0 0 1
```

As an aside (and asides can be very valuable in language learning) suppose now that a second thought was to change `v=.2&=@+` to `w=.2&(=@+)`. Only monadic verbs may appear to the left of `@`, and so `=` must now be self-classify, which returns the value `1` for all scalars. The next question is what does `2&` mean? The definition of *bond* with a noun is

<pre>
   x m&amp;v y <span class="plain">⇔</span> m&amp;v^:x y
</pre>

so `x 2&(=@+)y` means `2&(=@+)^:x(y)` in the present case. In words, the left argument is a repeat factor, so in the case of bits the verb `=@+` is repeated either `0` or `1` times. If it is `0` the result is simply the right argument, if it is `1`, the result is `1` by virtue of self-classify. The `2` in the bond is irrelevant, and could have been any number, real or complex. Put all this together and dyadic `w` is equivalent to *or*:

```
   0 w 1
1
```

Or is it? There is a snag, namely that the rank of the result of self-classify is a table comparing an object with its nub, and so has rank at least `2`, so unlike `v/"1 t`, `w/"1 t` is not a list of simple binary scalars:

```
     (0 w 0),(0 w 1),(1 w 0),(1 w 1)
0
1
1
1
  $w/"1 t
8 1 1 1 1
```

The `8` above is easy to explain, but why four `1`s when each item of `t` is a 3-list? The answer is that each insertion after the first adds 1 to the rank following the self-classify rule. This is confirmed by

```
   $w/"1 b 4
16 1 1 1 1 1 1
```

To return to the correct route, or call it perhaps the clear-thinking path, the experience of the exploration of wrong avenues suggests separating out the list summation process

```
   s=.= +/"1       NB. is x equal to the list sums of y
   3 s i.2 3       NB. sum two lists, return 1 if ‘=3’
1 0
```

The right argument in the context of combinations is `b y` and so the indicator list which identifies the relevant binary lists is the hook `(s b)y`.

Hence

```
d=.(s b) # b@]     NB. items of y which sum to x
```

delivers the bit strings which corresponds to the required combinations:

```
   2 d 3
0 1 1
1 0 1
1 1 0
```

and a final tally using bits selects the relevant lists of indices

```
   c=.d # i.@]     NB. convert binary to indices
   2 c 3
1 2
0 2
0 1
```

Here is the final (and correct!) stream of thought which led to this approach to the problem :

```
   b=.#:@i.@(2&^)  NB. i.2^y as binary numbers
   s=.= +/"1       NB. x is equal to list sums of y
   d=.(s b) # b@]  NB. items of y which sum to x
   c=.d # i.@]     NB. convert binary to indices
```

But to return to the beginning, the primary object of this article was not the final algorithm itself, but rather reflection on the journey to get there, and on the value of following up wrong paths taken on the way. If you have read this article, you will almost assent to the propositions that APL and J are labour-saving devices for thinkers. But more subtly:

1. are there people for whom both APL and J allow the expression of thoughts which they were previously able to think but not express?; and
2. are there people for whom both APL and J allow the thinking of thoughts which they were previously unable to think?
