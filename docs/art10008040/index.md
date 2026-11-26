---
vid: '10008040'
title: A Note on the Easter Algorithm in J
authors:
- Keith Smillie
byline: by Keith Smillie
volume: '10'
issue: '3'
page: '78'
source: trad/v103/smillie103_78.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- programming techniques
- mathematics and statistics
---

by Keith Smillie <span class="affil"></span> <span class="email"></span>
{ .byline }

The date of Easter Sunday for any year after 1582 can be determined by an algorithm due to the astronomer Aloysius Lilius and the Jesuit mathematician Christopher Clavius in the late 16th century. In the following version of the algorithm, which is taken from Knuth (1968), each step is given in conventional notation and is followed by the corresponding J verb or verbs:

Golden number of the year Y in the 19-year Metonic cycle:

```
 G ← (Y mod 19) + 1
 G=. >: @ (19&|)
```

Century

```
C ← Floor(Y/100) + 1
C=. >: @ <. @ (%&100)
```

Corrections. X is the number of centennial years in which leap year was dropped, and Z is a special correction to synchronize Easter with the moon’s orbit:

```
X ← Floor(3C/4) - 12       Z ← Floor((8C + 5)/25) - 5
X=. _12&+ @ <. @ (4: (%~) 3: * C)
Z=. _5&+ @ (<. @ (25&(%~) @ (5: + 8: * C)))
```

Find Sunday:

```
D ← Floor(5Y/4) - X - 10
D=. _10&+ @ (X (-~) (<. @ (4: (%~) 5&*)))
```

Epact which specifies when a full moon occurs:

```
E ← (11G + 20 + Z - X) mod 30
If E = 25 and G > 11, or if E = 24, then increase E by 1.

E1=. (30&|) @ ((11&* @ G) + (20&+ @ (Z - X)))
E=. E1 + 24&= @ (E1 @ ]) +. 25&= @ (E1 @ ]) *. 11&<@ (G@])
```

Find full moon:

```
N ← 44-E      If N < 21, then increase N by 30.
N=. (30&+) ^: (<&21) @ (44&- @ E)
```

Advance to Sunday:

```
N ← N + 7 - ((D + N) mod 7)
Sunday=. 7: + N - (7&| @ (D + N))
```

Get month:

```
If N > 31, then the date is April (N - 31); otherwise, it is March N.
Easter=. ((>&31) , >: @ (31&|) @ <:) @ Sunday
```

The value of Easter y, where y is any year after 1582, is a two-item list giving the month (0 for March and 1 for April) and the day of the month of Easter for that year. For example, Easter 1993 has the value 1 11 since Easter occurs on April 11, and Easter 1997 is 0 30 since Easter occurs on March 30.

The use of the power conjunction ^: for applying a verb conditionally in the verb N for finding the first full moon is due to McDonnell (1993). If we use his verb if=. ^: the present verb may be expressed as

```
N=. (30&+) if (<&21) @ (44&- @ E) .
```

The verb:

```
EasterTable=.(20 5)&$@(<"1 @(] ,."2 Easter"0)@((i.100)&+@]))
```

gives a table with 20 rows and 5 columns of the dates of Easter for a one-hundred-year period starting with the specified year. For example, EasterTable 1901 gives the dates of Easter for each year of the 20th century; the last two rows of this table given by \_2 5 \{. EasterTable 1901 are

```
 +---------+---------+---------+--------+---------+
 |1991 0 31|1992 1 19|1993 1 11|1994 1 3|1995 1 16|
 +---------+---------+---------+--------+---------+
 |1996 1 7 |1997 0 30|1998 1 12|1999 1 4|2000 1 23|
 +---------+---------+---------+--------+---------+
```

(A convenient reference for verifying the dates of Easter is The Oxford Companion to English Literature which gives in an Appendix the dates of Easter for each year from 1066 to 2000.)

### References

1. Knuth, D. E., 1968. The Art of Computer Programming, Volume 1. Addison-Wesley Publishing Company, Reading, Mass.
2. McDonnell, E. E., 1993. At Work and Play in the Fields of J. Iverson Software Inc., Toronto.

(webpage generated: 18 February 2006, 02:16)
