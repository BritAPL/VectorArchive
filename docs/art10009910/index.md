---
vid: '10009910'
title: 'Editorial: Winter in Naples'
authors:
- Stephen Taylor
volume: '22'
issue: '2'
page: '3'
source: trad/v222/ed222.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- language design
- conference reports
---

The annual APL2000 conference in Naples, Florida always produces interesting papers, and this winter has yielded a good crop. Your editor had his lifetime quota of hot weather when living in Australia, but the tireless Adrian Smith was there with golf club, camera and laptop, and filed a report.

The big news in the <acronym>APL</acronym> world this year is interpreter extensions to support classes and instances. Organising software into objects is the best programming paradigm devised for representing complex states that must respond to unpredictable events. The paradigm appeared with the graphical user interfaces it made possible. <acronym>GUI</acronym>s quickly made themselves indispensable, and object-oriented programming became the standard way to write software. Smalltalk-80, both operating system and programming language for the Xerox Star, demonstrated that all programs can be written as objects; programmers who started work after 1990 could be forgiven for thinking it is the only way.

Modern <acronym>APL</acronym>s support <acronym>GUI</acronym>s and provide object-oriented interfaces to them. Programmers writing in the <acronym>APL</acronym>s have found <acronym title="object-oriented programming">OOP</acronym> a useful paradigm for tackling some tasks, and some have devised their own ways to implement the class-instance relationship. But I am fond of quoting the J lab on <acronym>OOP</acronym>:

> It is a mistake to think of OOP as an alternative to FP (functional programming).  
> All languages, at the low level, have FP, and at a higher level, have OOP.

I am unsure of the truth of the last statement (is it true about Forth?), but have found it an excellent guide to designing applications.

Even without user-defined classes — arguably the heart of <acronym>OOP</acronym> — <acronym>APL</acronym> programmers can write about arrays of objects in ways that quicken the pulse. For example, in Dyalog APL, to arrange six button objects, `B1` to `B6` in a grid anchored at `offset` with `spacing` between rows and columns (`offset` and `spacing` are pairs):

```
(↑(B1 B2 B3)(B4 B5 B6)).Posn←offset∘+¨spacing∘×¨⍳2 3
```

Adding user-defined classes immensely extends this, blending two powerful abstraction paradigms. In tackling this, the interpreter writers have had to address problems largely unknown to <acronym>OOP</acronym>; for example, what is the prototype of an object, that enables arrays of objects to be empty or overtaken? Just as when nested array systems were explored, the consequences of alternative solutions are hard to foresee. Our best wishes go to the implementers at Dyalog and APL2000.

In this issue, Bill Rutiser introduces the MOM object system in APL+Win, and Morten Kromberg continues his 3-part introduction to <acronym>OOP</acronym>. Much of the interest in <acronym>OOP</acronym> arises from the need to interface to systems through object models. Adrian Smith has done useful work finding out how to develop in <acronym>APL</acronym> and deploy in C#. In *Vector* 21.2 Richard Smith described Rowan, an <acronym>APL</acronym>-like interpreter constructed from .Net components; here Eric Lescasse previews APL+Santa Fe, a similar project with larger ambitions for commercial use. MicroAPL has long focused on spanning platforms and supporting important interfaces; Richard Nabavi reviews just how far APLX can reach.

APL+Win has recently added try-catch exception handling, and Bill Rutiser explains how it can be used to write more clearly. Clarity is also in Norman Thomson’s thoughts, as he uses J to explain internal rate of return calculations in text messages.

The small size of array-programming teams limits our experience of collaboration tools such as libraries and source code managers (<acronym>SCM</acronym>). Often we are able to write our own simple tools, well adapted to our lesser need. This was the experience of SoftMed Systems, which is now migrating from its own <acronym>SCM</acronym>, written in <acronym>APL</acronym>, to a commercial product, Perforce. Richard Renich tells the story.

Lastly, Sylvia Camacho writes about the work of the late Gérard Langlet, some of it published in *Vector*, who used <acronym>APL</acronym> to question the mathematics underlying the physics, chemistry and biology of perception.

Stephen Taylor
