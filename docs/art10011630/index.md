---
vid: '10011630'
title: The Ruler's Edge
prefix: In Session
authors:
- Stephen Taylor
byline: Stephen Taylorsjt@5jt.com
volume: '23'
issue: '1'
page: '118'
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
source: trad/v231/insession.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- programming techniques
---

Stephen Taylor  
[sjt@5jt.com](mailto:sjt@5jt.com)
{ .byline }

This article begins the *In Session* columns, in which working programmers share from their session logs fragments that are useful or instructive (or just plain showing-off, like this piece) but don’t warrant an entire article. Please send your contributions to [editor@vector.org.uk](mailto:editor@vector.org.uk). *Ed.*
{ .ednote }

A Zen master famously began each day with the following conversation with himself.

— Master! Master!  
— Yes? Yes?  
— Wake up! Wake up!

It’s morning. Time to work. But are you awake enough? It’s not a silly question. To write well you want to be not just eyes-open awake, but <em>razor</em>-sharp awake. Coffee and wild dance music can do only so much.

To work, you need your duty sleep.

Sadly, our subjective sense of sleepiness is *entirely* unreliable. Hans van Dongen has shown \[[1](#ref1)\] that as sleep deficit accumulates, cognitive impairment rises steadily; but the subjective sense of sleepiness plateaus after an initial rise. We know we’re short of sleep; we feel a touch sleepy; but it seems manageable. And then we find it hard to finish sentences…

Are you ready to *cut code*? Or should you grab a nap—at worst fill in time with less demanding work? Here’s my Morning Rule to decide it. If I can write the Ruler’s Edge in the session, I’m sharp enough to work.

Dyalog installs with the session log configured to an 80Kb limit. I reset this to 8Mb; I need a log longer than my memory. Searching a long log is easier when it’s divided into sections, so I like to rule a line under each day’s work; and sometimes between different tasks during the day.

A good ruler shows units of measure. Character widths are less useful since laser printers and proportional fonts replaced line printers; but they’re still helpful for visually parsing displayed arrays. So I want a function `rule`. Its right argument will give the length of the line it draws; its left, the intervals at which numbers are displayed. Let’s say the left argument defaults to `10`, and that these periods are marked on the line by inverted carets, somewhat like notches on a physical ruler.

We’ll write the numbers above the line. So the gross structure of the function is clear:

```
      rule←{⍺←10 ⋄ ↑(exp1)(exp2)}
```

where `exp1` returns the superscribed numbers, and `exp2` the notched line.

## Cutting the notches

Let’s start with the notched line. This is a pattern `⍺` characters long, reshaped to length `⍵`:

```
      ⍵⍴'¯∨'/⍨ {something} ⍺
```

For an `⍺` of `10`, we need `something` to return `9 1`; in the general case, `(⍺-1)(1)`. So

```
      10 {⍵⍴'¯∨'/⍨(⍺-1),1} 55
¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯
```

does it. But the truly awakened mind sees unity in the two `1`s. They could both be, say, `2` or `3` (if one wanted double or triple notches) but they must be the same number. So we factor out the repetition:

```
      10 {⍵⍴'¯∨'/⍨(,⍨∘(⍺∘-)⍨)1} 55
¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯
```

## Numbering the notches

Now for the numbers.

We want to select from the numbers up to `⍵`, so `exp1` will be something like

```
      {(-⍺)↑¨⍕¨⍺ select ⍳⍵}
```

where `select` selects only the elements of its right argument that are multiples of its left; i.e.

```
      {⍵/⍨0=⍺|⍵}
```

or:

```
      {⍵/⍨~×⍺|⍵}
```

or, avoiding nesting D functions:

```
      /⍨∘~∘×∘(⍺∘|)⍨
```

Putting that together:

```
      10 {⊃,/(-⍺)↑¨⍕¨(/⍨∘~∘×∘(⍺∘|))⍨⍳⍵} 55
                10        20        30        40        50
```

and substituting for `exp1` and `exp2`:

```
      rule←{⍺←10 ⋄ ↑(⊃,/(-⍺)↑¨⍕¨(/⍨∘~∘×∘(⍺∘|))⍨⍳⍵)(⍵⍴'¯∨'/⍨(,⍨∘(⍺∘-)⍨)1)}
      rule 55
        10        20        30        40        50
¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯
      15 rule 55
             15             30             45
¯¯¯¯¯¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯¯¯¯¯¯∨¯¯¯¯¯¯¯¯¯¯
```

OK: we’re awake—time to get to work!

### Reference

1. Hans P.A. Van Dongen, PhD; Greg Maislin, MS, MA; Janet M. Mullington, PhD; David F. Dinges, PhD “The Cumulative Cost of Additional Wakefulness: Dose-Response Effects on Neurobehavioral Functions and Sleep Physiology From Chronic Sleep Restriction and Total Sleep Deprivation” *Sleep* 26.2 pp117-126  <a id="ref1"></a>[http://www.journalsleep.org/citation/sleepdata.asp?citationid=2198](http://www.journalsleep.org/citation/sleepdata.asp?citationid=2198)
