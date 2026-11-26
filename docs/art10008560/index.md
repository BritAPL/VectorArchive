---
vid: '10008560'
title: 'Review: “Les APLs Etendus” by Bernard Legrand'
authors:
- Adrian Smith
volume: '11'
issue: '2'
page: '38'
status: not online
issue_tab: n2-october-1994
---

Adrian Smith
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 38](../11/2/VOL.11-NO.2-OCTOBER-1994.pdf#page=40)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.11 No.2</p>
<p>Les APL Etendus(par Bernard Legrand) Masson, Paris, 1994 ISBN: 2-225-84579-4 reviewed by Adrian Smith</p>
<p>A Textbook on Extended APLs APL2, APL*PLUS, Dyalog APL under Windows First somestatistics: this is a substantial and comprehensive textbook of APL comprising over 500 pages in 17 chapters; it covers all aspects of the APL language, and makesclear the differences in dialect between APL2 and the other two implementations,e.g. the Deux Philosophies (page G21) towards expressions likeA B C[2] wherethe Force de liaison differs in APL2. All the chaptersclose with a set of exercises. These are nicely chosen to make you think, avoiding excessive repetition or trick questions which trip up the unwary reader and areliable to put him off APL altogether.It is nicely presented, with good indexing and a helpful set of summary tables showing (for example) all the possible reductions applied to the emptyvector.</p>
<p>Coverage Chapter A covers little history, and introduces the various APL keyboards. Chapter B takes us through variables and assignment, simple arrays of various ranks, and vector notation. It makes clear the differences between mixed and general arrays, and also notes the index primitive 0 offered by APL2 and the extension to bracket indexing MAT[(41 2)(3 4)] offered by Dyalog APL.</p>
<p>Chapters C and D are a straightforward introductionto the core APL primitives (with a good discussion of reduction and behaviour on empty arrays), and the basics of function definition. Again, the different rules for forming the function header (such as the need to include {} around optional arguments in Dyalog) are well explained, Chapter E continues with a discussion oferrors andthe State Indicator (although I must take issue with Bernard s definition of NONCE ERRORas something specifically related to a failed interface call), and has quite a comprehensive comparison of the varioustracing tools available in the different environments.</p>
<p>Chapters F and G areall aboutrestructuring and transformingarrays. In general, Chapter is classical APL, with the addition of selective specification and the use (in APL2) of an axis modifier for + and 4. The discussion of dyadic transpose</p>
<p>38</p>
<p>VECTOR Vol.11 No.2</p>
<p>is particularly thorough and supported with several helpful diagrams. Chapter G walks us through the use of the DISPLAY utility, and explains very clearly the meaning of the various indications it uses to hint at the type and rank of your data. It is also used to illustrate exceptionally clearly the different effects of catenation and juxtaposition on the structure of the resulting vector. This is (in my experience) one ofthe biggest problemsfora flat APLer to overcome in moving to an extended system.</p>
<p>Chapter H covers execute and (in some detail) format. Here the various APLs diverge dramatically, so the APL2 format by example is given a quite separate treatment to the use of OFMY. Both are again well illustrated with examples. ChapterI discusses operators, with some nice examples of inner products, and Chapter J sweeps up all the remaining math functions, with examples of &amp; in curvefitting. We now move on to the environment, with the various fns (such as ORE and OPW) outlined in Chapter K and the basics of shared variables (including DDE from Dyalog APL) in Chapter L. Chapter M is a good basic introductionto file processing in all three APLs and Chapter N introduces the event-handling facilities. Unfortunately these differ so completely in the three environments that a sensible treatmentis moreor less impossible, and I doubt whetherthe coverage is sufficient to enable the reader to program these with total confidence and safety. Finally, Chapter O covers editing, from the ever-present V to the windowedfacilities available with Dyalog. Chapter P is a nice gentle introduction to the Dyalog approach to Windows development (unfortunately the book pre-dates the APL*PLUS II] release) with some good examples andlots of screen-shots. Chapter Q hasthe obvious(but it needs saying) advice on documentation and housekeeping ( Assurez l hygiéne de votre environnement") and the book closes with some thoughts on Why APL Today . Bernard takes many of the accusationslevelled at APL andhits back at (for example) unreadability with some well-chosen counter-examples from Excel.</p>
<p>Summary This is the first really good book I have seen which takes an integrated approach to the extended APLs, rather than grafting general arrays on to the back of an existing text. It deserves to be the Gilman &amp; Rose of the 1990s. Someone should translate it, but please let that someone be an APLer with a smattering of French, nota professional translator who knows no APL!</p>
<p>39</p>
<p>S SOLITON. ASSOCIATES</p>
<p>SHARP APL: the high performance choice SHARP APL is superiorin the rapid developmentof mission-critical applications which meet the ever-changing demands of our customers.</p>
<p>SOLITON provides SHARP APL see for MVS and UNIX with:</p>
<p>+ Superior productivity + Ease in managing shared-fite multi-user applications « Powerful cooperative processing * High performance DBz2interface pos + Applications for end-users and programmers * * Responsive support services</p>
<p>For moreinformation, telephone or FAX: SOLITON ASSOCIATES LIMITED 44 Victoria Street, Suite 2100 Toronto, Ontario, Canada MSC 12 in Europe . Soliton Associates Limited of Canada Tel: (416) 364-9355 FAX: (416) 264-6159 vate (oe US. Soliton Associates Incorporaled Groot Blankenberg 63, 1082 AC Amsterdam, 1160 University Avenue, Suite 111 The Netherlands Rochester, New York, USA 14607 Tal: +31-20-646-4475 FAX: +31-20-644-1206. Tel: (716) 256-6466 FAX: (716) 266-6469,</p>
<p>orvia Internet to: sales@soliton.com</p>
<p>VECTOR Vol.11 No.2</p>
<p>RECENT MEETINGS This section of Vector documentsall British APL Association meetings, and any other events of interest to the APL world. If you have recently attended any gathering which youfeel would beinteresting to Vector readers, please Jet the Editor have a brief note, and we will includeit here. In this issue, we have notes (and photos - thanks to Ray Cannon) on the APL in Business Workshop, as well as first impressions of APL94 at Antwerp.</p>
<p>APLin Business at Swansea: (front Row) Adrian Smith, Jon Sandles, Tony O'Hagan,Ellis Morgan, Bob &amp; Todd Brown, George MacLeod, Gareth Brentnall.</p>
<p>41</p>
<p>VECTOR Vol.11 No.2</p>
<p>British APL Association Meeting APL in Business at EBMS, Swansea</p>
<p>Chairman s Impressions: Whatyou Saw was (moreor less) what you Got by Adrian Smith After a lapse of a couple of months,it is interesting to reflect on what we set out to do at Swansea, and how close we cameto achieving it. Of course the objectives were nevertotally stable we were trying to do something quite new but there were somefixed points in our agenda, and it is reasonable to ask:</p>
<p>+ did wereally show you the newest and best work being done under Windows ona PC?</p>
<p>+ did you pick up any leading edge techniques of quantitative management?</p>
<p>+ were you able to spend the afternoonstaking apart the morningplenaries in a well-equipped laboratory full of fast PCs?</p>
<p>+ wasita revitalising break from the daily routine?</p>
<p>+ did you get to take away some useful code?</p>
<p>These were all promises that we made onthe first page of the invitation: how well do you think we met them? Sofar, those of you who have written or e- mailed have all been remarkably positive (which makes me fee] better), but such samples tend to self-select! I might yet get around to writing to everyone who came, to ask how wecould do it better next time, but meanwhile, here are my views on what we did well, what we did badly, and what we left undone.</p>
<p>The Venue, Format and Organisation Swansea wasborn outof ourfailed bid to nun APL94 at York, and for the much smaller workshop format, I think it was close to ideal. We began by over- estimating the size of the gathering, which caused quite a few headaches as we cut back to match the bookings, but it would have been hard to run such an intensive 3-day workshop with many more people.</p>
<p>The EBMS was a goodself-contained base, the only snag being the need to borrow facilities in the nearby computer lab for some of the more heavy-duty</p>
<p>42</p>
<p>VECTOR Vol.11 No.2</p>
<p>afternoon sessions. However most people found their way around, and in the end mostof the equipmentco-operated, so this somewhat ad hoc approach stood the test just.</p>
<p>Several people commented favourably on the basic structure of running a morning plenary, with everyone attending all the sessions. This had the great advantage of simplifying administration, as you could always be sure that everyone had got the message about the bustimes, or the change of room in the afternoon.I hope the moreesoteric statistical stuff didn t cause the old-time APL hacks too much mental indigestion I thought Derek Bissell s talk on Control Charts wasbrilliant, but then again I learned mystatistics by weighing Kit Kat bars! I honestly think that if we had gonefor a straight-down-the-line Windows + GUIs agenda, we would have turned everyone off by the end of Day-1. It needed breaking up a bit, andtalks like Kath Dowsland s on Genetic Algorithms wereideal.</p>
<p>can take no creditatall for the excellence of the organisation! This wasall set up by the Alans (Sykes and Mayer) and administered most capably by Haydn Williams and Pam.The coffee was ontime, the signposting was pretty good, the overhead projector worked all the time what more can you ask?!</p>
<p>The Main Topics We took a quick poll around the room at the start of the first day (see Jon Sandles notes on page 45), and most people had really come for the experience of Windows programming. We had already warned everyone (Vector 10.4) that this wasnota jolly, and I think we went home feeling mentally quite battered by the amountof new stuff we had been exposed to in such a shorttime.</p>
<p>+ Day-1 wasshortof talk and chalk time for the main topic of programming the GUI APLs,so the afternoon workshop was somethingof a rabble. I had failed to appreciate the problemsof class-control when you take 24 keen APLers, sit them downinpairs in front of a new toy, and try to get them to listen to you.In the end, Duncan and gave up anyattemptat formal teaching and just worked aroundthe various groupsas fast as we could. [t was OK, but we did muchbetter on Day-3 (see Ray Cannon s note on the DLL workshop) when weprepared more material in advance.</p>
<p>+ Day-2 was aboutclient-server, timeseries and graphics. I found Morten Kromberg slecture on the basicsofeffective client-server architectures really superb, andI m sorry | missed the afternoon workshop. Theidea of aroom full of chunky 486 clients all happily running database queries against Morten s dinkylittle SparcStation server was remarkable enough. What's moreit all worked!</p>
<p>43</p>
</details>
