---
vid: '10011420'
title: 'Editorial: a lot happening'
prefix: Editorial
authors:
- Stephen Taylor
byline: Stephen Tayloreditor@vector.org.uk
volume: '23'
issue: '1'
page: '2'
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
source: trad/v231/ed231.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- APL community
---

Stephen Taylor  
[editor@vector.org.uk](mailto:editor@vector.org.uk)
{ .byline }

A lot to report on in this issue. That’s partly an artefact of how far we again dropped behind our publication schedule, so let’s start by looking at that.

*Vector*’s typesetting is unusually demanding, and requires some understanding of the code and the mathematics we publish, of the more advanced features of Microsoft Word (in which we assemble the camera-ready copy) and also of the markup and encoding schemes used on Web pages.

For many years production of *Vector* would have been beyond the means of the BAA were it not for the skills of Adrian Smith at Causeway Ltd. In the course of this work he has designed and produced the [APL385 Unicode](resource/apl385.ttf "download") font used both for *Vector* and the [APL Wiki](http://aplwiki.aplteam.com). (See [Kai’s article](wiki.htm).) But even minor interruptions to Adrian’s availability can disrupt regular production of *Vector*.

Happily, the spread of Unicode browsers enables much simpler typesetting solutions. So we’ve begun a project to simplify the markup and composition work. We have for some time preferred to receive articles as text files or HTML documents. As part of this project we’ll produce a simple guide for authors to marking up articles for *Vector*, either as HTML or as XML, using just a text editor. (We know this won’t work for everything we publish, so we’ll continue to negotiate format and content with authors as necessary.) This will reduce the work required to prepare an article for publication, and opens the way to publishing – eventually – articles promptly online before we collect them in a printed volume.

BAA is following Dyalog’s lead to Lulu.com’s print-on-demand service. We plan to use this to make printed or PDF back issues of *Vector* available on demand, and to publish a series of Vector Books, starting this autumn.

But there is also much to report. Kx Systems goes from strength to strength. Its first residential user conference is beginning in Ireland as we go to press. Much welcome news from Dyalog Ltd, which has moved into new offices, expanded to 13 staff, and has acquired Causeway’s admired line of software tools. Morten Kromberg has also been spotted playing with an experimental Dyalog interpreter with native Unicode character strings, and demonstrating that while the five thousand Chinese characters in the Unicode set might look alike under Western eyes, their shape and their nub’s shape are in fact the same.

Kai Jäger has, with help from Chris Burke at Jsoftware, established the APL Wiki, modelled after the successful J Wiki. It’s hard to overstate the potential importance of this. The difficulty of reading and writing the traditional APLs on ordinary PCs and on the Web has been widely blamed for their fall from favour.

Now, and for the first time, APL programmers have the prospect of sharing code over Web pages, cutting, pasting and typing their code. (If you are using a Unicode-enabled APL like APLX or Dyalog, you can also cut and paste between your session and the APL Wiki.) Hats off to Kai for envisaging this, and for contributing the time to make it all function. Making it useful is a job for the rest of us!

Dyalog has announced its user conference will be held in the USA this year, in Princeton, NJ from 30 September to 3 October. SIG-APL has stirred itself into life and announced APL2007 as part of OOPSLA 2007 in Montréal, Canada the following month. We note that OOPSLA is also home to the Dynamic Languages Symposium and expect that APLers will be there showing off their moves with Domain-Specific Languages.

More crossover action from the Kx Systems user conference just finished in Ennis, Ireland. Arthur Whitney’s Q programming language can be thought of as an APL savagely hacked for raw speed. In the course of developing it, Whitney excludes anything and everything that might compromise that goal. Once a Q program has worked its magic with billion-row tables, it leaves its author reaching for richer tools for presenting the results. Morten Kromberg of Dyalog showed how a full-featured APL could take over the running without losing the productivity of Q’s array paradigm. Expect to see more exploration of the APL-Q partnership.

I’m sorry to report that Eugene McDonnell plans to send us no more “At Play With J” articles. We’ve had 38 of these over the years; Eugene tells us each one took about a month to write. It’s the end of an era. To mark our appreciation of this work, the BAA will collect and publish it in book form this year.

This issue contains a substantial introduction to APL by Bernard Legrand, translated from the French, a labour of love by Sylvia Camacho. Richard Nabavi discusses the design decisions behind the 64-bit version of APLX.

Much practical help in APL+Win here from Ajay Askoolum. Ian Clark reviews Ajay’s new book, and we add two articles by Ajay, on writing C# COM DLLs, and on zipping and unzipping files in APL. Klaus Christiansen offers his APL wrapper for the widely-used Developers Image Library, and Ray Cannon, master of crash recovery, writes about handling and analysing <span class="smallcap">CONTINUE</span> workspaces. Adrian Smith offers his own take on classes in Dyalog 11.

This issue kicks off a new column *In Session*, successor to the long-running *Hackers’ Corner*. We’re looking for fragments from your session logs when you’ve done something elegant, impressive, useful or just curious – perhaps not worth working up into an article, but nonetheless worth sharing. Show us your moves!

As always, we have articles by people using the APLs to get results in their own fields. Sylvia Camacho adds another article to our series on the Monty Hall problem. Timothy Zirkel plays the Game of Life in 3-D, and Ralph Selfridge considers different ways to write the Black-Scholes algorithm. And Ian Clark describes his agile and innovative approach to digitalising the *Vector* archive.
