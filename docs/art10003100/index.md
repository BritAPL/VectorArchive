---
vid: '10003100'
title: 'Meeting: The Toronto Toolkit'
authors:
- David Eastwood
byline: by David Eastwood & Peter Cyriax
volume: '10'
issue: '4'
page: '48'
source: trad/v104/eastwood104_48.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- conference reports
- programming techniques
- performance
---

by David Eastwood & Peter Cyriax <span class="affil"></span> <span class="email"></span>
{ .byline }

*reported by Adrian Smith*

### Introduction

Two contrasting talks, both interesting! David Eastwood gave us an overview of the Toronto APL Toolkit (now 11 years old, and still going strong) and Peter Cyriax took us carefully through the steps needed to improve the performance of large APL systems.

As a reporter (and a member of the Vector working group) I would be very interested to hear readers’ reactions to the following proposition: I feel that we may be rather neglecting the mainframe, because it is old, boring and just generally part of the scenery. However, what Peter made very clear in his talk is that, on the mainframe, APL is still unique. It is one of the very few interactive environments available, it is pretty good at spreadsheets, it does very competent graphics, it can handle reasonably large amounts of data very quickly, and it is better than COBOL at doing hard sums (particularly if the sums involve floating point). If you are stuck on the mainframe (because that is where your data is), and you want to do any of the above, you will very likely use APL.

Does the mainframe readership of Vector feel alienated by all this Windows and Gui stuff? I think that there’s life in the old dog yet, and we should probably make a positive effort to get better mainframe coverage. Please let any of us know if you agree.

### The Toronto Toolkit

*outlined by David Eastwood*

Essentially, this consists of generic VS APL code, freely distributed in electronic form for more or less every APL you can name. It has been running for 11 years, originally in book form, and has been available on disk since 1992.

### Why use a Toolkit?

Sometimes you just need to cover a gap in the interpreter. If you are used to `⎕DBR` (delimited blank removal) on APL.68000, or `⎕SS` (string search) on APL\*PLUS/PC, and you move to Dyalog APL (which has neither) you need functions to fill the holes.

Sometimes you want to do something hard, like subtracting two calendar dates, which is a problem that has been solved many times before. An old (=reliable) utility will save you plenty of thinking and testing.

Platform independence has its attractions! Bulletin boards are well clogged with “how do I get this code from here → here?” questions.

It helps to teach you good APL. The people who wrote this stuff knew what they were at, and knew that it was going into the public domain. They cared about quality, and clarity, and readability. In answer to the question “where do I learn about good APL” you can reply “look at the toolkit functions, then copy and modify”.

### Observations

Setup is easy. Everything is `⎕IO` independent, so all you need to do is add your own error trapping and set up one global `∆CR` to tell the functions what your ‘end of line’ character is.

From this point, everything worked, and some of the functions are already out earning their keep in real systems. Two small points to note (for users of more modern APLs) are that there are no ‘default’ left arguments (i.e. no ambivalent functions), and that your particular APL may support many of the 158 supplied utilities already, so don’t just copy them all!

### Some Examples

There is a lot of stuff, so why not use its own ‘workspace browser’ code to explore ...

```
     ⍴⎕NL 3
158 15

     5 matacross ⎕NL 3    ⍝ to see a columnar list
     'ba*' nl 3           ⍝ wildcard list (3 across)

     explain 'nl'         ⍝ documents function 'nl'
```

You will find lots of handy stuff:

- a function lister which exdents comments and labels to order
- a global reference finder which tracks down rogue variables which should be local and aren’t
- something to put your locals into alphabetical order, and remake your line labels (if you like the L10, L20 variety)
- something to browse the entire workspace for a string or proper APL name. It will also replace (but unfortunately it does it within quoted strings) on a proper name basis.
- something to show the bracket balance within complex lines of code.

That is just one section of the Toolkit. There is plenty of other good stuff, for example the coverage of date conversion is quite comprehensive, allowing you to work on Julian dates internally (the number of days since 4712BC), find out when Easter will be in 2025, and so on. It is definitely worth having a copy on your system, and making sure someone knows about it and can publicise it.

### Performance of Large APL Systems

*by Peter Cyriax (working at Commercial Union)*

What do I mean by a “large system”?

- many workspaces, many users, so ...
- no informal method of finding out what is going on any more!

Where do you look to improve performance? Most people can optimise a piece of code, but what should you optimise? A tool like the APL\*PLUS compiler is useless on its own; you need a large range of tools, of which the compiler is only one.

### Background to Commercial Union’s Installation

There are two coupled mainframes, each of which consists of 6 CPUs, each rated at 45 MIPS (approximately equal to a 300MHz PC). Provided the entire cluster is run below 84% load (which it always is) each APL user has a 300MHz PC available for his sole use! What he sees is a 3Mbyte workspace, a 4Gbyte local disk, and an enormous ADABAS database accessed via ‘hot batch’ using the “Natural” query language.

Performance, as the user sees it, is dominated by line-speed. Everyone has enough CPU, so local users (like Peter) see better than ½ second response, while remote users out in the branches can be down to 3-10sec when the network is busy. However in general terms:

“It’s not as pretty as a PC, but it is as fast, and for some jobs it’s better”

### Why Care about Performance?

Two reasons: (1) Cost, (2) Response time. In general, cost is what people on mainframes care about; PC people are more concerned about response time. So – the DP charge to the CU Life Department is the thing to attack! Moving out to PCs is not an option, because the mainframe is where the data is, and the system is in constant use out in the branches answering real questions like “... if I terminate this policy, how much is it worth to me now?”.

Roughly 10% of the total machine load is APL, so there is plenty there to attack! In round terms, it costs the guy who holds the budget for APL £5M p.a. We look for 20% improvements in code efficiency – I only wish they’d paid me what I’d saved them!

Aside – “I believed that it was not possible to write code so badly that it would affect response time – I was wrong, it can be done!”

### The Tools

There are four subjects you need to address if you want code to go faster:

1. Attitude. You have to CARE!!
2. Analysis. The workspaces are too big to browse by eye.
3. Design. If the logic is sufficiently bad, no coding efficiency will help.
4. Coding Techniques. For things which are computationally subtle. For example to find ‘\*hi\*ho\*’ in a big text database. The routine we have will search the Bible in 1 CPU second – but it took a week to THINK about it!

First, the correct attitude to optimisation:

- Don’t do it
- Do it tomorrow
- Go for speed
- Correct results? Worry later!

Do one job at a time!

As soon as you start mixing “getting things to go quickly” with “getting them to run right” – you fail. Keep trying algorithms as long as they come out somewhere near.

Aside – the rate at which volumes of data are increasing is still outrunning the increases in CPU power. Typically, when you process data you compare one set with another, so the processing time goes up as the square of the data volume. The true price of resources is still going up.

Next, the analysis tools you will need. In particular we would like to know:

- which users use the CPU (easy)
- which applications use the CPU. This is much harder, (unless you organise some Trojan Horses in advance)
- which functions use the CPU (across 200 applications)
- what functions exist in any application
- which lines are doing the damage!

What you are trying to do is construct a pair of graphs:

```
                                        --- Users --->
     +------------------------------------------------>
     |
     |                       +-----------+
   A |                       | How much  |
   p |                       | How often |
   p |                       +-----------+
   s |
     |
     |
     |                             --- Functions --->
     |      +---------------------------------------->
     |      |
            |                       +-----------+
          A |                       | CPU used  |
          p |                       | in each   |
          p |                       +-----------+
          s |
            |
            |
```

Remember – if you can hit 1% of the CPU, that’s an ongoing £5,000 ongoing cost-reduction every year.

Aside – in an insurance business your central operation requires data lifetimes of 20-30-40 years. You must process where the data is!

How do you figure out what to hit (i.e. how do you make those two charts)? It is impossible without a degree of system and code management which will:

`<ul>`

<li>record cumulative CPU on entry to and exit from each workspace</li>

<li>switch on ⎕MONITOR within applications on demand</li>

<li>record which utility functions are present in each workspace</li>

<li>maintain master copies of utilities, which are shipped automatically when updated. There is an appalling function called GO in every workspace – it has nasty names and contains lots of horrid code, but it does the business!</li>

Unless you do these things, you might be able to optimise a workspace; you will never optimise the whole system.

What can you do about Design? A huge portion of all applications is utility code, so if we optimise that we can write highly optimal applications without having to think. Don’t optimise application code, it is too subject to changes in the specification.

### Optimising Code – the last stage.

`<ul>`

<li>you do need a reliable CPU monitor.</li>

<li>learn by experience.</li>

<li>THINK! You want the biggest element is a big numeric vector; 1↑A[⍒A] may look neat, but it will not be fast (why?).</li>

<li>when you need scalar looping code (which is sometimes unavoidable) get into some language which is tuned to do this. This is where the APL compiler fits.</li>

<li>use assembler FASTFNS if you have them, but hide them in your own utilities and make sure everyone is using them.</li>

### Some Examples

Here are five systems which have been attacked at various times over the years:

`<ol type="1">`

<li>PTP – which writes a few letters. Finding out what to attack!</li>

<li>“unnamed” to protect the guilty. The design was so bad that it had to be rebuilt.</li>

<li>IPSZ, a utility to take a long list of +ve numbers and break them into clumps such than none exceeds a given value. A classic use of this is wrapping text into a given page width. Just what you need a compiler for.</li>

<li>Making a useful ⎕FI for column matrices.</li>

<li>Various methods of reading files, particularly the variable-length kind.</li>

First, let’s look at PTP. The first thing to do was to uncompile it and run it again. The system as a whole remained balanced, but the CPU increased by 65%. This was interesting because the data volumes were small, and typically the compiler roughly halves CPU. Was it GDDM? This came out at 30ms per screen access, and GDDM calls accounted for only 25% of this – not a major issue. Now let’s look at print generation – aha! 3 seconds of CPU to run 3 pages – appalling! With some serious work this should improve at least 10 times, so now we know where we should concentrate our efforts.

Now let’s take a look at “unnamed”. This runs around 900 times a year. Up to June it averaged 13cpu sec per run; from August it is down to 2.5 sec and disk I/O is also down by a factor of 4 – so what was it doing!

```
+------------+
|  Big       |           <=============>
|  Flat      |  >>>>     <=============>  Component file
|  File      |           <=============>
+------------+
```

Basically, it submitted a batch extract and split out 50 fields into 50 separate file components, with some selection and ordering as it went. As originally written it read the flat file 50 times, compressing and indexing using the same criteria at each pass. With a little cunning and a scratch file, you compress and index once only, and (more to the point) read the original data once only!

Aside – you really would not believe what end-users get up to!! There was no spec (OK, we’re used to that), and the logic was uniformly distributed across the functions! In the end we synthesised a new structure from the code, and when we had finished over 90% of the code had simply vanished!

Text wrapping with IPSZ. This is quite a simple loop: chop off what fits, write a line, chop off what fits, write a line ... etc. All the sensible APL algorithms are of the n2 variety, so here is a great opportunity for the compiler. However you do need to do lots of horrid things to your code to keep the compiler happy:

```
 N←''⍴⍴L    ⍝ Force N to be (clearly) scalar
 S←¯1       ⍝ Unnecessary initialisation outside the main loop
 R←N⍴ ...   ⍝ Always prebuild result

Loop:
 R[]← ...   ⍝ Use indexed assignment into result

End:
 R←I↑R      ⍝ ... and clip it at the end!
```

However, the resulting function does still look quite like APL, and it is a damn sight easier for an APL person to do this than to rewrite functions in C (and then work out how to call them!).

Data validation with QUADFI. This executes character matrices on the assumption that there is one number per row. It is a little more expensive than `⎕FI`, but does more work, as it correctly handles blank rows and other gunk in the data. A useful trick:

```
B←~'' MATIOTA CHMAT
CHMAT←B⌿CHMAT            ⍝ Finds blank rows fast
```

... and only do the `⎕VI` where the data is legitimate.

Finally, reading IBM files. Some of the tricks are old chestnuts, but just in case you don’t know about RECFM=U, I’d better tell you!

The obvious loop: “open `⎕SVO`, read, check, read, check ... close” is SLOW, horribly slow, so what can you do? Well, checking the control variable takes the same time as reading the data variable, so don’t do it (after the first time). As far as anyone can tell, the time to read a record is totally independent of record size, so read blocks (up to 32K bytes) instead of 80-byte records and you get an immediate 400-fold reduction in CPU time. Oddly, this is what the TSO file AP does if you tell it you don’t know what your record format is – so do that and just trim the tail and reshape afterwards (’cos you knew that they were really 80-bytes, didn’t you!)

The fun starts if you have variable-length records! We gave up (wrongly) on these for a long time, then someone built a little sample file, pulled it into APL and examined it carefully. It turns out that there is a 4-byte length counter embedded at the start of each record, so you can just grab the whole file (as above) and chain down the records. Clearly, there is no way of doing this in parallel, but it works very well with compiled code – and saves us £1,000 per week in CPU costs.

### Reprise

Attitude (you have to care), analysis (you need to know where to look), design (don’t even try to optimise if the structure is too bad), and coding techniques (exploit the compiler if you have it).

If you do this intelligently and thoroughly, a year-on-year reduction of 20% in the CPU cost of current systems is quite within your grasp.

(webpage generated: 28 March 2006, 06:43)
