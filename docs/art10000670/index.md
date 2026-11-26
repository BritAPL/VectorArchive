---
vid: '10000670'
title: 'Review: SHARP APL Reference Manual'
authors:
- Jonathan Barman
byline: by Jonathan Barman
volume: '10'
issue: '1'
page: '53'
source: trad/v101/barman101_53.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- reviews
- implementations
- language design
---

*by Jonathan Barman*
{ .byline }

### Sharp APL Reference Manual

*Published by Iverson Software Inc*  
*reviewed by Jonathan Barman*

I thoroughly enjoyed reading this manual. In fact I read it all, from cover to cover, on the train to work over a period of about a week. The description of each APL feature is a delight. There are plenty of diagrams to guide you through the more arcane aspects of APL, and each feature is carefully and clearly explained.

It is many years since I have used Sharp APL, so I was particularly interested in all the new features that have been added to the language. My main experience with nested arrays has been in APL2 and APL\*PLUS, and I have only read the papers presented at conferences about the rather different approach to nested arrays taken by Sharp APL. Having all the details put together in the reference manual enabled me to see the overall effect of the changes.

There are 13 sections, starting with an overview of APL and ending with details of messages and interrupt event numbers. The majority of the text is taken up with the description of APL primitive functions and operators. The pages are numbered within each section, and there is no overall page numbering; which I dislike. However, the really heinous crime is, in my view, the lack of an index. A true reference manual has to have an index to enable you to refer to the topic of your choice. In order to find a topic you have to scan the Contents, which is a full listing of all subheadings going down 4 levels. If you already know APL this is quite easy, but I am not sure how the tyro APLer would manage.

The manual is not a beginners’ text. The Overview starts with a small example of an APL session which is used to define the terms nouns, pronouns, verbs, adverbs, etc. I enjoyed the terse and exact descriptions of the terms, but would not want to start an APL course that way.

The second section explains the APL Grammar in considerable detail – in so much detail that I felt it became a little long winded. However, the paragraphs on precedence, which explain how adverbs and conjunctions (operators) are evaluated before verbs (functions), are excellent. If I had read this description years ago it would have saved quite a bit of aggravation and unnecessary experimentation. Bracket indexing is dismissed as an anomaly in the grammar, and J is cited as the new dialect of APL where new, more convenient, mechanisms are provided to replace the bad old ways!

The next two sections cover Nouns and Pronouns and the naming of Nouns and Verbs. The scene is set for the Rank Conjunction with a detailed explanation of how arrays may be partitioned into cells. The sections also include details of the characters in `⎕av`, how nouns are displayed, the interface nouns which are `⎕` and `⍞` input, localisation of names, and indexed assignment. A bit of a hotchpotch, but all these topics have to go somewhere and the grouping is not unreasonable. This is exactly the situation where an index would be invaluable. For example, the detailed explanation of `⎕av` in Section 11 makes no mention that the character set is listed in Section 3.

Section 5 is the longest and details the verbs with outstanding clarity. Dyadic transpose (`⍉`) is always difficult to explain, and I was very pleased to see that two highly visual methods were used, one suitable for three dimensional arrays using pictures of bricks, and a more general method depicting the switching of axes which is easier to visualise when dealing with arrays of 4 or more dimensions. It was also good to see the relationship between ô and the left argument to `⍉` explained. There were many examples of other verbs which gave me a new insight on the workings of APL. I had not fully appreciated the subtleties of the collating sequence given in the left argument to `ô`. Also, as I have never used complex numbers in an application, I found the exposition of the way in which each primitive has been applied to complex numbers quite fascinating.

Adverbs and Conjunctions are covered in Section 6. The examples in Section 5 make liberal use of the Rank Conjunction so that on reading through the manual the rather technical explanation seems easy and obvious. Not having used `⍤` and `ã` before, I felt that the manual has given me an excellent grounding in how to use them properly. When I need to use ISI APL for real I will review this section before I do anything else.

The next sections cover editing verbs, branching, and event handling using `⎕trap`; all covered equally well.

After all this excellent material the manual seems to peter out. The section on APL Component files is a mere 4 pages, and the description of the System Nouns, Verbs and Commands is equally brief. Packages get mentioned, but there are few examples.

The manual is published by Iverson Software Inc. and has Iverson APL Reference Manual on the front cover and Sharp APL Reference Manual on the top of each page. The ISI sales literature states that this is a reprint of the second edition of the Sharp APL Reference Manual, and that by reducing the font size and removing material not relevant to the PC they were able to print it in a single volume with their standard wire binding. The result is a very handy reference booklet. It is printed on A5 paper and the wire binding enables it to lie flat when opened.

There are 362 pages, which is just about the maximum that a wire binding can cope with easily. The font size is very small. The print in my review copy was slightly smudged so that `⍟` was just a blob and in some cases it was only possible to distinguish `+` from `÷` by the context. This is a relatively minor quibble as normal text was quite clear enough to read easily.

I would recommend this manual to anyone who is interested in APL, whether or not they intend to use Sharp APL or any of the products from ISI. Even someone dedicated to the other brands of nested arrays would find the descriptions of the primitives that share a common definition invaluable.

(webpage generated: 5 December 2005, 18:10)
