---
vid: '10010520'
title: 'J-ottings 38: Decline and Rise: A Matter of Rank'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '20'
issue: '2'
page: '37'
source: trad/v202/jot202.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2003
tags:
- language design
- programming techniques
---

*by Norman Thomson*
{ .byline }

Rank is of enormous importance in J. There are two types of rank, noun rank and verb rank. The former can be applied to any object in the J universe, and is the “tally-of-the-shape” or in J :

```
   nrank=.#@$
```

Verb rank is somewhat more subtle, and is of even greater importance in understanding J language constructs. Henry Rich put things with admirable conciseness in “J for C Programmers”:

> **“If you don’t know the rank of a verb, you don”t know the verb!”**

It is not just primitive verbs which possess rank, user defined verbs also possess it, and so do compound verbs, that is verbs formed by combining or adapting primitive verbs using appropriate conjunctions and adverbs. Moreover, there is no excuse for J programmers not knowing any relevant verb ranks, since these are immediately obtainable for any verb by using the `b.` adverb with right argument 0 :

<pre>   %. b. 0                 NB. primitive<tr>
2 _ 2</tr><tr>
   +/ b. 0                 NB. compound (adverbial)</tr><tr>
_ _ _ </tr><tr>
   +.@*: b. 0              NB. compound (using conjunction)</tr><tr>
0 0 0</tr><tr>
   mean=.+/ % # </tr><tr>
   mean b. 0               NB. user defined</tr><tr>
_ _ _ 

<p>
Within the J literature I am not aware of any explicit categorisation of verbs by rank, which is why much of this article consists of a lengthy appendix which does just this. I hope that some readers may find this appendix helpful, perhaps long after this preamble has been forgotten.</p>
<p>
As postings on the J conference testify, the distinction between <tt>@</tt> and <tt>@:</tt> is a stumbling block in the path of almost every new learner; what I hope to show here is how this block can be rapidly and effectively smoothed by a conscious endeavour to understand verb rank.</p>
<p>
In general verbs have three unforced ranks, which are, in the order given in the dictionary definitions: </p><blockquote>
<b>monadic rank,   left rank,   right rank </b></blockquote>

<p>
The word “unforced” acknowledges that verbs can have their basic ranks changed to an explicit value by using the rank conjunction <tt>"</tt> .</p></tr>
Classification by rank has some affinity with verb declensions in the classical languages, and although this analogy should not be pushed too far, it seems reasonable to think of verbs as being grouped by declensions along lines such as the following :

<blockquote><b>
1. Pure scalar verbs, all three rank vector items = 0 <br>
2. Pure list verbs, all rank vector items = 1<br>
3. Verbs with hybrid ranks, mostly specialised verbs<br>
4. Pure structural verbs i.e. all rank vector items = infinite<br>
4a. ‘Pure structural plus’ verbs, ranks are  _ 1 _
</b></blockquote>
<p>
It should be stressed that a verb rank of, say, 1 should not be read as meaning that only objects of noun rank 1 are acceptable as arguments, but rather that all arguments will be <i>processed</i> as assemblages of rank 1 objects. There is an analogy with the operation of a clinic which has a large and motley assembly in the waiting room, from which patients may either be called one by one (processing at rank 0), or by families (that is lists, rank = 1) or the whole lot may be taken together as a single block of humanity (rank infinite). The analogy goes further than this, because the method of calling is independent of what happens once the surgery is entered, in other words, rank comes first, semantics later. Immediately following the above quotation in Henry’s book is an explanation in detail of how rank is applied in particular cases, that is how rank and semantics are married together in terms of cell and frame selection. However, focussing on ranks shows that, apart from the “special algorithm” verbs, J verbs can be grouped into quite a small number of categories.</p>
<p>
A general problem in categorising verbs by rank is that of deciding when the monadic and dyadic forms are sufficiently related to each other to justify retaining this association even if  they possess different rank vectors. At one extreme monadic <tt>&gt;</tt> (open) and dyadic <tt>&gt;</tt> (greater than) are completely unrelated in meaning, even although all their rank vector items are zero. On the other hand verbs such as monadic <tt>#:</tt> (= antibase 2) and dyadic <tt>#:</tt> (= antibase) have a strong semantic association although formally they belong to different declensions.</p>
<p>
The first declension is in many ways the most familiar, consisting of pure scalar verbs which fall into three broad categories: logical, arithmetic and algorithmic.</p>
Declension 2 is the next easiest to review, since it contains verbs which make no sense other than when applied to lists, for example integers <tt>i.</tt> (= integers) and <tt>;:</tt> (= word formation).
<p>
Declension 4 consists of the pure structural verbs, all of whose ranks are infinite, meaning that their arguments are processed in their entirety as single objects.</p>
Declension 4a is sufficiently similar to the previous set to merit sub-classifying rather than a new declension. In each case the primary monadic form has infinite rank, but there is also a matching dyadic form which applies left arguments at the list level (i.e. left rank = 1).
<p>
Finally declension 3 consists of hybrids. Three of these are general, <tt>#:</tt> (monadic and dyadic) and <tt>{</tt> (from), the other five are specialised algorithmic verbs such as %. (= matrix inverse / divide), which are of great value to a minority of J users, and of little or no interest to the rest.</p>
<p>
Finally, compound verbs possess rank vectors in exactly in the same way as primitive verbs, and Henry’s maxim might be extended :</p>
<blockquote>
<b>“If you don’t know the rank of a compound verb, you don’t know the conjunction/adverb!”</b></blockquote>
<p>
The dictionary descriptions may seem somewhat bewildering at first, with references to mv, lv, etc. Not so! The present appendix contains a list of conjunction rank rules, together with the permissible noun-verb combinations. For each vv combination the conjunction has the form "u conjunction v", and m=monadic, l=left, r=right. </p>
<p>
<i>atop</i> (<tt>@</tt>) has rank vector mv lv rv, which means that in all three cases the rank of the compound verb is identical to that of the right verb in the compound. For <i>at</i> (<tt>@:</tt>), however, all ranks are infinite regardless of the composed verbs. So for </p>

<pre>   2 3(+/@%)4 5      NB. rank of verb v(%) is zero </pre>
<p>
using the clinic analogy, the “patients” are called in matched pairs, and divided to give the result  <tt>0.5 0.6</tt>. Then <tt>+/</tt> at rank 0 leaves items unchanged for a final result <tt>0.5 0.6</tt>. However, for </p>

<pre>    2 3(+/@:%)4 5</pre>
<p>
the patients are called in two blocks (left and right), which are divided as blocks to give 0.5 0.6, and then "plus-insert" is applied to this single block to give 1.1.</p>
Informally the difference between atop and at is that u and v are more closely bound in the former. The name “atop” is very apt as it gives a picture of two creatures, one piggy-backing on the other, and thereby fusing to make a tight compound before any considerations of data come into play. “at” conveys less well the way in which infinite rank produces verb sequencing, that is “u following v”, is a more meaningful phrase than “u at v”. Here are some further examples of atop and at : 
<p>
Compare <tt>({:@*:)i.2 3</tt> and <tt>({:@:*:)i.2 3</tt> . As above, the only rank which matters in the first case is that of <tt>*:</tt> (= square) which is 0. The semantic rule which extends tail when applied to scalars (i.e. at rank 0) is “no change” and so the final result is </p>

<pre>0  1  4<tr>
9 16 25</tr>
In the second case, tail with infinite rank means the tail of the list of lists arising from squaring, which gives the result list <tt>9 16 25</tt>.

<p>
Compare <tt>({:@#.)2 3$1 0 1 1 1 0 </tt>  and   <tt>({:@:#.)2 3$1 0 1 1 1 0</tt></p>
<p>
Here the rank of the rightmost verb is 1, and so in the first case tail is applied to <i>each</i> of the antibase 2s of the two three-lists which make up the right argument, giving a final result  <tt>5 6</tt>. In the second case, tail applies to the entire result of <tt>#.</tt> which is a two-list, so that the final result is <tt>6</tt>.</p>
<p>
Compare <tt>2 3 4(+/@%.)m=.?2 3 3$10</tt>  and  <tt>2 3 4(+/@:%.)m</tt></p>
This is a dyadic example, in which the rank of the rightmost verb is 2.  <tt>m</tt> represents two sets of 3 by 3 linear equations with the same right hand side <tt>2 3 4</tt>.  In the first case the result is the sums of <i>each</i> of the three solution sets (x<sub>1</sub>+ y<sub>1</sub>+ z<sub>1</sub>,  x<sub>2</sub>+ y<sub>2</sub>+ z<sub>2</sub>) . In the second case summation applies to the two-list of solution values, so that the final result is (x<sub>1</sub>+ x<sub>2</sub>, y<sub>1</sub>+ y<sub>2</sub>, z<sub>1</sub>+z<sub>2</sub>) .
<p>
... and so I could continue. However, space is not unlimited, and in any case, if I have succeeded in explaining the general principles, then the reader will already have reached a point where he needs no further assistance from me!</p>

<hr>

<h1 align="center">Appendix : Verb Ranks</h1>

<h2>Declension 1, rank vector = 0 0 0 </h2>

<h3>Logicals (all dyadic)  </h3>

</pre></pre>

<table><tr><td>
<tt>=</tt> Equal</td><td><tt>~:</tt> </td><td> Not-Equals </td></tr><tr><td>
<tt>&lt;</tt> Less Than</td><td><tt>&lt;:</tt> </td><td> Less than or Equal</td></tr><tr><td>
<tt>&gt;</tt> Larger Than</td><td><tt>&gt;:</tt> </td><td>Larger or Equal</td></tr></table>

### Arithmetics

<table><tr><td></td><td><b>Monadic</b></td><td><b>Dyadic</b></td><td></td><td><b>Monadic</b></td><td><b>Dyadic</b></td><td></td><td><b>Monadic</b></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>+</tt></td><td>Conjugate</td><td>Plus</td><td><tt>+.</tt></td><td>Real/Imag</td><td>GCD(Or)</td><td><tt>+:</tt></td><td>Double  </td><td>Not-Or</td></tr><tr><td>
<tt>-</tt></td><td>Negate  </td><td>Minus</td><td><tt>-.</tt></td><td>Not </td><td></td><td><tt>-:</tt></td><td>Halve</td></tr><tr><td>
<tt>*</tt></td><td>Signum  </td><td>Times</td><td><tt>*.</tt></td><td>Len/Angle</td><td>LCM(And)</td><td><tt>*:</tt></td><td>Square  </td><td>Not-And</td></tr><tr><td>
<tt>%</tt></td><td>Reciprocal  </td><td>Divided by</td><td></td><td></td><td></td><td>%:</td><td>Sq Root </td><td>Root</td></tr><tr><td>
<tt>^</tt></td><td>Exponential</td><td>Power</td><td><tt>^.</tt></td><td>Natural Log</td><td>Logarithm</td></tr><tr><td>
<tt>&lt;.</tt></td><td></td><td>Lesser of</td><td><tt>&gt;.</tt></td><td></td><td>Larger of</td></tr><tr><td>
<tt>|</tt></td><td>Magnitude  </td><td>Residue</td></tr><tr><td>
<tt>!</tt></td><td>Factorial  </td><td>Out Of</td></tr></table>

### Algorithmics

<table><tr><td>
</td><td><b>Monadic</b></td><td><b>Dyadic</b></td><td></td><td><b>Monadic</b></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>?</tt></td><td>Roll</td><td>Deal</td><td><tt>p:</tt></td><td>Primes</td></tr><tr><td>
<tt>j.</tt></td><td>Imaginary</td><td>Complex</td><td><tt>q:</tt></td><td>Prime Factors</td><td>Prime Exponents</td></tr><tr><td>
<tt>o.</tt></td><td>Pi Times</td><td>Circle Functions</td><td><tt>r.</tt></td><td>Angle</td><td>Polar</td></tr></table>

## Declension 2, all rank vector items = 1

<table><tr><td>
</td><td><b>Monadic</b></td><td></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>i.</tt></td><td>Integers</td><td>1</td></tr><tr><td>
<tt>{</tt></td><td>Catalogue</td><td>1</td></tr><tr><td>
<tt>;:</tt></td><td>Word formation</td><td>1</td></tr><tr><td>
<tt>#.</tt></td><td>Base 2</td><td>1</td><td>Base</td><td>1  1</td></tr><tr><td>
<tt>".</tt></td><td>Do</td><td>1</td></tr></table>

## Declension 3, hybrid ranks

<table><tr><td>
</td><td><b>Monadic</b></td><td></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>#:</tt></td><td>Antibase 2</td><td><tt>_</tt></td><td>Antibase</td><td>1</td><td>0</td></tr><tr><td>
<tt>{</tt> </td><td></td><td></td><td>From</td><td>0</td><td><tt>_</tt></td></tr><tr><td>
<tt>%.</tt></td><td>Matrix Inverse</td><td>2</td><td>Matrix Divide</td><td><tt>_</tt></td><td>2</td></tr><tr><td>
<tt>A.</tt></td><td>Atomic Permute</td><td>1</td><td>Atomic Permute</td><td>0</td><td><tt>_</tt></td></tr><tr><td>
<tt>c.</tt></td><td>Characteristic</td><td>2</td><td>Eigenvalues</td><td>0</td><td>2</td></tr><tr><td>
<tt>C.</tt></td><td>Cycle</td><td>1</td><td>Permute</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>p.</tt></td><td>Roots</td><td>1</td><td>Polynomial</td><td>1</td><td>0</td></tr></table>

## Declension 4, all ranks = infinite

<table><tr><td>
</td><td><b>Monadic</b></td><td></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>=</tt></td><td>Self-Classify</td><td><tt>-.</tt></td><td>Less</td></tr><tr><td>
<tt>&lt;</tt></td><td>Box</td><td><tt>-:</tt></td><td>Match</td></tr><tr><td>
<tt>~.</tt></td><td>Nub </td><td><tt>i.</tt></td><td>Index Of</td></tr><tr><td>
<tt>~:</tt></td><td>Nub Sieve</td><td><tt>E.</tt></td><td>Member of Interval</td></tr><tr><td>
<tt>{:</tt></td><td>Tail</td><td><tt>".</tt></td><td>Numbers</td></tr><tr><td>
<tt>}:</tt></td><td>Curtail</td></tr><tr><td>
<tt>L.</tt></td><td>Level Of</td><td></td></tr></table>

<table><tr><td>
 </td><td><b>Monadic</b></td><td><b>Dyadic</b></td><td></td><td><b>Monadic</b></td><td><b>Dyadic</b></td><td></td><td><b>Monadic</b></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>,</tt></td><td>Ravel </td><td>Append</td><td><tt>,.</tt></td><td>Ravel Items</td><td>Stitch</td><td><tt>,:</tt></td><td>Itemize</td><td>Laminate</td></tr><tr><td>
<tt>/:</tt></td><td>Grade Up</td><td>Sort Up</td><td><tt>\:</tt></td><td>Grade Down</td><td>Sort Down</td></tr><tr><td>
<tt>;</tt></td><td>Raze</td><td>Link</td><td><tt>e.</tt></td><td>Raze In</td><td>Member In</td></tr><tr><td>
<tt>$.</tt></td><td>Sparse</td><td>Sparse</td><td><tt>$:</tt></td><td>Self-Reference</td><td>Self-Reference</td></tr><tr><td>
<tt>[</tt></td><td>Same </td><td>Left</td><td><tt>]</tt></td><td>Same</td><td>Right</td></tr><tr><td>
<tt>{::</tt></td><td>Map</td><td>Fetch</td></tr></table>

<table><tr><td>
 </td><td><b>Monadic and Dyadic</b></td></tr><tr><td>
<tt>s:</tt></td><td>Symbol</td></tr><tr><td>
<tt>u:</tt></td><td>Unicode</td></tr><tr><td>
<tt>x:</tt></td><td>Extended Precision</td></tr></table>

Constant Functions, that is `9:, _8:,..., 0:, 1:,2:, ... 9:` and also `_:` (infinity)

## Declension 4a, rank vector = inf 1 inf

<table><tr><td>
</td><td><b>Monadic</b></td><td></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>$</tt></td><td>Shape Of</td><td><tt>_</tt></td><td>Shape</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>|.</tt></td><td>Reverse</td><td><tt>_</tt></td><td>Rotate (Shift)</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>|:</tt></td><td>Default Transpose</td><td><tt>_</tt></td><td>Transpose</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>#</tt></td><td>Tally</td><td>_</td><td>Copy</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>{.</tt></td><td>Head</td><td><tt>_</tt></td><td>Take</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>}.</tt></td><td>Behead</td><td><tt>_</tt></td><td>Drop</td><td>1</td><td><tt>_</tt></td></tr><tr><td>
<tt>":</tt></td><td>Default Format</td><td><tt>_</tt></td><td>Format</td><td>1</td><td><tt>_</tt></td></tr></table>

## Conjunctions

<table><tr><td>
</td><td>nn</td><td>nv</td><td>vn</td><td>vv</td><td></td><td>Rank Vector</td></tr><tr><td>
<tt>&amp;</tt></td><td></td><td></td><td>x</td><td>x</td><td>bond, compose</td><td>mv</td><td>mv</td><td>mv</td></tr><tr><td>
<tt>&amp;.</tt></td><td></td><td></td><td>x</td><td></td><td>under</td><td>mv</td><td>mv</td><td>mv</td></tr><tr><td>
<tt>&amp;:</tt></td><td></td><td></td><td>x</td><td></td><td>appose</td><td><tt>_</tt></td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>@</tt></td><td></td><td></td><td></td><td>x</td><td>atop</td><td>mv</td><td>lv</td><td>rv</td></tr><tr><td>
<tt>@.</tt></td><td></td><td>x</td><td></td><td></td><td>agenda (with gerund)</td><td>mv</td><td>lv</td><td>rv</td></tr><tr><td>
<tt>@:</tt></td><td></td><td></td><td></td><td>x</td><td>at</td><td><tt>_</tt></td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>.</tt></td><td></td><td></td><td></td><td>x</td><td>dot</td><td> 2</td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>:.</tt></td><td></td><td></td><td></td><td></td><td>obverse</td><td>mu</td><td>lu</td><td>ru</td></tr><tr><td>
<tt>::</tt></td><td></td><td></td><td></td><td></td><td>adverse</td><td><tt>_</tt></td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>^:</tt></td><td></td><td></td><td>x</td><td></td><td>power</td><td><tt>_</tt></td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>`:</tt></td><td>x</td><td></td><td></td><td></td><td>evoke gerund</td><td><tt>_</tt></td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>;.</tt></td><td></td><td></td><td>x</td><td></td><td>cut</td><td><tt>_</tt></td><td>1/2</td><td><tt>_</tt>  *</td></tr><tr><td>
<tt>d.</tt></td><td></td><td></td><td>x</td><td></td><td>derivative</td><td>0</td><td></td></tr><tr><td>
<tt>D.</tt></td><td></td><td></td><td>x</td><td></td><td>derivative</td><td>mu</td></tr><tr><td>
<tt>H.</tt></td><td></td><td></td><td>x</td><td></td><td>hypergeometric</td><td>0</td><td>0</td><td>0 </td></tr></table>

<td>*</td>

<td>1/2 is appropriate because cut is the J mechanism for partitions, for which there is a wide range of options, some of which process the left argument on a rank 1 basis, others on a rank 2 basis.
<h2>Adverbs</h2>
<table><tr><td>
</td><td><b>Monadic</b></td><td></td><td><b>Dyadic</b></td></tr><tr><td>
<tt>/</tt> </td><td>Insert</td><td><tt>_</tt></td><td>Table</td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>/.</tt> </td><td>Oblique</td><td><tt>_</tt></td><td>Key</td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>\</tt></td><td>Prefix</td><td><tt>_</tt></td><td>Infix</td><td>0</td><td><tt>_</tt></td></tr><tr><td>
<tt>\.</tt></td><td>Suffix</td><td><tt>_</tt></td><td>Outfix</td><td>0</td><td><tt>_</tt></td></tr><tr><td>
<tt>~<tt>/.</tt> </tt></td><td>Reflexive</td><td><tt>_</tt></td><td>Passive</td><td>ru</td><td>lu</td></tr><tr><td>
<tt>{.<tt>/.</tt> </tt></td><td>Item Amend</td><td><tt>_</tt></td><td>Amend</td><td><tt>_</tt></td><td><tt>_</tt></td></tr><tr><td>
<tt>b.<tt>/.</tt> </tt></td><td>Basic characteristics</td><td><tt>_</tt></td><td>Boolean</td><td>0</td><td>0</td></tr></table>

<hr>

<!-- breadcrumbs -->


</td>
