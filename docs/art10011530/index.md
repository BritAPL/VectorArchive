---
vid: '10011530'
title: Kx Systems User Meeting
prefix: Meeting
authors:
- Stephen Taylor
byline: Stephen Tayloreditor@vector.org.uk
volume: '23'
issue: '1'
page: '25'
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
source: trad/v231/kxconf.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- conference reports
- implementations
---

Stephen Taylor  
[editor@vector.org.uk](mailto:editor@vector.org.uk)
{ .byline }

Soft grey clouds roll in over the low green hills, swollen with evaporation from the Gulf Stream. We’re on the western edge of the continent, the first Europeans to taste this treat from the Atlantic. It sifts down onto us through the warm air, glossing leaves and cars with a thin lacquer, darkening the golf links and stone of this mad hatter’s castle. This is Castle Dromoland in County Clare.

The setting is appropriate for the second residential conference of users of Kx Systems’ insanely fast, unthinkably compact, Q programming language. Most of what is reported here is hilariously different from the ever-more-complex abstractions of mainstream computing. But Q is what the big beasts of the financial markets use to get results when conventional products and practices fail them.

Three conference presentations focused on how to exploit an investment in kdb+. Brian Fitzpatrick & Felix Lungu described the Q-based tools, modules and applications that First Derivatives has built around kdb+. Veteran K author Charles Skelton spoke of the design issues critical to successful deployment of software using kdb+. And Morten Kromberg of Dyalog demonstrated how, once Q has worked its magic with billion-row tables, richly-featured Dyalog APL provides access to GUI, graphics, classes and .Net assemblies without sacrificing any of Q’s array-language productivity.

Niall Dalton, Kx’s new Chief Solutions Architect, addressed the challenges of getting from today’s powerful 64-bit PCs anything like the performance of which they are theoretically capable, and why Q is such a good environment in which to tackle that challenge. Performance was also the focus of another speaker, neither whose name nor affiliation can be reported here, who spoke from his experience of the huge difference that disk configurations make to actual application performance.

Response speed was the focus of the banquet speaker, Wendy Morgan, from the London Stock Exchange, who talked of the work the exchange has done to strip latency out of its market data feeds. Private trader and Q programmer Mark Sykes explained how the speed at which an algorithmic trading translates into ‘slippage’ – the difference between the price reported on a market feed and the price at which a subsequent order actually gets filled. He showed how the same model that trades profitably with a fast response time will lose money if it runs even slightly more slowly. The differences are measured in milliseconds.

Charles Skelton had addressed in his presentation matters of ‘style’ in writing Q programs, and he offered a set of prescriptions and proscriptions, observing that whether we agreed or disagreed with them individually, writing style has important consquences for the cost of optimising and maintaining programs.

A consequence of Q’s success is strong demand for programmers. Jeffry Borror addressed the question of “manufacturing” expertise in Q and kdb+, looking at who is likely to succeed with the technology, and how best to introduce them to it. Like Charles Skelton, he emphasised the importance of writing practices and made specific recommendations.

Something is stirring in the Q and kdb+ world. For the first time, conference speakers seem generally willing to have their presentations published. Expect to see much material from this conference in *Vector* 23.3.
