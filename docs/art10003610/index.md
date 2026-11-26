---
vid: '10003610'
title: APL98 – New Gems from Old Roots
authors:
- Bob Hoekstra
byline: by Bob Hoekstra bob.hoekstra@khamsin.demon.co.uk
volume: '15'
issue: '1'
page: '97'
source: trad/v151/bob151_97.htm
converter: vecrescue 0.1.0
issue_tab: n1-august-1998
tags:
- conference reports
---

<p class="byline" markdown="span"><b>by Bob Hoekstra</b>  
`bob.hoekstra@khamsin.demon.co.uk`</p>

Here are some extracts from my diary:

## Saturday, 25 July

Rome is a very beautiful city. Ray Canon and I settled into our hotel and immediately "did" Rome by walking for eight hours through the summer heat. Only mad dogs and Englishmen...

## Sunday, 26 July

After Saturday, we were only fit for the beach. Ostia is very pleasant. The view is good too...

## Monday, 26 July

## Opening Session

We were welcomed to Rome and APL98.

## The Adaytum Planning System Architecture

Gitte Christenson introduced Morten Kromberg, who proceeded to tell us about this software project, not just the the architecture, but the history as well. The origin lies in an internal IBM mainframe APL project, a planning tool called FREGI. The main force behind this was George Kunzle, who (to cut a long story short) eventually combined his company with Insight System and this was eventually to become Adaytum Software.

Morten demonstrated the package briefly, showing some of its characteristics, describing and demonstrating D-Lists, D-Cubes and D-Links, which probably won't mean anything to those who weren't there. The apparent ease with which he could create data and meaningful views into that data was very impressive. He then spoke of the reasons for its success, listing:

- Great Marketing,
- A CEO with technical insight and bravery,
- A great basic idea (thanking Kunzle and Iverson)
- A pragmatic approach, allowing them to mix APL and other languages where this made sense
- A great implementation platform (thanking Dyadic Systems)
- The fact that they had both developers and managers who had grown up using APL.

This was a great session, and I took a lot of interesting impressions away with me. Two comment that stay with me clearly are:

1. the fact that they were using J, not as a replacement for APL, but for C code (!) and
2. that they were encouraging growth in the company by bringing in young people and training them. This was borne out by the fact that the large Adaytum presence at the conference consisted mainly of APLers much younger than the average. Personally, I believe that Adaytum are to be congratulated on this and I hope that they continue in this way, showing that this is the way to make the APL community stronger in the future!

## Dyadic Vendor Forum

Peter Donnelly and John Scholes always seem to come up with the goods! This time they discussed new features in the soon-to-be-released version 8.2 of Dyalog APL. These included:

- Multithreading within the APL interpreter. This allows the user to create separate program strands which each get time-slices from the interpreter. As I understood it, this was not multithreading at the operating system level, and thus would not make better use of the hardware (e.g. forking processes to run on different processors) but is still a very interesting concept - one that I cannot wait to play with!
- The fact that the core of the interpreter is to be implemented as a DLL. This allows for *very* effective use of APL-created GUI objects as ActiveX controls.

Both these new features were to be the subjects of workshops delivered by Peter and John later in the week (see below).

## Dyalog APL Multithreading Workshop

I attended of John Scholes's workshop and emerged quite amazed at the extra power and flexibility offered to the APL user by this feature. It would not be an exaggeration to suggest that this opens up entirely new possibilities for the use of APL. The concept of an APL web server immediately springs to mind (do we *really* want to do this with Apache being freely - in all senses of the word - available?) but there are plenty of other examples - almost anywhere where it is critical that an action is to be taken within a limited time of an event occurring.

## CoSy: <u>Yesterday</u>, Today and Tomorrow

Bob Armstrong presented 3 very interesting sessions discussing the past, present and (planned) future of Cosy, his note-taking productivity software package for laptops. Bob managed to amuse everyone at the first (Monday) session by stating '...today is yesterday, Wednesday is today and Friday is tomorrow...'.He was referring (of course) to the subjects covered in his three talks.

Bob demonstrated the Cosy environment and discussed the historical development in today's talk. It certainly seems to be a valid replacement for paper as a note-taking device. The ability to edit notes and have executable (arithmetic) content within the document is very powerful and it is easy to imagine that this would have been easier to write in APL than any other language. His comment that "...paper notes are never there when you need them..." has a lot of truth to it.

## Turtlegraphics - hidden features in APL2

Hendrik Rama demonstrated some APL code which he had developed/used in the study of tesselations leading up to the fascinating world of Islamic tiling patterns. This code uses complex number arithmetic to produce patterns which were realised using GRAPHPAK. He showed the listeners many overhead transparencies of the work, building up to some quite beautiful and intricate patterns.

Most of his talk is in the paper in the Proceedings. Unfortunately some of the best graphics we saw at the conference are not reproducible as there is no colour. Nevertheless, this paper will reward the careful reader with a fascinating insight of the subject.

## Tuesday, 28 July

## News about J

Eric Iverson told us about some of the new developments in the forthcoming release 4.02 of J. To me the most striking new features were:

- *Memory mapped files*: These allow direct access of large files, regardless of their actual physical location (hard disk, CD, flashcard, network drive) as though they had been mapped into memory. There was talk of using this as a device to allow two J processes to share data, even when on two separate machines or even operating systems.
- *LAPACK*: Providing this as a compiled software add-on allows very rapid solutions to a large number of problems. Examples discussed included simultaneous linear equations and their least square estimates and eigen-value problems. This is to be available on all platforms: Windows (3.x, 9x, NT & CE), Mac and UNIX.
- *General C/Fortran package interface*: The interface has been simplified making it easier to call compiled languages from a J process. This was discussed in context of LAPACK as well as in more general terms. Certain problems are so compute-intensive that they are unsuitable for anything but an optimised compiled solution. Eric gave an SVD example where calculation times reduced from a matter of days to sub-second!
- *Windows CE*: This came as a big surprise to me! Iverson Software have ported the interpreter to the hand-held "toys" running CE. This is a complete implementation, including graphics and (as noted above) the ability to use compiled routines like the LAPACK product. Eric described CE as the cleanest Windows implementation and mentioned that he had had many pleased responses when demonstrating this product. One clear market opportunity exists to provide a simple graphing interface to Pocket Excel, which lacks graphics.

## Using APL to build science tutors for the high school level

Manuel Alfonseca introduced the program apparently used by many schools in Spain to assist in the teaching of Mathematics and Science. The Maths version is apparently much simpler, but the thought that remains with me was just how simple it all appeared to be! This was an excellent example of using the strengths of APL - any other language would have made this project much more complex.

## Teaching Classical Calculation Methods: APL Challenge

Andrei Buzin spoke about the experience of teaching University-level mathematical calculation methods in the University of Friendship, Moscow.

The talk was full of examples (as is his paper in the Proceedings) of the problems addressed. I was impressed by the breadth of the problem and to what extent APL had been able to help him. Many examples have been provided and graphical analysis is done by their own package, displaying graphs of functions on the screen. I would have liked more time to investigate this as a learning method for myself - it is too long since I was concerned with the problems of Cauchy, Euler and friends and my memory is a little hazy.

All in all, Andrei's methods look as though they might be very effective. I am sure that, had I had this sort of introduction to these subjects, I would remember more than I do now!

## Banquet

The evening brought an almost endless supply of great food, wine, good company and entertainment. Those who weren't there really missed something.

## Wednesday, 29 July

## Writing ActiveX Controls in Dyalog APL/W version 8.2

Peter Donnelly lead us gently into the art of creating ActiveX controls and calling them from Visual Basic. I cannot imagine how they could have made this any easier! All that now remains is for me to upgrade my copy when version 8.2 becomes available.

Of course I am keen to know how all this works on the UNIX platforms. No doubt I'll find out before too long.

## The GAMA/SOFIA Portfolio Management System (APL Italiana/InfoStroy)

Carlo Alberto Spinicci from APL Italiana opened this discussion in a humorous vein by stating that his was actually perfect and accent-free but had decided to affect a strong Italian accent and bad grammar as a token protest against the continued use of English as the language of APL conferences. He then (displaying a more than adequate grasp of English) went on to introduce us briefly to the GAMA/SOFIA software package, telling us of its Italian origins.

Alexei Mirochnikov (InfoStroy) then took the podium. He talked about the perceived need for a shrink-wrapped portfolio management system in Scandinavia, the meeting the various parties and how this led to the further development of this product by the Russians. InfoStroy made the product multi-lingual and converted from APL Plus II for DOS to Dyalog APL for Windows in about 3 months, no mean feat, considering there were some 4,000 functions, 300 screens, almost no documentation and most of the existing comments (apparently very sparse) were in Italian!

A demonstration of the software was followed by a brief mention of likely future directions. All in all, a very enjoyable and enlightening talk.

## Soliton Vendor Forum

Unfortunately I missed this, but I did hear about a remark concerning a possible APL interpreter for Linux. Those who know me will know that this is one of my pet subjects, so I spent most of the afternoon pestering the person responsible. Unfortunately I have nothing concrete to report, though there is a chance that such a product may see the light of day before long.

*If any readers are interested in such a product, please let me know by email.*

## CoSy: Yesterday, <u>Today</u> and Tomorrow

Bob Armstrong presented the second of the three sessions (see above) discussing the current state of the CoSy software.

## Thursday, 30 July

## All Searches are divided into Three Parts: String Searches Using Ternary Trees

In the title of this presentation. David E Siegel played on remarks Julius Caesar made about Gaul. However there was nothing classical about the subject matter - it was bang up to date!

His discussion of ternary tree search algorithms was very interesting. Of course, this type of algorithm is not a always optimal. Apply this method to a problem not suited to it and you could make a very slow search, but it can be optimal in the right conditions. For those who want to try it, the proceedings contain more detail than I can possibly relate - this paper, complete with function listings, greatly rewards the careful reader.

## I Can Succeed with APL - and So Can You

Eric Baelen (APL2000 Inc) provided us with a most encouraging presentation, giving numerous examples of people who had "made it" using APL.

## IBM Vendor Forum

Nancy Wheeler ran through the latest news from IBM. The APL2 product line now looks quite impressive, covering everything from MVS/TSO or VM/CMS mainframe, UNIX (AIX and SunOS/Solaris) and OS/2 to (the latest introduction) a Windows version.

Of course the Windows version got a lot of attention, and was demonstrated very well. I found the high level of compatibility between the versions very encouraging. This extends to the inclusion of AP124 and AP127 as part of the distribution. Further highlights (for me) were the ease of sharing data with Excel (ws 2 DDESHARE) and the setup of windows objects with direct mapping onto workspace variables.

## Making Nested Arrays Work For You

Philip Benkard's workshop took up the whole afternoon (excluding the tea break) and provided many interesting insights into the use of the modern APL.

Recently I have just "used" the APL interpreter without thinking too much about what I am doing. The 3 hours spent with Phil made me think about the language in a different way, and subsequent re-reading of his papers have provided more food for thought.

For me to give even a taster of his workshop and paper would be impossible. I suggest that most people could do worse than give his paper (The Epistemology of APL) a very careful read.

## Friday, 31 July

Unfortunately I had to fly home early in the morning and so missed the Friday sessions. I was (and am) very sorry to have missed several interesting sessions, in particular the last in the trilogy by Bob Armstrong. While I'm on that subject, I have heard from reliable sources that Bob blamed me for his incoherentness during this session, something which I wish to refute...

Looking back, a very enjoyable conference. As I said before, Rome is beautiful and I look forward to another visit before too long.

***
