---
vid: '10002400'
title: Analysing Blackjack with APL (Finnish Forests Seminar)
authors:
- Ian Clark
byline: by Ian Clark
volume: '16'
issue: '4'
page: '68'
source: trad/v164/bjack164.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2000
tags:
- applications
- mathematics and statistics
---

*by Ian Clark*
{ .byline }

*(presented at FinnAPL conference: APL Techniques and Cases, Imatra, 10-11 Feb., 2000.)*

## Introduction

In this talk I shall demonstrate an APL engine called JACK, a workbench to analyse the casino game of *blackjack*. Roulette aside (a game of little interest to intelligent players), blackjack has the most glamour of any casino game, because (unlike roulette) it is claimed to be a game of skill. It is wrapped about with mythology. One of the myths is that the game is beatable, i.e. that it is possible to make a steady profit playing the game, and that some professional gamblers actually do make their living at it.

My purpose in writing JACK was to investigate whether this myth might not contain an element of truth. The verdict? Yes! But...

## Gambling or Investment?

There is a fine line between *gambling* and *investment*. It boils down to asking: what are the odds? 50%? 51%? 49%? An *investor* bets on favourable odds, like 51%. A *gambler* (or a *fool*), by our definition, bets on unfavourable odds like 49%. We shall discuss *investing* in the casino game of blackjack, not *gambling*.

Many people who speculate in the commodities market don’t think of themselves as gamblers, yet few of them really know the odds in favour of their stake rising in value. There are a lot of hidden factors, events outside one’s control waiting to happen, or having happened already! Wars, the weather, what the largest traders are planning to do. What may seem a reasonable speculation may in actual fact be a pretty poor gamble. The advantage of investing in a casino game rather than in commodities trading, futures or options, is that casino odds can be precisely calculated. And everything that can possibly influence the outcome is there on display at the gaming table.

## The meaning of ‘odds’

What is meant by *odds*? This is a gambler’s measure of the chance of a *win*. If you are quoted ‘odds’ without further qualification, then by convention they are always in your favour. A further convention is that if you win, you get your original stake back doubled. If you lose, you lose only your stake. This is important to remember in some casino games which pay a bonus of more than your stake for certain outcomes (e.g. a Jack plus an Ace in blackjack usually pays 3 to 2) – the odds calculated on the simple chance of a win must then be adjusted (upwards) to allow for this.

Gamblers invented the Theory of Probability, but nowadays probabilists and statisticians use a different measure of chance called the *probability*, a value between 0 and 1. Odds of 50% are called *evens*, and correspond to the probability of a win of 0.5. But odds>50% need some caution when interpreting what they mean.

Odds of 51% (also called an *edge* of 1%) mean that you win 51 out of 100 unit bets (professional gamblers always bet in fixed units). Thus you lose 49 units, but double 51 units. So your 100 units have turned into 102, equivalent to a 2% return on investment. This isn’t bad for a day’s work – in nice surroundings too. Mind you, this is an ‘expected’ return. There’s a small but maybe significant chance that you’ll lose the lot! This is technically called *ruin*.

## Blackjack is hugely profitable

In Las Vegas, thanks to the gaming industry, they can afford to pull down existing hotels (‘resorts’ in USA terminology) and build new ones like Bellagio’s, costing $1.6Billion! Casinos are hugely profitable. But they argue (with some justification) that this profit is fairly earned. Las Vegas may have been founded by gangsters, but nowadays all its casinos are run by large corporations in a well-regulated and highly competitive industry. Casinos compete by offering the gambler slightly better odds than their competitors by adjusting the rules of the game. In fact Las Vegas casinos offer the best odds you’ll find anywhere in the world, and the tables are open round the clock. If you’ve money to invest in blackjack, it’s probably a waste of it to go anywhere else.

But of course casinos won’t offer better odds than 50% because then *their* odds would be worse than 50% and they aren’t fools. In Las Vegas, the odds offered to the *good* player are typically around 49.5% for blackjack, and are much the same for other games, i.e. 0.5% below evens. This 0.5% is called the *house edge*, and is justified as their legitimate profit. Of course, a hunch player might be giving the house an extra 5% to 10% edge thanks to sub-optimal play. This is what generates the enormous profits, especially in blackjack, one of the most profitable of casino games.

## Basic Strategy

The *good* player in blackjack employs what is called *Basic Strategy* (BS), which is to choose your available call option purely on the basis of your two cards dealt, and the dealer’s one card showing. Not on your hunch for what is going to turn up. So BS is defined in principle by a matrix of your hand’s total value, versus the dealer’s card, each cell prescribing your call option (Hit, Stand, Double-Down or Split). Here’s part of it as a sample:

```
Hit or Stand? (S=Stand, H=Hit)

Dealer's card: 2  3  4  5  6  7  8  9 10  A
               ----------------------------
Your hand: 17: S  S  S  S  S  S  S  S  S  S
---------- 16: S  S  S  S  S  H  H  H  H  H
---------- 15: S  S  S  S  S  H  H  H  H  H
---------- 14: S  S  S  S  S  H  H  H  H  H
---------- 13: S  S  S  S  S  H  H  H  H  H
---------- 12: H  H  S  S  S  H  H  H  H  H
------- Ace+7: S  S  S  S  S  S  S  H  H  H
--------Ace+6: H  H  H  H  H  H  H  H  H  H
```

Most published versions of the BS matrix have been computed by Monte-Carlo simulation (JACK can do this too), consequently they all agree, except in one or two places where the decision is borderline. Such as: do you hit soft-17 (ace+6) against the dealer’s ace, or do you stand? Here the actual house-rules (which vary from casino to casino) affect the outcome. This is an openly publicised technique—you can buy little plastic cards summarising the matrix in the casino shops. And the casinos declare this to be perfectly acceptable—although you are supposed to memorise the matrix and not consult it at the gaming table! It cuts the house edge to around 0.5% as stated. But of course a casino is never willingly going to play against you if the house-edge goes negative. By our definition of gambler/fool, they’d be fools to do so.

## Why the game can be beaten

Be that as it may, it is actually possible to ‘beat the game’, that is, to play in such a way that the house edge is whittled away to nothing and actually goes negative. BS is based on the expectation (expected value) of the next card dealt, assuming a perfect shuffle and *ignoring what cards have gone before*. And it says nothing about how much you should bet (which you place before you have seen any cards dealt). According to BS, your chances don’t vary as the pack is dealt, because they are pre-computed *a-priori* chances.

Now in blackjack the suit is disregarded, all picture cards score 10, and an ace scores 11 (or 1, if the total hand would otherwise exceed 21). Thus in a standard deck there are 16 cards of value 10, plus 4 aces, totalling 20 cards out of 52. The blackjack pack consists of one or more entire decks, 6 is commonest, but you can find 4-deck, 2-deck or even 1-deck games. So in any pack:

- 38.46% (20 out of 52) of the cards are *high* (ace or 10)
- 46.15% (24 out of 52) of the cards are *low* (2 thru 7).

If, in the cards still to be dealt, the percentage of highs goes above 38.46, or the percentage of lows goes below 46.15, then the dealer is more likely to lose by busting (dealing himself or herself a hand totalling more than 21). This is because high cards tend to bust the dealer, but benefit you, provided you stand of course (JACK can verify this). This effectively reduces the house edge, sometimes by as much as 5%-10%, until the cards are shuffled once more. By counting cards you can place only the minimum bet (typically $5 in Las Vegas) until you detect when the house edge drops below zero, and then bet high. Then you are not a gambler, you are an investor.

But don’t bet *too* high. The odds are still in the neighbourhood of 50%, so you will lose almost as often as you win. A player playing an arbitrary game of chance with unvarying bets against odds of 50% will find his *purse* executing a *drunkard’s walk*, a statistical term for a process whereby an object wanders at random from its original position. Brownian motion is an example of drunkard’s walk. The position after n steps is a random variable, normally distributed as n tends to infinity (according to the Central Limit Theorem). The upshot for blackjack is that if your *maximum* bet is 0.01 of your purse, the chance of *ruin* (losing your purse) is around 1 in 28. If it is 0.02 of your purse, the chance of ruin is around 1 in 5. JACK has functions to investigate and demonstrate this effect.

Players who count the cards as they are dealt are called *counters*. This is something quite distinct from being a BS player. BS determines your call: hit or stand, etc. The count determines how much you bet, which has to be placed before any cards are dealt. But of course good counters play BS too, to minimise their losses when the house edge happens to be 0 or positive (which is the case approximately 70% of the time). In this case they gamble the minimum allowable bet. Or else they stay out of the game whilst remaining at the table—a practice called ‘Wonging’ after Stanford Wong, a blackjack expert who invented it. That’s if they are allowed to get away with it.

## Enter the Count

In the film *Rain Man*, Raymond (Dustin Hoffman) contrives to memorise six decks of cards. But you don’t need to be able to do that. *The count* is a running statistic, invented by a mathematician called Ed Thorpe. For every high card dealt you subtract 2. For every low card you add 2, except 2 and 7 for which you only add 1. So if you start the count at 0, a positive count means that there are more highs than 38.46% or less lows than 46.15% remaining in the pack, moving the odds in your favour. Roughly speaking, 2 count-units are worth 0.5% edge in a 1-deck game, to wipe out the house-edge. In a 2-deck game 4 count units are worth 0.5% -- to get the *truecount* you have to divide the actual count by the fractional number of decks remaining to be dealt, which you can gauge from the height of cards in the transparent plastic discard stack.

There is more than one version of the count. One version simply adds 1 for a high and subtracts 1 for a low. The version above has the subtle advantage of being *balanced*: if you start at 0 you get back to 0 after the whole pack has been dealt. (The proof of this is left to the reader, or you can let JACK’s count-drill verify it for you.)

## Now feel the heat

Some skill is needed to keep the count. Although the mental arithmetic is trivial, you have to look as if you’re concentrating on the game. Pit bosses easily spot an amateur counter—they either sit there looking distracted, scarcely paying attention to whether they win or lose, or the amount they bet varies widely for no apparent reason (a hunch player typically increases the bet only when winning). Either way they stand out a mile against the typical players either side of them, doggedly committing financial hara-kiri. If they suspect you are counting, they will *give you heat*, as it’s called in the trade...

- subject you to a hard stare
- pass comments on your play
- refuse your bet, for some contrived reason
- insist you place level bets only (i.e. unvarying, which wipes out the benefit of keeping the count)
- shuffle the cards more frequently than they would otherwise
- if all else fails, ask you to leave.

Of course bringing any sort of hidden computing device to the table is cheating, which happens to contravene Arizona State Laws. But counting in itself is not illegal, although the casinos wish they could make it so. It seems a pity that they won’t let you play if you get too good, but of course they’re not in business to play against adverse odds—they’d be fools to do so.

## Workspace JACK

Written in Dyalog APL, workspace JACK...

- plays the game with you (as dealer) to given house rules (these can vary from casino to casino)
- advises on Basic Strategy, if you aren’t certain what to call at any stage
- keeps both the actual count and the truecount, plus other relevant statistics
- has drills to help you learn both BS and the count
- simulates an auto player playing BS and counting too, and aggregates results for n hands (typically n=300, a good day’s play)
- lets you fine-tune your betting strategy
- forcefully illustrates the health-hazards of gambling.

The author is working on a commercial version of JACK, ready for shipment by 3Q00.

> Dr. Ian A Clark,  
> IAC Human Interfaces,  
> R21, Auckland Business Centre,  
> St. Helen Auckland,  
> BISHOP AUCKLAND  
> Co. Durham, DL14 9TX England.

> Tel: +44 (0)1388 60 55 44  
> Mobile: +44 (0)7931 370 304  
> Email: [earthspot2000@hotmail.com](mailto:earthspot2000@hotmail.com)

***
