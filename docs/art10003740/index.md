---
vid: '10003740'
title: Celtic Knotwork in J
authors:
- Michael Horton
volume: '21'
issue: '1'
page: '90'
source: trad/v211/knot211.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2004
tags:
- graphics
- programming techniques
---

[www.vector.org.uk](../ "VECTOR home") > [archive](../) > [Vol.21](../) >  [![V21:1](/images/covers/32x45/v2101.jpg) No.1](../21/#n1-september-2004)

## Celtic Knotwork in J

*by Michael Horton ([mhorton@utas.edu.au](mailto:mhorton@utas.edu.au))*

## Introduction

Celtic knots are a simple but very decorative form of geometric art made to give the appearance of a thick line twisting around and crossing itself.

\[pic1\]

A systematic method of drawing knots using square grid was found by Andy Sloss [\[1\]](#ref1). A J code adaption of this method follows. It generates a graphical knot from a grid of pseudorandom integers. The picture is one example.

## The Squares

Each square in the knot can be defined by five properties: how lines enter from each of the four corners, and how the lines cross the square. The J program encodes these in a list of five integers.

Crossings may be 0 (vertical), 1 (diagonal) or 2 (horizontal).

\[pics\]

Corner entries may be 0 (no line), 1 (vertical), 2 (diagonal) or 3 (horizontal). Horizontal and vertical entries are one quarter of the side.

\[pics\]

## Line Segments

Since segments may start in one of 4 corners, enter 3 ways (not including lines that don't exist), cross in 3 ways and leave in 3 ways, there appear to be 324 possible segments. However, many of them are rotations and reflections of each other. For example, a segment entering diagonally, crossing horizontally and leaving horizontally is a rotation and reflection of a segment entering diagonally, crossing vertically and leaving vertically.

\[pics\]

In total, there are 10 distinct sections of lines crossing squares. These can be divided into 6 segments crossing horizontally (rotated to get lines crossing vertically) and 4 segments crossing diagonally.

\[pics\]

Horizontal segments (h11, h13, h21 h22, h23 and h33)

\[pics\]

Diagonal segments (d11, d12, d13 and d22)

For best aesthetics, the knotwork should mix curved and straight lines. This is why h13 and h21 use sharp angles while the other segments use curves.

Once the curves are constructed, they can be rotated and reflected to create all possible lines, and combined to create all possible squares. Geometry was generated with complex coordinates, as these are much easier to manipulate. In the end, the complex geometry needs to go into ‘gdlines’, which takes lists of real numbers.

The ‘graph’ script is needed for drawing and the ‘trig’ script for the curves.

```
  load 'graph trig'
  outerLine=:gdpen@2:,WHITE&(gdcolor@[)
  innerLine=:gdpen@1:,BLACK&(gdcolor@[)
  drawComplex=:$@0:`gdlines@:,@:+.@.(*@#)
  knotComplex=:(drawComplex@],innerLine),(drawComplex@}.@}:@],outerLine)
```

The drawComplex function will only call gdlines if it receives at least one number – some parts of the knot generate no geometry at all. The knotComplex function draws a thick white line followed by a thin black line. This deals with overlapping lines in the middle of a square.

The next problem is to generate the individual line segments. Each of the 10 basic line types is created once. They are then rotated and flipped to create the knot components.

```
   NB. Geometry modification functions
   flipVert=:+
   flipHoriz=:+@-
   rotLeft=:0j1&*
   flipBoth=:-

   NB. Horizontal line geometry, top left - top right
   h11=:(0j1-0.5*(cos j. sin) o.8%~i.9) "_
   h12=:flipHoriz@h21
   h13=:_0.5j1 _0.5j0.5 1j0.5 "_

   h21=:(0.5j1,0.5j0.5,((0.5-~%%:2) j. 0.5)+-:(1+_1%%:2)%~0j1-(sin j. cos) o.32%~i.9) "_
   h22=:(0j2-(%:2)*(sin j. cos) o.16%~i:4) "_
   h23=:(1j0.5,((0.5-~%%:2) j. 0.5)+-:(1+_1%%:2)%~0j1-(sin j. cos) o.32%~i.9) "_

   h31=:flipHoriz@h13
   h32=:flipHoriz@h23
   h33=:_1j0.5 0j0.5 1j0.5 "_

   NB. Diagonal line geometry, top left - bottom right
   d11=:((-:@sin@o.@%&16 j. %&_8) "0 i:8) "_
   d12=:((-@<:@(3r2&*@cos@-:@o.j.<:@+:)) 8%~i.9) "_
   d13=:(1j1-1.5*(cos j. sin) o.16%~i.9) "_
   d21=:flipBoth@d12
   d22=:_1j1 _0.75j0.75 _0.5j0.5 0.5j_0.5 0.75j_0.75 1j_1 "_
   d23=:flipHoriz@rotLeft@d12

   d31=:flipBoth@d13
   d32=:flipBoth@d23
   d33=:flipVert@rotLeft@d11
```

The overlapping segment code requires that every diagonal segment have internal coordinates, which is why the ‘d22’ function returns a straight line with 5 points in it.

These functions can be tested with commands like

```
  >(gdshow ''),(knotComplex d11 0),(gdopen '')
```

Which fills the graph window with the appropriate segment.

\[pic\]

## Choosing Segments

Segments are selected from a gerund of all the possible line drawing functions. The 'cut' versions of the lists have diagonal endpoints removed to allow line overlaps at the corners.

The topSeg, bottomSeg, leftSeg, rightSeg, sloshSeg and slashSeg functions take two input numbers – the entry and exit directions – and return the appropriate geometry function. Any segment with a zero at either end returns the function 0: . This is the easiest way to generate no geometry for a segment.

```
  hList=:0:`0:`0:`0:`0:`0:`h11`h12`h13`0:`0:`h21`h22`h23`0:`0:`h31`h32`h33`0:`0:`0:`0:`0:`0:
  hListCut1=:0:`0:`0:`0:`0:`0:`h11`}:@}:@h12`h13`0:`0:`h21`}.@h22`h23`0:`0:`h31`}:@}:@h32`h33`0:`0:`0:`0:`0:`0:
  hListCut2=:0:`0:`0:`0:`0:`0:`h11`h12`h13`0:`0:`}:@}:@h21`}:@h22`}:@}:@h23`0:`0:`h31`h32`h33`0:`0:`0:`0:`0:`0:

  dList=:0:`0:`0:`0:`0:`0:`d11`d12`d13`0:`0:`d21`d22`d23`0:`0:`d31`d32`d33`0:`0:`0:`0:`0:`0:
  dListCut=:0:`0:`0:`0:`0:`0:`d11`}:@d12`d13`0:`0:`}:@d21`}.@}:@d22`}:@d23`0:`0:`d31`}:@d32`d33`0:`0:`0:`0:`0:`0:
  segIndex=:(5 5)&#.@]

  topSeg=:hListCut2@.segIndex
  bottomSeg=:flipVert@hListCut1@.segIndex
  leftSeg=:rotLeft@hListCut1@.segIndex@(4:-])
  rightSeg=:rotLeft@flipVert@hListCut2@.segIndex@(4:-])

  sloshSeg=:dListCut@.segIndex
  slashSeg=:rotLeft@dList@.segIndex@(4:-])
```

To build the geometry for a square, call the selectAllGeometry function. This takes a list of 5 integers: the square crossing type followed by the top left, top right, bottom left and bottom right corners. It separates out the appropriate corners and passes them to the segment functions.

```
  selectAllGeometry=:((leftSeg@(3 1&{);rightSeg@(4
  2&{))`(sloshSeg@(1 4&{);slashSeg@(3 2&{))`(topSeg@(1
  2&{);bottomSeg@(3 4&{)))@.{.
```

The output is two boxed lists, which must be unboxed before sending to knotComplex.

```
   >(gdshow ''),((knotComplex@>@{.,knotComplex@>@{:)
   selectAllGeometry 1 2 1 1 3),(gdopen '')
```

\[pics\]

## Putting the Squares Together

Since the squares fill the graph window, multiple squares must be moved into place and scaled down. Each corner must be used correctly by the four surrounding squares if the geometry is to match up.

```
  getCornerIndices=:<@((0 1)&+),<@((1 1)&+),<@((0 0)&+),<@((1 0)&+)
  getCombinedList=:([(<@[{])>@{.@]),([(getCornerIndices@[{])>@{:@])
  getSquareGeometry=:selectAllGeometry@(]getCombinedList[)
  drawPositioned=:knotComplex@(({.@[%~{.@}.@[-+:@{:@[+1j1&+@>@]))
  drawSquare=:(>./@$@>@{.@[,j./@$@>@{.@[,j./@])
  (([drawPositioned{.@]),([drawPositioned{:@])) getSquareGeometry
```

The input to getCombinedList is a boxed list of crossovers and corners as its right argument, and the coordinates within them which it is to look up as its left argument. It calls getCornerIndices to turn its coordinates into the four corner coordinates within the corners list. The output is the list of five integers that describes the requested square.

The drawPositioned function takes a list of three numbers as its left hand argument:

1. **Scaling factor**: all coordinates are divided by this amount. It's the greater of the side lengths of the input rectangle.
2. **Complex size:** a complex form of the side lengths of the input rectangle. This is subtracted from the coordinates to centre the knot.
3. **Square coordinates:** where, in the original rectangle coordinates this segment will be drawn.

The right hand argument of drawSquare is the complex list of geometry to be manipulated.

Input for drawPositioned is generated by drawSquare. The left argument of drawSquare should be a boxed list, with the first item being the crossovers and the second being the corners. The right argument should be the coordinates of the square to draw.

```
  allIndices=:({.(|,.<.@%~)i.@*/)@$@>@{.
  drawKnots=:]drawSquare"1 allIndices
  showKnots=:gdshow,drawKnots,gdopen@$@0:
```

To draw all the squares at once, a list of all the square coordinates is needed. This is returned by allIndices, which reads what it needs out of the crossovers list. This is used by drawKnots to draw every square.

Since a graphics window must be open, the showKnots function provides a simple way to open a window, draw a knot and show it. All that's missing is some boxed knot information to feed into it.

## Generating the tables

The simplest way to create the knotwork tables is with random numbers. This is easy, but doesn't generate very nice knots.

```
  >corners=:>: ?. 11 5 $ 3
  >crossovers=:?. 10 4 $ 3
  >showKnots crossovers;corners
```

\[pic\]

Grids with rotational symmetry work a lot better. The easiest way to generate them is to generate a section, flip it over and copy it. One of the grids should only have its central row used once. In this case, the ‘corners’ array is chosen; its central row is replaced with twos, for diagonal crossing.

```
  flipAndDuplicate=:,(|."1@|.)
  flipAndDuplicateRemove=:,(|."1@|.@}:)

  >crossovers=:flipAndDuplicate
  ?. 6 3 $ 3 >corners=:flipAndDuplicateRemove
  (>: ?. 6 4 $ 3),2
  >showKnots crossovers;corners
```

\[pics\]

This is better, but the edges are untidy. These can be corrected by making two changes:

1. The crossings along the top and bottom must be horizontal and the crossings to the left and right must be vertical.
2. Either the top and bottom or the left and right corners must be zero, indicating no line segment at all.

```
  >crossovers=:flipAndDuplicate 0,(?. 5 3 $ 3)
  >corners=:flipAndDuplicateRemove (0,(3,.(>: ?. 5 2 $ 3),2),.3)
  >showKnots crossovers;corners
```

\[pics\]

The knots generally look better if they contain more crossover segments. This can be achieved by taking elements from a list that favours crossovers.

```
  >crossovers=:flipAndDuplicate 0,(?. 5 3 $ 5) { 0 1 1 1 2
  >corners=:flipAndDuplicateRemove (0,(3,.((?. 5 2 $ 5) { 1 2 2 2 3),2),.3)
  >showKnots crossovers;corners
```

\[pics\]

The entertaining part of Celtic knotwork is creating new designs, so the next step is to change from the ?. to the ? pseudorandom function.

```
  >crossovers=:flipAndDuplicate 0,(? 5 3 $ 5) { 0 1 1 1 2
  >corners=:flipAndDuplicateRemove (0,(3,.((? 5 2 $ 5) { 1 2 2 2 3),2),.3)
  >showKnots crossovers;corners
```

\[pics\]

It is also possible to request a knot of any size, although the overlapping lines will generate gaps on large knots. Always make sure that the corners table is one unit larger in both dimensions than the crossovers table.

```
  >crossovers=:flipAndDuplicate 0,(? 7 16 $ 5) { 0 1 1 1 2
  >corners=:flipAndDuplicateRemove (0,(3,.((? 7 15 $ 5) { 1 2 2 2 3),2),.3)
  >showKnots crossovers;corners
```

\[pics\]

Another option is to generate repeating borders.

```
  >crossovers=:((],],],]) (? 4 4 $ 5) { 0 1 1 1 2)
  >corners=:((],],],],{.) (3,.((? 4 3 $ 5) { 1 2 2 2 3),.3))
  >showKnots crossovers;corners
```

\[pics\]

Programmers are welcome to invent their own crossover and corner tables.

## Discussion

My first attempt at drawing Celtic knots used pairs of integers for geometry and was terribly cumbersome. The complex number coordinates made every operation much simpler.

Graph theorists may wish to see if they can rewrite the construction functions to guarantee a single continuous loop. Small independent circles are not a common part of traditional Celtic knots; this program generates them with regrettable frequency. An alternative would be to create user interface code so the knot can be adjusted after it is drawn.

**References**
:   <a id="ref1"></a>\[1\] A Sloss, *How to Draw Celtic Knotwork: A Practical Handbook*, Blandford Press, 1997
:   \[2\] Wilson, Anne, *Over and Under: Celtic Knotwork Patterns*, Vector Vol.17.4, p.101

***

[www.vector.org.uk](../ "VECTOR home") > [archive](../) > [Vol.21](../) >  [![V21:1](/images/covers/32x45/v2101.jpg) No.1](../21/#n1-september-2004)
