---
vid: '10005410'
title: 'At Play with J: Control Structures'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '11'
issue: '1'
page: '136'
source: trad/v111/mcdonnell111_136.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- language design
- implementations
---

by Eugene McDonnell
{ .byline }

There have been many proposals for control structures in APL systems before, and there are now current several APL systems which have them. This article will describe the control structures which are made available in the new release of J (version 8).

The material is largely drawn from the latest edition of the “J Introduction and Dictionary”, available from Iverson Software Inc, 33 Major Street, Toronto, Ontario, Canada M5S 2K9. The documentation which I have seen was in a preliminary form at the time of writing, so be sure to get a copy of the official document if you would like to use these new facilities.

This article assumes that you, like me, are not overly familiar with the general notion of control structures. If in fact you are a long-time user of Fortran or Algol or C, please forgive these callow comments.

The control words introduced are:

```
break.
catch.
continue.
do.
else.
elseif.
end.
goto_<name>.
if.
label_<name>.
return.
try.
while.
whilst.
```

The four control words if., try., while., and whilst. mark the beginnings of control structures that are each terminated by a matching end. control word.

The control words while. and whilst. differ in that the test block in a whilst. statement is skipped the first time (the “st” in whilst. can be thought of as meaning “skip test”) whereas in a while. statement it is always executed. As a consequence, the execution block in a whilst. statement is always executed at least once, but in a while. statement, it may execute zero times.

The words do. and else. and elseif. occur within control structures, separating them into blocks. The control word forms goto\_&lt;name>. and label\_&lt;name>. represent an infinite family of possible control words, for each of which &lt;name> is a different text. For example, one may write:

```
goto_ahead.
...(statements)
label_ahead.
```

or:

```
label_back.
...(statements)
goto_back.
```

A block consists of zero or more control words and sentences that are grouped together by control words occurring within a control structure. The role of blocks is summarized as follows:

```
if. T do. B end.
if. T do. B else. B1 end.
if. T do. B elseif. T1 do. B1 ... elseif. Tn do. Bn end.
try. B catch. B1. end.
while. T do. B end.
whilst. T do. B end.
```

Words with B or T denote blocks. If the first (or only) atom of the result of the last sentence executed in a T block is zero, the B block following is not executed, otherwise it is executed.

In a series of elseif. Ti do. Bi, if the Ti are not exhaustive, it is good practice to put a final elseif. 1 do. Bz, where Bz is a block covering the default procedure when all else has failed, so that Bz is executed when no other test has succeeded.

Perhaps an example will make some of these details more concrete. The program p23 represents a crude but effective process for determining x as the two-thirds power of y. for y. any positive cube.

The statement numbers are not part of the program. They are shown only for reference purposes.

```
   p23=.3 : 0
1  v=.0
2  w=.1
3  while. y.~:z=.v*x=.v*v do.
4    if. z>y. do. v=.v-w=.-:w
5           else. v=.v+w=.+:w
6    end.
7  end.
8  x
   )
```

Statements 1 and 2 give initial values to the local variables v and w. Statements 3 through 7, inclusive, are a while. statement. Statements 4 through 6 inclusive are an if. statement. The T block in the while. statement compares the argument (y.) for inequality with z, which is the cube of v. If they are unequal, the result of the T block will be 1 (nonzero) and the if. statement will be executed. The T block in the if. statement determines whether z is greater than y., and if it is the block following do. will be executed. Otherwise, the block following else. will be executed. The block following do. halves w and subtracts this from v; the block following else. doubles w and adds this to v. Continuing this process will eventually create a z which is equal to y., making the result of the test zero, and when this occurs the if. statement will no longer be executed. Line 8 will then be executed, giving as the program’s result the value of x, since the result of a program is the result of the last sentence executed that was not in a T block.

The purpose of the try. and catch. blocks is to permit recovery from a failure in execution. In a statement such as try. B catch. B1 end. if block B executes successfully, then B1 is not executed. If the execution of block B fails, then block B1 is executed.

The behaviour of the remaining control words can be summarized as follows:

```
break. Go to the end of a while. or whilst. control block
continue. Go the top of a while. or whilst. control block
goto_<name>. Go to the statement beginning with label_<name>.
label_<name>. Target of goto_<name>.
return. Exit the program
```

I must add a warning: I have not yet had access to the latest released J system, so that the information above is based on a reading of a preliminary version of the latest “J Introduction and Dictionary” and not on an actual machine execution. I have, however, submitted the program to two people who had access to the latest J system, and am told that it executed properly.

(webpage generated: 14 October 2007, 18:09)
