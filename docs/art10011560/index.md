---
vid: '10011560'
title: 'Review: System Building with APL+Win, by Ajay Askoolum'
prefix: Review
authors:
- Ian Clark
byline: Ian Clarkearthspot2000@hotmail.com
volume: '23'
issue: '1'
page: '63'
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
source: trad/v231/sysbldgrvw.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- reviews
- system interfaces
- user interfaces
---

Ian Clark  
[earthspot2000@hotmail.com](mailto:earthspot2000@hotmail.com)
{ .byline }

This book is about building “systems” using the APL+ interpreter sold by APL2000 Inc. to operate within the Microsoft Windows environment. “System” here means “application”, not “operating system” as a developer might at first suppose. According to the Oxford English Dictionary a (computing) system is “a group of related hardware units or programs or both, especially when dedicated to a single application”—which is exactly how the author means it.

From a developer’s point-of-view, MS Windows is a junk-box of parts from which the dextrous builder can, with modest effort, throw together a prototype, and with considerably more effort get it into a state fit to ship. By contrast the APL environment with CLEAR WS is highly generalised and tightly integrated: everything works with everything else every-which-way—and has done for the last 40 years. This enables you, the experienced APLer, to build functions from scratch with confidence and despatch. It also offers a strong disincentive to bolting-on ready-made components. Doing so is often far less satisfying than anything you might write yourself in APL, because you have to spend time getting used to each component’s funny little ways.

However, what seems satisfactory to you during the process of development can appear less so to your customers when they come to adapt the application at some future date—and to the programmers they employ to do the job. The author approaches the topic very much from the point-of-view of a maintenance programmer. He has an understandably sour view of “legacy code”: that is, old code built out of matchsticks (my term, not his). Matchstick systems soon looked dated and are not easily upgraded without considerable redesign. They can’t take advantage of improvements to the platform. Conversely applications written to use Windows components reap the benefits from upgrades to those components free of charge and (generally) without pain.

The standard Windows application, i.e. one written in Visual Basic (VB), is largely stitched together from ready-made components. And that’s all VB is good for, say its detractors. But Microsoft have put hundreds of man-years into tuning it up for that purpose, smoothly extending the language with dotted identifiers to handle the methods and properties of ready-made components and, most significantly, equipping it with a massive library of component documentation: exposition, worked examples and “IntelliSense”, all comprising a system of on-line help that is manifestly better-integrated than the assorted components of Windows itself.

The Windows gospel is that third-party software products to run on Windows should look as far as possible like extensions to the operating system, all using the same screen furniture and data access methods. The end-user is meant to leverage this to switch easily between different vendors’ applications. Customers expect the Windows look-and-feel to be honoured as a mark of software quality, even if they don’t necessarily like it. The book fulfils a real need in this regard, one which the reviewer too has spent years of his precious life in the service of.

The author has a message: legacy code is locked in the past and is a burden to maintain. “Real” Windows apps must call Windows DLLs for all essential services which the user can possibly see: screen interaction, national settings like decimal point/comma and thousands separator, date and time manipulation, data access. But, as the book makes clear, “real” Windows apps aren’t the sole preserve of the MS Visual Studio stable of languages which includes VB. They can be written in APL—and here’s how to do it.

One nevertheless has to accept that VB is the vendor’s flagship language. More pertinently it is the reference language for all function-header descriptions and coding samples in Microsoft’s online Help. All other languages feast in a sense on these crumbs from the rich man’s table.

The author is at pains to promote APL+Win as at least as good a medium for coding COM as VB. This is a lost cause, as the author himself accepts at times: it challenges VB on its strongest territory. Exhibiting examples side-by-side of corresponding COM calls in VB and APL+Win like this:

```
VB:        Set objXL = CreateObject("Excel.Application")
APL+Win:   'objXL' ⎕WI 'Create' 'Excel.Application'
```

not only sends the message that VB is in its element, maintaining the illusion that COM is a natural extension to the Basic language, but that the APL+Win programmer can’t help coding through a keyhole: a `⎕WI`-hole, you might call it. The `⎕WI`-hole actually offers an occasional advantage. Sometimes you badly need to see where COM stops and user-code begins. Generally this is in code written by somebody else. VB blurs that boundary, but APL+Win emphasises it: the user-command `]WSLOC` reliably listing every `⎕WI` in your workspace.

Nevertheless the Windows interface of APL+Win is in all honesty a distressing one. Even the author departs from loyalty to the cause to complain about “opaque APL syntax” (p157). But it’s an interface which has made me a good living in the past—so who am I to kick against the pricks?

So in me the author is preaching to the converted, albeit forcibly so. I approached the book as a professional, hoping to improve my Windows skills—and I was not disappointed. The author delivers the one thing that APL+Win badly needs to be a serious alternative to VB: copious samples of tested working code, accompanied by practically-minded explanations. He presents the major Windows components (both COM and older DLLs executed via `⎕WCALL`) in the form of APL cover functions: which proves to be the most efficient way to deploy this knowledge. My copy is going to get well-thumbed.

I looked in vain for a CD of working code inside the back cover. The Internet has made all that obsolete: instead you get a URL to the publisher’s site which lets you download a free zip-file of miscellaneous data-access bits-and-pieces, plus the thing you want: an APL+Win workspace (`SBWA`) containing all the cover functions described in the text. This way the reader gets the latest revisions of working code: no more out-of-date CDs. However there is still some way to go before `SBWA` looks presentable: the version I downloaded had been saved with a suspended function (viz. the introductory fn, itself called `SBWA`).

The book (plus working code) earned its £40 cover price for me in the first hour of use. The author had solved a problem which I was planning to tackle in APL+Win: how to run a spelling check on data input into my app. I consider myself a reasonably competent APL+Win coder, but the author’s solution amazed me with its simplicity. I would have imagined it necessary to create a document on disk of the material to be checked—but no: you can hold the target text in an APL variable and put it into the `Selection.text` property of Word.

This example alone made me very satisfied with my acquisition—but it continued in the same vein, teaching me a host of things I didn’t know about Windows, confirming things I knew (or suspected) about COM and delivering it all as working code which I could use immediately.

To give a flavour of what’s on the menu, there are chapters on working with Excel, Word, Access, ADO, SQL and VB itself—in various COM positions. In a chapter entitled Working with Windows you find out about Date/Time formatting, managing the Registry, creating a shortcut on the Desktop, app configuration with INI and XML, and reliably managing files on disk and their folders. The author avoids an encyclopaedic approach (which would have combinatorially exploded) in favour of telling you just what you need to know about everything (and only) what matters. Considering how much room for debate there is in that, I was struck by how far I found myself in agreement over what was, and was not, important. Though how to tackle the issues the author correctly identifies gives plenty of scope for debate. For which, alas, there is little room here.

Whilst predominantly about APL+Win, the author mentions other major contemporary APLs: APL2, APLX, APL/W and discusses their special strengths, identifying that of APL+Win as lying in the COM area. Having done serious work with two of the other three APLs, I would wholeheartedly agree, the reason being the APL+ history of developing the “`⎕WI`-hole” as a schematic to parallel what VB can achieve with GUI coding, to the extent of being able to leverage VB’s own documentation and worked examples (up to a point). Since COM is in effect a generalisation and standardisation of the VBX interface for the benefit of non-VB languages, `⎕WI` extends easily into COM. How far it succeeds in this approach and where it falls short is something the book usefully discusses—naturally enough, for it is the author’s main concern in writing it.

But primarily it is the code samples which give the book its value: functions which work first-go without you having to be creative in an unfamiliar area. If you already know APL, this book turns the tables on VB as the language of choice for building pukka Windows apps, downgrading it to language of last resort.

So where could I have hoped for more?

I looked in vain for coverage of internet access using `⎕NI`. A cover-function to download a file from a URL would have been really nice, one to upload even nicer. This is not to minimise the difficulty of coding `⎕NI` to engage in a FTP session with a remote site. Perhaps the author thinks it is better to buy one of the available tried-and-tested COM DLLs for the purpose than to call `⎕NI`? I do sympathise.

I felt the author was too laid-back about communicating with COM-objects, especially Excel. `0 0⍴⎕WI⍵` is a little bit sanguine. Though of course sample code is clearer when it is not cluttered with diagnostics. Nevertheless, problems with Excel as an ActiveX tend to appear far down the line (generally the Sunday night before code release). The day may come when you’ll need to trace that code and look at what’s returned by `⎕WI⍵`.

A slick code-hook for diagnostics is what is needed. The author should give some space to the time-wasting problems which arise from unwise choice of convention in dealing with returned values: especially those you think you’re never going to need. APL2000 in-house tends to use:

```
    ∇ SINK ship ∇
```

so that `0 0 ⍴⎕WI⍵` can become: `SINK ⎕WI⍵`. Besides being faster, this has the forensic merit of holding the ship for inspection at some `⎕STOP`-able point before it sinks without trace, and even of being substitutable at need by:

```
    ∇ SINK Z
[1]   →(z≡expected1)/0 ⍝--the most frequent instance
[2]   →(z≡expected2)/0 ⍝--the next most frequent
[3]   →(z≡expected3)/0 ⍝--etc
[4]   ⎕SAVE'DUMP'
    ∇
```

You should then arrange for the runtime initialisation function to fix the simplest form of `SINK`, thus avoiding swabs left in the patient.

Code hygiene, like fire safety, is all about making the improbable impossible. Likelihood must be factored by Regret. Some coding strategies simply farm bugs. Experience alone shows which these are. Gerry Weinberg’s “Worst Bug Ever” \[[2](#ref2)\] records how thousands of man-hours were once wasted on an elusive bug that turned out to be a certain memory-location labelled ONE occasionally containing something else, viz. 2. Ah, those good old days of Assembler coding! But isn’t APL, with no sensible facility for defining a constant, prey to the very same gremlin? The author warns us against using globals as constants. I applaud his recommendation to define constants as niladic functions: I thought I was the only person who did this routinely. For instance:

```
    ∇ z←ok
[1]   z←0
    ∇
```

for use like this:

```
→(ok=↑⎕WI ...)/LABEL_OK
```

Which highlights another problem concerning return codes: there are two conflicting popular conventions—a fact which digs no end of elephant traps in little-used code:

- 0=ok; 1=Error#1, 2=Error#2… etc. (as above)
- (1,xxx)=ok; (0,n)=Error#n

So when you come across `:If ~↑⎕WI` … what does it really signify? The construct `:If ok=↑⎕WI` … does at least make clear the programmer’s assumptions about what is being returned and what it means.

Some argue that such declarative information is best recorded in a comment because it entails zero execution overhead. But maintenance programmers frequently alter code in disregard of comments, which may then no longer apply. They argue that the interpreter doesn’t read the comment, so why should they? Self-documenting code does have its merits, in spite of having 99 different names for 0, not to mention 1.

To those who have never had to spend their weekends tracing bugs to a deadline (and see old friends cropping up time and again) these precautions must look utterly paranoid. But the heart of good programming practice surely is this. Modern programming techniques stress “rapid application development”, which is taken to mean ever-quicker ways to cobble-up a prototype, oblivious of the trouble being stored-up for the future. VB is especially bad in this: it is deceptively rapid in the initial stages of development—but it makes you pay for it all later.

VB is oversold for serious development work, particularly where COM-objects are involved. Where, for instance, does this ubiquitous VB statement have any place in professional code?…

```
  On Error Resume Next
```

…It’s Chernobyl programming! Yet the author shows you how to emulate it in APL+Win, using `⎕ELX` and `→⎕LC+1`. (I must confess, though, to having done so myself.) Tellingly the topic comes up in the chapter on working with ADO.

The time spent debugging a professional product often grossly exceeds that spent on coding the first draft. So why all this emphasis on coding speed nowadays? Are we developers (or more likely our managers) simply being sold snake-oil by the likes of Microsoft? There is a case for sacrificing some of that heady starting velocity, to recover it with interest down the line by squashing bugs known to be huge time-wasters. I would have liked to see more attention given to this matter.

One fault in the book, serious enough in my opinion to fix with an erratum sheet, is corrupt page-numbering in the index. It is a Cinderella of an index, overshadowed by the detailed 3-level table of contents. However every sample APL function is listed here alphabetically: a potentially useful facility. A corrected index can be downloaded from the same URL as the code-samples.

But these are minor grumbles—merely a wishlist for the second edition, which I hope and expect will appear shortly, to clue us APL+Win users up on .NET.

Apart from the function list in the index, the table of contents is better for finding things than the index itself: you can see a link to this at \[[1](#ref1)\]. I haven’t covered all the good things in the book and it’s well worth taking a look here to get an idea of what I’ve not had space for. Notably the first two chapters: a scholarly overview of system-building and a useful portfolio of APL techniques to help you build a seaworthy product. Outright professionals may feel they don’t need to know all this (but will still prize the book for the sample code). But trainees might well find these chapters immensely valuable: it is hard practical advice which you simply won’t find elsewhere. The more product development experience you get, the more you’ll come to appreciate how right the author is.

To sum up: this book fills a sizeable hole in the armour of APL+Win when it comes to do battle with VB, namely in the provision of meaty worked examples of ActiveX interfacing and Windows DLL calls generally (at which not even VB is well-documented!) Worth its price for the time it’ll save you—even at floor-cleaning rates of pay—and a must-have for any “APL+Winner’s” bookshelf.

## References

1. Ajay Askoolum, *System Building with APL+Win*, Wiley, 2006, ISBN 0470030208  <a id="ref1"></a>[http://eu.wiley.com/WileyCDA/WileyTitle/productCd-0470030208.html](http://eu.wiley.com/WileyCDA/WileyTitle/productCd-0470030208.html)
2. Gerald M. Weinberg,  <a id="ref2"></a>*The Psychology of Computer Programming*,  Coriolis, 1988, ISBN 0442207646
