---
vid: '10500750'
title: A way to write functions
authors:
- Phil Last
byline: by Phil Last (phil.last@ntlworld.com)
volume: '25'
issue: '3'
page: '67'
received: '2011-01-14'
online: '2012-01-01'
editor: Stephen Taylor
description: Phil Last tries to explain some of his more unusual or obscure programming techniques in Dyalog APL.
abstract: A companion article to Phil Last’s Co-operators paper to explain some of his more unusual or obscure programming techniques.
source: content/printed/253/functions.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- programming techniques
---

by Phil Last (phil.last@ntlworld.com)
{ .byline }

A companion article to Phil Last’s Co-operators paper to explain some of his more unusual or obscure programming techniques.
{ .abstract }

Someone recently stopped to look at my Dyalog development session. But only for a moment. He made a gesture to indicate his bewilderment and moved on. This is a man who writes most of his code in the direct (formerly known as “dynamic”) functional subset of Dyalog APL (D-fns or D:).

As do I.

So what was the problem?

Having bewildered many more in a presentation at Dyalog’11 at Boston and having been rebuked by a number of them since, I’ve come to the conclusion that it’s because I’ve discovered and use various techniques for doing common tasks in APL that have more verbose and perhaps more familiar counterparts.

As I’ve come to value all of these idioms and now find them highly recognisable I’d come to assume their self-explanatory nature but now accept that they present a relative impenetrability to many. This is hopefully to counter the latter and realise the former. As mentioned above almost all of my code is in D notation about which you can read in John Scholes' paper, “Dynamic Functions in Dyalog APL” [\[1\]](#ref1) so I’ll only mention here the parts whose possible unfamiliarity is the reason for this article.

## Ambivalence

I probably should start where most of my functions do. All D-fns are potentially ambivalent: they can be called with or without a left argument. Whether the code actually refers to that argument is another matter. The simplest D-fn is: `{}` that doesn’t refer to either of its arguments or anything else for that matter. Nevertheless most functions need to do something and if they expect a left argument the avoidance of a value error is something to be considered.

Traditional ways of doing this mostly check the name-class `⎕NC` of the argument name for zero and conditionally assign an array value.

When I started using IBM’s APL2, which was the first time the problem arose for me, an earlier observation made me realise that a more generally useful course would be to attempt to fix a right identity function with the argument name:

```
         ∇ RES←LARG F00 RARG
      [1]  0 0⍴⎕FX,⊂'R←LARG R'
      ...
         ∇
```

If `LARG` is supplied, `⎕FX` fails gracefully returning `⎕IO` because it won’t fix a function in place of an array. But if `LARG` has been elided then `⎕FX` creates it as an identity function and any use of the name within `F00` as a left argument behaves as if it isn’t there. In `[2]` below if `LARG`is an identity function, `F01` has no left argument and its result is passed to `RES`. So for elided `LARG`:

```
      [2] RES←LARG F01 RARG
```

is exactly the same as:

```
      [2] RES←F01 RARG
```

I went so far as to create a utility function whose argument was the left argument-name of the function that called it:

```
         ∇ AMBIVALENT AMBIVALENT
      [1]  AMBIVALENT←⎕FX,⊂'R←',AMBIVALENT,' R'
         ∇
```

It reassigns its eponymous argument with the result of `⎕FX` possibly having created a semi-global identity function with the name contained in that argument.

```
         ∇ R←LEFT F02 RIGHT
      [1]  AMBIVALENT'LEFT'
      [2]  R←LEFT F03 …
      …
         ∇
```

It was published in Vector 6.2 as a tongue-in-cheek response to something much worse in Vector 5.4 but didn’t go down well with the other correspondents. I thought it was nicely declarative and self-documenting.

I have to admit there are some who are surprised by the fact that what they expect to be an array could be a function instead. Given the flexibility of infix notation what surprises me is that most will immediately assume the expression `e t y`, which constitutes three letters randomly chosen from the line above, to represent a three item vector rather than any of the other eleven valid three-fold array or function expressions that can be formed from permutations of arrays, functions or operators.

## Ambivalence in D-fns

The D method to accommodate an elided left argument `⍺` is a single assignment that is executed conditionally on the argument’s absence.

I believe John Scholes was surprised to find that the equivalent of the above:

```
      [1] ⍺←{⍵}
```

actually worked. It has exactly the same effect creating the identity function as `⎕FX` did. I used this until Dyalog Version 13.0 finally introduced the right function `⊢` that implements the right identity as a primitive. Thus many of my functions now start with the even more terse but equally useful and recognisable:

```
      [1] ⍺←⊢
```

In twenty-five years I’ve found no situation where this technique fails to work and none where any other excels it.

## Assigning a default

Some might object here that it’s easier to assign a known value to the argument as default.

```
      [2] ⍺←default
```

This can still be done after `[1]` above if we are content to use another name:

```
      [2] larg←⍺⊣default
```

This uses the other new identity function of Dyalog 13.0, left `⊣` that returns its left argument if it has one or its right otherwise – following the J implementation of left `[` that I believe was Ken Iverson’s later preference, rather than that of SAPL and APLX that don’t return a result from the monad. Thus in our D-fn if `⍺` is supplied then `⊣` in the above expression gets its value as left argument and returns it to be assigned as `larg`; if not then `⍺` is `⊢` from `[1]`; `⊣` is called as a monad and returns its right argument default instead, to be passed by `⊢` and assigned to `larg`.

Very often though, assigning a default to be passed to another function is not the best thing to do. If a called function `f04` is designed to be ambivalent we can assume it must be able to take care of its own defaults. Were an ambivalent caller `f03` to provide another default it would override whatever was the intention of the author of `f04`. This must be decided case by case.

All this becomes much more important in the case of operators. If a derived function, an operator with its operand(s), is called without a left argument it very reasonably can be inferred that, all else being equal, the function operand is also expected to be called as a monad. Each `¨` is a primitive case in point.

## More ambivalence

We usually apply the word *ambivalent* to functions permitting the optional elision of left argument `⍺`: argument ambivalence. The dichotomy above where `⍺` can be an array or a function (supplied or assigned) also occurs in operators' accepting either an array or a function as operand `⍺⍺` and/or `⍵⍵`. Hence: operand ambivalence. The left operand of the `/` operator of APL2 and the right of the power `⍣` operator of Dyalog both have this property.

In:

```
      [4] ... ⍺⍺⊣⍵
```

the use of left `⊣` allows us to supply left operand `⍺⍺` as either a function or an array without our having to code separate cases for either of them. If `⍺⍺` is an array then `⍺⍺⊣⍵` is just `⍺⍺`, the left argument of `⊣`. If it’s a function then it’s applied to the result of monadic `⊣` which is `⍵`.

## Differential processing

Perhaps unusually, different processing is sometimes required for the monadic case of a function than the dyadic. Unusually in defined functions, that is; we should be surprised if primitives didn’t exhibit this behaviour. In a D-fn this would probably entail the using a guard. What should be the condition?

In:

```
      [2] ⍺←⊢
      [3] 1≡⍺ 1: expression
```

`1≡⍺ 1` returns true and invokes `expression` if the value 1 matches the value of `⍺ 1`. This might seem unlikely. How can a scalar 1 match a two item vector? Obviously it can’t but if `⍺` is `⊢` then `⍺ 1` is `⊢ 1` which is just 1. In fact this works equally well for any array replacing the 1 on both sides of the equality so, for instance, to make it more explicit:

```
      [3] 'monad'≡⍺'monad': expression
```

Scripting languages from very early on have permitted similar constructs:

```
      If . = . %1 Then expression-for-missing-%1
```

which behaved identically after parameter substitution for optional `%1` though the explanation would be somewhat different.

## Conditions and multi-line expressions

It’s possible to break any D-fn inside its enclosing left or right brace. The next two D-fns are equivalent:

```
      {single expression}

      {
            single expression
      }
```

Not only this but long lines embedding anonymous D-fns can also be broken:

```
      {this long line{embeds another D-fn}within it}

      {this long line{
            embeds another D-fn
      }within it}
```

Guards sometimes present a problem in that a quite long line is required for the terminating expression to run when the condition is true. By embedding the expression in an anonymous D-fn we can extend the statement onto more than one line:

```
      conditional expression: ⍺{
            long terminating expression to be triggered when true
      }⍵
```

Sometimes we want to run an expression conditionally or perhaps one expression or another to extract the value within, rather than terminating, the current function. In this case embedding the entire guard with its conditional, consequent and alternative expressions in an anonymous D-fn serves very well:

```
      value←⍺{condition: expression if true
            expression if false
      }⍵
```

In both these cases it’s worth supplying the original arguments so that all names and tokens inside the inner D-fn reflect those outside.

## Commented one-liners

The following doesn’t define a D-fn:

```
      f03←{expression ⍝ comment}
SYNTAX ERROR
      f03←{expression ⍝ comment}
                                ∧
```

The reason is that the final brace is a part of the comment and doesn’t actually close the D-fn. This has unfortunately and unjustifiably caused some to claim that D promotes uncommented code. The ability to create functions – D-fns or otherwise – without comments; believing them to be unnecessary; or that one doesn’t have time to write them promotes uncommented code.

Fortunately the left `⊣` function comes to the rescue yet again. We can use:

```
      f03←{expression⊣'comment'}
```

or the prettier:

```
      f03←{expression⊣'⍝ comment'}
```

or even:

```
      nb←⊣
      f03←{expression nb' comment'}
```

## Naming operands

This is something I’ve relearned to do only very recently having gone perhaps too far down the road of designing algorithms that required no assignments whatsoever.

It’s always useful and very often necessary or desirable to be able to define anonymous D-fns embedded within the coding of a D-fn or D-op. This entails another level of parameter substitution so for instance, operator `where`, that needn’t be understood, contains two levels of embedded operations:

```
      where←{
            (⍴⍵)⍴⍺⍺{⍵ ⍺⍺{⍵⊖⍺,[-0.1]⍵\⍺⍺ ⍵/⍺},⍵⍵⊣⍵}⍵⍵,⍵
      }
                   ↑                             ↑
                        ↑                  ↑
```

In order that the operands `⍺⍺`and `⍵⍵` continue to refer to the same thing they must be passed in as operands at each level making the inner operations operators also. It is also quite difficult to see that the innermost operation is a monadic operator deriving a dyadic function while the next out is a dyadic operator deriving a monadic function as is the outer, named, D-op. The following version appears much simpler merely by naming the operands:

```
      where←{f←⍺⍺ ⋄ g←⍵⍵
            (⍴⍵)⍴{⍵{⍵⊖⍺,[-0.1]⍵\f ⍵/⍺},g⊣⍵},⍵
      }
```

Lexical scope rules mean that named items are visible to any D-fn or D-op defined within the one wherein they are named. Thus they don’t have to be respecified for the inner levels so we actually reduce the number of tokens in the line as well as avoiding the added complication of the embedded operations’ being promoted to D-ops.

## Separating operands and arguments

Operator operands can be either functions or arrays. The power `⍣` operator takes either a terminating function or an iteration count as its right operand. In the latter case we have an array (scalar integer) operand and an array right argument. This brings up the question of the relative strengths of operand- versus vector-binding. Implementors chose differently in this regard, vector binding being the strongest of all in Dyalog APL and NARS2000, while both bracket- and right-operand binding exceed it in APL2 and APLX.

Consequently we have the following dichotomy. The expression:

```
      f dop a b c
```

where `f` is a function, `dop` is a dyadic operator and `a, b` and `c` are arrays, is parsed as follows:

APL2, APLX &c.:

```
      f dop a b c ←→ (f dop a)(b c)
```

an array expression; function with argument; `a` is the operand and `b c` is the argument.

Dyalog, NARS2000 &c.:

```
      f dop a b c ←→ f dop(a b c)
```

a function expression without an argument; `a b c` is the right operand.

This difference adds an extra complication to Dyalog which is well worth the cost considering the alternative:

APL2:

```
      f dop 1 2 3 ←→ (f dop 1) 2 3
```

what appears to be a numeric vector is treated as a scalar separated from the remainder.

So in Dyalog we need to separate the array right operand from the argument. The surest way to do it is to parenthesise the entire function expression:

```
      (f dop operand)argument
```

But a neater way is to insert an identity function between the operand and argument. Right-operand binding is stronger than left-argument so the identity is parsed as monadic:

```
      f dop operand+argument
      f dop operand,vector
```

Until recently plus `+` as above was common as a separator because its monadic definition was to return the right argument unchanged. But Dyalog 13.0 has complex numbers and monadic plus is now redefined as conjugate for that domain:

```
      + 12J34 'zxc'  →  12J¯34 'zxc'
```

so it is no longer a reliable identity in all cases. Fortunately and in part consequently Dyalog 13.0 also includes the two new identities mentioned above: `⊣` and`⊢`; either of which suffices for our purpose:

```
      f op operand⊢argument
      f op operand⊣argument
```

## Boolean equivalents

We learn that there are ten scalar dyadic boolean functions in APL: `< ≤ = ≥ > ≠ ∨ ∧ ⍱ ⍲`. Each one maps each of the four pairs (0 0)(0 1)(1 0)(1 1) onto one of the pair 0 1 in a different way. So there should be exactly sixteen such functions; there are six others missing from APL. And given that there is that mapping it must be possible to enumerate them in a standard way.

Applying `r ← ,0 1 ∘.f 0 1` gives us the four results. Applying `2⊥r` gives us a number between 0 and 15. Here they are put into the order dictated by that result:

```
   .---.--------------.---------.----.
   | ∧ | ,0 1 ∘.∧ 0 1 | 0 0 0 1 |  1 |
   | > | ,0 1 ∘.> 0 1 | 0 0 1 0 |  2 |
   | < | ,0 1 ∘.< 0 1 | 0 1 0 0 |  4 |
   | ≠ | ,0 1 ∘.≠ 0 1 | 0 1 1 0 |  6 |
   | ∨ | ,0 1 ∘.∨ 0 1 | 0 1 1 1 |  7 |
   | ⍱ | ,0 1 ∘.⍱ 0 1 | 1 0 0 0 |  8 |
   | = | ,0 1 ∘.= 0 1 | 1 0 0 1 |  9 |
   | ≥ | ,0 1 ∘.≥ 0 1 | 1 0 1 1 | 11 |
   | ≤ | ,0 1 ∘.≤ 0 1 | 1 1 0 1 | 13 |
   | ⍲ | ,0 1 ∘.⍲ 0 1 | 1 1 1 0 | 14 |
   '---'--------------'---------'----'
```

Alternatively we can define the operator or adverb:

```
      fnum←{2⊥,0 1 ∘.⍺⍺ 0 1}
      ∧fnum 0
1
      ∨fnum 0
7
      ...
```

The six missing would be scalar pervasive equivalents of: 0:`{0}`; 3: `⊣`; 5: `⊢`; 10: `{~⍵}`; 12: `{~⍺}`; and 15: `{1}`.

Given so many boolean dyads it seems odd that much of the time only two of them get used. We’ve all seen such as:

```
      :If p∨~q ⋄ ...
      :If (~p)∧q ⋄ ...
```

Applying fnum to each of these:

```
      {⍺∨~⍵}fnum''
11
      {(~⍺)∧⍵}fnum''
4
```

we find that `{⍺∨~⍵}` is function 11, `≥` while `{(~⍺)∧⍵}` is function 4, `<` so why don’t we see:

```
      :If p≥q ⋄ ...
      :If p<q ⋄ ...
```

for boolean `p` and `q`? I’ve been told that they’re unfamiliar in this context. But so was most of APL to each one of us when we started. Nevertheless many such equivalences exist. And the reason we don’t automatically associate them with the logical domain is because of their common names.

Most people refer to `≥` as *greater than or equal* and some as *not less than*; just two obvious ways to say the same thing. Very few if any call it *or not* or *implied by* but if the comparands are propositions or logicals with boolean values 1 or 0 then it makes complete sense.

Below are some of the more common boolean equivalents many of which I’ve seen in use. This is not just a matter of terseness versus verbosity. If `p` and `q` are large then applying two, three or even four functions to them rather than one is significant.

```
   .-------------.--------------.----.---.-----.
   | expression  | D-fn         |fnum| f | exp |
   |-------------+--------------+----+---+-----|
   | ~p∧q        | {~⍺∧⍵}       | 14 | ⍲ | p⍲q |
   | ~p∨q        | {~⍺∨⍵}       | 8  | ⍱ | p⍱q |
   | p∧~q        | {⍺∧~⍵}       | 2  | > | p>q |
   | p∨~q        | {⍺∨~⍵}       | 11 | ≥ | p≥q |
   | ~p∧~q       | {~⍺∧~⍵}      | 13 | ≤ | p≤q |
   | ~p∨~q       | {~⍺∨~⍵}      | 4  | < | p<q |
   | (~p)∧q      | {(~⍺)∧⍵}     | 4  | < | p<q |
   | (~p)∨q      | {(~⍺)∨⍵}     | 13 | ≤ | p≤q |
   | (~p)∧~q     | {(~⍺)∧~⍵}    | 8  | ⍱ | p⍱q |
   | (~p)∨~q     | {(~⍺)∨~⍵}    | 14 | ⍲ | p⍲q |
   | ~(~p)∧~q    | {~(~⍺)∧~⍵}   | 7  | ∨ | p∨q |
   | ~(~p)∨~q    | {~(~⍺)∨~⍵}   | 1  | ∧ | p∧q |
   | (p∨q)∧~p∧q  | {(⍺∨⍵)∧~⍺∧⍵} | 6  | ≠ | p≠q |
   | (p∧q)∨~p∨q  | {(⍺∧⍵)∨~⍺∨⍵} | 9  | = | p=q |
   '-------------'--------------'----'---'-----'
```

## Iteration

It’s often necessary to be able to repeat a function sequentially with one argument different but each time using the result of a previous iteration as the other argument. Say we have prototype `p` and vector `v` of items to be applied in order `a b c d e`. Depending which way round our function requires its arguments this will be:

```
      ((((p flr a)flr b)flr c)flr d)flr e
```

or:

```
      e frl(d frl(c frl(b frl(a frl p))))
```

where: `frl ←→ flr⍨`

Given the likely variable length of our vector most will code this as:

```
      :For i :In v
            p←p flr i   ⍝ or: p←i frl p
      :End
```

but look again at the expression with repeated `frl`. Not everyone will recognise it immediately but this is a precise definition of reduction:

```
      ⊃frl/e d c b a p
```

The right argument of the reduction is merely `v` reversed and with `p` stuck on the end:

```
      ⊃frl/(⌽v),⊂p
```

## Applying a monad to one of a pair

Given a 2-vector it’s required occasionally to keep the first item intact while running a function between the two preferably without having to prise the two apart beforehand. The description might not immediately suggest scan but that’s exactly what it requires:

```
      f\x y ←→ x(x f y)
      f\two ←→ (0⌷two),f/two
```

At least as often I find that what I really want to do is to keep the first while applying a monad to the second. All D-fns are ambivalent. But in this case I want the monadic definition of the function to apply. If the function ignores its left argument then the above is all we need:

```
      {2×⍵}\two ←→ (0⌷two),{2×⍵}/two
                ←→ (0⌷two),{2×⍵}1⌷two ⍝ for a de facto monad
```

But if our function is properly ambivalent and works differently in the two cases this will always use the dyad. We need a way to force an ambivalent function to act as a *de facto* monad. Composing it with right `⊢` as `⊢∘{…}` pushes the left argument out to become that of `⊢` which it will ignore. Our function never sees the argument at all:

```
      minus←{⍺←⊢ ⋄ ⍺-⍵}
      minus 5           ⍝ monad: negate
¯5
      8 minus 5         ⍝ dyad: subtract
3
      8 ⊢∘minus 5       ⍝ de facto monad: negate
¯5
      minus\8 5
8 3
      ⊢∘minus\8 5
8 ¯5
```

## Afterword

We all have our own favourite tricks and tips. The first version of this was a preamble to a presentation I gave at Dyalog’11 in Boston and this lengthier version is designed to accompany a re-presentation of that paper that appears in the same edition of *Vector* as this. I hope it’s been of some use.

## References

1. <a id="ref1"></a> http://dyalog.com/download/dfns.pdf
