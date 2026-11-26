---
vid: '10001830'
title: When is Easter?
authors:
- Ray Cannon
byline: by Ray Cannon, . (ray_cannon@compuserve.com)
volume: '10'
issue: '3'
page: '67'
source: trad/v103/cannon103_67.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- programming techniques
- mathematics and statistics
---

by Ray Cannon, <span class="affil">.</span> <span class="email">(<a href="mailto:ray_cannon@compuserve.com">ray\_cannon@compuserve.com</a>)</span>
{ .byline }

t

“There are many indications that the sole important application of arithmetic in Europe during the Middle Ages was the calculation of Easter date” (Knuth)

Easter, the major feast day of the Christian calendar, celebrates the anniversary of Christ’s resurrection. This is documented as having occurred on the Sunday following the Last (Passover) Supper. In the Hebrew calendar which is strictly lunar based, Passover is celebrated on the full moon of the 7th month, Nisan, which begins with the first appearance of the crescent spring (equinox) moon. So Passover should be on the first full moon after the spring equinox: the 14th day of Nisan.

Thus many early Christians (but not all) celebrated Easter on the 14th of Nisan, but by the 4th century, this had led to several problems.

### Problems

The Christian (Roman) world used the Julian calendar (developed at Julius Caesar’s request by Sosigenes in 46BC from an earlier Egyptian calendar). This is a solar calendar with no lunar base.

The Hebrew calendar did not observe the equinox with any precision. This made it difficult for far off communities to know when the month of Nisan had started. By the 4th century, the rift between the Christians and the Jews had grown to such an extent that the Christian Church could not allow Easter to fall on its Hebraic equivalent Passover.

The Church now insisted that Easter must be celebrated on a Sunday. Christ’s resurrection is documented in Luke 24 as having occurred on the Sunday morning. This was the reason that Christians adopted Sunday as the day of rest in preference to Saturday, the Jewish Sabbath. As a result, Easter was celebrated on different dates in different Christian communities. In particular, three methods were in use, the Alexandrian, the Roman, and the Quartodecimani (based on the 14th of Nisan).

### Decision Time

In 32AD the Council of Nicaea (Iznik in Asia Minor) had, as one of its goals, to set a single date for the celebration of Easter by Christians in both Eastern and Western Roman Empires. The “COMPUTISTS” who were specialists in charge of Christian calendric computations, came up with the following rule: “Easter Sunday is to be set on the first Sunday following the full moon that occurs on or after the Vernal Equinox.”

The actual definition refers to the “14th day after the new moon” rather than the full moon. This is useful, as it removes “rounding problems” of trying to pin-point the exact time and date of the full moon. At this time, the day was deemed to start at sunset (not midnight). A new moon appears first low in the western sky just after sunset, and is full 14 nights later. Under the Julian calendar, the Vernal Equinox was deemed to occur on the 25th of March (and was the start of the new year, according to some).

The computists drew up long-winded tables based on average full moon intervals. These tables called Epacts listed the ‘calendric’ age or phase of the moon on New Years Day. From these tables the date of Easter could be readily calculated. (For examples see the 1662 Book of Common Prayer.)

The first systematic algorithm for the Easter date appeared in the “canon paschalis” due to Victorius of Aquitania in 457 A.D. He made use of the 532-year cycle over which date, moon phase and day of the week re-occur in the Julian calendar.

### More Problems

In 730AD The Venerable Bede announced that the 365.25 day long Julian year was 11 minutes, 14 seconds too long, making a cumulative error of about 1 day every in 128 years. By the 16th century, the Vernal Equinox was 11 days out of step with the Julian calendar.

In 1582 Pope Gregory XIII appointed a commission to reform the calendar, and bring Easter back to its proper place in the seasons. This resulted in two steps being taken: first, 10 days were dropped from the calendar (Sunday, 4th October 1582 was followed by Monday, 15th October); and secondly the rules relating to leap years were reformed (a century would be a leap year only if divisible by 400). Thus the Gregorian calendar was created. Great Britain and its colonies did not adopt this calendar until 1752, and Russia not until 1917.

With the Gregorian calendar, the average length of the year (over long periods of time) is 365.2425 days, which is only 0.0003 longer than the tropical (astronomical) year, resulting in an error of 1 day in 3,300 years. (Minor reforms to the Gregorian calendar, relating to the years 4000, 8000 and 12000, and century leap years divisible by 900, made this century, have reduced this error to about 1 day in 44,000 years. Further accuracy appears to be futile, since the tropical year is itself changing.)

Under the Gregorian calendar, the vernal equinox occurs on (or around) the 21st of March. With this definition, Easter can occur any day between March 22 and April 25 inclusive.

### The Julian Period

In 1582, the Julian Period (not to be confused with the Julian Calendar) was devised by Joseph Scaliger, and named after his father Julius.

Scaliger had Julian Day number one (JD #1) begin at noon January 1st 4713 B.C. All other dates are referred to by their subsequent day number, so Noon of Dec 31 1989 was JD# 2447892. Use of the Julian Period makes the calculation of the number of days between two dates a very simple matter. Astronomers use the Julian Period to this day with a decimal part representing the time of day. (Many computer systems mis-call the day number within the year the Julian Date.)

The choice of 4713 B.C. for the start point was that Scaliger believed it to be the most recent time that 3 major cycles began on the same day. These cycles are the 28-year Solar cycle, the 19-year Metonic cycle, and the 15-year Induction cycle (used by the Romans to regulate taxes). Sometimes the modified Julian Date (MJD) is quoted. This is equal to JD-2400000.5, so MJD zero begins on November 17 1858.

```
    ∇ r←julian2date JD;I;F;A;B;C;D;E;G;d;m;y;X;S
[1]  ⍝Convert Julian day number to Gregorian date in CCYYMMDD format
[2]  ⍝Note on BC dates. 1BC is year 0 and 2BC is year ¯1
[3]  ⍝ so 31 Dec 1 BC and 31 Dec 2 BC are entered as 1231 and ¯11231
[4]   X← 0 1 ⊤,JD+0.5
[5]   A←int(X[1;]-1867216.25)÷36524.25
[6]   B←X[1;]+(X[1;]>2299160)×1+A-int A÷4
[7]   C←B+1524
[8]   D←int(C-122.1)÷365.25
[9]   E←int D×365.25
[10]  G←int(C-E)÷30.6001
[11]  d←C-E+int G×30.6001
[12]  m←100×G-1+12×G>13.5
[13]  y←10000×D-4715+m>250
[14]  S←(×y)+0=×y
[15]  r←(⍴JD)⍴S×(|y)+m+d+X[2;]
    ∇
    ∇ r←julian2dow JD
[1]  ⍝ Return DayOfWeek number for given Julian Period day number
[2]   r←⌊0.5+7×1⊤(⌊2+JD)÷7
    ∇
    ∇ r←date2julian date;ymd;y;m;d;a;b;c;D
[1]  ⍝Convert Gregorian date in CCYYMMDD format to Julian period day number
[2]  ⍝Note on BC dates. 1BC is in year 0 and 2BC is in year ¯1
[3]  ⍝so date of 31 Dec 1 BC and 31 Dec 2 BC are 1231 and ¯11231
[4]   ymd← 0 100 100 ⊤,|date
[5]   y←ymd[1;]××date
[6]   m←ymd[2;]
[7]   d←ymd[3;]
[8]   y←y-m<3
[9]   m←m+12×m<3
[10]  a←int y÷100
[11]  b←(date>15821015)×2+(-a)+int a÷4
[12]  c←int(365.25×y)-0.75×¯1=×y
[13]  D←int 30.6001×m+1
[14]  r←1720994.5+b+c+D+d
[15]  r←(⍴date)⍴r
    ∇
```

### The Solar Cycle

This has nothing to do with the movement in the heavens of the Sun, but to do with the days of the week, the first of which is Sunday (dies solis in Latin).

Under the Julian Calendar, the days of the week repeated themselves on the same date over a 28 year cycle. 28 being the product of 4 (the year leap year cycle) and 7 (the days in a week).

### The Metonic Cycle

19 solar years equal 235 lunar months. (To be precise 235.003.) This is the Metonic cycle. In this cycle, the moon will have the same phase on the same date after 19 years. i.e. 1st Jan 18AD was a new moon, so was 1st Jan 37, 19 years later. The Golden number of a year is its index value (origin 1) in the 19-year Metonic cycle. The Golden number for year 1990 is 15.

The epact is the “age of the moon at the start of the year”. For example, the epact for 1990 was 3, hence the full moon occurred on the night of 11th of January 1990.

### Functions

Most computer programs used to calculate the date of Easter are based on the algorithm due to Aloysius Lilius and Christopher Clavius in the 16th century. It is this algorithm that D. E. Knuth cites in several publications (D. E. Knuth The Art Of Computer Programming, vol 1, Addison-Wesley, 1968). Listings 1, 2 and 3 use this algorithm, (as does the EASTER function included in the Toronto Toolkit, part of the APL93 software exchange).

(Note. The Toolkit version 2.1 Easter function has a minor bug resulting in the date of 31st March being returned as 31st April. It also fails in 1583, returning the date of Easter in the Julian calendar, not the Gregorian.)

Here is my APL implementation:

```
    ∇ R←EASTER1 Y;G;C;X;Z;D;E;N;M
[1]  ⍝           LISTING-1
[2]  ⍝Compute the date(s) of Easter for the given Year(s)
[3]  ⍝Author Ray Cannon 10/10/1993
[4]  ⍝Based on the 16th century algorithm developed by the Neapolitan
[5]  ⍝astronomer Aloysius Lilius and the German Jesuit mathematician
[6]  ⍝Christopher Clavius.
[7]  ⍝Reference D.E.Knuth 'The art of Computer Programming' vol 1.
[8]
[9]  ⍝ Y←→Year(s) in the Gregorian Calendar (I.E. after 1582). Any rank
[10] ⍝ R←→Dates in CCYYMMDD format. (⍴R)←→(⍴Y)
[11]
[12] ⍝ Easter is to be held on the 1st Sunday following the first full
[13] ⍝ moon which occurs on or after 21st March, as calculated from
[14] ⍝ defined tables.
[15]
[16]  G←1+19|Y ⍝                         Golden Number
[17] ⍝ G is the so called 'Golden Number' for the year Y in the 19 year
[18] ⍝ Metonic Cycle. Every 19 years (according to the Juliaan Calendar)
[19] ⍝ the moon has the same phase on the same date.
[20]
[21]  C←1+⌊Y÷100 ⍝                       Century number
[22] ⍝ Note that the year 1900 is taken to be in the 20th Century
[23]
[24]  X←¯12+⌊3×C÷4 ⍝                     Gregorian Correction
[25] ⍝ X is the correction for the number of leap year days dropped by
[26] ⍝ Gregorian calendar to keep in phase with the sun
[27]
[28]  Z←¯5+⌊(5+8×C)÷25 ⍝                 Clavian Correction
[29] ⍝ Z is the correction for the Metonic cycle which is out by
[30] ⍝ 8 days over 2500 years
[31]
[32]  D←(⌊5×Y÷4)-X+10 ⍝                  Date of Sunday
[33] ⍝ March the ((-D)mod 7) in year Y is a Sunday
[34]
[35]  E←30|(11×G)+20+Z-X ⍝               Approximate Epact
[36] ⍝ The Epact is the 'age' of the moon at the start of the year Y
[37]
[38]  E←E+(E=24)∨(E=25)^G>11 ⍝           Church Epact
[39] ⍝ E is the value of the Epact found in the tables used by the Church.
[40] ⍝ It can be out by up to 3 days in some years.
[41]
[42]  N←44-E ⍝                           Find a full moon in year Y
[43] ⍝ Note that 44 days (44.295885 actually) represents 1.5 lunar months
[44]
[45]  N←N+30×N<21 ⍝                      Full moon after March 21
[46] ⍝ Add 1 month (30 days) if full moon is before the Vernal Equinox
[47]
[48]  N←N+7-7|D+N ⍝                      Next Sunday
[49] ⍝ Advance from full moon to following Sunday
[50]
[51]  M←3+N>31 ⍝                         Month number
[52] ⍝ March the 3rd month has 31 days
[53]
[54]  N←N-31×M=4 ⍝                       Day number
[55] ⍝ Adjust day if in April
[56]
[57]  R←N+100×M+100×Y ⍝                  Date of Easter
[58] ⍝ Date in CCYYMMDD format
    ∇
```

For a J implementation of this algorithm, see Gene McDonnell’s letter, reproduced on page ??.

For a little light relief, here is a debased form of Adrian Smith’s code that first appeared in Vector’s fore-runner “Crab Apple”. Adrian described his original as written in rhyming couplets. This one is more akin to cockney rhyming slang.

```
     R←EASTER3 Y
[1] ⍝          LISTING-3
[2] ⍝ Compute the date(s) of Easter for the given Year(s)
[3] ⍝ Author Ray Cannon 10/10/1993
[4] ⍝ Based on a 20th century APL program by Adrian Smith
[5] ⍝ Reference Adrian Smith  'Crab Apple'
[6]
[7] ⍝ Y←→Year(s) in the Gregorian Calendar (I.E. after 1582) Any rank
[8] ⍝ R←→Dates in CCYYMMDD format (⍴R)←→(⍴Y)
[9]
[10] ⍝ Note. Adrian Smiths origional was written in 'rhythming cuplets'
[11] ⍝       This version is in cockney rhyming slang.
[12]
[13] R←(17,⍴,Y)⍴0
[14] R[17;]←,Y
[15]
[16] R[ 2  3 ;]← 0 100 ⊤0    + 1  0   1     1 ×.+R[1  17 16 16 ;]
[17] R[ 4  1 ;]← 0  19 ⊤0    + 1  1   5     1 +.×R[1   3  2 16 ;]
[18] R[ 4  5 ;]← 0   4 ⊤0    + 1 25   3     1 ×.+R[6   2  6 16 ;]
[19] R[ 8  9 ;]← 0   4 ⊤R[3;]+ 1 60   5     1 ×.-R[6   6  5 16 ;]
[20] R[ 6 15 ;]← 0  25 ⊤0    + 1 11   8     1 ×.+R[6   2 16 16 ;]
[21] R[15  7 ;]← 0  30 ⊤0    + 1 19  ¯1     1 +.×R[4   1  6 16 ;]
[22] R[11 15 ;]← 0 319 ⊤0    + 1 11   1     1 +.×R[1   7 16 16 ;]
[23] R[15 10 ;]← 0   7 ⊤0    + 1 ¯1  ¯1     2 +.×R[11  9  7  8 ;]
[24] R[12 14 ;]← 0  30 ⊤110  + 1 ¯1   1     1 +.×R[7  11 10 16 ;]
[25] R[15 13 ;]← 0  33 ⊤5    + 1 ¯1   1     1 +.×R[14 12 16 16 ;]
[26] R[15 17 ;]← 0   0 ⊤0    + 1  1 100 10000 +.×R[13 16 12 17 ;]
[27]
[28] R←(⍴Y)⍴R[17;]
```

Listing 4 uses a different method devised in 1876, which first appeared in Butcher’s Ecclesiastical Calendar. Its internal working is however undocumented. Can anyone tell me how it works?

```
    ∇ R←EASTER4 Y;GN;Z;CC;T;FM;DN;MD

[1]  ⍝          LISTING-4
[2]  ⍝ Compute the date(s) of Easter for the given Year(s)
[3]  ⍝ Author Ray Cannon 10/10/1993
[4]  ⍝ Based on a 19th century algorithm defined in
[5]  ⍝ Butcher's Ecclesiastical Calendar (1876)
[6]  ⍝ Reference P.Duffett-Smith 'Practical Astronomy with your calculator'
[7]
[8]  ⍝ Y←→Year(s) in the Gregorian Calendar (I.E. after 1582) Any rank
[9]  ⍝ R←→Dates in CCYYMMDD format (⍴R)←→(⍴Y)
[10]
[11]  GN←,19|Y ⍝                             Golden number-1
[12]  Z← 0 100 ⊤,Y ⍝                         Split CCYY into CC and YY
[13]  CC←Z[1;] ⍝                             Save CC
[14]  Z← 0 4 ⊤Z ⍝                            Leap years and Leap Centuries
[15]  T←⌊(1+CC-⌊0.04×8+CC)÷3 ⍝               ?Clavian and Gregorian correction
[16]  FM←30|15+CC-T+Z[1;1;]-19×GN ⍝          ?Full moon
[17]  DN←7|32-FM+Z[2;2;]-2×Z[1;2;]+Z[2;1;] ⍝ ?Day number of week
[18]  T←7×⌊(GN+11×FM+2×DN)÷451 ⍝             ?
[19]  MD← 0 31 ⊤114+FM+DN-T ⍝                MM and DD of easter
[20]  R←(⍴Y)⍴1+MD[2;]+100×MD[1;]+100×,Y ⍝    Date of Easter in CCYYMMDD format
    ∇
```

I have tested these APL functions against each other, and they produce identical results for a 4000-year period starting from 1583. They have also been verified against the dates printed in the World Almanac 1990 for the date of Easter from 1901 to 2100.

Listing 5 is my implementation of the original rules. It uses utilities to convert between calendar dates, day numbers and day of the week numbers. It produces some WRONG results.

```
    ∇ r←LISTING5 year;dates;Equinox;ANewMoon;DaysInYear;DaysInMonth;
                 DaysBetween;MonthsBetween;EquinoxInYear;NewMoon;
                 FullMoon;SpringMoon;DayOfWeek
[1]  ⍝Return 'astronomically correct' date of Easter for the given Year(s)
[2]  ⍝HEALTH WARNING-This function does not return the TRUE date of EASTER
[3]
[4]  ⍝Author Ray Cannon 10/10/1993
[5]  ⍝Based on the definition:
[6]  ⍝    'Easter falls on the first Sunday after the first
[7]  ⍝     full moon on or after the Vernal Equinox'
[8]
[9]  ⍝ year←→Year(s) in Gregorian calendar
[10] ⍝ r←→Date(s) of Easter in CCYYMMDD format
[11]
[12]
[13] ⍝Utilities used:
[14] ⍝  julian2date←→Returns the date in CCYYMMDD.TIME format from julian day
[15] ⍝               number. Time of day specified as a decimal.
[16] ⍝  date2julian←→Returns the Julian day number for a date given
[17] ⍝               in CCYYMMDD.TIME format.
[18] ⍝  julian2dow ←→Returns day of week number for given julian day number
[19] ⍝               0←→Sunday, 6←→Saturday
[20] ⍝  j2d        ←→Rounds Julian day number to 'start' of day.
[21] ⍝               Julian period days start at midday
[22] ⍝               j2d assumes days start at 6pm, that is sunset.
[23] ⍝  int        ←→returns the integer part of its argument, with the
[24] ⍝               correct sign. eg ¯4←→int ¯4.5
[25]
[26] ⍝ Define some constants
[27]  dates←date2julian 19900320 19900225 +(19 54 +60× 21 8)÷1440
[28]  Equinox←dates[1] ⍝           Equinox on 20 March 1990 at 21:19 GMT
[29]  ANewMoon←dates[2] ⍝          New moon on 25 Feb 1990 at 8:54 GMT
[30]  DaysInYear←365.2422 ⍝        Number of days in the tropical year
[31]  DaysInMonth←29.53059 ⍝       Number of days in the lunar month
[32]
[33] ⍝ Calculate the time between 1990 and the given year(s)
[34]  DaysBetween←DaysInYear×year-1990 ⍝      Days between
[35]  MonthsBetween←DaysBetween÷DaysInMonth ⍝ Months between
[36]
[37] ⍝ Find (the start of) the day of the spring Equinox in the supplied year
[38]  EquinoxInYear←j2d Equinox+DaysBetween
[39]
[40] ⍝ Find a New Moon around the equinox just found
[41]  NewMoon←ANewMoon+DaysInMonth×⌈MonthsBetween
[42]
[43] ⍝ Find the Full Moon following the New Moon just found
[44]  FullMoon←NewMoon+14
[45]
[46] ⍝ Find next full moon if this one is before the equiniox
[47]  SpringMoon←⌊FullMoon+DaysInMonth×EquinoxInYear>j2d FullMoon-1.5
[48]
[49] ⍝ Find the day of the week the Spring Full Moon falls on
[50]  DayOfWeek←julian2dow j2d SpringMoon
[51]
[52] ⍝ Return the Sunday after the Spring Full Moon
[53]  r←⌊julian2date 0.5+j2d SpringMoon+7-DayOfWeek
    ∇
```

It works by using the known exact length of the tropical year and lunar month, along with the exact date and time of the spring new moon and equinox in 1990, to calculate (via Julian day numbers) the exact date and time of the equinox and spring new moon in the desired year. From these it calculates the appropriate full moon and following Sunday. This method is astronomically accurate, but is not quite 100% ecclesiastically correct. As Johannes Kepler is reputed to have said “Easter is a feast, not a planet”. Problems like converting (rounding) the instance of a full moon (which occurs at midnight local time) to a (GMT) date or day of the week, lead to some differences. Question: Should we be using GMT or Rome or Jerusalem local time?

Other differences can be attributed to inaccuracies in the Epact table (which can often be 2 to 3 days out) used to define under English law when Easter occurs.

One final question which one day may face the church: How do you fix the date of Easter on a different planet?

For further commentary see:

1. Puzzles and Paradoxes by T. H. O’Beirne. Oxford University Press 1965.
2. Counting the Eons by Isaac Asimov. Grafton Books 1984.
3. Practical astronomy with your calculator by Peter Duffett-Smith.
4. Cambridge University Press. 3rd edition 1988
5. Stonehenge Decoded by G.S.Hawkins. Doubleday and Co. Inc, 1965. (Within chapter 9 Eclipses, he describes how to fix the date of Easter Sunday using Stonehenge as the calculator.)

(webpage generated: 18 February 2006, 02:16)
