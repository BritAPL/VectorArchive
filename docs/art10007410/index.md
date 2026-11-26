---
vid: '10007410'
title: 'APL93: APL in Satellite Surveillance'
authors:
- Jack Rudd
byline: by Jack Rudd
volume: '10'
issue: '2'
page: '95'
source: trad/v102/rudd102_95.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- conference reports
- applications
---

by Jack Rudd <span class="affil"></span> <span class="email"></span>
{ .byline }

### Now it can be Told!

Much of what follows has been hinted at over the years in a series of papers (e.g. “An APL to ADA Translator” in 1987), but Jack has never been able to say exactly what he was doing, and why. Now that the “cold war” has relaxed, and the threat from Soviet ICBMs has almost totally vanished, much of what Jack is working on has been de-classified. He was clearly delighted at being able to talk freely about his work with APL in the analysis of satellite observations, and was also willing to offer some strong opinions (see below) on the future.

All in all, I felt that this was one of the most significant papers at the conference, and judging by the attentiveness of the audience, many others would agree with me.

### An Announcement

“Ballistic missile defense is now a possibility:  
     \*     the threat is reducing  
     \*     the $/performance ratio of sensors is improving  
     \*     we now have far greater computational capability  
     \*     we also have 22 years experience of algorithm development”

The experience of defending against SCUD attacks in the Gulf War, when satellite observations were relayed within (some classified piece of time) to ground stations, and the use of PATRIOT batteries was optimised in “near real time” confirms this.

Where is APL in All This?

- data analysis. Lots of satellites have historically seen lots of launches! There is now plenty of scope for verifying algorithms against past data.
- algorithm prototyping. APL is used to benchmark code (via quadNA to FORTRAN), to visualise algorithms, and often (via indirect translation to ADA) to specify the final software. “English is a very inefficient intermediate language, so we give them APL”.
- as of last year - APL is being delivered direct to satellite analysts.

### History

Things started back in 1971 with an IBM/360 (1+2Mb) which offered a 24hr batch turnaround - unacceptable when Soviet submarines were just offshore! APL over 2741 terminals was a huge improvement, even when a bad sunspot year gave so many RESENDs that it was hard to get a clean line of code. In those days, the threat was very real, and Jack’s group consisently met aggressive schedules. Today, it takes forever to get anything done - “you do not want to be a Type-A personality and work with ADA!”.

In 1985, Jack developed a translator from APL to “readable ADA”; the developers said they would use it, but by 1988 the ADA folk had gone OOPS - and their defined types pervaded the ADA environment. All APL wanted was INTs and LONGs and FLOATs - so the translator was not used; instead Jack hand-translated the generated ADA to FORTRAN as you cannot call ADA code with quadNA (ADA was designed to be the only language in the world). The ADA developers eventually re-translated the FORTRAN source into ADA anyway (by hand). However, although the route seems to have been a little perverse, the consequence is that much of the real-time ADA code that is used to defend against SCUD attacks closely embodies original APL algorithms.

### Satellite Constellation Management

The requirement is quickly to get satellites in the right place to view a specific piece of ground. For the first time, raw APL2 code (around 1500 lines to date) is being delivered direct to the analysts. The encouraging thing is that the analysts have already picked up enough APL to be able to operate and modify the code themselves. However:

“Our analysts have no complaint with your program. I want state-of-the-art displays to impress passing generals”

... is the message from the management! Accordingly, Jack has organised an APL2 course in October, with the objective of getting the interface GUIfied.

### A Catalogue of Space Objects

Some 7000 objects are tracked -

“Things that if they hit you, going at 18,000mph, they’re going to disturb your day!”

... but to track them all accurately would require around 1Gflop of processing power. However there is hope, because this is almost a perfect parallel processing application. The process starts with a detailed model of the Earth that includes every gravitational bulge. Lots of fancy numerical integration leads to well parallelised orbit-propagation algorithms which are ideal for the IBM “Power Parallel Tower” processor. The objective is drastically to reduce the need for software intervention when the software tries to guess which observation &lt;--> which object (at an average rate of 50,000 per day).

The “Parallel Tower” is based around the “Execube” processor, which is designed to be a scalable, affordable, massively parallel architecture. In its basic form, it will process 2.5 billion instructions per second in about 1 cubic foot. What is more, it is already “militarized”; it was designed to travel in AWACS aircraft, and could easily be put in a satellite. Many array-oriented primitives are wired into the processor (which was developed by a team with APL experience); the irony is that it requires a translator for ADA!

### Questions

     Q:     Do you need to track geostationary objects?

     A:     Actually, there is no such thing as a geostationary satellite. Anything left to itself will build up an oscillation of around 4 deg in a year, unless you expend energy in keeping it in place.

     Q:     Do you attempt “provability”

     A:     Provability is something we’ve never been able to afford. Only 100 million people are involved if we get it wrong, so we can’t afford the $1,000 per line of code that the Space-Shuttle people use!

     A:     No, I don’t know anything about UFOs!!

### Summary (by Adrian Smith)

Just as C hung around from 1976 until the mid 1980s, waiting for the right conditions to flourish, APL has been there in the wings quietly hoping that eventually a suitable computer might be built. Jack’s talk gives us hope on two counts:

<ol type="”1”">
<li>The hardware has almost caught up! APL was never designed for a von Neumann machine, and soon it will no longer be constrained to run on one. I am not aware of any other high-level languages which are “parallel-ready”; ADA obviously isn’t.</li>
<li>The CASE paradigm (design, verify, code, test, implement) seems at last to be running into the sand. When the pressure is on, the need to get results rapidly overrides the desire to operate “by the book”. Languages which build CASE deeply into their architecture (as ADA does) will die with it. Functional languages (like APL, LISP and PostScript) are best placed to exploit their downfall.</li>
</ol>

To repeat what I said at the start: this was a significant paper in the history of APL.

(webpage generated: 6 December 2005, 18:34)
