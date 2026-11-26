---
vid: '10007130'
title: 'Letter: More Solutions'
authors:
- Gianluigi Quario
byline: from Gianluigi Quario (giangiquario@hotmail.com)
volume: '22'
issue: '2'
page: '7'
source: trad/v222/idioms.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- programming techniques
---

from Gianluigi Quario (giangiquario@hotmail.com)
{ .byline }

1\. How many elements does a given variable have?

Restriction: assume that the keyboard does not have the comma (,) symbol.

```
CE←×/∘⍴ ⍝ ⍵←A
```

2\. Is a value within a given range?

Restriction: assume that the keyboard does not have any relational operators.

```
C2←{+/×⍵[⍋⍵]-⍺} ⍝ ⍺←D0 ⍵←D1 ⍴⍵←→2
```

3\. Sort a numeric array of integers in ascending order of the number of digits.

Restriction: assume that the keyboard does not have the format (<span class="APLU">⍕</span>) or quad (<span class="APLU">⎕</span>) symbol.

```
SN←{⍵[⍋⌈10⍟|(⍵+2×0=⍵)+×⍵;]} ⍝ ⍵←I2 (1↓⍴⍵)←→1
```

4\. Return the element(s) of a numeric array indexed by its first dimension.

Restriction: assume that the keyboard does not have the shape of (<span class="APLU">⍴</span>) symbol.

NB.(GQR) the definition of this problem is not complete:

a) should return an array with a lesser rank?

b) a scalar has not a “first dimension”, but it is required by the challenge:

```
  LD 6←→6

  LD←{,[1,(0<≡+/~0×⍵)/2]1↑[1],[(0<≡⍵)/1]⊂[⍬]⊖⍵}
       ⍝ ⍵←D (also scalar)
```

or

```
  LD_←{,[1,(0<≡+/~0×⍵)/2]1↑[1]⊖⍉1↓[1],[1 2]⍉,[⍬]0,⍵}
       ⍝ ⍵←D (also scalar)
```

or

```
  LD__←{1↑[1]⊖⍵} ⍝ ⍵←D ; ⍵ not scalar;
         NB.result has same rank of ⍵
```

5\. Return the sum of element(s) of a numeric array on its first dimension.

Restriction: assume that the keyboard does not have the plus (+) symbol.

NB.(GQR) a scalar has not a “first dimension”, but it is required by the challenge:

```
  LS 6←→6

  LS←{⎕ML←2 ⋄ ↑¨⍴¨∊¨⊂[(~0∊⍴⍴⍵)/1]⍵⍴¨1}
          ⍝APL2 ⍝⍵←D (also scalar)
```

6\. Convert the string representation of integers to numbers.

Restriction: assume that the keyboard does not have the execute (<span class="APLU">⍎</span>) symbol.

```
  CN←{10⊥2⊥4↓[⎕IO]⍉11 ⎕DR,[⍬]⍵} ⍝ ⍵←C1 (also scalar)
```

```
  CN_←{10⊥2⊥¨4↓¨11 ⎕DR¨⍵} ⍝ ⍵←C1 (also scalar)
```

```
  CN__←{10⊥(-⎕AV⍳'0')+⎕AV⍳⍵} ⍝ ⍵←C1 (also scalar)
```

```
  CN___←{10⊥(-⎕IO)+'0123456789'⍳⍵} ⍝ ⍵←C1 (also scalar)
```

7\. Return a numeric array as zeros, increasing the last dimension by 1.

Restriction: assume that the keyboard does not have the times (<span class="APLU">×</span>), take (<span class="APLU">?</span>), minus (<span class="APLU">-</span>), or comma (<span class="APLU">,</span>) symbols and it does not end with <span class="APLU">⍴0</span>.

```
ZM←{((((¯1+⍴⍴⍵)⍴0),1)+⍴⍵)⍴⍵=⍵+1} ⍝ ⍵←D
```

8\. Return the first ones from a Boolean vector.

Restriction: assume that the keyboard does not have the not (~) or rotate (²) symbols.

```
FO←{⍵^1,(1↓⍵)≠¯1↓⍵} ⍝ ⍵←B1
```

9\. Return the last ones from a Boolean vector.

Restriction: assume that the keyboard does not have the not (~) or rotate (²) symbols.

```
LO←{⍵^((1↓⍵)≠¯1↓⍵),1} ⍝ ⍵←B1
```

10\. Return the elements of a numeric array found at given coordinates.

Restrictions: assume that a looping solution is not allowed nor one involving semi-colon and that index origin is 1.

```
RE← {(,⍺)[1+(⍴⍺)⊥⍉⍵-1]} ⍝ ⍺←D2 ; ⍵←D 1
```

***
