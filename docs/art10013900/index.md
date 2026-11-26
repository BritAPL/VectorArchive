---
vid: '10013900'
title: 'Challenge: A Namespace-aware Function Tree'
authors:
- Ray Cannon
volume: '15'
issue: '4'
page: '39'
source: trad/v154/chall.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1999
tags:
- competitions and puzzles
- development practice
- programming techniques
---

## Challenge: “Design a Utility to Produce a Function Calling Tree under Dyalog APL/W”

*issued by Ray Cannon*

One of the first utilities I wrote under Dyalog APL/W produced a calling tree for a specified function. Use of the Dyalog system function `⎕REFS` makes the task easy, and this formed the basis of the code used in the “UCMD” utility “COPYUTILS” (see [UCMD](resource/ucmd.htm)).

However, this code was written before namespaces were introduced, and now needs to be updated to take them into account.

The challenge then is to write a utility that, given the name of top function as its argument, returns a list of all the functions called from that point down (including the top function).

It should be able to handle simple explicit calls to functions in other namespaces (via the &lt;name space path>.&lt;fn name> syntax), but cannot be expected to handle run time variable calls (*e.g.* Function name resolution via the `⎕PATH` system function, or via either the monadic or dyadic execute).

Each name in the calling tree should be qualified with its namespace path, either relative to the top function or as an absolute path. By this I mean that if the top function argument is AA and it calls BB and UTIL.CC, and BB calls #.ZZ the result would be:

AA  
BB  
UTIL.CC  
\#.ZZ

If the argument was given as #.app.AA, then the result would be

\#.app.AA  
\#.app.BB  
\#.app.UTIL.CC  
\#.ZZ

It must be able to handle recursive calls and should only list a called object once.

The main problem I have had in trying to write this code is that the `⎕REFS` system function can no longer be used as it does not treat a function name (which includes namespace path information) as a single reference. Thus for example, a call “#.app.BB” produces two references “app” and “BB”, and it cannot differentiate between calls to “#.app1.BB” and “#.app2.BB”.

The best solution(s) received will, I hope, be published in a future issue of *Vector* and incorporated into the UCMD code available from www.vector.org.uk.

***

© 1999 British APL Association
