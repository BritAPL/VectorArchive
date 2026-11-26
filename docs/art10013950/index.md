---
vid: '10013950'
title: Do I Have to Use Recursion?
authors:
- Anne Wilson
volume: '16'
issue: '1'
page: '10'
source: trad/v161/anne161.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1999
tags:
- programming techniques
- competitions and puzzles
---

<table width="90%">
<td>From: Anne Wilson</td>
<td align="right">6 June 1999</td>
</table>

## Do I Have to Use Recursion?

My problem started when I tried to do the *Limbering Up* exercise in Vector 15.4. All went well until I met the line “Oh, one more thing, TREE must be recursive. That is, it must call itself.” By the time I read this I had mentally solved the problem, and nearly finished coding it, allowing for recursive data but without the use of recursive programming. Why should TREE be recursive, and whatever will these recursive calls do? My other problem was the variable shown; because my code is not recursive I have no use for such a variable. Is there a hidden assumption here that because the data is recursive it requires a recursive function?

As a professional programmer, and proud to be one too, I have made a study of program design. My aim has always been to produce the simplest program that will solve the problem. Sometimes one starts to write code and thinks *“There must be a simpler way to solve this”*, and starts again finding the simpler method, usually by deliberately designing before starting on the code writing.

There are three main components in a program – input data, output data and processing. If the program design mimics the structure of one of these components then the programming is not adding extra complexity; if it mimics the simplest of these structures then it is producing the simplest program. (Obviously the programmer is always aware of the other components when making design decisions.) From this it follows that a program only needs to be recursive if all three components are recursive.

But before reaching this stage the program designer should see if a restatement of the problem will result in a simpler program. An obvious example is the summation of a geometric progression where the problem is usually stated in a recursive way. The recursive method calculates the next term, and then calls itself, returning with the accumulating sum; the accuracy is determined by a criteria embedded in the code. However, if you know the formula for the sum all that is required is one calculation; as an added bonus the answer is absolutely accurate (except for limitations of the machine). This requires the inclusion of knowledge, which may be unavailable to the person requesting the program.

In my measurement of complexity recursion is high, looping medium and a single calculation is lowest and hence most desirable. In APL terms looping should include any function that is hiding a generated loop, such as *each* and some *outer products*. Sometimes it can be demonstrated that looping or recursion will be required, because some of the input data can only be determined after earlier data has been processed.

I solved the *Limbering Up* with a loop to find the order that boxes will be shown, and their depth in the diagram. After that I used a single calculation to place the names, boxes and lines onto the output diagram. But I have also effectively restated the problem as I have knowledge that all tree relationships can be described by the *depth* of each box in the diagram and its position in a *traipze* round the tree diagram; and that once the data is in this format all the processing can be done in single statements without recourse to loops or recursion. Note that a true mathematical tree cannot be recursive - but that tree structures can be used to describe data that is recursive.

Now you have probably already thought what a simpleton I must be to think programs are so simple! Many of the nasty complications arise at the input and output stages, and they just have to be coded in but do not affect the structure of the program; others are due to conditional processing; but all is not lost even then if an OOPS approach is used to isolate these complications. Where a program performs several functions each one should be treated separately, so that a large system to input data, perform BOMP calculations, produce reports, etc etc. will have each component designed separately, with the overall system design being another part of the design.

***
