---
vid: '10004680'
title: The Ultimate Turing Proof
authors:
- Gérard Langlet
byline: by Gérard Langlet
volume: '10'
issue: '3'
page: '124'
source: trad/v103/langlet103_124.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- programming techniques
- mathematics and statistics
---

by Gérard Langlet
{ .byline }

Alan Turing is, with J. von Neumann and K. Iverson, the most important spirit in Computer Science. British readers who are involved in this last field, even if they do not live in Cambridge, probably all know the famous **Turing Machine**, which, as it was proven, could theoretically process ALL POSSIBLE IMAGINABLE ALGORITHMS.

Turing’s work on decision theory is less known; however, together with Goto’s in Japan and Peirce’s in the US, it represents a chef-d’oeuvre of simplicity and efficiency. And Turing could not have proposed his genial machine without thinking on decision first.

Here are two different APL models of a Turing Machine.

### Exercise : Scan these models to find the difference.

Let us execute a small session and explain the details after.

```
     ∇ TURING;C;I;e
[1] e←'P[I]←~P[I]' ⋄ I←⎕IO ⋄ P←B ⋄ vdo 'C←P[I] ⋄ I←I+1 ⋄ ⍎C/e'
     ∇
     ∇ TURINGD;C;I;e
[1] e←'P[I]←~P[I]' ⋄ I←⎕IO ⋄ P←B ⋄ vdo 'C←B[I] ⋄ I←I+1 ⋄ ⍎C/e'
     ∇
     RNDB ⎕←? 99 ⋄ I←B ⋄ TURING ⋄ B←P ⋄ TURINGD ⋄ I≡P
3
1
     RNDB ⎕←? 99 ⋄ I←B ⋄ TURING ⋄ B←P ⋄ TURINGD ⋄ I≡P
85
1
     RNDB ⎕←? 99 ⋄ I←B ⋄ TURING ⋄ B←P ⋄ TURINGD ⋄ I≡P
72
1
     RNDB ⎕←? 99 ⋄ I←B ⋄ TURING ⋄ B←P ⋄ TURINGD ⋄ I≡P
8
1
```

The APL line which is executed is a conjecture line. One may replace 99 by infinity and never get any answer: the Turing Machine would never stop. The computer’s universe is finite, so you are not allowed to replace 99 by infinity. Then, this Turing Machine will stop.

Another case which would stop the Machine is a Parity Error, in fact, any error, because all possible errors are combinations of parity errors. Even a break, coming from you, depressing the &lt;attn> key, or hitting the computer with a mallet or hammer (in case you’re fed up with your system) or coming from the electric power plant, will be perceived by the Machine as some parity error. Will it decide to stop? Anyway, it will stop.

Here comes an essential extension to APL, the “vital do” or “vital execute” function which programs you and me:

`vdo e` is supposed to iterate the execution of `e`, any APL expression which might program correctly the Universe and Life Game, as much as it can, i.e. until extinction or death, a) by cancer, brain or vessel or heart-break: any inner parity error, b) by outer influence: virus, bomb, bullet, Attn-key, starvation. Cancer may be also be induced by outer influence (e.g. AIDS or bit-mutation by a particle (neutrino?), ingestion of some aromatic hydrocarbons). The difference between a) and b) is small: both cases may kill the job.

### Exercise : Program the `vdo` function which is strongly implementation-dependent.

`RNDB N` produces as the global variable `B` with shape `N`, a Boolean sequence, binary vector with any possible content. So, `B` is any small part of the unknown Initial Program, or a small part of the unknown Initial Data.

The least-action-principle, formulated by Maupertuis, only states that “Nature is thrifty in all its actions”. Applied by Hamilton to Newton’s mechanics, it simplified the original equations, then allowed Maxwell to propose his general equations for electromagnetism. Reformulated by Ernst Mach as the “economy principle”, it was applied by Einstein in all his equations. So, we may suppose it is correct, at least in its original definition, because the Turing machine cannot be approximated correctly by any equation. Any bit may contain parity 0 or 1. There is nothing between. Transition from 1 to 0 or from 0 to 1 is the elementary quantum jump. “No transition” is “no jump”, and nothing else. The elementary principle of quantum mechanics is Pauli’s principle, which constrains particles (fermions, such as electrons, protons) to have, in the fundamental state, opposed spins `↑ ↓` (e.g. 0 1 or 1 0 in Boole’s notation) when all other parameters (quantum numbers) are the same.

Then, we shall replace “thrifty” by “minimal” which will also mean “optimal”. As a first consequence, e must contain the most elementary possible function which is monadic NOT, and nothing else. As a second consequence, for reasons of economy, the data ribbon and the program ribbon will be the same ribbon, i.e. a self-organising structure of information at its quantum level, the bit-level. (While initial APL made a difference between functions and data, LISP does not; “execute” `⍎` allows you to execute data as a niladic fn. It was introduced into APL-SV at the same time as scans). The vital execute overlaps execute as an endless loop; it corresponds to some special case of the `⎕EA` extension in APL2. But the regular execute has some terrible power over the vital execute. If it fails, life, i.e. the Machine, stops at once. The use of + in `I←I+1` ONLY means: now, process the next bit. The value of I has no importance except in present-time computers based on arithmetic... on any sequence of information, index I could be initialised at any value. The most important thing is death, i.e. caused by indexing (index error), when there is no more information written on the ribbon, “The End” of the movie. As long as there is something, i.e. a parity on the ribbon, life goes on. So, the Machine is not allowed, this time, in this implementation, to rub any bit out and to replace it by “NOTHING”; if it were allowed to do so, it could and would commit suicide...

So, this APL-reduced Turing Machine shall work for eternity, either with an infinite NOTHING-free ribbon, or with a circular (looping) one, which cannot be a Moebius ribbon, because the scenario changes, the ribbon having organised itself differently between two successive readings or writings of the same bit position. But, since combinations of bits in an original finite sequence are in finite number, the same initial sequence MUST appear again. A looping ribbon will produce periodic structures, whatever the content of `e` is.

Another rule has to be respected by the machine, time irreversibility. So, the APL-reduced implementation is not allowed to go back: index I cannot decrease; however, some structures, already observed in the past, will be reproduced because of periodicity effects (eclipses) with periodic ribbons (orbits). An infinite ribbon will produce self-affine structures, i.e. structures which look like the ones observed in the past, but not quite: exactly what is observed e.g. magnifying the Mandelbrot set (one can see, at any scale, structures which will reproduce, almost, the original one.)

The APL Turing minimal machine is faster, more accurate, more simple than any machine or formula. It does not use floating-point conversions or complex numbers, or integers. Just `NOT` is its heart and brain. The indexing business is a pointer-business. It could be replaced by a `1↑1↓` combination or a `1⌽` shift. It is programmed with I for a couple of reasons: a) the APL prototype can may be easily rewritten in any other programming language; b) we want to keep records of the past, in order to make post-mortem autopsis of the results (which would be lost otherwise, if the ribbon was cut just left of the read/write head). The Turing machine shall make the future. The present implementation preserves the past.

Why are there a couple of machines: `TURING` and `TURINGD`?

For a couple of reasons: `TURING` is the true future-oriented machine; `TURINGD`, a slightly different function, is another interesting machine which allows one to go back to the past although it works exactly the same way as `TURING`, except that `TURINGD`’s control variable is itself in the past (old B). `TURING`’s control variable for the future is `P`, the present.

So, `TURINGD` is an H.G.Wells machine, as the inverse function of `TURING`. The conjecture using `RNDB` and the identity `≡`, which corresponds to a strong theorem at the end of the paper, shows it, answering 1 for any combination of bits of any length. The couple is a marvellous Stevenson Machine for the exploration of a new Laputa island as one will discover it soon. It is also a Wonderland, a Lewis Carroll machine.

`TURING` and `TURINGD` are Stephenson machines too, able to run both ways with the most simple flip-flop expression using `NOT`.

Now, let us prove that `TURINGD` is a Babbage machine also, i.e. the prototype of the “error-free Difference Machine”.

Take any bit-sequence as B. Form the successive Boolean differences between bits. Submit B to `TURINGD` and compare the results. Then, `TURINGD` is a Pascal Machine, a Leibniz Machine too, a Prony machine too.

Of course, `TURINGD` is a Gray machine, since it produces for any `B`, the Gray code of `B`, used in signal and image processing...

If `TURINGD` is a difference machine for any `B`, i.e. a Differential machine, then, `TURING` is the Integral Machine, able to integrate infinitely any information at any order of integration with never an error!

If we can prove that `TURINGD` is useless, i.e. that `TURING`, the Least-Action Machine (LAM), is also able to perform the job of `TURINGD`, although `TURING` is a future-oriented integrator, then would not `TURING` become be the Universal Machine, i.e. the Machine of the Universe, the Absolute International Association for Computer Machinery-SIG/APL Vector?

First, if 1 0 is the elementary sequence, also symbolised by `X` as 1 and `Y` as 0, then with `X Y` for `B`, `TURING` produces `X X` . With `X X`, `TURING` produces `X Y`. All right, then, `TURING` is a Sex Machine. But `TURINGD` produces the same results! This will always be the case with a two-body problem such as man and woman. Period is 2 for a 2-body problem, 4 for a 3- or 4-body etc..., so that, indeed, `TURINGD` produces the results of `TURING` in reverse order. Except for an infinite universe for which the period is infinite, but all the sub-periods (cycles) also exist, `TURING` may produce the results of `TURINGD`, so that `TURINGD` is useless for any finite computer or algorithm.

But, now, does `TURING`, which is able to compute everything with almost nothing, represent itself an optimum in APL notation? No, of course, because APL is the most clever tool ever invented and implemented.

For `B` any binary vector or parity sequence, `TURING`, the Universal Machine, produces, as its product `P`, the same result as what one obtains (moreover on hyper-arrays) without branch, indexing, take or drop, circular shift, execute or even vital do, etc..., as:

```
≠\B    ⍝ NO COMMENT.
```

End of Proof.

### Begin Future

Note. The original Turing Machine was able to run both ways. It is not necessary if one can prove that future reproduces past at least on finite systems, so that if the results of `TURINGD` which goes backwards are also produced by `TURING` going forwards only, then, `TURINGD` can be sent to the museum.

### Annexes

In APL, `0*0` is 1, even after ISO8485. How NOTHING, raised to No power, gave birth to SOMETHING, is the most mysterious question of the Universe, in order to explain the Bit Bang!

Given `L` and `R`, a couple of vectors with the same shape, the least-square criterion is: `(L-R)*2` for any item. What is the least-square criterion if, in APL2, items become words?

Now, for L and R expressed in bits, correctly, with a unique code and not with multifractal polysemic “ad hoc” codes, such as IEEE or EBCDIC or many other ones (on exotic computers), expression `(L-R)*p` with `p` any power except 0, THUS the least-any-non-null-power criterion, reduces to `L≠R` without brackets or exponent, for any meaning of the bits in `L` and `R`.

If `⍺ do ⍵` executes `⍺`, an APL expression, `⍵` times, then, for any content of `L` and `R` with `D` an integral power of 2, expression:

```
V←L,R ⋄ 'V←≠\V' do D←⍴B ⋄ D↓V
```

or:

```
V←R,L ⋄ 'V←≠\V' do D←⍴B ⋄ D↓V
```

produces the same result as `R≠L` or `L≠R` (achiral expressions), proving that scalar dyadic `≠` also reduces to chiral (asymmetric) monadic `≠\`, or, rather, that `≠\` IS the elementary interaction, able to produce, as consequences of its Existence, all other useful APL constructs which never alter any Bit of information, producing all useful shapes and signals without a single incrementation of entropy. `≠\` is perfect, producing past, present and future as a gigantic spacetime linear hologram. Can it also think?

### Action Results from Decision

Do it if allowed, if the control variable is dominant, if the neurobit is excited (1).

Don’t do it if prohibited (also inhibited), if the control variable is recessive, if the neurobit is at rest (0).

All decisions, then all actions, follow this scheme which is generally admitted. Now, only the status of the neuron, either 0 or 1, will be taken into account, hence the word “neurobit”. The quantitative “measure” of neural potential disappears: it has no more meaning. Formerly, the control variable resulted from a collection of weights, after some arbitrary averaging. Alas, information, considered as bits, cannot (and MUST NOT) be averaged (what does “1.8 child per family” mean?); but it can be sampled.

In bits (rather in isomorphous modulo 2 algebra), weights are either 0 (not important) or 1 (important), i.e. information itself; this corresponds to a terrific economy principle... for which data and program were already unified; the results are new data which are also the new program, each bit being its own weight at all orders of integration, differentiation and power, each current bit being the control variable of its own next future, and also the parity check of its past. Could any processor be more clever than this one, knowing that the properties of such an all-mighty self-organiser, have not been inventoried yet?

A recursive decision chain such as:

If A then if B then if C then if D then do it ,

is equivalent to “reverse thinking”:

If D then if C then if B then if A then do it .

or to “any-order thinking” such as, e.g.:

If B then if D then if A then if C then do it.

All control variables `A,B,C,D`, in any order, must be TRUE (1).

Then, `^/ A,B,C,D` is equivalent (altogether-control variable).

Then, `≠/ 1 0 0 0` (parity check) is also equivalent.

This latter expression, seen as `¯1↑≠\1 0 0 0` , means, with much more economy of initially-excited neurobits:

If `A` (i.e. if you are to make a decision so that `A` is 1), then swap `B`, then if there is no inhibition coming from `B` (i.e. if `B` was 0 so that `A≠B` has now swapped `B` to 1), go on, otherwise, do nothing (the decision chain is broken).

So, `≠\` spares excited states, and has the same result as “AND propagated” although, theoretically, it was impossible to deduce AND from XOR in Boole’s algebra! But vector processing (parity integration) with `≠\` as the unique model of nerve influx, makes miracles: the important clue was to deduce `^\` (which, of course, contains AND) from `≠\`, THINKING of vectors and no more of scalars.

`≠\N↑1` with any `N`, is the “domino falling chain” with 1 for “domino falling or fallen” i.e. “going to reach or having reached its quantised stable state” and 0 for “domino standing, potentially able to fall, only if hit by its formerly-standing left-neighbour”. If any domino in the chain does not fall on its still-standing right-neighbour, the percolative (physical or decisional) chain breaks in both cases with the same mechanism, also known in electro-magnetism as the \{unexplained\} Branly effect, or, in coherent light-emission, as the Kastler effect, i.e. laser optical pumping.

When the AND-gate has appeared, then, everything becomes possible, (even arithmetic!) because scalar `⍺` AND `⍵` is, in fact, just a special case of the more general triadic MAJORITY function, when the 3rd argument is 0. The essential majority function is necessary to obtain the carry in addition for the 3rd register C. Such a carry becomes the third argument which is left-propagated in addition, but right-propagated in subtraction; the Boolean sum on 3 registers or the Boolean difference on 3 registers `A,B,C` is, of course, `A≠B≠C` i.e. `≠/A,B,C` i.e. `¯1↑≠\A,B,C` the parity check. It would make no difference using `=\A,B,C` instead, since two negations (Boolean, arithmetic or, more generally grammatical, always destroy themselves). `≠/` and `=/` return the same result for any odd number of neurobits along the chain, and opposite results for any even number of neurobits.

A clever brain need not count its neurobits; all it has to know is the parity of the chain in order to decide. And the parity is 1 if the number of bits which have been integrated by `≠\` from the beginning of the (now exponent-free) Markov chain, up to the current item (neurobit), i.e. the new current item, is 1, for all terms, for any chain.

A clever brain need not know the status of all the items in order to decide. The knowledge of the last swap is enough: if the chain were broken anywhere, in any order, i.e. if no parity swap has occurred in the last bit of the domino chain, it already knows that it has nothing to do. All the formerly-presupposed high-complexity combinatory arithmetics of the brain has vanished: the decision of “not to do it” as the result of any inhibition, may become as fast as light is...

So, time is, for the brain, created by swaps. No-swap is just sleep. When you sleep, you are not “aware of time”. When the result of `≠/` is 1, it is simply “time to wake up”, at any scale of time, either a fast decision (in quantum-time) on three bits, since the MAJORITY function is also the decision function, or up to a full night of unconscious time on billions of terabits. Unconscious time is dream time, when the whole chain is broken somewhere by any 0, while the whole memory is scanned, integral after integral, in the Sierpinskian process, now generalised to the MAJORITY decisions everywhere in the memory. After almost a whole cycle, when the initial pre-percolative chain `N↑1` is restored again, with only one neurobit excited, the first one, the `≠\N↑1` percolates it to the end of the chain, so that `^/` is produced by `≠/` as the alarm-clock; this single bit means “do it”, i.e. “wake up” i.e. “be conscious again”: another day begins.

The important point was indeed to realise that `^/` is a temporal process, and to introduce quantum time into logics. “If then if then if then...” is equivalent to `≠/` on resting neurobits except the first one, which contains the main decision (on ONE excited neurobit) to explore the chain, i.e. to scan it.

`^\` was atemporal in conventional binary algebra, and required too much stress, i.e. all bits excited. `≠\` THE clever parity-check algorithm, the laziest algorithm of all algorithms, excites all the sleeping neurobits by its wave, produces `^/` as a side effect within a short flash, while the second wave of `≠\` reduces the stress between neighbouring 1’s, swapping every even neurobit to 0 for rest, immediately, without aspirin, because of it short-range repulsive action:

“`1≠1` produces 0”, i.e. `≠/1 1` the “short-range repulse”, will become the secret key of a correct dynamic interpretation of mental cryptography, BOTH encryption and decryption; it was the secret of conventional cryptography already. But it will also decrypt gravitation in general, with and without inhibition, i.e. external repulsive effect: the dominos simply fall and stay lying because the Earth’s gravitational field (another `≠\` effect) inhibits the second wave and becomes dominant once the first wave has processed the chain: this explains what the macroscopic notion of the mysterious “potential energy” was. Avalanches and Newton’s apples indeed stay on the floor after the first wave; just remove the inhibitor field (e.g. experiencing stroboscopy on dominos or sugar lumps in space), then, because of time shift, i.e. phase delay, the full Sierpinski effect of the elementary interaction will appear on the spacetime image, because all successive waves of `≠\` will execute without Earth’s

gravitational inhibition. This is what occurs within the atom itself, Earth’s action (inhibition) being screened there too (inhibition is inhibited). Altogether, this will explain the famous chaos game by Barnsley, when tossing a hypothetical 3-sided coin (Head, Tail, Side) one million times and give an idea of what “chance” is.

The conventional Gaussian shape of combinatory statistics is just the envelope of the discrete sequence of the binomial coefficients `p!n` which form each row of the Pascal triangle; for any `p` and any `n` which are different from 0, i.e. exist; then, in parity algebra, with `n≡1` and `p≡1`, of course, the result is 1. For all other cases, the thrifty brain sleeps. Sierpinski is both the parity of Pascal and the result of “tossing” in the parity field. `≠\` produces the universal “roll”. And exclusion is also a “deal”. A good deal. A good action. A good auction.

God does not play dice. Everything plays `≠\` . Pascal’s bet is Sierpinski’s, i.e. chance on parities. Our best chance of understanding lies with the best tool for thinking & checking: APL.

(webpage generated: 9 July 2007, 00:12)
