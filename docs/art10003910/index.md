---
vid: '10003910'
title: A Sudoku Solver in J
authors:
- Roger Hui
byline: by Roger Hui
volume: '21'
issue: '4'
page: '49'
source: trad/v214/sudoku.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- competitions and puzzles
- programming techniques
---

*by Roger Hui*
{ .byline }

Fill the grid so that each row, column, and 3 by 3 box contains the digits 1 through 9.

```
  +-----+-----+-----+
  |2 0 0|6 7 0|0 0 0|
  |0 0 6|0 0 0|2 0 1|
  |4 0 0|0 0 0|8 0 0|
  +-----+-----+-----+
  |5 0 0|0 0 9|3 0 0|
  |0 3 0|0 0 0|0 5 0|
  |0 0 2|8 0 0|0 0 7|
  +-----+-----+-----+
  |0 0 1|0 0 0|0 0 4|
  |7 0 8|0 0 0|6 0 0|
  |0 0 0|0 5 3|0 0 8|
  +-----+-----+-----+
```

Welcome to Sudoku.

Sudoku is a popular puzzle in Japan (*su* is number, *doku* is place), to where it was imported from the U.S. It was popularized in the West by Wayne Gould, a New Zealander living in Hong Kong. He maintains a website [http://www.sudoku.com](http://www.sudoku.com) where you can find descriptions, examples, tutorials, and download a puzzle player. In a November 2004 article in the Times,  [http://www.timesonline.co.uk/article/0,,18209-1354181,00.html](http://www.timesonline.co.uk/article/0,,18209-1354181,00.html), Gould was quoted as saying that some Sudoku puzzles are so difficult that you can’t solve them if your life depended on it.

The following Sudoku solver uses a simple but effective strategy. Even puzzles rated as “very hard” require no more than 15 milliseconds and 30 Kbytes on a 500 MHz Pentium 3 computer.

```
j      =. (]/. i.@#) ,{;~3#i.3
r      =. 9#i.9 9
c      =. 81$|:i.9 9
b      =. (,j{9#i.9) { j

I      =: ~."1 r,.c,.b
R      =: j,(,|:)i.9 9

regions=: R"_ {"_ 1 ]
free   =: 0&= > (1+i.9)"_ e."1 I&{
ok     =: (27 9$1)"_ -:"2 (0&= +. ~:"1)@regions

ac     =: +/ .*&(1+i.9) * 1: = +/"1

ar     =: 3 : 0
 m=. 1=+/"2 R{y.
 j=. I. +./"1 m
 k=. 1 i."1~ j{m
 i=. ,(k{"_1 |:"2 (j{R){y.) #"1 j{R
 (1+k) i}81$0
)

assign =: (+ (ac >. ar)@free)^:_"1

guessa =: 3 : 0
 if. -. 0 e. y. do. ,:y. return. end.
 b=. free y.
 i=. (i.<./) (+/"1 b){10,}.i.10
 y. +"1 (1+I.i{b)*/i=i.81
)

guess  =: ; @: (<@guessa"1)
sudoku =: guess @: (ok # ]) @: assign ^:_ @ ,

see1   =: (;~9$1 0 0)&(<;.1) @ ({&'.123456789') @ (9 9&$) @ ,
see    =: <@see1"1`see1@.(1:=#@$)
diff   =: * 0&=@}:@(0&,)
```

A grid is the ravel of a `9 9` matrix of cells of `i.10` . A box is a 9-element subset of a grid, the ravel of one of the `3 3` regions.

A region is a row, column, or box. The object of Sudoku is to assign numbers to the zero cells of a grid `x`  while leaving unchanged the non-zero cells in `x`, so that each region has exactly the elements `1+i.9` .

`j` are the indices in a ravelled grid for each box. `r` are the indices for each row. `c` are the indices for each column. `b` are the indices for each box. Finally, `I` are the indices in a ravelled grid for regions that contain a cell, for each cell of a grid.

`regions x` computes a `27 9` matrix of the 27 regions of grid `x` . `free x`  computes a `81 9`  boolean array `y` such that `(<((9*i)+j),k){y`  is `1` if `1+k` can be assigned to cell `i,j`  of grid `x` . `ok` applies to one or more grids and returns a `1` for each valid grid.

`ac` and `ar` apply to the free list of a grid. `ac` assigns numbers to cells that have only one candidate. `ar` looks for a number which occurs exactly once in the candidates for a region, and assigns that to the cell for which it is a candidate. `ac` and `ar`  correspond to “forced moves”. (When `ac` or `ar` is applied to an “impossible” grid, the result can be assignments that are obviously in error.) `assign`  repeatedly applies `ac`  and `ar` to one or more grids until there are no more changes.

`guessa x` applies to to grid `x` and returns one or more grids with cells fill in with all possible candidates, for a cell that has the smallest set of candidates. `guess` applies `guessa` to one or more grids and returns all the grids generated thereby.

`sudoku x` finds all solutions for grid `x` . An error is signalled if `x` has no solution.

```
x=: , 0 ". ] ;._2 (0 : 0)
 2 0 0  6 7 0  0 0 0
 0 0 6  0 0 0  2 0 1
 4 0 0  0 0 0  8 0 0
 5 0 0  0 0 9  3 0 0
 0 3 0  0 0 0  0 5 0
 0 0 2  8 0 0  0 0 7
 0 0 1  0 0 0  0 0 4
 7 0 8  0 0 0  6 0 0
 0 0 0  0 5 3  0 0 8
)
   see x, sudoku x
+-------------+-------------+
|+---+---+---+|+---+---+---+|
||2..|67.|...|||283|671|945||
||..6|...|2.1|||976|548|231||
||4..|...|8..|||415|392|876||
|+---+---+---+|+---+---+---+|
||5..|..9|3..|||567|419|382||
||.3.|...|.5.|||834|267|159||
||..2|8..|..7|||192|835|467||
|+---+---+---+|+---+---+---+|
||..1|...|..4|||321|786|594||
||7.8|...|6..|||758|924|613||
||...|.53|..8|||649|153|728||
|+---+---+---+|+---+---+---+|
+-------------+-------------+
```

The following phrases show the intermediate steps leading to a solution.

<table>
        <tr valign="baseline">
          <td><tt class="APLU">f=: + (ac &gt;. ar)@free</tt></td>
          <td width="20"> </td>
          <td>one step of<tt class="APLU"> assign</tt></td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see t=: f^:a: x</tt></td>
          <td> </td>
          <td>forced moves leading from grid<tt class="APLU"> x</tt></td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see diff t</tt></td>
          <td> </td>
          <td>differences from one grid to the next</td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see assign x</tt></td>
          <td> </td>
          <td>same as the last grid above</td>
        </tr>
        <tr valign="baseline">
          <td nowrap><tt class="APLU">see g=: guess (ok#]) assign x</tt></td>
          <td> </td>
          <td>guesses after exhausting forced moves</td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see t0=: f^:a: 0{g</tt></td>
          <td> </td>
          <td>forced moves leading from guess 0</td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see diff t0</tt></td>
          <td> </td>
          <td>differences from one grid to the next</td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see t1=: f^:a: 1{g</tt></td>
          <td> </td>
          <td>forced moves leading from guess 1</td>
        </tr>
        <tr valign="baseline">
          <td><tt class="APLU">see diff t1</tt></td>
          <td> </td>
          <td>differences from one grid to the next; note
the obviously invalid assignments</td>
        </tr>
</table>
