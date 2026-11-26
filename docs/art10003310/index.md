---
vid: '10003310'
title: Defined Operator Simulation for APL*PLUS II
authors:
- Allan Gay
byline: by Allan Gay (Cocking & Drury)
volume: '10'
issue: '4'
page: '126'
source: trad/v104/gay104_126.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- language design
- development practice
---

by Allan Gay <span class="affil">(Cocking &amp; Drury)</span> <span class="email"></span>
{ .byline }

### Introduction

During a recent migration of mainframe APL2 applications to APL\*PLUS II/386, some defined operators were encountered. Because the target interpreter does not support defined operators, it was necessary to simulate them. This article describes how it was done.

### Materials

The APL\*PLUS II/386 code examples in this article all require Release 4.0 or higher, running in Evolution Level 2 mode.

For illustration purposes, we’ll be using the monadic defined operator woof, which produces an ambi-valent derived function. Here are the APL2 statements:

```
      ∇∆z←∆a(F woof)∆w;∆A;∆W
[1]   ⍝ Compute sum of ⍺ and ⍵, and then F these sums together.
[2]
[3]    ∆W←+/∊∆w                 ⍝  Sum ⍵.
[4]    →(2≠⎕NC '∆a')/MMM        ⍝  If no ⍺, do the monadic case.
[5]    ∆A←+/∊∆a                 ⍝  Sum ⍺.
[6]    ∆z←∆A F ∆W               ⍝  F the sums together.
[7]    →ZZZ
[8]
[9]   MMM:                      ⍝ MONADIC CASE:
[10]   ∆z← F ∆W                 ⍝  Just F the sum of ⍵.
[11]
[12]  ZZZ:
    ∇
```

This defined operator has been invented for the purposes of this article. The name is chosen to commemorate a recent canine contretemps \[FIDO 1993\]. The pooch was probably a Smalltalk aficionado.

### Concept

A defined function takes arrays as inputs and may produce an array as its result. A defined operator takes functions as inputs and produces a derived function as its result. The derived function then processes the input arrays supplied.

Like functions, operators may be monadic or dyadic. Unlike functions, they cannot be niladic or ambi-valent.

### Simulation

In APL\*PLUS II/386, it is necessary to replace each defined operator by a defined function. This frontend function first creates the derived function and then runs it. Each input function becomes a variable containing a function id.

Each function id is a character scalar or vector. It might for example be ‘`÷`’ or that old favourite ‘FOO’. What’s more, if defined operator sequences are being used, it might be the id of another derived function.

When the defined operator is monadic, there is one id; when it is dyadic, there are two.

The frontend inserts the function ids into a copy of a template variable and fixes the result as a derived function. The frontend then runs the derived function with the input variable or variables supplied. The template variable is easily produced by manually editing a copy of the APL2 defined operator’s source statements.

Given some standardisation, a utility function can handle template customisation and the production of the derived function. The frontend marshalls the relevant inputs for this deriver. In the simulation, the deriver is named derive\_fn.

### Coding Invocations

To use a given simulated defined operator, we code an invocation of its frontend function wherever that operator was used. The frontend will take between two and four inputs, depending on whether or not the operator is dyadic and whether or not the derived function is dyadic.

When more than two inputs are present, we batch them up into nested vectors. Here are the defined operator prototypes and their simulated equivalents.

```
 In APL2:                     Simulation:
       (LO MOP) R                    'LO'  MOP R
     L (LO MOP) R                 (L 'LO') MOP R
       (LO DOP RO) R                 'LO'  DOP ('RO' R)
     L (LO DOP RO) R              (L 'LO') DOP ('RO' R)
  Z←   (LO MOP) R              Z←    'LO'  MOP R
  Z← L (LO MOP) R              Z← (L 'LO') MOP R
  Z←   (LO DOP RO) R           Z←    'LO'  DOP ('RO' R)
  Z← L (LO DOP RO) R           Z← (L 'LO') DOP ('RO' R)
```

LO and RO are input functions to APL2 defined operators MOP (monadic) and DOP (dyadic). In the simulation, these inputs become function ids ‘LO’ and ‘RO’ respectively, and the defined operators become identically-named dyadic frontend functions. L and R are the input variables to the derived function. Z is the result of the derived function applied to its inputs.

With this approach, recoding defined operator invocations becomes a routine affair of moving parentheses around and adding quote symbols.

### A Sample Invocation

Let’s pause at this point to see how the defined operator woof fits into this scheme. It’s a monadic operator which yields an ambi-valent derived function. In APL2, its header is therefore in the form

```
      Z←L (LO MOP) R
```

and looks like this:

```
      ∆z←∆a(F woof)∆w;∆A;∆W
```

When we simulate it in APL\*PLUS II/386, the frontend function’s header looks like this:

```
      ∆z←∆a woof ∆w;∆A;∆f;derived_fn
```

In APL2, we might code an invocation of woof such as

```
      4 5 6 ÷woof 3 2
3
```

In APL\*PLUS II/386, we would code

```
      ((4 5 6) '÷') woof 3 2
3
```

### The Frontend Function

In the frontend, we carefully validate the inputs, we get the deriver to produce the derived function, and then we run it. We arrange to have the frontend tell the deriver what name we want the derived function to bear, and then we can localise that name so that the whole thing looks seamless.

Of course, if the application uses the defined operator many times, the concomitant repetition of the derivation process may be an overhead too great to be borne. If so, we might delocalise the derived function, run the deriver once to create it as a permanent resource, alter the relevant defined operator invocations to invoke the derived function instead of the frontend, and save the workspace.

Similarly, if an operator’s template makes multiple references to dependent defined operators, we might group all the derivation work at the beginning of the frontend and have the template use the derived functions explicitly throughout.

And finally, if defined operators are being chained, we will have to delocalise earlier derived functions so that their ids may be passed as inputs to later frontends.

### The Deriver

The deriver is a dyadic utility function which customises a copy of a named template variable with supplied data. The deriver is named derive\_fn. Its left argument comprises one or two input function ids, depending on whether the operator is monadic or dyadic. Its right argument is a two-item vector which supplies the name of the template variable and the name which is to be allotted to the derived function.

Here is a listing of the deriver:

```
     ∇∆a derive_fn ∆w;∆A;∆f;∆o;∆Q
[1]  ⍝  Simulate defined operator.                           ---
```

`PRIVATE [2] ⍝ Derive fn 2â⍵ using template 1â⍵ with fn[s] ⍺. [3] [4] ⍝ ⍺ is scalar or 2-item vector [5] ⍝ - each item must be a character scalar or vector naming a [6] ⍝ primitive, defined, or derived function (eg. ÷ plus tsNOT). [7] ⍝ ⍵ is 2-item vector [8] ⍝ ⍵[1] is text vector name of the defined operator's ⎕VR. [9] ⍝ ⍵[2] is text vector name which the derived function is to bear. [10] [11] (∆o ∆f)←∆w ⍝ Get op-template id & derived-fn id. [12] ⍎'∆Q←',∊∆o ⍝ Get ⎕VR of defined operator. [13] ∆Q[(∆Q='┌')/⍳⍴∆Q]←é∆f ⋄ ∆Q←∊∆Q ⍝ Insert desired derived-fn id. [14] ∆A←é' ',(∊↑∆a),' ' ⍝ Space out first input fn id for [15] ∆Q[(∆Q='?')/⍳⍴∆Q]←∆A ⋄ ∆Q←∊∆Q ⍝ syntax reasons and insert it. [16] →(~(,2)≡⍴∆a)/DDD ⍝ If two fn ids supplied, [17] ∆A←é' ',(∊∆a[2]),' ' ⍝ space out the second fn id for [18] ∆Q[(∆Q='└')/⍳⍴∆Q]←∆A ⋄ ∆Q←∊∆Q ⍝ syntax reasons and insert it. [19] [20] DDD: [21] ∆Q←⎕DEF ∆Q ⍝ Define the template as a function. [22] [23] ⍝ (0) APG 26AUG92 ∇`

As this code reveals, the deriver is a simple text substituter which relies on the presence of pre-arranged cue symbols embedded in the template. The deriver replaces these symbols with character data taken from its inputs.

### The Template

The template is a text vector in the form of a `⎕VR` output. Within it, the three graphics symbols “`┌`”, “`?`” and “`└`” are used as insertion cues. The first of the three symbols is the cue for the derived-function id. The others are used as cues for insertions of the input function ids.

Here is the woof defined operator’s template variable, KK\_woof. As a comparison with the listing of woof given at the head of this article will demonstrate, this template is a manually edited copy of woof’s APL2 source statements.

```
    ∇ ∆z←∆a ┌ ∆w;∆A;∆W
[1]   ⍝ ---- SIMULATED APL2 DEFINED-OPERATOR:  ?woof
[2]   ⍝ Compute sum of ⍺ and sum of ⍵, and then ? these sums together.
[3]
[4]    ∆W←+/⎕ENLIST ∆w          ⍝  Sum ⍵.
[5]    →(2≠⎕NC '∆a')/MMM        ⍝  If no ⍺, go do the monadic case.
[6]    ∆A←+/⎕ENLIST ∆a          ⍝  Sum ⍺.
[7]    ∆z←∆A ? ∆W               ⍝  ? the sums together.
[8]    →ZZZ
[9]
[10]  MMM:                      ⍝ MONADIC CASE:
[11]   ∆z← ? ∆W                 ⍝  Just ? the sum of ⍵.
[12]
[13]  ZZZ:
[14]
[15]  ⍝ THIS FN WAS GENERATED FROM KK_woof VIA ∇derive_fn
    ∇
```

Here is the simulation frontend for the woof defined operator.

```
    ∇ ∆z←∆a woof ∆w;∆A;∆f;derived_fn
[1]   ⍝  Simulate the "woof" defined operator                 --- PUBLIC
[2]   ⍝   ⍺  is one of:
[3]   ⍝     .  a character scalar or vector - function id (eg. +  xyz )
[4]   ⍝     .  a 2-item vector comprising
[5]   ⍝         ⍺[1]  a left-argument to the function to be generated
[6]   ⍝         ⍺[2]  a character scalar or vector - function id
[7]   ⍝   ⍵  is the data on which to operate.
[8]   ⍝   ←  is the result of the derived function.
[9]
[10]   ⎕ERROR (2≠⎕NC'∆a')/'∇woof MUST BE USED DYADICALLY'
[11]
[12]   ⎕ERROR (1<⍴⍴∆a)/'∇woof LEFT-ARG MUST BE FN ID OR A 2-ITEM VECTOR'
[13]   →((2>≡∆a)^^/' '=⎕TYPE¨∊∆a)/MMM       ⍝ If ⍺ is fnid only, monadic use.
[14]
[15]   ⎕ERROR (2≠⍴∆a)/'∇woof NON-SIMPLE LEFT-ARG MUST BE A 2-ITEM VECTOR'
[16]   (∆A ∆f)←∆a
[17]   ⎕ERROR (1<⍴⍴∆f)/'∇woof LEFT-ARG 2ND ITEM MUST BE SCALAR OR VECTOR'
[18]   ⎕ERROR (1<≡∆f)/'∇woof LEFT-ARG 2ND ITEM MUST BE SIMPLE'
[19]   ⎕ERROR (' '≠⎕TYPE ↑∆f)/'∇woof LEFT-ARG 2ND ITEM MUST BE A FN NAME'
[20]
[21]   (é∊∆f) derive_fn 'KK_woof' 'derived_fn'  ⍝  Generate ∇derived_fn
[22]   ∆z←∆A derived_fn ∆w                      ⍝  and run it dyadically.
[23]   →ZZZ
[24]
[25]  MMM:
[26]   ⎕ERROR (' '≠⎕TYPE ↑∆a)/'∇woof INVOKED WITH INVALID FUNCTION ID'
[27]   (é∊∆a) derive_fn 'KK_woof' 'derived_fn'  ⍝  Generate ∇derived_fn
[28]   ∆z←derived_fn ∆w                         ⍝  and run it monadically.
[29]
[30]  ZZZ:
[31]
[32]  ⍝ (0) APG 04JUN93
    ∇
```

In this function, the template variable is KK\_woof and the derived function is named derived\_fn. During the preliminary validation of arguments, messages documenting erroneous invocations of woof are issued; note that these relate to erroneous invocations of the frontend function, not the original defined operator. Finally, here is the derived function produced when use of the woof operator with Divide is simulated.

```
    ∇ ∆z←∆a derived_fn ∆w;∆A;∆W
[1]   ⍝ ---- SIMULATED APL2 DEFINED-OPERATOR:   ÷ woof
[2]   ⍝ Compute sum of ⍺ and sum of ⍵, and then  ÷  these sums together.
[3]
[4]    ∆W←+/⎕ENLIST ∆w          ⍝  Sum ⍵.
[5]    →(2≠⎕NC '∆a')/MMM        ⍝  If no ⍺, go do the monadic case.
[6]    ∆A←+/⎕ENLIST ∆a          ⍝  Sum ⍺.
[7]    ∆z←∆A  ÷  ∆W               ⍝   ÷  the sums together.
[8]    →ZZZ
[9]
[10]  MMM:                      ⍝ MONADIC CASE:
[11]   ∆z←  ÷  ∆W                 ⍝  Just  ÷  the sum of ⍵.
[12]
[13]  ZZZ:
[14]
[15]  ⍝ THIS FN WAS GENERATED FROM KK_woof VIA ∇derive_fn
    ∇
```

### Performance

In the migrated applications, defined operators were used widely. Some of these uses were nested, meaning that they were coded within the statements which comprised the body of another defined operator. Moreover, some nested operators were used several times within the body of a given defined operator.

These repetitions made it convenient to have the outermost defined operator pre-generate some of its dependents’ derived functions and invoke them directly. By doing this, duplicate tours through the generation process were eliminated, which helped to reduce overhead.

Even so, some performance bottlenecks were still attributable to some defined operator simulations. They were cleared by pre-generating all the relevant derived functions as global functions, saving them in the workspace, and recasting all the defined operator calls to reference the derived functions instead.

Where performance was satisfactory, defined operator simulations were left to continue generating their derived functions when required.

### Conclusions

The absence of defined operators from the repertoire of the APL\*PLUS II/386 interpreter is no barrier to the migration of mainframe APL2 applications. Just write a simple frontend function, recode the defined operator in the form of a template, and point the deriver at it. A little editing of the defined operator invocations is all that is then required.

### Reference

1. \[FIDO 1993\] When my friend Doreen brought her dog round.

(webpage generated: 28 March 2006, 06:50)
