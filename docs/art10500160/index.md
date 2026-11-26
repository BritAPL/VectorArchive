---
vid: '10500160'
title: A newbie’s discovery of APL
prefix: Review
authors:
- Rebecca Burriesci
byline: Rebecca Burriesci (rburriesci@gmail.com)
volume: '24'
issue: '2'
page: '14'
source: content/printed/242/burriesci.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- reviews
- education
---

Rebecca Burriesci (rburriesci@gmail.com)
{ .byline }

<cite>A Practical Introduction to APL 1&amp;2</cite>  
<cite>A Practical Introduction to APL 3&amp;4</cite>  
56pp & 196pp, Graeme Robertson, Basingstoke, 2008

Meant for newbies, Graeme Robertson’s *A Practical Introduction to APL (1&2 and 3&4)* seemed perfect for me because I've only just heard about APL. My background is in C++/C# .net/JAVA, although I've dabbled in C, PBasic, Ruby, PHP, JavaScript, and what I think will help me the most with APL, Matlab. I started programming when I was 10, which has impressed on me that all good introductory books are written so as to be appropriate for a pre-teen. As I go through the books, I’ll let you know whether Robertson can clear that high bar while introducing a new user to the subject.

## Getting started

The structure of the first book is split into day-long modules, each Day into a couple of sessions, and each session into six or seven lessons. The lessons generally start right away with some examples, then provide some notes or explanation, and occasionally end with exercises. The book is designed to complement a class on APL, complete with interspersed questions to ask a tutor and a certificate of achievement on the back cover.

There is a very brief introduction, but it doesn’t answer the who-what-why question of most introductions to programming books. The introduction also doesn’t explain the style conventions of the book – on the first page of lesson 1, I saw eight different font styles. The introduction does include Robertson’s claim that APL is simple and easy to learn, and encourages easy experimentation.  We’ll see if that claim is right.

The first problem I ran into is that I’m not taking the class.  Lesson 0 began by telling me the APL session should be visible and ready to interact with, which of course it’s not.  I don’t see a reference or appendix that tells me where to go to get whatever software Robertson is using.

Fortunately, I was able to get APLX, an APL interpreter, from MicroAPL \[3\]. The install was extremely simple and worked with only a few small hitches (which had more to do with me being too lazy to read the instructions and forgetting to install as root).

## Symbols and more symbols

After going through the first few sections, I can easily see why Robertson started with us exploring the keyboard and filling in a page with a blank keyboard with where our symbols are.  There are over 200 symbols in this nutty language! An index in the book with a summary per symbol would have been nice, as during the examples I found myself constantly flipping back to review what a symbol does.

The first surprising thing Robertson calls out is the lack of order of operations.  I guess with over 200 symbols for mathematical notation it probably is easier just to work right to left.

## Flexibility of lists, arrays and matrices

Starting with lists, I began to understand why APL is powerful and easy to experiment with. Robertson does a great job in showing, rather than just telling, the awesomeness of lists. To start with, the list notation of spacing makes working with lists and matrices easier than with any other language I’ve seen.

The best part is that the interpretation of the list of numbers is up to you – sometimes it is a list as a whole, sometimes an array of individual elements.

For example, here we generate a list of numbers from 1 to a scalar

```
     ⍳3
```

we can act on each element individually

```
     - 1 2 3
¯1 ¯2 ¯3
```

or sum the elements in the list

```
     + / 1 2 3
6
```

or multiply each matching element on each list

```
     1 2 3 × 1 2 3
1 4 9
```

Here we can use the reshape function `ρ`. We make the list as a whole have 7 numbers:

```
     7 ρ1 2 3
1 2 3 1 2 3 1
     7 ρ ⍳3
1 2 3 1 2 3 1
```

and now we are going to catenate that with another list

```
     (7 ρ ⍳3),5 6
1 2 3 1 2 3 1 5 6
```

and now turn it into a matrix

```
     3 3 ρ (7 ρ ⍳3),5 6
1 2 3
1 2 3
1 5 6
```

Have you ever seen anything so elegant?

Most languages fumble through this and make you constantly redefine the data explicitly. However, APL just lets you very naturally use lists to represent a grouping or a sequence of numbers as needed.

Robertson does an excellent job of showing us all the different operations you can perform on lists, and how easily to generate lists from scalars, matrices from lists, and lists and scalars from matrices, and round and round it goes.

## Fun with functions

A language can’t be complete without providing the user with a way to specify functions.  The syntax for APL is a little awkward, starting and ending the function with a special symbol. Reminiscent of Basic, we can branch to a line number or a label in the function.

The problem is that Robertson doesn’t tell us the rules for defining functions, just gives a few sentences and a few examples (one of which has a bug in it). I had to go through the examples carefully before I found that branching to line 0 exits the function.

As I’ve now come to expect, there is another level to functions – the function fixer and execute commands.  You can turn a string into a function, or just execute a string as an expression. This continues APL’s flexibility with lists in treating data as data and interpreting it as you want at will.  Is it a string?  A function?  An array?  A matrix?  You decide.

But the coolness factor isn’t just in executing strings – you can write programs that write programs. This is so interesting that I wish Robertson had given it more than a single line of explanation and short example.

Therefore, I decided to honour the coders before me and do something steeped in tradition – write a really inefficient factorial function.  With only Robertson’s book at my side, can I write a program that writes the factorial function for me?

I started by writing the simple recursive factorial function.  This proved harder than I thought because Robertson doesn’t explain how to do conditional branching, and I had to go over the examples carefully before I found what I needed in the looping sum example.

Recursive function:

```
[0] A←FAC N
[1] →(N=0)ρ4
[2] A←N×(FAC N-1)
[3] →0
[4] A←1
```

Here is my favorite factorial function demonstrating the power of the built in symbols:

```
[0] A←FAC2 N
[1] A←× / ⍳N
```

And lastly, here is a function that creates a function to just do the one factorial problem, and then executes it:

```
[0] A←FAC3 N
[1] ⎕FX 2 9 ρ 'A←FAC_',⍕N,'  A←× / ⍳N'
[2] ⍎'FAC_',⍕N
```

It took a bit of playing around with the symbols and syntax to be able to write the factorial functions, but I was eventually able to figure it out from Robertson’s descriptions of other functions and symbols.

## Reduce and Scan

The reduce operator deserves a little time and attention for making operations that are really messy to express in most languages, really easy to write in APL.  It applies a function left argument to a list right argument.

In the second factorial function above (`A←× / ⍳N`), I generated a list of numbers from 1 to N (`⍳N`), and then used reduce `/` to apply the multiply function to all the elements of the list and multiply them together. This lets us do very sophisticated things very easily.  In one of Robertson’s exercises, he asks us to output the sum of the squares of the first 10 positive numbers.  If I were using another language, I might write something like

```
int total = 0;
for (int i=1; i<11; i++) {
   total = (i*i) + total;
}
```

But in APL, I can now do

```
      +/(⍳10) × (⍳10)
```

Again, we can see how easy it is to go from a scalar `10`, to a list `⍳10`, operate on it like a list of separate items (`(⍳10) × ⍳10`), and then operate on that list like a stream of arguments (`+/`). This flexibility built into APL is what validates Robertson’s initial claim of APL being intuitive and powerful.

## Summary of Days 1-2

That is about all there is to Days 1 and 2.  Robertson interjects some nice APL history and philosophy into the lessons, but I’ll let you read that yourself. For myself, I feel like Robertson introduced me to enough APL that I could write basic functions and experiment with simple math operations and matrix manipulation quickly and easily.

## Days 3 and 4

Days 3 and 4 cover Dyalog and graphical user interfaces.  They follow the same format as Days 1 and 2, except with modules instead of lessons.

The book doesn’t start too smoothly, however.  Again, Robertson doesn't ground us – no introduction, no transition, no software setup. He just launches straight into a dense definition/example list for the next… well, two days.  It would take me as long as his book to go over the material, so here are just the highlights:

Robertson introduces us to Dyalog, an object oriented language with its roots in APL. The awesome power of Dyalog is its seamless integration with other languages and Windows programming. However, Robertson’s level of detail seems a little light: what it is, an example, and then onward. Robertson rushes through data types, more control flow, and GUI objects in Dyalog. If you’ve ever tried to do Windows programming in C++, then you’ll share my enjoyment of how easy manipulating forms is in Dyalog. However, if I hadn't already understood object oriented programming, I would have been confused by Robertson’s fast pace and lack of explanation.

The integration with other languages is also a neat feature. Robertson shows us the range of Dyalog with integrations with C, Microsoft .Net, TCP/IP and remote applications, and more.

## Conclusion

Overall, Robertson delivers on his promise: a practical introduction to APL. Robertson spends almost the entire first book translating mathematical notation into APL for us, and ends up being a little light on the explanation, examples, exercise solutions, and future direction. I wouldn’t recommend it for a 10-year-old first time programmer, as Robertson assumes knowledge of things like boolean arithmetic and object-oriented programming, and doesn’t provide any flashy long-running examples that teach you good principles of coding, and what to do with all the random symbols you’ve just learned. The second book is so densely packed that you can’t do much more than skim and remember where to return to when you want to use some of the functionality. However, by taking the APL tools out of the box one by one and describing each with an example, Robertson clearly demonstrates the flexibility, the power, and the intuitiveness of APL.

## References

1. *A Practical Introduction to APL 1&2*, Graeme Robertson, Basingstoke, 2008, 56pp
2. *A Practical Introduction to APL 3&4*, Graeme Robertson, Basingstoke, 2008, 196pp
3. APLX. MicroAPL. http://www.microapl.co.uk/apl/aplx\_downloads.html
