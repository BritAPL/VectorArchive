---
vid: '10013750'
title: Enigma 1368
authors:
- Adrian Smith
byline: attacked by Adrian Smith (adrian@apl385.com)
volume: '22'
issue: '3'
page: '55'
source: trad/v221/enigma.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- competitions and puzzles
---

*attacked by Adrian Smith (adrian@apl385.com)*
{ .byline }

## The Puzzle

EEN, VIER and NEGEN are Dutch for 1,4,9. Replacing letters by numbers we get three perfect squares (no number starts with zero). What is the value of the square root of <span class="APLU">( EEN  × VIER × NEGEN )</span>?

## The Attack

This one is interesting, in being largely just a logic puzzle rather than a huge combinatoric. It is a nice example of where a ‘desk calculator’ APL can be really helpful in expressing the tests you need to reduce the lists of numbers to a manageable size. No programming was required at all here, just typing.

The first stage is obvious - how many perfect squares are there with the pattern EEN. I immediately thought of 225, but let ’s list them to be sure:

```
      (10↓⍳31)*2
121 144 169 196 225 256 289 324 361 400 441 484 529 576 625 676 729
    784 841 900 961
```

Damn, forgot about 441. However there are only two of these, so we have only two choices for E and N. Let’s go with E=2, N=5 and see how we fare. Here are the candidates for NEGEN, created by copy/paste in the session:

```
    2⍕ 52025 52125 52225 52325 52425 52525 52625 52725 52825 52925 * 0.5
 228.09 228.31 228.53 228.75 228.97 229.18 229.40 229.62 229.84 230.05
```

Nope, none of those look very integer. Let ’s try E=4, N=1 and play the same game:

```
    2⍕ 14041 14141 14241 14341 14441 14541 14641 14741 14841 14941 * 0.5
 118.49 118.92 119.34 119.75 120.17 120.59 121.00 121.41 121.82 122.23
    121*2
14641
```

Got it! Now we just need a 4-digit perfect square with 4 as the third digit. There are only 9000 or so candidates, but at this point a bit of computer assistance is quite welcome!

```
      qq← (4=( 10 10 10 10⊤ (⍳9999))[3;])/⍳9999
      ⍴qq
1000
      qq/⍨←qq>1000
      ⍴qq
900
  qq/⍨ {⍵=⌊⍵} qq*0.5
1444 1849 3249 3844 5041 6241 7744 8649
```

Visual inspection (and the backspace key) can do the rest. Anything with repeated digits is out of it, for starters. Then we can remove anything with 1 or 6 as these are already allocated other letters. So the answer is ...

```
 ( 3249 × 441 × 14641 )*0.5
144837
```

Get your answers in by 4th Jan 2006 for a chance to win a £15 book token! On second thoughts, don’t bother. It really was rather a simple one.
