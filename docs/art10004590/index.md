---
vid: '10004590'
title: Why APL Programmers Don’t Use Libraries
authors:
- Morten Kromberg
byline: 'by Morten Kromberg (email: mkromberg@insight.dk)'
volume: '20'
issue: '1'
page: '62'
source: trad/v201/lib201.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-2003
tags:
- development practice
- APL community
---

*by Morten Kromberg (email: [mkromberg@insight.dk](mailto:mkromberg@insight.dk))*
{ .byline }

<i>

The next best thing<br>
 to  knowing something is<br>
 knowing where to find it.<br></i>  
Samuel Johnson

As Stefano Lanzavecchia announced in Vol.19 No.4 (aka “April 2003”), the BAA is starting a Library Project, and I have volunteered to be the Librarian. The goal is to create a repository for freely available code, decently documented – supporting as many APL (and A+, J, K) interpreters as possible.

## Here We Go Again

“Here we go again”, I hear the old APL dogs groan to themselves. I was tempted to join in the chorus myself. We’ve been there, done that and – as everyone knows – you can’t get APL programmers to use libraries (or, according to popular myth, adhere to standards of any kind). There is no doubt that – if you compare APL developers to most other programming communities – one of the ways in which we are different is in our use of libraries. For the most part, we don’t – and in many situations, there are good reasons for this. My task as the librarian – as I see it – is to identify the most important reasons why we make less use of libraries, and – to the extent that it is profitable to us – help develop an APL library culture. I have accepted the challenge because I am convinced that unless we do, APL cannot find its place in the programming community, and it can only continue its current slide into anonymity and finally extinction.

The two main reasons why I am convinced of this are:

1. There are currently almost no APL training programmes in existence. This may not be quite as bad as it sounds, because – as fast as things are developing in this day and age – self-study accounts for an increasing fraction of IT training. But without well-documented libraries, you can’t even teach yourself APL. Occasionally, APL, J and K get some sort of mention in relevant media, for example when Dyadic joined the DotNet programme or launched APL for the PocketPC – or when APL2000 was acquired by Cognos. Any interested young programmers who tried to dig a bit deeper and use the web to find out whether APL was fun almost certainly gave up within about 15 minutes.
2. The proportion of any application dealing with interfaces to other applications, using various standards, is growing continuously. Conforming to these interfaces without using libraries is ever more expensive. Knowledge about what interfaces are available is critical. This is not just a question of providing interface components in APL, but educating APL developers about how to tap in to the wealth of libraries which are available to other programmers. Modern APL environments actually allow you to do this quite easily, but we tend to be too ignorant to benefit. Paraphrasing Samuel Johnson, most of us don’t even know “where to find it”.

## Now is the Time

I know that there are active APL developers with sufficient exposure to other environments to understand the importance of these issues and to help do something about it, but somehow the APL establishment has been unable to recruit these “youngsters” (currently mostly in their 30s, 40s – and I guess we’d better include a handful of youngsters older than that) into active service. Many of these people have been too busy making a living writing code to take much notice of the – as they perceived it – rather academic APL community. The community has had less and less to offer the professional APL developer since the dispersal of the large, professional APL development groups at IPSA, STSC, IBM and others when timesharing came to an end together with the 1980s – at the same time as the language split into two or three significantly different dialects.

It is my hope that a handful of APL developers who are still young in mind will be able to step forward at this time and make a contribution towards the continued profitability of the use of the APL (family of) language(s). APL, J and K have a lot to offer end users in terms of enabling usability and flexibility which they might otherwise never experience. Not only that: I totally agree with Stephen Taylor when he says in his article “A Place for Us” – also in Vol.19 No.4 – that my productivity in APL commands a premium for which I cannot charge in other languages. We need to make an extra effort at this time – while we still have the energy – to ensure that this favourable state of affairs is maintained.

## So Why Don’t We…

… use libraries? There are probably a large number of factors which have conspired to cause this. Many of the strengths of APL turn out to have a dark side with respect to libraries (and we must remember that in many situations, this remains a strength: having to learn to use a huge library is not necessarily a quick way to get things done).

### Once Upon a Time, APL was WAY Ahead

When the art of APL application building was developed, in the late 70s and early 80s, the interactive APL environment was unique in its productivity and connectivity tools. While BASIC and ALGOL programmers needed libraries in order to do “trivial” things like sort a table or invert a matrix, we had these facilities built in to the language. This was not only true at the algorithmic level – many commercial APL systems had significantly better interfaces to operating system resources than competing environments such as FORTRAN and COBOL.

Using the Auxiliary Processors which were provided, any APL programmer could throw together a set of utility functions to interface to Sequential Files, TSO Commands, VSAM and DB2 in ways which fitted his or her personal taste or application requirements. People who had to do the same things from other environments had to get very well organised, help each other, and learn to accept other people’s tastes. When C came along, it was primarily used for interfacing and writing operating system components, so the art of building and using libraries was developed very early on. You could not really be productive at all in the other languages without libraries. APL’s Auxiliary Processors were the equivalent of libraries. At I.P.Sharp Associates, there were also APL-coded services for printing, graphing and so on, accessed in the same way as the APs, using shared variables. However, these “libraries” ran in a different address space and never made classical library builders of us.

APL’s primitives for getting user input included Quad, which would allow the user to execute APL statements to generate input! This quickly fell out of favour with serious application developers, but provided functionality which the FORTRAN guys would have given a limb for.

Managing user interfaces for large applications finally introduced complexity which prompted some of the first serious library-building in APL. Even before AP124 and GDDM appeared, some of us were working on sophisticated “libraries” to collect user input to multiple prompted lines of input, reacting to keywords like BACK, QUIT, HELP which allowed the user to navigate around TTY mode applications.

### Compilers Encourage Modularity

Because of compile times on early machines (which often ran into several minutes even for small applications), if for no other reason, the user of a compiled language quickly became accustomed to compiling small pieces of his application and linking them together, only working on a small part of his application at a time. While working on one piece, the rest of the application was static and unchangeable. The development environment was geared towards this mode of development, which encouraged clear division of the application into parts with interfaces which changed as little as possible.

A culture developed, with libraries, linking mechanisms, catalogues, publications and later web pages discussing and documenting what was available.

APL users saw no reason to adapt this cumbersome and restrictive mode of operation. The ability to perform rapid prototyping of all components simultaneously gave (and still gives) the APL developer a huge performance advantage.

### APLers Focus on “The Problem”

One of the most important reasons why APL is so productive is that it allows people who have great ideas but are not IT professionals to get deeply involved in the software development process, sometimes even writing entire applications themselves. Not only do these people have even less understanding of professional software development techniques and tools, including libraries – they care even less than many APL developers about standards and interfaces. The important thing is calculating the value of that portfolio or price of that insurance premium – not whether we can produce a nice data entry form for the parameters, adhere to good (or at least common) GUI principles, or whether we can save the data in an SQL database or as XML.

The following story illustrates the kind of thing that probably still happens in APL teams:

> I had the privilege of working with George Kunzle, who invented a internal budgeting and planning application called FREGI at IBM, which became the Kunzle Planning System, renamed Adaytum Planning and remains the cornerstone of Cognos Management Series Planning Analyst™. George hated (and probably still hates, although I believe he has learned to live with) Windows. He had developed a highly usable user interface in DOS, which made extensive use of colour to indicate whether numbers were input, calculated, changed (etc.) in his data entry screen. The white Windows background reduced the visual clues dramatically, and made data entry much slower. The “non-standard” DOS interface was superior in most respects, and George was quite right (from a functional point of view) to resist Windows, as it added no value for his users in the context of his application.
>
> Finally, with Michael Gould and Ellis Morgan, George developed a Windows Interface in Dyalog APL version 6, the very first Windows APL, which had extremely rudimentary GUI objects available. With George’s attitude to usability, the only acceptable approach was to build the thing from the ground up using PolyLine and Text to implement everything. The Windows Reporter component, with its decidedly non-standard user interface, was a superhuman feat of engineering, and the last thing I heard, it was alive and kicking at Cognos, still resisting all efforts to replace it using modern technology.
>
> At the end of the story, I am pleased to report, George was a wealthy man, but still had (most of) his principles intact.

APL developers are sometimes called uncompromising mavericks, who would rather rewrite something than try to understand what others have done. The truth is that much of what is done in APL is (or started out as) work on the leading edge. APL was successful because it was the only tool which could tread fast enough to avoid sinking where there were no libraries.

To take a positive view on this, our focus on the “real” issues contributes enormously to the value of software developed in APL, but has led us away from becoming good library users.

### “Throw Away Code”

Some people claim that APL attracts anti-social individuals who take pleasure in going their own way and have a superiority complex about APL. If this once was true, I doubt that many such individuals are still making a living with APL, so I think we can safely ignore the problem at this point in time.

However, it is true that the phrase “Throw Away Code” was coined, to describe the situation which would often occur during prototyping, where one or more lines of code was hacked together, often in a very experimental fashion, until the result had the desired shape and content. Since the entire process may only have taken minutes, APL developers would be content to leave it in the application, prepared to throw it away and rewrite it completely if input data or other constraints changed, rather than take the time to restructure the final code into something comprehensible, document it and perhaps isolate it in a utility function and save it in a library. Because of the speed of development, the effort of maintaining documentation and remembering the content of libraries was not considered worthwhile – and this is clearly another reason why a library culture failed to evolve in the APL community. However, I hasten to add that this is NOT a good reason not to use this approach – it may well be the best solution, depending on what you are trying to achieve – but some programmers are undoubtedly a bit too quick to reject the idea that a clean version of the code might be useful soon.

### Source Code Management Systems

The early APL workspace, much like a spreadsheet, was a very convenient sandbox in which to play. Unlike compiled environments, which had linkers and loaders, it provided absolutely no mechanisms for differentiating between the new application work you were doing, and any libraries you might be using. You would )COPY tools and utilities into the workspace, and they would become part of the big mess. Naming conventions were invented to solve the problem: one type of objects had underscored names, or were prefixed by delta, delta-underscore, the other not, thr chr nms for utilities, – and later uppercase vs. lowercase – but this was only OK until you met someone at a conference who gave you a diskette with a utility library, and you discovered that his company had the reverse convention (let’s leave index origin out of the discussion at this point, in order to maintain a positive frame of mind).

Workspace size limits, multi-developer projects, and eventually corporate info centres based on a core of common code, forced some teams to develop source code management systems outside the workspace. These were either used to build workspaces which were then saved, or – especially in SHARP APL which usually ran with smaller workspaces – dynamically loaded packages of functions and data at runtime. Some of the more sophisticated dynamic loaders went as far as creating a shell function which localised all the dynamically loaded code, so that it was isolated from the environment and disappeared when the selected applet terminated. Unfortunately, while these source code management systems often prompted the development of a good local library culture at each individual corporate site, and in many application development shops, they were home grown and all quite different – and could not help establish a global library culture. The APL vendors never really tried to lead the way. (OK, they did give us )PCOPY, so we could avoid accidentally copying over objects in the workspace J). IBM did release an enhanced version of the FSDESIGN application for 3270 panel design which had some source code management facilities, I believe it was named APE – but it never achieved widespread use. I.P.Sharp marketed a source code management system called LOGOS, and developed but never marketed an application environment called SHAPE. Before the vendors managed to get into the development environment business, they ran out of money, downsized, and much of the steam went out of the community. The introduction of the PC also contributed to making it harder to keep a community together.

### How Close Did We Come?

As mentioned above, many large APL shops do have – or once had – a library culture. The source code management system contained utility libraries – often versioned – and a developer could usually be located who was responsible for a library and could explain how it worked, even if the documentation was often scanty.

At I.P.Sharp Associates, back in the early 1980s, I remember learning most of what I know about writing APL code by poking around the “public libraries” with numbers below 1000, reading the code and talking to the developers. Many of the libraries had discussion groups in the mailbox system, and it was always possible to get good advice from people who had written or used the code in question.

Back then, I actually felt that there was a strong community which provided useful libraries and helped me learn.

### What Has Changed?

Unfortunately, I believe that one of the most important problems may be hard to deal with, namely, that the APL business (like many others) is nowhere near as lucrative as it was in the timesharing days. Back then, IPSA, STSC and IBM were making a bundle selling timesharing or hardware. Once the APL applications were built, they generated a steady monthly revenue stream and APL developers had plenty of time (compared to today), to sit back and think about how to do things faster. Most of the money was made by selling storage, CPU upgrades and new channel controllers, CPU cycles, connect time and KILOCHARS on the packet-switched network. Consulting was often given away or heavily subsidised, and poor programmer productivity clearly limited profitability. Today, a lot of APL developers make a living working in one-man companies selling consulting – and keeping very busy educating themselves (or driving a bus) in-between jobs. There is still a good deal of money being made by APL-based businesses, but due to fragmentation it is much less clear how “community work” can enhance productivity.

However, for the reasons I mentioned at the start of this article, I believe we are reaching a point where the use of libraries for interfacing is becoming critical, and the lack of new APL blood is seriously limiting the ability (or willingness) of many organisations to use APL to solve problems. We need to act now!

## Target Audience for the Library

I believe that the library project has three quite different target audiences:

- People who are learning (about) APL. If it contains well-documented examples of good APL code, the library will act as a natural extension to training materials.
- Developers starting new projects. Our library should be of a quality which makes it a natural choice to replace the CLEAR WS as the starting point for new APL projects.
- Developers with existing applications. We must provide interfaces to new protocols, storage mechanisms and data formats as they are taken into use.

### Benefits

If you are learning APL, the library will help you learn to write good APL. The libraries will make you productive in a shorter time, and you will be more likely to succeed in learning APL at all. As we all know, this will help you deliver more flexible solutions fast. If you are starting a new APL-based project, the library will help you get started more quickly. As the library routines become known to many APL developers, it will be easier to move people to new projects, since there is more common knowledge. If we succeed in increasing the number of APL developers, the use of APL becomes safer, more politically correct and more cost-effective.

However, I would maintain the most important reason why it is now critical that the APL community develop some sort of library culture, is the interface requirements which are forced upon modern commercial applications. Although some large APL organizations can afford the cost at this point in time, it is increasing. I believe that, in order to ensure our long term survival, the APL community must learn to share the cost of development of code to support industry standard interfaces and data formats – or simply the cost of acquiring knowledge about the interfaces developed by users of other languages, and how to use them effectively from APL.

## Content

To succeed, we must be pragmatic and set realistic goals. APL is most profitably employed to do rapid prototyping or to develop systems (or components) which must mutate rapidly to adapt to new market conditions. We will never use libraries in the same way as a C++ developer. I propose that we should set the following short-term goals for our APL library project, more or less corresponding to the three target groups:

<ol type="disc">
  <li><b>Education:</b> Provide an introduction to newcomers to APL. Help teach good APL coding practices, in connection with literature (on the web). The production of training materials as such is beyond the scope of the Library project, but we must ensure that our efforts to meet other goals are co-ordinated with the production of new materials.

  </li><li><b>Utilities/Idioms:</b> Provide standard and high quality (correct, robust, fast) versions of commonly used algorithms, from idioms to slightly more complex code (row to row iota, etc.). These can be written in APL or implemented using external calls to C or other tools.

  </li><li><b>Interfaces:</b> Significantly reduce the cost of writing APL applications by providing interfaces to (external libraries which provide interfaces to) the most common facilities such as databases, messaging systems, XML.

</li></ol>

If we achieve the above, we can consider extending our goals to include the following:

<ol type="disc">

  <li><b>Community:</b> Help form the backbone of a global community, and hopefully re-ignite the exchange of knowledge between users of array-oriented languages.

  </li><li><b>Foster Component Building:</b> In the future, I believe that the natural place for APL is as a component building tool. We need to educate the APL community about the use of libraries in other environments, in order that we can become purveyors of quality components to the rest of the development community.
  </li><li><b>Code Management:</b> Efficient use of library routines requires some standards for “linking and loading”. It would be nice if we could replace or extend the myriad of homegrown source code management systems with a universal standard. But I did say we should be pragmatic and realistic, and this may be more than we can manage.
</li></ol>

### Education

The BAA already has plans for a project to produce a modernised set of APL education materials to be made available via the internet. I propose that the Library project support this work by organising any code examples referenced in the training materials, and providing a collection of Utilities, Idioms and interface code which are documented in such a way that they provide a natural extension to the actual training materials.

### Utilities and Idioms

Although we questioned whether APL has a library culture, I actually believe that every APL programmer understands what we are talking about. Every application workspace contains a base of small functions which are used dozens or hundreds of times to perform very common operations for which APL contains no primitives. Over time, many of these functions have been promoted to primitives, or at least interpreter-optimised idioms. Our goal is to select the best versions of these utilities, clean them up, document them and provide complete groups of code, identical across different APL interpreters where possible.

Where external tools exist which render APL-code obsolete, we will build interfaces to such external computational libraries. The following list contains a few examples of utility functions which immediately come to mind, and is obviously not exhaustive:

<dl>
<dt><b>Simple Data Manipulation</b>
</dt><dd>String manipulation (justification, case conversion)
<dd>Seaching and lookup
<dd>Transformation between lists, matrices etc.
<dd>Formatting data

</dd></dd></dd></dd><dt><b>More Advanced Computational Tools</b>
</dt><dd>Dates and Time
<dd>Matrices; Inversion, Eigen Values, FFT, Statistics
<dd>Complex numbers

</dd></dd></dd><dt><b>Coding tools</b>
</dt><dd>Timing/profiling
<dd>Workspace searching/comparison
<dd>Calling tree analysis, name cross references
<dd>Array display, and editing
<dd>Tools for moving code and data between APL systems
</dd></dd></dd></dd></dd></dl>

### Interfaces

This part of the library will contain collections of code which are potentially more complex, and provide access to standard data formats or external resources which any modern application is going to be expected to support. While the Utility and Idiom part of the library is likely to remain relatively static, the interface library will be continuously evolving as new external resources become available. As such, it provides the greatest potential for payback to large development teams. Such teams are unlikely to replace existing utilities by a new set – the testing implications alone are enough to make this difficult. If our library can provide interfaces to new useful resources in a timely fashion, then even the largest and most professional teams can profit from shared development – at least as a source of research into uses of new technology as seen with an APL mindset.

Some interfaces will provide access to external computation libraries, to do things like statistical analysis, optimisation, linear programming – in cases where external libraries provide results faster, more accurately (or simply more “officially”), or (perish the thought!) with more flexibility than we care to write ourselves.

Others will provide access to operating system resources, such as a library to list, create, delete files and directories. We would hope to provide interfaces to database drivers, and to data formats like RTF, XML, bitmap files, Postscript, spreadsheets and other document formats.

We will provide a collection of GUI utilities to perform frequent operations like prompting for input in various ways (single/multiple selection), providing progress bars, file boxes, etc.

The library will contain code which implements client and server support for protocols like HTTP, FTP and SMTP.

## Contribute!

Before we ask anyone to write new code for the library, we would like to collect, select, “clean” (if necessary) and document the best code which is already in use.

Whether you are a one-man (or woman) consulting team, a member of a large development team, or work for a vendor of APL systems, please discuss the proposed library project with your colleagues and management. If you believe that you have code which might be relevant to any section of the library, and your company is willing to donate it, please submit it to us. If you have input which will help us understand what would make the library more useful to your organisation, please let us know.

Although clean, readable, well-documented code is preferred, all contributions are welcome, even if you believe that your code is likely to be used to document just how dire the situation is out there J. We are also very interested in descriptions of how source code is managed in contemporary APL development teams – in particular how utilities and interfaces are handled.

Once we have produced the first few sections in order to document how the library might work (and experiment a bit with delivery mechanisms), we will be looking for volunteers to take responsibility for sections of the library. Of course, you are welcome to volunteer now, if there is a particular section that you know you are the right person to manage!

I look forward to the day where APL library contributions outnumber the spam in my in-tray.

> Morten Kromberg  
> Librarian

***
