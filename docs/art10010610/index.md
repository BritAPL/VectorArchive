---
vid: '10010610'
title: 'J-ottings 46: Musical J-ers'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '22'
issue: '3'
page: '35'
source: trad/v223/jot222.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- mathematics and statistics
- programming techniques
---

by Norman Thomson
{ .byline }

In the course of the last year or two Ken and I had occasional internet chats on the subject of temperament in music, based on a book called “Temperament – the idea that solved Music’s Greatest Riddle” by Stuart Isacoff. (The word “temperament” was first used in this context round about 1500, and means, according to Chambers, “a system of compromise in tuning”). This is an excellent example of a technical subject, whose understanding and exposition is greatly facilitated by J. Putting this to the test, I scribbled down my thoughts from time to time with the idea of their eventually maturing into a J-ottings article, of which I think Ken would have taken note. Whether he would have liked it or not, alas I shall never know. No doubt he would have done it much better, but I hope that my attempts here may count as a small tribute to Ken.

## In the Beginning ...

Pythagoras, he of the hypotenuse, also had strong ideas about music. He realised that a vibrating string when stopped at its middle point produces a note melodically identical to the original, only, in modern terminology, an octave higher. Call the melodic value of both these notes the tonic, so that advancing an octave is a way of “listening” to the fraction 1/2. If the string is now stopped at its 2/3 point the result is another note called the dominant which, when sounded at the same time as the tonic, produces a pleasant sound combination. From this starting point two separate experiments proceed.

## Simple Fractions Sound Nice

The first experiment progresses by stopping a string at other fractional points with small integer numerators and denominators. Since the octave represents a full melodic circle which is repeated at 1/4 then 1/8 and so on, there is little point in considering stops other than those which lie between 1/2 and 1. The next “interesting” stop is thus at 3/4, followed by others at 3/5, 4/5 and 5/6. At this point all fractions with components of 6 or less have been exhausted, and in all cases pleasing sound combinations with the tonic are obtained. This experiment has thus provided a means of “hearing” the following range of fractions : 1/2 2/3 3/4 4/5 5/6 which incidentally are defined by the hook

```
   (%>:)1 2 3 4 5  NB. octave, fifth, fourth, third, minor third
 0.5 0.667 0.75 0.8 0.833
```

The interval names in the comment are descriptions of *distances* from the tonic. The difference between intervals and notes is analogous that between the gaps in the fence as opposed to the fence posts themselves. The piano tuner twists strings, and the organ tuner tweaks pipes to produce *notes*, but the listener primarily hears *intervals*, that is relatives rather than absolutes. Also, unlike violinists, say, for whom stopping, and thus tuning, is dynamic in performance, keyboard players must in the nature of things complete each performance on an instrument tuned in advance.

Advancing to higher integers in the above sequence, any fraction involving a 7, that is 4/7, 5/7, 6/7 and 7/8 produces unpleasant sound combinations. As for 8s, there is just one ratio which has not been investigated, namely 5/8 which sounds nice, and corresponds to the interval called a minor sixth. All of the intervals 1/2 2/3 3/4 3/5 4/5 5/6 5/8 are “pure” in the sense that they can be related to pleasant sounds which arise from the *physical* properties of a bowed string or a resonating pipe.

## Calculating a Circle of Fifths

The second experiment concerns how *several* strings should be tuned in order to produce pleasing combinations of sounds, and so it is about intervals rather than notes. Since local stopping on a single string produces a range of notes it makes sense to think in terms of a larger interval as a basic unit for between-string tuning when introducing new strings. The dominant interval is a natural choice for a second string, as Pythagoras was aware, then the dominant of the dominant for a third string and so on. The stopping position for the dominant of the dominant is clearly at 2/3 of 2/3 = 4/9, which is outside the range 1/2 to 1. However, the first experiment showed that doubling the fraction lowers the note by an octave but makes no difference to its melodic quality of the note, in which terms it is equivalent to a stopping position of 8/9. Next repeat the experiment to find the dominant of the dominant of the dominant at 2/3 of 8/9 = 16/27 which does not need doubling since it is already in the range 1/2 to 1. To continue this process, use J to develop a compound verb “multiply-by-2/3-and-double-if-outside-1/2-to-1”. To do this, observe first that compressing an infinite set of numbers into a finite range is already a familiar matter to anyone acquainted with scientific notation. Using logarithms, the fine detail of a real number is compressed into the range 1 (inclusive) to 10 (non-inclusive), while the exponent defines the wider territory within which the number lies. Thus, if a number is expressed as v e x then

```
    x=.<.@(10&^.)     NB. exponent
    x 2999
 3
```

```
    v=.%10&^@x   NB. value
    v 2999
 2.999
```

In pictorial terms v compresses numbers into the space

<pre>                ___________________________________________________
                 10<sup>0</sup>                                            10<sup>1</sup>                     </pre>

Now return to the musical experiment with its “special” multiplication in which e.g. (2/3) 2 = 8/9, so that the result always remains in the range 1/2 to 1. The “compression region” can be drawn as

<pre>                 ___________________________________________________
                 2<sup>-1</sup>                                             2<sup>0</sup>                       </pre>

Call the process “musical arithmetic”, which helps write the analogous verbs

```
    x2=.>.@(2&^.)     NB. 2-exponent
    x2 2r3
 0
```

```
    v2=.%2&^@x2       NB. value of fraction converted to range (0.5,1]
    v2 1r8 2r3 4r9
 1 0.667 0.889
```

In musical arithmetic the “nice” sounds as defined above, (or as Pythagoras would have more grandiosely called them “celestial harmonies”) are inverses according to the following plan:

2/3 : fifth 4/5 : major third 3/5 : major sixth  
1/(2/3) = 3/4 fourth 1/(4/5) = 5/8 minor sixth 1/(3/5) = 5/6 minor third

Now apply the power adverb to the “musical multiplication” verb to continue the progression of fifths, and at the same time recording the notes which are reached form a starting point of C :

```
    v2 (2r3)^>:i.12NB. progression of fifths
 0.667 0.889 0.593 0.79 0.527 0.702 0.936 0.624 0.832 0.555 0.74 0.987
 G     D     A     E    B     F#    C#    G#    D#    A#    F    C
```

After twelve iterations, a value 0.987 is obtained which is not too far from 1 representing the tonic.

Given that the numerators are powers of 2 and the denominators are powers of 3, there can never be any question of solving (2/3)&lt;sup>k&lt;/sup> = 1/2 exactly, and so it is reasonably satisfying to get as close as 0.987/2 in twelve steps. The musical interpretation is that progressing by intervals of a fifth twelve times take us through a cycle of notes which can then be made to repeat itself after a small adjustment to make the octave pure. This much was probably known to the Greeks of Pythagoras’ time, and the tuning system based on it is known as “Pythagorean tuning”. A natural place to make the adjustment necessary for a pure octave is at the final step, but it could be made at any intermediate step or indeed spread across several steps.

The next question concerns what would happen if the above exercise was repeated for 3/4 rather than 2/3. The primary interval in this case is the fourth, and the result is

```
    v2 (3r4)^>:i.12NB. progression of fourths
 0.75 0.563 0.844 0.633 0.949 0.712 0.534 0.801 0.601 0.901 0.676 0.507
 F    Bb    Eb    Ab    Db    F#    B     E     A     D     G     C
```

that is, the same notes only in reverse order, some with different names, and finishing round about the high octave, value 1/2, rather than the low octave at 1.

The great riddle referred to in the opening sentence comes about because, for example, if the stopped positions on a string are obtained by Pythagorean tuning, by the time F comes round its value will not be quite the “pure” value of 3/4 as dictated by the physics of a single string. Likewise the second list shows that under tuning by successive fourths, G would be a shade impure. Similar considerations apply to the other notes, which in turn means that the intervals will differ from the intervals identified in the first experiment. More importantly, the adjustment noted above which is needed to make the octave pure at the twelfth and final step is called a “comma”, or more specifically a “Pythagorean comma”.

## The Problem of D

So far six of the eight notes of the diatonic scale (that is the white notes on the piano in the scale of C) have been given places in the “simple vulgar fractions” scheme of things, the two remaining being D and B. Since these are symmetrically placed at either end of the octave, a discussion of one is automatically a discussion of the other, so focus on D. D is not consonant with C, so there is no physical “right” fraction for it, rather there are two candidates. The first comes from considering the fact that D is one whole tone away from C, and there is already a whole tone represented, namely F – G, whose ratio is (2/3) / (3/4) = 8/9. The second candidate arises from the fact that in order to make D – A a pure fifth D must be set by solving (3/5)/x = 2/3, that is, at (3/5) / (2/3) = 9/10. (A harpsichord with two such D keys was in fact built in Holland in 1639, but did not prove particularly popular for what would seem to be obvious reasons!) The diagram below shows D set to 8/9, in which case solve (1/2)/x = 8/9 to obtain 9/16 as the symmetrically consistent choice for B.

```
 1/2    9/16     3/5     5/8     2/3     3/4     4/5     5/6     8/9     1
 C       B        A       G#      G       F       E       Eb      D      C
octave  7th      6th    dim 6th   5th    4th     3rd    dim 3rd  2nd
```

## Symmetry and the Note in the Middle

A full octave in the chromatic scale (that is including two tonics) has 13 notes, and thus 12 intervals, and thus has a middle note, namely F#, analogous to the “six o’clock” position on a clock face. Where does it appear in the above table? The answer is that it doesn’t, because “half-way” on a multiplicative scale means the 1√2 position, so playing the interval C – F# on the piano is a way of “hearing” the square root of 2! Musicians call this the tritone, and it has also been called “the devil in music” on account of the difficulty for singers of pitching this interval. Further, the two progression series above show that under musical multiplication both (2/3) 6 and (3/4) 6 are approximations to 1√2, one being about 0.005 above and the other the same amount below. On either side of “six o’clock”, the interval of a fifth, C – G, consists of seven semi-tones, whereas a fourth, C – F, consists of five semitones, in which respect fifths and fourths are mirror images of each other. This also explains why the two progression series above are, in note terms, each the reverse of the other.

## Adjusting the Scale

The following is another copy of the 1/2 to 1 region in which fractions are labelled with interval names rather than notes (dim stands for “diminished”).

```
 1/2     9/16     3/5     5/8     2/3     3/4     4/5     5/6     8/9     1
  C       B        A       G#      G       F       E       Eb      D      C
octave   7th      6th    dim 6th   5th    4th     3rd   dim 3rd   2nd
```

Under this scheme the whole tones D – E and G – A have values (4/5) / (8/9) and (3/5) / (2/3), both of which are equal to 9/10, which was the alternative candidate for D. Also A – B has the value 15/16. This means that there are different types of whole tone in this scale, with the result that, for example, the first three notes of “Three Blind Mice” become a melodic progression having unequal steps, e.g. 9/10, then 8/9. Also the ratios for the main consonant intervals, obtained in each case by dividing the value of the second note by that of the first in musical arithmetic, are

<table border="1" cellspacing="0" cellpadding="6">
    <tr>
      <td>
        
          perfect
fifths
        
      </td>
      <td>
        
          major sixths
        
      </td>
      <td>
        
          major third
        
      </td>
    </tr>
    <tr>
      <td>
        
          <nobr>F - C  G - D  D - A  A - E</nobr>
        
      </td>
      <td>
        
          <nobr>F - D  G - E</nobr>
        
      </td>
      <td>
        
          F-A
        
      </td>
    </tr>
    <tr>
      <td>
        
          2/3  2/3  27/40  2/3
        
      </td>
      <td>
        
          16/27  3/5
        
      </td>
      <td>
        
          4/5
        
      </td>
    </tr>
  </table>

 

The values of 2/3, 4/5 and 3/5 are consistent with those for the tonic C, but D would need to be 9/10 to make D - A a pure fifth in which case G –D would be 27/40. Similarly in the key of G# the major third is G# – C, ratio 1 / (5/8) = 4/5 and the fifth is G# – Eb = (5/6) / (5/8) = 2/3 both of which are pure. However, in the key of E the major third E – G# has the ratio (5/8) / (4/5) = 25/32 or 0.781 which is just a touch impure. And so one could go on. Once a set of strings, say, is tuned for pure concordances in key C, compromises must be made, not only for melodies and harmonies in the key of C itself, but even more importantly for melodies played in other keys. How best to make such compromises has engaged the minds of musicians since medieval times, which is the subject of the book referenced at the head of this article. The history of the debates on temperament is complex, but broadly speaking, the D problem gave rise to two solution streams, one called **just intonation** which tolerated differences in whole tone values as a price worth paying for purity of most major thirds, the other called **mean tone temperament** which is based on making whole tones uniform. “Just” in this context should be thought of as being a derivative of “ad<u>just</u>ment”. The notion of making adjustments to organ pipes or strings on keyboards may well date as early as the late 14th century.

## Equal Temperament

In the mid 16th century the concept of the equal-tempered scale emerged which has dominated Western music to this day. In equal temperament each of the twelve semi-tone intervals are equal on a multiplicative scale. Such tunings first found favour among lute players for whom other forms of tuning necessitated the undesirable feature of having frets at unequal distances for different strings. Again, J can clarify and quantify what musicians and musical historians mean when they discuss this topic. In an equal-tempered system (not to be confused with the well-tempered system, which is yet another ingenious tuning scheme developed in the 17th century with similar objectives), the common ratio of the series of semi-tone values is, as a consequence of the definition, the reciprocal of the twelfth root of 2:

```
   ]e12=.%12%:2        NB. e12 is 1/12th  root of 2, could also be 2^-%12
0.944
```

and so the stopping ratios required to go up the scale are given by

```
   e12^i.13            NB. well-tempered stop positions
1 0.944 0.891 0.841 0.794 0.749 0.707 0.667 0.63 0.595 0.561 0.53 0.5
```

The following series is the corresponding ordered version of the ratios under Pythagorean tuning:

```
   \:~ v2 (2r3)^i.13   NB. Pythagorean stopping positions
 1 0.987 0.936 0.889 0.832 0.79 0.74 0.702 0.667 0.624 0.593 0.555 0.527
```

in which 0.987 is the approximation to the pure octave value of 0.5 in the equal-temperament system.

For ready comparison of tuning systems it is a near necessity to convert from a multiplicative scale ranging from 1 to 1/2 to an additive one from, say, 0 to 1200 in which each semitone interval is represented by 100 in an equal-temperament system. This converts

<table border="1" cellspacing="0" cellpadding="6">
    <tr>
      <td>the multiplicative scale</td>
      <td class="APLU">{1/2,
2<sup>-(1/2)</sup>, 2<sup>-(1/12)</sup>,  1}</td>
    </tr>
    <tr>
      <td>to the additive scale</td>
      <td class="APLU">{1200,   600 ,   100, 0}</td>
    </tr>
  </table>

 

Musicians call the unit which divides an octave into 1200 parts a “cent”, and J readily provides the means of conversion

```
   cent=.1200&*@(2&^.)@%   NB. convert stop positions to cents
   cent %12 4 3 2 1.5%:2   NB. well tempered C# Eb F F# G#
100 300 400 600 800
   cent e12^.13
0 100 200 300 400 500 600 700 800 900 1000 1100 1200
```

Pragmatically, only people with exceptionally sensitive hearing would be able to detect a difference of 12 cents or less (that is about an eighth of a semi-tone), but most people would find differences of 20 cents or there­abouts harmonically unpleasing, although melodically acceptable, that is fine when notes are heard in sequence but not when they are played together.

The inverse adverb in J makes the above process readily reversible, that is, given a cent value, the stop ratio is immediately available:

```
   (cent^:_1)cent e12^i.13
 1 0.944 0.891 0.841 0.794 0.749 0.707 0.667 0.63 0.595 0.561 0.53 0.5
```

The cent values of pure fifths, major thirds and major sixths are important numbers to keep in mind, and are calculated respectively as:

```
   cent v2 2r3 4r5 3r5     NB. cents for 5 th . maj 3 rd , and maj 6 th
  702 386.3 884.4
```

Thus the equal-tempered scale fails to achieve purity for fifths, major thirds and major sixths by margins of approximately 2, 14 and 16 cents respectively. By symmetry the values of the three complementary intervals in the diatonic scale (fourths, minor sixths and minor thirds) are simply 1200 minus the above values, as confirmed by

```
   cent v2 3r4 5r8 5r6     NB. cents for 4ths, min 3rds. 6ths.
498 813.7 315.6
```

Next establish the cent values of all twelve points on the Pythagorean chromatic scale, that is tuning by successive fifths, and then arranging the notes in ascending scale order (n.b. I have taken a few minor liberties in editing the J output in the interests of greater clarity):

<pre><span class="APLU">   /:~cent v2 (2r3)^i.13   NB. conversion of Pyth tuning to cents
0 23.5 114 204 318 408 522 612 702 816 906 1020 1110
C</span><sub><span class="APLU">1</span></sub><span class="APLU">  C</span><sub><span class="APLU">2</span></sub><span class="APLU">   C#  D  Eb   E   F   F#  G   G#  A   Bb    B   </span>   </pre>

All of this can be embodied in a single verb <span class="APLU">ptune</span> which takes as left input a fraction in the range (0.5 - 1) which is used to define a fifth, and as right input the powers to which this is to be raised:

```
   ptune=.cent@v2@^  NB. x. defines a 5 th ; y. is list of powers
   ]pscale=.}./:~2r3 ptune i.13
 23.5 114 204 318 408 522 612 702 816 906 1020 1110
 oct              3 rd   4 th       5 th       6 th
```

In order to avoid too much clutter, it is best to focus on just the major thirds, fourths, sixths and octave:

```
   2r3 ptune&>< 1 4 3 12 NB. Pythag'n tuning for various iterations
702 407.8 905.9 23.46    NB. cents for 5ths(input),3rds,6ths,comma
```

Increasing the input value of 2r3 means reducing the cent value of around 702 cents, or in musical terms, flattening the fifth. In particular flattening it to around 697 cents (an imperceptible difference to most listeners) gives nearly pure thirds and sixths at the cost of a severely impure octave :

```
   (cent^:(_1) 697) ptune&>< 1 4 3 12
 697 388 891 1164
```

The value of 23.5 in <span class="APLU">pscale</span> (in musical terms about a quarter of a semitone) represents the Pythagorean comma by which tuning by repeated pure fifths “misses” the octave whose purity in *any* tuning system is sacrosanct. “Comma” in music means essentially *discrepancy*, and when used unqualified it means the Pythagorean comma. Since F is the second last note to be tuned in a Pythagorean system, this is roughly the interval by which the fourth C – F is impure within a equal-tempered system. When a piano tuner tunes a set of twelve strings, his choice of techniques is analogous to finding ways of fitting a set of bricks lengthwise into a closed gap whose length is not quite equal to the sum of the lengths of the bricks. The amount of excess or shortfall is the comma associated with the technique.

In a Pythagorean scale the excess at the octave is 702-23.5 = 678.5 cents. Two notes at this interval produce a discordant sound known since mediaeval times as the “wolf fifth”, presumably because of its supposed likeness to a wolf braying. Of course it is possible, in principle at least, to spread the corresponding comma adjustment throughout the scale to provide a “smoothed” scale.

An octave can also be considered as the sum of three major thirds and of four minor thirds, views giving rise to discrepancies of 1200 – (3×386.3) = 41.1 below and (4×315.6) - 1200 = 62.4 above respectively, which are also considered as commas. Yet another type of comma is motivated by the fact that the major third is in some sense a more “beautiful” consonance than the “fifth” (think of songs in which soprano and alto voices proceed in a blend in parallel thirds). In Pythagorean tuning the note E required for the interval of a third is encountered after four steps (C – G – D – A – E), so in the same way that <span class="APLU">%12%:2</span> “equalises” the octave into twelve semitones, so <span class="APLU">%4%:5</span> (the reciprocal of the fourth root of five) equalises the third into four equal fifths. The value of this quantity is 0.66874, or in cent terms

```
   cent e4=.%4%:5
 696.578
```

Flattening the fifth to this value produces increasing discrepancies from Pythagorean tuning as follows :

<pre><span class="APLU">   (2r3 ptune i.5)- e4 ptune i.5
 0 5.38 10.8 16.1 21.5  </span>  </pre>

By the time E is reached at the fourth step there is a shortfall of 21.5 cents, which musicians call the **syntonic comma**, or sometimes the “comma of Didymus” (*didymos* is the Greek word for a twin, presumably because notes a third apart are in some sense like twins). Just as the Pythagorean comma is the adjustment needed to equalise the octave, so the syntonic comma is the adjustment needed to equalise the third. The effect of continuing this tuning procedure until the octave is reached is a shortfall of 41 cents:

```
   e4 ptune 12
 1159
```

Another way of looking at <span class="APLU">pscale</span> is to calculate intervals (differences) rather than note values. An appropriate adjustment is made to accommodate an embracing pure octave:

```
     2-~/\0,(}.pscale),1200NB. Pythagorean scale intervals
  114 90 114 90 114 90 90 114 90 114 90 90
 C      D      E   F     G      A      B  C
```

Every semitone here is worth one of two values, namely 90 cents or 114 cents. If now the two semitones in the diatonic scale (that is the C scale without any black notes) are equalised at 90 by interchanging the 114 and 90 between E and F#, then the five whole tones in the diatonic scale are also equalised at 204 cents. (5×204) + (2×90) = 1200, which confirms the purity of the octave. The difference of 24 cents between some semitones and others is not generally objectionable in a melody, and harmonically speaking all semitones are discordant anyway!

As an aside, the result of tuning by successive fourths is that each note is a comma smaller than its counterpart in fifths tuning, so that there is a shortfall of 23.5 cents rather than an excess when the octave is reached:

```
   cent \:~v2 3r4^i.13
0 90.2 180 294 384 498 588 678 792 882 996 1086.3 1177.5
```

## Just Intonation

Just intonation systems are developments of the ideas of the preceding section. Some of these ideas were known in the time of Ptolemy in the second century A.D., hence the occasional use of the term “Ptolemaic tuning” as a synonym. They gave rise to vigorous debate amongst musical theorists as early as the fifteenth century.

To illustrate, consider a scheme of intervals which accepts C – D as 204 cents as above, corresponding to a harmonic value of 8/9, but make the next tone D – E equal to 386 - 204 = 182 cents, a reduction of a comma. Reducing the interval G – A by the same amount simultaneously adjusts both the F – A and G – B thirds to the pure value of 386. This leaves the two diatonic semitones to take up the slack of 44 so each becomes 90 + 22 = 112. Now consider the chromatic notes. D – F# is currently 182 + 112 + 114 = 408, in excess by a comma. Switching the semi-tone values between F and G, and changing them slightly from 114/90 to 112/92 makes D – F# pure, as are also F# - A and G# - B. This little bit of ingenuity leads to the scheme

```
    ]just=.92 112;90 92;112;92 112;92 90;112 92;112
 +------+-----+---+------+-----+------+---+
 |92 112|90 92|112|92 112|92 90|112 92|112|
 +------+-----+---+------+-----+------+---+
 C      D     E   F      G     A      B   C
```

in which there are two types of whole tone, some 8/9 and others 9/10. It is now possible to use J to observe the effects of this particular just tuning on the principal intervals based on all the different possible starting notes:

```
    2+/\13$;just          NB. whole tones
 204 202 182 204 204 204 204 182 202 204 204 204
 C   C#  D   Eb  E   F   F#  G   G#  A   Bb    B
    7+/\18$;just          NB. fifths
 702 702 680 702 702 702 702 702 700 702 702 702
    4+/\15$;just          NB. major thirds
 386 406 386 408 408 386 406 386 406 408 408 406
    9+/\20$;just          NB. sixths
 884 904 884 906 906 906 904 884 904 906 906 906
```

All but one of the fifths remains pure, but most of the major thirds and sixths are sharp by 22 cents, that is, within rounding, a syntonic comma. As with Pythagorean tuning there are considerable differences in size between some semi-tones and others.

Other theorists had different ways of getting around the “pure thirds” problem, and because of the *ad hoc* nature of such systems, just systems are sometimes referred to as irregular temperaments. (It is no accident that the word “temperament” shares the same Latin root as “tamper”). It is doubtful whether such systems were much applied in practice to harmonised music after the sixteenth century.

## Mean-tone Temperaments

Mean-tone systems were designed primarily for keyboard instruments, and overcome the objection of different cent values for the two whole tones in a major third by replacing them with their average value, hence the name “mean-tone”. As already noted, the pure third measures 386.3 cents, so that the two tones which comprise it have a *mean* value of 193.15 cents (cf. 204 in the just system). This mean is the size of all the whole tones in this system. Carrying out the sort of accountancy in the previous paragraph means that the values of the semi-tones intervals must be half of 1200 - (5×193.15) = 117 cents.

This in turn means that those semi-tones which are not part of the diatonic scale must have the value 193 - 177 = 76 (cf. 92 under the just tuning described above) leading to a cent scale

```
 +------+------+---+------+-----+-------+---+
 |76 117|117 76|117|76 117|76 117|117 76|117|
 +------+------+---+------+-----+-------+---+
 C      D      E   F      G     A       B   C
```

in which there is nearly a quarter-tone difference in values between some semi-tones and others. (n.b. the numbers in the above list total 1199 due to a small rounding effect.)

But we can go further than this. From the above

<pre><span class="APLU">    /:~2r3 ptune i.13
 23.5 114 204 318 408 522 612 702 816 906 1020 1110 1200
 oct              3</span><span class="APLU">rd</span> <span class="APLU">4</span><span class="APLU">th</span> <span class="APLU">    5</span><span class="APLU">th</span> <span class="APLU">    6</span><span class="APLU">th</span> </pre>

shows that the purity of the fifth is obtained at a quite considerable over-valuation of 408 for the third. The effect of flattening the fifth by lowering its cent value has already been investigated. Another way to measure the effects of such flattening is to express it as a fraction of the syntonic comma. A natural fraction to use is 1/4 since that spreads the effect equally over the four Pythagorean steps needed to reach a third.

```
     /:~(cent^:(_1)702-21.5%4)ptune i.13
 0 76.36 193.2 269.6 386.5 462.8 579.7 696.6 773 889.9 966.2 1083 1159
```

This time the octave is undershot at 1159, so that although the third is nearly pure at 386.5, the observed comma at the octave is 41. A whole range of possible compromises can be tested by, for example

```
   ((cent^:_1)&><702-1r3 2r7 1r4 2r9 1r5 1r6 3r14*21.5)ptune&><1 4 3 12
                            NB. fraction of observed comma
                            NB. syntonic comma,  at octave

 694.8 379.3 884.5 1138     NB.    1r3                    62
 695.9 383.4 887.6 1150     NB.    2r7                    50
 696.6 386.5 889.9 1159     NB.    1r4                    41
 697.2 388.9 891.7 1167     NB.    2r9                    33
 697.7 390.8 893.1 1172     NB.    1r5                    28
 698.4 393.7 895.2 1181     NB.    1r6                    19
 697.4 389.6 892.2 1169     NB.    3r14                   31
```

The first three columns above represent fifths, major thirds and sixths which should be compared as before with the pure values of 702, 386.3 and 884.4 respectively. The final column is subtracted from 1200 to give the *observed* comma at the octave. The third row confirms the purest thirds at 1/4 comma, whereas the first row gives almost pure sixths at the expense of a big comma at the octave.

## Frequencies

In terms of frequencies life is even simpler, since for Pythagorean tuning the relative frequencies of notes in the scale are now compressed into

 

<pre>                   _____________________________
                    2<sup>0</sup>                        2<sup>1</sup>  </pre>

```
   fx=.<.@(2&^.)          NB. exponent
   fv=.%2&^@fx            NB. value
```

Using the verb fv of course requires some correction due to the comma effect and the practical requirement that successive octaves should have values 1 and 2 :

```
    /:~fv 1.5^i.13
1 1.014 1.068 1.125 1.201 1.266 1.352 1.424 1.5 1.602 1.688 1.802 1.898
C   C'    C#    D     Eb    E     F     F#    G    G#    A     Bb     B
 octave dim 2nd 2nd dim 3rd 3rd  4th dim 5th 5th dim 6th 6th dim 7th  7th
```

Under equal temperament the corresponding frequencies are

```
    /:~(%e12)^i.12
 1  1.059 1.122 1.189 1.26 1.335 1.414 1.498 1.587 1.682 1.782 1.888
```

## Comparison of Systems

The following table summarises in cents the values of the notes of the chromatic scale of C in some of the systems of tuning considered in detail above, prior to comma adjustment to make the octave pure :

```
             C  C#  D   Eb  E   F   F#  G   G#  A   Bb    B    C
 eq-temp:    0 100 200 300 400 500 600 700 800 900 1000 1100 1200
 Pyth :      0 114 204 318 408 522 612 702 816 906 1020 1110 1223
 just :      0  92 204 294 386 498 590 702 794 884  996 1088 1200
 1r4 mean    0  76 193 270 386 463 580 697 773 890  966 1083 1159
```

There is nothing in the above which cannot be found in, say, *Encyclopedia Brittanica*, or *Grove’s Dictionary of Music and Musicians*. However, the accounts there are not altogether easy to understand, and exposition in J would have helped me greatly. Perhaps like so many others who have followed in Ken’s footsteps, I am just temperamentally inclined towards J ...

\[received 12 March 2005\]
