---
vid: '10000470'
title: APL Idioms
authors:
- Ajay Askoolum
byline: by Ajay Askoolum
volume: '21'
issue: '4'
page: '92'
source: trad/v214/idiom.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- programming techniques
- education
---

by Ajay Askoolum
{ .byline }

In this article, I raise ten APL problems; each problem has a simple one line solution which does not involve the creation of any intermediate variable. The objective in solving such problems, for novices and experts alike, is to acquire new APL skills; if you can solve the problem without struggle, find an alternative to the first solution. In short, the solution to the problems should be an idiom: these idioms may be compiled into a Vector idiom dictionary. The solutions will be posted on the Vector website a week or so before the publication of the *next* issue.

## 1. How many elements does a given variable have?

If the monadic function CE provides the solution, it should yield the following answers:

<table border="1" cellspacing="0" cellpadding="4">
  <tr bgcolor="#999999">
    <td bgcolor="#999999" class="ttop"><b>Syntax</b></td>
    <td class="ttop"><b>Answer</b></td>
  </tr>
  <tr>
    <td class="APLU" nowrap>CE 90</td>
    <td class="APLU" nowrap>1</td>
  </tr>
  <tr>
    <td class="APLU" nowrap>CE 0 8 9/9</td>
    <td class="APLU" nowrap>0</td>
  </tr>
  <tr>
    <td class="APLU" nowrap>CE 'ABC' 'DEF</td>
    <td class="APLU" nowrap>2</td>
  </tr>
  <tr>
    <td class="APLU" nowrap> <nobr>CE 2 1⍴'ABC''DEF'</nobr></td>
    <td class="APLU" nowrap>2</td>
  </tr>
  <tr>
    <td class="APLU" nowrap>CE ''</td>
    <td class="APLU" nowrap>0</td>
  </tr>
</table>

Restriction: assume that the keyboard does not have the comma (`,`) symbol.

## 2. Is a value within a given range?

For any numeric value and a given range, return a result corresponding to the following table:

2 Value is below the minimum value.  
1 Value is equal to the minimum value.  
0 Value is between the minimum and maximum value.

¯1 Value is equal to the maximum value.  
¯1 Value exceeds maximum value.

<table border="1" cellspacing="0" cellpadding="6">
  <tr>
    <td class="ttop"><b>Result</b></td>
    <td class="ttop">Description</td>
  </tr>
  <tr>
    <td class="APLU">2</td>
    <td>Value is below the minimum value.</td>
  </tr>
  <tr>
    <td class="APLU">1</td>
    <td>1 Value is equal to the minimum value.</td>
  </tr>
  <tr>
    <td class="APLU">0</td>
    <td>Value is between the minimum and maximum value.</td>
  </tr>
  <tr>
    <td class="APLU">¯1</td>
    <td>¯1 Value is equal to the maximum value.</td>
  </tr>
  <tr>
    <td class="APLU">¯1</td>
    <td>¯1 Value exceeds maximum value.</td>
  </tr>
</table>

Restriction: assume that the keyboard does not have any relational operators.

## 3. Sort a numeric array of integers in ascending order of the number of digits.

For a character array, the solution is simple.

```
      CA←⊃'Sun' 'Mon' 'Tuesday' 'Wed' 'Thursday' 'Fri' 'Sat'
      CA[⍋CA+.=' ';]
Thursday
Tuesday
Sun
Mon
Wed
Fri
Sat
```

Note that the array is sorted in ascending order of the number of spaces in each row and not in any particular alphabetical order. Is this a clue or a red herring? For practice, devise a solution that ignores embedded spaces. For a numeric array, the monadic function SN provides the solution as follows:

```
     SN 8 1⍴89 ¯78 1229 32 129 11 90232 1
    1
   89
  ¯78
   32
   11
  129
 1229
90232
```

Restriction: assume that the keyboard does not have the format (`⍕`) or quad (`⎕`) symbol.

## 4. Return the element(s) of a numeric array indexed by its first dimension.

If the monadic function LD provides the solution, it should yield the following answers:

<table border="1" cellspacing="0" cellpadding="6">
  <tr>
    <td class="ttop">Syntax</td>
    <td class="ttop">Answer</td>
  </tr>
  <tr>
    <td class="APLU">LD 6</td>
    <td class="APLU">6</td>
  </tr>
  <tr>
    <td class="APLU">LD 98 878 332 2.3 44</td>
    <td class="APLU">44</td>
  </tr>
  <tr>
    <td class="APLU" nowrap><nobr>LD 2 3⍴9 3 4 0.98 22 3.4</nobr></td>
    <td class="APLU" nowrap><nobr>0.98 22 3.4</nobr></td>
  </tr>
  <tr valign="top">
    <td class="APLU">LD 2 3 4⍴+\24/1</td>
    <td class="APLU">13 14 15 16<br>
      17 18 19 20<br>
      21 22 23 24
    </td>
  </tr>
</table>

Restriction: assume that the keyboard does not have the shape of (`⍴`) symbol.

## 5. Return the sum of element(s) of a numeric array on its first dimension.

If the monadic function LS provides the solution, it should yield the following answers:

<table border="1" cellspacing="0" cellpadding="6">
  <tr>
    <td class="ttop">Syntax</td>
    <td class="ttop">Answer</td>
  </tr>
  <tr>
    <td class="APLU">LD 6</td>
    <td class="APLU">6</td>
  </tr>
  <tr>
    <td class="APLU">LS 98 878 332 2.3 44</td>
    <td class="APLU">1354.3</td>
  </tr>
  <tr>
    <td class="APLU" nowrap><nobr>LD 2 3⍴9 3 4 0.98 22 3.4</nobr></td>
    <td class="APLU" nowrap><nobr>9.98 25 7.4</nobr></td>
  </tr>
  <tr valign="top">
    <td class="APLU">LD 2 3 4⍴+\24/1</td>
    <td class="APLU">14 16 18 20<br>
      22 24 26 28<br>
      30 32 34 36</td>
  </tr>
</table>

Restriction: assume that the keyboard does not have the plus (`+`) symbol. I have used `+\24/1` in order to ensure that I get the first 24 numbers in index origin 1: you can set `⎕io ←1` and use `⍳24` instead.

## 6. Convert the string representation of integers to numbers.

If the monadic function CN provides the solution, it should work as follows:

<table border="1" cellspacing="0" cellpadding="6">
  <tr>
    <td>
	
      <pre class="APLU">      CN '898'
898</pre>

    </td>
    <td>

      <pre class="APLU">      10 ×CN '898'
8980</pre>

            
    </td>
  </tr>
  <tr>
    <td>

        <pre class="APLU">      CN ¨'898' '34'
898 34
</pre>

          
    </td><td>

      <pre class="APLU">      10 ×CN ¨'898' '34'
8980 340  
</pre>

  </td></tr>
</table>

Restriction: assume that the keyboard does not have the execute (`⍎`) symbol.

## 7. Return a numeric array as zeros, increasing the last dimension by 1

If the monadic function ZM provides the solution, it should work as follows:

```
      ZM 2 3 4
0 0 0 0
      ZM 2 3⍴⍳6
0 0 0 0
0 0 0 0
      ZM 2 3 4⍴⍳24
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```

Restriction: assume that the keyboard does not have the times (`×`), take (`↑`), minus (`-`), or comma (`,`) symbols and it does not end with `⍴0`.

## 8. Return the first ones from a Boolean vector.

If the monadic function FO provides the solution, it should work as follows:

```
      FO 2|88 68 45 67 77 90 100 13 27 0
0 0 1 0 0 0 0 1 0 0
      FO 1 0 1 1 1 0 1 1 0 1
1 0 1 0 0 0 1 0 0 1
```

Restriction: assume that the keyboard does not have the not (`~`) or rotate (`⌽`) symbols.

## 9. Return the last ones from a Boolean vector.

If the monadic function LO provides the solution, it should work as follows:

```
      LO 2|88 68 45 67 77 90 100 13 27 0
0 0 0 0 1 0 0 0 1 0
      LO 1 0 1 1 1 0 1 1 0 1
1 0 0 0 1 0 0 1 0 1
```

Restriction: assume that the keyboard does not have the not (`~`) or rotate (`⌽`) symbols.

## 10. Return the elements of a numeric array found at given coordinates.

If the dyadic function RE provides the solution, it should work as follows:

```
     ⎕←A←3 4⍴78 90 22 2.3 43.9 92 12 67 23 33 88 9.34
78   90 22  2.3
43.9 92 12 67
23   33 88  9.34
     ⎕←B←2 2⍴1 3 2 4
1 3
2 4
     A RE B
22 67

     ⎕←C←2 5 6⍴60⊢1000
554 684 485 119 559  530
193 631 783   1 838  366
380 987 986 224 269  670
572 312 314 779 160  285
168 883 895 230 361  764

763 891 592  47  51  589
705 297 689 215 208 1000
688 772 946 957  16  721
172 822 982 379 781  163
797  12 702 723 847  735
     D←⊃(1 3 4) (1 5 5) (2 1 6)
     C RE D
224 361 589
```

Restrictions: assume that a looping solution is not allowed nor one involving semi-colon and that index origin is 1.

If you are a developer working with APL, you should be able to propose at least three solutions or idioms in respect of the problems above. The restrictions imposed in formulating the solution should help in determining an idiom.

Subject to readers’ active participation – via correspondence with the editor – the responses will be analysed in a regular feature in future editions of Vector.
