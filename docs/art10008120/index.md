---
vid: '10008120'
title: 'Kenneth Iverson, APL and J: Some Personal Recollections'
authors:
- Keith Smillie
byline: from Keith Smillie
volume: '22'
issue: '3'
page: '115'
source: trad/v223/keith222.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- APL in perspective
---

from Keith Smillie
{ .byline }

The *American Heritage Dictionary* gives as a definition of **epiphany** the following: “A sudden manifestation of the essence or meaning of something.” Its etymology is “Middle English *epiphanie*, from Old French, from Late Latin *epiphania*, from Greek *epiphaneia*, manifestation, from *epiphainesthai*, to appear : <i>epi</i>-, forth; see EPI- + *phainein*, <i>phan</i>-, to show; see **bhā-<sup>1 </sup>** in Appendix.” One of the best known epiphanies in Western culture is possibly that of Saul on the way to Damascus.

My own epiphany in programming languages was much less dramatic than Saul’s. I was not going anywhere, there was no blinding light or voice from above, nor did I fall down. It was in 1965 or 1966, and I was sitting quietly in my office in the Department of Computing Science at the University of Alberta thinking about the commonly occurring statistical problem of classifying a number of observations given the left-hand end of the first frequency class, the class width and the number of classes. This is a problem I had already handled in other languages but now I was trying to program it in a new language called Iverson’s notation or APL.

I had heard of Iverson’s notation in a lecture just before I came to the University of Alberta in 1963 and had even read a little of *A Programming Language*. On arriving at the University, I found that my friend and colleague Bill Adams, whom I had known for several years, had just completed an MSc thesis in which he had used it. However I promptly forgot about all of this work for a couple of years while establishing an academic career, which involved amongst other duties teaching a course in probability, statistics and numerical analysis and another in Fortran.

I first met Ken Iverson in the Department during one of his visits to his native Alberta. I was soon caught up in Ken’s enthusiasm which he had already imparted to Bill, and we soon had an IBM 1050 terminal linked by a dial-up connection to a computer in the IBM Research Center in Yorktown Heights. So Bill and I were able, with the cooperation of an indulgent Department which paid the telephone bills, to experiment with APL as an executable, rather than just a paper-and-pencil, notation.

Now to return to my own undramatic epiphany. After some work on the frequency classification problem, I realized I could express the algorithm in a one-line APL function. Gone were the do-  and if-then-else-statements and the type and array declarations of Fortran. I was so excited by this discovery that I carefully wrote the “one-liner” on a small strip of cardboard which I glued to a tie clip. I’m not sure whether I ever wore it outside my office or Bill’s.

And so from such inauspicious beginnings began my association with APL, and my later association with J. Bill and I both used APL in courses, and in addition Bill used it in his work with computer architecture and I in my work with statistical algorithms. The technical details although important and exciting at the time may be passed over briefly: after the single 1050 there were one or two IBM 2741 terminals located in a hallway and connected to the new IBM 360/67, then one or two laboratories equipped with them, then a 2741 in the classroom with a connection to the main computer, in the mid 1970s an IBM 5100 for both classroom and office use, and of course from the early 1980s the personal computer. Then there were the conferences, only a few of which we went to, starting with the 1969 conference in Binghamton , the celebrated “March on Armonk”. Reading the conference Proceedings today gives a rare glimpse of the beginnings of APL, and a look at how some of us appeared when we were 35 years younger.

My solution to the frequency distribution problem which kindled my enthusiasm for APL marked the beginning of many years of the use of array languages in statistical calculations. On looking over the lectures notes for my first APL lectures – given in about 1966 or 1967 and carefully handwritten in pencil on yellow paper – I see functions for mean, variance and standard deviation, frequency distributions in one, two and an arbitrary number of dimensions, contingency tables, probability distributions, and also various calendar problems which would dog me for years. This work would result in a package of statistical functions named *STATPACK* which would keep appearing for many years in various reincarnations with extensions into such areas as analysis of variance and non-parametric methods, always carefully documented in technical reports with red covers and black bindings and distributed by postal mail with a 5¼-inch floppy disk included.

Apart from adapting this work to the personal computer, I set APL aside for most of the 1980s while many of us in academia grappled with the seemingly insatiable demand for courses in  “computer literacy”, whatever that term might have meant. Fortunately this period was enlivened, even made bearable, by a close and most enjoyable association with Trenchard More and his Nested Interactive Array Language Nial during which I wrote amongst other things a Nial equivalent of *STATPACK*.

In 1991, a year before retirement, I chanced upon J which Ken was then introducing as a modern dialect of APL. I forget the paper which drew my attention to J. My earliest work in J was a one-page listing of some 60 J verbs for statistical calculations which I can remember showing to Ken on a visit to Toronto in August of the same year. I continued my work with J throughout the 1990s and into the early 2000s during which I had the pleasure of using J in its increasingly convenient implementations while enjoying the great luxury of retirement.

During all of my work with APL and J Ken Iverson was always ready, by precept and godly example, to educate, encourage, correct, and even chastise as he considered necessary. I believe that the most important lesson I learned from Ken was that programming languages should be learned and taught as we learn natural languages, i.e., by using them in the real world to solve real problems and not by studying the grammar. I can still remember Ken saying at the Binghamton conference something like “No more manuals!” Ken’s writings have influenced me very much and I would like to mention just a few of them.

The *APL\\360 User’s Manual*, which he wrote with Adin Falkoff, in its several editions was a constant companion and guide for many years. The technical report *APL in Exposition* was a source of splendid examples. I can still remember Ken asking me what I thought of the 15-page section “The Computer: A Device for the Automatic Execution of Algorithms” which he said was his “summary of computer science”. (I loved it!) Then there was his landmark Turing Award Lecture “Notation as a Tool of Thought” which impressed upon me the vital importance of a good notation. Alfred North Whitehead’s remark quoted on the first page set the tone for the entire paper: “By relieving the brain of all unnecessary work, a good notation sets it free to concentrate on more advanced problems, and in effect increases the mental power of the race.” The paper in the 1991 *IBM Systems Journal*, “A personal view of APL” provided an excellent transition from APL to J. One phrase which appears on the first page, “When I retired from paid employment, ...”, I have quoted many times to explain my continued work after retirement, much to the mystification of a few friends and colleagues who consider work and money to be inseparable. And, finally, will I ever understand everything in the *Introduction and Dictionary* which Ken considered the only necessary reference for J? No, never!

I deliberately began this paper with a definition from the *American Heritage Dictionary* which was a great favourite of Ken’s and which he introduced to countless users of APL and J. I find it indispensable in my reading and writing, and have four copies: on my computer desk (one of three dictionaries there), in the living room, in my study, and in my campus office. I know I don’t refer to the Indo-European roots as much as Ken would have liked, but I would be lost without the *AHD* with its definitions, etymology, Synonyms and Usage notes.

I do little programming these days, and spend most of my time on the history of computing. From time to time I use J as a very convenient means to model, say, a Turing machine or a simple machine-language computer. Then once again I become entranced by the power and beauty of the language as I develop, test and polish my programs. I also think of all the people I have met and worked with during my association with array languages, and the influence they have had on my life and work. I cannot imagine what my professional career would have been like without APL and J.  I cannot imagine, either, how these languages would have been developed without the insight and perseverance of a person like Kenneth Iverson. To him I am profoundly grateful.

Thank you, Ken.

*Keith Smillie is Professor Emeritus of Computing Science at the University of Alberta, Edmonton, Alberta T6G 2E8. His email address is [smillie@cs.ualberta.ca](mailto:smillie@cs.ualberta.ca).*

November 2004
