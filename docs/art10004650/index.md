---
vid: '10004650'
title: Recreation with Transcendental Numbers
authors:
- Gérard Langlet
byline: by Gérard Langlet
volume: '8'
issue: '2'
page: '25'
source: trad/v082/langlet82_25.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1991
tags:
- competitions and puzzles
- programming techniques
---

by Gérard Langlet
{ .byline }

First, let us fix `⎕PP←16` to have the maximum precision. What is the shortest APL expression producing the Golden Number? Probably:

```
       .5×1+5*.5
1.618033988749895
```

Then, what is the shortest expression without using `*`? I propose:

```
       ⍎112⍴'1+÷'
1.618033988749895
```

(This is an interesting use of Execute, that may be generalised to produce other transcendental numbers, summing continuous fractions. Moreover, in APL.68000, the expression becomes as short as the preceding one, if one knows that the closing quote may be omitted in that implementation...)

This gives the following idea: What is the shortest APL expression to get the Golden Number using only `1` either as a numeral or as a character? I propose:

```
       ⍎'1',111⍴'+÷1'
1.618033988749895
```

Then, is it possible to obtain the same number without any figure or numeral, even as a character?

```
       ⍎⍎'''',I,''',',I,I,I,'⍴''+÷'',I',⍴⍎I←⍕'I'='I' 1.618033988749895
```

(The `⍎` on the right is necessary in APL\*PLUS II: `⍕` of a scalar being a vector of length 1.)

Now, knowing that

```
       ⍎111⍴'1+÷1+'     ⍝ returns 2*.5:
1.414213562373095
```

we may try:

```
       ⍎⍎I,I,I,'⍴I,''+÷'',I,''+''',⍴⍎I←⍕'I'='I' 1.414213562373095
```

Similarly, a good value of `10*.5` is obtained by:

```
        ⍎1111⍴'1+1+1+÷1+1+ 1+'      ⍝ (Mind the blank!) 3.162277660168379
```

Exercise: find the expressions that return all the famous transcendental numbers without using numbers!

(webpage generated: 20 January 2007, 23:26)
