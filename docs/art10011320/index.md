---
vid: '10011320'
title: 'Meeting: NYC JUG 20060314'
authors:
- Devon McCormick
byline: by Devon McCormick
volume: '22'
issue: '4'
page: '50'
source: trad/v224/jug224.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2006
tags:
- conference reports
- APL community
---

by Devon McCormick
{ .byline }

We had a good, meandering meeting that somehow managed to touch on all the agenda items. I’ll present them in the order of the agenda though that’s not necessarily the order in which we touched on them.

For the “Beginner’s regatta”, I presented an update of a topic from last month’s meeting: different ways of waiting for an external command to finish. As is often the case in these matters, there are routines in the standard J libraries superior to my hand-crafted solutions.

In this case, the functions in *task.ijs*, fork and shell, do a fine job of invoking a command and either returning immediately or waiting for it to finish. Using the “wait”command I’ve been invoking to test for any lag in waiting for a command to finish, one could enter

load 'task'

\_1 fork 'cmd /c wait 5'

where the <span class="aplu">\_1 </span>left argument tells fork to wait for the command to finish. The small disadvantage of invoking the command processor “cmd” (necessary because “wait” is an internal DOS command), is that it brings up a DOS window. I haven’t figured out a way around this. For example tests, see [cmdWaitTestingFork.txt](cmdWaitTestingFork.txt).

In the “Show and Tell” section, we looked at a problem posed by Mike Day on the J Forum: find soln to <span class="aplu">a^2 + b^2 + c^2 = (a - b)(b - c)(c - a)</span>. There were a few replies to this, after which Mike clarified that he was looking for solutions over integers. See [findEqnEqualityPrecis.txt](findEqnEqualityPrecis.txt) for a solution. However, I had a method for real numbers that I liked but became interested in the shape of the solution-space. To see this, I needed a way to plot 3-dimensional points.

Fortunately, Oleg Kobshenko provided a way to do this with his *plot3d.ijs* contribution to the forum. The plots are somewhat interesting and there are a number of useful lessons to be learned from different aspects of solving the system of equations, but I intend to write these up later. See [ptsBtw10&100Dis.jpg](ptsBtw10%26100Dis.jpg) for example plots.

## Regular Expression Classification Table

We explored a technique I’ve been using with good success: table-driven file classification using regular expressions. This is a table consisting most essentially of a column of regular expressions and another column of executable expressions. Files sent to a central server during the day are processed by a routine that classifies files according to which regular expression they match, then running the corresponding expression on the files thus selected. See [RegExTableTests.txt](RegExTableTests.txt) for an example.

A most important restriction is that each file be classified by only one of the regular expressions; also, each expression should pick up a likely file. This quickly highlights the necessity of building the test suite alongside the code, a common practice in “Extreme Programming”. However, as the test suite grows in complexity, it, too, needs to be tested. This raises the question of how we test the tests. For instance, we need to be sure we’ll pick up a duplicate classification but we don’t want to pollute our good table with deliberate errors. Dan had some good suggestions on dynamically modifying the table to accomplish this.

## Learning/Teaching Array Thinking

We discussed a problem from the “TopCoder” website involving finding the longest prime-free intervals for a given range of positive integers. See [topcoderfarfromprimeEG.txt](topcoderfarfromprimeEG.txt) for a fuller explanation.

We compared the looping solutions presented with more array-oriented approaches but were stymied to come up with any sort of general scalar-to-array method. Much of array-oriented thinking comes from a different initial approach that might just require lots of practice.

 
