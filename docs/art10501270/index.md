---
vid: '10501270'
title: One reason that APL is so cool
authors:
- Brian Becker
byline: Brian Becker
volume: '26'
issue: '2'
page: '41'
received: '2013-09-28'
online: '2014-01-23'
description: Why APL is cool
source: content/printed/262/apliscool/apliscool.htm
converter: vecrescue 0.1.0
issue_tab: n23-september-2014
tags:
- APL in perspective
- programming techniques
---

Brian Becker
{ .byline }

The code shown in this article was not intended to be the most elegant or efficient means to solve the problem presented, but rather to demonstrate that APL's suitability for quick, ad hoc, data analysis and problem solving.
{ .ednote }

I had the good fortune to learn APL when I was but a freshman in high school. I found APL to be a great tool to solve problems. The phrase “APL as a Tool of Thought” has been around for quite some time and it still holds true. I’ve never viewed myself as a programmer, but as a problem solver. APL enables me to take a solution I conceive in my mind and translate it into a form executable by a computer with the least effort. Over the years, I’ve been lucky enough to work on some rather interesting problems, but in my spare time, I’ve also found APL to be fun for recreational computing. I’ll leave it to the gentle reader to assess just how much of a geek this makes me.

One such opportunity presented itself recently. Our local newspaper, the Rochester Democrat and Chronicle, along with other entities in the Rochester area, including the Rochester Institute of Technology (RIT), had been conducting a contest for several weeks called “Picture The Impossible”. It consisted of 7 weeks of challenges and puzzles all relating to aspects of the Rochester area and its history. Monday through Friday there featured puzzles on the web. There were weekly excursions or challenges that one could participate in around the local area and on Sunday there was a crossword puzzle and another challenge or puzzle. The puzzle of October 25, 2009 is the subject of this article.

This puzzle consisted of sets of scales and weights to be assigned to various points on the scales. For instance, given the scale:

```
┌───┴───┐
A   ┌───┴───┬───┐
    B       C   D
And weights of: 2 3 7 and 12
```

Each point will be assigned a weight and the force that point applies is its weight times the distance from the fulcrum. The example above gives the following:

```
A = B + C + D
B = C + (2 x D)
```

So, it’s pretty easy to work out that A=12, B=7, C=3, and D=4.

The challenge in the newspaper consisted of 6 such puzzles with up to 9 points. Because of some scheduling constraints, I had less than an hour to solve all the problems. While I’m a pretty good puzzle solver, I decided that the quickest way was to use APL. I figured that I could easily represent the algebra as a set of assertions and then run every combination of weights through those assertions until I found a set that worked. The first part was to build something that would generate all the combinations of weights. I remember that the number of combinations is the factorial of the number of elements, so nine elements would result in 362,880 possible combinations.

Now, had I paid more attention in school those many years ago, I’d probably have the “create all combinations” algorithm committed to memory. But, the way I thought of the problem is that combinations of set of nine weights is each of those weights concatenated with all the combinations of the other eight weights, then the combinations of a set of eight weights is each of those weights concatenated with all the combinations of the other seven weights, then the combinations of a set of seven weights… wait a minute… this is recursive! So, the terminal case is when you get down to a single item, and the only combination is the item itself. That’s easy enough to code.

```
        ∇ r←allcombinations v
[1]    →(1=⍴v)↓l1 ⋄ r←v ⋄ →0
[2]   l1:r←v cat¨allcombinations¨(⊂v)~¨v
        ∇
```

```
        ∇ r←a cat b
[1]    →(1=≡b)↓l1 ⋄ r←a,b ⋄ →0
[2]   l1:r←a cat¨b
        ∇
```

Why did I write “cat”? Well, I started out with using APL concatenation, the “,” function. But that resulted in a nested result that wasn’t quite what I was looking for, as shown below. DISPLAY is a wonderful utility that displays an array with its structure. The result below is using the APL concatenate primitive function instead of “cat”.

```
                DISPLAY allcombinations 1 3 5 7 ⍝ using ,
┌→────────────────────────────────────────────────────────────────────────────
│ ┌→────────────────────────────────────────────────────────┐ ┌→──────────────
│ │   ┌→──────────────┐ ┌→──────────────┐ ┌→──────────────┐ │ │   ┌→──────────
│ │ 1 │   ┌→──┐ ┌→──┐ │ │   ┌→──┐ ┌→──┐ │ │   ┌→──┐ ┌→──┐ │ │ │ 3 │   ┌→──┐ ┌→
│ │   │ 3 │5 7│ │7 5│ │ │ 5 │3 7│ │7 3│ │ │ 7 │3 5│ │5 3│ │ │ │   │ 1 │5 7│ │7
│ │   │   └~──┘ └~──┘ │ │   └~──┘ └~──┘ │ │   └~──┘ └~──┘ │ │ │   │   └~──┘ └~
│ │   └∊──────────────┘ └∊──────────────┘ └∊──────────────┘ │ │   └∊──────────
│ └∊────────────────────────────────────────────────────────┘ └∊──────────────
└∊────────────────────────────────────────────────────────────────────────────
```

Using cat, I got…

```
                DISPLAY  allcombinations 1 3 5 7 ⍝ using cat
┌→────────────────────────────────────────────────────────────────────────────
│ ┌→────────────────────────────────────────────────────────────────────────┐
│ │ ┌→────────────────────┐ ┌→────────────────────┐ ┌→────────────────────┐ │
│ │ │ ┌→──────┐ ┌→──────┐ │ │ ┌→──────┐ ┌→──────┐ │ │ ┌→──────┐ ┌→──────┐ │ │
│ │ │ │1 3 5 7│ │1 3 7 5│ │ │ │1 5 3 7│ │1 5 7 3│ │ │ │1 7 3 5│ │1 7 5 3│ │ │
│ │ │ └~──────┘ └~──────┘ │ │ └~──────┘ └~──────┘ │ │ └~──────┘ └~──────┘ │ │
│ │ └∊────────────────────┘ └∊────────────────────┘ └∊────────────────────┘ │
│ └∊────────────────────────────────────────────────────────────────────────┘
└∊────────────────────────────────────────────────────────────────────────────
```

Which is a lot closer to what I wanted…

However, I was still stuck with a nested array that had as many levels as the number of elements.

```
                ∇ r←n chunk a
[1]    r←∊a
[2]    r←((⍴r)⍴n↑1)⊂r
        ∇
        DISPLAY 4 chunk allcombinations 1 3 5 7
┌→────────────────────────────────────────────────────────────────────────────
│ ┌→──────┐ ┌→──────┐ ┌→──────┐ ┌→──────┐ ┌→──────┐ ┌→──────┐ ┌→──────┐ ┌→────
│ │1 3 5 7│ │1 3 7 5│ │1 5 3 7│ │1 5 7 3│ │1 7 3 5│ │1 7 5 3│ │3 1 5 7│ │3 1 7
│ └~──────┘ └~──────┘ └~──────┘ └~──────┘ └~──────┘ └~──────┘ └~──────┘ └~────
└∊────────────────────────────────────────────────────────────────────────────
```

Perfect! So, I’ve got the “all combinations” part of the problem solved. Now, let’s build the rest… The problem for this example is:

```
                        │
                ┌───────┴───────┬───────┐
                │                       │
┌───┬───┬───┬───┴───┐   ┌───┬───┬───┬───┴───┬───┬───┐
E           F       │   G           H               I
                    │
                    │
        ┌───┬───┬───┴───┬───┬───┬───┐
        A                   B   C   D
```

Weights are: 2 4 5 8 10 13 17 18 23

This gives the following relations:

```
3A = 2B+3C+4D
4E+F = A+B+C+D
3I = 4G+H
A+B+C+D+E+F = 2(G+H+I)
```

This turns into the following APL function. I won’t go into all the aspects of APL’s right to left execution, etc. If you know APL, it would be redundant, if you don’t know APL, there are plenty of resources to learn it. But basically, it tests each assertion. If an assertion fails, the function exits returning a result of 0. If all assertions pass, the set of weights is displayed and a 1 is returned.

```
        ∇ r←p5 arg;a;b;c;d;e;f;g;h;i
[1]    (a b c d e f g h i)←arg
[2]    →(r←(3×a)=2 3 4+.×b c d)↓0
[3]    →(r←(f+4×e)=a+b+c+d)↓0
[4]    →(r←(3×i)=h+4×g)↓0
[5]    →(r←(2×g+h+i)=a+b+c+d+e+f)↓0
[6]    ⎕←arg
        ∇
```

So, now we have something to generate all combinations, and something to solve for a single combination. It would be easy to use the APL each operator (`¨`) to run the solution against all combinations, but I wanted to have it stop as soon as it found a solution and not evaluate all the combinations. So, I wrote a simple “solve” program…

```
        ∇ solve z;cnt
[1]    cnt←0
[2]   lp:→((⍴z)<cnt←cnt+1)⍴0
[3]    →(p5 cnt⊃z)↓lp
        ∇
```

This will check the assertions against all the combinations until either all combinations have been checked, or a solution is found. The solution, if found, is displayed and the program exits.

Putting it all together:

```
        solve 9 chunk allcombinations 2 4 5 8 10 13 17 18 22
22 17 4 5 10 8 13 2 18
```

So, A=22, B=17, C=4, D=5, E=10, F=8, G=13, H=2, I=18. I’ll leave it to the reader to verify the result.

I was able to solve all 6 problems in the paper in less than 30 minutes, about 28 of which were spent thinking about the problem and coding the solution. Well, in truth, I didn’t solve them, APL solved them, but I got the results I needed in probably less time than it would have taken me to do manually. Besides, it was fun!

The goal of this paper is to demonstrate that APL is a terrific tool for solving problems quickly. There are no doubt better ways to code this in APL, but elegance wasn’t my goal. I had a problem to solve and limited time to do it within. This has always been one of APL’s strengths and whether for recreational or commercial purposes, APL remains a tool of thought.
