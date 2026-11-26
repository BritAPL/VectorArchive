---
vid: '10003660'
title: Unix APL Power Shootout (Part 2)
authors:
- Bob Hoekstra
volume: '17'
issue: '1'
page: '48'
source: trad/v171/bob171.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-2000
tags:
- reviews
- implementations
- performance
---

##### By Bob Hoekstra ([Bob.Hoekstra@khamsin.demon.co.uk](mailto:Bob.Hoekstra@khamsin.demon.co.uk))

## Abstract

> Continuing from last issue's article, we compare the compatibility, performance and features of *APL+Unix version 5.3.2* from APL2000, *Dyalog APL/M version 8.2.2* from Dyadic Systems and *SAX (<b>S</b>HARP <b>A</b>PL for UNI<b>X</b>) version 5.0.0* from Soliton.

## Introduction

This article is the second of a two-part series, and completes my reviews of Unix APLs (at least for the moment). It follows on from the article in Vector Vol. 16 No. 4<sup>\[<a href="#Note1">1</a>\]</sup>.

For those who missed the previous instalment, it is available on the Vector web site<sup>\[<a href="#Note1">1</a>\]</sup>.

## More on the installation

### Solaris 8

Since part 1 went to press I have upgraded my Sun Ultra 5<sup>\[<a href="#Note3">3</a>\]</sup> workstation with a new 20GB hard disk containing Solaris 8, the latest version of Sun Microsystems's version of the Unix operating system. Naturally I was curious about any performance differences, so immediately installed the APL interpreters under this new OS.

Both APL/M and APL+Unix installed under Solaris 8 exactly as they did under Solaris 7, but I had great difficulty with SAX: I never did get the NSVP processor running and thus could not run SAX under Solaris 8. I suspect the problem may be that Solaris 8 is more critical of daemons running on it. I tried installing SAX while running Solaris 8 in 32-bit mode, but this did not help. I did not spend too much time on this, or I would not have made the deadline for this article.

I completed this article working in Solaris 7. I did some brief comparisons between the two OSes using the other two APLs though, and found no significant differences in performance or any other aspect. I am confident that Soliton will address this problem soon, and I do not see this as a major problem with their interpreter.

### Solaris 2.6 on a Tadpole

I have also recently come into possession of a Tadpole<sup>\[<a href="#Note4">4</a>\]</sup> SPARCbook 3GX running Solaris 2.6. All the interpreters seemed to install on this machine satisfactorily, but with a previous generation MicroSPARC processor running at only 110 MHz and a mere 32 MB of memory, performance is less than ideal. I did not use this platform except to confirm that it works.

## APL speed comparisons

Tests were created specifically to compare execution speeds and file system access speeds of the three interpreters. Because of other system activity and the state of workspaces, timings were not constant when tests were repeated, but all tests were repeated enough times to ensure that the results were representative.

In all cases, I was very careful to run **identical APL code** on the three interpreters, which meant "standard" APL was used. I also tried to level the playing field by creating approximately the sam size of workspace throughout: 4 MB.

### Code performance

I spent quite a lot of time timing various bits of standard APL code, but there were few conclusive results. Generally, APL+ seems to be the fastest on most code, with APL/M following close on its heels (differences in time taken are typically less than 10%).

SAX generally came last, but again the difference in expired time (between APL/M and SAX) was only of the order of 10%, and frequently less. SAX seemed to perform worse when there was more looping involved. Some code examples showed a slightly different picture, with SAX coming almost level with APL+, followed closely by APL/M.

The differences shown here are not conclusive. After the comparison was completed I tweaked some of the APL/M code, converting some of the code to dynamic functions. In all cases I got a significant speed-up with dynamic functions: a minimum of 10% and one case (which lent itself to tail recursion) an reduction in processing time of nearly 50%! I am sure that much of the code could have been improved for the other interpreters as well, but I didn't pursue this as deadlines were looming.

Furthermore, Soliton is due to come out with a new version of SAX shortly. I have heard that some of the work that has gone into the Linux version has meant that they have got significant performance improvements in the next generation Solaris version. If this is true, then my results could be back to front by the time a reader of this article makes a purchase.

In any case, I would be very surprised if the differences in performance between these interpreters were significant. By that I mean that these differences would be swamped by the effect of hardware improvements, say adding memory, upgrading the processor speed, or getting an extra processor. Even a new disk drive (say a 10,000 rpm disk replacing a 7,200 rpm disk) could make a difference which would make these performance differences seem irrelevant.

If there is a lesson here, it is that all three of these interpreters perform adequately. If you have to choose which to buy, basing your choice on performance is unlikely to be a wise decision. Let's face it, if performance were that critical, you would probably want to think about compiling at least part of the APL code to maximize performance.

### File performance

Here differences were far more marked. I timed (a) the creating, "stuffing" and untying, (b) tying, reading and untying and (c) tying and erasing of component files in a tight loop. To get a reasonable idea, there were two basic file types: "small", where the component contents consisted of the atomic vector, and "large", where the components comprised a 20x20x20 numeric matrix. The loop was such that the product of the number of components and the number of files was 3,000.

In all cases, APL/M was significantly faster than the other two APLs. APL+ came close on the "small" files, but fared less well when the components were larger. SAX was tested with the two extreme synching modes: no synchronising (fsync=0) and "full" synchronising (fsync=2). The latter was very slow indeed, generally taking 2 to 3 times as long as the former, which in turn was significantly slower than the APL+ performance in most cases. The only exception was the tie/read/untie test, were APL+ and SAX (with fsync=0) were more or less level, but both still slower than APL/M.

No doubt a very small part of the poor performance of SAX is that it performs less well in loops, but this cannot possibly be all the difference (or even a significant part of it). Perhaps more interesting is a comment in the manual that, after a file has been created, SAX has this as a shared tie, rather than an exclusive tie. I suspect this carries a performance penalty. Note that the tie/read/untie cycle compares quite favourably with APL+. But this does not explain the difference in the tie/erase cycle.

An interesting point about APL/M's good performance is that the resulting files seem much smaller. I cannot explain why, but a component file containing 100 "large" components was 1,603,788 bytes in the Dyalog component file, but 3,227,428 bytes for SAX and 3,203,856 bytes with APL+.

This points to a slightly different conclusion than the code performance because the differences in performance are so much greater. For an application where file system performance is critical, APL/M might be a good choice.

## Features

### Dyalog APL/M

This appears at first sight to be the most feature-rich of the three interpreters. Immediately obvious is the GUI development environment. For the developer this is absolutely wonderful, and once you have grown accustomed to it, it is difficult to do without. However, I did come across a few glitches: editing windows would suddenly (and for no good reason) disappear behind the session manager when I opened a traces window on a variable, and my only crash happened while I had many windows open (I think it was more than 30). Furthermore, the syntax sensitive colouring doesn't seem to work quite as well as it does in Windows.

The GUI environment stretches into the code as well, with the very easy creation of GUI objects which can be used during run-time. Attractive and intuitive applications can result from this, and perhaps the greatest strength of the APL/M GUI area (which neither of the other interpreters have) is that code for Microsoft Windows is so easily ported to Unix (well, actually Motif). Even this has a few little buggettes though, as the Motif widget sizes are not quite the same as those of Windows, ported applications need a little tweaking to get positioning right. Sometimes the automatic positioning gets it a little wrong as well. But these little problems should not detract from what is a great feature.

Dyadic's APL interpreters are known for their support of namespaces. This is an integral part of APL/M as well, and can help keep code neat, as well as allowing code imports with less fear of name clashes. GUI objects are namespaces in themselves, as are TCP/IP interfaces (the existence of which is a feature in itself).

But the features do not stop here. My personal favourite is dynamic functions, which could be seen as Dyadic Systems's response to J and direct definition all in one. It can give code which is extremely elegant, easy to maintain and lightning fast.

There are several other great features, not least the ability to create user defined operators. Personally, I do not use this much, but one feature that I do use constantly while debugging code is the ability to step through code using the windows that can be set up to appear automatically when code crashes. Even the fact that function editors are separate windows (i.e. the session manager is directly available while you are editing a function) can be a great productivity aid.

APL/M also supports a very full implementation of control structures. This is largely the same as the control structures in APL+.

Then there is the high degree of compatibility between the different versions. APL written for any Microsoft operating system will usually just run. Of course there are things like different naming conventions for files that might be a problem, but the code (including GUI generation) will run. Component files will have to be ported, but Dyadic Systems provide the tools to do this.

Lastly, many people might value the compatibility with other APLs. Using the migration level system variable, the developer can tailor the compatibility to APL2, which sets the standard in many ways.

### APL2000's APL+Unix

By comparison, APL+ seems almost devoid of features. However, this is not quite true. No GUI, no syntax colouring. In fact, a bit like the STSC APL\*Plus for DOS many years ago.

Of course, this is unfair. While there are few cosmetic features, there are many really useful ones, like a good TCP/IP socket interface. The editor has hardly changed since the early STSC days, but it works well. There are several features which point towards compatibility with APL2, and the language features are slowly being extended, e.g. scalar dyadic functions with axis are supported, as is n-wise reduction and APL2-style function attributes.

There is also support for control structures, more or less identical to those in APL/M. Also, component files have the same structure between Unix, DOS and Windows, meaning that one has data portability between platforms. Note that this is the opposite of Dyadic Systems's implementation, where the workspace has the same format but not the component files.

The user commands are a very useful feature. One of the many useful sub-features that this creates is the concept of packages. This is somewhat different from the packages of SAM and SAX, but many of the concepts are similar.

To me the most important thing was that, while there were no features that APL/M didn't have, every feature worked perfectly every time. APL+Unix did not crash once. Not even a hiccup.

### Soliton's SAX

SAX also has fewer obvious features. But then, SAX is a slightly special case as the APL is somewhat different. It shows a family resemblance to J in many respects, introducing the concepts of rank, frame and cell which the others don't have.

The existence of the NSVP is a very strong plus point. This could be invaluable to those who want to access say a DB2 database on a mainframe from a Unix APL session.

The concept of "intrinsic functions" - essentially an easy interface to compiled code - is also a very strong feature. This can be done in both the other interpreters, but it would appear that the SAX implementation allows the greatest flexibility,

As yet, there is no GUI implementation, but with the recent announcement of the Java interface for SAX/Linux, I suspect that the other Unix versions will get a Java interface as well.

SAX is highly compatible with SAM, the mainframe version. This extends to the use of the same rather clumsy editor. This takes some getting used to, but eventually becomes quite usable (although I am still not fond of it). The other powerful feature that both share is packages, which can be used to squeeze large workspaces into only a small amount of space. This might not be all that useful now that memory prices have decreased, but like APL/M's namespaces, packages can also be used as a name clash preventative.

I suspect that this interpreter will have many enhancements soon and will become a very strong contender indeed. Having said that, it has a language implementation which many will like - it took me a while, but it is definitely growing on me.

## Recommendations

It is impossible to recommend purchase of one APL above the other. All of the interpreters reviewed are "good", though not necessarily in the same areas.

If you have used a Dyalog interpreter in the past and liked them, then APL/M is definitely the right choice. You will feel at home immediately. The portability of workspaces from DOS or Windows is also a very strong point. This interpreter might also have some additional benefit if file system performance is critical.

Alternatively, previous users of STSC, Manugistics or APL2000 interpreters would get on much better with APL+Unix. A particular strong point here is compatibility of component files across architectures. This means that one could keep APL running on multiple operating systems with a single data source.

Last, but by no means least, SAX would suit those who want to port from SAM on a mainframe. It might also be better for those who have been using J and don't want to give up all the benefits of this advanced APL derivative.

The only suggestion I can give to the prospective purchaser is to read the features section above carefully and decide which points are applicable in the situation.

## Thanks

I thank the manufacturers of the 3 interpreters reviewed. All have helped me at some stage.

***

## Notes, References and web sites

1. <a id="Note1"></a> Refer to *[APL Power Shootout, Part 1](../art10003650/)*, *Vector* Vol. 16 No. 4, *[My SAX Experience](../art10003640/)*, *Vector* Vol. 16 No. 3, and *Dyalog APL for Motif Version 8.1 Release 2 for Sun Solaris 5.5*, *Vector* Vol. 14 No. 3. all by Bob Hoekstra.
2. <a id="Note2"></a> The web site for Soliton is [http://www.soliton.com](http://www.soliton.com), APL2000 have a web site at [http://www.apl2000.com/](http://www.apl2000.com/) and Dyadic Systems's web site is at [http://www.dyadic.com/](http://www.dyadic.com/).
3. <a id="Note3"></a> The web site for Sun's Ultra 5 page is [http://www.sun.com/desktop/products/ultra5/](http://www.sun.com/desktop/products/ultra5/).
4. <a id="Note4"></a> Tadpole and RDI now share their web prescence at [http://www.rdi.com/](http://www.rdi.com/). They were competitors in the portable SPARC market, but joined forces some time ago. Unfortunately my SPARCbook 3GX (being an older machine) gets no mention on the site.

***
