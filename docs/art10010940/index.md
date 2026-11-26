---
vid: '10010940'
title: Cows and Bulls – A Solution
authors:
- Anne Wilson
byline: by Anne Wilson
volume: '11'
issue: '2'
page: '29'
source: trad/v112/wilson112_29.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1994
tags:
- competitions and puzzles
- programming techniques
---

by Anne Wilson
{ .byline }

Herewith my solution to Cows and Bulls, when the computer guesses. The problem is one of reducing the number of possibilities using the ‘hits’. I used this as the design of the function:

- evaluate all possibilities \[4\] – \[8\]
- drop those with wrong bull score \[16\]
- drop those with wrong cow score \[18\] – \[19\]

```
[0]   FROM BULLCOW PICK;ABC;BC;FREQ;GUESS;POSS;TRIES;X
[1]   ABC←FROM↑'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
[2]   TRIES←0
[3]  ⍝ This ran out of space   POSS←ABC[⍉1+(PICK⍴FROM)⊤¯1+⍳FROM*PICK]
[4]   X←((PICK,19)⍴'((FROM*PICK)⍴(FROM⍴')
[5]   X←X,[2]((PICK,5)⍴ 5 0 ⍕FROM*⌽¯1+⍳PICK)
[6]   X←X,[2](PICK,10)⍴')/ABC),[2]'
[7]   X←'((FROM*PICK),PICK)⍴ ',(,X),'((FROM*PICK),0)⍴'' '''
[8]   POSS←⍎X
[9]  loop:→(0 1 =1↑⍴POSS)/lerr2,lok
[10] ⍝ A random choice for the next guess
[11]  'Is it ',(GUESS←,POSS[?1↑⍴POSS;]),' Enter BULLS (hits) and COWS (near)'
[12]  →(2≠⍴BC←⎕)/lerr1
[13]  →(PICK<+/BC)/lerr1
[14]  TRIES←TRIES+1
[15] ⍝ Keep only if the number of BULLS is correct
[16]  POSS←(BC[1]=POSS+.=GUESS)⌿POSS
[17] ⍝ Next look at COWS + BULLS
[18]  FREQ←+/ABC∘.=GUESS
[19]  POSS←((+/BC)=+/((+/[2]POSS∘.=ABC)⌊((1↑⍴POSS),⍴FREQ)⍴FREQ))⌿POSS
[20]  →loop
[21] lerr1: 'Try again - you have made a mistake'
[22]  →loop
[23] lerr2: 'You have made a mistake - I''VE WON'
[24]  →lend
[25] lok: 'After ',(⍕TRIES),' tries I reckon it is ',,POSS
[26] lend:
```

Sample run, where solution is ABCD:

```
       4 BULLCOW 4
Is it DBBA Enter BULLS (hits) and COWS (near)
 ⎕:       1 2
Is it AABD Enter BULLS (hits) and COWS (near)
 ⎕:       2 1
Is it ABCD Enter BULLS (hits) and COWS (near)
 ⎕:       4 0
After 3 tries I reckon it is ABCD
```

Editorial comment: This is an excellent solution to the general `m×n` problem. It works provided `FROM*¯1+PICK` is less than 100000 (though this could be extended by minor modification of lines \[4\] to \[8\]). For the larger problems, of course, you need lots of megabytes of RAM!

(webpage generated: 14 October 2007, 19:04)
