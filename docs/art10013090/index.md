---
vid: '10013090'
title: 'Review: "Les APLs Etendus" by Bernard Legrand'
authors:
- Adrian Smith
byline: by Adrian Smith
volume: '11'
issue: '2'
page: '38'
source: trad/v112/smith112_38.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1994
tags:
- reviews
- education
---

by Adrian Smith
{ .byline }

Masson, Paris, 1994 ISBN: 2-225-84579-4  
reviewed by Adrian Smith

A Textbook on Extended APLs  
APL2, APL\*PLUS, Dyalog APL under Windows

First some statistics: this is a substantial and comprehensive textbook of APL comprising over 500 pages in 17 chapters; it covers all aspects of the APL language, and makes clear the differences in dialect between APL2 and the other two implementations, e.g. the ‘Deux Philosophies’ (page G21) towards expressions like `A B C[2]` where the ‘Force de liaison’ differs in APL2.

All the chapters close with a set of exercises. These are nicely chosen to make you think, avoiding excessive repetition or trick questions which trip up the unwary reader and are liable to put him off APL altogether. It is nicely presented, with good indexing and a helpful set of summary tables showing (for example) all the possible reductions applied to the empty vector.

### Coverage

Chapter A covers a little history, and introduces the various APL keyboards. Chapter B takes us through variables and assignment, simple arrays of various ranks, and vector notation. It makes clear the differences between mixed and general arrays, and also notes the index primitive  offered by APL2 and the extension to bracket indexing `MAT[(1 2)(3 4)]` offered by Dyalog APL.

Chapters C and D are a straightforward introduction to the ‘core’ APL primitives (with a good discussion of reduction and behaviour on empty arrays), and the basics of function definition. Again, the different rules for forming the function header (such as the need to include \{\} around optional arguments in Dyalog) are well explained. Chapter E continues with a discussion of errors and the State Indicator (although I must take issue with Bernard’s definition of `NONCE ERROR` as something specifically related to a failed interface call), and has quite a comprehensive comparison of the various tracing tools available in the different environments.

Chapters F and G are all about restructuring and transforming arrays. In general, Chapter F is ‘classical’ APL, with the addition of selective specification and the use (in APL2) of an axis modifier for `↑` and `↓`. The discussion of dyadic transpose is particularly thorough and supported with several helpful diagrams. Chapter G walks us through the use of the DISPLAY utility, and explains very clearly the meaning of the various indications it uses to hint at the type and rank of your data. It is also used to illustrate exceptionally clearly the different effects of catenation and juxtaposition on the structure of the resulting vector. This is (in my experience) one of the biggest problems for a ‘flat’ APLer to overcome in moving to an extended system.

Chapter H covers execute and (in some detail) format. Here the various APLs diverge dramatically, so the APL2 ‘format by example’ is given a quite separate treatment to the use of `⎕FMT`. Both are again well illustrated with examples. Chapter I discusses operators, with some nice examples of inner products, and Chapter J sweeps up all the remaining math functions, with examples of `⌹` in curve fitting.

We now move on to the environment, with the various `⎕fns` (such as `⎕RL` and `⎕PW`) outlined in Chapter K and the basics of shared variables (including DDE from Dyalog APL) in Chapter L. Chapter M is a good basic introduction to file processing in all three APLs and Chapter N introduces the event-handling facilities. Unfortunately these differ so completely in the three environments that a sensible treatment is more or less impossible, and I doubt whether the coverage is sufficient to enable the reader to program these with total confidence and safety. Finally, Chapter O covers editing, from the ever-present `∇` to the windowed facilities available with Dyalog.

Chapter P is a nice gentle introduction to the Dyalog approach to Windows development (unfortunately the book pre-dates the APL\*PLUS III release) with some good examples and lots of screen-shots. Chapter Q has the obvious (but it needs saying) advice on documentation and housekeeping (“Assurez l’hygiène de votre environnement”) and the book closes with some thoughts on ‘Why APL Today’. Bernard takes many of the accusations levelled at APL and hits back at (for example) unreadability with some well-chosen counter-examples from Excel.

### Summary

This is the first really good book I have seen which takes an integrated approach to the extended APLs, rather than grafting general arrays on to the back of an existing text. It deserves to be the Gilman & Rose of the 1990s. Someone should translate it, but please let that someone be an APLer with a smattering of French, not a professional translator who knows no APL!

(webpage generated: 14 October 2007, 19:17)
