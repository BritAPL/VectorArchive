---
vid: '10005640'
title: 'At Play With J: Four Cubes Redux'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '17'
issue: '3'
page: '113'
source: trad/v173/gene173.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2001
tags:
- competitions and puzzles
- programming techniques
---

*by Eugene McDonnell*
{ .byline }

*A Festschrift for Kenneth Iverson  
on his 80th birthday,  
2000 December 17*

I recently cleaned out the chest of drawers in my bedroom, in the course of which I got rid of many frayed and yellowed handkerchiefs, never-worn T-shirts, paper thin undergarments, and, much to my pleasant surprise, I excavated a set of four coloured cubes, an inch and a quarter to the side, a modern version of a puzzle dating back at least a century, under various names; the set I had is marketed under the name *Instant Insanity* in the United States by Parker Brothers, the purveyor of many other games, most notably Monopoly. The cubes’ faces are coloured with one of the four colours blue, green, red, or white, in some mixture. You are asked to stack the cubes one above the other in such a way that each side of the stack contains a face with each of the four colours, in some order.

In 1981 I had written a pamphlet on the puzzle called *The Four Cube Problem*,[<sup>1</sup>](#note1) subtitled “a case study in Basic, APL, and functional programming.” In the pamphlet a prize-winning Basic solution and an APL solution written by me were compared.

The Basic program had 91 non-comment lines, and the APL had nine; the average length of a Basic line was 31 characters; of the APL line 21 characters; Basic used 18 variables, APL none; Basic had 21 loops, APL none. The Basic program was written for and executed on the Sorcerer computer, which I suspect was a fairly early desktop computer, and so its execution time of 3 minutes and 5 seconds can’t be fairly compared with the APL program, which took 0.7 seconds to execute on one of the largest and fastest commercial computers available in 1981, the Amdahl V8 computer in the I. P. Sharp machine room in Toronto.

The APL solution emphasized the *functional programming* approach introduced by John Backus[<sup>2</sup>](#note2), employing the direct definition form by K. E. Iverson as a way of facilitating the definition of functions and their use in exposition. The solution used one constant function, one dyad, and seven monads.

The APL solution from my 1981 pamphlet could, I believe, be easily translated into Dyalog APL:

```
I:C S (B⍵[1;A])O S(B⍵[2;A])O S(B⍵[3;A])O G B⍵[4;X]
 X:3 4⍴ 3 5 4 6  1 3 2 4  1 6 2 5
 G:2 1 3 4⍉(1,⍴⍵)⍴⍵
 B:(R⍵)∘.='BGRW'
  R:(∨/<⍀⍵^.=⍉⍵)⌿⍵
 A:(⍳24)⌽4⌿X,[1]⌽X
 O:((1↑⍴⍵)⌿⍺),[2](((1↑⍴⍺),3⍴1)׽⍵)⍴⍵
 S:(^/^/^/⍵=<\[2]⍵)⌿⍵
 C:'BGRW'[⍵+.׼4]
```

This solution took as argument a 4 by 6 character matrix representing the cubes, with the initial letters of the face colours given in the order top, bottom, left, right, front, and back; the colours were blue, green, red, and white, abbreviated by ‘bgrw’. The result yielded was a three-dimensional array of shape n, 4, 4, where n is the number of solutions for the given set of cubes. A belt in a cube consists of a circuit of four faces; there are three basic belts in a cube: left, front, right, back; top, left, bottom, right; and top, back, bottom, front. The last cube is taken as the base of the stack, and only its three basic belts are considered, since rotating or reversing these belts will not provide any essentially different solutions. The face numbers of the basic belts are given by the constant `X`. For the other cubes, a total of 24 belts are considered, obtained by rotation and reversal of the basic belts in all possible ways. These are given by the function `A`.

The faces are represented by a boolean vector of length four containing a single 1 in the position corresponding to the colour:

```
Blue:   1 0 0 0
Green:  0 1 0 0
Red:    0 0 1 0
White:  0 0 0 1
```

A boolean representation was chosen in order to economize on space. This conversion is done by B, and the inverse by C. In the display below you can see the sizes of the intermediate arrays formed during execution. The largest needs space for \*/720 4 4 4 or 46,080 items. That is how many bytes are needed for a character representation. If bits are substituted, only one-eighth, or 5,760 bytes are needed. Integers would require four times, or 184,320 bytes: a prohibitive size for many systems of that day.

The development of the result, and the shapes of the intermediate arrays are shown below:

```
I:C S (B⍵[1;A])O S(B⍵[2;A])O S(B⍵[3;A])O G B⍵[4;X]
        ←2 4 4→     ←2 4 4→     ←2 4 4→     ←3 4-→   1
      ←16 4 4-→   ←24 4 4-→   ←24 4 4-→    ←3 4 4→   2
                                         ←3 1 4 4→   3
                              ←-----72 2 4 4-----→   4
                             ←-----28 2 4 4------→   5
                  ←------------672 3 4 4---------→   6
                 ←-----------45 3 4 4------------→   7
      ←-----------------720 4 4 4----------------→   8
    ←-----------------1 4 4 4--------------------→   9
  ←-----------------1 4 4------------------------→  10
```

At the right of line 1 the shape of the belts of the bottom cube is 3 4. In line 2 the boolean conversion has been made, and the shape is 3 4 4. In line 3 the function `G` has added a dimension of length 1; this is needed so that each belt from the next cube can be stacked on each existing stack.

Function `O` does this stacking; function `S` removes stacks containing duplicate face colours. This process continues with the remaining cubes, and finally `C` converts the boolean vector forms into colour letters.

This brief synopsis is a necessary prelude to the description of the J solution.

The function `I` in APL has this equivalent in J:

```
i =: (0&{) (s@o) (1&{) (s@o) (2&{) (s@o x) (3&{)
```

J’s *insert* (`/`) adverb is defined this way in the J dictionary on gerund left arguments:

> `m/y` *inserts successive verbs from the gerund* `m` *between items of* `y`<i>, extending <tt>m</tt> cyclically as required.  Thus,</i> ``+`*/i.6`` *is* `0+1*2+3*4+5`<i>. </i>

This suggests that `i` could be rewritten like this:

```
   g =: (s@o)`(s@o)`(s@o x)/
```

Whereas in both APL’s `I` function and J’s (irrelevant) `i` function it is necessary to specify index values to select rows of the argument, the gerund form of `g`, using J’s *insert* primitive (`/`) depends on the nature of insertion to go through the rows of the argument in the proper order, from the bottom up.

The remaining functions are detailed as follows:

```
   b =: 0 2 1 3  0 4 1 5  2 4 3 5
   x =: (3 1 4$ b)&{
   o =: [: ,/ ([: ~. [: a [) ,"1 2/ ]
   a =: (i.24)"_ |."_1 [: [ 4: # [: (] , |."1) (3 4$b)"_ { ]
   s =: ] #~ 2: > [: >./"1 [: ,"2 [: +/"3 ] =/ [: ~. ,
```

The constant `b` is used here in order to keep the length of displays within the confines of the page width. The `b`’s in functions `x` and `a` should be replaced by the value of `b`. Thus there are actually only five functions needed: `g`, `x`, `o`, `a`, and `s`, and, indeed, `g` could be replaced by its body, reducing it to only four functions. The function `R` of the APL version is replaced by the J primitive *nub* (`~.`). The functions `B` and `C` of the APL solution convert between the character and the boolean list representations used. These are not needed in the J solution because it is designed to be generic, permitting any form of representing the cubes. The nine APL functions use 151 tokens. The five J functions use 106 tokens.

The function x applies to the bottom row of the argument, and produces the three basic belts in the form of 1 by 4 tables. Because it is used with the insert primitive, there is no need to specify the index value 3; this comes about because of the nature of insertion. This is the seed needed to start the building of the candidate piles. The result of each step is a three-dimensional array of shape m,n,4, where m is the number of successful piles so far, initially 3; and n is the height of each pile, starting at 1, and ending at 4.

The effect of `s@o` is to insert the dyad o between two arrays c and d, where c is shape k by 24, where k is at most 24, and at least 1; and d is the result of the previous step, with shape m,n,4 as described above. Each of the k rows of a is placed on each of the n by 4 tables, and then these are joined to form a single table of k times m items. As noted above, the winnowing of the 24 belts selected by function `a` is accomplished by the J nub primitive (~.) within `o`, ensuring that there are no duplicate solutions.

The result of `o` is passed on to `s`. I rather like s; notice the nice way the rank operator gets used: first rank 3, then rank 2, then rank 1. It uses the phrase `] =/ [: ~. ,` to form the outer product equal of the argument with the nub of its ravel. This gives a four-dimensional array of k boolean tables, with a table for each of the rows in each solution. It sums the 3-dimensional arrays, which effectively counts the number of appearances in each column of each colour. The tables are raveled, and a mask is formed with a 1 for each row which contains no item greater than 1, and applied to the argument, which gets rid of all of the candidate solutions containing duplicate colours, leaving only those which remain as candidates for final solutions. Here is a sample of how `s` works, using `t`, containing two 3-high stacks:

```
   $t
2 3 4
   t
brwg
wwgr
rgbg
```

```
brwg
wwgr
rbbw
   $t0=: t = / ~. , t
   $t0
2 2 2 4
```

```
   $t1=:+/"3 t0
2 4 4
   t1
1 1 1 0
0 1 1 1
1 0 1 1
0 1 0 2
```

```
1 1 1 0
1 1 1 0
1 0 1 1
0 1 1 1
```

```
   ] t2=:,"2 t1
1 1 1 0 0 1 1 1 1 0 1 1 0 1 0 2
1 1 1 0 1 1 1 0 1 0 1 1 0 1 1 1
```

```
   $t3=:>./"1 t2
2 1
   ] t4=:2>t3
0 1
```

This solution can be improved a bit. Suppose I replace the function `x` with a similar function `y` which has the property that it is executed only if its argument has shape `,6`,. This can be done using J’s function power primitive. Then we could do without the gerund form altogether. Here is function `y`:

```
   y=:(3 1 4$b)&{^:((,6)"_ -: $)
```

Now we can solve the problem with:

```
   (s@o y)/
```

That’s all there is to it. J has helped to give a solution significantly shorter than the APL solution.

Now that I had a generic solution, I was able to test it on four different representations: a single character, a symbol, an integer, and a boxed name. All of these are scalars. Here are the four representations of the one-solution set of cubes:

```
   nu
3 1 0 0 2 2
3 2 2 1 3 0
3 3 1 1 2 0
3 0 1 1 3 2
   ch
wgbbrr
wrrgwb
wwggrb
wbggwr
   sy
`white `green `blue  `blue  `red   `red
`white `red   `red   `green `white `blue
`white `white `green `green `red   `blue
`white `blue  `green `green `white `red
```

<table border="1" cellpadding="4" cellspacing="0">
<tr align="center">
<td><tt>white</tt></td>
<td><tt>green</tt></td>
<td><tt>blue</tt></td>
<td><tt>blue</tt></td>
<td><tt>red</tt></td>
<td><tt>red</tt></td>
</tr>
<tr align="center">
<td><tt>white</tt></td>
<td><tt>red</tt></td>
<td><tt>red</tt></td>
<td><tt>green</tt></td>
<td><tt>white</tt></td>
<td><tt>blue</tt></td>
</tr>
<tr align="center">
<td><tt>white</tt></td>
<td><tt>white</tt></td>
<td><tt>green</tt></td>
<td><tt>green</tt></td>
<td><tt>white</tt></td>
<td><tt>blue</tt></td>
</tr>
<tr align="center">
<td><tt>white</tt></td>
<td><tt>blue</tt></td>
<td><tt>green</tt></td>
<td><tt>green</tt></td>
<td><tt>white</tt></td>
<td><tt>red</tt></td>
</tr>
</table>

And the solutions for each:

```
   (s@o y)/ nu
1 0 3 0
2 2 1 3
0 1 2 1
3 3 0 2
```

```
   (s@o y)/ ch
gbwb
rrgw
bgrg
wwbr
   (s@o y)/ sy
`green `blue  `white `blue
`red   `red   `green `white
`blue  `green `red   `green
`white `white `blue  `red
   (s@o y)/ bn
```

<table border="1" cellpadding="4" cellspacing="0">
<tr align="center">
<td><tt>green</tt></td>
<td><tt>blue</tt></td>
<td><tt>white</tt></td>
<td><tt>blue</tt></td></tr>

<tr align="center">
<td><tt>red</tt></td>
<td><tt>red</tt></td>
<td><tt>green</tt></td>
<td><tt>white</tt></td></tr>

<tr align="center">
<td><tt>blue</tt></td>
<td><tt>green</tt></td>
<td><tt>red</tt></td>
<td><tt>green</tt></td></tr>

<tr align="center">
<td><tt>white</tt></td>
<td><tt>white</tt></td>
<td><tt>blue</tt></td>
<td><tt>red</tt></td></tr>
</table>

A set of cubes which gives 22 solutions is:

```
`white `blue  `red   `blue  `white `green
`red   `blue  `red   `blue  `green `white
`red   `blue  `red   `green `green `white
`green `white `white `red   `blue  `red
```

I measured the time needed to produce solutions for each of these representations using two different cube sets, one which had only one solution, and one which had 22 solutions.

Here is a tabulation of the times taken using each of the two test cubes:

<table border="1" cellpadding="4" cellspacing="0">
<tr>
<td><br></td>
<th>one solution</th>
<th>23 solutions</th></tr>
<tr>
<td>integer</td>
<td align="right">0.012</td>
<td align="right">0.047</td></tr>
<tr>
<td>single character</td>
<td align="right">0.010</td>
<td align="right">0.036</td></tr>
<tr>

<td>symbol</td>
<td align="right">0.012</td>
<td align="right">0.047</td></tr>
<tr>

<td>boxed name</td>
<td align="right">0.058</td>
<td align="right">0.254</td></tr>
</table>

The table shows that the symbol datatype is competitive in time with the single character and integer data types, which is what we hoped would happen. Symbols are a much more efficient datatype to work with than are boxed names. It also shows, which should not be a surprise to anyone, that I have on my desk a computer that is seventy times faster and has much larger memory and disk storage than did the large roomful of computer and disk packs that had seemed so very powerful twenty years ago.

<a id="note1"></a><sup>1</sup> McDonnell, E. E., *The Four Cube Problem*, APL Press, Palo Alto 1981

<a id="note2"></a><sup>2</sup> Backus, John, “Can Programming Be Liberated from the von Neumann Style? A Functional Style and its Algebra of Programs”, *Communications of the ACM 21*, 8, August 1978

***
