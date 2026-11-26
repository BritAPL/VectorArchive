---
vid: '10501530'
title: 4xTra Alliance industry news
authors:
- Chris Hogan
byline: Chris Hogan
volume: '26'
issue: '4'
page: '22'
received: '2015-05-17'
online: '2016-05-05'
description: 4xTra Alliance industry news
source: ../../php-site/content/printed/264/4xtraalliance/4xtraalliance.htm
converter: vecrescue 0.1.0
issue_tab: n4-march-2016
tags:
- implementations
- APL community
---

Chris Hogan
{ .byline }

Not a lot to say in this issue. The separate members of the alliance continue to pursue their own projects, with BAA London being the main activity we perform in common at the moment.

We've continued to organize the regular monthly meetings on APL and related topics and in May we combined with the BAA Annual General Meeting, which will be reported elsewhere in this issue. The meetings can cover a wide range of topics – besides APL specific presentations, we've had talks on general coding principles, test driven development,using GPUs for parallel processing in K, using Dyalog to program the Dyabot robots. Something for everyone.

There may be changes in future as we've been told that our regular venue, the upper room of the Albion pub on New Bridge Street is being redeveloped later this year and it might not be available for hire after that. We'll keep you posted through the Google group.

That's getting a bit off the topic of sustaining member news, so if any of this sounds interesting then keep an eye on the BAA London Google group:

https://groups.google.com/forum/?hl=en#!forum/baa-london

As I can't really say what the other members of 4xtra are up to, I'll conclude this short piece, I'll say a little about what HMW has been doing recently and as this is Vector, it will be focused on the APL side of things. Besides continuing to work on DFS for Dyalog, which uses Conga for client/server communications, we've been employing conga in a different context for Web Services.

Freely using Dyalog's SAWS as a model (and raiding Miserver for its JSON to (and from) APL classes, these tools provide an APL client to send notifications via a third party server to smart-phones and an APL service to receive requests from the external server. For pragmatic reasons the incoming service calls use SOAP and the outgoing ones JSON. To co-ordinate the multi-threaded environment these services need, we've included an APL thread management sub-system.

This project will form the basis of a talk at BAA London and maybe a future Vector article.
