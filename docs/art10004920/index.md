---
vid: '10004920'
title: Lucidity Through Infix Notation
authors:
- Phil Last
volume: '22'
issue: '1'
page: '72'
source: trad/v221/nap221.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- programming techniques
- language design
---

## or Destructuring APL

*by Phil Last*

## Abstract

The napkin, not the first to end up in Italy, I believe, was of superb quality, as evidenced by minimal spread of the ink from John’s fibre tipped pen, as was the meal it was served with.

## Introduction

What I wrote on it, or tried to write, my hand-eye coordination not being what it had been earlier, recalls a paper by David Eastwood in 1993 \[[E1](#e1)\] suggesting we could use defined operators to abolish the goto & get around the absence of control structures in APL. Something I'd been doing since APL2 arrived 10 years earlier.

My contention now is that in many situations, using control structures, which were not available in 1993, actually obscures the intent of our programs whereas infix notation using conjunctions is a more lucid way to describe them.

Meaningful sentences don’t start with “If”.

> “Well, it ain’t no use to sit and wonder why, Babe, if you don’t know by now.”\[[D1](#d1)\]

Most people have seen something like this ...

```
   --------------------------
    else←{    | then←{
       ⍺:⍺⍺ ⍵ |    ⍺⍺ ⍵:⍵⍵ ⍵
       ⍵⍵ ⍵   |    ⍵
    }         | }
   --------------------------
```

We can do ...

```
    res←test consequence else alternative arg                 (1)
```

where `res` is the data result, determined by boolean singleton `test`, of either `consequence` or `alternative` both of which are functions with `arg` in their domains,

and ...

```
    res←proposition then consequence arg                      (2)
```

where again both `proposition` & `consequence` have `arg` in their domain, `res` being either `arg` as is or the data result of `consequence` if `proposition’`s boolean result is true.

David Eastwood hoped to combine these two expressions to produce ...

```
    res←proposition then consequence else alternative arg     (3)
```

To quote David ...

> The simplified construction:
>
> ```
> ... test THEN expl ELSE exp2 data
> ```
>
> can be written with user-defined operators THEN and ELSE but presents a problem, The order of execution of the expression is:
>
> ```
> ... test THEN expl
> ```
>
> which will generate the left operand for
>
> ```
> ... ELSE exp2
> ```
>
> It is difficult to come up with a safe, general return code from THEN which can instruct ELSE to evaluate its right operand ...

I suspect that as (proposition then consequence) is the left operand to “else” David was viewing it as atomic. The way operands are passed to operators BEFORE being executed means it is not the business of “then”to instruct “else” to do anything but rather for “else” to call its operand function which is derived from “then”. It can do this or not in any way the coder chooses.

By constructing both operators such that “else” passes an otherwise meaningless left argument to the derived function which becomes “then” ’s left argument, “then”can call either its “proposition”or its “consequence”rather than calling “consequence”conditionally on the result of “proposition”.

And we can construct an “if” with commuted syntax (consequence if proposition) and if it is made to rely on “then” then it will also work under “else” giving us, besides a headache ...

```
    res←consequence if proposition arg                        (4)
    res←consequence if proposition else alternative arg       (5)
```

Here is what else was written on the napkin ...

```
----------------------------------------------------------------
then←{                 | else←{             | if←{
   ⍺←'M'               |    ⍺←'M'           |    ⍺←{⍵}
   ⍺='Q':({⍵}∘⍺⍺)⍵     |    ⍺=1:({⍵}∘⍺⍺)⍵   |    ⍺(⍵⍵ then ⍺⍺)⍵
   ⍺='Y':({⍵}∘⍵⍵)⍵     |    ⍺=0:({⍵}∘⍵⍵)⍵   | }
   ({⍵}∘⍺⍺)⍵:({⍵}∘⍵⍵)⍵ |    'Q'⍺⍺ ⍵:'Y'⍺⍺ ⍵ |
   ⍵                   |    ({⍵}∘⍵⍵)⍵       |
}                      | }                  |
----------------------------------------------------------------
```

I use two techniques here that might not be universally recognised ...

```
    ⍺←{⍵}
```

allows ambivalent expressions to be coded. It is executed conditionally by the D: interpreter only in a monadic use. In a dyadic use, array (<span class="aplu">⍺</span>) to the left of a function indicates another dyad. In a monadic use, identity function (<span class="aplu">⍺</span>) to the left of a function indicates another monad.

I use this here in “if”, though I missed it from the napkin, so that it can pass on its current valence to “then”, being called monadically in the standalone call (4) and dyadically under “else” in (5).

The other technique uses another type of ambivalence ...

```
    ({⍵}∘⍺⍺)⍵
```

is ambivalent in the sense that (<span class="aplu">⍺⍺</span>) can be either an array or a function. If it is a function the <span class="aplu">\{⍵\}∘ </span>forces a monadic call even in the presence of a left argument. If it is an array (variable) it is the curried right argument to the function <span class="aplu">\{⍵\}</span>& is therefore returned unchanged.

This is used above wherever an explicitly monadic call is made to an operand and permits the following variations on the above sentences (1–5), using CAPITALS to indicate arrays ...

```
      RES←TEST consequence else alternative ARG                 (1)
      RES←TEST(consequence else OTHER)ARG                       (1.1)
      RES←TEST(VALUE else alternative)ARG                       (1.2)
      RES←TEST(VALUE else OTHER)ARG                             (1.3) *

      RES←proposition then consequence ARG                      (2)
      RES←(proposition then VALUE)ARG                           (2.1)
      RES←TEST then consequence ARG                             (2.2)
      RES←(TEST then VALUE)ARG                                  (2.3)

      RES←proposition then consequence else alternative ARG     (3)
      RES←(proposition then consequence else OTHER)ARG          (3.1)
      RES←proposition then VALUE else alternative ARG           (3.2)
      RES←(proposition then VALUE else OTHER)ARG                (3.3)
      RES←TEST then consequence else alternative ARG            (3.4)
      RES←(TEST then consequence else OTHER)ARG                 (3.5)
      RES←TEST then VALUE else alternative ARG                  (3.6)
      RES←(TEST then VALUE else OTHER)ARG                       (3.7) *

      RES←consequence if proposition ARG                        (4)
      RES←(consequence if TEST)ARG                              (4.1)
      RES←VALUE if proposition ARG                              (4.2)
      RES←(VALUE if TEST)ARG                                    (4.3)

      RES←consequence if proposition else alternative ARG       (5)
      RES←(consequence if proposition else OTHER)ARG            (5.1)
      RES←consequence if TEST else alternative ARG              (5.2)
      RES←(consequence if TEST else OTHER)ARG                   (5.3)
      RES←VALUE if proposition else alternative ARG             (5.4)
      RES←(VALUE if proposition else OTHER)ARG                  (5.5)
      RES←VALUE if TEST else alternative ARG                    (5.6)
      RES←(VALUE if TEST else OTHER)ARG                         (5.7) *
```

In all the sentences lacking “else”, the standalone expressions in “then” & “if”, it can be correctly inferred that if (TEST) or (proposition ARG) is false then RES is ARG.

In the sentences marked (<span class="aplu">\*</span>) ARG is not referenced and plays no part in the result. It is required only as a syntactic placeholder.

Normal rules of function derivation and grouping with parentheses permit us to add to the complexity without obscuring the sense to produce further constraints as with nested :Ifs or those qualified with :ElseIf and :AndIf but perhaps with greater lucidity. I hesitate to mention :OrIf which I have so far completely failed to circumcapitate ([i](#i)) ....

```
                                                              (6)
      RES←proposition1 then(proposition2 then consequence else alternative2)else alternative1 ARG
```

which works as expected.

The ability to write in-line D:fns and if necessary to break them inside of the braces \{  
thus  
\} makes it possible to control the visual representation of clauses which some may consider an advantage ...

```
    RES←proposition1 then{                                    (6.1)
         proposition2 then consequence else alternative2 ⍵
      }else alternative1 ARG
```

## Asides

Scarily, we can replace the character flags ‘M’, ‘Q’& ‘Y’ in “then” & “else” above with arbitrary numbers 2, 1 & 0 and the operators become nearly indistinguishable though the semantics of the identical lines are quite different.

```
  ------------------------------------------
   then←{                 | else←{
      ⍺←2                 |    ⍺←2
      ⍺=1:({⍵}∘⍺⍺)⍵       |    ⍺=1:({⍵}∘⍺⍺)⍵
      ⍺=0:({⍵}∘⍵⍵)⍵       |    ⍺=0:({⍵}∘⍵⍵)⍵
      ({⍵}∘⍺⍺)⍵:({⍵}∘⍵⍵)⍵ |    1 ⍺⍺ ⍵:0 ⍺⍺ ⍵
      ⍵                   |    ({⍵}∘⍵⍵)⍵
   }                      | }
  ------------------------------------------
```

Oh yes, and in case anyone has noticed that none of this touches on the most ubiquitous use of “:If”, try ...

```
   ∇ res←{larg}f00 rarg;default
     default←42
     larg←(2=⎕NC'larg')then{larg}default
     ...
   ∇
```

## Conclusions

I have shown that it is possible to write defined operators that, along with a functional approach to the code being controlled, offer the possibility of a more “natural language” style of programming in place of the commoner control structures. And that writing them on napkins is the quickest way to get them published.

## Note

<a id="i"></a>(i) circumcapitate - vt - to get one’s head around something

## References:

<a id="e1"></a>\[E1\] David S. Eastwood. “Structuring functions with operators”. *ACM SIGAPL APL Quote Quad* Volume 24, Issue 1 (August 1993)

<a id="d1"></a>\[D1\] Bob Dylan. “Don’t think twice, it’s alright”.<i>The Freewheeling Bob Dylan</i>. Columbia. (1963)
