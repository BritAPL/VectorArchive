---
vid: '10010040'
title: J-ottings 4
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '11'
issue: '3'
page: '17'
source: trad/v113/thomson113_17.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- language design
- education
---

by Norman Thomson
{ .byline }

J-ottings is about learning J rather than about J itself – that is left to those more expert. J is much more tantalising than APL ever was. Somehow it is much more difficult to get properly started, and yet the rewards of having done so are great. The J literature is in some respects too polished, which can lead to the feeling of running in a race where the leaders keep disappearing out of sight. It thus seemed worth while to record an account of some failures and wrong avenues encountered on the path to writing a simple J verb.

Eugene McDonnell in “At Play with J” (Vector Vol.10 No.3) articulated the fact that in learning new computer languages, there is a need to have as a handhold the confidence of being able to write simple multi-line programs in the style of more primitive languages. He described a nine-liner to compute primes – I propose to do something much simpler, namely emulate in J the Basic program:

```
10 i=1
20 if i=11 then exit
30 print i
40 i=i+1
50 goto 20
```

and to record a catalogue of intermediate failures. Of course 1+i.10 can achieve my objective at a stroke, but that is not the point. The object is to generate the feeling of security that comes from being able to do it in a step-by-step multi-line program, or as it is called in J, a multi-line verb (mlv).

Multi-line verbs come little and late in the J Introduction and Dictionary. A first reading leaves the vague feeling that they have something to do with something called suite ($.) which counts lines, and is somewhat similar to `⎕LC`. (Suite has in fact been removed from the more commercially oriented J Release 2, however I judge that readers of this section of Vector are more likely to continue to be users of the earlier shareware versions.)

In APL a user-defined function is an entity whose roots are well grounded in traditional programming. However, in J a multi-line verb is a table (or possibly pair of tables in the ambivalent case), where a table is a character matrix. By analogy with APL it is as if the Canonical Representation IS the function. The analogy of suite with `⎕LC` is quite strong in that suite is a vector of row numbers referencing the table, and represents the list of statement numbers which will be executed in sequence provided that this sequence is not interrupted by explicit assignment to suite. Suite is initially set to i.n where n is the number of rows in the table. When the value of suite becomes an empty vector, this is a signal to exit the verb.

A table is built up from its component rows using link (;). Suppose these rows are the character strings a,b,c,... Then define

```
table=.a;b;c
```

followed by

```
mlv=.table : ''
```

if the verb is monadic, or

```
mlv=.'' : table
```

if it is dyadic, or

```
mlv=.table1 : table2
```

if it is ambivalent.

a, b and c are NOT program variables within mlv; they are temporary names used to store the program lines as the verb is built up.

In editing simple tables I find it convenient to edit a line, then redefine table and mlv, since this is made very convenient by the line recall feature of the J interpreter.

Here is my first attempt at reproducing the Basic program above (remember rows are numbered in origin zero) :

```
a=.'$.=.(1+y.=10),i. y.~:10' NB. ~: is not equal
b=.']y=.y.+1' NB. y. is right argument
c=.'y.'

table=.a;b;c
mlv=.t : ''
```

The idea is that, assuming an argument of less than 10, suite will be set to 1 0 in line 0, so that y. is incremented and displayed following execution of line 1, then the 0 in suite restores control to the top line. This process is then repeated until eventually y.=10, suite becomes 2, 10 is displayed, and execution terminates. Before reading further see if you can spot the flaw.

The reason for it is stated clearly by Eugene, viz. the result of a verb is the result of the sentence executed last. Execution is thus silent in the sense that a verb such as the above does not produce a line-by-line result. Also, since all variables including y. and suite are local, it is not possible to work out after the event what happened within the verb. It is possible to write a verb

```
write=.1!:2&2
```

which uses one of the foreign conjunctions to transmit its argument to the screen, and so replacing \] in the second line with write helps, but now the 10 is displayed twice, once by the trace verb write, and once by virtue of the “result-is-last-sentence” rule.

Educated by my failure so far here is a second attempt at the verb (the intermediate stages of building up the table are omitted):

```
$.=.(10-y.)#1
y.=.y.,y.+1
```

This time I calculate in the first line the appropriate number of times the second line has to be repeated. At every stage the newly incremented value of y. is catenated, until last time round the full vector from start point to 10 is printed. Again try to spot the flaw before reading on.

Consider f 9. This indeed has the value 9 10 as anticipated. Now consider f 8. First time round y. becomes 8 9. Next time round 8 9 is joined to y.+1 to give 8 9 9 10 and so on.

A successful verb is:

```
$.=.(10-y.)#1
y.=.y.,1+{:y. NB. {: is tail
```

The above example illustrates a simple way to deal with if/then logic. Extension to the case statement follows in an obvious way:

```
a=.'$.=.y.'
b=.'''one'''
c=.'''two'''
d=.'''three'''

t=.a;b;c;d
f=.t : ''

f 2 two
```

J has labels which use ) where APL uses :, and so if/then logic can be expressed:

```
a=.'$.=.(y.=0)}lab2,lab1'
b=.'lab1)''zero'''
c=.'lab2)''not zero'''

t=.a;b;c
f=.t : ''
f 0 zero
f 7 not zero
```

This is used in a simple recursive verb to calculate triangular numbers.

```
a=.'$.=.(y.=0)}lab2,lab1'
b=.'lab1)r=.0'
c=.'lab2)r=.y.+f y.-1'

t=.a;b;c
f=.t : ''
f 5 15
```

In writing multi-line verbs it is not necessary to name each row explicitly in the table build-up phase. For example the above verb could be written:

```
a=.'$.=.(y.=0)}lab1,lab2' ; 'lab2)r=.0' ; 'lab1)r=.y. + f y.-1'

f=.a : ''
f 5 15
```

f in either form is of course a travesty of J style, nevertheless I consider it important to be ABLE to do it this way even although one wouldn’t! An acceptable J verb definition would use agenda(@.), tie(’) and $: which means “self-reference”:

```
f=.0:'(+$:@<:)@.* NB. <: is decrement by 1, * is signum
f 5 15
```

This says take the signum of the right argument. If it is zero use the verb 0: to initialize to 0. Otherwise add (+) the value of f used recursively ($:) after decrementing its argument by 1.

(webpage generated: 22 October 2007, 02:02)
