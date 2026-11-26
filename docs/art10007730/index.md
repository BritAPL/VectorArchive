---
vid: '10007730'
title: 'Meeting: Dyalog APL Namespaces'
authors:
- John Scholes
byline: by John Scholes
volume: '11'
issue: '1'
page: '101'
source: trad/v111/scholes111_101.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- APL community
- language design
- implementations
---

by John Scholes
{ .byline }

### Introduction

This was the first chance any of us had had to see the new Dyalog environment, but perhaps more significant for the future of APL was the implementation of ‘namespaces’ which Dyadic have pioneered in version 7 of Dyalog/W.

John Scholes worked gently through a series of examples of Namespaces, and I felt at the time that he was answering many of Dave Piper’s requirements from the previous talk. Vector will need to look very hard at the details of this idea in future issues – for the moment all we have time to do is report John’s talk and leave you to judge for yourselves if Dyadic have got it right.

Peter Donnelly showed us a splendid pot-pourri of the new GUI features in Dyalog/W, and trumped James Wheeler’s bouncing ball with the (inevitable?) bouncing duck. Again, there is far too much stuff here to review in detail, so all we have here is a few screen shots and some very brief comment to give you the flavour.

### John Scholes on ‘Namespaces’

John began by acknowledging his debt to Adrian Smith and Duncan Pearson for sowing the seeds of these ideas, back in January of this year. The notion of namespaces has been about in the APL world for a long time – it is really a very simple idea! Namespaces do for APL what directories did for DOS: up to now the workspace has been a flat structure (like A: before DOS 2.0), but with directories you had the chance to organise your files into sensible groups (like C:\\word\\docs\\vector) which kept like things together, and also allowed you to have more than one thing with the same name!

“It’s nice if the number of functions in a WSDOC don’t outnumber the functions in the workspace you are documenting”

At a stroke, namespaces have the ability to resolve for ever the name-clash problems that have always constrained our ability to re-use code, and they eliminate the need for nasty kludges like local `⎕FX` to hide support functions within the utility that requires them.

John started with a clear workspace, and defined a couple of variables:

```
     vec←⍳4
     )copy util DISP
     DISP ⍳¨vec
┌─┬───┬─────┬───────┐
│1│1 2│1 2 3│1 2 3 4│
└─┴───┴─────┴───────┘

     DISP ⎕NL¨⍳9
┌┬────┬──────┬┬┬┬┬┬┐
││mat │DISP  │││││││
││vec │      │││││││
└┴────┴──────┴┴┴┴┴┴┘
```

The next stage was to leave the ‘root’ namespace by creating a ‘subdirectory’ called conv, like this ...

```
     ⎕NS 'conv'  ⍝ Create namespace and move to it
#.conv
```

... at this point the WS name in the session’s titlebar changed helpfully to ‘CLEAR WS\[conv\]’ to let us know where we were.

Now there were no functions and no variables to be seen. You can’t see in or out of a namespace, it behaves just like a mini-workspace – )COPY operates as you would expect – and is very quick with very low overhead (a couple of pointers get switched). Note the use of ‘#’ which is the name of the root namespace – this is different from the DOS convention where the ‘\\’ is used both as the separator and the name of root. Note also that there is only one `⎕WA` – in just the same way as DOS always reports the space on the whole disk.

## ![figure\_1](scholes111_101-fig1.gif)

The rule is that when you execute a qualified function name such as conv.encܼ4 it collects its argument and then switches context to the namespace given. This allows you to have persistent local variables (enc might refer to a ‘global’ in its working namespace) as well as collecting functions together.

Of course, system functions can also be qualified:

```
conv.⎕NL ¨⍳9      ⍝ runs a namelist in the conv namespace
conv.⎕ED 'decode' ⍝ edits in the requested namespace
#.⎕ED 'foo'       ⍝ edits in the root namespace
##.⎕ED 'foo'      ⍝ edits in the parent namespace
```

... as can many system variables, such as `⎕IO`.

Finally, the session itself is a namespace (with one or two system variables such as `⎕PW`), so you can define variables and utility functions within it, and have them persist across )load.

### Mr. Donnelly and his Amazing Bouncing Duck

Peter started by illustrating two significant enhancements to the Dyalog environment: timers and the elimination of the need for an explicit `⎕DQ` to start objects listening. This allows you to make objects which respond immediately to events (which makes for much easier testing and debugging, as you can throw events to things from the session and see how they catch them), as well as having objects running in the background while you carry on working in the APL session.

Naturally, he showed a bouncing ball, and then upped the stakes significantly with a bouncing duck. Of course the duck could be modified ‘in flight’ with `duck←2×duck`, or its behaviour could be changed as it bounced, by editing the callback code. Peter also pointed out that the utility code for the callbacks all resided (as it should) in the ‘duck’ object (as all the GUI objects are now namespaces), so that the demonstration was quite a powerful example of the benefits of encapsulation, as well as being fun to watch!

Peter continued with a crash course on the .VBX extensions – these are a huge resource of 3rd-party tools which Visual Basic programmers have found essential. He demonstrated a ‘cheap and cheerful’ graph, and a file-manager tree as examples of the kind of tools which are readily available. The definition and operation of these looked clean and straightforward, and they operated easily enough, and without a hint of a GPF.

The next major topic was the definition of MDI frames, and the management of ‘child windows’ within them. For those readers who have never met Winword (or Excel or Jigsaws or any of a rash of new Windows toys) MDI is a neat way of having several ‘documents’ open within one application (for ‘document’ read ‘spreadsheet’ or ‘drawing’ or ‘puzzle’ as appropriate), such that they all share one main menu and toolbar. The main form is the MDI frame, and has some built-in actions, for example it knows how to cascade or tile the child windows.

## ![figure\_2](scholes111_101-fig2.gif)

MDI also tries to be helpful to the user – for example as you run up and down the menus (or allow the mouse to dwell over a tool in the toolbar) you can have a ‘hint’ appear in the status field at the bottom, or (horror of horrors) a little yellow ‘speech bubble’ will pop-up to advise you what the tool is meant to do. Microsoft have a lot to answer for!

Anyway – if you want to make your APL application look like MS Office, you can do it without lots of hairy APL code to look for mouse-moves. This stuff is much better supported by the interpreter, which can do it faster and more reliably than an application ever could.

We had time for just two more topics before the doors closed and we were thrown out to catch our trains. The ‘grid’ object is surely the answer to many an APLer’s prayers. Getting a numeric matrix editor to work reliably was a tough challenge in Dyalog/6, and linked scrolling columns (which is what you need to edit any kind of relational table) was possible, but too slow to be comfortable for the user. The grid looks excellent – it is visually very effective, and you can attach more or less any 2-dimensional variable to it. The behaviour can be set at the cell level (so you can have one column read-only, the next one blue, and the third one set to accept dates). It has proper row and column titles (so it handles the linked scrolling for you) and generally looks to take Windows programming out of flatland into a genuinely multi-dimensional space.

## ![figure\_3](scholes111_101-fig3.gif)

Finally, Peter showed us how to place a Windows ‘metafile’ on a form (or a button, or an image – in which case you could make an array of them), and how you could take it apart within APL. In the simple case, this just gives us the ability to save graphics to the clipboard in a scalable, compact format, but there were clearly many more interesting things you could do once you had decomposed the metafile in the workspace.

### Commentary

*by Adrian Smith*

This was just a first look at late beta code, but I have to say that it showed a lot of promise. I think that the addition of ‘namespaces’ to the GUI objects must go a long way towards combining the functional syntax of APL with all the benefits of encapsulation and modularity that Dave Piper was seeking in his talk. It would be rather nice to have the benefits of the Smalltalk environment without the weirdness of the syntax!

The analogy with DOS is a good one. When I owned a twin-floppy 256K PC, I ‘loaded’ applications from the floppy disks which I kept in a stack of plastic boxes under my desk. The applications were small (e.g. Turbo Pascal ran from a single .EXE which fitted easily into 360K) and self-contained, so why would I need a directory? I did once experiment with mkdir, but really, what was the point?

When hard disks first became common (the original 10Mb kind), people still tended to dump everything into a single root directory, and rapidly lost track of things in the resulting mass of files. We soon learned that not only did things become more manageable with directories, but they ran rather faster into the bargain, because DOS had far fewer names to hunt through.

And what of the bouncing duck? I think Dyalog missed a trick by omitting timers from the very first version of the Windows product. How else do you write decent games simulations? It is also good to have a live APL session while your objects run – this makes it so much easier to test callbacks under controlled conditions. The MDI stuff is probably necessary, particularly in the big corporates who tend to lay down tight interface standards along the lines of “if it doesn’t look like Word we’re not having it around here”. It also couples well with namespaces, as it becomes far easier for an application to open several files simultaneously when the data variables can be localised within each child window.

Metafiles are essential (at least for me they are), and I shall explore the .VBX add-ins with great interest. I spent a couple of days mocking up a tree-editor in Dyalog/6 – if the .VBX is any good at all I shall gladly throw mine away and use one that works. The grid looks brilliant – I have been using Excel and DDE for ‘quick and dirty’ data input for too long, hoping that something like this would relieve me of the burden of writing one of these with the low-level controls. I really can’t wait to get my hands on this one!

(webpage generated: 14 October 2007, 18:13)
