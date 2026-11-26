---
vid: '10013745'
title: News from Causeway Graphical Systems
authors:
- Causeway Graphical Systems Ltd
volume: '22'
issue: '1'
page: '7'
source: trad/v221/cgs221.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- implementations
- APL community
---

The Dyalog APL conference left me with two strongly conflicting emotions: pride in the apparent pervasiveness of much of our APL utility software, and a deep concern about the degree of dependence of so many successful APL-based products on it (and by implication, ourselves). Let’s take a very brief survey of the programme, looking specifically at the business-oriented presentations.

> **Hologram** (from John Miller) is end-to-end CPro for the Gui, with NewLeaf reports and RainPro charting. John and Guy would be positively shocked at the thought of programming the Dyalog Gui directly.
>
> **The PX-Edit Statistical Table Editor** (from Statistics Finland) uses RainPro for graphs. As far as I know, that is the end of it.
>
> **X2.Net – for TakeCare**. A long-term NewLeaf customer, this one.
>
> **APL-based .NET Extensions** (from Pertti Kalliojärvi) – it seems they chose Dyalog because they needed SVG output from a graphics engine, and the only decent one they could find was RainPro! Enough said.
>
> **ARQUE** (from Alan Sykes) is written in CPro, with RainPro graphics and NewLeaf reporting. I don’t think Alan would know a <span class="APLU">'ff' ⎕WC 'Form'</span> if it emerged from Swansea bay one dark night and bit him!
>
> **Assembly and Morning Song** (from Michael Baas) was the kind of little Gui-based utility that experienced CPro users can whip up in a day. Mike has been coding in Causeway from the beginning, and is responsible for most of the ‘cool’ stuff (like clicking on data-points and running callbacks) that Alan Sykes was exploiting so effectively in ARQUE.

Which is a remarkable hit-rate. *RainPro* and *NewLeaf* are no great worry for me. They are both shipped as open (fully commented) code and really don’t do anything very clever. NewLeaf in particular just takes away the mind-numbing tedium of report-formatting. They work fine in Dyalog 11, and are likely to keep on working into the indefinite future. One day soon, I can start encouraging my users to work with SharpPlot (and whatever the .Net build of NewLeaf is called) which are smaller, faster, and use the .Net tools directly to make much better raster output.

*CPro* is another animal entirely – it started back in 1993 (before there were Namespaces) and prospered as a way to superimpose a rational object-oriented model on the original Dyalog Gui. It is a very productive development tool, and I still like it a lot. But I am seriously concerned that Dyalog 11 may (quite reasonably) leave people wanting more. They may want direct access to the .Net Gui, they will probably expect Causeway classes to behave like the native V11 classes, and so on. This may just be my natural paranoia showing through, but I would ask all our users to start thinking sooner rather than later about what comes after CausewayPro. It won’t be a 2-minute job to rebuild it.
