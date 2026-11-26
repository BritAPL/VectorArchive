---
vid: '10002740'
title: Performance of Large APL Systems
authors:
- Peter Cyriax
volume: '10'
issue: '4'
page: '50'
status: not online
issue_tab: n4-april-1994
---

Peter Cyriax
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 50](../10/4/VOL.10-NO.4-APRIL-1994.pdf#page=52)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.4</p>
<p>+ something to put yourlocals into alphabetical order, and remake yourline labels (if you like the L10, L20 variety) + something to browse the entire workspacefor a string or proper APL name.It will also replace (but unfortunatelyit doesit within quoted strings) on a proper namebasis. + something to show the bracket balance within complex lines of code.</p>
<p>That is just one section of the Toolkit. There is plenty of other good stuff, for example the coverage of date conversion is quite comprehensive,allowing you to work on Julian dates internally (the number of days since 4712BC), find out when Easter will be in 2025, and soon. It is definitely worth having a copy on your system, and making sure someone knowsaboutit and can publiciseit.</p>
<p>Performance of Large APL Systems by Peter Cyriax (working at Commercial Union) Whatdo I meanbya large system ?</p>
<p>+ many workspaces, manyusers,so... + no informal method offinding out whatis going on any more!</p>
<p>Where do you look to improve performance? Most people can optimise a piece of code, but what should you optimise? A tool like the APL*PLUS compiler is useless on its own; you needa large range oftools, of which the compiler is only one.</p>
<p>Background to Commercial Union sInstallation There are two coupled mainframes, each of which consists of 6 CPUs, each rated at 45 MIPS (approximately equal to a 300MHz PC). Provided theentire clusteris run below 84% load (which it always is) each APL user has a 300MHz PC available for his sole use! What he sees is a 3Mbyte workspace, a 4Gbyte local disk, and an enormous ADABAS database accessed via hot batch using the Natural query language.</p>
<p>Performance,as the user seesit, is dominated by line-speed. Everyone has enough CPU,solocal users(like Peter) see better than ¥2 second response, while remote users out in the branches can be down to 3-10sec when the networkis busy. Howeverin general terms:</p>
<p>It s not as pretty as a PC, butit is as fast, and for some jobsit s better</p>
<p>50</p>
<p>VECTOR Vol.10 No.4</p>
<p>WhyCare about Performance? Two reasons: (1) Cost, (2) Response time. In general, cost is what people on mainframes care about; PC people are more concerned aboutresponse time. So the DPcharge to the CU Life Departmentis the thing to attack! Movingoutto PCsis not an option, because the mainframeis where the datais, and the system is in constant use out in the branches answering real questions like ... if I terminatethis policy, how muchis it worth to me now? .</p>
<p>Roughly 10% of the total machine load is APL, so thereis plenty there to attack! In round terms, it costs the guy who holds the budget for APL £5M p.a. We look for 20% improvementsin code efficiency I only wish they'd paid me what I'd saved them!</p>
<p>Aside I believed that it was not possible to write code so badly that it would affect response time I was wrong,it can be done!</p>
<p>The Tools Thereare four subjects you need to address if you wantcodeto gofaster:</p>
<p>1. Attitude. You have to CARE!</p>
<p>2. Analysis. The workspaces are too big to browse by eye.</p>
<p>3. Design.If the logic is sufficiently bad, no codingefficiency will help.</p>
<p>4, Coding Techniques. For things which are computationally subtle. For example to find *hi*ho in a big text database. The routine we havewill search the Bible in 1 CPU second but it took a week to THINK aboutit!</p>
<p>First, the correct attitude to optimisation:</p>
<p>+ Don tdoit + Doit tomorrow + Go for speed » Correct results? Worrylater! Doonejob at a time!</p>
<p>As soon as you start mixing getting things to go quickly with getting them to run right you fail. Keep trying algorithms as long as they come out somewherenear.</p>
<p>Aside the rate at which volumesof data are increasing is still outrunning the increases in CPU power. Typically, when you process data you compare one set</p>
<p>51</p>
<p>VECTOR VoL10 No.4</p>
<p>with another, so the processing time goes up as the square of the data volume. Thetrueprice of resourcesis still going up.</p>
<p>Next, the analysis tools you will need. In particular we would like to know:</p>
<p>+ which users use the CPU(easy) + which applications use the CPU.This is much harder, (unless you organise some Trojan Horses in advance) + whichfunctions use the CPU (across 200 applications) + what functionsexist in any application + whichlines are doing the damage!</p>
<p>Whatyou are trying to do is construct a pair of graphs:</p>
<p>| How much [</p>
<p>noo &gt; } How often |</p>
<p>{ '' + ' { CPU used [| } in each | nOU&gt; 1 treenrene--- + t</p>
<p>Remember if you can hit 1%of the CPU, that s an ongoing £5,000 ongoing cost-reduction every year.</p>
<p>Aside in an insurance business your central operation requires datalifetimes of 20-30-40 years. You must process where the datais!</p>
<p>How do you figure out whatto hit (ie. how do you make those two charts)? It is impossible without a degree of system and code management which will:</p>
<p>+ record cumulative CPUonentry to and exit from each workspace + switch on OMONITORwithin applications on demand + record which utility functions are present in each workspace</p>
<p>52</p>
<p>VECTOR Vol.10 No.4</p>
<p>+ maintain mastercopiesofutilities, which are shipped automatically when updated. There is an appalling function called GO in every workspace it has nasty names andcontainslots of horrid code, butit does the business!</p>
<p>Unless you do these things, you might be able to optimise a workspace; you will never optimise the whole system.</p>
<p>What can you do about Design? A hugeportionofall applications is utility code, so if we optimise that we can write highly optimal applications without having to think. Don t optimise application code, it is too subject to changes in the specification.</p>
<p>Optimising Code thelast stage.</p>
<p>+ you do need a reliable CPU monitor. + learn by experience. + THINK! You want the biggest element is a big numeric vector; 1+ AC 4A] maylook neat, but it will not be fast (why?). + when you need scalar looping code (whichis sometimes unavoidable) get into some language whichis tuned to do this. This is where the APL compilerfits. + use assembler FASTFWSif you have them, but hide themin your own utilities and make sure everyoneis using them.</p>
<p>Some Examples Here are five systems which have beenattackedat various times over the years:</p>
<p>1. PEP whichwrites a few letters. Finding out whatto attack! 2. unnamed to protect the guilty. The design was so bad thatit had to be rebuilt. 3. IPSZ,a utility to take a long list of +ve numbers and break them into clumps such than none exceeds a givenvalue.A classic useof this is wrapping text into a given page width. Just what you need a compilerfor. 4. Making a useful [FI for column matrices. 5. Various methodsof readingfiles, particularly the variable-length kind.</p>
<p>First, let s look at PTP. Thefirst thing to do was to uncompileit and runit again. The system as a whole remained balanced, but the CPU increased by 65%. This wasinteresting because the data volumes were small, and typically the compiler roughly halves CPU. Was it GDDM?This came out at 30ms per screen access, and GDDMcalls accounted for only 25% of this not a major issue. Now let's look at print generation aha! 3 seconds of CPU to run 3 pages appalling!</p>
<p>53</p>
<p>VECTOR Vol.10 No.4</p>
<p>With some serious work this should improveat least 10 times, so now we know where we should concentrate ourefforts.</p>
<p>Nowlet's take a look at unnamed . This runs around 900 times a year. Up to Juneit averaged 13cpu sec per run; from Augustit is down to 2.5 sec and disk 1/O is also downbya factor of 4 so what wasit doing!</p>
<p>Component file</p>
<p>Basically, it submitted a batch extract and split out 50 fields into 50 separate file components, with someselection and ordering as it went. As originally writtenit read the flat file 50 times, compressing and indexing using the samecriteria at each pass. With little cunning and a scratch file, you compress and index once only, and (more to the point) read the original data once only!</p>
<p>Aside you really would not believe what end-users get up tol! There was no spec (OK, we're used to that), and the logic was uniformly distributed across the functions! In the end we synthesised a new structure from the code, and when we hadfinished over 90% of the code had simply vanished!</p>
<p>Text wrapping with IPSZ. This is quite a simple loop: chop off whatfits, write a line, chop off whatfits, write a line ... etc, All the sensible APL algorithmsare of the n? variety, so here is a great opportunity for the compiler. However you do needto do lots of horrid things to your code to keep the compiler happy:</p>
<p>N«''pok a Force N to be (clearly) scalar Sed A Unnecessary initialisation outside the main loop R«Np ... a Always prebuild result</p>
<p>Loop: Rte ... a Use indexed assignment into result</p>
<p>End: R«ItR A... and clip it at the end:</p>
<p>However, the resulting function doesstill look quite like APL, andit is a damn sight easier for an APL personto dothis than to rewrite functions in C (and then workout howto call them!).</p>
<p>Data validation with QUADFI. This executes character matrices on the assumptionthat there is one numberper row.It is a little more expensive than</p>
<p>54</p>
<p>VECTOR Vol.10 No.4</p>
<p>OFI, but does more work, asit correctly handles blank rows and other gunk in the data. A usefultrick: B+~!' MATIOTA CHMAT CHMAT+*B/CHMAT a Finds Blank rows fast</p>
<p>... and only do the DVI wherethedata is legitimate.</p>
<p>Finally, reading IBM files. Some of the tricks are old chestnuts, butjust in case you don t know about RECFM=U,I'dbettertell you!</p>
<p>The obvious loop: open DSV0, read, check, read, check ... close is SLOW, horribly slow, so what can you do? Well, checking the control variable takes the same time as reading the data variable, so don t doit (after the first time). As far as anyonecantell, the time to read a recordis totally independentof record size, so read blocks (up to 32K bytes) instead of 80-byte records and you get an immediate 400-fold reduction in CPU time. Oddly, this is what the TSO file AP does if you tell it you don t know what your record format is so do that and just trim the tail and reshape afterwards ( cos you knew that they were really 80- bytes, didn t you!)</p>
<p>The fun starts if you have variable-length records! We gave up (wrongly) on these for a long time, then someonebuilt a little samplefile, pulled it into APL and examined it carefully. It turns out that there is a 4-byte length counter embeddedat the start of each record, so you can just grab the wholefile (as above) and chain downthe records. Clearly, there is no way of doing this in parallel, but it works very well with compiled code and saves us £1,000 per weekin CPU costs.</p>
<p>Reprise Attitude (you have to care), analysis (you need to know where to look), design (don t even try to optimise if the structure is too bad), and coding techniques (exploit the compiler if you haveit).</p>
<p>If you do this intelligently and thoroughly, a year-on-year reduction of 20% in the CPU cost of current systems is quite within your grasp.</p>
<p>55</p>
<p>_ APLPreferences V</p>
<p>APL.68000</p>
<p>Platforms Versions</p>
<p>RISC System/6000 © Level!</p>
<p>XH Xe Ww Apple Macintosh @© Leveilll</p>
<p>Commodore Amiga</p>
<p>Atari ST</p>
<p>APL for GUls</p>
<p>MicroAPL Ltd., South Bank Technopark, 90 London Road, LONDONSE1 6LN, UK</p>
<p>Tel: 071 922 8866 Fax: 071 928 1006 Applelink: microapl Internet: microapl@applelink.apple.com</p>
<p>VECTOR Vol.10 No.4</p>
<p>(GENERAL ARTICLES This section of Vectoris intended forarticles about the applications of APL, and non-technical (ie. having a minimal content of actual program code) material aboutthe developmentofthe language.</p>
<p>In this issue, we have a plea from Kromberg and Gfeller for standardisation in the APL environment, and a proposal from Smith and Pearson which at least offers some hope that access to the Windows GUI can be commonacrossall major APLs.</p>
<p>Wealso have two applications: the paper from Sylvia Camacho addresses the tricky problem of finding matches in a database of names, given mis-spelled or confused information; the paper from ThomasOlsenis a complete example of a Windowsapplication programmedin J.</p>
<p>Finally, George MacLeod explores the limitations of Windowsas a high-quality graphics engine, and suggests some solutions.</p>
<p>57</p>
</details>
