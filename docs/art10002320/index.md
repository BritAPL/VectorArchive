---
vid: '10002320'
title: 'Jot-Dot-Floor: Domino'
authors:
- Ian Clark
byline: by Ian Clark, . (earthspot2000@hotmail.com)
volume: '11'
issue: '2'
page: '20'
source: trad/v112/clark112_20.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1994
tags:
- education
- mathematics and statistics
---

by Ian Clark, <span class="affil">.</span> <span class="email">(<a href="mailto:earthspot2000@hotmail.com">earthspot2000@hotmail.com</a>)</span>
{ .byline }

A guy from an IBM laboratory (which will be nameless) once offered me for analysis dozens of reels of magnetic tape containing the record of two years’ work by a team of 20 APL programmers using a central APL service. On the tape was captured every keystroke during that time. Nothing had been done with the tapes beyond elementary counts of keystroke by type, but it emerged that in all that time, Domino (`⌹`) had been used precisely twice.

Once was a mis-key. The other was when the programmer wanted a nice blobby character for printing a histogram.

This doesn’t say much for the depth of theory brought to bear on the APL programming in that particular location. Perhaps they’d all have been better off using BASIC. Now as every educational APL user knows, Domino is one of the most impressive primitives of all. It not only inverts matrices, it divides one matrix by another, like this: `Y⌹X`.

In fact `⌹X` , the matrix inverse of `X`, is just a special case of `I⌹X`, where `I` is the Identity Matrix (with 1s down the diagonal, 0s elsewhere), just as `÷X` is a special case of `1÷X`. Get your free copy of I-APL out and try it. And while you are about it, you can verify that `÷X` is in a sense a special case of `⌹X`, by defining `X` as a one-by-one matrix of some given number, yes, like this:

```
      X←1 1⍴99
```

Yes that’s all very well, but is there a practical use for it? Is there any mileage in it, I think they say nowadays (or was that last year?) Well, there’s a lot of mileage in it, both literally and metaphorically. Consider this, now.

Petrol is expensive these days. To fill my car with petrol generally costs me over £20 a time. Do you know how much it costs for a litre? What’s that in gallons? If it weren’t for the scale still stuck to the pumps in some of the more backward parts of the country I’d never know. In fact I’m invited implicitly to trust the pump to deliver what it says it’s doing. For all I know they’ve got a little icon on their coloured screens saying "muggins" which they touch whenever they’re certain I’m not the weights and measures man.

Nobody likes to pay lots of money for things when they don’t know how much it really costs. You know what you’re being charged, of course, but do you know what you’re getting for it? Now it may come as a surprise to the oil companies, but when you buy petrol you don’t really want so many litres of dangerous smelly fluid. What you really want is so many miles for your car to go. So how many miles are you really getting for a tankful of petrol costing £20?

The measure of how good a car is at turning petrol into distance travelled is called its (fuel) consumption. The manual that comes with the car will give you a figure to expect, generally in miles per gallon. But it varies. A mile uphill takes more petrol than a mile downhill. Just how much more you only begin to appreciate when you see the petrol actually being sprayed in the carburettor as somebody puts their foot on the accelerator pedal. More importantly, the more your car needs servicing the more petrol it tends to use - to go a set distance of course. So how can you possibly find out your car’s consumption, here and now?

A car-manufacturer’s manual once told me, in all seriousness, to fill up a jerrycan with exactly one gallon of petrol, and then to drive around until the tank runs dry and the engine stops (it’s no good relying on the petrol gauge on the dashboard showing "empty", the book said). Once the car goes phut and stops, get out and pour in the gallon of petrol. Write down the tachometer (milometer) reading - most important! Then drive around steadily on level roads without stopping and starting if at all possible until the engine stops again. Read the tachometer once more and subtract the first value from it. You’ll get a number, say 35.5 (miles). This is actually your car’s fuel consumption in miles per gallon!

Why not get your class to ask their dads to do this the very next weekend? Why should teachers always be the ones to bear the burdens of others’ loose thinking? Then, when they’re all back in class the following Monday, and you’ve administered motivational therapy to the defaulters, tell them all there’s a smarter way to measure fuel consumption, without wasting time and petrol.

Each time you buy some petrol, note down the tachometer reading (X) and the quantity bought (Y). Plot the two series of numbers, Y against X, on a graph. Draw the best straight line through the points (it’s best to have four or more) and hey presto, your car’s consumption is the gradient of the straight line!

With APL you don’t even have to plot the graph and "guess-timate" the line. Suppose X and Y are the two sets of readings. For test purposes try:

```
      X←1.1 2.1 3.1 4.1 5.1

      Y←0.88 1.68 2.48 3.28 4.08
```

In fact you can compute Y by:

```
      Y←X×.8
```

Now `Y⌹X` gives the gradient of the best straight line through the set of points having the coordinates:

```
      (X[1],Y[1]), (X[2],Y[2]), ...
```

Try it - you’ll see 0.8 as the result.

You can try adding a bit of noise to Y - varying it up and down a little to suggest figures got from the real world. That’s the great thing about APL. It’s so easy to generate test data, which brings dry calculations to life. We’ll leave noise generation as an exercise here (something for a future issue of EV).

Wait a minute though - this line we’ve fitted happens to pass through the origin (0,0). This is not what we want. The best straight line doesn’t necessarily go through the origin, because you can’t expect to start recording petrol purchases with a zero mileage reading. Not even with a car straight from the factory.

Hovever you can still use "domino" (`⌹`) to fit the correct line, but you must modify X slightly. Instead of just a sequence of distances, we want a 2-column matrix, with 1’s all the way down column 1, and the original X becoming column 2. Thus, instead of X being:

```
1.1 2.1 3.1 4.1 5.1
```

we want X to be:

```
      1 1.1
      1 2.1
      1 3.1
      1 4.1
      1 5.1
```

To make X into this, enter:

```
      X←X∘.*0 1
```

Now:

```
      Y⌹X
```

gives a result consisting of a pair of numbers: 0 0.8 i.e. the intercept and the gradient, which together define the line. Instead of 0 the first number might turn out to be a very small but non-zero number in "scientific notation", eg:

```
5.846505E¯10 0.8
```

which is really only saying that when we calculate a number which ought to be 0, using an approximating algorithm which doesn’t know that, we’d be very lucky to get exactly 0.

How on earth does it work? `Y⌹X` is an example of "matrix division", of which scalar division is a special case. In other words, if `L` is the one-by-two matrix `(0 0.8)`

```
      L = Y ⌹ X
```

is just solving for `L` the equation:

```
      L +.× X = Y
```

by (matrix) dividing throughout by `X`.

In APL, matrix division is done by a process called polynomial regression. A special case of this is linear regression, better known as "drawing the best straight line" (the least-squares best fit). The APL tutorial by Alvord and Thomson explains it all in detail.

So it really is some use being smart and knowing some mathematics! It can save you having to drive around all day going nowhere and risk coming to a dead stop in an awkward place. British Industry - Please Take Note.

***

A sample workspace PETROL, which performs this activity, is provided on the Freeware Disk of IAPL/Mac, the Macintosh version of I-APL. Write to the Editor, EV. An APL Tutorial, by Alvord and Thomson, is available from I-APL Ltd, 11 Auburn Road, Bristol BS6 6LS.

(webpage generated: 18 October 2006, 03:52)
