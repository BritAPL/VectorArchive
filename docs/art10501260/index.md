---
vid: '10501260'
title: A letter from Dijkstra on APL
authors:
- Roger Hui
byline: Roger K.W. Hui
volume: '26'
issue: '2'
page: '69'
received: '2013-12-20'
online: '2014-01-19'
description: A Letter from Dijkstra on APL
source: content/printed/262/dijkstra/dijkstra.htm
converter: vecrescue 0.1.0
issue_tab: n23-september-2014
tags:
- APL in perspective
---

Roger K.W. Hui
{ .byline }

**Acknowledgments.** I would like to thank Bob Bernecky, Nicolas Delcros, Jay Foad, and Eric Iverson for comments on the manuscript.

Nick Nickolov brought to my attention comments by Dijkstra on APL [\[1\]](#ref1) that I had not seen before. I contacted the author of the website and obtained a copy of Dijkstra’s letter, transcribed below:

<pre>
 <strong>Burroughs</strong>
 PROF DR EDSGER W DIJKSTRA                                       PLATAANSTRAAT 5
 RESEARCH FELLOW                                  5671 AL NUENEN THE NETHERLANDS

 Dr A.Caplin
 [street address]
 CROYDON, Surrey
 United Kingdom
                                    Tuesday 12 January 1982

 Dear Dr Caplin,

    thank [<em>sic</em>] you for your letter dated 31 May (?) 1981. You were right in
 your reference to an APL “cult”: some adore it and others abhor it with very
 few in between. Allow me to offer you another explanation for that
 phenomenon.

    I think that most people (be it subconsciously) realize that “ease of use”
 is not the most significant aspect. Experience has show that, provided
 people are sufficiently thrilled by a gadget, they are willing to put up with
 the most terrible interfaces. Much more important is that the tool shapes the
 one who trains himself in its usage, just as the words we use shape our
 thoughts and the instrument forms the violinist. I think that a major reason
 for shunning APL is that many people are repelled by the influence APL has
 on its devotees. They implement the prayer “Dear Lord, don’t let me
 become like them” by ignoring it.

    A typical characteristic of the APL devotee is, for instance, his closeness
 to an implementation of it. I know of a visiting professor at an American
 University [<em>sic</em>] who, trying to teach APL, bitterly complained about the
 absence of APL terminals. He was clearly unable to teach it without them.
 And you, too, write to me that you would like to meet me in your part of the
 world, so that you can “demonstrate APL” to me. This is in sharp contrast to
 people who prefer programming languages that can be adadequately [<em>sic</em>]
 “demonstrated”—i.e. shown, taught and discussed—with pencil and paper.

    The fact that the printed or written word is apparently not the proper
 medium for the propagation of APL may offer a further explanation for its
 relative isolation; at the same time that fact may be viewed as one of its
 major shortcomings.

    Your writings made me wonder in which discipline you got your
 doctor’s degree.

    With my greetings and best wishes,
                                yours ever,
                                  (signed) Edsger W. Dijkstra

 PS. I apologize for the quality of my signature; having broken my right arm
 I have to sign with my left hand.
                                   EWD
</pre>

I find Dijkstra’s comments deeply ironic, because Ken Iverson invented his notation as a means of communications among people [\[2\]](#ref2), and it was only years later that the notation was implemented on a computer at which time it became APL. Moreover, Dijkstra encountered “Iverson notation” no later than August 1963 before there was an implementation [\[3\]](#ref3). Even with APL, perhaps especially with APL, one can reasonably do non-trivial things without ever executing it on a computer.

I have read at least one of Dijkstra’s EWDs in which he wrote programs using formal methods, at the end of which is derived a provably correct program. As I read it/them, I thought to myself, “APL should have been natural for Dijkstra”. One can argue what “provably correct program” means. To me, it means what a typical mathematician means when he/she says a theorem has been proven. I know it is far from saying that the program will produce a correct result in all circumstances (compiler/interpreter has a bug, somebody pulled the plug, cosmic ray strikes a transistor, etc.), but I believe I am using “prove” in the same sense that Dijkstra did.

Like Dijkstra’s “visiting professor at an American university”, I would be distressed if I had to teach a course on APL without an APL machine. Were it a course on formal methods, one can get by without a machine; but even in a course on formal methods executability would be an asset, because executability keeps you honest, a faithful servant that can be used to check the steps of a proof. Were it a general programming course, it seems extreme to eschew the use of a machine in showing, teaching, and discussing. It would be like trying to learn a natural language without ever conversing with a speaker of that language.

Herewith, two examples of using APL in formal manipulations. Further such examples can be found in Iverson’s Turing Award Lecture [\[4\]](#ref4). A proof is here presented as in [\[4\]](#ref4), a sequence of expressions each identical to its predecessor, annotated with the reasoning.

A Summary of Notation is provided at the end.

## Example 1: Ackermann’s Function

The derivation first appeared in 1992 [\[5\]](#ref5) in J and is transcribed here in Dyalog APL.

Ackermann’s function is defined on non-negative integers as follows:

```
   ack←{
     0=⍺: 1+⍵
     0=⍵: (⍺-1) ∇ 1
     (⍺-1) ∇ ⍺ ∇ ⍵-1
   }

   2 ack 3
9
   3 ack 2
29
```

Lemma: If `⍺ ack ⍵ ←→ f⍢(3∘+) ⍵` , then `(⍺+1)ack ⍵ ←→ f⍣(1+⍵)⍢(3∘+) 1` .

Proof: By induction on `⍵` .

<table>
<tr><td><code>(⍺+1) ack 0                    </code></td><td>basis</td></tr>
<tr><td><code>⍺ ack 1                        </code></td><td>definition of ack</td></tr>
<tr><td><code>f⍢(3∘+) 1                      </code></td><td>antecedent of lemma</td></tr>
<tr><td><code>f⍣(1+0)⍢(3∘+) 1                </code></td><td><code>⍣</code></td></tr>
<tr><td><br></td></tr>
<tr><td><code>(⍺+1) ack ⍵                    </code></td><td>induction</td></tr>
<tr><td><code>⍺ ack (⍺+1) ack ⍵-1            </code></td><td>definition of ack</td></tr>
<tr><td><code>f⍢(3∘+) (⍺+1) ack ⍵-1          </code></td><td>antecedent of lemma</td></tr>
<tr><td><code>f⍢(3∘+) f⍣(1+⍵-1)⍢(3∘+) 1      </code></td><td>inductive hypothesis</td></tr>
<tr><td><code>¯3∘+ f 3∘+ ¯3∘+ f⍣(1+⍵-1) 3∘+ 1</code></td><td><code>⍢</code></td></tr>
<tr><td><code>¯3∘+ f f⍣(1+⍵-1) 3∘+ 1         </code></td><td><code>+</code></td></tr>
<tr><td><code>¯3∘+ f⍣(1+⍵) 3∘+ 1             </code></td><td><code>⍣</code></td></tr>
<tr><td><code>f⍣(1+⍵)⍢(3∘+) 1                </code></td><td><code>⍢</code></td></tr>
<tr><td></td><td>            QED</td></tr>
</table>

Using the lemma (or otherwise), it can be shown that:

```
0∘ack = 1∘+⍢(3∘+)
1∘ack = 2∘+⍢(3∘+)
2∘ack = 2∘×⍢(3∘+)
3∘ack = 2∘*⍢(3∘+)
4∘ack = */∘(⍴∘2)⍢(3∘+)
5∘ack = {*/∘(⍴∘2)⍣(1+⍵)⍢(3∘+) 1}
```

## Example 2: Inverted Table Index-Of

Presented at the 2013 Dyalog Conference [\[6\]](#ref6).

A *table* is a set of values organized into rows and columns. The rows are records. Values in a column have the same type and shape. A table has a specified number of columns but can have any number of rows. The extended *index-of* on tables finds record indices.

```
   tx                   ty                    tx ⍳ ty
┌──────┬─┬───┬──┐    ┌──────┬─┬───┬──┐     3 1 5 2 5 5
│John  │M│USA│26│    │Min   │F│CN │17│
├──────┼─┼───┼──┤    ├──────┼─┼───┼──┤        tx ⍳ tx
│Mary  │F│UK │24│    │Mary  │F│UK │24│     0 1 2 3 4
├──────┼─┼───┼──┤    ├──────┼─┼───┼──┤
│Monika│F│DE │31│    │John  │M│UK │26│        ty ⍳ ty
├──────┼─┼───┼──┤    ├──────┼─┼───┼──┤     0 1 2 3 4 4
│Min   │F│CN │17│    │Monika│F│DE │31│
├──────┼─┼───┼──┤    ├──────┼─┼───┼──┤
│Max   │M│IT │29│    │Mesut │M│DE │24│
└──────┴─┴───┴──┘    ├──────┼─┼───┼──┤
                     │Mesut │M│DE │24│
                     └──────┴─┴───┴──┘
```

An *inverted table* is a table with the values of a column collected together. *Comma-bar each* (`⍪¨`) applied to an inverted table makes it look more like a table. And of course the columns have the same *tally* (`≢`). A table can be readily inverted and *vice versa*.

```
   x
┌──────┬─────┬───┬──────────────┐
│John  │MFFFM│USA│26 24 31 17 29│
│Mary  │     │UK │              │
│Monika│     │DE │              │
│Min   │     │CN │              │
│Max   │     │IT │              │
└──────┴─────┴───┴──────────────┘
   ⍪¨x                     ≢¨x
┌──────┬─┬───┬──┐       5 5 5 5
│John  │M│USA│26│
│Mary  │F│UK │24│
│Monika│F│DE │31│
│Min   │F│CN │17│
│Max   │M│IT │29│
└──────┴─┴───┴──┘

   invert ← {↑¨↓⍉⍵}
   vert   ← {⍉↑⊂⍤¯1¨⍵}

   x  ≡ invert tx
1
   tx ≡ vert x
1
```

A table has array overhead per element. An inverted table has array overhead per column. The difference that this makes becomes apparent when you have a sufficiently large number of rows. The other advantage of an inverted table is that column access is much faster.

An important computation is `x index-of y` where `x` and `y` are compatible inverted tables. Obviously, it can not be just `x⍳y` . The computation obtains by first *verting* the arguments (un-inverting the tables) and then applying `⍳` , but often there is not enough space for that.

```
   ⍪¨x                   ⍪¨y
┌──────┬─┬───┬──┐     ┌──────┬─┬───┬──┐
│John  │M│USA│26│     │Min   │F│CN │17│
│Mary  │F│UK │24│     │Mary  │F│UK │24│
│Monika│F│DE │31│     │John  │M│UK │26│
│Min   │F│CN │17│     │Monika│F│DE │31│
│Max   │M│IT │29│     │Mesut │M│DE │24│
└──────┴─┴───┴──┘     │Mesut │M│DE │24│
                      └──────┴─┴───┴──┘
   x ⍳ y
4 4 4 4

   (vert x) ⍳ (vert y)
3 1 5 2 5 5
```

We derive a more efficient computation of *index-of* on inverted tables:

```
(vert x) ⍳ (vert y)             (a)
({⍉↑⊂⍤¯1¨⍵}x) ⍳ ({⍉↑⊂⍤¯1¨⍵}y)   (b)
(⍉↑⊂⍤¯1¨x) ⍳ (⍉↑⊂⍤¯1¨y)         (c)
(⍉↑x⍳¨x) ⍳ (⍉↑x⍳¨y)             (d)
```

(a) The indices obtain by first uninverting the tables, that is, by first applying `vert` .

(b) Replace `vert` by its definition.

(c) Replace the D-fn by its definition. We see that `⊂⍤¯1` plays a major role. `⊂⍤¯1` encloses, or alternatively computes a *scalar representation*.

(d) For purposes of *index-of* `x⍳¨x` and `x⍳¨y` have the same information as `⊂⍤¯1¨x` and `⊂⍤¯1¨y` , but are much more efficient representations (small integers *v* the data itself).

Point (d) illustrated on column 0:

```
   ⊂⍤¯1⊢x0←0⊃x
┌──────┬──────┬──────┬──────┬──────┐
│John  │Mary  │Monika│Min   │Max   │
└──────┴──────┴──────┴──────┴──────┘
   x0 ⍳ x0
0 1 2 3 4

   ⊂⍤¯1⊢y0←0⊃y
┌──────┬──────┬──────┬──────┬──────┬──────┐
│Min   │Mary  │John  │Monika│Mesut │Mesut │
└──────┴──────┴──────┴──────┴──────┴──────┘
   x0 ⍳ y0
3 1 0 2 5 5
```

That is, the function `{(⍉↑⍺⍳¨⍺)⍳(⍉↑⍺⍳¨⍵)}` computes `index-of` on inverted tables.

I believe that in another language a derivation such as the one above would be very long (in part because the program would be very long), possibly impractically long.

## Summary of Notation

The following table lists the APL notation used in the paper. A complete language reference can be found in [\[7\]](#ref7). D-fns are described in \[7, pp. 112-127\] and [\[8\]](#ref8).

<table>
<tr><td><code>←→ </code></td><td>equivalent (extralingual)</td></tr>
<tr><td><code>←  </code></td><td>assignment                </td></tr>
<tr><td><code>⍺  </code></td><td>left    argument</td></tr>
<tr><td><code>⍵  </code></td><td>right argument</td></tr>
<tr><td><code>×  </code></td><td>times</td></tr>
<tr><td><code>*  </code></td><td>exponentiation</td></tr>
<tr><td><code>⍴  </code></td><td>reshape; <code>n⍴s</code> makes <code>n</code> copies of <code>s</code></td></tr>
<tr><td><code>⍉  </code></td><td>transpose</td></tr>
<tr><td><code>⍳  </code></td><td>index-of</td></tr>
<tr><td><code>⊂  </code></td><td>enclose</td></tr>
<tr><td><code>⊃  </code></td><td>pick</td></tr>
<tr><td><code>↑  </code></td><td>mix (disclose)</td></tr>
<tr><td><code>↓  </code></td><td>split (enclose rows)</td></tr>
<tr><td><code>⍪  </code></td><td>table, ravel the major cells</td></tr>
<tr><td><code>≡  </code></td><td>match</td></tr>
<tr><td><code>≢  </code></td><td>tally, the length of the leading dimension</td></tr>
<tr><td><code>⊢  </code></td><td>right (identity function)</td></tr>
<tr><td><code>f∘g</code></td><td>function composition</td></tr>
<tr><td><code>a∘f</code></td><td>currying (fix left    argument)</td></tr>
<tr><td><code>f∘a</code></td><td>currying (fix right argument)</td></tr>
<tr><td><code>f⍤r</code></td><td>rank operator; <code>f</code> on rank <code>r</code> subarrays</td></tr>
<tr>
    <td>
        <code>f⍣n</code>
    </td>
    <td>
        power operator; <code>n</code> applications of <code>f</code> ; the n-th iterate of <code>f</code><br> (<code>f⍣¯1</code> is the inverse of <code>f</code>)
    </td>
</tr>
<tr><td><code>f⍢g    </code></td><td>dual operator; <code>g⍣¯1∘f∘g</code> (not yet implemented in Dyalog APL)</td></tr>
<tr><td><code>f/     </code></td><td>reduce (fold)</td></tr>
<tr><td><code>f¨     </code></td><td>each (map)</td></tr>
<tr><td><code>{⍺ … ⍵}</code></td><td>D-function</td></tr>
<tr><td><code>∇      </code></td><td>D-function: recursion</td></tr>
<tr><td><code>:      </code></td><td>D-function: guard</td></tr>
</table>

## References

1. <a id="ref1"></a> Daylight, Edgar Graham, A Letter about APL, 2012-04-05.  
    [http://www.dijkstrascry.com/node/90](http://www.dijkstrascry.com/node/90)
2. <a id="ref2"></a> Iverson, Kenneth E., A Personal View of APL, IBM Systems Journal, Volume 30, Number 4, 1991-12. [http://www.jsoftware.com/papers/APLPersonalView.htm](http://www.jsoftware.com/papers/APLPersonalView.htm)
3. <a id="ref3"></a> Iverson, Kenneth E., Formalism in Programming Languages, Communications of the ACM, Volume 7, Number 2, 1964-02. See the last question in the discussion.  
    [http://www.jsoftware.com/papers/FPL.htm](http://www.jsoftware.com/papers/FPL.htm)
4. <a id="ref4"></a> Iverson, Kenneth E., Notation as a Tool of Thought, Communications of the ACM, Volume 23, Number 8, 1980-08. [http://www.jsoftware.com/papers/tot.htm](http://www.jsoftware.com/papers/tot.htm)
5. <a id="ref5"></a> Hui, Roger K.W., Three Combinatoric Puzzles, Vector, Volume 9, Number 2, 1992-10; also in Ackermann’s Function, J Wiki Essay, 2005-10-14.  
    [http://www.jsoftware.com/jwiki/Essays/Ackermann%27s%20Function](http://www.jsoftware.com/jwiki/Essays/Ackermann%27s%20Function)
6. <a id="ref6"></a> Hui, Roger K.W., Rank & Friends, 2013 Dyalog Conference, 2013-10-22.  [http://www.dyalog.com/dyalog\_13/presentations/D08\_Rank\_and\_Friends/ friendsscript.htm](http://www.dyalog.com/dyalog_13/presentations/D08_Rank_and_Friends/friendsscript.htm)
7. <a id="ref7"></a> Dyalog Limited, Dyalog APL Programmer’s Guide & Language Reference, Version 13.1, 2012. [http://docs.dyalog.com/13.1/Dyalog%20APL%20Programmer%27s%20 Guide%20&%20Language%20Reference.pdf](http://docs.dyalog.com/13.1/Dyalog%20APL%20Programmer%27s%20Guide%20&%20Language%20Reference.pdf)
8. <a id="ref8"></a> Scholes, John, D: A Functional Subset of Dyalog APL, Vector, Volume 17, Number 4, 2001-04. [http://archive.vector.org.uk/art10007770](../art10007770/)

Written in honor of Ken Iverson’s 93rd birthday.
