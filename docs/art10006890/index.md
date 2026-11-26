---
vid: '10006890'
title: 'Meeting: AnOOPL – An Object-Oriented Programming Language'
authors:
- David Piper
volume: '11'
issue: '1'
page: '96'
status: not online
issue_tab: n1-july-1994
---

David Piper
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 96](../11/1/VOL.11-NO.1-JULY-1994.pdf#page=98)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.11 No.1</p>
<p>AnOOPL An Object Oriented Programming Language Talk by David Piper; notes by Adrian Smith</p>
<p>Introduction David's talk fell naturally into two main sections: firstly a summary of the benefits to the developer of the Object Oriented approach, and secondly some very speculative suggestions as to how APL could take the Object-oriented road with surprisingly little change to the appearanceof the language. Heclearly felt that it was wrong that companies like Eagle Star are being pushed towards C++ and Smalltalk just so that they can get the benefits of Objects; APL offers something quite different, so a fusion of object concepts with APL notation really should add powerto both.</p>
<p>This is what I want! Currentstatus is grey matter only only 2 days experimenting so far</p>
<p>Why Objects? Partly becauseit is the fashion. A lot of people talk aboutit, so if APL hasit they will talk about APL!</p>
<p>Partly because there are real benefits (see Adrian s Technical Editorial in Vector 10.4) to the developer, particularly when you are working on large, complex applications. You can expect: enhanced quality; more code re-use; easier and faster development; more function points per £K"; easier maintenance. Perhaps you've heardthis all before...</p>
<p>+ enhanced quality comes mainly from better encapsulation. We have all used nameprefixing to mimicthis, but (as with all manual methods) this is vulnerable to mistakes.</p>
<p>+ polymorphism means you don t need to worry whatsort of data you have, and inheritance means you only need to worry about what's new. You know that certain key functions are alwaysavailable. This keeps functions short, so they are easy to write and fix.</p>
<p>APL has had code re-use since the beginning with ) COPY, butit is too often ignored. Code-managementtools have helped, as have APL2 packaged WS OO makesthis partof the language, and hence makesit automatic.</p>
<p>96</p>
<p>VECTOR Vol.11 No.1</p>
<p>APLersare also used to short functions, and know that these are easier to write and to maintain. Theyarealso fairly blasé about polymorphism (pity the poor C programmer who needs separate functions to do the same job for integers, unsigned integers, long integers, reals) so they should adapt well to an environment which offers Point and Grunt editing and makes modularity a design necessity with a message passing paradigm.</p>
<p>So having established that an AnOOPL should help us to do things better, quicker and moreaccurately...</p>
<p>What AnOOPL? Should we be designing APL++ in the mould of C++? This implies patching an object layer on top of the existing structure, but there are some very real and fundamental problems with a hybrid solution. C++ has Instance variables whichare private to each instance of a class (no problem here) butit also has Class variables which shouldbelocal to each class, but C has no mechanicsfor handling these, so any class can get at them and (potentially) tweak the behaviourof an unrelated class. We are back to manual discipline as the only means of ensuring proper encapsulation.</p>
<p>There are also lots of nasty complexities lying in the gungy stuff whichsticks together the new object-based bits of an application with the older procedural bits. Complexity gathers in the twilight zone , as you have to design a very different style of interface on each side of the horizon.</p>
<p>Conclusion it would be muchbetter to go for a SmAPLtalk which offers true object orientation with no DXITCHENSINK kludges, but somehowretains the look and feel of APL. Can it be done? Here are somepreliminary ideasto set you thinking...</p>
<p>How AnOOPL Can we makea truly pure OOPthatstill looks like APL? Start by looking at a familiar syntax with different semantics: a FN w parameter message object</p>
<p>Thereis still work to be done to implementthe change in semantics, but at least by keeping the syntax, we keep APL!</p>
<p>VECTOR Vol.11 No.1</p>
<p>A simple example... APL AnOOPL</p>
<p>date+ADGD 31 3 1993 date«toDate 31 3 1993 date date 727653 31-03-1993 ADGC date dayNumber date 31-03-1993 727654 2+date 2+date 727655 02-04-1993 ADGC 2+date dayNumber 2+date 02-04-1993 727655 2xdate axdate 1455306 Domain Error (no method)</p>
<p>APLreveals the internal data type to us dates are stored as integers, so that s how we see them. In AnOOPL we are sending the toDate message to a numeric vector and getting back an encapsulated date which (among other things) knowshow to display itself, which it duly does when we send it the see you message (in APL we have alwayshad the convention that no message means display yourself ) on the nextline.</p>
<p>If wereally want the day-count, we need to send our date object a dayNumber message. It has a method forthis, which returns an integer. Similarly, we can send it a + message (with a parameter of 2) as it knows how to add numbers to dates, but we cannot sensibly send it a x message whoever heard of multiplying a date by something! It does not have a methodforthis, so naturally it fails witha Domain Error.</p>
<p>Can arrays befitted into this scheme? Giventhat arrays are the foundation of APL,they mustalso be the foundation of AnOOPL, so how mightthis be done... here is another example: APL AnOOPL</p>
<p>nested+date char! nested+«date 'char' nested nested 727653 char 31-03-1993 char classOf nested Arrayofobjects 2pnested 2p nested 727653 727653 ch 32-03-1993 31-03-1993 ch class0f nested Date ArrayOfCharacters 2tdate 22 2+date 22 727655 24 02-04-1993 24</p>
<p>98</p>
<p>VECTOR Vol.11 No.1</p>
<p>Any nested or heterogeneousarray is a member of class ArrayOfObjects these closely resemble nested arrays in APL. Polymorphic methods are applied like scalar pervasive functions , and Each is available to apply methodsitem by item.</p>
<p>AnOOPLvs APL Differences So far, this talk has stressed the high degree of similarity between APL and AnOOPL,butthere are four key differences, some of which arise out of object orientation, and some of which are just the removal of excess baggage (APL should havebeenlike this anyway): + static scoping. Variablesarestrictly local or global in scope. Variables which are local to an object are available to all methodsof that object. + no bracket indexing or axis specification. The message passing paradigm requires a clean syntax. + no niladic functions. All messages mustbe passed to an object. + no distinguished names(system functions).</p>
<p>Andasa final bonus no more execute! Instead we can have the remarkable J-like concept of an undespatched message : Rank+pp Rank 2 2pchar 2 classOf Rank UndespatchedMessage Iner+i+</p>
<p>wane AnOOPL Incr § 20 2 631471</p>
<p>to conclude</p>
<p>final overhead foil): + Pure Object Oriented Language ~ not APL++ with kludges * Itis APL ~ only 4 differences, + Extensions into J-like features undespatched messages ~ possibly tacit definition"??? + Power of arrays and objects</p>
<p>dyalogVAFB)I The Definitive APL for Windows</p>
<p>Clos Fourtetjul</p>
<p>sem pst)</p>
<p>Cauter</p>
<p>Fourle-de-Soutard</p>
<p>Fourtet Franc-Mayne Franc-Pourret du GrandFaurie GrandMayne</p>
<p>Heady...</p>
<p>Experience counts Since we launched Dyalog APL/W at APL92, nearly two years ahead of our nearest competitor, our customers have developed hundredsof successful industrial-strength GUI applications. With the benefit of their experience, we have enhanced and refined Dyalog APL/W into the mature, stable and aboveall useful Windows developmenttool that it is today.</p>
<p>Not only does Dyalog APL/W provide the most comprehensive set of GUI facilities available for any APL, but you can be confident that your workspaces will run unchanged on Unix workstations under OSF/Motif.</p>
<p>With Version 7.0 we have added support for Visual Basic Custom Controls, a powerful built-in Grid object, Numeric, Currency, and Date fields, ToolBar, StatusBar and TabBar objects, automatic context-sensitive Hints and Tips, Metafiles, MDI, 3-D Forms and Controls, a fully customisable Session, an ODBC interface, Namespaces for encapsulation, and a host of other improvements.</p>
<p>That's why Dyalog APL/W remainsthe professional choice. Fora free trial copy, contact Dyadic or your local distributor today.</p>
<p>Dyadic Systems Limited., Riverside View, Basing Road, Old Basing, Basingstoke, Hants RG24 7AL, United Kingdom. Tel:(0256) 811125 Fax: (0256) 811130 Email: sales@dyadic.com.</p>
</details>
