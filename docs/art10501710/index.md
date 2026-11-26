---
vid: '10501710'
title: Writing a utility function
authors:
- Dan Baronet
byline: Dan Baronet danb@dyalog.com
volume: '27'
issue: '1'
received: '2016-05-01'
online: '2016-08-30'
editor: John Jacob
description: This paper tackles writing a piece of code in APL that will be reused often. It is for relatively new APLers
source: ../../php-site/content/printed/271/writingutilfn/writingutilfn.htm
converter: vecrescue 0.1.0
issue_tab: n1-20162022
tags:
- programming techniques
- education
---

![](featuredImg_writing-a-utility-function.jpg){ .lead-image }

Dan Baronet  
danb@dyalog.com
{ .byline }

This is an article on writing a piece of code in APL that will be reused often. It is aimed at people relatively new to APL.

It depends on what you are trying to achieve. Is it supposed to be a blazingly fast specialized utility or will it be a general-covers-all-possibilities utility?

Let's take an example. APL has a Rotate function, `⌽`. It is dyadic, it requires two arguments. `⌽` will rotate a vector, a matrix, an N dimensional array, on any axis. It will do it from the front or from the back. Like this:

```
    V←⍳7
    V
1 2 3 4 5 6 7
    3 ⌽ V
4 5 6 7 1 2 3
    ¯3 ⌽ V
5 6 7 1 2 3 4
```

APL does not have a SHIFT function. For example, it might be useful to have a function that Shifts instead of rotating, left or right by dropping the first N elements and padding with a prototype. As in

```
    V
1 2 3 4 5 6 7
    3  S  V
4 5 6 7 0 0 0
    ¯3  S  V
0 0 0 1 2 3 4
```

Let's say we need a function to shift ONE number to the left. A solution could be

```
    ∇r←S1 vec
[1]   r← 1 ↓ vec,0
    ∇

    S1 V
2 3 4 5 6 7 0
```

No need for a left argument. This function is very limited in domain and only works sensibly on a numeric vector.

If we need a more general function, one that could shift N places, like Rotate, we could add a left argument to specify the number of places to shift as in

```
    ∇r← n S2 vec
[1]   r← n ↓ vec,n⍴0
    ∇
```

Obviously `S1 V` is faster than `1 S2 V` but it is specialized and does less work. S2 is more general. Both would probably produce undesirable results with a string:

```
    2  S2  'danb'
nb 0 0
```

here we get a mixture of characters and numbers (0s). We probably should prefer spaces instead of zeroes. S3 fixes that:

```
    ⍴⎕← 2 S3 'danb'
nb
4
    ∇r←n S3 v
[1]   r←(⍴v) ↑ n↓v
    ∇
```

Speed wise `S2` and `S3` are pretty much equivalent.

But they only work by shifting to the left. We could modify `S3` to “take” from either direction by using the sign of “n” to take:

```
    ∇r←n S4 v
[1]   r← ((×n)×⍴v) ↑ n↓v
    ∇

    ¯3  S4  'dbaronet'
   dbaro
```

This will slow it down a bit but has the advantage of working both ways. An important disadvantage is that it doesn't work with 0:

```
    ⍴0 S4 V
0
```

Oops! Let's fix that. The sign of 0 is 0 and we're taking 0 elements. It should be 1 (or -1) when 0:

```
    ∇ r←n S4 v
[1]    r←((×n+n=0)×⍴v) ↑ n↓v
    ∇
    0 S4 V
1 2 3 4 5 6 7
    ¯3  S4  'dbaronet'
   dbaro
```

That's better.

Is this enough? Do we need a function that would work for higher rank arrays? As in

```
    ⎕← M← 4 3⍴ ⍳12
 1  2  3
 4  5  6
 7  8  9
10 11 12

    2  S5  M
 7  8  9
10 11 12
 0  0  0
 0  0  0
```

If so, we need to modify `S4` like this:

```
    ∇r←n S5 v;s
[1]   n←(⍴⍴v)↑n  ⍝ make sure we specify the shift for each dimension
[2]   s←×n+n=0   ⍝ a 0 shift means take all on that dimension
[3]   r←(s×⍴v) ↑ n↓v
    ∇

    2 0  S5  M
 7  8  9
10 11 12
 0  0  0
 0  0  0
    ¯2 1  S5  M
0 0 0
0 0 0
2 3 0
5 6 0
```

Note we only need to specify the leading axes' shift values.

We now have a generalized utility to shift any array in any direction in any dimension. This should work in any known APL.

## <a id="addendum"></a>Addendum

If your APL supports dynamic functions you could use

```
S1← {1↓⍵,0}
S2← {⍺↓⍵,⍺↑0}
S3← {(⍴⍵)↑⍺↓⍵}
S4← {((×⍺+0=⍺)×⍴⍵)↑⍺↓⍵}
S5 ←{
   n←(⍴⍴⍵)↑⍺  ⍝ make sure we specify the shift for each dimension
   s←×n+n=0   ⍝ a 0 shift means take all on that dimension
   (s×⍴⍵) ↑ n↓⍵}
```

or trains if they are available:

```
    S1← 1 ↓ ,∘0
    S2← ⊣ ↓ ⊢ , 0 ↑⍨ ⊣
    S3← ⊢∘⍴ ↑ ↓
    S4← ((× ⊣ + 0=⊣) × ⊢∘⍴) ↑ ↓
```

`S5` is feasible as a train but too much of a mouthful for this article .

## <a id="epilogue"></a>Epilogue

This was only a brief look at writing utilities in general. The key idea here is that the more general your code is the more costly it is likely to be.

You have to decide for yourself how much time and effort is worth the problem. Maybe a series of specialized utilities is what you need.

Happy coding.

## <a id="references"></a>References

1. If you want to see a video about this take a look at  
    [www.youtube.com/watch?v=iRko0SirZ2Q](https://www.youtube.com/watch?v=iRko0SirZ2Q)
