---
vid: '10011800'
title: Tacit J and I
authors:
- Neville Holmes
byline: Neville Holmes School of Computing and Information Systems, University of Tasmania Launceston 7250, Australia Neville.Holmes@utas.edu.au
volume: '23'
issue: '3'
page: '52'
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
source: trad/v233/tji.htm
converter: vecrescue 0.1.0
issue_tab: n3-june-2008
tags:
- education
- language design
---

Neville Holmes  
School of Computing and Information Systems, University of Tasmania  
Launceston 7250, Australia  
[Neville.Holmes@utas.edu.au](mailto:Neville.Holmes@utas.edu.au)
{ .byline }

This essay gives the background to the series of articles expounding the joys of functional calculation that starts in this issue of *Vector*. It is largely autobiographical, finishing with a description of experience teaching tacit use of J to Honours students using the articles of the series.

## APL in Boxes

It’s so long ago that I can’t remember just when I first heard of Ken Iverson’s APL, but it was in the mid-60s and I remember two particular events.

A colleague at IBM Australia, Jacques Grenot, introduced me to Iverson’s 1962 book *A Programming Language*.<sup><a href="#ref1">\[1\]</a></sup>  Ken had taught a class at IBM’s Manhattan Systems Research Institute using the book and its ‘box notation’ that Jacques had taken and been very impressed by.

At roughly the same time, 1964, the *IBM Systems Journal* came out with a formal definition of IBM’s pending unified computer architecture written by Ken and two IBM colleagues.<sup><a href="#ref2">\[2\]</a></sup> This definition used box notation to compress into two wallcharts with accompanying explanation what internal gossip claimed had previously taken two six-foot shelves of ringbinders to contain. The same gossip claimed also that numerous errors had been uncovered through use of the notation.

Box notation had two related virtues, at least for people with a technical background: it used familar mathematical symbols and it was extremely succinct. Also, the boxes and the sequencing lines placed outside the boxes made the structure very plain to see. For these reasons the notation found some subsequent use for specification in proposals for computing systems, though not by me.

## APL\\360

In 1970 I took up a staff position in IBM Australia’s Systems Development Institute in Canberra. Because of my familiarity with box notation, and because of its development into an interactive system, I pressed to have APL\\360 installed on the Institute’s 360 Model 67 running, after a false start with TSS/360, under CP/CMS which later came to be called VM/370.

APL\\360 was delightfully immediate. You keyed the expression for your computation into a typewriter terminal and the result was typed on the next line, typically straight away. The 2741 terminal had a ‘golf ball’ printing element that allowed most of the box notation’s mathematical symbols to be used. Because the boxes were impractical on a line-by-line terminal, a few extra symbols had to be added to allow sequencing within function definitions to be defined.

The APL notation was quite easy to learn for anyone in the right frame of mind, and so I had quite a few joint development projects using it in tertiary education in fields such as conventional and naval architecture.

Because computers were still quite novel to the public, the Institute often played host to groups of people from schools or from the community who wished to see a wonderful computer in action. I wrote several interactive packages for them in APL, for example to test them on roman numerals, so that after they had had the equipment in the machine room explained to them, they could be taken into the terminal room to actually *do* something on the computer themselves.

The enthusiasm of young people was remarkable, particularly primary school pupils who were often very reluctant to be moved away by their teachers. Adults were usually much more sedate. On one memorable occasion an elderly visitor took much persuasion to actually seat himself at a terminal but could not be persuaded to touch the keyboard.

The advent of the IBM 5100 enabled me to work with teachers, mainly primary school teachers, to put a computer temporarily in their classroon. The 5100, a ‘luggable’ computer, rather cunningly used microprogram to simulate a System/360 host for the IBM Basic and APL\\360 interpreters. There was no graphic capability, but I wrote an APL package to allow primary school children to key in expressions like

```
   A RECTANGLE 17 BY 5 OF 'MW '
```

to be shown interesting patterns. Gratifyingly, some children learnt how to add useful functions to my package. Sadly, I only had the one machine and so could not lend it for long periods.

Unfortunately, the Institute was not successful and was closed down in the mid 70s and I reverted to conventional field work. However, I was still able to get APL used, though usually indirectly through the excellent spreadsheet and database packages that were available. When visual display terminals started being adopted, use of APL was greatly encumbered by IBM’s prolonged delay in providing the APL character set for them.

## Tacit J

In the 80s I shifted to Sydney and APL moved into my background. Then I retired from IBM and at the beginning of 1989 took up a position as lecturer in a new degree course in Applied Computing just starting at the Tasmanian State Institute of Technology in Launceston. Ironically, my first duty was to teach Fortran in a service course unit; my first programming language in 1960 had been Fortransit for the IBM 650.

As an APL enthusiast I had planned to insinuate APL into my teaching, and I got a copy of IBM’s APL2 for the PC for use in teaching eventually, and to keep my hand in in the meantime. Unhappily, the PCs available to me would not run APL2 properly, and after a long struggle I had to give up. Then I reluctantly tried J.

It worked first try. Two differences were uncomfortable. J’s use of the ASCII character set was neat but ugly compared to APL’s mathematical symbols. The replacement of the up-to-date workspace by the indirect script seemed to be a backwards stride. What more than compensated for these, though, was my discovery of the tacit style of coding in J.

The idea of inferring a functional expression’s use of its arguments seemed to me then, and still seems so, to be the purest form of functional programming available. Since then I have only used explicit definitions when deficiencies in my knowledge or in the notation forced me to. At one stage I coded a simulation for von Neumann’s Princeton IAS computer and an assembler for it entirely in tacit mode.

An early use I found for J was in a first-semester first-year course unit called “Professional Computing”.<sup><a href="#ref3">\[3\]</a></sup> As part of this unit I had students learn elementary use of four kinds of coding schemes: functional (tacit J), logical (Prolog), database (SQL), and object-oriented (Python). Each was learnt entirely through interactive lessons, written in tacit J in the first case.

More seriously, for about a decade I taught an Honours unit called “Functional Programming” in which students were required to undertake a serious project using tacit J. For the first half of the semester students were given a sequence of lectures, actually more like demonstrations, using the handouts that are the basis of of this series of articles. Each week the students were required to complete a test given in the handout to confirm their learning of the material.

The students seemed to enjoy the unit, though in the later years they complained of the difficulty of the work. Significantly, this was mostly from full-fee paying overseas students.

I enjoyed teaching them. My main difficulty was, time and time again, getting around a shortcoming of tacit J. One not quite tacit aspect of J is the use of brackets as primitive functions to bring an argument freely into an expression when it was inconvenient to imply its use: `[` for a left argument, `]` for a right argument.

The problem was that there was, and is, no similar way to bring an operand freely into a tacit expression. All too often I had to give up insisting on tacit coding and let the student code explicitly. Earlier versions of J had something of this kind, *lev* `[.` for a left operand and *dex* `].` for a right operand, but tragically, because these were parsed as operations they were extremely difficult to use and were dropped out early on.

Occasionally I passed the more interesting results of student projects on to *Vector* for publication.<sup><a href="#ref4">\[4\]</a></sup> They appreciated getting them published, and getting a year’s free subscription, but they didn’t renew. Sometimes I asked why, and the answer was usually along the lines that, although they found tacit J fascinating, they didn’t see it as something they would be able to use in their postgraduate work. Their coursework had told them that programming was synonymous with Java, and tacit J and I were seen as novelties.

## Conclusion

Teaching tacit J coding was a most enjoyable experience. I hope that the publication of my course material for such teaching will bring enjoyment to future readers of *Vector*.

As my “Functional Programming” course unit was only one semester long, my hand-out material was almost entirely on tacit J proper. If a student wanted to link to system capabilities then they had to work this out with little help from me. That many did so perhaps indicates their interest in pure functional programming.

## References

1. <a id="ref1"></a> Iverson, K.E. (1962) *A Programming Language*, John Wiley & Sons, New York, 286pp.
2. <a id="ref2"></a> Falkoff, A.D., Iverson, K.E., & Sussenguth, E.H. (1964) “A formal description of SYSTEM/360”, *IBM Systems Journal*, 3(3), 198-261 ([PDF in archive](http://www.research.ibm.com/journal/sj/032/falkoff.pdf)).
3. <a id="ref3"></a> Holmes, W.N. (2000) “Teaching ‘Professional Computing’”, unpublished ([PDF in archive](http://eprints.utas.edu.au/2605/)).
4. <a id="ref4"></a> Benn, D. (1996) “Turtle Graphics in J”, *Vector*, 12(3)  
    Towers, A. (1999) “An Approximation of the N-body Gravitation Problem in J”, *Vector*, 15(4)  
    Schofield, B.A. (1999) “Windows BMP Files and J”, *Vector*, 16(1)  
    Charman-Williams, A. (1999) “Bayesian Machine Learning in J”, *Vector*, 16(2)  
    Chamley, C.A. (1999) “Functional Implementation of Moments Based Region Descriptors in J”, *Vector*, 16(3)  
    Dazeley, R. (2001) “J for JPEG: Windows Bitmap to JPEG Baseline Compression”, *Vector*, 18(1)  
    Horton, M. (2004) “Celtic Knotwork in J”, *Vector*, 21(1)
