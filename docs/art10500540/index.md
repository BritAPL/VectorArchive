---
vid: '10500540'
title: Odd-order magic squares expressed in J
authors:
- John C. McInturff
byline: John C. McInturff
volume: '25'
issue: '1'
page: '119'
received: '2010-09-26'
online: 2011-04-14 15:55
editor: Beau Webber
description: An elegant implementation of the classic Magic Squares
abstract: This note illustrates an array-oriented approach to solving odd-ordered magic squares. Part 1 illustrates a computer-sensible solution expressed in J. The resulting solution is then subjected to eight symmetrical transformations and each tested for ‘magic properties’. Part 2 describes the underlying two-step rule for the solution, and illustrates how this rule can be applied to the entire magic square (matrix), and carried out graphically without a computer. Each graphical step is made computer-sensible and is executed.
source: content/printed/251/mcinturff.htm
converter: vecrescue 0.1.0
issue_tab: n1-june-2011
tags:
- mathematics and statistics
- programming techniques
---

John C. McInturff
{ .byline }

This note illustrates an array-oriented approach to solving odd-ordered magic squares. Part 1 illustrates a computer-sensible solution expressed in J. The resulting solution is then subjected to eight symmetrical transformations and each tested for ‘magic properties’. Part 2 describes the underlying two-step rule for the solution, and illustrates how this rule can be applied to the entire magic square (matrix), and carried out graphically without a computer. Each graphical step is made computer-sensible and is executed.
{ .abstract }

## Part 1

The following verb, `MS` is an array-oriented solution to an odd-ordered Magic Square, expressed in J. This expression is intended to minimise keystrokes, not maximise the understanding of the thought behind it. The latter objective is addressed in Part 2.

```
   MS=. 3 : ' (1|. N) P |: (N=. ,.i.y) (P=. |."1) (<.-:y) |. >:i.2# y'
```

Shown below are four examples, for `n=. 3 5 7 9`. The verb, *each*, is: `ea=. &.>`

```
   MS ea 3 5 7 9
+--------------------------------------------------------------------+
¦8 1 6¦17 24  1  8 15¦30 39 48  1 10 19 28¦47 58 69 80  1 12 23 34 45¦
¦3 5 7¦23  5  7 14 16¦38 47  7  9 18 27 29¦57 68 79  9 11 22 33 44 46¦
¦4 9 2¦ 4  6 13 20 22¦46  6  8 17 26 35 37¦67 78  8 10 21 32 43 54 56¦
¦     ¦10 12 19 21  3¦ 5 14 16 25 34 36 45¦77  7 18 20 31 42 53 55 66¦
¦     ¦11 18 25  2  9¦13 15 24 33 42 44  4¦ 6 17 19 30 41 52 63 65 76¦
¦     ¦              ¦21 23 32 41 43  3 12¦16 27 29 40 51 62 64 75  5¦
¦     ¦              ¦22 31 40 49  2 11 20¦26 28 39 50 61 72 74  4 15¦
¦     ¦              ¦                    ¦36 38 49 60 71 73  3 14 25¦
¦     ¦              ¦                    ¦37 48 59 70 81  2 13 24 35¦
+--------------------------------------------------------------------+
```

The sum of each row, column, right diagonal, and left diagonal, is required to be equal to the value known as the *magic value* produced by the verb `val=. [:-:]*1+*:`.

The magic value for *each* magic square above is therefore:

```
   val ea 3 5 7 9
+-------------+
¦15¦65¦175¦369¦
+-------------+
```

The question is: do the above matrices satisfy the ‘magic requirement’? The answer requires n calculations where n is equal to the magic value plus the two diagonals plus double the number of sides for each matrix. For matrices of order 3, 5, 7, 9 and 41, n would be:

```
   ]n=. (1+2++:) ea 3 5 7 9 41
+-------------+
¦9¦13¦17¦21¦85¦
+-------------+
```

The objective now is to see if the four matrices meet the above conditions. The sum of each row, column, left diagonal, and right diagonal is given by the following verbs:

```
   row=. +/"1
   col=. +/"2
   d1=.[: +/ (<0 1) |: ]
   d2=. [: +/ (<0 1) |: |.
   v=. [: val #
```

For brevity, the following verb `f` combines the above 5 verbs and illustrates an example of its use for `n=. 5`

```
   f=. (v;' ';row;col;d1;d2)
   f (MS 5)
+----------------------------------------+
¦65¦ ¦65 65 65 65 65¦65 65 65 65 65¦65¦65¦
+----------------------------------------+
```

It is seen that the order-5 matrix took thirteen calculations and met all conditions. An order-41 matrix would require 85 conditions. The following verb `Test`, when applied to the matrix order, takes all of the above requirements into account and returns a 1 if all conditions are met; e.g.,

```
   Test=. 3 : 0
   n=. val y
   q=. MS y
   *./n=(,>(row q)),(col q)),(d1 q),(d2 q)
   )
```

This will now be illustrated for all odd matrices from order 3 through 41.

```
   odd=. (1+2*i.21)
   n=.(3++:) odd
   test=. Test ea odd

   >< ea odd,n,:(>test)
+------------------------------------------------------------+
¦1¦3¦5 ¦7 ¦9 ¦11¦13¦15¦17¦19¦21¦23¦25¦27¦29¦31¦33¦35¦37¦39¦41¦
+-+-+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--¦
¦5¦9¦13¦17¦21¦25¦29¦33¦37¦41¦45¦49¦53¦57¦61¦65¦69¦73¦77¦81¦85¦
+-+-+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--¦
¦1¦1¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦1 ¦
+------------------------------------------------------------+
```

The value of a magic square is unaffected by the following eight transformations. These are the identity transformation `t0`, plus three clockwise rotations and their four respective reflections. There are, therefore, eight magic squares associated with the verb `MS`.

These eight transformations are expressed by the following eight verbs and are illustrated below:

```
   t0=: ]
   t1=: t6@t7
   t2=: t4@t6
   t3=: |.@|:
   t4=: |.@]
   t5=: t2@t7
   t6=: |."_1@]
   t7=: |:@]
```

For brevity, these eight transformations are combined into the single verb, `t`, and are more clearly illustrated below for the rectangular 3×2 matrix, `a`.

```
   t=. (t0;t1;t2;t3;t4;t5;t6;t7)

   ]a=. >:i.3 2
1 2
3 4
5 6
```

```
   ]T=. t a
+---------------------------------------+
¦1 2¦5 3 1¦6 5¦2 4 6¦5 6¦6 4 2¦2 1¦1 3 5¦
¦3 4¦6 4 2¦4 3¦1 3 5¦3 4¦5 3 1¦4 3¦2 4 6¦
¦5 6¦     ¦2 1¦     ¦1 2¦     ¦6 5¦     ¦
+---------------------------------------+
```

The eight magic squares are therefore:

```
   t (MS 3)
+-----------------------------------------------+
¦8 1 6¦4 3 8¦2 9 4¦6 7 2¦4 9 2¦2 7 6¦6 1 8¦8 3 4¦
¦3 5 7¦9 5 1¦7 5 3¦1 5 9¦3 5 7¦9 5 1¦7 5 3¦1 5 9¦
¦4 9 2¦2 7 6¦6 1 8¦8 3 4¦8 1 6¦4 3 8¦2 9 4¦6 7 2¦
+-----------------------------------------------+
```

## Part 2

The third magic square of the eight above is the 650-BCE Lo Shu magic square[\[1\]](#ref1), often credited as being the first recorded magic square. (Somehow it got on the back of a turtle!)

The French diplomat Simon de la Loubère 1642-1749[\[2\]](#ref2) published the following “well known” rule for solving odd-ordered magic squares.

1. Initialise a square grid (matrix) by placing the integer 1 in the center column of the first row.
2. Place the next number, 2, in the square diagonally up and to the right.
    1. If filled, move vertically down one square,
    2. If ‘off the square’, wrap around (odometer-wise) to the last row, or first column, respectively.
3. Continue with the next number 3 etc. (repeating the above rule if necessary) until the square is filled.

One can start with any number other than 1 and follow the above rule to derive other magic squares belonging to the group of eight mentioned in Part 1.

Although the above rule involves a single number at a time and an iterative process, it can be carried out extremely rapidly by hand. Furthermore, it is the basis for the thought process that takes place when one applies the same principle to the matrix as a whole and follows the same two-step rule.

1. The first step is to initialise the matrix such that the integer 1 will always appear in the centre column of an outside edge. This is assured by verb `f2` in the Appendix.
2. The second step is to move the entire matrix diagonally up and to the right. The term *move* is a vector instruction that successively shifts each row of the matrix, as illustrated in the Appendix.

Two important comments must be made:

- For the array-oriented language used, it may be more desirable to ‘translate’ the matrix and move equivalently, left and up as was done in the verb `MS`.
- *Left* and *up* can be thought of as two degrees of freedom. If one has only one degree of freedom, it can be equivalently accomplished by an anti-clockwise translation and a left shift as was done in the verb `MS`.

The array approach using the above method can be carried out by hand as well as executed by a computer as described in detail in the Appendix.

## References

1. <a id="ref1"></a> Lo Shu Square  
    [http://en.wikipedia.org/wiki/Lo\_Shu\_Square](http://en.wikipedia.org/wiki/Lo_Shu_Square)
2. <a id="ref2"></a> Simon de la Loubère  
    [http://en.wikipedia.org/wiki/Simon\_de\_la\_Loub%C3%A8re](http://en.wikipedia.org/wiki/Simon_de_la_Loub%C3%A8re)

## Appendix

<table>
			<tr>
				<th style="border-right: 1px solid gray; text-align: center">Graphical</th>
				<th style="text-align: center">Computer</th>
			</tr>
			<tr>
				<td style="border-right: 1px solid gray; vertical-align: bottom;">

					<br>
					<table style="border: 1px solid gray">
						<caption>0. Populate Matrix</caption>
						<tr align="right">
							<td>1</td>
							<td>2</td>
							<td>3</td>
							<td>4</td>
							<td>5</td>
						</tr>
						<tr align="right">
							<td>6</td>
							<td>7</td>
							<td>8</td>
							<td>9</td>
							<td>10</td>
						</tr>
						<tr align="right">
							<td>11</td>
							<td>12</td>
							<td>13</td>
							<td>14</td>
							<td>15</td>
						</tr>
						<tr align="right">
							<td>16</td>
							<td>17</td>
							<td>18</td>
							<td>19</td>
							<td>20</td>
						</tr>
						<tr align="right">
							<td>21</td>
							<td>22</td>
							<td>23</td>
							<td>24</td>
							<td>25</td>
						</tr>
					</table>
				</td>
				<td class="code">
					<table>
						<tr>
							<td style="width: 60%; text-align: left; white-space: pre">   y=. 5</td>
							<td style="width: 40%; text-align: left; white-space: pre">   ]a=. f1 y</td>
						</tr>
						<tr>
							<td style="width: 60%; vertical-align: top; white-space: pre">   f1=. [:&gt;:[:i.2#]</td>
							<td style="width: 40%;" align="right">
								<table border="0" cellpadding="2" cellspacing="2">
									<tr align="right">
										<td>1</td>
										<td>2</td>
										<td>3</td>
										<td>4</td>
										<td>5</td>
									</tr>
									<tr align="right">
										<td>6</td>
										<td>7</td>
										<td>8</td>
										<td>9</td>
										<td>10</td>
									</tr>
									<tr align="right">
										<td>11</td>
										<td>12</td>
										<td>13</td>
										<td>14</td>
										<td>15</td>
									</tr>
									<tr align="right">
										<td>16</td>
										<td>17</td>
										<td>18</td>
										<td>19</td>
										<td>20</td>
									</tr>
									<tr align="right">
										<td>21</td>
										<td>22</td>
										<td>23</td>
										<td>24</td>
										<td>25</td>
									</tr>
								</table>
							</td>
						</tr>
					</table>
				</td>
			</tr>
			<tr>
				<td style="border-right: 1px solid gray; vertical-align: bottom;">
					<table style="border: 1px solid gray">
						<caption>1. Initialize Matrix</caption>
						<tr align="right">
							<td>11</td>
							<td>12</td>
							<td>13</td>
							<td>14</td>
							<td>15</td>
						</tr>
						<tr align="right">
							<td>16</td>
							<td>17</td>
							<td>18</td>
							<td>19</td>
							<td>20</td>
						</tr>
						<tr align="right">
							<td>21</td>
							<td>22</td>
							<td>23</td>
							<td>24</td>
							<td>25</td>
						</tr>
						<tr align="right">
							<td>1</td>
							<td>2</td>
							<td>3</td>
							<td>4</td>
							<td>5</td>
						</tr>
						<tr align="right">
							<td>6</td>
							<td>7</td>
							<td>8</td>
							<td>9</td>
							<td>10</td>
						</tr>
					</table>
				</td>
				<td class="code">
					<table>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   (f2 y);a</td>
							<td style="text-align: left; white-space: pre; width: 40%;">   ]b=. (f2 y) |. a</td>
						</tr>
						<tr>
							<td>
								<table>
									<tr>
										<td valign="top" style="border: 1px solid gray">
											2
										</td>
										<td valign="top" style="border: 1px solid gray">
											<table>
												<tr align="right">
													<td>1</td>
													<td>2</td>
													<td>3</td>
													<td>4</td>
													<td>5</td>
												</tr>
												<tr align="right">
													<td>6</td>
													<td>7</td>
													<td>8</td>
													<td>9</td>
													<td>10</td>
												</tr>
												<tr align="right">
													<td>11</td>
													<td>12</td>
													<td>13</td>
													<td>14</td>
													<td>15</td>
												</tr>
												<tr align="right">
													<td>16</td>
													<td>17</td>
													<td>18</td>
													<td>19</td>
													<td>20</td>
												</tr>
												<tr align="right">
													<td>21</td>
													<td>22</td>
													<td>23</td>
													<td>24</td>
													<td>25</td>
												</tr>
											</table>
										</td>
									</tr>
								</table>
							</td>
							<td>
								<table>
									<tr align="right">
										<td>11</td>
										<td>12</td>
										<td>13</td>
										<td>14</td>
										<td>15</td>
									</tr>
									<tr align="right">
										<td>16</td>
										<td>17</td>
										<td>18</td>
										<td>19</td>
										<td>20</td>
									</tr>
									<tr align="right">
										<td>21</td>
										<td>22</td>
										<td>23</td>
										<td>24</td>
										<td>25</td>
									</tr>
									<tr align="right">
										<td>1</td>
										<td>2</td>
										<td>3</td>
										<td>4</td>
										<td>5</td>
									</tr>
									<tr align="right">
										<td>6</td>
										<td>7</td>
										<td>8</td>
										<td>9</td>
										<td>10</td>
									</tr>
								</table>
							</td>
						</tr>
					</table>
				</td>
			</tr>
			<tr>
				<td style="border-right: 1px solid gray; vertical-align: bottom;">
					<table style="border: 1px solid gray">
						<caption>2a. LEFT shift</caption>
						<tr align="right">
							<td>11</td>
							<td>12</td>
							<td>13</td>
							<td>14</td>
							<td>15</td>
						</tr>
						<tr align="right">
							<td>17</td>
							<td>18</td>
							<td>19</td>
							<td>20</td>
							<td>16</td>
						</tr>
						<tr align="right">
							<td>23</td>
							<td>24</td>
							<td>25</td>
							<td>21</td>
							<td>22</td>
						</tr>
						<tr align="right">
							<td>4</td>
							<td>5</td>
							<td>1</td>
							<td>2</td>
							<td>3</td>
						</tr>
						<tr align="right">
							<td>10</td>
							<td>6</td>
							<td>7</td>
							<td>8</td>
							<td>9</td>
						</tr>
					</table>
				</td>
				<td class="code">
					<table>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   n=. [:,.i.</td>
						</tr>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   N=. n y</td>
						</tr>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   N;' ';b</td>
						</tr>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   P=.|."1 </td>
							<td style="text-align: left; white-space: pre; width: 40%;">   ]c=. N P b</td>
						</tr>
						<tr>
							<td>
								<table>
									<tr>
										<td style="border: 1px solid gray">
											<table>
												<tr align="right">
													<td>0</td>
												</tr>
												<tr align="right">
													<td>1</td>
												</tr>
												<tr align="right">
													<td>2</td>
												</tr>
												<tr align="right">
													<td>3</td>
												</tr>
												<tr align="right">
													<td>4</td>
												</tr>
											</table>
										</td>
										<td style="border: 1px solid gray"></td>
										<td style="border: 1px solid gray">
											<table>
												<tr align="right">
													<td>11</td>
													<td>12</td>
													<td>13</td>
													<td>14</td>
													<td>15</td>
												</tr>
												<tr align="right">
													<td>16</td>
													<td>17</td>
													<td>18</td>
													<td>19</td>
													<td>20</td>
												</tr>
												<tr align="right">
													<td>21</td>
													<td>22</td>
													<td>23</td>
													<td>24</td>
													<td>25</td>
												</tr>
												<tr align="right">
													<td>1</td>
													<td>2</td>
													<td>3</td>
													<td>4</td>
													<td>5</td>
												</tr>
												<tr align="right">
													<td>6</td>
													<td>7</td>
													<td>8</td>
													<td>9</td>
													<td>10</td>
												</tr>
											</table>
										</td>
									</tr>
								</table>
							</td>
							<td>
								<table>
									<tr align="right">
										<td>11</td>
										<td>12</td>
										<td>13</td>
										<td>14</td>
										<td>15</td>
									</tr>
									<tr align="right">
										<td>17</td>
										<td>18</td>
										<td>19</td>
										<td>20</td>
										<td>16</td>
									</tr>
									<tr align="right">
										<td>23</td>
										<td>24</td>
										<td>25</td>
										<td>21</td>
										<td>22</td>
									</tr>
									<tr align="right">
										<td>4</td>
										<td>5</td>
										<td>1</td>
										<td>2</td>
										<td>3</td>
									</tr>
									<tr align="right">
										<td>10</td>
										<td>6</td>
										<td>7</td>
										<td>8</td>
										<td>9</td>
									</tr>
								</table>
							</td>
						</tr>
					</table>
				</td>
			</tr>
			<tr>
				<td style="border-right: 1px solid gray; vertical-align: bottom;">
					<table>
						<tr>
							<td>
								<table style="border: 1px solid gray">
									<caption>2b. UP (Transpose)</caption>
									<tr align="right">
										<td>11</td>
										<td>17</td>
										<td>23</td>
										<td>4</td>
										<td>10</td>
									</tr>
									<tr align="right">
										<td>12</td>
										<td>18</td>
										<td>24</td>
										<td>5</td>
										<td>6</td>
									</tr>
									<tr align="right">
										<td>13</td>
										<td>19</td>
										<td>25</td>
										<td>1</td>
										<td>7</td>
									</tr>
									<tr align="right">
										<td>14</td>
										<td>20</td>
										<td>21</td>
										<td>2</td>
										<td>8</td>
									</tr>
									<tr align="right">
										<td>15</td>
										<td>16</td>
										<td>22</td>
										<td>3</td>
										<td>9</td>
									</tr>
								</table>
							</td>
						</tr>
						<tr>
							<td>
								<table style="border: 1px solid gray">
									<caption>2b: UP (LEFT shift)</caption>
									<tr align="right">
										<td>17</td>
										<td>23</td>
										<td>4</td>
										<td>10</td>
										<td>11</td>
									</tr>
									<tr align="right">
										<td>24</td>
										<td>5</td>
										<td>6</td>
										<td>12</td>
										<td>18</td>
									</tr>
									<tr align="right">
										<td>1</td>
										<td>7</td>
										<td>13</td>
										<td>19</td>
										<td>25</td>
									</tr>
									<tr align="right">
										<td>8</td>
										<td>14</td>
										<td>20</td>
										<td>21</td>
										<td>2</td>
									</tr>
									<tr align="right">
										<td>15</td>
										<td>16</td>
										<td>22</td>
										<td>3</td>
										<td>9</td>
									</tr>
								</table>
							</td>
						</tr>
					</table>
				</td>
				<td class="code">
					<table>
						<tr>
							<td style="width: 60%;"></td>
							<td style="text-align: left; white-space: pre; width: 40%;">   ]d=. |: c</td>
						</tr>
						<tr>
							<td></td>
							<td>
								<table>
									<tr align="right">
										<td>11</td>
										<td>17</td>
										<td>23</td>
										<td>4</td>
										<td>10</td>
									</tr>
									<tr align="right">
										<td>12</td>
										<td>18</td>
										<td>24</td>
										<td>5</td>
										<td>6</td>
									</tr>
									<tr align="right">
										<td>13</td>
										<td>19</td>
										<td>25</td>
										<td>1</td>
										<td>7</td>
									</tr>
									<tr align="right">
										<td>14</td>
										<td>20</td>
										<td>21</td>
										<td>2</td>
										<td>8</td>
									</tr>
									<tr align="right">
										<td>15</td>
										<td>16</td>
										<td>22</td>
										<td>3</td>
										<td>9</td>
									</tr>
								</table>
							</td>
						</tr>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   k1=. monad : '1|. ,.i.# y'</td>
							<td></td>
						</tr>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   K=. k1 N</td>
							<td></td>
						</tr>
						<tr>
							<td style="text-align: left; white-space: pre; width: 60%;">   K;' ';d</td>
							<td style="text-align: left; white-space: pre; width: 40%;">   ]e=. K P d</td>
						</tr>
						<tr>
							<td>
								<table>
									<tr>
										<td style="border: 1px solid gray">
											<table>
												<tr align="right">
													<td>1</td>
												</tr>
												<tr align="right">
													<td>2</td>
												</tr>
												<tr align="right">
													<td>3</td>
												</tr>
												<tr align="right">
													<td>4</td>
												</tr>
												<tr align="right">
													<td>0</td>
												</tr>
											</table>
										</td>
										<td style="border: 1px solid gray"></td>
										<td style="border: 1px solid gray">
											<table>
												<tr align="right">
													<td>11</td>
													<td>17</td>
													<td>23</td>
													<td>4</td>
													<td>10</td>
												</tr>
												<tr align="right">
													<td>12</td>
													<td>18</td>
													<td>24</td>
													<td>5</td>
													<td>6</td>
												</tr>
												<tr align="right">
													<td>13</td>
													<td>19</td>
													<td>25</td>
													<td>1</td>
													<td>7</td>
												</tr>
												<tr align="right">
													<td>14</td>
													<td>20</td>
													<td>21</td>
													<td>2</td>
													<td>8</td>
												</tr>
												<tr align="right">
													<td>15</td>
													<td>16</td>
													<td>22</td>
												<td>3</td>
													<td>9</td>
												</tr>
											</table>
										</td>
									</tr>
								</table>
							</td>
							<td>
								<table>
									<tr align="right">
										<td>17</td>
										<td>23</td>
										<td>4</td>
										<td>10</td>
										<td>11</td>
									</tr>
									<tr align="right">
										<td>24</td>
										<td>5</td>
										<td>6</td>
										<td>12</td>
										<td>18</td>
									</tr>
									<tr align="right">
										<td>1</td>
										<td>7</td>
										<td>13</td>
										<td>19</td>
										<td>25</td>
									</tr>
									<tr align="right">
										<td>8</td>
										<td>14</td>
										<td>20</td>
										<td>21</td>
										<td>2</td>
									</tr>
									<tr align="right">
										<td>15</td>
										<td>16</td>
										<td>22</td>
										<td>3</td>
										<td>9</td>
									</tr>
								</table>
							</td>
						</tr>
					</table>
				</td>
			</tr>
		</table>
