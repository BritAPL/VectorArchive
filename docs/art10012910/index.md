---
vid: '10012910'
title: 'Meeting: AnOOPL– An Object-Oriented Programming Language'
authors:
- David Piper
byline: by David Piper
volume: '11'
issue: '1'
page: '96'
source: trad/v111/piper111_96.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- APL community
- language design
---

by David Piper
{ .byline }

### Introduction

David’s talk fell naturally into two main sections: firstly a summary of the benefits to the developer of the Object Oriented approach, and secondly some very speculative suggestions as to how APL could take the Object-oriented road with surprisingly little change to the appearance of the language. He clearly felt that it was wrong that companies like Eagle Star are being pushed towards C++ and Smalltalk just so that they can get the benefits of Objects; APL offers something quite different, so a fusion of object concepts with APL notation really should add power to both.

“This is what I want! Current status is “grey matter” only – only 2 days’ experimenting so far”

### Why Objects?

Partly because it is the fashion. A lot of people talk about it, so if APL has it they will talk about APL!

Partly because there are real benefits (see Adrian’s Technical Editorial in Vector 10.4) to the developer, particularly when you are working on large, complex applications. You can expect: enhanced quality; more code re-use; easier and faster development; more “function points per £K”; easier maintenance. Perhaps you’ve heard this all before ...

- enhanced quality comes mainly from better encapsulation. We have all used name prefixing to mimic this, but (as with all manual methods) this is vulnerable to mistakes.
- polymorphism means you don't need to worry what sort of data you have, and inheritance means you only need to worry about what's new. You know that certain key functions are always available. This keeps functions short, so they are easy to write and fix.

APL has had code re-use since the beginning with )COPY, but it is too often ignored. Code-management tools have helped, as have APL2 packaged WS – OO makes this part of the language, and hence makes it automatic.

APLers are also used to short functions, and know that these are easier to write and to maintain. They are also fairly blasé about polymorphism (pity the poor C programmer who needs separate functions to do the same job for integers, unsigned integers, long integers, reals) so they should adapt well to an environment which offers “Point and Grunt” editing and makes modularity a design necessity with a “message passing” paradigm.

So – having established that an AnOOPL should help us to do things better, quicker and more accurately ...

### What AnOOPL?

Should we be designing APL++ in the mould of C++? This implies patching an object layer on top of the existing structure, but there are some very real and fundamental problems with a hybrid solution. C++ has “Instance variables” which are private to each instance of a class (no problem here) but it also has “Class variables” which should be local to each class, but C has no mechanics for handling these, so any class can get at them and (potentially) tweak the behaviour of an unrelated class. We are back to manual discipline as the only means of ensuring proper encapsulation.

There are also lots of nasty complexities lying in the gungy stuff which sticks together the new object-based bits of an application with the older procedural bits. “Complexity gathers in the twilight zone”, as you have to design a very different style of interface on each side of the horizon.

Conclusion – it would be much better to go for a SmAPLtalk which offers true object orientation with no `⎕KITCHENSINK` kludges, but somehow retains the look and feel of APL. Can it be done? Here are some preliminary ideas to set you thinking ...

### How AnOOPL

Can we make a truly pure OOP that still looks like APL? Start by looking at a familiar syntax with different semantics:

```
     ⍺        FN        ⍵
parameter  message   object
```

There is still work to be done to implement the change in semantics, but at least by keeping the syntax, we keep APL!

A simple example ...

```
     APL          AnOOPL

     date←∆DGD 31 3 1993          date←toDate 31 3 1993
     date          date
727653     31-03-1993
     ∆DGC date          dayNumber date
31-03-1993     727653
     2+date          2+date
727655     02-04-1993
     ∆DGC 2+date          dayNumber 2+date
02-04-1993     727655
     2×date          2×date
1455306     Domain Error (no method)
```

APL reveals the internal data type to us – dates are stored as integers, so that’s how we see them. In AnOOPL we are sending the toDate message to a numeric vector and getting back an encapsulated date which (among other things) knows how to display itself, which it duly does when we send it the ‘see you’ message (in APL we have always had the convention that ‘no message’ means ‘display yourself’) on the next line.

If we really want the day-count, we need to send our date object a dayNumber message. It has a method for this, which returns an integer. Similarly, we can send it a `+` message (with a parameter of 2) as it knows how to add numbers to dates, but we cannot sensibly send it a `×` message – whoever heard of multiplying a date by something! It does not have a method for this, so naturally it fails with a Domain Error.

Can arrays be fitted into this scheme? Given that arrays are the foundation of APL, they must also be the foundation of AnOOPL, so how might this be done ... here is another example:

```
     APL          AnOOPL

     nested←date 'char'          nested←date 'char'
     nested          nested
 727653  char      31-03-1993  char
               classOf nested
          ArrayOfObjects
     2⍴¨nested          2⍴¨nested
 727653 727653  ch      31-03-1993 31-03-1993  ch
               classOf ¨nested
           Date ArrayOfCharacters
     2+date 22          2+date 22
 727655  24      02-04-1993  24
```

Any nested or heterogeneous array is a member of class ArrayOfObjects – these closely resemble nested arrays in APL. Polymorphic methods are applied like “scalar pervasive functions”, and “Each” is available to apply methods item by item.

### AnOOPL vs APL – Differences

So far, this talk has stressed the high degree of similarity between APL and AnOOPL, but there are four key differences, some of which arise out of object orientation, and some of which are just the removal of excess baggage (APL should have been like this anyway):

- static scoping. Variables are strictly local or global in scope. Variables which are local to an object are available to all methods of that object.
- no bracket indexing or axis specification. The \`message passing' paradigm requires a clean syntax.
- no niladic functions. All messages must be passed to an object.
- no distinguished names (system functions).

And as a final bonus – no more execute! Instead we can have the remarkable J?like concept of an ‘undespatched message’:

```
     Rank←⍴⍴
     Rank 2 2⍴char
2
     classOf Rank
UndespatchedMessage
     Incr←1+
     Incr 5 2 0 ¯2
6 3 1 ¯1
```

to conclude (with David’s final overhead foil):

## ![figure\_1](piper111_96-fig1.gif)

(webpage generated: 10 October 2007, 16:44)
