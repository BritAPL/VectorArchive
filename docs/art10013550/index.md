---
vid: '10013550'
title: 'Editorial: Vector 18:2'
authors:
- Stefano Lanzavecchia
byline: Stefano Lanzavecchia
volume: '18'
issue: '2'
page: '3'
source: trad/v182/ed182.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2001
tags:
- system interfaces
---

*Stefano Lanzavecchia*
{ .byline }

“I find your lack of faith *disturbing*”  
Darth Vader

Like Jedi knights, software developers have a powerful ally both in their daily chores and when they fly higher in pursuit of a space princess or of a million dollar project. Some are aware of the protection it gives them, of the numerous services provided, and this makes them respect it and seek for its help whenever needed. Some others don’t understand it and simply wish it wasn’t there. It wasn’t long ago, even if it feels like centuries, that we were having fun but at the same time were really struggling on the noble task of colouring one single pixel on a black screen. We’d managed somehow to switch from text mode to graphic mode, the brand new graphic card we’d just bought and that was giving us hope of success. Amazing And then there was the printer: I admit it, it was a good way to spend an afternoon, trying to understanding how to draw a picture on the painfully slow IBM 80 cps. At least it was less dangerous than to try the same thing on a mainframe’s printer where a trivial mistake could send the printer into a maniac printing rampage that would consume 200 white sheets in the time required to reach the power switch.

Didn’t you almost throw your shiny modem out of the window because you couldn’t make it hear a word of the inspired declarations of love you thought you’d sent him? Yet, the cool serial port manipulation library you’d written after many late night hacking sessions was supposed to be flawless, never mind that the sole aid you had came from the technical documentation beautifully translated into NippInglish from the original Japanese. Those days are over, the powerful ally is here for us: taking more and more of the burden on its shoulders, the *Operating System*. Call it uselessly bloated, mind-bogglingly complex, stupidly intrusive, if you like. I’ll keep on using it and make it do all the simple and complex things that I don’t have time nor I want to redo.

First of all: Operating Systems are not what they look like, but what they do. Mind you, nobody said Operating Systems have to be crude or ugly to be functional. Yet, there’s a fundamental difference between a glamorously looking shell and a real Operating System. If you have programmed, not just used, both Windows 3.1 and Windows 2000 or Linux you know what I mean.

So here I am, staring at an empty APL workspace, with the desire to harness the power of my newly discovered mighty ally. “Don’t!” “I will” “Don’t!” “I will”. Quad-NA, yes sir. The dice have been thrown: in the Microsoft world, COM components now and .NET objects in the future will provide a cleaner and less painful interface to the services exposed by the Operating System. In fact, in its richness, the .NET framework abstracts not only simple storage and graphic services, but gives support for regular expressions, inter-process communication, database access, to mention a few only. The Java Enterprise framework is already capable of all this, but it wasn’t designed to be exploited from other languages, and, because of that, is less appealing to me. Still, with Sharp APL’s interface to Java it may actually be possible to benefit from it even now. If any of you has experience in this field, let us know because I am really interested.

But let’s return to the here and the now, to the fiend, Quad-NA, Dyalog APL’s Name Association’s primitive, to be precise, that is, the possibility of importing foreign functions exported from native DLLs. There’s an understandable but, in my opinion, unjustified, fear of using this powerful feature. Any APL interpreter has to rely on the Operating System eventually to open a file, read data from it, close it, so it isn’t intrinsically more dangerous to call straight from the Win32 API CreateFile, FileRead, CloseHandle than to use NTIE, NREAD, NUNTIE. It may be more difficult. (or is it just lack of habit?) and, in this particular case, most likely unjustified, but in no way more dangerous. I know it introduces an annoying dependence on the host Operating System and we would all love to be able to write platform-independent code, the ever-sighed “write once, run anywhere”, but we also know that this is a chimera.

Many of the *Hackers’ Corner* columns (like the one in this issue) contained wonderful examples of how easy it is to delegate the solution of a particular problem to an external source, more often than not, the Operating System itself. Here’s another simple but interesting show-case: let’s say you need to delete an APL’s component file. What could be easier? Just FTIE it and the FERASE it. Let’s now say that your component file is on a network drive somewhere and the network is known to be flaky, therefore the file is likely to be damaged. In other words the FTIE could very well fail, and it will surely fail at the customer’s site. Still easy! NTIE it, instead, and NERASE it. Wonderful. Let’s now say that the number of the files to be deleted is in the thousands and the network is overloaded (hundred of users on a 2 Mbit/s token ring). Do you really want to force the network to transmit all your open requests, cause the file server to get all excited preparing caches and various other data structures, just to delete the file immediately after? Wouldn’t it be easier to simply delete the file?

Your Operating System can do it already. Just ask it to do it for you. The Win32 API has a handy kernel function called *DeleteFile* that does just that. Maybe it’s something that should be supported by interpreter natively? Quad-NDELETE maybe? Maybe. And, then again, maybe not. APL interpreters have been around for a while and they have wisely tried to avoid the proliferation of Quad system functions. J is developing exactly in this direction: instead of covering more and more of the Operating System’s APIs with the aid of the foreign conjunction, the implementers are de-committing more and more of the custom bang-colon (also known as !:) uses like ODBC, regular expressions, TCP sockets, and soon OpenGL as well, in favour of J’s incarnation of Quad-NA coupled with supporting J scripts. The benefits are multiple: instead of opaque compiled C code, the front-end is written in plain J, there available to be customised by a developer in need; at the same time, the interpreter is slicker and the implementers can concentrate on the core language, where their time is best spent. Remember that J comes in a Windows version as well as a Linux version, so cross-platform compatibility is a serious issue, but it’s possible to achieve with the right team of people and the right approach.

The Force is strong but Jedi knights need to be trained to learn to control it, or, if you prefer, powerful tools need time and affection from the user to unleash their full potential. My opinions, of course.

***
