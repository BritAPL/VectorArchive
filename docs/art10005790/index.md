---
vid: '10005790'
title: 'At Play with J: Token Counting: APL versus J'
authors:
- Eugene McDonnell
byline: Eugene McDonnell
volume: '22'
issue: '3'
page: '49'
source: trad/v223/gene222.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- language design
- programming techniques
---

Eugene McDonnell
{ .byline }

<b> </b>One measure of the difference between APL\\360 and J is the number of tokens needed in a function to do the same work. I’ll discuss two examples, comparing well-written, but rather old, APL solutions, to well-written J solutions.

## Direct from Atomic

The first APL example is a marvellous function written by Luther Woodrum, that appears on page B.11 of the *APL\\360 User’s Manual* of 1968. Luther is best known to me by his design and implementation of the original *upgrade* and *downgrade*. His function <span class="APLU">PERM</span>, below, is given a left argument <span class="APLU">A</span>, the length of the permutation to be constructed, and a right argument <span class="APLU">B</span>, the anagram index of the permutation to be constructed. It uses an algorithm first discussed, I believe, by D. H. Lehmer of the University of California in Berkeley. Here it is:

<pre><span class="APLU">
       ∇ Z←A PERM B;I;Y
   [1]   I←⍴Z←1+(⌽⍳A)⊤B-1
   [2]   →0×⍳0=I←I-1
   [3]   Z[Y]←Z[Y]+Z[I]≤Z[Y←I+⍳A-1]
   [4]   →2
       ∇</span></pre>

Disregarding the header, footer and line numbers, this has 55 tokens. What does it do? Let’s suppose that <span class="APLU">A</span> is <span class="APLU">9</span> and <span class="APLU">N</span> is <span class="APLU">288918</span>. Then the result <span class="APLU">Z</span> is

<pre><span class="APLU">   7 1 3 2 6 4 0 5 8</span></pre>

and this is the 288918<sup>th</sup> permutation of order 9.

Here I should tell you that much of the material that follows is taken from my *At Play With J* article in *Vector 12.1* (July 1995). This is not surprising, since it deals with the subject of Luther’s function.

The first step is to make a function that gives the factorial digits of permutations of length A. Luther’s function uses origin 1, and that obfuscates things, so I’ll use the more suitable 0-origin indigenous to J. To give you some idea of what the factorial digits number system is like, here are the six factorial digits in the system for three:

```
     fdb =: >: @ i. @ -

     (fdb 3) #: i. ! 3
  0 0 0
  0 1 0
  1 0 0
  1 1 0
  2 0 0
  2 1 0
```

You can see the regularity in the rows. Notice also that every row ends in zero. This is true for all factorial digits systems.

In <span class="APLU">PERM</span>, line 1 gives <span class="APLU">Z</span> the factorial digits value for <span class="APLU">B</span>.

```
     fdb 9   NB. fdb n yields the radix digits of order 9
  9 8 7 6 5 4 3 2 1
     (fdb 9) #: 288918
  7 1 2 1 3 1 0 0 0
```

Lines 2 and 4 control the executions of line 3, so that line 3 is executed only as long as <span class="APLU">I</span> is positive. Line 3 can be defined as function <span class="APLU">g</span> :

```
g =:  [ , ] + ] <: [    NB. left , right + right >: left
```

Here I’ll have to pause, and to point out that what I’m doing with <span class="APLU">g</span> is taking the clutter out of <span class="APLU">PERM</span> line 3. What we’ve done so far reduces line 3 from 26 tokens to 7, yet it does precisely what line 3 does. Perhaps if I show the successive uses of <span class="APLU">g</span> you’ll get the idea:

```
                   0
                 0 1
               0 1 2
             1 0 2 3
           3 1 0 2 4
         1 4 2 0 3 5
       2 1 5 3 0 4 6
     1 3 2 6 4 0 5 7
   7 1 3 2 6 4 0 5 8
```

Successive lines are formed this way: for example, given line

```
1 4 2 0 3 5
```

the next line is formed by beginning with the corresponding factorial digit, in this case 2, and following with the previous line in which each item greater than or equal to 2 has 1 added to it.

```
2 1 5 3 0 4 6
```

Notice that each line is a permutation.

The function <span class="APLU">g</span>  is made of three forks:

```
      g
   +-+-+--------------+
   |[|,|+-+-+--------+|
   | | ||]|+|+-+--+-+||
   | | || | ||]|>:|[|||
   | | || | |+-+--+-+||
   | | |+-+-+--------+|
   +-+-+--------------+
```

The three forks are:

```
   fz =: ] >: [
   fy =: ] + fz
   fx =: [ , fy
```

Here’s a J function, a functional duplicate of <span class="APLU">PERM</span>:

```
      sr                          NB. standard form from reduced
   /:@/:@,/
      ra                          NB. reduced form from atomic
   ([: fdb [) #: ]
      sra =: sr@ra f.             NB. standard from atomic
      sra
   /:@/:@,/@(([: >:@i.@- [) #: ])
```

```
      ;: sra                      NB. tokens of sra
   +--+-+--+-+-+-+-+-+-+--+--+-+--+-+-+-+-+--+-+-+
   |/:|@|/:|@|,|/|@|(|(|[:|>:|@|i.|@|-|[|)|#:|]|)|
   +--+-+--+-+-+-+-+-+-+--+--+-+--+-+-+-+-+--+-+-+
```

```
      # ;: 5!:5 <'sra'            NB. count of tokens in sra
   20
```

The function <span class="APLU">sr</span> uses an identity I found March 9, 1970, when I was looking at Luther’s <span class="APLU">PERM</span> once more:

```
      N =: 7
      P =: 1 3 2 6 4 0 5 7
      (/:/:N,P) -: (N,P+N<:P)  NB. double upgrade matches addition
   1
      N,P+N<:P
   7 1 3 2 6 4 0 5 8
      /:/:N,P
   7 1 3 2 6 4 0 5 8
```

So double-upgrade can take the place of Luther’s line 3, and sra squeezes <span class="APLU">PERM</span> down from 55 to 20 tokens.

## Pyramigram

In *APL Quote Quad 11*.1, September, 1980, I asked for solutions to a problem posed by Linda Alvord, of Scotch Plains Fanwood High School in Scotch Plains, New Jersey. Here it is:

Write an APL function PG that takes a scalar integer argument from 1 to 26 and produces a rectangular character matrix containing a pattern like this:

```
      PG 5
       Q
      W Q
     Q E W
    R Q W E
   Q E R T W
```

In each row *r* there are *r* randomly selected and randomly ordered letters, separated by single spaces, arranged to form an equilateral triangle. The (<i>n</i>-1) letters in row <i>n</i>-1 are selected from the *n* letters in row *n*.

This was one of the most popular problems I’d ever given, and there were a wide variety of solutions, including ones by some fairly gifted programmers, but one stood out from the rest, from  Roger Hui. It took me several hundred words to describe what his function did. Roger recently told me that he had written his function without having access to an APL implementation. His function PG was not written in the conventional way that a function was defined in APL\\360. Instead, it uses the alpha-omega form introduced by Ken Iverson, in which the left and right arguments are denoted by <span class="APLU">⍺</span> and <span class="APLU">⍵</span>. Here is a J function which uses the same algorithm as his from 1980:

```
PG =: ([:i.[:-])(|."0 1)1j1"_ #"0 1(([:/:(([:-/\]-
~[:|.[:i.[:+:])#([:i.[:+:])
*[:*:])+[:?~[:*:]){[:,(],]){.(('ABCDEFGHIJKLMNOPQRSTUVWXYZ'"_{~]?26"_
) {.~[:-.[:+:])$~],[:+:])$~],]
```

I won’t dwell on this version other than to say that (1) it has 103 tokens and (2) it is in tacit form. The details are discussed in the cited issue of *APL Quote Quad*. It is a truth universally acknowledged that a good programming language, worked over and pondered over for a sufficiently long time by the same people who had produced the original, may very well show advantages over the original. Thus I sent an email to the J Forum list, and messages to key people in the APL community, for new solutions to the problem. I made it clear that the degree of improvement in expressiveness, as measured by token count, would be the criterion used. I received new solutions from the J and the APL communities. The shortest token count among the numerous APL solutions was 30, and there were several that used up to 60 tokens. The shortest J solution, by Roger Hui, was 20 tokens long, so I’ll discuss that one only. This is it:

```
   h   =:  /: # ? #
   pyr =:  i. &. - @ # |. " _1 [: 1j1 & # @ h \ h
```

His <span class="APLU">h</span> has 4 tokens, and <span class="APLU">pyr</span> has 16, totalling 20.

The subfunction <span class="APLU">h</span> is a hook.  It randomizes its argument. Its first function is <span class="APLU">/:</span> and its second function is the fork <span class="APLU"># ? #</span> .

```
   h 'qwert'
   ertqw
```

The function <span class="APLU">pyr</span>’s structure may best be seen using *box* display:

```
   +---------------+---------+--------------------------+
   |+---------+-+-+|+--+-+--+|+--+-------------------+-+|
   ||+--+--+-+|@|#||||.|"|_1|||[:|+---------------+-+|h||
   |||i.|&.|-|| | ||+--+-+--+||  ||+---------+-+-+|\|| ||
   ||+--+--+-+| | ||         ||  |||+---+-+-+|@|h|| || ||
   |+---------+-+-+|         ||  ||||1j1|&|#|| | || || ||
   |               |         ||  |||+---+-+-+| | || || ||
   |               |         ||  ||+---------+-+-+| || ||
   |               |         ||  |+---------------+-+| ||
   |               |         |+--+-------------------+-+|
   +---------------+---------+--------------------------+
          A             B                 C
```

The three outer boxes, A, B, and C, tell us that we have a fork. Box C yields the object to be rotated, and is the most interesting part. It produces a right-triangle version of the equilateral result required.

Its copy function <span class="APLU">#</span> is a dyad. By bonding ( <span class="APLU">&amp;</span> ) it on the left with <span class="APLU">1j1</span> we create a monad that makes one copy of its argument item and follows this with one fill item. A further function is made by using atop ( <span class="APLU">@</span> ) between the aforesaid monad and <span class="APLU">h</span>. One further step is to apply the prefix adverb ( <span class="APLU">\\</span> ) to this combined function to yield <span class="APLU">1j1
&amp; # @ h \\ </span>. One last step supplies cap ( <span class="APLU">\[: </span>) to the left of this and <span class="APLU">h</span> to the right, and we have a function that randomizes the argument ‘<span class="APLU">qwert</span>’, then randomizes each prefix, and provides a space after each item of the prefix, like this:

```
      ([: 1j1 & # @ h \ h) 'qwert'
   r
   r t
   t q r
   e r t q
   w q e r t
```

Rotation (box B) uses reversal, item rank (<span class="APLU">|. “ \_1</span>) so that scalar items from the left argument (box A) rotate list arguments from the right argument (box C).

Box A creates the left argument. It makes good use of negative arguments to <span class="APLU">i.</span> and of *dual*.  Ordinarily<span class="APLU"> i. – y</span> yields a descending list of positive integers, but we want to do right rotations, which require a negative value. That’s the reason for using *dual minus*. (<span class="APLU">&amp;. -</span> ).

```
      i. _5
   4 3 2 1 0
      (i. &. -) 5
   _4 _3 _2 _1 0
```

Thus, the whole result is given by 20 tokens:

```
      (i. &. - @ # |. " _1 [: 1j1 & # @ h \ h)'qwert'
       e
      r e
     r e t
    e q r t
   r w q t e
```

From 103 tokens in 1980 to 20 in 2005,  and by the same author, is a huge reduction.

\[12 Jan 2006\]
