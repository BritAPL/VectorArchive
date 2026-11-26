---
vid: '10003710'
title: 'Letter: Sudoku Madness'
authors:
- Neville Holmes
byline: from Neville Holmes (nholmes@leven.comp.utas.edu.au)
volume: '22'
issue: '2'
page: '16'
source: trad/v222/holmes.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- competitions and puzzles
- programming techniques
- education
---

from Neville Holmes (nholmes@leven.comp.utas.edu.au)
{ .byline }

I just read your Sudoku message on the J Forum. By a strange coincidence, one of my students chose Sudoku for his J assignment.  He was having a lot of trouble, so I wrote the script below to give him a start. He didn’t get all that far with writing some player support but I’ll be working with him after I come back from overseas.

Because I’m leaving early Friday for a week of meetings in the US I haven’t got time to do much more than send you the code. For it is that I’ve commented it fairly fully for the student’s sake, but against it is that I left the assignments global to make it easy for him to test it.

Perhaps its interest is that I’ve generalised it way beyond the 3×3×3, which is, I think,  a bit of a straight jacket. Also, you should be able to jazz up the code because I’m not an expert by any means, and the code is just one evening’s work.

```
 NB.
 NB. General Sudoku Arrays
 NB.
 NB. Generate an array of numbers, for example, shape 2 3 4 5
 NB.   which is thought of as a 2x3 array of 4x5 rectangles,
 NB.   or a 2x4 by 3x5 array of numbers, where
 NB.   (a) no number is repeated within
 NB.       0. any 4x5 subarray,
 NB.       1. any 2x4 subarray, and
 NB.       2. any 3x5 subarray,
 NB.   (b) the numbers are whole
 NB.          from 1 to the maximum of 4x5, 2x4, and 3x5.
 NB.
 NB. Final Functions : Dsply Fnl  (try "Dsply Fnl 2 2 3 3" or "Dsply Fnl 3")

 NB. Some Utility Functions

 Dsply =: <"2       NB. Dsply y : boxes the 4x5 subarrays
 PnDy  =: ,"2@(0 1&|:)@:(,"2)@(0 2&|:)
                    NB. PnDy y : converts to 8x10 array
 InDy  =: Dsply@(]$({.~ -@(*/)))~ >@(1&{)
                    NB. x InDy y : constructs & displays intermediate array
 Vldt  =: 2&>.@(4&$)@,@:>.
                    NB. Forces 4 integers greater than 1
 Mxmm  =: >./@(*/)@:((2 3$2 0 1 3 2 3)&{)
                    NB. Calculates the maximum of 4x5, 2x4, and 3x5

 NB. Functions to set up running list of boxes
 NB.
 NB. 0. The next index (starts as <: of 2x3x4x5)
 NB. 1. The overall array as a list (starts as an empty list)
 NB. 2. 2x3 table of lists of numbers used so far for each 4x5 subarray
 NB. 3. 3x5 ditto for each 2x4 subarray
 NB. 4. 2x4 ditto for each 3x5 subarray

 SSAry =: ($&a:)@:{  NB. sets up the subarray of empty boxes
 Strt  =: <:@(*/) ; (i.0)"_ ; 0 1&SSAry ; 1 3&SSAry ; <@(0 2&SSAry)

 NB. Functions to calculate the next number
 NB.
 NB.   Require shape as left argument, running array as right

 Ndx   =: #: >@{.   NB. 4 element index to next number
 UsdFS =: <@(2&{.)@Ndx >@{ >@(2&{)@]
                    NB. x UsdFS y : gets 4x5 used so far
 UsdSS =: <@(1 3&{)@Ndx >@{ >@(3&{)@]
                    NB. x UsdSS y : gets 2x4 used so far
 UsdTS =: <@(0 2&{)@Ndx >@{ >@(4&{)@]
                    NB. x UsdTS y : gets 3x5 used so far
 UsdS  =: UsdFS ~.@, UsdSS , UsdTS
                    NB. x UsdTS y : gets all numbers used so far
 NxtNr =:  >:@i.@Mxmm@[ (0:`({~?@#)@.(*@#))@:-. UsdS
                    NB. x NxtNr y : selects from numbers not yet used
 AdNN  =:  (NxtNr <@, >@(1&{)@]) 1} ]
                    NB. x AdNN y  : puts the next number to list front

 NB. Functions to put away the next number in the three boxes
 NB.
 NB.    Further updates the running array

 NwN   =: {.@>@(1&{)@] NB. picks out the new number
 NwFS  =: (UsdFS <@, NwN)`(<@(2&{.)@Ndx)`(>@(2&{)@])}
                     NB. x NwFs y : updates the 4x5 subarray
 NwSS  =: (UsdSS <@, NwN)`(<@(1 3&{)@Ndx)`(>@(3&{)@])}
                     NB. x NwFs y : updates the 2x4 subarray
 NwTS  =: (UsdTS <@, NwN)`(<@(0 2&{)@Ndx)`(>@(4&{)@])}
                     NB. x NwFs y : updates the 3x5 subarray
 UdtRA =: <:@>@{.@] ; 1&{@] , NwFS ; NwSS ; <@NwTS
                     NB. x UdtRA y : updates the running array

 NB. Functions to do all, and to clean up
 NB.
 NB. a =. Fnl y : to get the array
 NB. Dsply a    : to see the boxed structure
 NB. PnDy a     : to see the 2D structure

 Good =: -.@(0&e.)@>@(1&{)
 Once =: ([ UdtRA AdNN)^:(Good@:])^:(*/@:[)
                     NB. (Once Strt) y : one attempt at the whole
 Try  =: ('No'"_`InDy@.(Good@]) (Once Strt))@Vldt
                     NB. Try y : works for once
 Cln  =: Strt@[^:(-.@Good@:])
                     NB. (([ Cln Once) Strt) y :
 Fnsh =: [([ Once Strt@[)`]@.(Good@])^:_ (Once Strt)
                     NB. Fnsh y : repeated attempts at the whole
 Fnl  =: ($ >@(1&{)@Fnsh)@Vldt
                     NB. Fnl y : the structure extracted
                     NB. Dsply Fnl y : to see the structure
```

***
