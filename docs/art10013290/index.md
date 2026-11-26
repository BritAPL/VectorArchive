---
vid: '10013290'
title: 'Letter: Non-syllogistic mathematical proof'
authors:
- Gérard Langlet
byline: by Gérard Langlet
volume: '12'
issue: '1'
page: '123'
source: trad/v121/langlet121_123.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1995
tags:
- mathematics and statistics
---

by Gérard Langlet
{ .byline }

Given any sophisticated function f of any number of variables with the result *r=f(x,y,z,...)*, we can experiment with as many runs as we wish on any available computer.

First, we shall read a series of values for *x,y,z,...* as well as for initial *r* (e.g. set to 0), and store them into the linear memory of the computer (note: we do not impose that such variables – arguments as well as result(s) – be numerical scalars; they can be heterogeneous arrays of arrays).

Second, we take a snapshot of `V`, the occupied sequence of the computer memory for such a problem. Of course, this snapshot ignores the input-conversion format routines that were used, and ignores the naming of the variables as well as their respective lengths. `V` is just a binary sequence of 0s and 1s with length `N` matching `⍴V`.

Third, we let the function operate.

Fourth, we take a second snapshot of `W`, the binary sequence of the modified memory at the same locations (it has the same length `N`).

Now, we shall repeat the procedure with random values of the argument variables (a Monte Carlo technique), at least `N` times, let us say `P` times. So, we get two rectangular arrays `AV` and `AW`, with each `P` rows and `N` columns in which the columns are the successive `V`s and `W`s respectively.

Is it always possible for `N=P`, to find a matrix `M` so that `AW` is `M.AV` (if the period stands for the modulo2 matrix product, i.e. `≠.^` in APL)?

If the answer to such a question is YES, we have a proof that function *f* may be replaced by a matrix product, then a proof that *f* is a linear function even if it is thought of as nonlinear! (Even if `N` had to be replaced by `N`<sup>2</sup> or by any still larger but finite number, this would hold!)

It has to be so, because in modulo2 algebra, nonlinearities do vanish. In order to simplify, let *f* be a secret nonlinear fn of just one variable, let us say a numeric variable *x* coded in floating-point arithmetic, e.g. with 64 bits. The result *r=f(x)* is supposed to be another numeric variable of the same type; so, storing *r* into another location than the one used for *x* would waste memory; once the value of *x* is used by *f*, it becomes useless, so we shall store the result at the same location; our application has become *x=f(x)*.

The initial value for *x* can be chosen among 2<sup>64</sup> values (supposing our arithmetic has no “nan” (not-a-number).

What we say here for numbers coded in 64 bits also holds for numbers coded in n bits, with n small, e.g. 2 So, for pairs of bits, initial *x* has to be one of the 4 couples (00) (01) (10) (11). The result *r* or final *x* has to be also a member of the same 4 couples.

Function *f* may transform (00) into (00) or (01) or (11) or (11), i.e. 4 possibilities.

Function *f* may transform (01) into (00) or (01) or (11) or (11), i.e. 4 possibilities again.

Etc...

Altogether, one could think we have a restricted choice between 4x4x4x4 functions *f*, i.e. 256 functions. In fact, this is much too much...

Function *f* has to reduce to an algorithm which:

If the first bit of a couple of bits is 0, either swap it to 1 or don’t (2 possibilities). Then either swap the second bit or don’t (2 more possibilities).

If the first bit of a couple of bits is 1, either swap it to 0 or don’t (2 possibilities). Then either swap the second bit or don’t (2 more possibilities).

Obviously, only 2<sup>4</sup> i.e. 16 possibilities exist for f, not 256.

Only 2<sup>4</sup> i.e. 16 possibilities exist for matrix M: `?2 2⍴2` in 0-origin.

Among the 16 possible matrices, only 6 are invertible; only 4 of these are self-inverse (modulo 2) which corresponds to a reversible function (among which the do-nothing fn or identity matrix, the mirror-function i.e. `⌽` in APL or the anti-identity matrix, and 2-genitons Gh and Gv, responsible for the cognitive and the helix transform respectively).

The same reasoning holds for any finite value of *n*.

Without repeating the geniton story, the proof that – at least numerically, i.e. with any type of computer which would use any type of representation based on bits, in order to code any type of data – all functions coalesce into linear functions, implies that no function may be chaotic, except if one allows strange rules to alter precision, such as the ones which are used e.g. to subtract values in floating-point arithmetic and transform PI either into 0 on the PC and 360-like mainframes or into ∞ on the Macintosh.

(webpage generated: 2 July 2007, 05:55)
