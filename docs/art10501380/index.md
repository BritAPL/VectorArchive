---
vid: '10501380'
title: A Tool of Thought
authors:
- Dan Baronet
byline: Dan Baronet (danb@dyalog.com)
volume: '26'
issue: '2'
page: '50'
received: '2014-02-13'
online: '2014-08-07'
description: Using APL to assist thinking when problem solving
abstract: I am often asked “what is APL good for”? I reply that APL is good for almost anything but that it is also very good at prototyping. With it you can experiment and use it as a tool for thinking about the problem at hand. It is easy in APL to manipulate data and build tools to get a better view of the problem and come up with solutions. In the following text we will use APL to think of a solution to a problem involving calculations to solve a mathematical problem. The problem originates from Kakuro[1], a popular puzzle found in newspapers, where you need to know the sets of numbers making up a solution.
source: content/printed/262/toolofthought/toolofthought.htm
converter: vecrescue 0.1.0
issue_tab: n23-september-2014
tags:
- programming techniques
- mathematics and statistics
---

Dan Baronet (danb@dyalog.com)
{ .byline }

I am often asked “what is APL good for”? I reply that APL is good for almost anything but that it is also very good at prototyping. With it you can experiment and use it as a tool for thinking about the problem at hand. It is easy in APL to manipulate data and build tools to get a better view of the problem and come up with solutions. In the following text we will use APL to think of a solution to a problem involving calculations to solve a mathematical problem. The problem originates from Kakuro[\[1\]](#ref1), a popular puzzle found in newspapers, where you need to know the sets of numbers making up a solution.
{ .abstract }

## The problem

In this problem we need to come up with all the sets of N unique positive single digit numbers (1..9) making up a particular sum S. N and S are the key numbers here, they will be the input to our problem. The output is all the possible sets. The format is unimportant; it could be e.g. a list of sets or a square matrix, N wide.

Most of the code should work in any modern APL. However, the examples were created with Dyalog Version 14. When features are used which are available in Dyalog APL only this is mentioned. `⎕IO←1` is assumed.

For example, there are only 2 sets of 4 unique digits 1 to 9 adding up to 12: (1 2 3 6) and (1 2 4 5).

### Attempt #1

The first thought is the easiest: brute force. Can we generate all the possibilities and screen out unwanted ones?

We need to form sequences of N numbers, each from 1 to 9. For example, pairs are (1,1) (1,2) (1,3)…(2,1) (2,2)… (9,9), 81 combinations in all. We can use *catenate* (`,`) to put numbers together:

```
      i3 ← ⍳3  ⍝ define a vector of the numbers 1, 2 and 3
      i3 , i3
1 2 3 1 2 3
```

Not quite what we want, we want to catenate each number to each other:

```
      i3 ,¨ i3
 1 1  2 2  3 3
```

In Dyalog APL V14 there is a new user command that allows us to box enclosed arrays automatically to better see their nature:

```
      ]box on
Was OFF
      i3 ,¨ i3
┌───┬───┬───┐
│1 1│2 2│3 3│
└───┴───┴───┘
```

Again this is not what we want, what we want is to do the catenation for each element in `i3` to each other element in `i3`, like this:

```
      1 ,¨ i3
┌───┬───┬───┐
│1 1│1 2│1 3│
└───┴───┴───┘
      2 ,¨ i3
┌───┬───┬───┐
│2 1│2 2│2 3│
└───┴───┴───┘
      3 ,¨ i3
┌───┬───┬───┐
│3 1│3 2│3 3│
└───┴───┴───┘
```

APL allows us to do this nicely, distributing the function `,` without looping, using *jot-dot* (`∘.`) :

```
     i3 ∘., i3
┌───┬───┬───┐
│1 1│1 2│1 3│
├───┼───┼───┤
│2 1│2 2│2 3│
├───┼───┼───┤
│3 1│3 2│3 3│
└───┴───┴───┘
```

That’s better. This will work is all modern APLs. In Dyalog we can use *commute* (`⍨`) to avoid repeating the argument. Commute normally swaps (commutes) the arguments of a function so `a∊⍨b` becomes `b∊a`, but when used monadically it repeats the argument so `+⍨a` becomes `a+a`:

```
      ∘., ⍨ i3
┌───┬───┬───┐
│1 1│1 2│1 3│
├───┼───┼───┤
│2 1│2 2│2 3│
├───┼───┼───┤
│3 1│3 2│3 3│
└───┴───┴───┘
```

Let's do it for the numbers 1 to 9:

```
      ∘., ⍨ ⍳9
┌───┬───┬───┬───┬───┬───┬───┬───┬───┐
│1 1│1 2│1 3│1 4│1 5│1 6│1 7│1 8│1 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│2 1│2 2│2 3│2 4│2 5│2 6│2 7│2 8│2 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│3 1│3 2│3 3│3 4│3 5│3 6│3 7│3 8│3 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│4 1│4 2│4 3│4 4│4 5│4 6│4 7│4 8│4 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│5 1│5 2│5 3│5 4│5 5│5 6│5 7│5 8│5 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│6 1│6 2│6 3│6 4│6 5│6 6│6 7│6 8│6 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│7 1│7 2│7 3│7 4│7 5│7 6│7 7│7 8│7 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│8 1│8 2│8 3│8 4│8 5│8 6│8 7│8 8│8 9│
├───┼───┼───┼───┼───┼───┼───┼───┼───┤
│9 1│9 2│9 3│9 4│9 5│9 6│9 7│9 8│9 9│
└───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

Let's keep this list in a variable and find the sum of each and let's find those that add up to, say, 6:

```
      v← , ∘.,⍨ ⍳9  ⍝ turn the table into a list with ravel (,)
      sum← +/¨ v    ⍝ sum each set
      six← 6=sum    ⍝ find the 6s
      six/v         ⍝ extract them
┌───┬───┬───┬───┬───┐
│1 5│2 4│3 3│4 2│5 1│
└───┴───┴───┴───┴───┘
```

Let's write a function to find pairs adding up to a specific number. Here we’ll use Dyalog’s dynamic functions:

```
      pairs←{ok←⍵=+/¨all←, ∘., ⍨ ⍳9 ⋄ ok/all}
      pairs 6
┌───┬───┬───┬───┬───┐
│1 5│2 4│3 3│4 2│5 1│
└───┴───┴───┴───┴───┘
```

There are 2 problems with this code:

\#1, the rule says digits must be unique, so let’s add code to only keep the numbers that are different:

```
      pairs←{ok←⍵=+/¨all←, ∘.,⍨ ⍳9 ⋄ ok←ok ∧ ≠/¨all ⋄ ok/all}
      pairs 6
┌───┬───┬───┬───┐
│1 5│2 4│4 2│5 1│
└───┴───┴───┴───┘
```

That's better, but we still need to solve problem #2: some are duplicates, e.g. (1 5) and (5 1) are the same. We should remove them. Let's create a sorting function to reorder the sets:

```
     Sort←{⍵[⍋⍵]}
```

and use it in our function to reorder each set and use *unique* (`∪`) to extract the unique ones:

```
      pairs←{ok←⍵=+/¨all←,∘.,⍨⍳9 ⋄ ok←ok∧≠/¨all ⋄ ∪ Sort¨ ok/all}

      pairs 6     ⍝ pairs that add up to 6
┌───┬───┐
│1 5│2 4│
└───┴───┘
      pairs 9     ⍝ pairs that add up to 9
┌───┬───┬───┬───┐
│1 8│2 7│3 6│4 5│
└───┴───┴───┴───┘
```

Looks good. What about triples? We can use `∘.,` twice:

```
      i3 ∘., i3 ∘., i3    ⍝ generate a 3 x 3 x 3 of 1, 2 and 3s
┌─────┬─────┬─────┐
│1 1 1│1 1 2│1 1 3│
├─────┼─────┼─────┤
│1 2 1│1 2 2│1 2 3│
├─────┼─────┼─────┤
│1 3 1│1 3 2│1 3 3│
└─────┴─────┴─────┘
┌─────┬─────┬─────┐
│2 1 1│2 1 2│2 1 3│
├─────┼─────┼─────┤
│2 2 1│2 2 2│2 2 3│
├─────┼─────┼─────┤
│2 3 1│2 3 2│2 3 3│
└─────┴─────┴─────┘
┌─────┬─────┬─────┐
│3 1 1│3 1 2│3 1 3│
├─────┼─────┼─────┤
│3 2 1│3 2 2│3 2 3│
├─────┼─────┼─────┤
│3 3 1│3 3 2│3 3 3│
└─────┴─────┴─────┘
     triples←{ (⍵=+/¨all)/ all←, i ∘., i ∘., i←⍳9 }
     triples 6
┌─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┐
│1 1 4│1 2 3│1 3 2│1 4 1│2 1 3│2 2 2│2 3 1│3 1 2│3 2 1│4 1 1│
└─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘
```

Some doubles are still there - we can't use `≠` this time. `≠/` on more than 2 numbers is meaningless:

```
      ≠/4 1 1       ⍝ same as 4≠ (1≠1) or  4≠FALSE!!!
1
```

We’ll use the nub (unique) of each set to see if it is valid with function `{⍵≡∪⍵}`: if the digits are unique we’ll keep the set. We’ll then sort each set and keep the unique ones:

```
      clean←{ ∪Sort¨ ({⍵≡∪⍵}¨⍵)/⍵ }
      triples←{all←(⍵=+/¨all)/all←,i ∘., i ∘., i←⍳9 ⋄ clean all}
      triples 6
┌─────┐
│1 2 3│
└─────┘
```

That's better; we now need to write a function that will do it for any number of digits. The left argument will be the number of digits required. That means looping over `∘.,` until we have the proper number of iterations. In Dyalog the *power* operator (`⍣`) will help with this, it will do the looping for us:

```
      (i3  ∘.,  i3  ∘.,  i3) ≡ (i3 (∘., ⍣ 2) i3)
1
```

So we can write (for N digits we need to run `∘.,` N-1 times)

```
    NCat←{⍵ (∘., ⍣(⍺-1)) ⍵}
```

Or, since the argument is the same on both sides, we can use `⍨` :

```
          NCat←{ ∘., ⍣(⍺-1)⍨ ⍵}
    ntuple←{ok←⍵=+/¨all←, ⍺ NCat ⍳9 ⋄ all←ok/all ⋄ clean all }
```

Let’s find how many ways we can make twelve with four different numbers:

```
        4 ntuple 12
┌───────┬───────┐
│1 2 3 6│1 2 4 5│
└───────┴───────┘
      ⍴4 ntuple 12
2
```

Just two. There should also be only one way for 9 digits to add up to 45 (all the numbers 1 to 9):

```
      ⍴9 ntuple 45
WS FULL
NCat[0] NCat←{∘.,⍣(⍺-1)⍨⍵}
             ∧
```

Oops! Looks like we have a problem Houston. We're trying to generate

```
      9 × 9*9
3486784401
```

more than 3 billion numbers! This is too big on my machine, even with `⎕wa=64039748`.

```
      6 ntuple 35
┌───────────┬───────────┬───────────┬───────────┐
│1 4 6 7 8 9│2 3 6 7 8 9│2 4 5 7 8 9│3 4 5 6 8 9│
└───────────┴───────────┴───────────┴───────────┘
      7 ntuple 39
WS FULL
NCat[0] NCat←{∘.,⍣(⍺-1)⍨⍵}
             ∧
```

Looking at

```
      i3 ∘., i3 ∘., i3
┌─────┬─────┬─────┐
│1 1 1│1 1 2│1 1 3│
├─────┼─────┼─────┤
│1 2 1│1 2 2│1 2 3│
├─────┼─────┼─────┤
│1 3 1│1 3 2│1 3 3│
└─────┴─────┴─────┘
…
```

we can see that the numbers in the boxes are the indices of each box. APL has a primitive to produce the indices of any structure: *iota* (`⍳`) :

```
      (i3 ∘., i3 ∘., i3 ) ≡ ⍳ 3 3 3
1
```

This primitive should take less space to generate and is a lot faster than looping over :

```
      ntupleB←{ok←⍵=+/¨all←,⍳ ⍺⍴9 ⋄ all←ok/all ⋄ clean all}
```

Let’s see how much faster it is. There is a user command in Dyalog that allows us compare timings:

```
      ]runtime  "6 ntuple 35"  "6 ntupleB 35"  -compare

  6 ntuple 35  → 2.4E¯1 |   0% ⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕
  6 ntupleB 35 → 1.4E¯1 | -43% ⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕
```

But it still suffers from space problems:

```
      7 ntupleB 39
WS FULL
ntuple2[0] ntupleB←{ok←⍵=+/¨all←,⍳⍺⍴9 ⋄ all←ok/all ⋄ clean all}
                               ∧
```

But wait, maybe we can do it another way. How about using encode? Here are the 81 pairs again:

```
      1+ 9 9 ⊤ ¯1+ ⍳9*2
1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 4 4 4 4 4 4 4 4 4 5 5
1 2 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1 2

      3 4 5 6 7 8 9 1 2 6 6 6 6 6 6 6 7 7 7 7 7 7 7 7 7 8 8 8 8 8 8 8 8 8 9
      5 5 5 5 5 5 5 6 6 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1

      9 9 9 9 9 9 9 9
      2 3 4 5 6 7 8 9
```

Again, that would also require too many numbers when N is greater than 6. This brute force method fails for large N, let's see if we can modify it.

## Attempt #2

Maybe we can refine the process. Let’s have a look at pairs again:

```
      pairs
{ok←⍵=+/¨all←,∘.,⍨⍳9 ⋄ ok←ok∧ ≠/¨all ⋄ ∪Sort¨ok/all}
```

We don’t really need `Sort` nor the *unique* (`∪`) after. We can eliminate a lot of cases by reordering the checks and taking into account that the numbers in the sets must be in increasing order:

```
      pairs2← {all←(</¨all)/all←, i9 ∘., i9←⍳9 ⋄ (⍵=+/¨all)/all}
      pairs2   9
┌───┬───┬───┬───┐
│1 8│2 7│3 6│4 5│
└───┴───┴───┴───┘
```

Triples are similar. We cannot use `</` on 3 numbers but since the last ones are already ordered we can use `</` on the first 2 . We could write

```
       triples2←{
              all←(</¨2↑¨all) /all←, i9 ∘., (</¨all)/all←,i9 ∘., i9←⍳9
              (⍵=+/¨all)/all
           }
      triples2  19
┌─────┬─────┬─────┬─────┬─────┐
│2 8 9│3 7 9│4 6 9│4 7 8│5 6 8│
└─────┴─────┴─────┴─────┴─────┘
```

Seems to work. Quadruples would work similarly. We should use a function, like Ncat, to generate the combinations, something like

```
      Gen←{ ((</¨2↑¨all)/ all←,(⍳9) ∘., ⍵}
```

We can use the user command `]ROWS` (newly introduced in Version 14.0 of Dyalog) to cut the output to the width of the window (paper here):

```
      ]rows -style=cut
Was -style=long
      Gen ⍳9
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬──•
│1 2│1 3│1 4│1 5│1 6│1 7│1 8│1 9│2 3│2 4│2 5│2 6│2 7│2 8│2 9│3 4│3 •
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴──•
      Gen Gen ⍳9
┌─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬•
│1 2 3│1 2 4│1 2 5│1 2 6│1 2 7│1 2 8│1 2 9│1 3 4│1 3 5│1 3 6│1 3 7│•
└─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴•
      Gen Gen Gen ⍳9
┌───────┬───────┬───────┬───────┬───────┬───────┬───────┬───────┬──•
│1 2 3 4│1 2 3 5│1 2 3 6│1 2 3 7│1 2 3 8│1 2 3 9│1 2 4 5│1 2 4 6│1 •
└───────┴───────┴───────┴───────┴───────┴───────┴───────┴───────┴──•
```

Let’s try it:

```
      ntuple2←{ ok←⍵=+/¨all← Gen⍣(⍺-1) ⍳9 ⋄ ok/all}
      3  ntuple2  19
┌─────┬─────┬─────┬─────┬─────┐
│2 8 9│3 7 9│4 6 9│4 7 8│5 6 8│
└─────┴─────┴─────┴─────┴─────┘
```

The ultimate test: can it find the only solution to a 9 digits sequence adding up to 45 `+/⍳9`?

```
      9  ntuple2  45
┌─────────────────┐
│1 2 3 4 5 6 7 8 9│
└─────────────────┘
```

It works! We now have a solution. Mission accomplished. Let’s see how much space is needed to run it; the user command `]spaceneeded` will provide that information:

```
      ]space  "9 ntuple2  45"
85824
```

Not bad. How much CPU does it take?

```
      ]runtime "9 ntuple2 45" -repeat=1s

* Benchmarking "9 ntuple2 45", repeat=1s
                     Exp
 CPU (avg):  2.835227273
 Elapsed:    2.866477273
```

3 ms. That’s good enough.

Out of curiosity, could we have done better?

### Attempt #3

Looking carefully at pairs we see that the first number can be 1 to 9 and that the second number can be whatever remains (as long as it is in the range 1 to 9) but not the same, i.e. for pairs adding up to a specific Sum e.g. 10, we have 1 and (10-1), 2 and (10-2), 3 and (10-3), etc. In APL `⌈`:

```
      (⍳9) ,¨ 10-⍳9
┌───┬───┬───┬───┬───┬───┬───┬───┬───┐
│1 9│2 8│3 7│4 6│5 5│6 4│7 3│8 2│9 1│
└───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

The sets should be ordered in increasing order so we can limit the numbers 1 to 4 as first number because the largest combination we will have will be n followed by n+1, i.e. 10=n+n+1 or n=4.5, or 4 since we only deal with integers.

```
      pairs3← {i,¨ ⍵-i← ⍳⌊ (⍵-1)÷2 }
      pairs3  9
┌───┬───┬───┬───┐
│1 8│2 7│3 6│4 5│
└───┴───┴───┴───┘
      pairs3  10
┌───┬───┬───┬───┐
│1 9│2 8│3 7│4 6│
└───┴───┴───┴───┘
```

Fine. How about triples? Using the same idea we find that the largest set will be n,(n+1),(n+2) so we can use the numbers from 1 to `⌊(Sum-3)÷3` followed by all the pairs of `Sum` minus that number. For example

```
      Sum←10
      (Sum-3) ÷ 3   ⍝ we start with the numbers 1 and 2
2.333333333
      1,¨ pairs3  Sum-1
┌─────┬─────┬─────┬─────┐
│1 1 8│1 2 7│1 3 6│1 4 5│
└─────┴─────┴─────┴─────┘
      2,¨ pairs3  Sum-2
┌─────┬─────┬─────┐
│2 1 7│2 2 6│2 3 5│
└─────┴─────┴─────┘
```

Not quite. We should ignore any pair starting with a number smaller or equal to our first number.

Let’s modify `pairs3` to accept a (optional) left argument specifying the starting numbers to skip:

```
      pairs3← {⍺←0 ⋄ i,¨ ⍵-i← ⍺↓ ⍳⌊ (⍵-1)÷2 }
      pairs3  9
┌───┬───┬───┬───┐
│1 8│2 7│3 6│4 5│
└───┴───┴───┴───┘
      1 pairs3  9
┌───┬───┬───┐
│2 7│3 6│4 5│
└───┴───┴───┘
```

```
      1,¨ 1 pairs3  Sum-2
┌─────┬─────┐
│1 2 6│1 3 5│
└─────┴─────┘
```

```
      2,¨ 2 pairs3  Sum-2
┌─────┐
│2 3 5│
└─────┘
```

Triples?

```
      triples3←{i← ⍳⌊ (⍵-3)÷3 ⋄ i,¨ i pairs3¨ ⍵-i }
      triples3  9
┌───────────┬───────┐
│┌─┬───┬───┐│┌─┬───┐│
││1│2 6│3 5│││2│3 4││
│└─┴───┴───┘│└─┴───┘│
└───────────┴───────┘
```

Not quite, we need to use *each* (`¨`) twice:

```
      triples3←{i← ⍳⌊ (⍵-3)÷3 ⋄ i,¨¨ i pairs3¨ ⍵-i }
      triples3  9
┌─────────────┬───────┐
│┌─────┬─────┐│┌─────┐│
││1 2 6│1 3 5│││2 3 4││
│└─────┴─────┘│└─────┘│
└─────────────┴───────┘
```

That’s better but this enclosing business is getting out of hand. Let’s work with matrices:

```
      pairs3B←{⍺←0 ⋄ i,⍪⍵-i←⍺↓⍳⌊(⍵-1)÷2}
      pairs3B  9
1 8
2 7
3 6
4 5
      2 pairs3B  9
3 6
4 5
      triples3B← {i← ⍳⌊ (⍵-3)÷3 ⋄ ↑⍪/i,¨ i pairs3B¨ ⍵-i }
      triples3B  9
1 2 6
1 3 5
2 3 4
```

Seems to work. But there is a pattern here. It looks like a recursive definition.

- If we want a single digit set then the set is the sum if it is below 10.
- If we want a N digit set then it is all the digits from 1 to `⌊(Sum-+/⍳N-1)÷N` followed by the N-1 digit set of `Sum` minus that number.

We’ll need to adjust the starting number and remove any number smaller or equal to the first digit. We need to supply that number as argument, something like:

```
     ntuple3← {
  ⍝ Generate all monotonic combinations of ⍺ numbers adding to ⍵
     (nn sn)←2↑⍺      ⍝ # of numbers needed, numbers to skip
     nn=1 : (⍵≤9)⌿ ⍪⍵ ⍝ solution for 1 number is ⍵ if ≤9
  ⍝ More than 1 #, drop any number ≤ sn
     n1←sn↓⍳8⌊0⌈⌊(⍵- +/ ⍳ nn-1)÷nn ⍝ all possible starting #
     0∊⍴n1 : 0 nn⍴0  ⍝ no solution?
  ⍝ All are starting # followed by the new combination
     ↑⍪/ n1 ,¨ ((nn-1),¨n1) ∇¨ ⍵-n1
 }
```

This solution returns a matrix instead of a list of vectors. Let’s try it:

```
      4 ntuple3  12
1 2 3 6
1 2 4 5
      9 ntuple3  45
1 2 3 4 5 6 7 8 9
```

How does it compare with the previous solution?

```
      ]runtime "9 ntuple2  45 " "9 ntuple3  45" -compare

  9 ntuple2 45  → 2.9E¯3 |   0% ⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕⎕
* 9 ntuple3 45  → 5.3E¯5 | -99% ⎕
      ]space  "9 ntuple3  45"
3620
```

Quite a difference. With just a little more effort we improved our original solution a lot. And there are even faster solutions.

### Conclusion

APL provides us with an environment where we can experiment “hands on” to study situations, verify results and make better decisions. With it we can even come up with prototypes that we can use to make further forays into our problem. There is a lot of thinking that could have been done mentally but being able to use the computer really is a bonus.

If you want, there is a video accompanying this article you can have a look at. Go to [YouTube](http://www.youtube.com/) and look for “Brute force method to finding all the sets in a row of Kakuro”. You can also try this link: [http://youtu.be/bJssWsdXjmY](http://youtu.be/bJssWsdXjmY/)

Have fun!

## References

1. <a id="ref1"></a> [Kakuro](http://en.wikipedia.org/wiki/Kakuro)
