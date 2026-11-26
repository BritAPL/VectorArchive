---
vid: '10004080'
title: Data Compression with Dfns
authors:
- Veli-Matti Jantunen
byline: by Veli-Matti Jantunen, Statistics Finland (veli-matti.jantunen@stat.fi)
volume: '20'
issue: '2'
page: '107'
source: trad/v202/dfn202.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2003
tags:
- programming techniques
- data and files
---

*by Veli-Matti Jantunen, Statistics Finland ([veli-matti.jantunen@stat.fi](mailto:veli-matti.jantunen@stat.fi))*
{ .byline }

> “There are zillions of programs already written in other programming languages which address data compression; some of them might even be better at the job than an APL programmer could knock up in half an hour… Build APL on top of common tools rather than write everything for ourselves all the time.”  
> –Dick Bowman

> “The ultimate compression: binary information only contains ones and zeros, and because zeros mean nothing, you can just count the ones… I’m sure that someday someone will invent the way to extract thus easily compressed data…”  
> —Anonymous

## Why bother?

The years in the DOS world have left their scars: I still try to squeeze everything possible to smaller entities. I detest the idea of my workspace containing unnecessary white space or other compressible stuff, which unnecessarily doubles the download time or fills every diskette, not to mention the myriad of different version workspaces on my hard disk.

This article tells briefly how I developed a data compressor (actually plenty of them), but the reader should appreciate that I had the support of a dfns mailgroup behind me all the time. I could always just send my airy-fairy ideas and code fragments to the group and got useful feedback every time, either via the mailgroup or direct. Thanks to every one of you! APL development is fun, especially with professional help ;-).

## Early Habits

The simplest data packing algorithms have been in use for years. When these tools were converted to dfns, the ideas shone through nicely. Null packing simply rips off the zeroes and uses the Boolean vector to mark their places.

```
packN←{

     cmp←{mask←,⍵ 1↑0⍴⍵ ⋄ (⍴⍵)mask(mask/,⍵)}

     exp←{(shape mask items)←⍵ ⋄ shape⍴mask\items}

     ⍺←1 ⋄ ⍺:cmp ⍵ ⋄ exp ⍵

 }
```

Note that this clever programming style (shamelessly adapted from John Scholes) lets you write both the packer and the unpacker functions in the same body. Furthermore, you can write the testing operator which utilises this structure:

```
     PackTest←{⍵≡0 ⍺⍺ ⍺⍺ ⍵}
     packN PackTest 2 3 4⍴10 0 0 20 0 0 0
1
```

Unique packing is a nice and quickly implemented method (if you have cases with more than 127 but less than 256 unique elements, you may subtract 128 from the indices when compressing to abuse the internal representation; remember to add them when extracting).

```
packU←{⎕IO←0
     cmp←{u←∪,⍵ ⋄ (⍴⍵)u(u⍳,⍵)}
     exp←{(0⊃⍵)⍴(1⊃⍵)[2⊃⍵]}
     ⍺←1 ⋄ ⍺:cmp ⍵ ⋄ exp ⍵
 }
```

The third widely used tool is simple RLE packing, which can be found commented upon (!) in the dfns workspace (packR), just like the other packing functions mentioned here.

## New Tricks to an Old Dog

When Adrian and Stefano revealed in their Vector article how to use the GZIP engine to squeeze data inside the workspace, I felt that surely there must be better ways to pack data than I had been using. Although the idea to use external dll is a quick and effective one, I happen to be an old stick-in-the-mud: no external parts if possible. Back to the old drawing board - and discussion about compressing algorithms ignited instantly in the dfns mailgroup.

My aim was to develop a quick compressor with a decent packing ratio and as universal use as possible. I could always use GZIP as a measurement rod, and thus could separate interesting but useless ideas apart from those that seemed worth a closer look.

## Borrowed Ideas

Stefano explained an old algorithm that was used to pack certain text data, which contained long patterns of the same character. The idea was something like this: there is one character (“esc”) that is used to denote the packed parts, the following character tells how many times the third character is to be repeated. When I had finished the function it became clear that this was meant to be a packer for special purposes, not the all-round text packer, although it bears the name packT.

The next idea was to try to find an open algorithm (as opposed to LZW, which is patented, and anyhow seemed too complicated for me), and I came across the Shanno-Fano algorithm, which belongs to the Huffmann compression family.

The compressor (packS) was pretty straightforward to do (use variable length keys, use the shortest for the most frequent items), the uncompressor was a pain in the brain. Eventually I cooked up something that works, but every time I look at the packH code (John’s version of Huffmann compressor), I feel uneasy.

## An APL Approach: Do It Yourself

After furious coding I felt it a little bit awkward to try to mimic other people’s ideas instead of doing something original.

The idea came to me when I was finishing this GUI application with lots of bitmaps hanging around. Although the first compression can be made by diminishing the palette to 16 colours, they still use much of the ws estate. Sixteen colours means sixteen indices which means that they can be presented with just three bits! One night with motivation, and I had it: first find the unique items and store them separately, then calculate the smallest amount of bits that those indices need, and decode them as a Boolean vector. Afterwards, I added a quick RLE coder, and the bitmap packer packB was born. I was astonished to see how well it performed when compared with the really professional tools. Especially the extracting speed was a pleasant surprise.

But this wasn’t quite the general tool I was looking for.

After some months’ quiet life, I got this simple idea: instead of packing single items, I could concentrate on paired items. The pairings can be derived with the unique vector’s outer product with itself, and using Booleans and clever indexing I realised that the packing ratio was in some cases better than with the earlier functions.

The last idea thus far came the next day: what about packing the items according to frequencies so that the first 1 will start a new key, i.e. the shortest keys would be 0, 1 0, 1 1 0, and 1 0 0. Quickly, this idea was coded as:

```
packQ←{(⎕ML ⎕IO)←3 1
     key←⊃,/{⍵{↓⍺ ⍺↑⍵}¨⊂⍵∘.>⍵}⍳23
     cmp←{dt←,⍵ ⋄ u←∪dt
         f←{+/⍵=dt}¨u ⋄ u←u[←f]
         b←∊(1,¨(⍴u)↑key)[u⍳dt]
         u((⍴⍴⍵),⍴⍵)b}
     unc←{(u d b)←⍵ ⋄ r←(↑d)⍴1↓d
         p←b≤¯1↓0,b ⋄ r⍴u[key⍳p⊂b]}
     ⍺←1 ⋄ ⍺=1:cmp ⍵ ⋄ unc ⍵
 }
```

Which is by far the best I have been able to put together.

## A Teaser: An Enchanting Universal Algorithm

Just an idea: any simple array can be changed to the binary form and back without loss of information. Take two integers, the first is the random seed for roll, the next is the number of binaries to be created. Because there are patterns that can be really long, and thus easily created, this might be the Ultimate Packing Algorithm.

Why doesn’t it work? (I don’t confess I tried, though.)

Happiness is a warm compressor!

P.S. To subscribe to the dfns group, send email to [dfns@dyalog.com](mailto:dfns@dyalog.com) with subject: subscribe

### References

> \[1\] Compressing APL arrays with GZIP, Vector 18.1, 84  
> \[2\] Workspace dfns.dws from www.dyalog.com/download/dsamples.zip.

***
