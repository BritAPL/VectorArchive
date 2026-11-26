---
vid: '10002310'
title: 'Jot-Dot-Floor: Working with Large Integers'
authors:
- Ian Clark
volume: '11'
issue: '1'
page: '31'
source: trad/v111/clark111_31.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- education
- mathematics and statistics
- programming techniques
---

\-- webpage generated: 18 October 2006, 03:50 using: VARCH v1.0 1 VARCH43 clark111\_31 -->

## Jot-Dot-Floor: Working with Large Integers

by Ian Clark, <span class="affil">.</span> <span class="email">(<a href="mailto:earthspot2000@hotmail.com">earthspot2000@hotmail.com</a>)</span>

The National Curriculum is Dead – Long Live the National Curriculum. A consultation process now begins, from which whatever emerges may bear little resemblance to the contents of the set of white folders that everyone in the educational trade in England, and not a few teachers, have zealously collected and studied. There’s hardly an educational supplier who hasn’t got the existing structure of Key Stages and Levels of Attainment off backwards, at least in their own subject, because unless a supplier could state exactly what boxes in a pupil’s grid of AT levels could be ticked-off as a result of the pupil running the product, he hadn’t a ghost of a chance of selling to British schools. The NC defined a market which was created by an Act of Parliament, just like vehicle insurance or personal pensions. In today’s Britain the best sort of market to be in is one in which people have to buy your product by law, because nobody has any money to buy anything else.

Someone once buttonholed me and said wouldn’t it be nice if EV could publish some classwork, running on almost-free software like I-APL, which a teacher could simply set before the class and then tick off the boxes on the attainment-tracking grid – or, better still, have I-APL do it automatically. Dream on, MacDuff. However one good thing about the now-obsolete NC (1991) Order documents was that they gave some neat projects, or “programmes of study” as they called them, plus examples, to meet the so-called “statements of attainment”. In future issues of EV we shall discuss these examples in the Mathematics and Science curriculum in the idiom of I-APL. This will go some way towards MacDuff’s Dream, or would do if they weren’t going to change it all. Another teacher tells me I couldn’t choose a worse time to run a series on the National Curriculum, but is it? Presumably the new NC will only be a delta-change on the old one (a minus-delta one hopes!) so a discussion of the existing one will not be completely wasted. And there’s also the promised consultation. Ah, well.

Another subject of perennial interest to mathematics teachers it seems are prime numbers. A paper accepted for APL’94 is entitled “The First One Hundred Million Prime Numbers and APL with Applications in Public Key Cryptography” by Prof. Henriques. It seems that the purer the mathematics the spookier its applications nowadays. Ted Emms article “A Note on Primes” (Education Vector 10.4) has prompted a contribution by Guido Leeten of an on-line investigation into complex integer primes, i.e. numbers of the form aJb where a and b are integers. Just the stuff for a Key Stage 4 project. Here’s some likely questions: If a and b are primes, is aJb? And vice-versa? Is prime factorisation still unique? Does the set of complex integer primes contain the real primes? Watch out for Mr Leeten’s researches in a future issue.

One of the things Mathematica claims to do well is handle very large numbers. Most computer languages top-out at 2\*64 and APL is no exception. In fact I-APL (our pet APL interpreter in EV) only gives you 10 significant figures. Which puts a ceiling on classroom investigations – or so you might think... (shush, experts, I’m not talking to you). However, whole numbers don’t really get interesting until they get big. So what do we do? Trash I-APL and go and spend £450 or so on Mathematica? Not at all – at least not for that reason. Here’s a cautionary tale from the distant past.

A certain Chinese sage once performed a signal service for his emperor and was invited to name his price, provided the Lord High Treasurer agreed. Now the LHT was a stingy chap, so calling to mind the unfortunate experience of his friend the Pied Piper, the sage took a chessboard and set one grain of rice in the first square, two in the second square, four in the third square and 8 in the 4th. He then invited the LHT to fill the remaining squares, doubling the previous square each time – and the total would be his fee. The LHT, not possessing his own copy of I-APL, promptly agreed, and lived (though not very long) to regret it. The moral is twofold: (1) get your copy of I-APL without delay, and (2) don’t sign any deal until you’ve costed it in real figures.

How much rice did the sage ask for? A binary numeral consisting of 64 ones, or (using I-APL) `64/1`. Each one represents a square, and of course each successive one is worth double the previous. What’s that as a real number?

`2⊥64/1 1.844674E19`

which is more than all the rice in China, even at a milligram a grain.

But wouldn’t it be nice to see all 20 digits of that. Especially by a general procedure that handles numbers up to the limit imposed by workspace capacity.

Define G as

```
      G:⍎'Z←Z ADD10 Z'
```

Then running G 64 times starting with `Z←1` gives one more than the required number, as a vector of decimal digits: `1 8 4 4 6 7 4 4 0 7 3 7 0 9 5 5 1 6 1 6`

Why one more than the required number? Well, what you see at each iteration is the number of grains on the next square. The first square has `1=2*0` grains, the second has `2*1`... and the 64th has `2*63`. But the sage has submitted an expense claim for all of these added together, which just so happens to be `(2*64)-1`. Why? Because when you add 1 to a binary number 111...1 all the bits flip over and you get 1000...0.

Also it is well known (to those of us smart enough to add up a series) that for any X besides 2 and (natural) N besides 64 the series: `+/X*⍳N` sums to: `(1-X*N)÷(1-X)` provided `⎕IO=0`. Write these expressions into a function:

```
     ∇ Z←X TRY N;⎕IO
[1]   ⎕IO←0
[2]   (1-X*N)÷(1-X)
[3]   +/X*⍳N
```

and try different values of X and N. Does it work for fractional X? I’m not so dumb as to ask if it works for fractional N.

Come to think of it – and APL is meant to be a tool of thought – why doesn’t it work for fractional N? Anybody care to try developing a theory of fractional series? If Mandelbrot can have fractional dimensions, why not? It would be the biggest thing in Functional Analysis since infinite series.

Meanwhile, back at the nursery, here’s a fairly robust version of ADD10:

```
     ∇ADD10[⎕]∇
    ∇ Z←X ADD10 Y;CARRY;R
[1]   ⍝ adds 2 decimal vectors treated as long integers
[2]   R←¯1↑1,(⍴X)⌈⍴Y ⍝--the greater length (never null)
[3]   Z←⌽(R↑⌽X)+(R↑⌽Y) ⍝--the straight sum
[4]   →(~∨/CARRY←(10≤Z),0)/0 ⍝--stops recursion if all CARRY=0
[5]   Z←(10|Z) ADD10 (~1↑CARRY)↓CARRY ⍝--drop leading 0 from CARRY
    ∇
```

also (to save you entering G sixty-four times) a function REPT to be used like this: `'G' REPT 64`

```
    ∇ ZZ REPT ZM;ZI
[1]   Repeats given line ZM times
[2]   ZI←0
[3]  LOOP:→(ZM<ZI←ZI+1)/0
[4]   ⍎ZZ
[5]   →LOOP
    ∇
```

Is there anyone out there using Macintoshes in class? Send me a blank disk with an SAE (or the cash equivalent) for a copy of my big-number workspace, plus a copy of IAPL/Mac to run it. PC users can be accommodated too (the workspaces are byte-compatible).

How does ADD10 work? Let’s simply say that it adds the two numerals X and Y (having made them the same length). This ends up with a vector Z possibly containing “digits” greater than 9, but  `10|Z` fixes that, and `CARRY←(10≤Z),0` computes the ones to carry. ADD10 then calls itself to add CARRY into Z and that’s all there is to it.

How’s your long multiplication? Anybody want to try coding MUL10? MUL2 is a bit easier (pun intended) and can be written to illustrate how the computer does it, especially if you use logic primitives instead of addition. Say, wait a minute! I’ve got AND, OR, NOT, NAND and NOR on the keyboard, but where’s that vital logic function XOR, or non-carrying addition, as it used to be quaintly called?

You know, for years I didn’t know the answer to that. At APL parties I’d blush and bashfully change the subject. Then some kind friend whispered “Have you tried not-equals?”. And the moral to that is: it’s all there on the keyboard – somewhere (if you can find it). The chaps who developed APL weren’t born yesterday. Last half-century maybe...

As for finding things on the keyboard, my PowerBook has APL.68000 stickers on its keys. I do get some dirty looks during customer visits. It’s almost as bad as sporting a baby’s rattle with satanic symbols on it. Did you know that you can get a flexy transparent keyboard cover for most major PCs? It fits snugly over and between the keys and you simply type through it. It’s meant to protect the keyboard from dust, but it’s marvellous for sticking APL symbols on. When you leave the computer you peel it off and nobody knows your wicked secret. For a blank cover to stick your own symbols on (price £9.50) phone HCS Global Computer Supplies on 0800-252252, quoting stock-numbers 5801 onwards for IBM keyboards.

Practise Safe Keyboard-Entry, I say. Actually IAPL/Mac doesn’t need one. There’s a little on-screen palette you click for the symbol. Hold down \{option\} and it tells you what the symbol does, too. Using IAPL/Mac I haven’t typed an APL symbol for years (other vendors please copy).

While on the subject of keyboard entry, isn’t it amazing how vulnerable computers still are to misspelling, especially of surnames. I still bifurcate on mailing lists (very painful!). Years ago the Readers Digest was reputed to have one of the best weighted-matching systems around for reconciling duplicate names and addresses. Other good ones were owned by the Police National Computer and the Mormon Church. Most of these were adaptations of the Russell-Soundex system, invented around the turn of the century for USA census work, I believe. See also Sylvia Camacho’s article, “What’s in a Name?” (VECTOR 10.4, pp78-82). Now the Levenshtain Distance is a measure of how close two words are in spelling. It is defined as the minimum number of single-letter insertions, deletions or substitutions needed to turn one word into another. It would make a good fuzzy-match APL function to handle misspelt words, if you can program the algorithm efficiently (all the methods I’ve come across find the shortest diagonal route across a grid). Send solutions to me, or functions for slightly different metrics which would serve almost as well.

Ian Clark, alias Clarke.

(webpage generated: 18 October 2006, 03:50)
