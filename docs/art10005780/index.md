---
vid: '10005780'
title: 'At Play with J: Belgian Numbers'
authors:
- Eugene McDonnell
byline: Eugene McDonnell
volume: '22'
issue: '1'
page: '96'
source: trad/v221/belgium.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- mathematics and statistics
- programming techniques
---

Eugene McDonnell
{ .byline }

Belgian numbers were recently introduced by Eric Angelini. For <span class="APLU">N</span> to be a Belgian number, it is necessary that it appear in the sum scan of lots of replications of <span class="APLU">N</span>’s digits. For example, try the digits of <span class="APLU">16</span>:

```
   +/\8 $ 1 6
1 7 8 14 15 21 22 28
```

So <span class="APLU">16</span> isn’t Belgian. What about <span class="APLU">17</span>?

```
   +/\8$ 1 7
1 8 9 16 17 24 25 32
```

So <span class="APLU">17</span> *is* Belgian.

My first concern on hearing of them was to find out how one could tell whether a number was Belgian or not. Assume that <span class="APLU">N</span> is <span class="APLU">176</span>; how many copies of <span class="APLU">1 7 6</span> would be needed to reach <span class="APLU">176</span> or thereabouts? This can be found by taking the ceiling of <span class="APLU">N</span> divided by the sum of its digits <span class="APLU">S</span>. Like this:

```
     N =: 176
     ] D =: 10&#.^:_1 N
     1 7 6
   ] S =: + / D
14
   ] C =: >. N % S
13
```

Then multiplying this by its number of digits, in this case, <span class="APLU">3</span>, and sum scanning:

```
   ] R =: + / \  (C * 3) $ 1 7 6
1 8 14 15 22 28 29 36 42 43 50 56 57 64 70 71 78 84 85 92 98 99 106 112
113 120 126 127 134 140 141 148 154 155 162 168 169 176 182
```

and, sure enough, we find that <span class="APLU">176</span> is a Belgian number. This was, for the moment, an easy way to determine which numbers were Belgian – if they were not too big. I thought it might be possible to use it for numbers up to about ten million – but thereafter it gets ridiculous. For example, suppose that one wants to see whether <span class="APLU">1234567898765</span> is Belgian:

```
   N =: 1234567898765x
   ] D =: 10 &. #. ^_1 N
   #D
13
   ] N=:+/D
71
   >. B % S
17388280265
   13 * >. B % S
226047643445
```

Ouch. A fifth or so of a trillion (US style) digits. I concluded that this was impractical.

There had to be a better way. Eventually I thought I saw that in the case of <span class="APLU">176</span> there was a threeness lurking. I decided to form them into a table of three-item rows, together with the first difference of the rows, and the 14-residues of the whole table:

```
   qn =: 13 3 $ + / \ 39 $ 1 7 6
   qn      ;(2-~/\qn);14|qn
+-----------+--------+-----+
|  1   8  14|14 14 14|1 8 0|
| 15  22  28|14 14 14|1 8 0|
| 29  36  42|14 14 14|1 8 0|
| 43  50  56|14 14 14|1 8 0|
| 57  64  70|14 14 14|1 8 0|
| 71  78  84|14 14 14|1 8 0|
| 85  92  98|14 14 14|1 8 0|
| 99 106 112|14 14 14|1 8 0|
|113 120 126|14 14 14|1 8 0|
|127 134 140|14 14 14|1 8 0|
|141 148 154|14 14 14|1 8 0|
|155 162 168|14 14 14|1 8 0|
|169 176 182|        |1 8 0|
+-----------+--------+-----+
```

The first row of the first table shows that the sum of the integers is <span class="APLU">14</span>; the second table shows that each row differs from the previous row by <span class="APLU">14</span>, the third table shows that the <span class="APLU">14</span>-residue of each row is <span class="APLU">1 8 0</span>.

The point made by the second table is that each row after the first is formed by adding <span class="APLU">14</span> to the preceding row. This means that I could create qn exactly by adding multiples of 14 to the starting row:

```
   (14 * i. 13) + / 1 8 14
  1   8  14
 15  22  28
 29  36  42
 43  50  56
 57  64  70
 71  78  84
 85  92  98
 99 106 112
113 120 126
127 134 140
141 148 154
155 162 168
169 176 182
```

The point made by the third table is that any concern about residues after the first row is irrelevant.

With this information at hand, we’re almost there. The number <span class="APLU">176</span> is <span class="APLU">8</span> beyond <span class="APLU">168</span>, which is <span class="APLU">14
\* 12</span>. The <span class="APLU">14 \* 12</span> is irrelevant; the <span class="APLU">8</span> is significant. It means that<span class="APLU"> 8</span>, which is <span class="APLU">14|176</span>, is of great importance. It is one of the numbers in the first row of the table, and after being added to by multiple <span class="APLU">14</span>s, must arrive at <span class="APLU">176</span>, and so, is Belgian! This is a perfectly general observation, and allows any integer to be tested for Belgianity. Here is an outline of the steps to take to see if <span class="APLU">N</span> is Belgian:

Let <span class="APLU">D</span> be the digits of <span class="APLU">N</span>.

Let <span class="APLU">S</span> be the sum-scan of the digits of <span class="APLU">N</span>.

Let <span class="APLU">T</span> be the sum of <span class="APLU">D</span> (which is also the last item of <span class="APLU">S</span>).

Let <span class="APLU">R</span> be the <span class="APLU">T</span>-residue of <span class="APLU">N</span>.

Then <span class="APLU">N</span> is Belgian if <span class="APLU">R</span> is in <span class="APLU">S</span>.

For the case of <span class="APLU">N = 176</span>, we have:

<span class="APLU">D</span> is <span class="APLU">1 7 6</span>

<span class="APLU">S</span> is <span class="APLU">1 8 14</span>

<span class="APLU">T</span> is <span class="APLU">14</span>

<span class="APLU">R</span> is the <span class="APLU">T</span> residue of <span class="APLU">N</span>, or <span class="APLU">8</span>.

We ask: is <span class="APLU">R</span> in <span class="APLU">S</span>? The answer is Yes!

The general case is that <span class="APLU">N</span> is Belgian if:

```
(M | N) e. S
```

is <span class="APLU">1</span>.

Now it’s clear that <span class="APLU">176</span> is Belgian if any of these residues is the same as <span class="APLU">14 | 176</span>:

<pre class="APLU">   14| 176<br>
8</pre>

It is certain that if one adds the proper multiple of <span class="APLU">14</span> to <span class="APLU">8</span>, the result must be <span class="APLU">176</span>. It isn’t necessary to find what this multiple is. It is sufficient that there is an <span class="APLU">8</span> in the sum scan of <span class="APLU">1 7 6.</span>

Now we can write an expression to let us determine whether <span class="APLU">176</span> is Belgian:

```
   (14 | 176) e. + / \ 1 7 6
1
```

Another way of doing this is: since <span class="APLU">8</span> is the <span class="APLU">14</span> residue of <span class="APLU">176</span>, it is sufficient to see whether there is a zero in the <span class="APLU">14</span> residue of <span class="APLU">176 - + / \\ 1 7 6</span>

```
   14 | 176 - 1 8 14
7 0 8
```

So we may write:

```
   0 e. 14 | 176 - + / \ 1 7 6
1
```

to determine that <span class="APLU">176</span> is Belgian.

We can now work with large numbers with some confidence. First, we’ll need a way of getting the integer components of <span class="APLU">N</span>. The function <span class="APLU">dig</span> works very well:

```
   dig =: 1j1 & # &. ": @ x:
```

We’ll use this on <span class="APLU">1234567898765</span>:

```
   dig 1234567898765
1 2 3 4 5 6 7 8 9 8 7 6 5
   + / \ dig 1234567898765
1 3 6 10 15 21 28 36 45 53 60 66 71
   +/1 2 3 4 5 6 7 8 9 8 7 6 5
71
   71 | 1234567898765 - + / \ dig 1234567898765
20 18 15 11 6 0 64 56 47 39 32 26 21
```

There is a zero in this list, indicating that <span class="APLU">N</span> is Belgian. We can verify this.

<pre class="APLU">   N =: 1234567898765x
   S =: + / \ dig N
   M =: {: s
   S
1 3 6 10 15 21 28 36 45 53 60 66 71
   M
71
   Q =: &lt;. N % M
   Q
17388280264
   P =: Q * M
   P
1234567898744
   R =: S + P
   R
1234567898745 1234567898747  1234567898750 1234567898754
1234567898759 <b>1234567898765</b> 1234567898772 1234567898780
1234567898789 1234567898797  1234567898804 1234567898810 
1234567898815
   N e. R
1  </pre>

So <span class="APLU">1234567898765</span> is Belgian.

Here is a function that will determine whether or not a number is Belgian:

```
   BN =: 3 : 0 " 0
   N =. x: ^: (y. >: 2147483648) y.
   'S M' =. (] ; {:) + / \ (1j1 & # &. ":) N
   (M | N) e. S
   )
```

The first line makes the argument an extended integer <span class="APLU">N</span> if it is larger than the largest positive 32-bit integer, and otherwise leaves it alone. <span class="APLU">M</span> and <span class="APLU">S</span> are modulus and digits sum, as discussed above. See if <span class="APLU">BN</span> agrees that <span class="APLU">N</span> is Belgian:

```
   BN 1234567898765
1
```

## Why Belgian?

Why Belgian, indeed. Well, there are Roman numbers, Arabic numbers, even Catalan numbers. It’s about time Belgium was recognized. I could pretend that Eric Angelini (the creator of these numbers) knew that my wife Jeanne was a Fulbright Scholar at the University of Brussels for the 1952-1953 year – and named them Belgian in her honour. But I have to admit that this is quite far-fetched. The truth undoubtedly is – because Angelini is Belgian.

1 Dec 2006
