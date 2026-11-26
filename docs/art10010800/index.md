---
vid: '10010800'
title: A First Look at APL*PLUS III
authors:
- Adrian Smith
byline: James Wheeler at Cocking & Drury, 10th December 1993; notes by Adrian Smith
volume: '10'
issue: '3'
page: '60'
source: trad/v103/wheeler103_60.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- implementations
- user interfaces
---

James Wheeler at Cocking & Drury, 10th December 1993; notes by Adrian Smith <span class="affil"></span> <span class="email"></span>
{ .byline }

### Introduction

This was a one-day stop on James’ hectic European tour. He used the time very effectively; in around three hours we were given a clear understanding of how Manugistics see the Windows market moving, how they feel their APL should fit, and what it will look like. What he had to show us was ‘APL\*PLUS III as it stood at 5.00am last Saturday’ – and a remarkable amount of it actually worked! What follows is more or less a verbatim transcript of James’ presentation – my comments are in italics.

### Background

Part of Bill Gates’ plan to rule the world involves turning Windows into a modern operating environment. This means the WIN32 API, which is important for APL, because all good APLs need 32 bits! We already have WIN32s (a subset to allow Windows 3.1 to run 32-bit applications), and Windows NT is just the beginning:

### “The future is the WIN32 API” (Microsoft VP)

What I am going to show you is the first APL development system to run on this platform. APL\*PLUS III for Windows is the first of a series of products – beta release due mid-January 94, with expected final release date in the first half of the year. What you will see is the APL as it stood 5.00am Saturday – I understand it’s made considerable progress since then.

### Design Objectives

Assuming someone already knows APL (I’m not going to address that issue just yet) and Windows – can they become functionally literate with this product without using the manuals?

A fast, tidy development system. Users of APLGui said “too slow”, “too much junk in the workspace” and I agree with them. This afternoon I’m going to show you a GUI running with no functions at all in the workspace!

An “open” APL that supports and encourages integration. Software is becoming a set of components (from various vendors) that the application programmer (or even the user) can connect together:

- DDE links
- ODBC connectivity to all major databases
- extensible GUI toolkit with 3rd-party objects
- all those DLLs (not just the ones that come with Windows) are there to be called.

The follow-on product will be APL itself packaged as a DLL so that it can easily be called from C++, Visual Basic, and so on. Clever Excel programmers will make APL appear as just another Excel ‘Add-in’.

As part of this policy:

- no dongles
- free unfettered runtime

Although it is being treated as a ‘new’ product, there will be a migration policy for existing APL\*PLUS II users.

### The Language

At the bottom of the heap is the standard APL\*PLUS II interpreter, but stripped of:

- the inbuilt keyboard driver
- character screen IO (`⎕WPUT`, `⎕WIN` and so on)
- serial port handling (`⎕ARBIN` etc)
- sound and graphics

... i.e. anything ‘close to the metal’ which must be left to Windows to manage. The immediate benefit is a dramatic reduction in the weight of documentation – around 2” of manuals replace the current APL\*PLUS II package which is nearer 7” in thickness.

The only major addition to the language comes in the form of simple control structures:

```
:IF stuff>100
   do something here
:END

:WHILE i>0
   process i
   i←i-1
:ENDWHILE

⍝ and so on
```

... see Wilhoft’s Toronto paper for the basic idea. I suppose we should all stop beefing about this subject now! The way APL\*PLUS III implements these is as non-invasive as anything I have yet seen, so please will everyone else copy this! We can choose to ignore it all, and carry on branching, or use it where it helps clean up messy code. Either way, it has to be a good decision, and all credit to Manugistics for taking the plunge and pioneering this.

At the moment the GUI support is all ‘external’ to the core language definition, and is driven through `⎕`functions. However, this too will be fully integrated, as part of a general move to make the interpreter more aware of ‘objects’.

### How the GUI Support Works

The ‘object approach’ is fundamental – each object is a self-contained control structure containing functions and data. This is essential to hide the complexity and minimise interactions. The essence of an object is that other software cannot see inside it! Objects are accessible only through a well-designed and documented interface.

Some terms which need to be defined:

- FORM Very much like a mainframe ‘screen’.
- CONTROL In the trivial case, like a mainframe field, but potentially much richer. Think of the controls on the dashboard of a car.
- METHOD This is something you can ask an object to do. Kind of like an APL function, in fact an object is kind of like a tiny APL workspace!
- PROPERTY Very like a shared variable. If you assign a value to the ‘caption’ property, the visible picture on the object changes.
- EVENT Something which can be signalled on a object. Event-handling is the heart of GUI development. An event is a notification from the system that may invoke an ...
- EVENT HANDLER A bit like an Error-trap, but much easier to specify and use.

APL\*PLUS III is itself an event-driven interpreter – it is continually waiting for ‘events’ from Windows. These may cause the interpreter to do something, or may cause parts of your application to respond.

### Building a GUI Program

1. Structure all application code to reflect basic user actions
2. Define user-interface objects to invoke the actions
3. Use a visual editor to draw it
4. For each ‘control’ decide which things the user might do, and write handlers to call (1).

The program executes very simply ...

1. Initialise all necessary data, forms etc.
2. Show the form(s)
3. Sit back and wait. The user may do something.

The tools in this release of APL\*PLUS III are:

`⎕WI` – short for Windows Interface. This handles all the interactions with the GUI objects ...

```
   'object' <tt class="aplu">⎕WI</tt> 'new' 'class' ('prop' value)( ... ) ⍝ make one
qq←'object' ⎕WI 'property'          ⍝ reference a property
   'object' ⎕WI 'property' value    ⍝ set a property
```

These last examples will be true reference and assignment – come back next year! In addition, there are some extra `⎕`functions to help with the processing of events. These are:

`⎕wself`     during a callback, this is who invoked you  
`⎕wevent`     what the event name was (e.g. ‘click’)  
`⎕wargs`     and additional details (where the mouse was)  
`⎕wres`     the result of the callback (you might reject it)  
`⎕wgive`     yield to Windows during very heavy APL processing  
`⎕wcall`     interface to Windows DLLs  
`⎕NA`     interface to anything else.

As a simple example ...

```
name←'myfm' ⎕WI 'new' 'Form'     ⍝ create a new form
name←'myfm.b1' ⎕WI 'new' 'Button' ('where' x y w h) ⍝ add child
```

... and so on. Anything can be a child of anything ... even buttons on buttons if that is what you want! The size and position are defined in terms of the ‘base font’ average character size. If necessary, you can set the base font on a ‘system’ object which notionally owns all the top-level forms, and hence control this globally.

Active (open) forms live in dynamic Windows memory. When a form is closed, it can be stored in the workspace as a ‘magic’ variable (effectively a vector of machine code) which can be assigned (but not edited), stored on file etc.

A form has some other helpful properties:

- Children. The list of all child objects – so you can document them hierarchically.
- State. The list of all properties which have a non-default value.
- Class. What sort of animal this is.
- Properties. A full list of everything it can know about itself.
- Events. A catalogue of the events it can respond to. (built-in events only – APL\*PLUS III does not have user-defined events).
- Methods. A list of all the things it knows how to do.

A system object ‘#’ owns all forms, which may in turn own other forms or controls. This is not yet ‘MDI’ but it will be in a future release. However you can easily program Unix-style ‘widgets’ – i.e. intelligent forms which re-arrange themselves to show the essential bits when re-sized.

### Event Handlers

Every class has a list of things it will listen for: ‘Click’ ‘Keypress’ and so on. You simply set the ‘onEvent’ property of the object to an APL expression, and it runs the expression whenever it receives that event. Note that this is any APL expression – very different from the APLGui approach!! No longer do you need a workspace full of carefully-named 1-line functions.

### Graphics and DDE

All visible objects have a ‘draw’ method, which itself has a simple graphics language:

```
'photo.image' ⎕WI 'draw' 'bitmap' (?32 32⍴15)
```

Forms can act as DDE servers (they have a ‘ddeserver’ property which might be set to ‘excel¦mysheet.xls’) and controls can be DDE clients (‘ddetopic’ could be ‘R2:C3’). There is a ‘ddechange’ event which is used to notify controls that the data they are showing has changed.

### Extending the Base System with .APX libraries

“Totally shameless theft is a very important design principle. There are lots of good ideas out there – if you like it, rip it off!”

Visual Basic has a .VBX, essentially a DLL by another name. So why shouldn’t APL have a .APX? This would be a repository for callable APL functions as well as GUI definitions which would be seamlessly added to the existing GUI classes. Manugistics will release the specification of the .APX, and will also repackage existing 3rd-party toolkits (e.g. the bits-per-second graphics server) in this form. Maybe there will even be a ‘universal adaptor’ so that APL systems can hook up directly to .VBX libraries?!

### Where Next?

The next major release will include ‘capsules’, which will add true objects to APL. Objects currently accessed through `⎕WI` will become capsules accessed through the normal APL primitives:

```
form.button.where←23 34
```

... but capsules will provide much more than this ... essentially they are an arbitrary namespace where applications and modules can reside. The capsule itself won’t be a `⎕NL` object, but something called a ‘capsule reference’ will be.

### What Does it all Look Like?

A very standard MDI frame, in which reside the APL session, multiple edit sessions, and the debug and trace tools (unfortunately not yet hooked up). The APL\*PLUS II debugger in particular is “one of the most overlooked advances in APL technology for a long time” and fits well into a Windows look and feel.

### Summary

Not everything is fully hooked up yet, but what is there all works, and this new 32-bit WIN32s code runs very fast. Speed comparisons with APLGui are unfair, but you can expect at least a 15-fold improvement in the GUI, and a very competitive interpreter to back it.

### Commentary by Adrian Smith

There is no doubt that Dyalog really scared the wits out of Manugistics at Toronto! The result has been a U-turn of quite startling speed – out goes INCA (whatever that was) and in comes APL\*PLUS III at the gallop. Basically, everything we saw looks good, and the design has improved immeasurably over the APLGui that Duncan struggled with at BAA-GUI. It is comforting from an application-builder’s point of view that Manugistics have quite shamelessly copied all the best ideas from the Dyalog APL GUI, and may even have improved on some of them. This should mean that everything we now have in Dyalog we could easily port to APL\*PLUS III as soon as the finished product hits the streets. This level of cross-platform compatibility must be good for APL.

So, where does this leave the current APL\*PLUS II developer? Here are some points to consider:

- Manugistics are almost exactly a year behind Dyalog. I was working on the first beta release of the full Dyalog GUI early in January. Most of it functioned correctly, and the bugs have got steadily more obscure as time has passed. However good the Manugistics programmers, APL\*PLUS III is unlikely to reach stability until early 1995.
- The APL\*PLUS II (for DOS) session and debugger are currently ahead of anything Dyadic have to offer. If these are done really well in Windows, they will give APL\*PLUS III a genuine edge. I really liked the fact that the session is stays active while your forms run – this is when you most need a debugger!
- the issue of porting code from APL\*PLUS II is almost exactly as big for APL\*PLUS III as for Dyalog. Anything which touches the screen or listens to the keyboard (or plays tunes or draws graphs) will need a bottom-up rewrite. At the end of this process, the GUI bits should be portable anyway!

I think the overall conclusion has to be that GUI support will no longer be a strong discriminant. Now is undoubtedly the time to evaluate thoroughly both Dyalog and APL\*PLUS III, but you should be more interested in raw APL speed, developments in the base language (control structures and ‘capsules’), connectivity and the developer support than in the implementation of the Windows GUI.

(webpage generated: 18 February 2006, 02:16)
