---
vid: '10003580'
title: Ken Iverson at the End of the World
authors:
- Richard Hill
byline: From Richard Hill
volume: '22'
issue: '3'
page: '93'
source: trad/v223/hill.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- APL in perspective
- APL community
---

From Richard Hill
{ .byline }

I am unaware of Iverson visiting Kerguelen, or even Perth, WA, so we can consider his visit to Melbourne, Australia in November 1994 the furthest he ever got from his origin in Alberta.

Melbourne is the capital of Victoria. The State of Victoria is the inverse of the Province of Alberta (regally) which should appeal to a J-ish sense of humor. Iverson came to Australia to run a workshop in J, sponsored by Walter Spunde of the University of Southern Queensland. After the workshop he came down to Melbourne to give a lecture at Melbourne University, sponsored by Moshe Sneidovich. I remember the lecture well. There were around 200 people present and quite a vigorous exchange of views on tacit and functional programming. After the lecture Iverson stayed overnight at our place in Black Rock.

As a fairly simple engineer, I felt on quite a different plane to Ken Iverson and the only specific saying of his that I remember is <q>Engineers are distinguished, not so much by their ignorance, as by their pride in it</q>. I asked him a question, <q>If you compare J and APL, over a range of problems, what can you say about the number of tokens needed on average to program the solution?</q> He said that he thought that J needed about half as many tokens as APL for the same problem.

This leads on to a humble suggestion for a practical memorial to Ken Iverson. A new APL primitive to be called Nub.

My own work is supporting APL programs written by various people, and involves a lot of “data rummaging”. After meeting Iverson, I tried to read the J dictionary and wrote a couple of small things in J, and I came across J’s primitive “Nub”.

Nub is trivially achieved in APL, being ((V\{iota\}V)=\{iota\}\{rho\}V)/V.

But, I wrote a little APL utility called Nub, which also works with arrays, I found it so handy that I realised what Iverson meant by “Notation as a Tool of Thought”. It is really true. Being able to type Nub and not have to think about the construct means that you don’t lose the ‘flow’ of thinking about the problem at hand.

Ken Iverson wrote a couple of papers during the nineties about areas where there could be some unification in the APL area. Nub is mentioned in at least one of them, published in Quote Quad, I believe. It is respectfully suggested that in memory of the originator of APL, one of his later ideas be incorporated as a new primitive ‘Nub’… in all flavours of APL.
