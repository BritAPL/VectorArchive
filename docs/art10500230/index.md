---
vid: '10500230'
title: Ten divisions to Easter
authors:
- Ray Polivka
byline: Ray Polivka (polivkar@juno.com)
volume: '24'
issue: '2'
page: '89'
source: content/printed/242/polivka.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- programming techniques
- competitions and puzzles
---

Ray Polivka (polivkar@juno.com)
{ .byline }

See also Ray Cannon’s article [“When is Easter”](../art10001830/) in *Vector* 10.3. *Ed.*
{ .ednote }

No doubt many of you have heard of the problem of determining the calendar date of Easter. Perhaps you have seen a program solving this problem or have written one.

While browsing in one of my mathematical puzzle books, I found this intriguing solution. It is an unambiguous algorithm, involving only ten divisions, that produces the proper date for Easter given only an integer representing the year for which you wish it. This intriguing algorithm is described by T.H. O’Beirne in his book entitled *Puzzles and Paradoxes*.[\[1\]](#ref1) He described the algorithm in great detail. This is preceded by a lengthy and fascinating history of the calculation of Easter. The basic difficulties in computing the date of Easter occur because of the discrepancies between the ecclesiastical needs and astronomical data.

This ten-division algorithm is organized as a table. In presenting this table, O’Beirne stated “we shall give a purely number rule – subject to no exceptions of any kind – which calculates the date of Easter from a knowledge only of the year number, with no reference to any other rules or tables.” He went on to state his tabular scheme is based on a correct procedure published in *Nature* in 1876 by an anonymous New York author. He then said “Based on this procedure, we have developed similar methods of our own, which are now published for the first time.” Here is the table he gave.

<table>
			<caption>First Ten-Division Table</caption>
			<thead>
				<tr><th>Step</th>	<th>Dividend</th>				<th>Divisor</th>	<th>Quotient</th> <th>Remainder</th></tr>
			</thead>
			<tbody>
				<tr><td>1.</td>		<td>x</td>							<td>19</td>				<td>–</td>				<td>a</td>	</tr>
				<tr><td>2.</td>		<td>x</td>							<td>100</td>			<td>b</td>				<td>c</td>	</tr>
				<tr><td>3.</td>		<td>b</td>							<td>4</td>				<td>d</td>				<td>e</td>	</tr>
				<tr><td>4.</td>		<td>8b+13</td>					<td>25</td>				<td>g</td>				<td>–</td>	</tr>
				<tr><td>5.</td>		<td>19a+b-d-g+15</td>		<td>30</td>				<td>–</td>				<td>h</td>	</tr>
				<tr><td>6.</td>		<td>a+11h</td>					<td>319</td>			<td>u</td>				<td>–</td>	</tr>
				<tr><td>7.</td>		<td>c</td>							<td>4</td>				<td>i</td>				<td>k</td>	</tr>
				<tr><td>8.</td>		<td>2e+2i-k-h+u+32</td>	<td>7</td>				<td>–</td>				<td>q</td>	</tr>
				<tr><td>9.</td>		<td>h-u+q+90</td>				<td>25</td>				<td>n</td>				<td>–</td>	</tr>
				<tr><td>10.</td>	<td>h-u+q+n+19</td>			<td>32</td>				<td>–</td>				<td>p</td>	</tr>
			</tbody>
		</table>

In the <em>x</em>th year A.D. of the Gregorian calendar, Easter Sunday is the <em>p</em>th day of the <em>n</em>th month. The date of the Easter full moon is obtained if -1 is substituted for q in steps 9 and 10.

O’Beirne also included a second table in which he felt computation may be easier. This is the second table.

<table>
			<caption>Second Ten-Division Table</caption>
			<thead>
				<tr><th>Step</th>	<th>Dividend</th>	<th>Divisor</th>	<th>Quotient</th>	<th>Remainder</th></tr>
			</thead>
			<tbody>
				<tr><td>1.</td>		<td>x</td>				<td>100</td>			<td>b</td>				<td>c</td></tr>
				<tr><td>2.</td>		<td>5b+c</td>			<td>19</td>				<td>–</td>				<td>a</td></tr>
				<tr><td>3.</td>		<td>3(b+25)</td>	<td>4</td>				<td>d</td>				<td>e</td></tr>
				<tr><td>4.</td>		<td>8(b+11)</td>	<td>25</td>				<td>m</td>				<td>–</td></tr>
				<tr><td>5.</td>		<td>19a+d-m</td>	<td>30</td>				<td>–</td>				<td>h</td></tr>
				<tr><td>6.</td>		<td>a+11h</td>		<td>319</td>			<td>n</td>				<td>–</td></tr>
				<tr><td>7.</td>		<td>60(5-e)+c</td><td>4</td>				<td>j</td>				<td>k</td></tr>
				<tr><td>8.</td>		<td>2j-k-h+n</td>	<td>7</td>				<td>–</td>				<td>w</td></tr>
				<tr><td>9.</td>		<td>h-n+w+110</td><td>30</td>				<td>n</td>				<td>q</td></tr>
				<tr><td>10.</td>	<td>q+5-n</td>		<td>32</td>			<td>–</td>			<td>p</td></tr>
			</tbody>
		</table>

Again, in the <em>x</em>th year A.D. of the Gregorian calendar, Easter Sunday is the <em>p</em>th day of the <em>n</em>th month.

This is a fine problem for someone learning APL. It is a nice illustration of *encode*. The problem could be stated as:

> Create a function `easter` that, given the year, will calculate the date of Easter using the second Ten Division Table.
>
> ```
>       easter 2009
> Easter is April 12, 2009
> ```

Here is a solution using the first table.

```
∇ z←easter y;⎕ML;a;b;c;d;e;g;h;i;k;l;m;n;p;q;u
 ⍝ y: year as an integer
 ⍝ z: date of Easter
  ⎕ML←3
  a←19⊤y
  (b c)←0 100⊤y
  (d e)←0 4⊤b
  g←↑0 25⊤13+8×b
  h←30⊤(19×a)+b+15-d+g
  u←↑0 319⊤a+11×h
  (i k)←0 4⊤c
  q←7⊤(2×e+i)+u+32-k+h
  n←↑0 25⊤h+90+q-u
  p←32⊤h+q+n+19-u
  z←'Easter is ',(↑(n=4)⌽'March' 'April'),' ',(⍕p),', ',⍕y
∇
```

## References

1. <a id="ref1"></a> O’Beirne, T.H., *Puzzles and Paradoxes*, Oxford University Press, 1965, pages 168-184
