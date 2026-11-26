---
title: 'Puzzle Corner: Chess Board Pattern'
authors:
- Alan Sykes
volume: '7'
issue: '2'
page: '19'
unindexed: true
transcribed: 'from page images of VOL.9-NO.2-OCTOBER-1990.pdf (misnamed: Vol.7 No.2), page 21 (printed 19; within The Education VECTOR, after its introduction on p.17); Claude, 2026-10-11'
review: draft
tags:
- competitions and puzzles
- education
queries:
- No byline is printed; the puzzle is written in the first person by the editor of The Education VECTOR, Alan Sykes, to whom it is attributed here.
- 'The patterns are printed in an APL font: a small square (transcribed ⎕) and a small circle (transcribed ∘), as in the string ''⎕∘'' named in the text.'
status: transcribed
issue_tab: n2-october-1990
---

*Transcribed from the printed issue; not yet reviewed. [Read it in the PDF of the issue, page 19](../7/2/VOL.9-NO.2-OCTOBER-1990.pdf#page=21)*
{ .transcribed }

## Chess Board Pattern

On the basis that APL can be (and usually is) fun, previous Education Vectors have included a puzzle or challenge section. Although I don’t get inundated with responses, a significant number of readers do send in their thoughts. Indeed, I am still receiving solutions to the original puzzle I included (converting marks into grades).

In this edition I decided to continue the theme of strings and look at the sort of patterns that can be created from a simple vector of two characters, say `⎕` and `∘`. The following 9 by 9 squares can be generated easily by APL expressions which end with the string `'⎕∘'`.

```
⎕⎕⎕∘∘∘⎕⎕⎕  ⎕⎕⎕∘∘∘⎕⎕⎕  ⎕⎕⎕∘∘∘⎕⎕⎕
∘∘∘⎕⎕⎕∘∘∘  ∘∘∘⎕⎕⎕∘∘∘  ∘∘∘⎕⎕⎕⎕⎕⎕
⎕⎕⎕∘∘∘⎕⎕⎕  ⎕⎕⎕⎕⎕⎕∘∘∘  ∘∘∘⎕⎕⎕∘∘∘
∘∘∘⎕⎕⎕∘∘∘  ⎕⎕⎕∘∘∘⎕⎕⎕  ⎕⎕⎕⎕⎕⎕∘∘∘
⎕⎕⎕∘∘∘⎕⎕⎕  ∘∘∘⎕⎕⎕⎕⎕⎕  ⎕⎕⎕∘∘∘⎕⎕⎕
∘∘∘⎕⎕⎕∘∘∘  ∘∘∘⎕⎕⎕∘∘∘  ⎕⎕⎕∘∘∘⎕⎕⎕
⎕⎕⎕∘∘∘⎕⎕⎕  ⎕⎕⎕∘∘∘⎕⎕⎕  ∘∘∘⎕⎕⎕⎕⎕⎕
∘∘∘⎕⎕⎕∘∘∘  ⎕⎕⎕∘∘∘⎕⎕⎕  ∘∘∘⎕⎕⎕∘∘∘
⎕⎕⎕∘∘∘⎕⎕⎕  ∘∘∘⎕⎕⎕∘∘∘  ⎕⎕⎕⎕⎕⎕∘∘∘


⎕⎕⎕∘∘∘⎕⎕⎕  ⎕⎕⎕∘∘∘⎕⎕⎕
∘∘∘⎕⎕⎕⎕⎕∘  ⎕⎕⎕∘∘∘⎕⎕⎕
∘∘⎕⎕⎕∘∘∘⎕  ⎕⎕⎕∘∘∘⎕⎕⎕
⎕⎕⎕⎕∘∘∘⎕⎕  ∘∘∘⎕⎕⎕∘∘∘
⎕∘∘∘⎕⎕⎕⎕⎕  ∘∘∘⎕⎕⎕∘∘∘
∘∘∘⎕⎕⎕∘∘∘  ∘∘∘⎕⎕⎕∘∘∘
⎕⎕⎕⎕⎕∘∘∘⎕  ⎕⎕⎕∘∘∘⎕⎕⎕
⎕⎕∘∘∘⎕⎕⎕⎕  ⎕⎕⎕∘∘∘⎕⎕⎕
⎕∘∘∘⎕⎕⎕∘∘  ⎕⎕⎕∘∘∘⎕⎕⎕
```

This edition’s puzzle corner is to identify simple APL expressions that will produce some of these patterns; in particular the last pattern which is basically a chess-board. Having produced this, then production of a full size chess board should be easy. Please send your solution(s) or patterns to the editorial address.
