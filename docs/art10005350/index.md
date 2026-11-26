---
vid: '10005350'
title: 'Report: Meeting of NYC JUG, June 2005'
authors:
- Devon McCormick
byline: notes by Devon McCormick
volume: '22'
issue: '1'
page: '20'
source: trad/v221/nyjug221.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- conference reports
- APL community
---

notes by Devon McCormick
{ .byline }

## Various Contributions

Dan raised an idea not related to our project but interesting in its own right: how can we define J primitives in terms of other J primitives? This might have some pedagogical merit if we can separate the most basic primitives (first learned) from the more advanced ones. We could then define the more advanced ones in terms a novice may already understand. However, Dan seems more interested in this than is the rest of the group. We probably are more interested in developing and publicizing functions that accomplish small, commonly needed tasks.

Jim’s work on an indexer is relevant to this latter notion. As we’ve noted repeatedly, J’s richness sometimes makes it difficult to find what we need for a task at hand. John reiterated how deeply difficult it is to automatically create a good index – he bases this on his own extensive experience working with bibliographic data and academic papers.

Jim’s preliminary work on an APL/J Rosetta Stone may provide a leg-up for the small community of APL programmers learning J. This points up how useful it is to be able to relate the vocabulary of one language to another. While some aspect of J, such as array indexing, may differ deeply from how this is done in APL, it’s a good start to be able to look up the common term.

This again leads us to a “Rosetta Stone” notion of synonyms: simply listing related or equivalent terms could be useful, especially if the synonyms tie into a widely-used language like Basic.

An alternative to either the automatic indexing scheme or the Rosetta Stone is the notion of logical classification. However, this patently requires the most manual labour so is an approach we’d like to put off. Also, one person’s logical classification will not necessarily agree with another’s. For an unusual and widely discussed notion of classification, see the last section here titled “Classification”.

John talked a little more about his method for combining the best of both GUI and command-line interfaces using his “inert” adverb. This takes an argument of a command and either runs it or saves the command for future use, e.g. in a script. He called this the J analogue of the “command pattern”.

John provided an example of using the “inert” adverb – see “[Copy.ijs](copy.ijs)”.

## The “Save Work” Project

My “action item” for the this meeting was to come up with a “Vicki” use-case. “Vicki” is our hypothetical, primary target user. See the text file <tt><a href="VickiUseCases.txt">VickiUseCases.txt</a></tt>.

Looking at these cases, we debated whether we would need to examine the entire hard disk each time to find files to back up. Dan came up with a list of likely locations where we’d probably find all files modified by a naive (Windows) user. Our preliminary list looks like this:

- Windows executables history
- Run box history
- Application registry history
- Recent Documents
- Application data directories
- Desktop shortcut directories
- Desktop
- My Documents

Does the registry track all most-recently-used files? We have to figure this out and if we can depend on this to be complete.

We’d like to accommodate other platforms besides Windows, so this sort of thing may or may not have analogues in the Mac and Linux worlds. We also came up with the idea of having interval-based classification of backups into something like daily, weekly, and infrequently. The idea is to have pre-set menu options for the most common use cases.

The converse problem to backing up is the potentially much more difficult problem of locating backed-up information. This may be ameliorated by imposing a naming convention on backups or by maintaining a database of items backed up.

An additional consideration is how to handle multiple target devices to store backups. I believe John suggested the possibility of somehow aliasing different devices.

## Miscellaneous

We also discussed a few off-topic notions. One was the idea of tagging bitmaps of hand-written signatures with unique, identifying information. This was generally dismissed as not too useful. One somewhat interesting issue this raised was how to write text programmatically onto an existing image. See “[sigTag.ijs](sigTag.ijs)” (which uses the file [“chars16x16\_32x22.jpg](chars16x16_32x22_3.jpg)” to crudely accomplish writing image text) and “[Example of Using SigTag.htm](Example%20of%20Using%20SigTag.htm)”.

Another thing I brought up, not as off-topic because it relates to learning about math and computers in general, was inspired by the cover of Howard Peelle’s new J book where he lists functions with old, familiar names like “pythagoras” and “pascal”.

His [Pascal’s triangle function](pascal.htm) led me to revisit some interesting topics that this touches on, especially the ideas of colouring different modulus bases of the triangle and degradation of precision for large values. See “Fun with Pascal’s Triangle.doc” for a brief overview of these issues.

## Classification

In *The Analytical Language of John Wilkins*, Borges describes ‘a certain Chinese Encyclopedia,’ the Celestial Emporium of Benevolent Knowledge, in which it is written that animals are divided into:

1. those that belong to the Emperor,
2. embalmed ones,
3. those that are trained,
4. suckling pigs,
5. mermaids,
6. fabulous ones,
7. stray dogs,
8. those included in the present classification,
9. those that tremble as if they were mad,
10. innumerable ones,
11. those drawn with a very fine camelhair brush,
12. others,
13. those that have just broken a flower vase,
14. those that from a long way off look like flies.
