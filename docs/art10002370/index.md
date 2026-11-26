---
vid: '10002370'
title: 'Jot-Dot-Min: Weaving Patterns and Cellular Automata'
authors:
- Ian Clark
byline: by Ian Clark
volume: '12'
issue: '4'
page: '20'
source: trad/v124/clark124_20.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1996
tags:
- programming techniques
- graphics
- education
---

by Ian Clark
{ .byline }

In a previous JDM I let slip that one of the joys of APL for me was replacing loops by bit-arrays. Bitting instead of knitting, you might say. Throughout the ages philosophers have built calculating engines, talking heads and other logical machines, but the real father of commercial data processing isn’t any of these – it’s the Jacquard Loom, an engine which can weave beautiful intricate pictures in silk like the one I possess of the Kinkakuji Temple in Kyoto. Visitors think it is a photograph, until they examine it with a magnifying glass. The loom is driven by a deck of punched cards and considerably predates the Hollerith punched card popularised by IBM before electronic computers came along. Joseph Marie Jacquard produced his design in 1805; it was in some sense just a mechanical improvement on Basile Bouchon’s 1725 concept, which draws an endless loop of perforated slats across the tops of the so-called needles which raise each individual warp thread, letting some rise and others fall.

You can illustrate the principle like this. Let WARP stand for the threads and CARD stand for a card (or a Bouchon slat). Choose a string of 25 pretty APL symbols to stand for the different coloured threads (if you’ve never used them all, now’s your chance):

```
     WARP←'⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟'
```

Make CARD a string of 25 0s and 1s, as many elements as there are in WARP. A quick way to do this is to generate them at random by putting ? in front of a vector of 25 2s. But this ends up with a string of 1s and 2s. So we make a comparison out of it (a so-called ‘logical’ expression) by putting 1&lt; in front. That gives us a sequence of FALSE and TRUE results, which happen to be 0 and 1 in APL.

```
      CARD←1<?25⍴2
      CARD
0 0 0 1 0 1 0 1 1 1 1 1 0 1 0 0 0 1 0 1 0 0 0 1 0
```

or something like that (it will be different each time).

Now we can apply CARD to WARP to raise all those threads where CARD is 1 and lower them where CARD is 0. The raised threads show on top of the cloth, the lowered threads get hidden underneath.

```
       WARP
⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟
      CARD\CARD/WARP
   ⍞ ⍎ ⍬⍞⍟⍎⍕ ⍞   ⍬ ⍟   ⍞
```

CARD/WARP would simply knock out the 0-positions and close up the gaps. CARD\\... puts the gaps back again.

Let’s make a function out of the expression which makes CARD (just cut and paste from the session, if your APL lets you). We call it: nextcard, because it acts like it’s getting the next card in the endless belt. This function is clever enough to count the number of threads in WARP so that you don’t have to fix it at 25.

```
      ∇nextcard
[0]   nextcard
[1]   CARD←1<?(⍴WARP)⍴2
      ∇
```

Now there must be loads of ways of turning a sequence of numbers (or characters, or threads, or anything!) into a sequence of 0s and 1s. That’s the principle of Bitting. We like it the way we’ve shown because you can tinker with the numbers 1 and 2 to get any proportion of 1s and 0s you like in the random stream. How? Just try some examples...

```
            ,CARD←5<?25⍴10
1 0 1 0 1 1 0 0 1 1 0 1 1 0 1 1 1 0 0 1 0 0 0 1 0
      ,CARD←1<?25⍴10
1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 0 1
```

Get the idea? It’s like tossing a weighted coin. The ‘weight’ is that first number, 1 or 5, in the expression.

The comma in front is just a lazy way to kid APL it’s computing an uncommitted expression (which it outputs) rather than an assignment (which it doesn’t). It simply saves us typing CARD each time. Be careful what you use it on, because it strings the result out flat.

Now we write a short function to behave like the working loom:

```
      ∇weave
[0]   weave
[1]   nextcard
[2]   CARD\CARD/WARP
[3]   →1
      ∇
```

It’s a good use for a loop here (uncommonly, for APL). As you see, the function:

1. Gets the next card CARD
2. Applies it to WARP to weave a row of cloth
3. Goes back and does it again

So here we go:

```
      weave
⍎  ⍞⍟⍎⍕  ⍟⍎ ⍬⍞⍟⍎⍕ ⍞⍟
⍎ ⍬⍞⍟⍎⍕⍬  ⍎   ⍟  ⍬  ⍎⍕⍬ ⍟
⍎⍕ ⍞⍟⍎⍕⍬⍞  ⍕⍬ ⍟⍎⍕ ⍞⍟⍎ ⍬⍞
⍎⍕⍬⍞⍟ ⍕⍬⍞   ⍬  ⍎ ⍬⍞  ⍕  ⍟
   ⍞ ⍎ ⍬⍞⍟⍎⍕ ⍞   ⍬ ⍟   ⍞
...
```

Press the Interrupt key (&lt;ctrl+break> perhaps, or &lt;esc>) when you’ve had enough. Then type a branch to clear out the suspended function:

```
      →
```

There are cleverer ways to stop a function, but they just clutter the discussion here.

Now let’s change the pattern of the cloth it weaves. If we change the function: nextcard, that’s like changing the endless belt on the loom. Make up a collection of ‘nextcard’ functions, nextcard1, nextcard2... and alter the function nextcard to simply run any one of them:

```
      ∇nextcard
[0]   nextcard
[1]   nextcard2
      ∇
      ∇nextcard1
[0]   nextcard1
[1]   CARD←¯1+?(⍴WARP)⍴2
      ∇
      ∇nextcard2
[0]   nextcard2
[1]   CARD←1⌽CARD
      ∇
```

So, to change the endless belt on the loom, edit nextcard to slot in a different nextcard\* function. The version: nextcard2 is a bit of a cheat, because it needs a valid result in CARD to work, which you can get by running nextcard1 once. It simply alters the existing pattern by ‘rotating’ CARD, ie. moving the first element to the end. Here’s what it looks like with nextcard2...

```
      weave
  ⍬ ⍟ ⍕⍬⍞⍟⍎ ⍬   ⍕ ⍞   ⍬
 ⍕ ⍞ ⍎⍕⍬⍞⍟ ⍕   ⍎ ⍬   ⍕
⍎ ⍬ ⍟⍎⍕⍬⍞ ⍎   ⍟ ⍕   ⍎
 ⍕ ⍞⍟⍎⍕⍬ ⍟   ⍞ ⍎   ⍟    ⍟
⍎ ⍬⍞⍟⍎⍕ ⍞   ⍬ ⍟   ⍞    ⍞
 ⍕⍬⍞⍟⍎ ⍬   ⍕ ⍞   ⍬    ⍬ ⍟
⍎⍕⍬⍞⍟ ⍕   ⍎ ⍬   ⍕    ⍕ ⍞
⍎⍕⍬⍞ ⍎   ⍟ ⍕   ⍎    ⍎ ⍬ ⍟
⍎⍕⍬ ⍟   ⍞ ⍎   ⍟    ⍟ ⍕ ⍞⍟
⍎⍕ ⍞   ⍬ ⍟   ⍞    ⍞ ⍎ ⍬⍞⍟
⍎ ⍬   ⍕ ⍞   ⍬    ⍬ ⍟ ⍕⍬⍞⍟
 ⍕   ⍎ ⍬   ⍕    ⍕ ⍞ ⍎⍕⍬⍞⍟
⍎   ⍟ ⍕   ⍎    ⍎ ⍬ ⍟⍎⍕⍬⍞
   ⍞ ⍎   ⍟    ⍟ ⍕ ⍞⍟⍎⍕⍬ ⍟
  ⍬ ⍟   ⍞    ⍞ ⍎ ⍬⍞⍟⍎⍕ ⍞
...
```

(Once again, press Interrupt when you’re tired of it.)

Well, it’s not the Kinkakuji Temple, but Rome wasn’t built in a day. Read on...

Not long after the Jacquard loom revolutionised weaving (and nearly got its inventor drowned in the river Rhône by furious Lyonnaises, who thought it would put them out of work) the theory of Cellular Automata arose in mathematicians’ minds. Don’t let anyone tell you that mathematicians dream up new things all the time. Often they just sweep up the philosophical droppings of technological innovation. The original challenge was this (the Napoleonic Wars not long over): imagine a line of musketeers in the mist. Each man is only able to communicate with his two neighbours, which he does once a second. Each man is an automaton and goes into a different state as a result of giving or getting messages. Thanks to parade-ground drilling, all the automata are identical. (Remember that the drill for loading and firing a musket had something like 131 steps, so the National Curriculum had nothing on the common soldier.) Devise a system of messages and states which allows the line to get messages from one end, and this to result (several seconds later) in them all firing at the same time.

Now this problem is actually quite difficult. At least it is if you aren’t allowed to build in the number of soldiers into the system, or let the end-soldier recognise he is at the end. But just to show how you might tackle the problem, let’s give each soldier only two states, called 0 and 1, and a repertoire of one message which he sends exclusively to the soldier on his right. He switches to his other state when he receives the message, and he sends the message when he’s in state 1 and has just been switched to state 0. Speak up at the back there... Yes, we’ve just turned him into a Binary Flip-Flop and constructed a Shift-Register, but you’re in the wrong class, my lad.

Now let’s be clever and use our Jacquard loom to model the situation. Here’s the ‘program’ to do it, called nextcard3. But now it’s not the ‘next card’ but the ‘next state’ for each of the soldiers, i.e. what’s to happen one second later. People who write simulations call this a ‘discrete simulation’ and the step of one second they call an ‘epoch’.

```
      ∇nextcard3
[0]   nextcard3
[1]   CARD←CARD≠(¯1↓0,CARD)
      ∇
```

Not-equals behaves like ‘exclusive-or’ in this context, or ‘non-carrying addition’ as the pioneers used to call it. Line \[1\] says: “Add CARD to itself shifted along one position using non-carrying addition”. That corresponds to each soldier passing the message to the right.

Now let all the soldiers start in state 0...

```
      CARD←25⍴0
      weave
```

And there they’ll stay... until we give the first soldier the order (i.e. the one message he understands), by setting CARD to 1 for that position.

```
?(...endless blank lines, until you press Interrupt...)
...
weave[2]
```

So give the first soldier the order and re-start the simulation...

```
      CARD[1]←1
      →⎕LC
⍎
⍎⍕
⍎ ⍬
⍎⍕⍬⍞
⍎   ⍟
⍎⍕  ⍟⍎
⍎ ⍬ ⍟ ⍕
⍎⍕⍬⍞⍟⍎⍕⍬
⍎       ⍞
⍎⍕      ⍞⍟
⍎ ⍬     ⍞ ⍎
⍎⍕⍬⍞    ⍞⍟⍎⍕
⍎   ⍟   ⍞   ⍬
⍎⍕  ⍟⍎  ⍞⍟  ⍬⍞
⍎ ⍬ ⍟ ⍕ ⍞ ⍎ ⍬ ⍟
⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎⍕⍬⍞⍟⍎
⍎               ⍕
⍎⍕              ⍕⍬
```

...etc.

Very pretty. A Binary Tree. Still not the Kinkakuji Temple, but we’ve only just started. Computers themselves may have started with Jacquard looms, but what nobody tells you is that a Jacquard loom with self-generating cards could in principle handle any computing task. Tripping over boxes and boxes of printout at my old place of work, I came to that conclusion myself, and I’m not Alan Turing. If they had only used a loom and some silk instead of forests of paper and a lineprinter, it could have made someone a nice kimono.

A less well-known fact about Alan Turing was that he was an expert at knitting (as well as bitting). He once got the whole department at Manchester trying to knit a Riemann Surface. This piece of intelligence was told me by my mother-in-law who worked there as a Computer.

No joke! ‘Computer’ was a job-title, like ‘navvy’, until the steam-navvy came along. Ma-in-law was an expert knitter too, but she never went as far as drowning Alan in the Mersey. Meanwhile, back at the Battle of Waterloo, if you want to give your soldiers more than 2 states, as you’ll need to, you’ll have to replace the bit-string: CARD with a string of integers and make the loom a little more elaborate. I’ll publish the most interesting solution I receive in a future issue.

But until then, as you play with the loom, remember... you are experimenting with Cellular Automata, the forerunner of the fashionable Neural Network. Yes, if you consider each of your soldiers to be a neurone (a nerve cell), it can ‘fire’ (i.e. send a message) when it’s leaving a particular state, and its next state depends solely upon its current state and the messages it receives from other neurones.

In 1968, in my first job and faced with my first computer (an IBM 1130 – remember it? – and it sported APL, too!) I wrote a simple neural network simulation. Then I sat and stimulated it with the sense-switches. (Why doesn’t my PC have sense-switches any more?) I was fascinated by the wallpaper that came off the lineprinter. Young as I was, I even wondered eerily if it might be ‘thinking’ in there, and whether it might be wrong to switch it off. Happily a perusal of the printout showed the activity gradually descending from conscious to comatose, so I reluctantly switched off the machine and caught the late train home.

You think I’m being funny? What if I had been an embryologist?

In the classical situation, each cell can only receive messages from next-door neighbours. In animals, this is probably only true for jellyfish (which have neurones almost like ours). The more advanced applications of neurones in the animal world collect bundles of fibres into cables for making trunk calls to distant parts of the network. This is the ‘white matter’ beneath the ‘grey matter’ of the human brain.

But there’s plenty of mileage in a nearest-neighbour topology. If you abandon bit-strings in favour of bit-squares, you can play Conway’s Game of Life (which we’re coming to in the next issue). And of course, if you go to higher dimensions in the form of an n-dimensional hypercube, you can make every one of n cells a next-door neighbour of every other...

```
      n←15
      z←(n⍴2)⍴1
```

Or you can put 3 instead of 2 and try developing cellular automata to play Tic-Tac-Toe.

How high does your version of APL let n go until it complains: LIMIT ERROR? Dyalog APL lets n go up to 15. The celebrated Connection Machine goes up to 32767, I understand. Just imagine 32767-dimensional Tic-Tac-Toe. You’d have to have your wits about you.

(webpage generated: 8 February 2007, 04:44)
