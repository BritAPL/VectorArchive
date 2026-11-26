---
vid: '10007880'
title: The N-Queens Problem in One Line
authors:
- R.G. Selfridge
byline: by R.G. Selfridge
volume: '10'
issue: '3'
page: '94'
source: trad/v103/selfridge103_94.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- programming techniques
- competitions and puzzles
---

by R.G. Selfridge <span class="affil"></span> <span class="email"></span>
{ .byline }

The July 1993 edition of Vector has an article discussing putting Queens on a chessboard so that no two can attack each other (Backtracking, Queens and Permutations, pp34-38). Given the old myth (?) that any program can be written as a single APL statement, I naturally ask if this is possible for the Queens problem. Here is the ‘yes’ answer to that question:

```
     ∇ z←x q b
[1]    →⍎(((0=⍴x)/'0,(⍴p←z←'''')q(b,b)⍴0×d←
         ((n-k)⊖d)∨d∨⌽d←n∘.=n←⍳¯1+2×k←b')),
         (z←0≠⍴x)/'⍎(((k=⍴x)∨~i)/''0''),
         ((i^k≠⍴x)/''(x,0)q b∨(⍴b)↑((⍴b)-x[⍴x],⍴x)↓d''),
         ((i^k=⍴x)/'',⍴p←x,p''),(((1=⍴x)^~i)/'',p''),
         ((i^(⎕pw<(2×k+1)×1+(⍴p)÷k)^k=⍴x)∨
         (1=⍴x)^~i←k≥x[⍴x]←x[⍴x]+(x[⍴x]↓,b[;⍴x])⍳0)/
         ''←0⍴⎕←''''| _Q''''[(2+p[1;]>2),[1]
         p←1+((2×1↓⍴p)⍴0 1)\1+p←(((1+k)×(1↓⍴p)÷k)⍴(k⍴1),0)\
         1+p←(⍳k)∘.=p]'''
     ∇
     '' q 4
 _ _ _ _   _ _ _ _
|_|Q|_|_| |_|_|Q|_|
|_|_|_|Q| |Q|_|_|_|
|Q|_|_|_| |_|_|_|Q|
|_|_|Q|_| |_|Q|_|_|
```

Let me add that no claim is made that this is ‘good’ APL, only that it was a challenge to see if it could be done. Timing against a ‘proper’ program actually shows that this function is only twice as slow. The function will take any board size, greater than 3, and will print out as many boards across the screen as permitted by `⎕PW` (sanity suggests that board sizes greater than 9 or 10 not be tried, since there will be quite a wait for the output to complete).

(36 minutes for ’’ q 11 on a DX2/50 with Dyalog APL/W ... and I think Richard Smith deserves the next Outstanding Achievement Award for typing this in from the listing and getting it to run first shot! Production Ed.)

(webpage generated: 18 February 2006, 02:17)
