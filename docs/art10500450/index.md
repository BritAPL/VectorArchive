---
vid: '10500450'
title: A personal view of APL2010
prefix: Meeting
authors:
- Graeme Robertson
byline: by Graeme Robertson
volume: '24'
issue: '4'
page: '16'
online: '2010-11-13'
description: A personal view of APL2010
keywords:
- APL Conference APL2010
abstract: How should APL deal with multicore machines? Should a programmer be able to assign a thread to a CPU? Or should the interpreter automatically distribute a task over a number of CPUs at its own discretion? Or something else? Questions like these permeated APL2010. Apart from the main sessions, there were four parallel streams and since an individual could not distribute himself over all streams, he had to choose. This brief summary of the first day reflects one individual’s choice of sessions.
source: content/printed/244/robertson.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- conference reports
- performance
---

by Graeme Robertson
{ .byline }

How should APL deal with multicore machines? Should a programmer be able to assign a thread to a CPU? Or should the interpreter automatically distribute a task over a number of CPUs at its own discretion? Or something else? Questions like these permeated APL2010. Apart from the main sessions, there were four parallel streams and since an individual could not distribute himself over all streams, he had to choose. This brief summary of the first day reflects one individual’s choice of sessions.
{ .abstract }

Roger Hui gave an interesting account of how J deals with the concept of infinity and he successfully showed its potential value to APL programmers.

Dr Scholz argued enthusiastically for the need for programmers to ‘tune’ their applications to make the most efficient use of multicore CPUs and graphics card GPGPUs, and he made a related case for functional programming without side effects by the removal of imperative programs (read niladic functions) and of state data (read global variables). “Maximise frames and minimise cells” in your programs is the way to think about extracting the most from parallel processing, he suggested.

Helmut Weber pointed to the fundamental limits of faster chip technology with theoretical limits to the number of threads per core and the number of cores per die being nearly reached. With paramount future demands being measured by performance per milliWatt, he urged multicore and hybrid designs with single purpose function elements for high-spec robustness.

David Liebtag described some recent advances in APL2. As well as the goal to produce a workstation with the same functionality as mainframe APL2, he outlined new socket technology, the ability to call APL from other languages, new data type facilities and a new parallel processing each operator. He ended with a comparison of IBM graphics through the years, culminating with JAVA graphics.

Brian Becker afforded hands-on experience of a Stand Alone Web Service, the SAWS framework, written in Dyalog 12.1. It does not require IIS, only an open port on the server. One can then communicate very effectively under APL with web services on a client machine.

Morten Kromberg introduced APL# which is a new functional scripting language intended for deployment with Silverlight on a client machine. The language incorporates strong data typing, including a rank-zero string data type. Functions and operators are written in dynamic programming style with a new type of header line and new symbols for current namespace (`⌻`), shallow copy (`«`), new namespace (`¤`) and a rank operator (`⍤`). The timeframe for releasing APL# is one or two years.

Bob Smith chaired a panel discussion on the target form of APL by 2020. Issues discussed included: APL’s lack of visibility, the evident lack of experimentation in traditional computer science, promotion of arrays to objects, rationalising APL by removing anomalies, and disappearance of the APL character set obstacle.

And this was just the first day of four fascinating, entertaining and instructive days of conference at the Technical University of Berlin, the most fascinating talk for me being the account by Professor Zuse of the unquestionably heroic efforts of his father, Konrad Zuse, to invent, construct and protect through the war, the first automated programmable computer ever. It was a truly amazing story!

I went to APL2010 because I wanted to meet some of the many friends that I have found in the APL world over the years. I also went in order to learn something. Not particularly about parallel processing, which I hope will be incorporated behind the scenes without too much effort on my part by way of some primitive operator such as *spawn* that will allow us APLers to harness multicore hardware effortlessly; not in order to learn some new languages such as APL# and WPF and Silverlight that one day might enable us to empower a client to talk fruitfully to an APL server; not even to learn about the latest APL applications and supportive hardware. None of these. I went with the principal purpose of learning how to write an application immediately on the *internet platform*, using raw ASP.NET if needs be but preferably using a thin cover function like `⎕WC` provided by an APL vendor. I didn’t find anything like that.

The internet browser is now the obvious platform for new applications just as the PC was in the 80s. By the early 90s it was possible to access Microsoft Windows objects such as Forms and Buttons via ⎕NA calls to the Windows API DLLs. This was difficult without significant knowledge of C programming and very few people succeeded until Dyadic and other vendors supplied cover functions such as `⎕WC`, whereafter success was manifest and ubiquitous.

I am pleading for a thin cover over Microsoft .Net that will make it easy to program rich APL applications on the cloud, thus making them immediately accessible to any browser in the world. I know it is possible to do this now using ASP.NET, but it is far too difficult for me to do well.

Ken Iverson invented a powerful notation with very few rules and an extensible collection of profound functions such as `+` and profound operators such as `/` whose combination according to two simple rules led all the way to consolidated reports on the desks of chief executives of many of the largest corporations in the world. This was no coincidence.

Ken rightly strongly objected to the unnecessary new rules in APL2. When I proposed direct function definition at Dyadic in 1995 with constructs such as `3(⍵-⍺)4` or `(⍵[⍋⍵])1 3 2` it was more or less ignored. No new syntax rules were invoked, only a simple interpretation of `⍺` and `⍵` which had been understood for many years in the APL literature. On the other hand, dynamic programs as later introduced by Dyadic introduced lots of new rules.

Every time a new rule is introduced into APL I turn off. APL# introduces new rules all over the place and I don’t like what I see. I used to agree with just about everything Ken Iverson said, even when he criticised the irrational parts of APL itself. I was convinced by his simple argument that APL2 introduces new rules unnecessarily and makes APL more difficult. Nowadays so many new rules have been introduced that APL is becoming as difficult as any other language and the pure elegance and simplicity of the original conception with essentially just two or three fundamental rules is being lost.

Windows Presentation Foundation, well presented by Michael Hughes, is interesting but, like APL#, it seems to be a solution for a distant future rather than the solution for today that I am begging for. ASP.NET seems to me to be the current way forward. Dyalog APL can talk to it, with difficulty. I think that an APL vendor could put a cover on this quite easily and give us a way of writing applications on the modern application platform, the browser window, but I heard nothing encouraging at APL2010, from Dyalog, APL2000 or APL2, nothing that gave me confidence that an APL programmer who does not have the time or inclination to learn a new language will be able to write serious applications on the cloud any time soon. This was exactly the situation with Microsoft APIs and Windows before Dyalog introduced the beautiful `⎕WC` solution.

The beauty of APL is its fundamental simplicity. Without that it risks sliding into oblivion with a hundred other corrupted languages that once were used to communicate effectively and profoundly between humans or between humans and machines. How exactly APL manages to add one matrix to another is not of any great interest to most APL programmers because they are thinking at a higher level and ask most of all that the interpreter will free them to think abstractly.

When I suggested rewriting an APL interpreter based on the algebraic foundation of GiNaC or the like \[see *Vector* 20.1 (2003) 132-142\], the only response from Dyadic was that too much had been invested in the current interpreter ever to consider starting again from scratch even if it meant getting infinity and many other mathematical goodies for free, and yet now we have a proposal to do exactly that with APL# – but this time adding all sorts of new rules, getting no more (perhaps less) significant functionality, while at the same time abolishing canonical forms and even the concept of the workspace. I can’t agree with that.
