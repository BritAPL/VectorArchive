---
vid: '10500270'
title: 'In Session: On average'
prefix: In Session
authors:
- Roger Hui
byline: Roger Hui
volume: '24'
issue: '2'
page: '122'
source: content/printed/242/insession.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- programming techniques
- language design
---

Roger Hui
{ .byline }

How do you compute the average in APL? Many authors and speakers say:

```
      avg←{(+/⍵)÷⍴⍵}
```

The function is often used to demonstrate the beauty and power of the language. It is presented in the [About the APLs](https://web.archive.org/web/20080701182900/http://www.vector.org.uk/?area=about) page of *Vector*. It is the first program in the *Dyalog APL Tutorial* (page 10). At the recent Dyalog User Conference in Princeton it was presented in at least three sessions, and in one was described as “the very best APL expression”.

Let’s look into it:

```
      avg 1 2 3 4
2.5
```

So far so good. What about

```
      avg¨ (1 2) (3 4 5) (6 5 4 3.14159)
 1.5  4  4.5353975
```

The extra blanks hint at the trouble. Application of the monad `↑` to the result brings the problem into relief:

```
      ↑ avg¨ (1 2) (3 4 5) (6 5 4 3.14159)
1.5
4
4.5353975
```

That is, `avg` should return a scalar rather than a vector result.

Now consider:

```
      avg 1 2 3 4
2.5
      avg 1 2 3
2
      avg 1 2
1.5
      avg 1
```

What just happened with the last expression? A few moments of reflection reveal that `avg` mishandles scalar arguments.

What about matrices and higher-ranked arrays?

```
      ⎕←data←?3 4⍴10
1 7 4 5
2 0 6 6
9 3 5 8

      avg data
RANK ERROR
avg[0] avg←{(+/⍵)÷⍴⍵}
           ∧
```

In summary, the problems with `avg←{(+/⍵)÷⍴⍵}` are:

- It gives a non-scalar result for vector arguments.
- It fails on non-vector arguments; in particular, it gives a puzzling (and wrong) result for scalar arguments.

Fortunately, a better definition readily obtains. It is somewhat longer than the commonly promulgated version, but fixes its defects:

```
      avg1←{(+⌿⍵)÷⍬⍴(⍴⍵),1}

      avg1 1 2 3 4
2.5

      avg1¨ (1 2) (3 4 5) (6 5 4 3.14159)
1.5 4 4.5353975

      ↑ avg1¨ (1 2) (3 4 5) (6 5 4 3.14159)
1.5 4 4.5353975

      avg1 1
1

      avg1 data
4 3.333333333 5 6.333333333
```

There is one more point:

```
      avg ⍳0
1
      avg1 ⍳0
1
```

I would argue that `0` is a better answer for the average of an empty vector. For example, `(+/⍵)÷⍴⍵ ←→ +/⍵÷⍴⍵` and `(+⌿⍵)÷⍬⍴(⍴⍵),1 ←→ +⌿⍵÷⍬⍴(⍴⍵),1` for vector `⍵`, except when `⍵` is an empty vector. If instead `0` is that average, then the identity holds for all vectors. Possibly `1` is a reasonable answer, but if so it should be a considered answer and not an unintended consequence of that `0÷0` is `1`.

Finally, the common definition of average in J *is* arguably “the best expression in J”:

```
   avgj=: +/ % #

   avgj 1 2 3 4
2.5

   avgj&> 1 2 ; 3 4 5 ; 6 5 4 3.14159
1.5 4 4.5354

   avgj data
4 3.33333 5 6.33333

   avgj 1
1
   avgj i.0
0
```

Together with the rank operator (`"`), this definition of the average makes it easy to find the average of various parts of an array.

```
   avgj"1 data
4.25 3.5 6.25

   ] x=: ? 2 3 4 $ 20
10  7 18  9
 2 12 15  5
15 13 18 10

 0  5  9  6
14  0  6  4
 9  8 15 17

   avgj x
 5    6 13.5  7.5
 8    6 10.5  4.5
12 10.5 16.5 13.5

   avgj"3 x
 5    6 13.5  7.5
 8    6 10.5  4.5
12 10.5 16.5 13.5

   avgj"2 x
      9 10.6667 17 8
7.66667 4.33333 10 9

   avgj"1 x
11 8.5    14
 5   6 12.25

   avgj"0 x
10  7 18  9
 2 12 15  5
15 13 18 10

 0  5  9  6
14  0  6  4
 9  8 15 17
```
