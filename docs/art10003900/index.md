---
vid: '10003900'
title: Queens and Knights
authors:
- Roger Hui
byline: by Roger K.W. Hui
volume: '21'
issue: '3'
page: '78'
source: trad/v213/hui213.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- competitions and puzzles
- programming techniques
---

by Roger K.W. Hui
{ .byline }

## The Problem

The webpage [http://www.itasoftware.com/careers/programmers.php](http://www.itasoftware.com/careers/programmers.php) often contains interesting programming puzzles. The “Queens and Knights“ puzzle was posted in 2003-2004:

> In 1850, Carl Friedrich Gauss and Franz Nauck showed that it is possible to place eight queens on a chessboard such that no queen attacks any other queen. The problem of enumerating the 92 different ways there are to place 8 queens in this manner has become a standard programming example, and people have shown that it can be solved using many different search techniques.
>
> Now consider a variant of this problem: you must place an equal number of knights and queens on a chessboard such that no piece attacks any other piece. What is the maximum number of pieces you can so place on the board, and how many different ways can you do it?

The `n` queens problem can be solved succinctly and efficiently. A solution is presented in the `for.` page of the J dictionary.

The queens and knights problem is considerably more difficult.

## Configs

A *config* is an `n*n (n=:8)` element string recording the placement of queens (`Q`) and knights (`N`) together with the coverage, the squares under attack by pieces already on the board and the squares from where additional pieces would attack an existing piece.

`x` below is a config. `Q` indicates a square where a queen is placed, on row 2 and column 6. `x` indicates a square under attack. `n` indicates a square where a knight, if there placed, would attack `Q`. A “`.`” indicates a free square. A queen can be placed on a “`.`” or `q` square. A knight can be placed on a “`.`” square.

<pre><tt>   n=: 8

   x=: '....xqxq....qxxxxxxxxxQx....qxxx....xqxq...x..x.
                ..x...x..x....x.'

   (n,n) $ x
....xqxq
....qxxx
xxxxxxQx
....qxxx
....xqxq
...x..x.
..x...x.
.x....x.</tt></pre>

`y` below is another config. `q` indicates a square where a knight is placed, on row 6 and column 1. `x` indicates a square under attack. `n` indicates a square where a queen, if there placed, would attack `N`. A “`.`” indicates a free square. A knight can be placed on a “`.`” or `n` square. A queen can be placed on a “`.`” square.

<pre><tt>   y=: '.n.....n.n....n..n...n...n..n...xnxn....nnnx....
                nNnnnnnnnnnx....'

   (n,n) $ y
.n.....n
.n....n.
.n...n..
.n..n...
xnxn....
nnnx....
nNnnnnnn
nnnx....</tt></pre>

Our plan for solving the queens and knights problem is as follows: starting with the empty config (`(n*n)$'.'`), place queens and knights one piece at a time on all possible squares. As a piece is placed the config is updated. Placing a piece and updating the config means merging the config with a config that represents putting a piece on a particular square, to derive a new config.

Consider again the configs `x` (a `Q` on the (2,6) square) and `y` (an `N` on the (6,1) square). How do we merge them? That is, what is the config for a board with a `Q` on (2,6) and an `N` on (6,1)?

<pre><tt>   (&lt;n,n) $&amp;.&gt; x;y;'?'
+--------+--------+--------+
|....xqxq|.n.....n|????????|
|....qxxx|.n....n.|????????|
|xxxxxxQx|.n...n..|????????|
|....qxxx|.n..n...|????????|
|....xqxq|xnxn....|????????|
|...x..x.|nnnx....|????????|
|..x...x.|nNnnnnnn|????????|
|.x....x.|nnnx....|????????|
+--------+--------+--------+</tt></pre>

## A Calculus of Configs

We have seen that a config can contain the letters `.NQxnq` . To merge two configs, we need to determine what letter should be the result for every combination of two letters.

<pre><tt>.  free
N  N sits here
Q  Q sits here
x  under attack
n  Q here would attack N
q  N here would attack Q</tt></pre>

It is clear that “`.`” (a free square) with any letter should result in that letter. `N` (a knight on this square) with `n` (queen here would attack a knight) should be `N`, but `N` with anything other than “`.`” or `n` should be `?`, an error.

The combinations `(n,q)` or `(q,n)` merit some comment. A queen can be placed on a `q` square but not on an `n` square, and a knight can be placed on an `n` square but not on a `q` square. Therefore, a square that is both `n` and `q` is equivalent to an `x` square, “under attack”, because no piece (neither queen nor knight) can be placed there.

The results for other combinations are derived similarly.

<pre><tt>
   free                      .  N  Q  x  n  q
N  N sits here               N  ?  ?  ?  N  ?
Q  Q sits here               Q  ?  ?  ?  ?  Q
x  forbidden                 x  ?  ?  x  x  x
n  Q here would attack N     n  N  ?  x  n  x
q  N here would attack Q     q  ?  Q  x  x  q
?  error</tt></pre>

The list `MT` records the ravelled operation table. The rows and columns are in the order `.NQxnq`. Having `MT`, it is straightforward to write a verb that merges configs `x` and `y:` index into `MT` using letters in `x` as row indices and letters in `y` as column indices.

<pre><tt>MT   =: ' '-.~' .NQxnq N???N? Q????Q x??xxx nN?xnx q?Qxxq'
MTy  =: (%:#MT){.MT
MTx  =: (##])MTy
merge=: MT"_ {~ MTx&amp;i.@[ + MTy&amp;i.@]

   (,~%:#MT) $ MT
.NQxnq
N???N?
Q????Q
x??xxx
nN?xnx
q?Qxxq

   (-: |:) (,~%:#MT) $ MT
1
   (&lt;n,n) $&amp;.&gt; x;y;x merge y
+--------+--------+--------+
|....xqxq|.n.....n|.n..xqxx|
|....qxxx|.n....n.|.n..qxxx|
|xxxxxxQx|.n...n..|xxxxxxQx|
|....qxxx|.n..n...|.n..xxxx|
|....xqxq|xnxn....|xnxnxqxq|
|...x..x.|nnnx....|nnnx..x.|
|..x...x.|nNnnnnnn|nNxnnnxn|
|.x....x.|nnnx....|nxnx..x.|
+--------+--------+--------+

   x (merge -: merge~) y
1</tt></pre>

The table `QT` is an `n*n` row table of configs, one for each possible placement of a queen. The table `NT` is an `n*n` row table of configs, one for each possible placement of a knight.

<pre><tt>UI  =: &gt;,{;~i.n
NI  =: _2]\_2 _1 _2 1 _1 _2 _1 2 1 _2 1 2 2 _1 2 1
QI  =: 0 0 -.~ (0&amp;,. , ,.&amp;0 , ,.~ , (,.|.)) i: n
mask=: 3 : 'UI e."2 UI +"1"1 2 y.'
QT  =: '.Qqx' {~ (=i.n*n)+(2*mask NI)+3*mask QI
NT  =: '.Nxn' {~ '.Qqx'i.QT

   $ QT
64 64
   $ NT
64 64
   (&lt;n,n) $&amp;.&gt; &lt;"1 (16 17 18 19){QT
+--------+--------+--------+--------+
|xqx.....|qxqx....|xqxqx...|.xqxqx..|
|xxq.....|xxxq....|qxxxq...|.qxxxq..|
|Qxxxxxxx|xQxxxxxx|xxQxxxxx|xxxQxxxx|
|xxq.....|xxxq....|qxxxq...|.qxxxq..|
|xqx.....|qxqx....|xqxqx...|.xqxqx..|
|x..x....|.x..x...|..x..x..|x..x..x.|
|x...x...|.x...x..|..x...x.|...x...x|
|x....x..|.x....x.|..x....x|...x....|
+--------+--------+--------+--------+

   (&lt;n,n) $&amp;.&gt; &lt;"1 (16 17 18 19){NT
+--------+--------+--------+--------+
|nxn.....|xnxn....|nxnxn...|.nxnxn..|
|nnx.....|nnnx....|xnnnx...|.xnnnx..|
|Nnnnnnnn|nNnnnnnn|nnNnnnnn|nnnNnnnn|
|nnx.....|nnnx....|xnnnx...|.xnnnx..|
|nxn.....|xnxn....|nxnxn...|.nxnxn..|
|n..n....|.n..n...|..n..n..|n..n..n.|
|n...n...|.n...n..|..n...n.|...n...n|
|n....n..|.n....n.|..n....n|...n....|
+--------+--------+--------+--------+</tt></pre>

## Queens and Knights

With verb `merge` and tables `QT` and `NT`, the queens and knights problem is ready for solution.

`q1` applies to a table of configs and produces new configs by placing one more queen in all possible (“`.`” or `q`) squares. Similarly, `n1` applies to a table of configs and produces new configs by placing one more knight in all possible (“`.`” or `n`) squares.

`x qx y` puts `x` additional queens on the configs `y` by calling `q1 x` times, at each step removing configs with less than the required number of “`.`” or `q` squares. Similarly, `x nx y` puts `x` additional knights on the configs `y`.

`qn y` finds all solutions of putting `y` queens and `y` knights on a chessboard, starting from the table of the empty config (`(1,n*n)$'.'`). Since a queen attacks many more squares than a knight, thereby reducing many more possibilities, it is more efficient to place the queens first, then the knights.

<pre><tt>q1=: 3 : '~.((+/"1 b)#y.) merge QT {~ (n*n)|I.,b=.
              y.e.''.q'''

n1=: 3 : '~.((+/"1 b)#y.) merge NT {~ (n*n)|I.,b=.
              y.e.''.n'''

qx=: 4 : 'for_j. (-i.)x. do. y.=. q1 y.#~j&lt;:+/"1
              y.e.''.q'' end.'

nx=: 4 : 'for_j. (-i.)x. do. y.=. n1 y.#~j&lt;:+/"1
              y.e.''.n'' end.'

qn=: 3 : 'y. nx y. qx (1,n*n)$''.'''</tt></pre>

It is obviously not possible to place 8 queens and 8 knights, since 8 queens alone already attack the entire board. We start with 7 queens and 7 knights and work down from there.

<pre><tt>   # t=: qn 7
0

   # t=: qn 6
0

   # t=: qn 5
16</tt></pre>

That is, up to 5 queens and 5 knights can be placed on an 8-by-8 chessboard, and there are 16 solutions. The solutions can be displayed by mapping the letters `Q` and `N` to themselves and other letters to “`.`”. Boxing and reshaping make a pleasing display.

<pre><tt>   $ t
16 64

   (n,n) $ {. t
xQxxxxxx
xxxxxxQx
xxQxxxxx
xxxxxQxx
xxxxxxxQ
Nxxxxxxx
NxxNNxxx
xxxNxxxx

   see=: ,~@%:@# $ 'QN'&amp;i. { 'QN.'"_

   see {. t
.Q......
......Q.
..Q.....
.....Q..
.......Q
N.......
N..NN...
...N....

   4 4 $ &lt;@see"1 t
+--------+--------+--------+--------+
|.Q......|..Q.....|..Q.....|...Q....|
|......Q.|.....Q..|.....Q..|......Q.|
|..Q.....|.Q......|.......Q|....Q...|
|.....Q..|......Q.|N.......|.N......|
|.......Q|........|NN......|NN......|
|N.......|Q.......|......Q.|.....Q..|
|N..NN...|....N..N|....Q...|.......Q|
|...N....|...NN..N|NN......|.NN.....|
+--------+--------+--------+--------+
|....Q...|.....Q..|.....Q..|......Q.|
|.Q......|..Q.....|..Q.....|.Q......|
|...Q....|Q.......|......Q.|.....Q..|
|......N.|.......N|.Q......|..Q.....|
|......NN|......NN|........|Q.......|
|..Q.....|.Q......|.......Q|.......N|
|Q.......|...Q....|N..N....|...NN..N|
|.....NN.|......NN|N..NN...|....N...|
+--------+--------+--------+--------+
|.....NN.|......NN|NN......|.NN.....|
|Q.......|...Q....|....Q...|.......Q|
|..Q.....|.Q......|......Q.|.....Q..|
|......NN|......NN|NN......|NN......|
|......N.|.......N|N.......|.N......|
|...Q....|Q.......|.......Q|....Q...|
|.Q......|..Q.....|.....Q..|......Q.|
|....Q...|.....Q..|..Q.....|...Q....|
+--------+--------+--------+--------+
|...NN..N|N..NN...|....N...|...N....|
|....N..N|N..N....|...NN..N|N..NN...|
|Q.......|.......Q|.......N|N.......|
|........|........|Q.......|.......Q|
|......Q.|.Q......|..Q.....|.....Q..|
|.Q......|......Q.|.....Q..|..Q.....|
|.....Q..|..Q.....|.Q......|......Q.|
|..Q.....|.....Q..|......Q.|.Q......|
+--------+--------+--------+--------+</tt></pre>

## Variants

Let `s` be one of the solutions as displayed in the previous section. It is clear that reflections and transpositions of `s`, and compositions thereof, are also solutions.

<pre><tt>   s=: see {. t
   (] ; |: ; |. ; |."1) s
+--------+--------+--------+--------+
|.Q......|.....NN.|...N....|......Q.|
|......Q.|Q.......|N..NN...|.Q......|
|..Q.....|..Q.....|N.......|.....Q..|
|.....Q..|......NN|.......Q|..Q.....|
|.......Q|......N.|.....Q..|Q.......|
|N.......|...Q....|..Q.....|.......N|
|N..NN...|.Q......|......Q.|...NN..N|
|...N....|....Q...|.Q......|....N...|
+--------+--------+--------+--------+
   NB. self ; transposition ; reflection 0 ; reflection 1</tt></pre>

`var` computes all reflectional and transpositional variants of a solution. There can be up to 8 variants.

<pre><tt>   var=: ~.@(] , |:&amp;.&gt; , |.&amp;.&gt; , |."1&amp;.&gt;)^:3
   var &lt;s
+--------+--------+--------+--------+--------+--------+--
|.Q......|.....NN.|...N....|......Q.|.NN.....|....Q...|
|......Q.|Q.......|N..NN...|.Q......|.......Q|.Q......|
|..Q.....|..Q.....|N.......|.....Q..|.....Q..|...Q....|
|.....Q..|......NN|.......Q|..Q.....|NN......|......N.| ...
|.......Q|......N.|.....Q..|Q.......|.N......|......NN|
|N.......|...Q....|..Q.....|.......N|....Q...|..Q.....|
|N..NN...|.Q......|......Q.|...NN..N|......Q.|Q.......|
|...N....|....Q...|.Q......|....N...|...Q....|.....NN.|
+--------+--------+--------+--------+--------+--------+--</tt></pre>

To find the unique solutions with respect to reflections and transpositions, we can proceed as follows:

- find all the variants of each solution;
- sort the variants;
- select the “smallest” of each set of variants;
- find the nub of these smallest variants.

<pre><tt>   uvar=: ~. @: ({.@(/:~)@var&amp;&gt;) @: (&lt;@&lt;@see"1)
   uvar t
+--------+--------+
|......Q.|......NN|
|.Q......|...Q....|
|.....Q..|.Q......|
|..Q.....|......NN|
|Q.......|.......N|
|.......N|Q.......|
|...NN..N|..Q.....|
|....N...|.....Q..|
+--------+--------+</tt></pre>

## Summary

- At most 5 queens and 5 knights can be placed on an 8-by-8 chessboard so that no piece attacks another.
- There are 16 different ways to place the 5 queens and 5 knights.
- If reflectional and transpositional variants are considered to be the same, then there are 2 different ways to place the 5 queens and 5 knights.

The following are the collected definitions for the queens and knights problem. The material in this article is available in the J 5.04 distribution as the “Queens and Knights” lab.

<pre><tt>NB.  .  free                    .  N  Q  x  n  q
NB.  N  N sits here             N  ?  ?  ?  N  ?
NB.  Q  Q sits here             Q  ?  ?  ?  ?  Q
NB.  x  forbidden               x  ?  ?  x  x  x
NB.  n  Q here would attack N   n  N  ?  x  n  x
NB.  q  N here would attack Q   q  ?  Q  x  x  q
NB.  ?  error

n    =: 8
MT   =: ' '-.~' .NQxnq N???N? Q????Q x??xxx nN?xnx q?Qxxq'
MTy  =: (%:#MT){.MT
MTx  =: (##])MTy
merge=: MT"_ {~ MTx&amp;i.@[ + MTy&amp;i.@]

UI   =: &gt;,{;~i.n
NI   =: _2]\_2 _1 _2 1 _1 _2 _1 2 1 _2 1 2 2 _1 2 1
QI   =: 0 0 -.~ (0&amp;,. , ,.&amp;0 , ,.~ , (,.|.)) i: n
mask =: 3 : 'UI e."2 UI +"1"1 2 y.'
QT   =: '.Qqx' {~ (=i.n*n)+(2*mask NI)+3*mask QI
NT   =: '.Nxn' {~ '.Qqx'i.QT

q1   =: 3 : '~.((+/"1 b)#y.) merge QT {~ (n*n)|I.,b=.
              y.e.''.q'''
n1   =: 3 : '~.((+/"1 b)#y.) merge NT {~ (n*n)|I.,b=.
              y.e.''.n'''
qx   =: 4 : 'for_j. (-i.)x. do. y.=. q1 y.#~j&lt;:+/"1
              y.e.''.q'' end.'
nx   =: 4 : 'for_j. (-i.)x. do. y.=. n1 y.#~j&lt;:+/"1
              y.e.''.n'' end.'
qn   =: 3 : 'y. nx y. qx (1,n*n)$''.'''

see  =: ,~@%:@# $ 'QN'&amp;i. { 'QN.'"_

var  =: ~.@(] , |:&amp;.&gt; , |.&amp;.&gt; , |."1&amp;.&gt;)^:3
uvar =: ~. @: ({.@(/:~)@var&amp;&gt;) @: (&lt;@&lt;@see"1)</tt></pre>

***
