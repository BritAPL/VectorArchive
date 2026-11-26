---
vid: '10500690'
title: What is an array?
authors:
- Roger Hui
byline: by Roger Hui (rhui000@shaw.ca)
volume: '25'
issue: '3'
page: '29'
received: '2011-03-29'
online: '2011-12-21'
editor: Stephen Taylor
description: Interpreter implementor Roger Hui reflects on the nature of arrays
keywords:
- APL
- J
- Iverson
- Scholes
- array
- arrays
- function
- index
- infinity
- string
- scalar
- string scalar
source: content/printed/253/hui.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- language design
---

by Roger Hui (rhui000@shaw.ca)
{ .byline }

![](featuredImg_what-is-an-array.jpg){ .lead-image }

In a recent e-mail [\[1\]](#ref1 "Scholes, J.M., e-mail on
			2010-10-11 11:41."), John Scholes reminded me of his last encounter with Ken Iverson, originally described as follows [\[2\]](#ref2 "Christensen, G., Ken Iverson in Denmark, Vector,
			Volume 22, Number 3, 2006-08."):

> In Scranton in 1999 during one of the sessions I was sitting next to Ken, and he leaned over and said to me – in his impish way – John, what is an array? Now I knew better than to rush into an answer to Ken. I guess I’m still working on my answers to that question.

Fools rush in where angels fear to tread…

## What is an array?

An *array* is a function from a set of indices to numbers, characters, … A rank-n array is one whose function *f* applies to n-tuples of non-negative integers. A rank-n array is *rectangular* if there exist non-negative integer maxima s = (s<sub>0</sub>, s<sub>1</sub>, …, s<sub>n-1</sub>) such that *f* (i<sub>0</sub>, i<sub>1</sub>, …, i<sub>n-1</sub>) is defined (has a value) for all integer i<sub>j</sub> such that (0≤i<sub>j</sub>)^(i<sub>j</sub>&lt;s<sub>j</sub>). s is called the *shape* of the array. Etc.

This definition accommodates:

- APL/J rectangular arrays
- J sparse arrays
- infinite arrays
- dictionaries (associative arrays)

## APL/J rectangular arrays

A typical APL/J rectangular array:

```
   2 2 3 ⍴ 'ABCDEFGHIJKL'
ABC
DEF

GHI
JKL
```

Listing the indices with the corresponding array elements makes the index function more apparent:

```
0 0 0   A
0 0 1   B
0 0 2   C
0 1 0   D
0 1 1   E
0 1 2   F
1 0 0   G
1 0 1   H
1 0 2   I
1 1 0   J
1 1 1   K
1 1 2   L
```

APL rectangular arrays to-date have been implemented by enumerating the array elements in row-major order (and employ the ‘implementation trick’ of not storing the indices). But there are ways to represent a function other than enumerating the domain and/or range of the function.

## J sparse arrays

Sparse arrays were introduced in J in 1999 [\[3\]](#ref3 "Hui, R.K.W., Sparse Arrays in J, APL99 Conference
			Proceedings, APL Quote Quad, Volume 29, Number 2, 1999-08-10 to
			\-14."), [\[4\]](#ref4 "Hui, R.K.W., and K.E. Iverson,
			J Introduction and Dictionary, 2010."). In the sparse representation, the indices and values of only the non-‘zero’ elements are stored.

```
   ] d=: (?. 3 5 $ 2) * ?. 3 5 $ 100
 0 55 79 0  0
39  0 57 0  0
 0  0 13 0 51
```

```
   ] s=: $. d   NB. convert from dense to sparse
0 1 │ 55
0 2 │ 79
1 0 │ 39
1 2 │ 57
2 2 │ 13
2 4 │ 51
```

```
   3 + s
0 1 │ 58
0 2 │ 82
1 0 │ 42
1 2 │ 60
2 2 │ 16
2 4 │ 54
```

Reference [\[3\]](#ref3 "Hui, R.K.W., Sparse Arrays in J,
			APL99 Conference Proceedings, APL Quote Quad, Volume 29, Number 2,
			1999-08-10 to -14.") has an example of solving a 1e5-by-1e5 tridiagonal sparse matrix in 0.28 seconds.

## Infinite arrays

Infinite arrays were described by McDonnell and Shallit [\[5\]](#ref5 "McDonnell, E.E., and J.O. Shallit, Extending APL
			to Infinity, APL80 Conference Proceedings, 1980.") and Shallit [\[6\]](#ref6 "Shallit, J.O., Infinite Arrays and
			Diagonalizaton, APL81 Conference Proceedings, APL Quote Quad, Volume
			12, Number 1, 1981-09."). Having infinite arrays facilitates working with infinite series and limits of sequences.

<pre>
   ⍳4
0 1 2 3

   ⍳<span class="infin">∞</span>
0 1 2 3 4 5 …

   - ⍳<span class="infin">∞</span>
0 ¯1 ¯2 ¯3 ¯4 ¯5 …

   3 * - ⍳<span class="infin">∞</span>
1 0.333333 0.111111 0.037037 …

   +/ 3 * - ⍳<span class="infin">∞</span>
1.5

   ⌽ ⍳<span class="infin">∞</span>
DOMAIN ERROR
   ⌽⍳<span class="infin">∞</span>
  ^
</pre>

Infinite arrays can be implemented by specifying the index function as a function. For example, the index function for <code>⍳<span class="infin">∞</span></code> is the identity function, `⊢` or `{⍵}`.

Let `x` and `y` be infinite vectors with index functions `fx` and `fy`. If `s1` is a scalar monadic function, then `s1 x` is an infinite vector and its index function is `s1∘fx`, `s1` composed with `fx`. If `s2` is a scalar dyadic function, then `x s2 y` is an infinite vector and its index function is the *fork* `fx s2 fy`, or the dynamic function `{(fx ⍵) s2 (fy ⍵)}`.

In the following examples, the infinite vectors are listed with the index function, both as an operator expression (tacit function) and as a dynamic function.

```
   ⍳∞                   │  ⊢
0 1 2 3 4 5 6 7 …       │  {⍵}
                        │
   ∞ ⍴ 2                │  ⊢∘2
2 2 2 2 2 2 2 2 …       │  {2}
                        │
   - ⍳∞                 │  -∘⊢
0 ¯1 ¯2 ¯3 ¯4 ¯5 …      │  {-⍵}
                        │
   3 * - ⍳∞             │  (3∘*)∘-∘⊢
1 0.333333 0.111111 …   │  {3*-⍵}
                        │
   ⎕←x←3*⍳∞             │  3∘*∘⊢
1 3 9 27 81 243 729 …   │  {3*⍵}
                        │
   ⎕←y←(⍳∞)*2           │  *∘2∘⊢
0 1 4 9 16 25 36 …      │  {⍵*2}
                        │
   x+y                  │  3∘*∘⊢ + *∘2∘⊢
1 4 13 36 97 268 765 …  │  {(3*⍵)+(⍵*2)}
```

## Dictionaries (associative arrays)

The proposed *string scalars* are suitable for use as indices in dictionaries. For example:

```
   ⍴⍴Caps
1
   Caps["UK" "China" "France"]←'London' 'Beijing' 'Paris'
   Caps
"UK"     │ London
"China"  │ Beijing
"France" │ Paris

   Caps["China"]
 Beijing

   Caps["USA"]
INDEX ERROR
   Caps["USA"]
  ∧

   Caps ⍳ 'Paris' 'Tokyo' 'London'
"France" λ "UK"

   ⌽ Caps
DOMAIN ERROR
   ⌽Caps
  ^
```

## References

1. <a id="ref1"></a> Scholes, J.M., e-mail on 2010-10-11 11:41.
2. <a id="ref2"></a> Christensen, G., Ken Iverson in Denmark, *Vector*, Volume 22, No. 3, 2006-08.  [http://archive.vector.org.uk/art10002270](../art10002270/)
3. <a id="ref3"></a> Hui, R.K.W., Sparse Arrays in J, APL99 Conference Proceedings, *APL Quote Quad*, Volume 29, Number 2, 1999-08-10 to -14.
4. <a id="ref4"></a> Hui, R.K.W., and K.E. Iverson, J Introduction and Dictionary  [http://www.jsoftware.com/help/dictionary/d211.htm](http://www.jsoftware.com/help/dictionary/d211.htm), 2010.
5. <a id="ref5"></a> McDonnell, E.E., and J.O. Shallit, Extending APL to Infinity  [http://www.jsoftware.com/papers/eem/infinity.htm](http://www.jsoftware.com/papers/eem/infinity.htm), APL80 Conference Proceedings, 1980.
6. <a id="ref6"></a> Shallit, J.O., Infinite Arrays and Diagonalizaton, APL81 Conference Proceedings, *APL Quote Quad*, Volume 12, No. 1, 1981-09.
