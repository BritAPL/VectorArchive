---
vid: '10005370'
title: 'At Play with J: MIMD Machines'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '10'
issue: '2'
page: '128'
source: trad/v102/mcdonnell102_128.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- language design
- programming techniques
---

by Eugene McDonnell <span class="affil"></span> <span class="email"></span>
{ .byline }

I had a request recently from someone who wanted to apply a verb a different number of times to a list of arguments. What was wanted was a simpler way of writing:

```
     (f a),(f f b),(f f f c)
```

My initial response was to say that J did not as yet have a way of describing Multiple Instruction-Multiple Data machine architectures (MIMD), although such a mechanism had been described \[Be91\]. I pointed out that a collapsing transpose could solve the problem, but my questioner would have none of that, as it implied a great deal of useless computation. There the matter rested for a while. After several months I had another request from the same person who wanted to know if I had made any progress on the problem. Actually, I hadn’t thought about it at all in the interim, but since my questioner seemed to be a determined type, I gave it a few minutes more thought, and found what I think is a neat use of one of J’s more interesting differences from APL, the way scan is defined: that is, the verb applied is monadic, not dyadic.

For example, whereas in APL one writes `+\1 2 3` to obtain the continued sum of the values in the argument, in J one would have to write `+/\1 2 3` to obtain the same result.

```
     +\1 2 3
1 0 0
1 2 0
1 2 3
```

Here the monadic verb `conjugate`, denoted by `+`, is being applied, first to `1`, next to `1 2`, and last to `1 2 3`; since these are real numbers, their conjugates are the same as the arguments, and since J reshapes results so that they conform, and then appends them, we get the zero fills at the right of the top two rows. Compare this with

```
     +/\1 2 3
1 3 6
```

which is the analog to APL’s `+\1 2 3`.

Finally, here is the solution to the MIMD problem.

First I define three variables, `a`, `b`, and `c`:

```
     'abc'=.3 4 5
```

Next, I define a verb `f` to be the natural logarithm (`^.`).

```
      f=.^.
```

and apply it once, twice, and thrice, to `a`, `b`, and `c`, respectively:

```
     f a
1.09861
     f f b
0.326634
     f f f c
_0.742579
```

This is the desired result, but done the hard way. Now for the easy way:

Define a verb `g` which in which the verb `f` is applied (`@`) to the tail (`{:`) of its argument a number of times (`^:`) equal to the length (`#`) of its argument:

```
     g=.f@{:^:#
```

For example, `g 3 1 4 1 5 9` applies `f` six times to `9`:

```
     f f f f f f 9
0.854804j1.01575
     g 3 1 4 1 5 9
0.854804j1.01575
```

Perhaps you already see how this will end. We apply the prefix scan (`\`) adverb to `g`, and apply this derived verb to `a,b,c`:

```
     (g\)a,b,c  NB. apply g to successively longer prefixes
1.09861 0.326634 _0.742579
     NB. q.e.f.
```

Showing once again that where there’s a will there’s a way. Note that because of the way prefix scan is defined, it is easy to visualize how, in a multiprocessor environment, the applications of `g` to all three arguments can be carried out simultaneously.

     \[Be91\]     Bernecky, Robert, and Roger Hui, ‘Gerunds and Representations’, APL Quote Quad 21, 4, August 1991, Stanford, California 1991, pp 39-45

(webpage generated: 6 December 2005, 23:37)
