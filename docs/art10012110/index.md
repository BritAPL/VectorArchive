---
vid: '10012110'
title: APL "RISC Programming Style"
authors:
- Gérard Langlet
volume: '6'
issue: '2'
page: '999'
source: trad/v062/langlet62_23.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1989
tags:
- programming techniques
- development practice
- performance
---

17:02 using: VARCH v1.0 1 VARCH44 langlet62\_23 -->

## APL “RISC Programming Style”

by Gérard Langlet

In general, APL functions listed as examples in books or tutorials can be improved in many ways. Let us take as an example the nice COUNTALL function presented by Alan Sykes in Vector Vol.5 No.3 page 31. This well known algorithm returns the frequency table of vector X with unique values in ascending order within the first column.

```
      ∇R←COUNTALL X;I;N
[1]   X←X[⍋X] ⋄ R←1 2 ⍴(1↑X),⍴X
[2]   →(X[1]=X[⍴X])/0
[3]   I←(1⌽X)≠X ⋄ N←I/⍳⍴I
[4]   R←(I/X),[1.5]N-0,¯1↓N
      ∇
```

It works perfectly except when X is not a vector (RANK ERROR), `⎕IO` is set to 0 (INDEX ERROR), or X is an empty vector (INDEX ERROR). Let us now introduce the following version.

```
      ∇R←COUNTALLV X
[1]   X←,X ⋄ R←¯1+1↑X←X[⍋X] ⋄ R←X≠1⌽X←R,X ⋄ X←1↓R/X ⋄
     R←X,[⎕IO+.5]1↓R-¯1⌽R←R/⍳⍴R
      ∇
```

```
      ⎕NSIZE'COUNTALL'
272     (Size of object in APL*PLUS/PC)
      ⎕NSIZE'COUNTALLV'
192     (i.e. 40% less)
```

COUNTALLV runs correctly for any numeric array in either origin. Both functions still produce DOMAIN ERROR when X contains characters; people would probably prefer to take another algorithm for counting words in a text anyway. Benchmarks have shown that both functions consume the same CPU time in various situations, e.g. about 2 seconds on an 8MHz PC-AT for `100=⍴X`.

Why use goto, locals and, most of all parentheses when their use is absolutely unnecessary, even for didactic purposes?

I have written thousands of functions in APL.68000, Dyalog, APL90, Sharp and APL\*PLUS without labels, branches, parentheses (except for `⎕SS` in APL.68000) - and of course without any diamond in APL2 and I-APL. Moreover, the use of matrices or higher arrays is to be avoided when possible. So many nice algorithms can be developed with vectors! Matrices are nice for output only ... when the specification requires them. One can also almost completely discard execute and also encode, decode, transpose, outer product, `^⌿`, `+⌿`, and catenate on large objects. The use of trigonometric functions and of the domino can be drastically reduced in graphics with a minimum of thinking. The ’each’ operator will also remain a monster as long as APL interpreters or compilers do not really optimise its effects.

The "RISC" style will consist in using:

<ol type="a">
<li>Simple and fast primitives, most of the time on vectors only.</li>
<li>Structured programming so that any function will always be executed from the first to the last line.</li>
<li>Short modules: no unlocked function should exceed one screen in size.</li>
</ol>

I even strongly discourage people from buying or reading books in which branches are a strong topic. In all cases, functions provided in courses should take care of the `⎕IO` effects, and give no errors or yield explicit messages when arguments are empty or have the wrong rank; they should respect the ISO standard, be executable in several implementations without pernicious side effects and continue to work when the workspace is almost full; the use of locals, parentheses or long lines often eats space unnecessarily. People should teach how to build APL applications from small bug-free modules and how to avoid the traps.

P.S. A compromise between size, readability, execution time and portability has to be permanently found; this is true for all programming languages, but very difficult with APL. It was a good reason to discard the parentheses, especially when I discovered that parenthesis-free code was executed 20% faster, on average, by APL.68000. You can read it quicker, you are obliged to split your expressions, you get less WS FULL hazards.

```
(If you
          (have
               (friends
                    (within
                         (the
                              (Lisp community)
                         )
                    )
               )
          )
)
```

It might be a good suggestion there too.

(webpage generated: 1 January 2007, 17:02)
