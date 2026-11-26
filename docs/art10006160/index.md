---
vid: '10006160'
title: Literate Programming with J and noweb
authors:
- Martin Neitzel
volume: '16'
issue: '4'
page: '107'
status: not online
issue_tab: n4-april-2000
---

Martin Neitzel
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 107](../16/4/VOL.16-NO.4-APRIL-2000.pdf#page=109)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.16 No.4</p>
<p>Literate Programming with J and noweb by Martin Neitzel (neitzel@gaertner.de)</p>
<p>The integrated creation of program source and its documentation is called literate programming. This term goes back to Donald Knuth and his WEB system, a literate programming system for Pascal.</p>
<p>This article describes my first experiences with using Norman Ramsey s noweb system for J programming. Wealsotake a look at a few other approachesto create nice representations of J code.</p>
<p>Introduction Perhapsinterestingly, I took my first look at noweb when I wrote my first Vector article. I already hadall the J coding done, and I just wanted to documentit for Vector readers. Rather than starting a new text document and including some source snippets via cut and paste, I extended the original J script file with additional text material.</p>
<p>Keeping the source code and its documentationin the samefile is the central idea behind literate programming. This way it becomes easy to maintain these two aspects of a program consistently. As the old adage says: nothing is more misleading than outdated comments and incorrect documentation. By tying the code andall its documentation closely together, literate programming makesit easier to keep both in syne.</p>
<p>The unified source is then filtered in different ways for different purposes. One can either extract the program code for the compileror, as is the case with J, the interpreter; or, alternatively, pull out the documentation with embedded code fragments to be nicely typeset.</p>
<p>Nowebis a freely available WEB cousin written by Norman Ramsey. The website http:/ /www.eecs.harvard.edu/~nr/noweb/ is the home page of the system. Noweb s main characteristics are simplicity and language-independency. It is available for all J platforms and already comesas an installable package with all of the free Unix/Linux distributions. All in all, enough reasons for me togiveit a serioustry with a few small J projects.</p>
<p>107</p>
<p>VECTOR Vol.16 No.4</p>
<p>The following are the main tools of the noweb system:</p>
<p>* notangle extracts just the source code proper from theliterate program, ie. aJ script.</p>
<p>* noweave extracts documentation with embedded code prepared for some typesetting program such as TeX/LaTex,troff, or an HTML browser;it optionally creates and formats cross references along the way.</p>
<p>Noweb supports a handful of programming languages and typesetting backends outof the box. For use with J, I provided a very simple recogniser for declarations in order to get the cross referencer working. One could give explicit hints with each code chunk about the defined objects but an automatic recogniser is much more convenient.</p>
<p>Since I deemed only global definitions at the module level worthwhile to be indexed, this was easy. A single regular expression was all I needed, namely Afa-zA-Z0-9_]+ =: . This pattern matchesall assignments to J names at the beginning of a line. I always indent J sentences within a verb definition, a style that in my opinion makes a script more digestible for both human readers and. such simplistic analysers alike.</p>
<p>Noweb s way of integration of such tools is straightforward. One just needs to providea textfilter operating on a commonintermediate format.</p>
<p>Allin all, a 20-line awk script takes care of definition recognition, index collection, and some other amenities like using &lt;&lt;&gt;&gt;= for the continuation of the previous code chunk withoutspelling out its name again.</p>
<p>A Simple Example</p>
<p>Input (aka: The Source) Here is a simple example.First, a noweb inputfile with some documentedJ code, presumably stored as stat .nw:</p>
<p>&lt;&lt;statistics&gt;&gt;= &lt;&lt;auxiliaries&gt;&gt; &lt;&lt;averages&gt;&gt; @ Averages come in various forms. There is the well-known [[mean]] function, for a start: &lt;&lt;averages&gt;&gt;= mean=:sum % #</p>
<p>108</p>
<p>VECTOR Vol.16 No.4</p>
<p>@ The geometric mean is another one: &lt;&lt;averages&gt;&gt;= geomean=: # %: prod @ This works with the help of some aggregating functions: &lt;&lt;auxiliaries&gt;&gt;= sum=: +/ prods: */</p>
<p>See how code chunksstart or are continued with names in double angle brackets. Documentation modeis entered with a single @ character. Embedded code in documentation is written in double square brackets.</p>
<p>Output 1: The Code The command</p>
<p>hotangle ~t8 -Rstatistics -filter emptydefn stat.nw &gt; stat.ijs</p>
<p>will extract the J script file stat. ijs. It will look like this:</p>
<p>Mean#=:sum % # geomean=: # %: prod</p>
<p>The ~Rstatistics option tells noweave to start extraction at the root node statistics. This node is then expanded from its arbitrarily large tree of sub nodes. The important things to note aboutthe nodes are:</p>
<p>1. You can extract different code parts, i.e. different J scripts, from one and the same nowebsource.</p>
<p>2. The orderof the extracted code can very well be different from the order in the nowebsource whichis typically the one deemed mostsuitable for a lucid presentation.</p>
<p>Output 2: The Documentation The command</p>
<p>noweave -html stat.nw &gt; stat.html creates the documentation variantof thefile. It can generate TeX, LaTeX, troff, and HTML markupoutof the box,</p>
<p>109</p>
<p>VECTOR Vol.16 No.4</p>
<p>Whenviewed with your browser, you'd see roughly the following, including the font changes:</p>
<p>&lt;statistics&gt;= sauxiltaries&gt; &lt;averages&gt;</p>
<p>Averages comein various forms. Thereis the well-known mean function,for a start:</p>
<p>&lt;averages&gt;= mean=:sum % #</p>
<p>The geometric meanis another one:</p>
<p>&lt;averages&gt;+= geomean=: # 4%: prod</p>
<p>This works with the help of some aggregating functions:</p>
<p>&lt;auxiliaries&gt;= sum=: +/ prods: «/</p>
<p>The same command with a few additional options will create hyperjumps and indexes for chunk names anddefinitions:</p>
<p>noweave -html -filter autodefs.ijs -index stat.nw &gt; stat.html</p>
<p>Check out http://juggle.gaertner.de/bnp/litprog-hml to experience that version ofthis article. Manyreferences created by noweave are marked as U or D , pointing to uses anddefinitions of the named code nodesor global J names.</p>
<p>The text sections in a noweb source can also use any markup commands appropriate for the noweave backend used.If you wantto have chapter headings or bulleted lists in your documentation,this is the way to go.</p>
<p>With the output generated for the heavy-duty typesetting programs LaTeX or troff, you can makea high-quality booklet or even tome. The cross-references will go into the page margin, of course with the proper pagereferences. 1 stumbled over noweb whenI read the following gem: A Retargetable C Compiler: Design and Implementation by C. Fraser and D. Hanson - 592 pages noweb-based</p>
<p>110</p>
<p>VECTOR Vol.16 No.4</p>
<p>compiler source and documentation in finest typography. The PDF for a sample chapter can be downloaded from the Lec homepage:</p>
<p>hitp:/ / www.cs.princeton.edu/software/lcc/</p>
<p>Nitty-gritty Details Knuth s original WEB tool wastailored to Pascal and TeX.(It wasactually written to overcome some of Pascal s shortcomings: the lack of a module system and the very strict order of definitions in a standard Pascal program.) The good thing about nowebis thatit can be used with any source language and generate output for different text processing systems. Whilethe J code excerpt shown aboveis digestible by J, you don t wantto touch it ever again. For example, if you have to fix a bug, you should do this in the stat.nw file, not in stat.ijs produced by notangle. Likewise, there is little use to put stat. ijs under version control.</p>
<p>I have therefore written my little Load variant Loadnw which notangles a script on the fly and loads the output:</p>
<p>&lt;loader&gt;= NB. [chunkname] Loadnw filename NB. *loads a node from a noweb file. loadnw_z_ =: verb define "*" Loadnw y.</p>
<p>01:100 (21:0) 'notangle -t8 ' -R', x., '' -filter emptydefn</p>
<p>The chunk name * used as default left argument here is noweb s default root node. You'll often find nowebfiles just containing one or more &lt;&lt;*&gt;&gt; chunks and no further sub-nodes.</p>
<p>The option to extract different code parts from a script also comes in very handy for test code, Users of the library just want to get the definitions from the script. But when I work on thescript, it helps to have an additional test chunk executed which prepares and runs some initial tests. The library code is defined as a subordinate node to the test node so that I can choose which version to extract from the script by specifying the corresponding root node. Nowebis effectively taking therole ofa classic conditional preprocessor whenusedin this style.</p>
<p>111</p>
<p>VECTOR Vol.16 No.4</p>
<p>Unfortunately, J will report errors with a line count referring just to the extracted code. It would be really helpful if the line number could refer properly to the noweb source file. After all, this is where the fix has to be made. Noweb can indicate the proper source coordinates in its notangled output. By default, it uses C preprocessorstyle #Line 123 stat.nw directives but it can generate any other syntax as appropriate for the programming language used. It would be really helpful if J could take such a hint- not just for working with noweb but for any other code generator as well.</p>
<p>The nountanglLe utility is a variant of notangLe. It creates a program, too, but keeps the full documentation as block comments in the extracted code. NountangLe neededto be extended to support J s NB. syntax. As luck hasit,it is a shell script on Unix, so this was very simple to arrange.</p>
<p>Comparisons with Other Approaches</p>
<p>Plain Old Comments Lines prefixed with NB .s are easy to ignore in small doses. Beyond a dozenlines, the letters can begin to irritate readers. Longer documentation also benefits from decent typesetting. After all, you seldom see a Vector article only consisting of NB ed code.</p>
<p>Just for improvedreadability, I ve seen some people using literate noun scripts to include one or more paragraphs of commentsin a script. This runsas follows:</p>
<p>0:0 todayno: converts dates to day numbers. Method is to subtract dates on a calendar basis to determine integral number of months plus the exact number of days remaining. This is converted to payment periods, where # days remaining are calculated as: (# days)%365 )</p>
<p>The script containing the notes is simply parsed and immediately discarded again. Without any leading NB. letters, it is readable with less distraction than ordinary commentblocks.</p>
<p>Scriptdoc J/Windows comes with the scriptdoc utility. It will extract traditional NB. comments written according to some scriptdoc-specific markup conventions.</p>
<p>112</p>
<p>VECTOR Vol.16 No.4</p>
<p>Basically, you just have to put an asterisk and a one-letter code in a strategic place.</p>
<p>The comments list the namesof the routines, their typical use, and perhaps some notes on their implementation. It is scr iptdoc s job to extract this info and create a nice overview. This can then be used as a quick reference or manual.</p>
<p>Scriptdoc s job is to suppress implementation details. The outputis usually biased towards the users of a library script. Noweb in tum targets other developers working on the code which is includedin its entirety.</p>
<p>HITML-Publish J/ Windows Release 4.03 comes with a code-to-HTMLconverter. It uses syntax- based colouringto create nice output according to user-definable colour schemes. One would typically use HTML-Publish to save the generated output to a separate web page which could then be viewed by others.</p>
<p>vgrind Vgrind is a traditional pretty-printing tool for the Unix nroff/troff typesetting system. Code fragments are embedded in the text within .vS/.vE start and end lines.</p>
<p>Vgrind will then insert the appropriate font change instructions to mark keywords, constants, and comments.It detects these elements guided by a short language description defining somepatterns. The patterns will also enable vgrind to collect an index for whateverlooks like a procedure definition . You can find sucha vgrind definition for J on juggle.gaertner.de .</p>
<p>The mixed documentation/code approach is not unlike the style used in noweb sources, but I am not aware that people ever used this system for literate programming. Thatis, I never saw the practice or recommendationto extractjust the code for further processing. Most often vgrind is simply run on original code files without any additional text or .vS/.vE delimiters, with the pretty-printed output directly sent to the printer. This use is not much different from HTML Publish .</p>
<p>TheJ Project Manager The J Project Manager is not a tool concerned with documentation in any way. There is a reason why I am includingit in this short list of comparisons: both the</p>
<p>113</p>
<p>VECTOR Vol.16 No.4</p>
<p>Project Manager and noweb can be used to create J scripts to be shipped to a customer.</p>
<p>The Project Manager usually takes the J script files belonging to the project and merges them together with the required standard library scripts into onesingle J script executable by a J Runtime system. Even when thecode is not encrypted, the customerwill have a hard time to exploit the code on his own:all comments have been stripped off, and the organisation as one huge script doesn t exactly invite you to make any changes.If you wantto convince yourself, try to modify one of the PM-generatedscripts in the standard library distributed with the system. You will quickly recognise thatthis is no fun atall.</p>
<p>Noweb s notangle can be used to the same effect: as a developer, you can maintain the code nicely organised and including excellent documentation. When you ship it off-site, you have the option whether you want to include the documentation (converted into classic NB. comments) and whether you want to keepthe script organisation modularor not. Another typical task of the PM is to compile a number of source scripts and referencedlibrary scripts into a single runtime script. With noweb, you have some freedom how noweb inputfiles relate to extractedJ scripts. It is up to you which nodes you will extract into a script file. You could distil several script files from one nowebfile, keep a one-to-one relationship, or collect the code from different nowebfiles into a single script.</p>
<p>Summary Using noweb with J was simple to arrange, easy to learn, and straightforward to use. WhatI appreciate most in noweb is its minimal-fuzz approach to switching between code and documentation.I fell in love with the [L[embedded code]] markers which are so much less obtrusive to my eyes than the \verb'foo' or &lt;code&gt;bar &lt;/code&gt; or \fIbaz\fP markups known from other systems I used before to write articles.</p>
<p>Tam a bit unsure at this moment where to draw the line between NB. comments as part of the code as opposed to comments as part of the additional documentation. My gut feeling is that only little paragraph headers and small hints at the coding level would remain as NB. comments.</p>
<p>haven't used noweb beyond the toy level yet. My biggest nowebbed project has a whoppingthreeJ scripts. It yet remains to be seen whether programmers with Real Projects benefit from noweb. Would a developer team be happier one day</p>
<p>1144</p>
<p>VECTOR Vol.16 No.4</p>
<p>with jumping around in the HTMLrendition than with, say, traditional tags in the editor? The functionality is the same, but many people value a visually nice appearance highly. Other topics of my future investigations will be. noweb s interaction with other supporting tools. For example, this article is written with noweb,too, [Ed: to produce HTML: the HTML-to-Word conversion was done the old- fashioned way...]. It is also version-controlled, and its most current HTMLrendition as well as its output scripts should be available on the juggle server anytime.</p>
<p>One thingis certainly clear: nowebis fine for writing articles on J. | always have to improveorfix my code when writing an article. With noweb, I am doing the corrections in a single place and I can be sure that the codeprinted in thearticle will actually run and not be ruined by any transcription errors.</p>
<p>Acknowledgements I thank Gill Smith and Christian Lindig for very helpful comments on drafts of this article.</p>
<p>115</p>
</details>
