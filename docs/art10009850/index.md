---
vid: '10009850'
title: 'Review: Kx Systems Users’ Meeting'
authors:
- Stephen Taylor
volume: '21'
issue: '2'
page: '50'
source: trad/v212/kx212.htm
converter: vecrescue 0.1.0
issue_tab: n2-february-2005
tags:
- conference reports
- implementations
- applications
---

Kx Systems came to London in February for their annual user meeting. Arthur Whitney gave a brief demonstration of Q, his new and more accessible ‘skin’ over kdb+ and the K programming language. He started his demonstration by downloading and installing Q from his server in California. This took less than a second.

Various Kx customers spoke, but the star of the show was Mark Sykes from Deutsche Bank Global Trading in London. Sykes is an arbitrage trader, which is to say he looks for mismatches in financial markets from which he can make a risk-free profit. Or if not risk-free, one in which the odds are heavily in his favour.

This is an obvious field for automation. Arbitrage traders have theories about what opportunities look like. But most ‘windows of opportunity’ slam fast quickly. Despite a generation of traders raised on Sony Playstations, the real action is in getting computers to watch and trade.

Sykes told us when he first came to arbitrage, traders desks sported thick books on Excel and Visual Basic. Now they are more likely to carry books on <acronym>J2EE</acronym>, as traders’ home-grown programming becomes more sophisticated. Europe, he thinks, is ahead of America in this, but the gap is narrowing.

He plugged in to his trading systems at DB and showed us live market prices and his positions in 10-year Bundesbank interest-rate futures. He described a well-known arbitrage model “Reversion to Mean”: when the price moves a standard deviation from its rolling mean, the probability of it reverting to the mean almost immediately is very high.

A chart of tick (trade-by-trade prices) against the standard deviation from the rolling mean demonstrated the truth of this. Every time the price line crossed the deviation line, it jumped back to the mean very shortly afterwards.

If you acted promptly, you could trade at the deviant price and reverse the trade at a near risk-free profit very shortly afterwards. ‘Very shortly’ is the key here. From the chart we could see that the window of opportunity typically closed in 600-1200 milliseconds.

Sykes explained the rules of the game to us. Reuters has used 200 milliseconds of your window just getting the price to you. You have to recalculate the rolling mean and the standard deviation, decide whether to trade or not, compose the trading order and get it on the wire to the exchange. This will succeed, but if you are too slow you risk getting your order filled after the market price has reverted to the mean. Speed is everything. He aims to get everything done in under half a second.

The speed advantage of Q exposes second-order opportunities. Q programs get their orders filled first and so scoop the best out of the opportunity. Behind them come Java programs. By the time Excel models have joined in, the price is swinging the other way, past the rolling mean. Canny Q programs might be out there waiting for a second bite!

<acronym>APL</acronym>s have long been favoured for financial markets software. A+ ([<acronym>APL</acronym>usdev.org](http://www.aplusdev.org)) was developed on a Manhattan trading floor. The rich set of mathematical primitives and fast array handling are obvious advantages in this business. But equally important is the low volume of <acronym>APL</acronym> source code. In fast-moving business environments, no one writes and ‘signs off’ program specifications.

Coding here is a design activity, a process of thinking through both the problem and its solution. Low code volume keeps the software soft and pliable, a tool for thought. Nowhere was this more evident than in Mark Sykes’ display of his own code.

Though he apologised for failing to achieve the brevity of Whitney’s own code, we could see on even a casual inspection a high degree of semantic density. That is, a high proportion of the defined tokens denoted entities in the problem domain, such as prices, dates, trades, rather than artefacts of the programming language, such as labels and counters.

Mark Sykes has no time to brief analysts and programmers as his thinking develops. Instead of discussing his problems and their solutions in ambiguous, untestable English, he writes Q, with help from the professional programmers at First Derivatives.

Once again, in one of the most competitive business environments humans have ever devised, an <acronym>APL</acronym> is winning by collapsing software development and execution times. Expect to hear more stories like this.

***
