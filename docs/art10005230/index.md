---
vid: '10005230'
title: 'Letter: Ajay Askoolum’s APL Idioms'
authors:
- Steve Mansour
byline: from Steve Mansour (steve@carlislegroup.com)
volume: '22'
issue: '2'
page: '6'
source: trad/v222/idioms.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- programming techniques
---

from Steve Mansour (steve@carlislegroup.com)
{ .byline }

<a id="steve"></a> All solutions are written using dynamic functions in Dyalog APL, <span class="APLU">⎕ml←3</span>.  All are origin independent.

### Some notes:

I have two solutions to idiom #1,  CE and CE1. CE1 doesn't use the rho symbol or the comma symbol.

```
  CE←{×/⍴⍵}
  CE1←{+/∊⍵≡¨⍵}
```

I have two solutions to idiom #2, RA and RA1. Both require a 2-item vector on the left as the range. RA requires a scalar right argument. RA1 permits any array as the right argument and doesn’t use each. For more details on this solution, please see my article “*Arithmetic Boundary Checking*”in Vector Vol 21 No. 2.  Solution to this particular problem is at the top of page 122.

```
  RA←{+/×⍵-⍺}
  RA1←{+/×⍵∘.-⍺}
```

Idiom #3:

```
  SN←{⍵[⍋⌊10⍟|⍵;]}
```

Idiom #4:

```
  LD←{{⍵}⌿⍵}
```

Idiom #5:  Curried left argument can be any suitable value > 0.  Domain error may result if the base of the exponential function is not chosen correctly.

```
  LS←0.9∘{⍺⍟×⌿⍺*⍵}
```

Idiom #6:  Note – <span class="APLU">⎕D</span>  is equivalent to <span class="APLU">'0123456789'</span> :

```
  CN←{10⊥-⎕IO-⎕D⍳⍵}
```

Idiom #7:

```
  ZM←{0+/⍵}
```

Idiom #8:

```
  FO←{2</0,⍵}
```

Idiom #9:

```
  LO←{2>/⍵,0}
```

Idiom #10:

```
  RE←{↑¨↓∘⍺¨↓⍵-1}
```

***
