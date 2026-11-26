---
vid: '10500810'
title: What is it about infinity?
authors:
- Sylvia Camacho
byline: by Sylvia Camacho (sylviac@blueyonder.co.uk)
volume: '25'
issue: '3'
page: '27'
online: '2012-03-24'
editor: Kai Jaeger
description: Camacho answers Thomson regarding some paradoxes of infinity, helped by J notation.
abstract: Sylvia Camacho's answer to Thomson's letter on Infinity.
source: content/printed/253/infinity_2.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- mathematics and statistics
---

by Sylvia Camacho (sylviac@blueyonder.co.uk)
{ .byline }

Sylvia Camacho's answer to Thomson's letter on Infinity.
{ .abstract }

Dear Norman,

I read Cornelius Lanczos on classes of the number field some years before I was introduced to APL, so I was immediately receptive to Iverson's emphasis on improving the rigour of mathematical notation by distinguishing between the subtraction operator and the notation for a negative number. This made me hyper-sensitive to the ambiguity of the conventional expression for the infinite alternative of reciprocals. Your comment on the definition of 'convergent series' shows that the reciprocal series cannot converge as a simple sum but it does if every alternate value is subtracted. This, in conventional maths implies an order of evaluation:

```
   (((((((9-8)+7)-6)+5)-4)+3)-2) NB.conv.maths evaluates lft to rt
4
```

and is insensitive to a change of sequence providing each term is assumed to include its preceding operator and that no term is omitted, which was how I read Derbyshire's "but only if you add up the terms in this order.":

```
   (((((((9+7)-8)-6)+5)-4)+3)-2) NB.conv.maths: lft to rt
4
```

but, of course, an infinite series implies an infinite procedure and so any actual evaluation will be an approximation equivalent to a truncated series which will be sensitive to omissions. Graham, being a mathematician, appreciated this: hence his final table. Your reference to the need to balance the p and n of any re-arrangement is a way of expressing the same restriction.

If like Iverson we follow Lanczos and use notation which distinguishes the class of positive from that of negative numbers, the conventional expression contains only positive numbers and preceding addition and subtraction operators and will be sensitive to bracketing which can split an operator from the following number. Lanczos explains that the subtraction operator came to be accepted as the indicator of a negative number by giving it an implied left argument of zero. This is also the way that J interprets it:

```
   -3 NB. enter minus 3
_3    NB. J returns negative 3
```

Do you think that conventional mathematicians believe that making this distinction between operator and negative sign is to make a mountain out of a molehill? I have to accept that Lanczos did not use any special notation in his regular mathematical publications.

Your explanation of Riemann's Theorem is intuitively clear, although I could not begin to prove it. The infinity of reciprocals of integers comprises all possible fractions of the unit, from which any number could be composed just as any number can be represented in base-2 (I think).

Regards from sylviac@blueyonder.co.uk
