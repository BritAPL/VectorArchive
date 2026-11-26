---
vid: '10002350'
title: 'Jot-Dot-Min: Stereo Vision'
authors:
- Ian Clark
byline: by Ian Clark, . (earthspot2000@hotmail.com)
volume: '12'
issue: '1'
page: '20'
source: trad/v121/clark121_20.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1995
tags:
- graphics
- education
---

by Ian Clark, <span class="affil">.</span> <span class="email">(<a href="mailto:earthspot2000@hotmail.com">earthspot2000@hotmail.com</a>)</span>
{ .byline }

My father once told me that as a boy he used to fish for flatfish in the river Clyde with a table fork tied to a stick. The idea was to creep about barefoot in the shallows until you felt something wriggle under your foot, then lunge down with the fork. Inevitably there was somebody who didn’t get his foot out of the way in time and priceless excuses had to be invented for bloody footprints in the kitchen.

British flatfish like the plaice and sole have a pretty neat camouflage pattern on their topsides which mimicks the random speckle of a sandy bottom. But you don’t stand a chance if you’re a flatfish and your predator has stereoscopic vision, unless you can flatten yourslf on the bottom and stay still. According to the evolutionists, mankind’s marvellous capacity to spot speckled fish underwater must have been many millions of years in the making, around the shores of clearer waters than the Clyde, where we can imagine our brutish hominid ancestors peering into pools brandishing sticks with the elder-day equivalent of a table fork lashed to them.

Here’s the sort of thing they were looking for, and (since we’re all here) successfully spotted.

## ![figure\_1](clark121_20-fig1.gif)

The two squares of random dots A and B together form a stereogram. This is not as elaborate a construction as the often-named “stare-ee-o” pictures created using J, as recently described in our parent magazine (Clough, VECTOR Vol 11 no 4, 110-116), but still impressive nonetheless. Some people find it easier than a “stare-ee-o” to see the illusion. For best results you could mount the pair of images in a Victorian device called a stereoscope, which directs each image exclusively to one or the other eye. Failing this, fusion lines are provided below the squares to assist you. Simply allow the fusion lines to drift together, then focus on the pattern inside the fused image.

After a few seconds you will see a shape, a rectangle in this case, clearly detach itself and float above the surrounding dots. Dover soles aren’t normally rectangular, but we’ll overcome that problem later.

Not only do squares A and B together form a stereogram, but so do B and C — and a quite distinct one too (it’s a different rectangle). If you’re lucky, you can see both sets simultaneously, because when you fuse A and B, by happy chance you also fuse B and C.

Were you to cut out the squares A and B and transpose them, you would again see the same rectangular shape, but this time in the form of a window. We’ve done it for you below:

## ![figure\_2](clark121_20-fig2.gif)

Let us consider for a moment just how marvellous this capacity is. With two eyes having overlapping fields of vision you can clearly see an object which, for each eye considered separately, quite simply isn’t there. What do we mean by that?

Image B is a random array of square blobs, actually blown-up pixels. Each pixel can be black or white with equal probability. The Apple Macintosh supports both black-and-white and coloured icons, 32 by 32 pixels each. APLomb (a software construction kit using I-APL as its scripting language) lets you turn an icon into a 32 by 32 array of numbers (a black-and-white icon becomes a Boolean array, with 1 for black and 0 for white) and back again. So Image B was produced as follows:

```
B←¯1+?32 32⍴2
```

Here’s the explanation. Roll (`?`) followed by an integer value (scalar or array) returns an integer value of the same shape, but of random integers. `?2` is a random integer between 1 and 2, but we want a random 0 or 1. You can’t get this with `?1`, you just get 1 back again, so we must use `?2` and subtract 1 from it.

If you want an array of \{0,1\} which you can bias towards more 0s, you can use `50>?100` in place of `?2`. If you increase 50 to 85, say, then 0 will tend to arise for approximately 85% of the entries.

The “Dover sole” S was defined as a sub-array of bitmap B by means of the indexed assignment:

```
S←B[6+⍳16;9+⍳18]
```

Then S was rotated by one pixel (simulating the shift rightwards of S, or rather a rectangle one pixel narrower than S) and put back into C (a copy of B) over the same rectangle as before:

```
C←B
C[6+⍳16;9+⍳18]←¯1⌽S
```

The result C is still a random array, with exactly the same statistical distribution of pixels as before. Mathematically speaking, nothing about the operation distinguishes the boundary of the Dover sole in C, except that it happens to be derived from B. Considered apart from B, it is mathematically impossible to detect in C that an arbitrary rectangle has been rotated by 1 pixel. Just to make the point, a different rectangle has been rotated the other way in B, using `1⌽S` instead of `¯1⌽S` to make A.

However, fuse the images B and C together (or A and B), and the brain detects parallax. That is, it recognises that a region of pixels has shifted bodily sideways against a background formed by the remaining pixels, when viewed first by one eye, then by the other. Why doesn’t the brain just see a blurred area where the value of the expression `B=C` contains 0s, meaning no-match? If you repeat the experiment you’ll notice that the 0s don’t form a solid block, yet the illusion is of a crisp floating rectangle. Moreover both the pattern on the rectangle and the background pattern are precisely fused! You’d think the eyes could superimpose only one or the other at a time, not both at once. So B=C is not what the brain is computing.

Instead it would seem to be making a crafty guess at what might have caused the displaced pixels and decides, from its experience of Nature, that the most plausible explanation is that there is a well-camouflaged object with a rectangular border floating above a speckled background. But then the brain goes and presents its hypothesis to you, not as a hazy probabilistic suggestion, but as certainty, i.e. as a rock-solid illusion of a floating rectangle.

If you equip yourself with a suitable APL capable of printing dot-arrays from rectangular Boolean arrays (or as we called them in the last issue, bitmaps), then here are a few experiments for you to try:

1. Use a non-rectangular mask, like a fish-shape. Does the brain still “see” a clear boundary? Don’t forget, some of the displaced speckles might accidentally match the “underlying” ones, although that doesn’t seem to bother the brain with examples A B and C above.
2. The Dover sole is “opaque”, i.e. no underlying pixels show through. What if it were “transparent”? Is the boundary still crisp? Does swapping the two images still give the impression of a window, instead of a floating shape?
3. How small can the displaced rectangle be before the illusion is lost? Will the brain detect a single displaced pixel and portray it as a tiny “floating” fish? Or will it write off a single pixel out-of-place as “noise”. Does it matter if the pixel coincides with an underlying speck when it is shifted?
4. There are programs which allow you to view a collection of similar pictures as an animated sequence. By generating several successive pairs of images, can you get the sole to “move”? Can you make it rise, by increasing the pixel displacement from one to two, or sink to the bottom by going the other way? If it does sink to the bottom, can you still “see” it, even briefly? If you can, you’re seeing something for which there’s absolutely no statistical evidence, at least in one particular frame.
5. What if the sole is displaced vertically or diagonally with respect to the background instead of horizontally? Do you still see a crisp illusion? Don’t forget that the brain knows your eyes are in-line horizontally, so a vertical displacement going from one eye to the other is rather unlikely to arise as a result of parallax.

Now for some APL techniques with bitmaps to help in your investigations. To get a fish-shaped Dover sole, it’s best to abandon indexed assignment to pick out and rotate a given sub-rectangle. The trouble is that defining a shape by means of `B[U;V]` where U and V are vectors of consecutive numbers, means the shape has got to be rectangular. Used above, it helped us to make a philosophical point (the randomness statistics of the image with its rotated shape remained totally unaltered), but in practice you will find it easier to generate the speckled background once for all, then the fish as a separate array, then to overlay the fish first in one place, then shifted sideways by one column or maybe two.

First you need a mask of a fish in the form of a bitmap. It’s best to edit this by hand. Suppose F is the fish shape as a 32 by 32 Boolean array, G is the random speckle on the fish, H the random speckle on the background. G and H may be generated as follows:

```
G←¯1+?32 32⍴2
H←¯1+?32 32⍴2
```

making two independent random patterns. Now use F as a cookie cutter on both G and H; like this. `G^F` zeroes everything outside the fish shape, and `H^~F` zeroes everything inside the fish shape. Put them together and you have just placed the fish opaquely on the background (although it won’t show up of course): `A←(G^F)∨(H^~F)` Now rotate both the fish and the speckled background: `F←¯1⌽F G←¯1⌽G` and repeat the above steps to make B: `B←(G^F)∨(H^~F)` B now forms a stereo pair with A.

That’s all the space we have for bitmaps this week. We’ll get onto games of life and epidemic models shortly.

(webpage generated: 18 October 2006, 03:55)
