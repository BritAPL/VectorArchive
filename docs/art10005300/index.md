---
vid: '10005300'
title: Cows and Bulls
authors:
- Alan Mayer
byline: by Alan Mayer (a.d.mayer@swansea.ac.uk)
volume: '10'
issue: '1'
page: '39'
source: trad/v101/mayer101_39.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- education
- competitions and puzzles
---

*by Alan Mayer*  ([a.d.mayer@swansea.ac.uk](mailto:a.d.mayer@swansea.ac.uk))
{ .byline }

This game has appeared in many different forms under many different names for more years than I care to remember. The computer chooses a random selection of letters, and the player takes a guess at what the list may be. The computer then reports the number of bulls (correct letters in correct places) and the number of cows (correct letters in incorrect places). The idea is to get the correct solution in as few attempts as possible.

In my version, you can choose how many distinct letters can be chosen, and how long the list should be. For example if the game is initiated by the command 4 COWBULL 5 a vector of five letters will be selected from A,B,C,D (i.e. there must be at least one double). On the other hand 5 COWBULL 4 will select four letters from A,B,C,D,E.

```
[0]   M COWBULL N;BULLS;COWS;ENTRY;I;INDEX;SOURCE;TARG2;TARGET
[1]  ⍝ M IS NO.OF LETTERS ALLOWED, N IS NUMBER OF VALUES IN TARGET
[2]   SOURCE←M↑'ABCDEFGHIJKLMNOPQRSTUVWXYZ' ⍝ Define available letters
[3]   TARGET←SOURCE[?N⍴M]                   ⍝ Generate target string
[4]   I←1                                   ⍝ Initialise attempt number
[5]  LOOP:'ATTEMPT ',⍕I                     ⍝ Start loop: prompt for entry
[6]   ENTRY←⍞                               ⍝ Enter attempt
[7]   →((⍴ENTRY)≠⍴TARGET)/ERROR             ⍝ Check length of entry
[8]   →(M<⌈SOURCE⍳ENTRY)/ERROR2             ⍝ Check validity of entry letters
[9]   TARG2←TARGET
[10]  BULLS←+/TARG2=ENTRY                   ⍝ Count number of bulls
[11]  →(BULLS=N)/END                        ⍝ Stop if entry is correct
[12]  INDEX←TARG2≠ENTRY
[13]  TARG2←INDEX/TARG2                     ⍝ Remove bulls from target
[14]  ENTRY←INDEX/ENTRY                     ⍝ Remove bulls from entry
[15]  TARG2←+/SOURCE∘.=,TARG2               ⍝ Freq. of letters in TARG2
[16]  ENTRY←+/SOURCE∘.=,ENTRY               ⍝ Freq. of letters in ENTRY
[17]  COWS←+/TARG2⌊ENTRY                    ⍝ Calculate no. of cows
[18]  (⍕BULLS),' BULLS, ',(⍕COWS),' COWS.'  ⍝ Print no. of bulls and cows
[19]  I←I+1                                 ⍝ Increment attempt number
[20]  →LOOP                                 ⍝ End of loop
[21] ERROR:'WRONG NUMBER OF LETTERS'        ⍝ Error: wrong length entry
[22]  →LOOP
[23] ERROR2:'INVALID LETTER(S)'             ⍝ Error: wrong letters entered
[24]  →LOOP
[25] END:'CORRECT! YOU TOOK ',(⍕I),' ATTEMPTS.'  ⍝ Successful completion
```

Error messages are produced (a) if the length of any attempt is not equal to the length of the target (specified by the right argument) and (b) if any letters are used which are not in the allowed list (specified by the left argument).

Let’s see how the game is played.

```
     4 COWBULL 4
ATTEMPT 1
ABCD
0 BULLS, 3 COWS.

ATTEMPT 2
BABC
2 BULLS, 0 COWS.

ATTEMPT 3 DAAC
3 BULLS, 0 COWS.

ATTEMPT 4 DADC
CORRECT! YOU TOOK 4 ATTEMPTS.
```

Our choice of 4 COWBULL 4 produces a vector of four letters chosen from A,B,C,D. Our first attempt, ABCD, produced 0 bulls and 3 cows. In other words, three of the letters are correct, but all are in the wrong place. Clearly one letter must be dropped, and another must be doubled up to take its place.

Our second effort, BABC, is consistent with what we know so far. We have tried dropping D and doubling up on B. The result is very useful - 2 bulls and 0 cows. B was the wrong one to double; there must be at least one D.

The third attempt, DAAC, is consistent with the previous results, and produces 3 bulls and 0 cows. The D must now be in the right place, as must the first A and the C. The only possible solution is now DADC.

If you find this logic a bit trivial, and you have a few hours to spare, try 10 COWBULL 10 or even 26 COWBULL 26.

Finally, for all you programmers, I’ve done the easy bit. With my program you can play the game and the computer keeps score. Now it’s your turn - how about a program in which the computer decides the strategy?

<pre class="BOXED">APL Health Warning: the game COWBULL can be addictive!<br></pre>

(webpage generated: 5 December 2005, 18:45)
