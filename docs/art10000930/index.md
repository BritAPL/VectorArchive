---
vid: '10000930'
title: Conformity of APL to ISO APL Standard Extended
authors:
- F.H.D. van Batenburg
byline: by F.H.D. van Batenburg
volume: '21'
issue: '3'
page: '135'
source: trad/v213/eke213.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- standards
- implementations
---

by F.H.D. van Batenburg
{ .byline }

## Introduction

For many years I have been using more than one particular APL implementation. In the process of switching I got acquainted with many handy features found in one implementation and not in others. Some of those unique features I could very well do without, others I love so much that I miss them dearly in other APL implementations. Of course I tried to convince the vendor to add some of those features, but some vendors are interested in such user-requests, others less so.

Why the occasional reluctance to add a beautiful feature that is present in another implementation? I can imagine that pleas for namespaces or dynamic functions (as in Dyalog APL), APL-like structured control (as in APL2C), rank (as in SHARP APL and in APL2C), framed outer product (as in SHARP APL) or component file primitives (as in APLX) are not so trivial to implement, but implementation of assignment operator (Dyalog APL), left and right (APL2C) or indirect copy (APL2) do not seem so hard.

Pondering the question of what are reasonable additions to ask for in an APL implementation, I decided that the reasonable minimum set of features is what is defined in the ISO standard for APL.

It was for this reason that I started to investigate how well various APL implementations conform to the ISO standard.

## APL ISO Standards

Currently there are two ISO standards for APL. The first one, ISO8485, was published in 1989 \[7\]. This standard defines “flat” APL without nested arrays.  After this milestone, work started on the next standard, titled “Programming language APL, extended” to define APL with nested arrays. This resulted in a final draft thanks to the work of many, but not in the least thanks to the dogged perseverance of chairman Lee Dickey and the enthusiastic and vigorous debating desire and the hard work of Leigh Clayton. A few years ago this draft was accepted without any changes as ISO/IEC13751, so now we also have a standard for APL nested arrays \[8\]. It was this second standard, with nested arrays, that I used as a yardstick for the various APL implementations.

The first thing I did was to go through the standard and jot down all the items that were not in the old APL\\360 books. This resulted in a selection of items that I intended to check. After all, I had no intention of checking for the occurrence of basic items that have been in all APL implementations for decades, such as addition, subtraction, multiplication, reshape and similar functions. This did not mean that simply checking for unfamiliar symbols would be sufficient, there were also checks to be made on whether the existing symbols were implemented in the way the standard prescribes; in particular if the functionality of the standard extends that of the first standard. This browsing resulted in (a superset of) the list that you see in column one of the table.

Next I checked the six APL implementations that I had at my disposal (either the code or the manuals or Help files) for their implementation of each item in that list. As I said, I consulted the Help files or the manuals and often did a quick check by simple typing an appropriate expression. For APL2000 I consulted program and documentation of version 4 \[1\], for APLX program and documentation of version 1.1 \[2\], for APL2C program version 5.0.3, for Dyalog APLdocumentation and program version 10 \[4\], for APL2 I used the language reference and program version 2.0 \[6\] and finally for SHARP APL I used the Unix documentation suite \[5\] downloaded from internet with most manuals dated around 2000.

The results of my findings were coded by + and – marks in the table.

## Implementation Scores

In general, investigating the various items in the table and assigning a proper plus/minus was not much of a problem. Most of the decisions I had to make were rather easy: a particular symbol was available or not.

If the symbol was available, the subtler question arose of whether the function worked as defined in the standard or not. Mostly this was also rather straightforward to answer with “yes”, but not always, e.g. the symbol of “Table” is defined in APLX, but its function is different from the standard. A similar case is GCD (`∨`) and LCM (`^`); these symbols are present in all implementations for the functions “and”and “or” but their extended functionality for GCD and LCM is not always implemented. Another case was `⎕DL` that returned its result on screen in APL2C, but not as a result. I choose to consider this a simple bug that will be repaired, hopefully soon, rather than a design decision.

A few choices that had to be made were more questionable. Another investigator might have taken another position. Those choices are elaborated upon below.

First of all I decided not to take into account component files, shared variables and complex numbers. These functions are optional facilities in the standard and I felt that these should be omitted for this investigation. Maybe these features could be the topic of another article. Probably APL2 would have scored better if I had included these optional features. On the other hand, some other choices might have worked out in the reverse direction, e.g. error trapping.

Error trapping functionality was also simply taken from the standard. This ignores the fact that some APL implementations have a much more powerful way of error trapping so they have no need for the, probably in their view inferior, error trapping that is defined in the standard.

As could be expected the biggest problems rose with the scores of SHARP APL.

A non-issue was reduction. SHARP APL does this quite differently from other implementations. It manipulates frames, which roughly looks like it reduces over the first dimension (this is a superficial description, in essence the ramifications are more profound). So the initial plusses that I had for all the implementations regarding reduction suggested a universality that was not there. The standard however, defines the reduction operator in both ways, so I ended up with “plusses” for all implementations of reduction.

The real problem was nested arrays. SHARP APL has quite a different brand and uses different symbols (for sometimes comparable operations). Nested arrays in SHARP APL are similar to those in J. This type is sometimes called “boxed” arrays (because the data is boxed in and no longer available for manipulation except by specially designed operators). It is also called “grounded” arrays (because each enclosure increases the enclosure-level by one including scalars, whereas in ungrounded nested arrays you can enclose scalars as much as you want to without ever rising above level 0).

Theoretically speaking one could argue that SHARP APL has boxed arrays and can add nested arrays whenever they want to atone to the standard. The reasons for this is that boxing is done by &lt; and unboxing by >, which creates a new datatype “boxed array” rather than a special structure of familiar data. So one can have a nested array with an element of datatype boxed array. Eventually I chose to consider boxed arrays to be a rather weak type of nested arrays and awarded both minus and plus for “comparable” functions using other symbols.

In the end there appeared several items that were available in all implementations. These I removed from the table in order to show only the items that are still a problem for one or more vendors. Examples of such removed functions are “without” (the dyadic “`~`”), match and compress with integers (now called replicate). The results are presented in the following table.

<table border="1" cellspacing="0" cellpadding="4">

  <tr valign="top">

    <td> </td>
    <th>APL2000</th>
    <th> APLX </th>
    <th> APL2C</th>
    <th> Dyalog APL</th>
    <th>SHARP APL</th>
    <th>IBM/APL2 </th>
  </tr>

  <tr>

    <td>


  Table: <tt>⍪</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  - <sup><a href="#fn1">1)</a></sup></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  Join: <tt>⍪</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  N-wise reduction</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Depth: <tt></tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  + <sup><a href="#fn2">2)</a></sup></td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Each: <tt>¨</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Enclose: <tt>⊂</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  + <sup><a href="#fn2">2)</a></sup></td>
    <td width="81" valign="top" class="Normal">


  ±<tt>(&lt;)</tt></td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Disclose: <tt>⊃</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  + <sup><a href="#fn2">2)</a></sup></td>
    <td width="81" valign="top" class="Normal">


  ±<tt>(&gt;)</tt></td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Enlist: <tt>∊</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  + <sup><a href="#fn2">2)</a></sup></td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  First: <tt>↑</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  + <sup><a href="#fn2">2)</a></sup></td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Duplicate: <tt>⍨</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  ±<tt>(⊂)</tt></td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  Commute: <tt>⍨</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  ±<tt>(⊂)</tt></td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  Rank: <tt>⍤</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  Unique: <tt>∪</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  ±<tt>(↑)</tt></td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  Left: <tt>⊣</tt><br>
  right: <tt>⊢</tt></td>

    <td width="67" valign="top" class="Normal">


  -<br>
  -</td>
    <td width="67" valign="top" class="Normal">


  -<br>
  -</td>
    <td width="67" valign="top" class="Normal">


  +<br>
  +</td>
    <td width="74" valign="top" class="Normal">


  -<br>
  -</td>
    <td width="81" valign="top" class="Normal">


  +<br>
  +</td>
    <td width="88" valign="top" class="Normal">


  -<br>
  -</td>
  </tr>

  <tr>

    <td>


  LCM: <tt>^</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  GCD: <tt>∨</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  -</td>
  </tr>

  <tr>

    <td>


  EventMessage: <tt>⎕EM</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  EventType: <tt>⎕ET</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  EventSimulate: <tt>⎕ES</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Stop: <tt>⎕STOP</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Trace: <tt>⎕TRACE</tt></td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  ExecuteAlt: <tt>⎕EA</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Transfer Form: <tt>⎕TF</tt></td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Defined Operators</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  )NMS</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  )OPS</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  -</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  )SIC</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="74" valign="top" class="Normal">


  -</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  )SINL</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  +</td>
    <td width="67" valign="top" class="Normal">


  -</td>
    <td width="74" valign="top" class="Normal">


  +</td>
    <td width="81" valign="top" class="Normal">


  +</td>
    <td width="88" valign="top" class="Normal">


  +</td>
  </tr>

  <tr>

    <td>


  Total -</td>
    <td width="67" valign="top" class="Normal">


  17</td>
    <td width="67" valign="top" class="Normal">


  9</td>
    <td width="67" valign="top" class="Normal">


  8</td>
    <td width="74" valign="top" class="Normal">


  12</td>
    <td width="81" valign="top" class="Normal">


  15.5
  <sup><a href="#fn3">3)</a></sup></td>
    <td width="88" valign="top" class="Normal">


  10</td>
  </tr>

</table>

## Results

The total score of ISO deviations is summarised in the bottom line of the table. For this summary I charged one penalty for a “`-`”and 0.5 for the “`+/-`” for boxed array functions.

You can see that in the end APL2C, APLX and APL2 have the best (lowest) score. At the other end of the spectrum are SHARP APL and APL2000 with the worst score. Dyalog APL lies more or less in between.

The good score of APL2C did not surprise me very much. The implementer Tilman Otto used the functionality of APL2 as a reference and in private conversations he expressed his interest in also agreeing with the ISO standard.

The third good rating, APLX was also no surprise. This APL implementation originates from APL.68000 for Mac. For a long time this has been a very powerful APL for (first Atari and later) Macintosh and the MicroAPL team was (and is) always interested in listening to user comments.

Dyalog APL does not rate badly with 12, but not very well either. It has a lot of handy features that are nòt in the standard and I wish that other vendors would implement them, but being not ISO I will ignore them for the moment. On the other hand it misses several items of the standard. Some of those deficiencies are not too bad; for example the fact that )SIC is missing is not so much of a problem as )RESET gives the same functionality. Others such as left, right, LCM, GCD, rank and `⎕TF` are more serious deficiencies though.

The good score of APL2 was also not surprising. I already had the feeling that IBM its implementation of nested arrays has laid the groundwork for some discussions in the standards committee so it is not so surprising that the ISO standard agrees not too badly with the IBM implementation (or is it the other way around?).

As expected, SHARP APL does not rate well. This is mainly due to its boxed arrays. This was to be expected, of course. Some people with more experience in boxed arrays than I have and who I trust as fairly objective assured me that in the long run one get used to handling them. Nevertheless they also confessed to me that they found boxed arrays clumsier to handle and they would rather have nested arrays though. It would be interesting to see if many people share this opinion and if Soliton can be convinced to add nested arrays in the future. After all, they also felt the need after many years to standardise and implement the control structures in the ugly and non-APL-ish way that APL2000 has introduced.

It was a surprise to discover that the rating of APL2000 was so bad, worse even than the rating of SHARP APL. I can only hope that this might stimulate APL2000 to invest some effort in adding functionality to their implementation to atone for the ISO standard.

## Conclusion

Did this survey bring me the enlightenment I was after? Partly, and partly not. I am now convinced that some of my wishes were not unreasonable; rank, left, right and commute are part of the standard and should be available in every APL. Some of my other wishes such as such as assignment operator, APL-ish control, indirect copying, namespace and dynamic functions should probably wait for the next “extra-extended” standard.

F.H.D. van Batenburg; Leiden, 11 June 2005 <p class="I0flushindent">
F.H.D. van Batenburg<br>
Theoretical Biology of Leiden University<br>
Kaiserstraat 63<br>
2311GP Leiden<br>
The Netherlands<br>
email: Batenburg@rulsfb.LeidenUniv.nl<br>
http://wwwbio.LeidenUniv.nl/~Batenburg/index.html</p>

## References

\[1\]   APL2000 (2001):  
4.0 User manual  
4.0 System Functions manual.  
Help files version 4

\[2\]   APLX(2002):  
APL language manual version 1.1.  
Help files version 1.1

\[3\]   APL2C (2001):  
User manual version 5

\[4\]   Dyalog (2003):  
Language Reference  manual version 10  
Help files version 10

\[5\]   I.P.Sharp Asociates (2000):  
SHARP APL for Unix; Documentation Suite. UW-000-0401 (9905).

\[6\]   IBM (1994):  
Language Reference. SH21-1061-1.  
(no date) APL2 language summary SX26-3851-00

\[7\]   ISO (1989):  
Programming Language APL. ISO8485(1989).

\[8\]   ISO (2001):  
Programming languages, their environments and system software interfaces -- Programming language Extended APL. ISO/IEC13751 (2001).

***

<a id="fn1"></a> <sup>1)</sup> Implementation error.  
<a id="fn2"></a> <sup>2)</sup> Using `⎕ML←2`  
<a id="fn3"></a> <sup>3)</sup> +/- was counted as 0.5
