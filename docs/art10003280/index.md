---
vid: '10003280'
title: Migrating Mainframe Applications to APL*PLUS II
authors:
- Allan Gay
byline: by Allan Gay (Cocking & Drury)
volume: '10'
issue: '2'
page: '130'
source: trad/v102/gay102_130.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- development practice
- implementations
---

by Allan Gay <span class="affil">(Cocking &amp; Drury)</span> <span class="email"></span>
{ .byline }

### Introduction

For the past few months, my colleague Kevin Ryall and I have been working on migrating a client’s mainframe APL2/MVS applications into APL\*PLUS II/386 on a 486 machine. Kevin has shouldered the lion’s share of the drudgery while I have had all the fun. In this article I introduce some of the interesting aspects of the project. Subsequent articles will expand these.

I am indebted to Dave Ziemann for his very helpful critiques of this article and the others in this series. Any errors which remain are mine, not his.

### Core Issue

The mainframe applications make extensive use of GDDM, VSAM, and APL2-isms, none of which are included in APL\*PLUS II/386. This posed a major question right at the outset – substitute or simulate?

Substitution entails rewriting large sections of the apps to eliminate all references to unavailable facilities. For instance, the functions to build and operate a complex fullscreen panel via GDDM have to be completely rewritten to use APL\*PLUS II/386’s `⎕WGET`, `⎕WIN`, `⎕WKEY` and `⎕WPUT` facilities. Worse still, conceptual differences tend to enforce a different functional decomposition of the task. Before you know it, you can be embroiled in rewriting great swathes of code from scratch.

Simulation, in contrast, means adding a layer of new code whilst largely preserving the existing code and the interfaces. If the simulation is sufficiently comprehensive, migration can be relatively straightforward, although there will be a weight penalty.

Because the use of unavailable mainframe facilities was so extensive, the simulation option was chosen.

### Simulation Form

Each simulation takes the form of a workspace which must be copied into the application workspace to supply a missing facility. Because defined functions are being substituted for intrinsic language elements such as shared variables, some recoding at the actual point of interface is unavoidable, but application logic is still preserved.

### Which Simulations?

This is the first in a series of articles in which I’ll be describing simulations of GDDM, VSAM, APL2 defined operators, and some other APL2-isms.

Looking at this list, you might be forgiven for thinking that APL\*PLUS II/386 seemed pretty limited compared to mainframe APL2, but you’d be wrong. If we were migrating in the opposite direction, we’d find ourselves bewailing the lack of `⎕WIN`, the component filing system, and the Split and Mix functions.

All the simulations require APL\*PLUS II/386 Release 4 or higher, operating in Evolution Level 2 mode.

### Examples of Use

First, GDDM’s ASDFMT operation. Here’s how we do it on the mainframe:

```
      CTLg←402,(0 1+⍴FLDS),,(⍳1↑⍴FLDS),FLDS
      RES←FSchk CTLg
```

And here’s how we do it on the PC –

```
      RES←FSchk 1⊃GDEXEC 402,(0 1+⍴FLDS),,(⍳1↑⍴FLDS),FLDS
```

In essence, we replace two references to the shared variable by “1 GDEXEC”. The important thing to note is that the actual data and the application logic by which it is developed remain exactly the same.

Next, let’s look at how we replace a record on a VSAM key-sequenced dataset (KSDS). On the mainframe, we do –

```
      DATV←RECORD             ⍝  Put data into shared-variable.
      CTLV←'W'                ⍝  Issue the WRITE request.
      VSAMchk CTLV            ⍝  Check the return code.
```

And on the PC we do:

```
      VSAMchk 1⊃VSKSW RECORD     ⍝  Write the record.
```

In this case, the defined function VSKSW replaces the shared variable references.

Now let’s turn to APL2 defined operators. In mainframe APL2, we say:

```
      ALPHA ÷defined_op OMEGA
```

but in APL\*PLUS II/386 we say

```
      (ALPHA '÷') defined_op OMEGA
```

Things have got a little more hairy here because defined\_op has now become a defined function. Therefore we have to bundle its input function and one of the inputs to the resulting derived function as a nested array.

Finally, some axial arithmetic. Mainframe APL2’s

```
      A+[1]3 3⍴⍳6
```

becomes

```
      A plus_with_axis 1,⊂3 3⍴⍳6
```

and, again we have had to bundle one of the inputs with the axis specification.

### Conclusion

That was just a taster but you can see that use of the simulations does not entail any violent upheavals in the original code. Using them, we’re happily operating fullscreen apps with thirty-plus panels – many built on the fly – to front 7000-record KSDSs which have multiple indexes. The simulated defined operators are stacked n-deep, and our arithmetic is truly axial.

(webpage generated: 6 December 2005, 23:53)
