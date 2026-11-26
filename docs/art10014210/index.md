---
vid: '10014210'
title: News from Sustaining Members
authors:
- Causeway Graphical Systems Ltd
volume: '21'
issue: '2'
page: '10'
description: VECTOR news article
source: trad/v212/sm212cgs.htm
converter: vecrescue 0.1.0
issue_tab: n2-february-2005
tags:
- implementations
- APL community
---

## Causeway Graphical Systems

Turning *RainPro* into *SharpPlot* has taken almost exactly 18 months, during which time we have had very little time or energy to do anything else. By choosing to keep development in <acronym>APL</acronym>, but ship as fully-managed (compiled) C# we took a considerable risk – as far as I know there have been no commercially successful compiler projects since the early days of STSC back in 1979. However, all’s well that ends well and [*www.sharpplot.com*](http://www.sharpplot.com) is out there for all to see. We now have a graphics engine that is fast, lightweight and fits all the requirements of a top-quality .Net component. Great for Causeway, but why should the <acronym>APL</acronym> world care?

It occurs to me that we may not be alone in wanting to ship <acronym>APL</acronym>-authored components to .Net customers! I also remember the time when <acronym>APL</acronym>ers cared enough about speed to buy the STSC compiler (not cheap in those days) just to get a few bottleneck functions coded down into assembler. Maybe there is someone else out there who could use this stuff? What could we offer to the <acronym>APL</acronym> world?

- a lightweight library of <acronym>APL</acronym> primitives, hand-coded in C# and callable from any .Net language (such as C# or VB.Net). These implement the <acronym title="International Standards Organisation">ISO</acronym>-standard for first-generation <acronym>APL</acronym> and as much ‘nested’ stuff as we have needed so far. This is well-tested, well-documented and very reliable. Guide-price £1,000 per developer with site-wide deployment £5,000. Royalty-free runtimes, as always.
- a bunch of functions (implemented in both APL2000 and Dyalog APL) which take your <acronym>APL</acronym> source-code and generate one or more C# source-files which can be compiled into a <acronym title="Dynamic Link Library">DLL</acronym> or a stand-alone application. This code is constantly evolving (see any recent backchat on the DFns newgroup), falls over in a huge heap at the slightest provocation, and issues amusing error messages like `FUNCTION ABUSE` at things it doesn’t like. Seriously, it is not all that bad, but no way is it a product we could sell.
- a lot of expertise. Between us, we reckon we could look at an <acronym>APL</acronym> application and within a day, give a reliable estimate on what would need to be done to get it through the compiler hoops. We targeted C#, but there would be no problem in creating source in VB.net, Java, or even <acronym title="American National Standards Institute">ANSI</acronym> C if those were the language standards in your organisation.

Which leaves me thinking that the best way forward is probably a short seminar (say 2 days) which is both a technology preview and a workshop. My aim would be to assemble enough people to give the converter a serious workout, nail the more egregious bugs by teatime on the last day, and hand over a time-limited version to all the participants.

This way, you get to see the tool in action before you try it for yourself. I appreciate that this is not Causeway’s usual approach, but I am really scared of the support cost of simply handing out trial copies. Did anyone watch *Raiders of the Lost Ark* (for the fourteenth time) the other night? Just think of `#.cc` as a very dangerous box, and you will see why I want to make sure you know enough of the right words before you lift the lid!

[Causeway Graphical Systems Ltd](http://www.causeway.co.uk)

***
