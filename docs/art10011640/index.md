---
vid: '10011640'
title: Chance Misunderstandings
authors:
- Sylvia Camacho
byline: Sylvia Camachosylviac@blueyonder.co.uk
volume: '23'
issue: '1'
page: '121'
description: Vector, the journal of the British APL Association
keywords:
- algebra
- APL
- array
- computation
- computing
- J
- K
- mathematics
- programming
- programming language
- programs
- Q
source: trad/v231/chance.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- mathematics and statistics
---

Sylvia Camacho  
[sylviac@blueyonder.co.uk](mailto:sylviac@blueyonder.co.uk)
{ .byline }

I wrote this article before the Editor pointed out that Devon McCormick had treated the same topic in 21.1 \[[1](#ref1)\]. How could I have missed it? No sign of 21.1 among *my* shelf of Vectors so I went to the obvious hiding place, *Anthony’s* shelf of *Vectors*: Eureka! On reading Devon’s piece, I think our take on the story is somewhat different. He gives a very full account of its history and the psychological ramifications of the game itself: I am interested in the psychological response of one of the mathematicians who had such problems with it.
{ .ednote }

Up to this point, I have spoken of numeracy as if it were an all-or-nothing quality. Either you have it or you don’t. In truth, there are grades of numeracy and even the most numerate have their bad moments. A. K. Dewdney

I first read about the Monty Hall dilemma in Dewdney’s book, *200% of Nothing* \[[2](#ref2)\] and I had to think about his argument for a day or two before I could convince myself that I understood how it works. However, when I tried to pass this fascinating insight on to my family, it led to a kitchen-based altercation which came near to burning the supper. It was a marvellous confirmation of the outraged reaction the story so often meets at first hearing.

It all began in 1990 with a column in an American magazine, written by Marilyn vos Savant, who specialised in answering brain-teasers. She was asked about a TV game show and how she would advise the contestant to act. I came across a typical account of the game in the Spanish version of *Scientific American*, which translates as follows \[[3](#ref3)\]:

> Monty Hall is the presenter of a television competition in the United States. In the last part of the competition Monty shows three chests to a long-suffering competitor. In one of them there is a grand prize and the other two are empty. The competitor nervously selects one of the chests. Monty then puts aside the selected chest and looks slowly and theatrically into the inside of the other two. He closes one of them again, takes the other in both hands and upturns it before the eyes of the competitor and the audience: it is empty.
>
> The competitor sighs with relief although there is no reason to do so. Monty, generously shows him the two chests which remain closed and offers him the possibility of changing his initial decision: “You may now choose either of them,” he announces, as the spotlights brighten. What ought the competitor to do?

Marilyn’s answer, that the competitor is more likely to win if he accepts the offer to abandon his first choice, provoked intense controversy even among mathematicians, some of whom wrote to her in abusive terms.

I was intrigued to come across the story again in Paul Hoffman’s delightful book about Paul Erdös, who is known to some at least of the *Vector* community from Eugene McDonnell’s article in 17.4 explaining Erdös Numbers. There is a conceit among mathematicians in the style of the 1920s music-hall song, “I danced with a man who danced with a girl who danced with the Prince of Wales.” It seems that Roger Hui has an Erdös Number of 2, as he co-authored a paper with a man who had co-authored a paper with the great man himself. Eugene was at first credited with number 3, having written a paper with Roger, but then he was up-graded to 2 by the discovery of a different collaboration. Our Editor tells me that he is an Erdös 4. Never having written a mathematics paper and being wholly unqualified to do so, I am at the bottom of the pecking order with Erdös Number ∞, so reading about his reaction to the Monty Hall dilemma was very reassuring.

It seems that a friend told Erdös of the fuss that the dilemma had caused \[[4](#ref4) p237\]:

> Vázsonyi told Erdös about the Monty Hall dilemma. “I told Erdös that the answer was to switch,” said Vázsonyi, “and fully expected to move to the next subject. But Erdös, to my surprise, said, ‘No, that is impossible. It should make no difference.’ At this point I was sorry I brought up the problem, because it was my experience that people get excited and emotional about the answer, and I end up with an unpleasant situation. But there was no way to bow out, so I showed him the decision tree solution I used in my undergraduate Quantitive Techniques of Management course.”

Vos Savant had used the same technique on her readers but did not succeed in convincing all of them and Erdös was similarly resistant:

> “An hour later he came back to me really irritated. “You are not telling me why to switch,” he said. ‘What is the matter with you?’ I said I was sorry, but I didn’t really know why and that only the decision tree analysis convinced me. He got even more upset.” Vázsonyi had seen this reaction before, in his students, but he hardly expected it from the most prolific mathematician of the twentieth century.
>
> “Physical scientists tend to believe in the idea that probability is attached to things,” said Vázsonyi.

The argument turns upon the exact circumstances of the game: “the total experimental situation,” as Niels Bohr once said. Since *it is a game* one can assume that the chest which Monty shows to be empty, has been *chosen by him* to be an empty one, or the ‘game’ would be over. That being so this is a game of two parts. The competitor’s initial choice has a probability of one-third of winning but that choice *creates a second probability* for the remaining two chests, which together have a two-thirds chance of including the prize. At this point one of these two is eliminated, investing all the two-thirds chance in the other.

<table class="fleft">
					<caption>Choose Box 1 &amp; stick</caption>
					<tr><th>box 1</th><th>box 2</th><th>box 3</th><th>outcome</th></tr>
					<tr><td>prize</td><td></td><td></td><td>win</td></tr>
					<tr><td></td><td>prize</td><td></td><td>lose</td></tr>
					<tr><td></td><td></td><td>prize</td><td>lose</td></tr>
				</table>

<table>
					<caption>Choose Box 1 &amp; switch</caption>
					<tr><th>box 1</th><th>box 2</th><th>box 3</th><th>outcome</th></tr>
					<tr><td>prize</td><td></td><td></td><td>lose</td></tr>
					<tr><td></td><td>prize</td><td></td><td>win</td></tr>
					<tr><td></td><td></td><td>prize</td><td>win</td></tr>
				</table>

It is the change of probability, consequent upon the opening of the box, which is difficult for some to accept. Unless the contestant sees it chosen by Monty and that it is empty, he will have no reason to alter his original choice. Vos Savant drove this lesson home by pointing out that an outsider who was not present at the game, if asked to assign probabilities for a prize to be in either of the two boxes still closed, could necessarily only assume the odds to be even, 50:50. The dilemma only arises if Monty’s intervention is seen to be deliberate and part of his game plan. This makes it a psychological tease, but the lesson that probability is never a property of objects, but only of the changing situations in which they are placed, can cause confusion even when the pertinent situation *does not change*. Eugene Northrop published a little ‘book of paradoxes’ \[[5](#ref5)\] in 1944, from which the following example is taken.

Imagine three closed purses containing respectively two gold coins, two silver coins and one with one gold and one silver. If we choose a purse at random we can agree that the chance that it holds the mixed coins is one-third. If we take a coin from the selected purse we can agree that it will be either like the second coin or unlike it. So can we say that the chance is one-half that the one we removed differs from its companion? If this is what we think, does that mean that removing a coin has raised the chance of having selected a mixed pair, from one-third to one-half? Of course not, merely transferring the coin from the purse to the hand changes nothing: the probability of having selected unlike coins, given the total experimental situation, is still one in three. Moreover, if we look at the coin in our hand and find it is gold, we have a one-in-three chance that the second coin in our selected purse is also gold, although there are only two gold but three silver still hidden from us.

In both of these puzzles we are having to consider how objects are grouped. In both we start with three equal possibilities. Monty Hall then creates two groups. The group of one has a one in three chance of holding the prize. In the group of two each starts as a one-in-three chance, making two thirds in total, which does not change when one is shown to be empty. In the Northrop puzzle there are three groups of two but the possibilities associated with the other two groups must be disregarded after the initial choice. This still leaves three possibilities for the contents of the chosen purse; gold-gold, silver-silver, gold-silver. So in the Monty Hall case the probabilities must be re-calculated after the initial choice but in the Northrop case they cannot be.

Those who know my prejudices will realise that I am still pursuing my objections to the assumption behind Bell’s Inequality \[[6](#ref6)\], that probability is a fixed property of the phenomena under test and does not change with the changing experimental situation.

Erdös was unconvinced by the simple decision table but reluctantly accepted that vos Savant was right after Vázsonyi ran a Monte Carlo simulation on his PC. I have never written J, so as an exercise I have appended some code to run a succession of randomised games and collect the statistics. I know that some of my readers will wince and convert my crude attempt into an elegant form more worthy of Erdös: “a proof from the Book”, as he would have said. My amateur J code ran an iteration of one hundred thousand simulated Monty Hall games in ten seconds on my 600MHz Pentium III:

```
   play 100000
Total Games Played: 100000
        switch     no switch
win       33379      16516
lose      16613      33492
sum       49992      50008
% wins       66         33

l1=: 40{.' Total Games Played: nnnnnn'
l2=: 40{.'          switch     no switch'
l3=: 40{.' win      nnnnnn     nnnnnn'
l4=: 40{.' lose     nnnnnn     nnnnnn'
l5=: 40{.' sum      nnnnnn     nnnnnn'
l6=: 40{.' % wins       nn         nn'
play=: 3 : 0
x=: 0 0 0 0
games (^:y.) 0
l1=:(6.0":y.) (21+i.6)}l1 NB. total games
l3=:(6.0":2{x) (10+i.6)}l3 NB. switch & win
l3=:(6.0":1{x) (21+i.6)}l3 NB. no switch & win
l4=:(6.0":3{x) (10+i.6)}l4 NB. switch & lose
l4=:(6.0":0{x) (21+i.6)}l4 NB. no switch & lose
l5=:(6.0":(2{x)+3{x)(10+i.6)}l5 NB. total wins
l5=:(6.0":(1{x)+0{x) (21+i.6)}l5 NB. total losses
l6=:(2.0":<.100*(2{x)%(2{x)+1{x)(14+i.2)}l6 NB.% if switch
l6=:(2.0":<.100*(1{x)%(1{x)+0{x)(25+i.2)}l6 NB.% if no switch
stats=: 6 40$l1,l2,l3,l4,l5,l6 NB. display results
)
games=: 3 : 0
p=.1?3 NB. assign prize to box 0,1 or 2
c=.1?3 NB. contestant chooses 0, 1 or 2
s=.1?2 NB. contestant chooses stick=0 or switch=1
n=.#.s,p=c NB. n is outcome 0,1,2,3
x=: x+0 0 0 0 (n}~) 1 NB. accumulate wins & losses
)
```

### References

1. <a id="ref1"></a>Devon McCormick, “Further Surprises involving a Man and a Goat: The ‘Monty Hall’ Problem Solved”, *Vector*, 21.1 pp85-89
2. <a id="ref2"></a>A.K.Dewdney, *200% of Nothing*, 1993, John Wiley & Sons Inc.
3. <a id="ref3"></a>*Investigación & Ciencia*, Prensa Científica, Barcelona, Dec 2001, ISSN 0210136X
4. <a id="ref4"></a>Paul Hoffman, *The Man Who Loved Only Numbers*, 1998, Fourth Estate Ltd.
5. <a id="ref5"></a>Eugene P. Northrop, *Riddles in Mathematics*, 1960, Pelican Books
6. <a id="ref6"></a>Sylvia Camacho, “How Wrong Was I?”, *Vector* 21.2 pp78-90
