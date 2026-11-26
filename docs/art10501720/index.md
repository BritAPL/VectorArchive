---
vid: '10501720'
title: J-ottings 59 Love Actuarily...
authors:
- Norman Thomson
byline: Norman Thomson (ndt4@btinternet.com)
volume: '27'
issue: '1'
received: '2016-04-09'
online: '2016-10-31'
editor: John Jacob
description: Working through the Chain-Ladder method the author describes a developers approach to implementing an actuarial solution.
source: ../../php-site/content/printed/271/jot59/jot59.htm
converter: vecrescue 0.1.0
issue_tab: n1-20162022
tags:
- mathematics and statistics
- applications
---

![](featuredImg_j-ottings-59-love-actuarily.jpg){ .lead-image }

Norman Thomson  
(ndt4@btinternet.com)
{ .byline }

In my experience of demonstrating APL and later J to potential customers, the one group for which it was always guaranteed to generate excitement was actuaries. I was therefore intrigued to read Jeremy Smith’s article “Using J for Actuarial Applications – Part 1: The Chain-Ladder Method” in *Vector* 26 no. 4 on the so-called Chain-Ladder method (a new term to me) which recalled several such happy experiences.

The chain-ladder problem arises when insurance companies make claims relating to accidents which occurred in earlier years, the assumption being that after ten years such payout claims will effectively cease. I have to say that I found myself initially befogged by the quantity and size of the seven-digit values in the example data, also by the fact that the displayed data became transposed between Figures 1 and 2. Being a computer person and not an actuary, I like to work with numbers which are small enough not to obscure the method, but at the same time distinctive enough to be recognizable by their position in the scheme of things, so allowing a degree of hand checking. Scaling up can be left to someone else - an actuary, say! Also I am somewhat bewildered as to why insurance companies continue to pay out on motor accidents ten years after the event, but again as a computer person I don’t need to know about this to extract the essence of the problem – the substantive background is a matter for coffee break, as is the name ‘chain and ladder’. (I know ‘chain’ and I know ‘ladder’ but can’t quite see how the combination describes the problem – again not a ‘need to know’).

Turning to the problem, my first suggestion which I have made a few times before in these columns, is to ‘think list’ when structuring data in J. This straight away obviates the need for Jeremy’s first two verbs `exportboxes` and `removezeros` to which I will nevertheless return later. Here in list terms is my extract of the problem:

The data is a list, each item of which is a list of cumulative values. The first of these is of full length, say five in my illustrative example, the second is of length four, the third of length three and so on. The task is to devise a method of estimating the ultimate, (that is fifth in my example) value in each of the sequences after the first. Here is my sample data :

```
   data
┌──────────────┬───────────┬────────┬─────┬──┐
│15 24 33 40 45│14 18 22 26│13 16 19│12 14│11│
└──────────────┴───────────┴────────┴─────┴──┘
```

for which the period-to-period growths in the first list is :

```
   x
15 24 33 40 45
   }.x%_1 |.x            NB. (24/15), (33/24) and so on
1.6 1.375 1.21212 1.125
```

(Personal note: I have at times struggled to remember the difference between the symbols for ‘take’ and ‘drop’. The best I can suggest is that the character ‘\}‘ looks slightly like a poorly scrawled ‘dr’.)

The final period growth of 1.125 multiplied by the last value in the second list gives an estimate of the ultimate value of 26 x 1.125 = 29.25. To estimate the ultimate value for the third list, bring in more data by averaging the two known values of fourth period growth from the first two lists, viz. divide (40+26) by (22+33) to give 1.2, then multiply this by the known third period growth to estimate of the ultimate value for the third list as 19 x 1.2 x 1.125 = 25.65.

At the next step there are three known measures of third period growth which can be averaged, viz. (33+22+19)/(24+18+16) = 1.276 leading to an estimated ultimate value for the third list of 14x1.276x1.2x1.125 = 24.12.

Now express this in J. The first calculation step is to describe the age-to-age factors (Jeremy’s term), that is the succession of values 1.125 1.2, 1.276 ….

It helps to see where these quantities are coming from by displaying

```
   >data
15 24 33 40 45
14 18 22 26  0
13 16 19  0  0
12 14  0  0  0
11  0  0  0  0
```

which shows that the successive numerators are

```
    }.+/>data
72 74 66 45
```

Progressing from expression to verb can be achieved either by judicious insertion of `@` and `@:` taking rank inheritance into account

```
       num=.}.@(+/@:>)
   num data
72 74 66 45
```

or more easily by defining

```
   num=.monad : '}.+/>y'
   num data
72 74 66 45
```

The denominators are slightly more complicated, but with a little thought are seen to be

```
    +/}:>}: each data
54 58 55 40
   den=.monad :  '+/}:>}: each y'
   den data
54 58 55 40
```

leading to the four age-to-age ratios as the fork

```
   ata=.num % den
   ata data
1.33333 1.27586 1.2 1.125
```

Age-to-age ratios work like compound interest, so that what Jeremy calls age-to-ultimate ratios are what in APL terms would be called ‘multiply scan’ with reverse applied and with a 1 joined at the start to take into account that the ultimate value in the first list is already known :

```
   atu=.monad : '1,(*/\&|.)ata y'
   atu data
1 1.125 1.35 1.72241 2.29655
```

To find all the ultimate values simultaneously all that remains is to isolate the last non-zero items in each list and multiply these by the above ratios.

```
   lnz=.>@({:each)        NB. last non-zeros
   lnz data
45 26 19 14 11
   ults=.lnz * atu        NB. ultimate values
   ults data
45 29.25 25.65 24.1138 25.2621
```

Reserves are then simply the difference between the ultimates and the last known values :

```
    res=.ults – lnz        NB. reserves
    res data
0 3.25 6.65 10.1138 14.2621
```

If data has to be exported into to a spreadsheet with zeros inserted for rectangularity, this has already been achieved above as >data. `<"1` reverses this process, thereby dealing with Jeremy’s verb `exportboxes` :

```
   <"1 >data
┌──────────────┬─────────────┬────────────┬───────────┬──────────┐
│15 24 33 40 45│14 18 22 26 0│13 16 19 0 0│12 14 0 0 0│11 0 0 0 0│
└──────────────┴─────────────┴────────────┴───────────┴──────────┘
```

`removezeros` is achieved by :

```
   rz=.(#~(~:&0))each        NB. remove zeros
   rz <"1 >data
┌──────────────┬───────────┬────────┬─────┬──┐
│15 24 33 40 45│14 18 22 26│13 16 19│12 14│11│
└──────────────┴───────────┴────────┴─────┴──┘
```

This technique here is to use `~:` (not-equals) to create binary lists which are then used as selectors (`#`) in a ‘bridge hook’, that is one of the form `f~g`. This is a particular case of a general method for removing specified items from a list by *value*. Another possibility, also applicable here, would be to remove items by *position*, in this case by dropping 0, 1, 2… items from the right :

```
   rz=.monad : '(-i.#y)}.each y'
```

The final check was to test my verbs on Jeremy’s data – they work! (Incidentally in his Figure 6 there appear to be a couple of `+/` s missing on the lines which total ultimates and reserves).

Albeit at a distance, this seems to me to illustrate nicely how actuaries and J users can collaborate. Jeremy rightly says that ‘chain-ladder’ is a very basic technique involving no more than simple arithmetic, that is no calculus, probability or time series – the things in which actuaries revel. However its very simplicity helps to emphasise the values of simultaneous calculation, and more importantly an approach that, unlike Excel, needs no adjustment for different data sizes, a point stressed by Jeremy to which I give my whole-hearted assent!

```
   >js
36 112 174 221 274 332 347 361 383 390
35 124 217 335 380 412 465 491 534   0
29 129 221 324 399 413 463 491   0   0
31 142 220 376 403 438 459   0   0   0
44 114 213 290 340 387   0   0   0   0
40 133 218 299 369   0   0   0   0   0
44 129 242 348   0   0   0   0   0   0
36 142 286   0   0   0   0   0   0   0
38 136   0   0   0   0   0   0   0   0
34   0   0   0   0   0   0   0   0   0
```

## <a id="references"></a>References

1. Jeremy Smith, “Using J for Actuarial Applications – Part 1: The Chain-Ladder Method”, *Vector* 26 no. 4, [britishaplassociation.co.uk/art10501580](https://britishaplassociation.co.uk/art10501580)
