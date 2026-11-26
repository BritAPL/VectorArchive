---
vid: '10500740'
title: Co-operators
authors:
- Phil Last
byline: by Phil Last (phil.last@ntlworld.com)
volume: '25'
issue: '3'
page: '51'
received: '2011-01-14'
online: '2012-01-01'
editor: Stephen Taylor
description: Phil Last develops some demonstrative operators to solve some common programming problems in Dyalog APL.
abstract: Based on a presentation given at Dyalog’11 in Boston on 5 Oct 2011, Phil Last develops some demonstrative operators to solve some common programming problems.
source: content/printed/253/cooperators.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- programming techniques
- language design
---

by Phil Last (phil.last@ntlworld.com)
{ .byline }

![](featuredImg_co-operators.jpg){ .lead-image }

Based on a presentation given at Dyalog’11 in Boston on 5 Oct 2011, Phil Last develops some demonstrative operators to solve some common programming problems.
{ .abstract }

## Preamble

I’ve been developing user-defined operators since a time when it was only possible to simulate them by executing prepared expressions that include a function name passed as left argument to another function. The following allowed first-generation APLs to use reduction with any dyad. (See the Appendix for definitions of the functions `f`, etc. used to explicate the parsing.)

```
    ∇ R←F RED W
[1] ⍝ R←F⌿W
[2]  R←''⍴⍴W
[3]  R←⍎(⍴,F)↓,((R,⍴,F)⍴F),' ','W','[',(⍕(⍳R)∘.+,0),((R,-1-⍴⍴W)⍴';'),']'
    ∇
      'f' RED ⍳6
( 0 f( 1 f( 2 f( 3 f( 4 f 5 )))))
      '∊' RED 2 5⍴'aeiou','vowel'
0 1 0 1 0
```

So when IBM announced a mechanism for users to define their own operators in about 1983 I had a queue of them awaiting proper implementation.

When Dyalog announced D-fns with lexical scope in 1997 I unashamedly switched to using that as my standard notation for all new functions and operators and embedding D-fns for most amendments to old ones. So without apology I’ll warn you that almost everything you’ll see here is in D-notation. Also that some coding techniques might be new to you.

In the original paper to the Boston presentation I included a short note about those techniques. This time I refer you to a companion article that appears in the same edition of *Vector* as this: “A way to write functions”. If you see anything here that is confusing or appears to make no sense at all you’ll probably find it explained there.

Application-specific operators tend to be rare, often restricted to two or three in a large application while I currently have a collection of over forty more-or-less general purpose ones. It’s always mystified me that so few APLers seem to write or even use them at all.

A simple piece of code that maps or operates on one or two input arrays of known domain and structure can be extracted and encapsulated into a function that can be called upon whenever needed and this perhaps should almost always be done. An extension of that where different but syntactically congruent operations were to be applied would encourage us to write an operator and pass the operation in as a parameter along with the arrays.

I should mention here that, strictly speaking, when we write an operator what we are writing is the definition of the derived function of that operator. When we say an operator returns a function, the operator actually does nothing of the sort. The parser merely binds it with its one or two operands. All the work specific to that operator is still to be done by the derived function and that after it has one or two arguments to operate on. In what follows I’ll almost certainly forget this distinction between the derived function and the operator and refer to the operator’s doing things which are actually the province of the derivation. I doubt that this will confuse.

## Co-operators

A class of operator that I’ve become interested in comprises those that are designed to be called multiple times within the same expression:

```
      [a] f op g op h op j op k op l w
```

where `a` and `w` are arrays; `f, g, h, j, k` and `l` are functions (see Appendix); and `op` is the operator in question. The first and most obvious thing to notice about this is that the operator is dyadic – it takes two operands – making the number of operands in the expression one more than the number of calls to the operator. I’ll mention here that I’ll use Iverson’s names for monadic and dyadic operators – *adverb* and *conjunction* – if I need to distinguish them. Although without an awful lot of messy code an operator can’t examine its operands – it just runs them on trust – in this context we need to know something about them. I’m calling these *co-operators* because in order to be useful they must take account of the likelihood that they are not operating alone.

An examination of the calling sequence of the above expression will help. Redundant parentheses show how it’s parsed:

```
      [a]     f op g op h op j op k op l w
      [a](((((f op g)op h)op j)op k)op l)w
```

In first-generation APL this was explained as operators’ having ‘long left scope’ as opposed to functions’ having ‘long right scope’. Along with strand-notation and the generalisation of operators came the idea of ‘binding strength’. The above parsing is now explained on the basis of right-operand binding’s being stronger than left – so an operand between two conjunctions is bound by the one whose right operand it is; that to its left.

In the case above `g` is bound by `op` to its left, `h` by that to its left and so on to the rightmost `op` that has the whole function expression to its left `f...k` as its left operand – ‘long left scope’. A corollary to this is that all but the leftmost call to `op` have left operands derived from `op` itself, that exception being the greatest complication in all that follows; we need to identify what is that ‘leftmost call’ because we’ll have to treat it differently. We also can be sure that the right operand of every instance of `op` is one of the original operands of the unparenthesised expression. The arguments to the expression become the arguments to the first call, the rightmost.

## Function selection

Here’s a simple operator to start us off. I never got into writing case statements like:

```
      :Select whatever
      :Case this ⋄ ...
      :Case that ⋄ ...
      ...
```

because I’d already started using operators to control program flow before control structures were implemented in APL and D-fns don’t permit them even if I wanted them. Instead consider an operator – call it `or` – to be used as:

```
      0 0 1 0 f or g or h or j w ←→ 0 0 1 0(((f or g)or h)or j)w
```

I hope you can guess what this is intended to do. In the specific case above I want the result to be the result of `h w`, the boolean vector being used as a mask to select from the four operand functions which to apply to argument `w`.

We could imagine this as being implemented something like:

```
      (0 0 1 0 / (f g h j))w
```

where a boolean compression is applied to an isolated list of functions but such a list is not recognised by APL as something it can deal with in any way let alone being able to apply compression to it. More about this later.

I’ll allow only the first, leftmost, 1 to select the function, effectively treating the boolean as if it’s its own less-than scan `<\`. If the mask is all zeros we shouldn’t actually want to run any function at all; we should merely return the argument. So here are the first two lines:

```
      or←{
            ~∨/⍺:⍵
```

Looking at the parenthesised version above we can see that the rightmost `or` has:

```
      ⍺⍺ ←→ f or g or h
      ⍵⍵ ←→ j
```

So if only the final element of `⍺` is a 1 we can run `j`:

```
          </⍺:⍵⍵ ⍵
```

If this isn’t the case then we need to do something with `⍺⍺`. We need to call it without the final element of `⍺` that applied to `j`. So:

```
      or←{
            ~∨/⍺:⍵
            </⍺:⍵⍵ ⍵
            (¯1↓⍺)⍺⍺ ⍵
      }
      0 0 0 0 f or g or h or j 23
23
      0 0 0 1 f or g or h or j 23
(j 23 )
      0 0 1 1 f or g or h or j 23
(h 23 )
      0 1 0 0 f or g or h or j 23
(g 23 )
      1 0 0 1 f or g or h or j 23
( 1 f 23 )
```

These all look right except for `f`. I mentioned the complication of the leftmost call in the eponymous section above. In this we’ve called `f` as dyad `⍺⍺` in the final line of `or`. Another statement is needed to call `⍺⍺` when it’s the leftmost of the supplied operands. We know that by then our expression is `f or g` so `⍺` must have a length of 2. If it were 0 0 we should have returned `⍵` unchanged on line:

```
      [1] ~∨/⍺:⍵
```

If it were 0 1 we should have run `g` on line:

```
      [2] </⍺:⍵⍵ ⍵
```

So it must be 1 0 or 1 1 and in either case we should run `f`:

```
      [2.1] 2=⍴⍺:⍺⍺ ⍵
```

So:

```
      or←{
            ~∨/⍺:⍵
            </⍺:⍵⍵ ⍵
            2=⍴⍺:⍺⍺ ⍵
            (¯1↓⍺)⍺⍺ ⍵
      }
      1 0 0 0 f or g or h or j 23
(f 23 )
```

We can make an alternative version of this that, rather than selecting the function indicated by only the first 1, will select all corresponding functions:

```
      0 1 0 1 f or g or h or j 23
(g(j 23 ))
```

We need to look at the last item of `⍺` whether it’s the only one or not and use the power operator to run `⍺⍺` and `⍵⍵` conditionally:

```
      or←{
            ~∨/⍺:⍵
            g←⍵⍵⍣(⊢/⍺)
            2=⍴⍺:⍺⍺⍣(⊣/⍺)g ⍵
            (¯1↓⍺)⍺⍺ g ⍵
      }
      1 0 1 0 1 1 f or g or h or j or k or l 45
(f(h(k(l 45 ))))
```

## Function sequence

If we look at the reduction operator we see:

```
      f/12 23 34 45
 ( 12 f( 23 f( 34 f 45 )))
```

the function is inserted between the items of the argument. If the function could be a list then perhaps we could do:

```
      (f g h j)/12 34 56 78 90
 ( 12 f( 34 g( 56 h( 78 j 90 ))))
```

Again we can’t do this because `f g h j` is illegal in APL. J implements this concept with the `tie` conjunction `` ` `` to bind the functions in a list of ‘gerunds’ that it applies with `/`:

```
   +`-`*`% / 1 2 3 4 5
0.6
```

But we can interpose a defined operator both to bind the functions and insert them:

```
      f seq g seq h seq j 12 34 56 78 90
            ←→ ((f seq g)seq h)seq j 12 34 56 78 90
            ←→ 12 f 34 g 56 h 78 j 90
```

Notice we have one more item in the argument than there are operands in the expression and one more of them than there are calls to the operator. Also that the operands are dyads while the derived function is a monad. The first (rightmost) call will have:

```
      ⍺⍺  ←→  f seq g seq h
      ⍵⍵  ←→  j
      ⍵   ←→  entire right argument
```

so we can run `j` between the last two items using reduction and catenate its result to the rest running `⍺⍺` on that result:

```
      seq←{
            ⍺⍺(¯2↓⍵),⍵⍵/¯2↑⍵
      }
      f seq g seq h seq j 12 34 56 78 90
(f 12  ( 34 g( 56 h( 78 j 90 ))) )
```

Visually scanning from the right looks good until we get to `f`. Again it’s the leftmost function that adds the complication. When `f` is the left operand, `f seq g` must be the sub-expression, there must be three items in `⍵` so we do almost the same but we apply `⍺⍺` between rather than to the remaining pair after doing the same with `⍵⍵`:

```
      seq←{
            3=⍴⍵:⍺⍺/(¯2↓⍵),⍵⍵/¯2↑⍵
                 ⍺⍺ (¯2↓⍵),⍵⍵/¯2↑⍵
      }
      f seq g seq h seq j 12 34 56 78 90
 ( 12 f( 34 g( 56 h( 78 j 90 ))))
      + seq - seq × seq ÷ 1 2 3 4 5
0.6
```

and get our expected result.

## Function arrays

Another meaning that could be attributed to an isolated list of functions was in fact my first accidental encounter with them after about three months of APL. I had to solve the simple problem of the dimensions of a matrix formed from three others; one above the juxtaposition of the other two:

```
.----------------------------.
|              A             |
|-------------.---------------------.
|             |         C           |
|      B      |                     |
|             |---------------------'
'-------------'
```

APL seemed so good at doing whatever I hoped it would that what I wrote was:

```
      (⍴A)(+⌈)(⍴B)(⌈+)(⍴C)
```

expecting the pairs of functions to be distributed between the pairs of dimensions of the three matrices:

```
           .----.-------.----.-------.
           |    |       |    |       |
      (⍴A)[0 1](+⌈)(⍴B)[0 1](⌈+)(⍴C)[0 1]
             |   |        |   |        |
             '---'--------'---'--------'
```

Of course it didn’t work, for the same reason that the other constructs above didn’t work. Although in this case there are only two functions in each list we can extrapolate this to a list of functions corresponding to a vector of any length. If we define conjunction `fv` (function vector) we’ll want:

```
      a b c d (f fv g fv h fv j) w x y z       ⍝ [0]
      a b c d (f fv g fv h fv j) ⊂w            ⍝ [1]
      (⊂a)    (f fv g fv h fv j) w x y x       ⍝ [2]
              (f fv g fv h fv j) w x y z       ⍝ [3]
```

Our perennial problem of identifying the leftmost operand will depend on the length of the argument(s) as they must conform to the number of functions. We can check for a monad and for the two-item final call. But in cases \[1\] and \[2\] above `⍺` or `⍵` is a scalar so before the length check we’ll resolve scalar extension with laminate and split creating `A` as 0 if `⍺` isn’t supplied. If that’s the case we won’t use it anyway but it’s easier to do it redundantly than to skip it:

```
      fv←{
            ⍺←⊢
            m←1≡⍺ 1
            (A W)←↓(⍺⊣0),[-0.1]⍵
            t←2=⍴W
```

For the two-item monad we want the two functions `⍺⍺` and `⍵⍵` applied to the two items of `W`:

```
            t∧m:(⍺⍺ 0⊃W)(⍵⍵ 1⊃W)
```

If it’s a monad and this doesn’t run it must have more than two items. We apply `⍵⍵` to the last item of `W` and `⍺⍺` to the rest:

```
            m:(⍺⍺ ¯1↓W),⍵⍵¨⊢/W
```

If it’s still two items it must be a dyad so we do similar to the `t∧m:` case but using `A` also:

```
            t:((0⊃A)⍺⍺ 0⊃W)((1⊃A)⍵⍵ 1⊃W)
```

We must be left with the dyad with more than two items so we do similar to the `m:` case but with `A` also:

```
            ((¯1↓A)⍺⍺ ¯1↓W),(⊢/A)⍵⍵¨⊢/W
      }
```

Putting this all together:

```
      fv←{
            ⍺←⊢
            m←1≡⍺ 1
            (A W)←↓(⍺⊣0),[-0.1]⍵
            t←2=⍴W
            t∧m:(⍺⍺ 0⊃W)(⍵⍵ 1⊃W)
            m:(⍺⍺ ¯1↓W),⍵⍵¨⊢/W
            t:((0⊃A)⍺⍺ 0⊃W)((1⊃A)⍵⍵ 1⊃W)
            ((¯1↓A)⍺⍺ ¯1↓W),(⊢/A)⍵⍵¨⊢/W
      }
```

```
      1 2 3 4 f fv g fv h fv j 5 6 7 8
 ( 1 f 5 )  ( 2 g 6 )  ( 3 h 7 )  ( 4 j 8 )
      1 2 3 4 f fv g fv h fv j 5
 ( 1 f 5 )  ( 2 g 5 )  ( 3 h 5 )  ( 4 j 5 )
      1       f fv g fv h fv j 5 6 7 8
 ( 1 f 5 )  ( 1 g 6 )  ( 1 h 7 )  ( 1 j 8 )
              f fv g fv h fv j 5 6 7 8
 (f 5 )  (g 6 )  (h 7 )  (j 8 )
      1 2 3 4 + fv - fv × fv ÷ 5 6 7 8
6 ¯4 21 0.5
```

## Conditionals

```
      :If f w
      :AndIf g w
          r←h w
      :Else
          r←j w
      :End
```

As I mentioned above I really can’t do with having to do stuff like that. It was always easy to write operators that did:

```
      f then g w              ⍝ if f w then r←g w else r←w
      t g else h w            ⍝ if t then r←g w else r←h w
                              ⍝ see *else* below
```

But putting them together to form something like:

```
      f then g else h w       ⍝ if f w then r←g w else r←h w
```

was never going to be so easy. The two conjunctions `then` and `else` need to co-operate, `f then g` being an operand of `else`, and introducing interdependencies should be avoided whenever possible. So how about a single co-operator that fulfils both tasks? The biggest problem is what to call it. I believe at least one language uses `?` for this – not a bad choice:

```
      antecedent ? consequent ? alternative arg
```

I’ll use cond which I also believe is used elsewhere so that in:

```
      f cond g cond h w  ←→  ((f cond g)cond h) w
```

we apply `f w` first then `g w` if `f w` proves true or `h w` otherwise. The length of the argument isn’t going to help as it could be anything at all. One thing we do know is that there’s no left argument. It wouldn’t make any sense if there was. The right call has:

```
      ⍺⍺  ←→  (f cond g)
      ⍵⍵  ←→  h
```

We can’t do anything with `h` yet as that’s the alternative we want to run if `f` returns false so we need to run `⍺⍺` and we need to do it in such a way that it runs one or other of `f` and `g`. We can use that missing left argument as an internal flag to tell the left call what to do. Arbitrarily we’ll give it a 1 to tell it to run `f` and a 0 for `g` if `f` returns true:

```
      cond←{
            ⍺←⊢
            1 ⍺⍺ ⍵:0 ⍺⍺ ⍵
```

but this is no good because as soon as we call it it’ll just call itself again unconditionally. We actually need to test `⍺` for being a 1 or 0 before we use it! Please see the companion article if lines \[2\] and \[3\] below confuse:

```
      cond←{
            ⍺←⊢
            1=⍺⊣0:⍺⍺ ⍵     ⍝ if ⍺ is 1 run f ⍵
            0=⍺⊣1:⍺⍺ ⍵     ⍝ if ⍺ is 0 run g ⍵
```

If `⍺` is ever going to be 1 or 0 we need to make it so, so now we run the line higher up with the two calls to `⍺⍺`. If `1 ⍺⍺ ⍵` which calls `f ⍵` is true we run `0 ⍺⍺ ⍵` that calls `g ⍵`

```
        1 ⍺⍺ ⍵:0 ⍺⍺ ⍵
```

but if `f w` isn’t true we run `h ⍵` instead.

```
            ⍵⍵ ⍵
      }

      cond←{
            ⍺←⊢
            1=⍺⊣0:⍺⍺ ⍵     ⍝ if ⍺ is 1 run f ⍵
            0=⍺⊣1:⍵⍵ ⍵     ⍝ if ⍺ is 0 run g ⍵
            1 ⍺⍺ ⍵:0 ⍺⍺ ⍵  ⍝ if f ⍵ then g ⍵
            ⍵⍵ ⍵           ⍝ else h ⍵
      }

      >∘0 cond j cond k 23
(j 23 )
      >∘0 cond j cond k ¯45
(k ¯45 )
```

*else*: now if we look at the `else` expression that we saw a couple of pages back; I’ve copied it here for convenience:

```
      t g else h w            ⍝ if t then r←g w else r←h w
```

and just out of interest we try it with `cond` we find:

```
      1 g cond h 67
(g 67 )
      0 g cond h 67
(h 67 )
```

that cond is indeed a working else and we can see why on those lines \[2\] and \[3\] above. This isn’t the way we should have written `else` because lines \[4\] and \[5\] won’t run in either case but it’s quite nice that the co-operator also works as a stand-alone:

```
      f cond g cond h w       ⍝ if f w then r←g w else r←h w
           t g cond h w       ⍝ if t then r←g w else r←h w
```

It turns out that that arbitrary 1 and 0 were not so arbitrary after all.

And if we want the usual semantics we can define:

```
      then←{⍺←⊢ ⋄ ⍺ ⍺⍺ cond ⍵⍵ ⍵}
      else←{⍺←⊢ ⋄ ⍺ ⍺⍺ cond ⍵⍵ ⍵}
```

## More conditionals

I started the previous section with an `:If` construct that included `:AndIf`. It turns out that conjunctions `and` and `or` are very easy to write. They aren’t co-operators. They just happen to work together:

```
      and←{⍺⍺ ⍵:⍵⍵ ⍵ ⋄ 0}     ⍝ if ⍺⍺ ⍵ then r←⍵⍵ ⍵ else r←0
      or←{⍺⍺ ⍵:1 ⋄ ⍵⍵ ⍵}      ⍝ if ⍺⍺ ⍵ then r←1 else r←⍵⍵ ⍵
      f and g or h ←→ (f and g)or h
```

This is *not* the same as `(f ⍵)∧(g ⍵)∨(h ⍵)` that would run all functions, `f, g` and `h` and determine the result in the usual APL right to left mode. Operands here are run in left to right order and then only if they are capable of changing the result exactly as a series of `:AndIf` or `:OrIf` clauses in an `:If` clause.

We can now construct the statement at the very top of the “Conditionals” section:

```
      f and g then h else j w ←→
            (((f and g)then h)else j)w
            ⍝ if f w and g w then r←h w else r←j w
```

but unlike the exclusivity of `:AndIf` and `:OrIf` control clauses and remembering that those operands that run will do so in a strictly left to right order we can write:

```
      f or g and h and j then k else l w ←→
            (((((f or g)and h)and j)then k)else l)w
```

## Forks

J refers to lists of functions as *trains* and parses them in a very particular way:

A train of 2 functions (2-train) is called a *hook*, which counts as a function in its own right; an APL equivalent would be:

```
      (f g)       ←→ f∘g⍨⍨
```

so that:

```
      a(f g)w     ←→ a f(g w)
      (f g)w      ←→ w f(g w)
```

A 3-train constitutes a *fork* (another function) where:

```
      a(f g h)w   ←→ (a f w)g(a h w)
       (f g h)w   ←→ (  f w)g(  h w)
```

Beyond the 3-train a fork peels off from the right and becomes the right tine of the next construct, hook or fork, recursively:

```
      (g f h j)         ←→ (g(f h j))
      (g f h j k)       ←→ (g f(h j k))
      (g f h j k l)     ←→ (f(g h(j k l)))
```

Even-trains above 2 that simulate a hook whose right tine is a fork aren’t possible with a single defined operator but we can simulate an odd-train with a fork operator `fk`. The only difference will be that due to operand binding as mentioned above forks will be bound from the left:

```
      f fk g fk h fk j fk k   ←→ (f fk g fk h)fk j fk k
```

We’ll only consider a ‘simple’ fork of three functions here:

```
      a(f fk g fk h)w   (a f w)g(a h w)
       (f fk g fk h)w   (  f w)g(  h w)
```

Again at the first call we have:

```
      ⍺⍺ ←→ f fk g
      ⍵⍵ ←→ h
```

so can do something with `h`, but we need to pass its result along with the original argument(s) to the next call where `f` and `g` will be available:

```
      fk←{
            ⍺⍺(⍺ ⍵)(⍺ ⍵⍵ ⍵)
      }
```

But at that next call we don’t want to do the same thing at all.

```
      ⍺⍺ ←→ f
      ⍵⍵ ←→ g
```

We want to apply `g` between the results of `f` and `h`. This is the same problem as ever –  identifying the subsequent call – but this time we can’t rely on the length of the argument(s) to tell us when we are there. We need to pass in a flag to the second call to tell it – as with `cond` above – and we might as well have two; one for the monad and one for the dyad. In the first monadic call we present the flag as left argument to the next call. We have to hope no-one ever calls our fork with these flags as supplied arguments:

```
      fk←{
            ⍺←⊢
            (m d)←'left monadic fork' 'left dyadic fork'
```

if it’s a monad, `⍺` will be assigned `⊢` and for all `x`

<pre>
      <code>x ≡ ⍺ x</code>
</pre>

so we pass `m, ⍵` and the result of `h` to the left call:

```
            ⍵≡⍺ ⍵:m ⍺⍺(⍵)(⍵⍵ ⍵)
```

If we get past here we know `⍺` exists so if it’s our monadic flag we run `g` between results of `f` applied to the origin `⍵` and that of `h` which we computed above as `(⍵⍵ ⍵)`:

```
      fk←{
            ⍺←⊢
            (m d)←'left monadic fork' 'left dyadic fork'
            ⍵≡⍺ ⍵:m ⍺⍺(⍵)(⍵⍵ ⍵)
            ⍺≡m:(⍺⍺ 0⊃⍵)⍵⍵ 1⊃⍵
      }
      f fk g fk h 23
((f 23 )g(h 23 ))
```

So far so good. The dyad should be very similar. If `⍺` is `d` – the dyadic flag – we run `f` between our original arguments and `g` between that result and `h`:

```
            ⍺≡d:(⊃⍺⍺/0⊃⍵)⍵⍵ 1⊃⍵
```

If not we’d better make it so passing `d`, `⍺ ⍵`, and `⍺ h ⍵` in the right dyad:

```
            d ⍺⍺(⍺ ⍵)(⍺ ⍵⍵ ⍵)
      }

      fk←{
            ⍺←⊢
            (m d)←'left monadic fork' 'left dyadic fork'
            ⍵≡⍺ ⍵:m ⍺⍺(⍵)(⍵⍵ ⍵)
            ⍺≡m:( ⍺⍺ 0⊃⍵)⍵⍵ 1⊃⍵
            ⍺≡d:(⊃⍺⍺/0⊃⍵)⍵⍵ 1⊃⍵
            d ⍺⍺(⍺ ⍵)(⍺ ⍵⍵ ⍵)
      }
```

```
      f fk g fk h 23
((f 23 )g(h 23 ))
      +/fk÷fk⍴ 12 23 34 45 56 67 78
45
      12 f fk g fk h 23
(( 12 f 23 )g( 12 h 23 ))
      12 34 56 78 90 >fk∨fk= 98 76 54 32 10
0 0 1 1 1
```

## Afterword

If it were decided to allow APL to acknowledge an isolated list of functions as a valid syntactic construct it would remain to be decided how it should be applied and how the individual members should interact.

The designers of J decided to go for hook and fork.

There is currently discussion that Dyalog might make the same decision.

It isn’t necessary to go for a single solution except as a default. The selection of hook and fork as default behaviour wouldn’t need to preclude the introduction of operators that apply the list in some other way. The co-operators described here hint at some of those other ways. Instead of building up a derived function by alternating with their operands, they could take the list as a single operand and apply its members appropriately.

## Appendix

Functions used to demonstrate calling sequences:

```
      f←{⍺←⊢ ⋄ '(',⍺,'f',⍵,')'}
      g←{⍺←⊢ ⋄ '(',⍺,'g',⍵,')'}
      h←{⍺←⊢ ⋄ '(',⍺,'h',⍵,')'}
      j←{⍺←⊢ ⋄ '(',⍺,'j',⍵,')'}
      k←{⍺←⊢ ⋄ '(',⍺,'k',⍵,')'}
      l←{⍺←⊢ ⋄ '(',⍺,'l',⍵,')'}
```
