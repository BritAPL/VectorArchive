---
vid: '10011410'
title: news from Causeway
authors:
- Causeway Graphical Systems
volume: '22'
issue: '4'
page: '12'
source: trad/v224/cgs224.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2006
tags:
- implementations
- APL community
---

## Causeway Graphical Systems Ltd

Firstly a recommendation – if you feel that you really need to get acquainted with this C# stuff but would much prefer to avoid Visual Studio and all that comes with it, search Amazon for “Gunnerson C#” and pick one of the early editions. I have the latest one (for .Net 2.0) and the very first one and I think for APLers learning this stuff for the first time, the early one is best.

### Learning to work with .Net

It is an odd thing to say, but I reckon that writing APL with a view to compiling it results in better quality APL. Declaring argument types is good for you (and for those who come after you) and I suspect that ripping out ‘rank independence’ from utilities is actually a good thing. My old mainframe utility library spent far too much time conditioning arguments and far too little getting on with the job in hand for the common case.

The other thing I am finding is that writing with C# as a target makes me use all those helpful libraries ‘out there’, rather than staying with the old stuff. If I want to read a file, I get a *FileStream* and run its *ReadToEnd* method rather than calling my own code that wraps <span class="aplu">▯NREAD</span> with a few traps. This has the (obvious) advantage that the code just compiles (as long as it mentions System.IO) but also makes me long for the day when APL returns to its roots as a maths notation and leaves all this extraneous external stuff to the operating system where it belongs. At the Dyalog Users Meeting I showed a simple application with a Form, a TrackBar, and a Chart which was all written in standard CPro, but with no hint of a <span class="aplu">▯WC</span>. This has to be the way to go.

### NewLeaf for .Net – progress report

As you know, rebuilding *RainPro* as *SharpPlot* took 2 of us around 18 months. We started on NewLeaf in February and made startlingly rapid progress, getting the basic typesetting running by the end of March. Marc then took on the RTF parser, and I took on Tables and my side of the project has been taking rather longer than I had hoped. RainPro was basically an exercise in tidying up, renaming a few variables, adding compilation tags and shipping. I took one long look at *leaf.table.Spread* and decided to ignore it completely and write a new one. Thanks to a long history of users asking for features (and sometimes offering to pay for them) the current code does all sorts of things it was never really designed to do, and has got to the point where I really don’t want to touch it ever again. So ...

Now that we have a well-tested *TextBlock* object (used for all the simple text flow) a *Cell* is just a wrapper around a *TextBlock* with some extra stuff like borders and vertical alignment. A *Table* is a matrix of *Cells* with some extra stuff like page-fitting rules. Once a *Cell* has been told how wide it can be, it can re-wrap its *TextBlock* and find out how deep it is. A row in a table is just a vector of *Cells*, and suddenly APL is really working with you. The little extension to Dyalog that allows you to type <span class="aplu">⌈/tablerow.depth</span> and find the deepest cell in the row is very, very nice! I really hope that the C# guys one day get the ability to work with arrays of objects, refer to element properties, and get back arrays of numbers. If we APLers can somehow get this idea back into the mainstream, we will have done the world a service.

The other interesting thing we have found out is that you can put ‘traditional’ DLL entry points into .Net DLLs. This means that we are building up a collection of <span class="aplu">▯WCALL</span> definitions for APL+Win which make a simple (and very low-overhead) gateway from the old world to the new. As a very rough benchmark, the code to loop around nlFlow enough times to create 120 pages of PDF took approx 12 sec with NewLeaf, a little under 1 sec running the compiled code through <span class="aplu">▯WCALL</span> and a little under 1 sec running the same test entirely in C#. Conclusion – it is possible to build a bridge from the old world to the new which has almost no impact on performance. Of course Dyalog already did it for us – the same test run direct from Dyalog 11 runs at exactly the same speed.

### Where next

Finishing NewLeaf has to be the focus just now, but I am seriously beginning to wonder if I can rebuild *CPro* to run entirely off the .Net Gui controls. The *Grid* is the interesting one, and I am not even going to start reading about it until NewLeaf is well and truly out of the way. Most of the rest are easy.

Whether CPro (in its current form) can ever be compiled I rather doubt. It relies on being able to execute arbitrary APL expressions which are saved as data with each dialog definition. However .Net is moving towards XAML, where your dialogue definition is saved as XML data, and .Net 2.0 has partial classes (where the code for a class can be in several source files) both of which are big steps in the direction I want to go. I think the message for my CPro users is to hang in there and wait for the world to catch up.
