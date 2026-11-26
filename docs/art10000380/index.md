---
vid: '10000380'
title: To Suffer the Slings and Vectors of the BAA
authors:
- Ajay Askoolum
byline: by Ajay Askoolum
volume: '11'
issue: '1'
page: '131'
source: trad/v111/askoolum111_131.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- APL community
- APL in perspective
---

by Ajay Askoolum <span class="affil"></span> <span class="email"></span>
{ .byline }

I have read VECTOR regularly since 1986. During this time, the Journal has consistently echoed the ambivalent sentiment that ‘we must convince the outside world that APL is the most powerful and most productive programming language’ (VECTOR Vol 10 No 3 Page 6) and that the APL community is rapidly becoming ‘a gathering of veterans from the FORTRAN wars! – so the language is threatened with extinction within the current generation.’ (VECTOR Vol 10 No 3 Page 121).

Indeed this is not new : ‘After APL 91, I upset a few people by wondering to myself if it had been a conference or a reunion.’ (VECTOR VOL 10 No 2). I have used the verb ‘echoed’ because on the very rare occasions that the computing press mentions APL these days, it does so purely to decry the fact that the language still exists! Is this a conspiracy? If not how is VECTOR ‘showing its \[APL’s\] facilities in practice, illustrated by successful applications.’?

Since VECTOR does not explicitly state its objectives, I cannot reasonably take issue with the coverage it gives to J and Windows. However is it not presumptuous of VECTOR to join the fashionable crusade of Windows, OOPS and control structures while failing what most readers of VECTOR would regard as its most critical mission: ‘To show that APL is the most powerful and most productive programming language by illustrating its facilities within successful applications in industry.’ Given the absence of readily available reference material on APL, should not VECTOR capitalise on the opportunity to establish itself as the de facto standard for APL?

An extremely useful addition to VECTOR would be a keyboard map showing the APL symbols and a glossary of terms relating to them - on the inside back cover, perhaps? IBM APL2 must be the most widely used interpreter given its environment and supplier, especially with its )LANGUAGE facility. This interpreter is text based. APL\*PLUS/PC must be a close contender for the second most widely used interpreter – as APL\*PLUS II works better as a protected DOS application (no DDE, ODBC or OLE support), it too must be included in the category of text based interpreters. VECTOR itself has not published any findings on the usage of the various types of interpreters. If VECTOR is to contribute to the effort to demonstrate that APL is the most powerful and productive language (I strongly believe that it is), it has to represent APL as it is:

- a language with peculiar symbols
- a language in which the processing of vectors and arrays is intuitive
- and a language without control structures found in other languages.

DB McIntyre’s contribution ‘Language as an intellectual tool : From hieroglyphics to APL’ (IBM Systems Journal Vol 30 No 4 1991) eloquently justifies symbols: “APL’s concise notation helps us grasp the intellectual content of an algorithm without the distraction of extraneous and irrelevant matters prescribed by a machine.”

My point is that APL symbols provide an undeniable advantage – it is free of (spoken) language barriers much as the other language-independent means of communications such as international ground to air signals, semaphore using two flags, road traffic signs etc. These have a consistent international meaning. Arrays and vectors are the most natural concepts in which programmers and non-programmers think of data : CostOfShopping is PriceOfEachItem times NumberOfEachItem not as the sum of the cost of the first item plus the cost of the second item plus .. until the last item. What of control structures? Surely the attraction of APL is that it does NOT have the formal control structures found in other languages.

I would find the introduction of these structures acceptable only in return for tangible benefits. Among these, I rank the following as the top two:

- increase in speed, perhaps arising from compilation of APL code at run time
- APL is no longer workspace bound, as encapsulated in the following type of structure:

```
WHILE NOT EOF(n) ⋄
⍝ While the end of the file tied to N is not reached processes ⋄
⍝ the application code WEND ⋄
⍝ end processing
```

where prior knowledge of the shape of the data is not a prerequisite. A more pertinent consideration is the sort of control structures which APL will appropriate. It must be quite obvious to proponents of control structures that these structures have their own variations and limitations and that it is not a case of adopting the behaviour of these structures in any one specific product.

It is my strong belief that control structures exist in other languages because, unlike APL, their dynamism is restricted to scalars (one at a time, from the first to the nth where n is known in advance) and because of the inadequate use of Boolean logic. In APL, Boolean logic is used to great advantage, thereby removing the need for IF THEN ELSE IF THEN .. structures. An added advantage of this approach is that it overcomes the need for hardcoded algorithms, and is more productive or tolerant of programmer omissions, depending of your point of view.

APL has survived for over 30 years in spite of all the adversity it has encountered. However, in the absence of strong sponsorship, its continued survival is endangered beyond the current generation as suggested.

VECTOR is eminently placed to promote APL in a more active way, by showing its relevance to business and by recruiting new entrants to the realm. How? My suggestions are:

- a section in VECTOR dedicated to the considerations of APL application development which will provide utilities which can simply be transcribed into commercial applications, sponsor and discuss legitimate coding practices etc aimed at the readership with working experience of APL
- a section in VECTOR dedicated to beginners, illustrating the comparative advantage of APL.

For instance, showing how the diagonal of a matrix can be extracted with ease i.e. `1 1⍉10 10⍴⍳100` yields `1 12 23 34 45 56 67 78 89 100` or how APL can generate all five possible results of a comparison of any scalar or nx1 matrix with a minimum and maximum i.e. `¯2` if less than the minimum `¯1` if equal to the minimum 0 if in the range of the minimum and maximum values 1 if equal to the maximum 2 if greater than the maximum.

Thus: with `⎕IO←1 (5 1⍴⍳5)` `INRANGE 2 4 ¯2 ¯1 0 1 2` where the function looks like:

```
     ∇ R←L INRANGE R
[1]   R←(×L-1↑R)+×L-¯1↑R
     ∇
```

- a tutorial service which is free to the student/school but where the resources are provided by APL vendors/sustaining members and experienced volunteer APL users
- making APL visible to the outside world by means of free demonstration disks, school competitions (for sixth forms, say) where the prizes can be tangible (copies of APL interpreters, courtesy of sustaining members) or complimentary (coverage in VECTOR, even free annual subscription)
- consolidating the suppliers of APL into a forum and organising an annual commercial conference (delegates from industry pay a fee) in the UK where the platform is dedicated to the demonstration of APL in business critical applications
- a survey amongst the readers of VECTOR to establish what the readers, (most of whom would be users of APL?) put APL to and how VECTOR can best cater for their needs

The promotion of APL on this scale can be undertaken by VECTOR as it has a wide circulation and is independent of a profit motive. Personal attempts to do so albeit on a much smaller scale is, in my experience, akin to providing a stick to be assaulted with. APL deserves better!

Ajay Askoolum  
42 Hanworth Road  
Redhill  
Surrey RH1 5HT

(webpage generated: 18 October 2006, 03:41)
