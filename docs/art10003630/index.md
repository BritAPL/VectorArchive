---
vid: '10003630'
title: APLs for Linux
authors:
- Bob Hoekstra
volume: '16'
issue: '1'
page: '7'
source: trad/v161/bob161.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1999
tags:
- implementations
- APL in perspective
---

<table width="90%">
<td>From: Bob Hoekstra</td>
<td align="right">March 1999</td>
</table>

## Finally it is Happening!

Many of us have been predicting this for years, although the more pessimistic among us may have doubted that it would ever really happen. What am I burbling about? The *Rise of Linux* of course. The recent flush of news articles caused my pulse to skip a beat, but the crowning glory is IBM’s announcement of *support*. For those who missed it, please point your web browser at http://www.sunworld.com/swol-03-1999/swol-03-linuxworld.html, but for the lazy I’ll quote the juiciest bits:

> “San Francisco (March 3, 1999) – This week at the LinuxWorld Expo in San Jose, IBM detailed its plans for Linux, including extensive, seamless, single point-of-contact global support to be offered in conjunction with the four major Linux vendors, Red Hat, SuSE, Caldera, and Pacific HiTech ...  
> IBM also announced its plans to port several IBM products to Linux ... and a port for Linux to certain RS/6000 models, including the high-end POWER3-based 43P Model 260 ...”

Your readers may well want to know why I am writing about this to an APL publication. Fortunately I have a very good reason, and (I believe) a convincing argument. Please read on.

Many times I have heard APLers whinging about the apparent decline in APL usage and the fact that many APL projects seem to be floundering. Personally I try to ignore most of this, but there is no denying that APL is not doing as well as it deserves to be. Many times I have asked myself if there is a good reason for this, and the only real reason I can come up with is “lack of support” (some readers may cry “obvious!” at this point, but please bear with me).

The first time I became aware of this phenomenon was when I noted (I think it was in 1986 or 1987) that APL was not on a list that IBM sent to clients describing “strategic software” products (I think that was the phrase used). I asked about this and was told that APL had not been (could not be?) ported to the AS/400 platform and hence IBM was not going to market our beloved language very actively. To any who think that IBM’s best marketing efforts aren’t worth much (e.g. the spectacular lack of success that OS/2 has enjoyed) I would like to point out that the PC itself only really took off because IBM produced it first. Please don’t tell me about the importance of the spreadsheet in the development of the PC. Spreadsheets they were also introduced on other small computers, some arguably more advanced (better) than the PC, but nobody remembers them. A blessing from IBM was (and probably still is) critical for a new product.

IBM is no longer the force it used to be. Nevertheless, people still tend to sit up and take notice when Big Blue says something. They may disagree, or even ridicule, but they notice. This endorsement of Linux is (in my opinion) the most important step thus far to what I hope will be near world domination for this excellent operating system. Please don’t get me wrong, I don’t want to convert all the readers to Linux with this letter (though that would be nice as a longer term goal). Instead I want to alert your readers to the fact that there is no good (not even adequate) APL interpreter for Linux.

THIS IS A BIG, **BIG** MISTAKE!

Nobody knows how many Linux users there really are, but I have heard estimates exceeding 12 million, with 7 million aparently being a reliable lower estimate. Compare this to around 11 million OS/2 licenses sold. Until recently they were the loony types (like me) who wanted to get away from MickeySoft and run a real OS at home, but this is changing. Big companies are installing free operating systems (Linux, FreeBSD, NetBSD and others, with Linux being by far the fastest growing OS) and running them on critical systems. If you don’t believe me, search the web. They are developing many, many applications in C(++), Java, and many other languages, but not APL.

These people need an APL. Even the loony types (like me) want an APL. Not something like APL/II, brain dead and without a decent APL character set, slow, non-standard and ancient, nor J (which is available) as I think this initial learning curve is far steeper than for a conventional APL. They want a modern, fully featured APL with nested data structures and a good X11R6-friendly interface. They want something that will produce applications quickly using the power they have to hand. They have an efficient operating system on powerful machines – *i.e.* APL heaven!

Linux is going from strength to strength. In 1998, 17% of new servers got Linux as their operating system. Pledges of support have come from all sorts of sources, including IBM/Lotus, Sun Microsystems, Hewlett Packard, Silicon Graphics, SCO, Oracle, Sybase, Corel and more. IBM have indicated this as an alternative to AIX on RS/6000 machines (note that they are doing the port themselves!) an Sun have been giving critical support to the developers of SPARCLinux for the UltraSPARC processor (as an alternative to Solaris 7). This is serious support – these companies are not playing around!

Many implementations seem to be heading for the desktop, not just the server. APL might be able to buck this growth, but I believe this could be fatal. There is a huge market of computer fanatics out there who would benefit from APL (and vice versa) but they cannot. I believe that the interpreters ported to Linux will benefit greatly in high volume sales over the coming years. Those that don’t will have high cost, low volume sales which will become increasingly difficult to justify in a corporate context and impossible to justify in a personal/home context.

More directly, I am convinced that Dyadic Systems, APL2000, IBM and Soliton (and any others) could port their UNIX APL interpreters to Linux with little effort. Economic arguments against this don’t hold water. In the long term, they must do it. I would like to suggest that these companies try this with an old version. For instance, Dyalog APL/X version 6.x would be interesting to port (perhaps even release as a no-support freebie) to see the response for a relatively small capital cost.

I’d like to close with a quote from another SunWorld article (http://www.sunworld.com/swol-03-1999/swol-03-sco.html):

> “...Today, any company that doesn’t include Linux somewhere in its roadmap is missing the boat...”

Please let us see APL grow and prosper.

Bob Hoekstra ([Bob.Hoekstra@khamsin.demon.co.uk](mailto:Bob.Hoekstra@khamsin.demon.co.uk))

PS: In case its not obvious, I feel quite strongly about this.

***
