---
vid: '10004310'
title: 'Namespaces: Just another Means to Multiply your Chaos?'
authors:
- Kimmo Kekäläinen
byline: by Kimmo Kekäläinen
volume: '11'
issue: '3'
page: '92'
source: trad/v113/kek_l_inen113_92.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- language design
- development practice
---

by Kimmo Kekäläinen
{ .byline }

My waiting was finally rewarded by Dyalog version 7 last summer. About namespaces I had heard John Scholes’ introduction in Swansea. Although 7 was full of new fancies – toolbar, hints & tips, VBX, timer, MDI, metafile and so on – they all were just new features – welcome, useful indeed, but mainly to be classified as add-ons to GUI-functionality, even the Grid. Namespaces were something different. When exploring 7, I saved it till last. As I could have concluded from John’s demo, this new concept seemed to provide thus far unreached possibilities to organize your working environment in a new, more controllable and productive manner.

### Extensions to the Interpreter

Who had not a subset of little utils like OVER, DEB, ROWFIND, DLTB? You need them time after time, in every workspace. To do anything with APL, you soon miss some of them, you copy them. Why not have them around like primitives or `⎕FUNCTIONS`? Now namespaces provide you with this possibility, at least in a logically analogous manner. You don’t happen to have `⎕ROWFIND`. However, if you had, `⎕ROWFIND` or `⎕SE.ROWFIND`, what’s the difference to a calling program?

But you can go much further. If you are an application developer using APL, you definitely have a subset of tools to do common tasks that are needed, whatever application area you are involved in. You probably use some dialog boxes to ask questions, give WAIT- and OK-messages, print, show reports to the user, maybe graphics, file handling etc. The problem with these in every task is not only to copy those, but also the varying range of sub-functions they are calling. And you also develop them, whether to correct bugs or do improvements. But where on earth was the last version.

Then, after a programming period you take )FNS to see what you have and are faced with the problem of extracting the problem-specific functions from the common namejunk of OVERs and QUERYSTRINGs. Then you think again that shouldn’t they have put these into the interpreter, of course deliberately ignoring the fact that if they really would have, your EXE would finally end up exceeding the size of your RAM. Well, Namespaces allow you to do this yourself, without overloading the exe-file, but logically at the interpreter-level. Here is the way to make your own enhancements to the environment. Having these in mind I started.

### Step into Spaces

First I put my ROWFINDs directly under the `⎕SE`. That’s a proper place, at least for idiom-like things. They are small, mostly oneliners, old, safe and robust, familiar like primitives. Good to have them around, always. The same counts for my set of general GUI functions named according to task like QUERYSTRING, CHOICEFROM, POP\_UP\_MENU, OK\_MSG, WAIT\_MSG and so on.

When you proceed with hacking your toolset, you probably face the problem that the complexity of your common task-related tools grows. They call sub-functions, both idiom-like and task-related. If you want them to be always available, the answer is to create your own namespace and save it directly under `⎕SE`. Basically, this is a very straightforward clear concept. However, if you have a GUI-tool to show a report on a scrollable form on screen, you probably want to provide the possibility within that tool to print it, guide it to the clipboard or Excel, maybe give your user a chance to change fonts or colours on screen and so on. But to provide parallel things like these you may need to call functions from other namespaces under `⎕SE`, or in namespaces under them. Still, this showing a report on screen is a very common need. Most of my application programming somehow relates to reporting. It must be handled in a unique manner, with one general tool. So I did it ...

... and went on. In a few days I had about 15 different sized namespaces under my `⎕SE`. There were my goodies, available in a clear workspace as I always had wanted. My goal was gained. Hadn’t I earned I feeling of relief and satisfaction – perhaps a cold beer! \[Have two – Ed\]

Still this made me wonder. Most of my `⎕SE` was OK, definitely, idiom-like stuff and so on. What made me nervous was the growing number of references between functions in different namespaces. How ever am I going to maintain this in the future? The reason in the start was sound and clear – they were already there. Another way would have been to copy and commit a sin of multiplication; wasn’t that to be something of the past age, before namespaces? Dyalog allows you to go as deep as you wish in namespace structure. If you really do, good luck – and prepare a map. I began to feel a bit, if not lost, at least confused. What if I want to use some of these tools independently of current `⎕SE`? Am I now somehow stuck with my profile? I had happily got rid of the namejunk in my application workspace, at the price of namejunk in my `⎕SE`.

A consistent naming convention is a thing I often preach to others as a free and powerful way to give readability to code and to provide self-carrying information to an application structure. This was something I forgot to plan in my anxious start. Some namespace-names under `⎕SE` were UPPERCASE, some were lowercase, some started with Uppercase and so on. Had I adopted a disciplined standard it would have made functions calling these easier to read and extract in application code. OK, go and change the names of the namespaces. Yes, but then go and change all the references to functions in them that lie there waiting in those 15 namespaces or under them. Then there was this trouble of finding those AP- or DLL-functions that you forgot to kill or localize. There they stayed lost in some unfound namespace and efficiently prevented you from saving your latest work.

While worrying about these I made a little DIR to find out that my original `⎕SE`-file had grown from 100 Kilos to half a megabyte (part of that goes to a bug in the Dyalog saving structure, but still ...). What I did? I created a workspace copy of `⎕SE`- functions and of every single `⎕SE`-namespace under it. And stopped.

### Conclusions

This process was worthwhile. I made a good inventory of what I have. My attitude towards namespaces is totally positive. They are not only a promise, I’m convinced that they really provide you with a way of handling your utils better. Mistakes were mine. I think I learned a lot. Benefits don’t come automatically. So, when I next start organizing my little Dyalog World, I will carefully think and judge my existing toolset in the following terms:

- What are the general routines you really are going to need as functions directly under `⎕SE`? What really are the general routines you are going to need as namespaces directly under `⎕SE`?
- Are you going to allow a namespace-located function to call a function living directly under `⎕SE`, and more important, do you allow it to call to functions in another namespace?
- How many namespaces under a single `⎕SE` will you be supporting from the point of view of memory load and maintainability?
- How many session profile files do you think will be needed to provide a controllable and productive environment, starting from the fact that if you are an application programmer, the minimum is two – one to develop applications, one to run them (you don't need WSDOC to do that)?
- What's the proper place to save and maintain the “source” code for utility functions that end up in `⎕SE.namespace`? Will the traditional WS still the be the most flexible alternative for that, after all? It would allow the independence of the ns-concept (totally if ns-references are not allowed) in the case of possible use in environments where namespaces are not supported. Still they could be easily called and hooked into `⎕SE` by copying in at the start and easily stripped off when not needed. I'll probably never be able to expunge a third of my interpreter as I in certain situations wish, but should at least be able to do so for my session enhancements.
- Currently I still have more questions than answers, sorry!

When you keep developing applications with APL for several years, you will get accustomed to doing things in a certain way, adopting habits, preferring certain techniques. They give a kind of “stamp” to your work. This is good in the sense that it brings consistency to your work, bad in the way that you may get stuck with them and miss possibilities that others have discovered, which might help you do things better.

Every now and then, it is good to stop and do some evaluation; look around. If you haven’t done this lately, namespaces is a good place for that, in fact a must. They are a stop point, in case you don’t want to miss the point and lose the benefits. They can help you to organise your application development environment to be a productive and disciplined world, or just stay as an alternative means to multiply your chaos.

(webpage generated: 29 October 2007, 15:44)
