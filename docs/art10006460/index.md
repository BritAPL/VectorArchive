---
vid: '10006460'
title: 'Guest Editorial: Namespaces'
authors:
- Duncan Pearson
byline: by Duncan Pearson
volume: '11'
issue: '3'
page: '3'
source: trad/v113/pearson113_3.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- language design
- implementations
---

by Duncan Pearson
{ .byline }

We are standing at an important point in the development of mainstream APL. The interest in namespaces or contexts or whatever we call them, from the commercial users of APL (that is, everyone who pays good money for a professional interpreter) is generating some action from the developers. Dyadic have introduced namespaces in Dyalog v7 and a great deal of interest has been shown in them. Last May James Wheeler promised that a future release of +III would have similar features, not only providing encapsulation of APL code but also integrating the GUI elements fully with the APL programming structure. Finally J, the newest professional development environment, has locales.

Whatever Manugistics produce I hope that it is different from the Dyalog implementation. This is not because I think that the Dyalog way is the wrong way. I have not used it sufficiently to judge. My point is that until a reasonable body of serious developers have spent time building big applications using these features we cannot tell whether the design is right or not. It is clear from the experience of Kimmo Kekäläinen that there is a world of difference between having a pretty demo that shows some code sitting in the button that calls it, and the reality of using namespaces to organise large chunks of utility code across multiple applications.

So let us have as many different, independently conceived, approaches to this problem as there are interpreters. Furthermore, why should we rely on the writers of interpreters to do the design work? Many people will have tried to solve this problem in their own way using local definition and assignment (the +II user command processor being an example). If you think that your approach has merits then write and explain them.

In the long term this is the feature that will decide what we are using in ten years time, and it had better be right. Let us, the users of APL, discuss freely the relative merits of whatever approaches come along, share our experience of using them and let the developers know when they get it right.

(webpage generated: 7 February 2007, 21:27)
