---
vid: '10009890'
title: 'Editorial: The Wind from Denmark'
authors:
- Stephen Taylor
volume: '22'
issue: '1'
page: '2'
source: trad/v221/ed221.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- APL community
- language design
---

Insight Systems ApS marked their acquisition this year of Dyalog APL by hosting the Dyalog APL User Meeting in Elsinore. The autumn wind from Scandinavia blows cold and clear; this issue of *Vector* is dominated by [reports](vik221.htm) and papers from Hamlet’s home town.

Perhaps the biggest news for Dyalog users is the arrival of native support for classes and instances, the foundation stones of object-oriented programming. Readers new to the <acronym>APL</acronym>s, possibly unaware there *are* useful alternatives to <acronym>OO</acronym> programming, may find it striking to see so fundamental an <acronym>OO</acronym> construct added to <acronym>APL</acronym> at such a late date. (They should take this as an indication of just how much can be done without it.)

Object-oriented programming (<acronym>OOP</acronym>) and functional programming (<acronym>FP</acronym>) are not rivals but complements to each other. Nothing beats <acronym>OOP</acronym> for managing complex collections of states, most especially <acronym title="graphical user interface">GUI</acronym>s; nothing is as succinct as <acronym>FP</acronym> for describing data transforms. Most complex software can be viewed as an <acronym>OOP</acronym> layer encapsulating an <acronym>FP</acronym> layer. Morten Kromberg of Insight Systems begins in this issue a series on “[OO for APLers](oops221.htm)”.

One of the drivers towards object orientation has been the access it provides to processes outside <acronym>APL</acronym>. In the Windows world, <acronym>.NET</acronym> is a key interface, and Pertti Kalliojärvi writes about using [APL behind ASP.NET](dino221.htm).

The <acronym>.NET</acronym> interface not only provides a way to expose processes written in <acronym>APL</acronym> for use elsewhere; it provides Richard Smith the components for his own <acronym>APL</acronym> interpreter, Rowan. In this issue, Richard discusses how the use of [delegates](delegates.htm) simplifies construction of the interpreter, and Walter Fil reviews the work of Richard’s father Adrian in exploiting <acronym>.NET</acronym> to write an [APL to C# translator](fil221.htm).

Fresh from a napkin at the conference comes Phil Last’s advice on [infix alternatives to control structures](nap221.htm). Control structures make for greater clarity than the universally deprecated branch arrows; but can still seem laborious compared to the deftness of an infixed

```
(age<65) accept ELSE reject application
```

Deft writing is a theme of my own report on “[Pair Programming With The Users](sjt221.htm)”, in which I describe how traditional <acronym>APL</acronym> development techniques and careful writing have enabled us at Britain’s largest life and pension office to outperform even [Extreme Programmers](http://www.extremeprogramming.org/). My talk, recapitulated here, covers some themes I shall expand on in coming issues.

The <acronym>APL</acronym>s have always been tools of choice for analysing large sets data. Alan Sykes discusses his use of <acronym>APL</acronym> for assessing the [quality of university examinations](arque.htm), and the colourful Peter-Michael Hager shares a simple and useful method for [spotting exceptional results](hager221.htm) in large arrays. (We can’t afford to reproduce in print the subtle use of pastels in his displays, but you can see them online in their full glory.)

Congratulations to J Software on their new [jsoftware.com](http://www.jsoftware.com) website, and in particular on the J Wiki, a valuable and growing repository of documentation, code, and [essays](http://www.jsoftware.com/jwiki/Essays).

Stephen Taylor
