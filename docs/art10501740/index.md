---
vid: '10501740'
title: Conquering recursion
authors:
- John Earnest
byline: John Earnest (johnearnest@gmail.com)
volume: '27'
issue: '1'
abstract: This article presents a set of combinator patterns which can be used to decompose recursive procedures into smaller, more reusable components. Examples of the resulting programming style are contrasted with their more conventional equivalents. We additionally show that the semantic information gained by using these combinators offers opportunities for improved performance.
source: ../../wayback/archive.vector.org.uk/art10501740.html
wayback: https://web.archive.org/web/20200924233332id_/http://archive.vector.org.uk/art10501740
converter: vecrescue 0.1.0
issue_tab: n1-20162022
tags:
- programming techniques
- language design
- performance
---

John Earnest (johnearnest@gmail.com)
{ .byline }

![](featuredImg_conquering-recursion.jpg){ .lead-image }

This article presents a set of combinator patterns which can be used to decompose recursive procedures into smaller, more reusable components. Examples of the resulting programming style are contrasted with their more conventional equivalents. We additionally show that the semantic information gained by using these combinators offers opportunities for improved performance.
{ .abstract }

## Background

Vector languages inspire a unique programming ethos. It is considered good style to avoid loops, instead favoring adverbs which capture abstract patterns of iteration. The behavior of the K *over* could be captured with a `do[]` loop, but *over* is far more specific and thus more suggestive to a reader or interpreter. By reducing the number of moving parts in a program, one can avoid would-be bugs before they're written.

Conditional statements- both the K `if[]` and `$[;;...]` (*cond*)- should likewise be used sparingly. Sometimes a conditional is the most efficient or convenient way to express logic, but there is one context in which conditionals are unavoidable: recursive procedures. With cleverness, many problems which appear to require recursive solutions yield to a vector solution, such as the idiom `,//` for flattening an arbitrarily-nested structure. For other problems, however, a recursive solution is by far the most natural and concise. How do we solve these problems without using explicit conditionals and recursive calls?

The <cite>Joy</cite>[\[1\]](#ref1) programming language contains an unusual and enticing set of features which are applicable to our problem. Joy offers a variety of <em>combinators</em>- pure, higher-order functions which strictly rearrange and apply their arguments- to express the manipulation of data and control flow. Among these are combinators which capture abstract patterns of recursion.

Linear- and tail-recursive algorithms, being isomorphic to loops, have straightforward translations to an adverb-based equivalent and are therefore idiomatically expressed in K without recursion. In this discussion we will focus specifically on recursive procedures which exhibit regular "fanout". This occurs when traversing trees or problem spaces which implicitly have a similar structure. Building on ideas from Joy, we will describe a generalization of its `binrec` combinator which can be expressed neatly in K. The following examples and discussion incorporate a number of suggestions by my friend and colleague Stevan Apter.

## Constructing `rec[]`

All the examples in this article will be given in K4, but should be applicable, with minor modifications, to any dialect of the language. As a refresher, `.z.s` is how K4 references the current function, for self-recursion. K4 lambdas, verbs and adverbs have typecodes (obtained via `@:`) greater than 99. Dictionaries and tables have type codes 99 and 98, respectively.

Consider the definition of the following pentad, `rm`:

```
  rm: {[p;f;d;c;x]$[p x;f x;c@.z.s[p;f;d;c]'d x]}
```

For clarity, we will refer to the arguments `p`, `f`, `d` and `c` as *predicate*, *finalize*, *divide* and *combine*, respectively. If the *predicate* of `x` is true, *finalize* `x`. Otherwise, *combine* the results of recursively invoking `rm` with each of the results of *dividing* `x`.

This combinator is meant to be projected along its first four arguments, yielding a monad. Compare two definitions which recursively calculate the Nth term of the Fibonacci sequence. `fib1` uses conventional explicit recursion while `fib2` is written in terms of `rm`:

```
  fib1: {$[2>x;x;+/.z.s'x-1 2]}
  fib2: rm[2>;::;-2 -1+;+/]
```

The first thing you may notice about our `rm`-based formulation is that it now consists entirely of <em>tacit expressions</em>- there is no need for an explicit variable named `x`. The same sequence of operations takes place, but we have separated the components of the algorithm in `fib1`, allowing us to consider their roles separately. *predicate* distinguishes between the base case and recursive case of the definition, *divide* identifies the children of the current node of the implied recursive tree, and *combine* fuses results together. In this particular example, *finalize* is the identity function, but we will see other situations where it is more complex.

Sometimes *combine* needs context lost in the process of *dividing*. For these situations we'll use a variation on `rm` which we will call `rd`, for which *combine* is a dyad:

```
  rd: {[p;f;d;c;x]$[p x;f x;c[.z.s[p;f;d;c]'d x;x]]}
```

To illustrate the necessity of *rd*, consider a procedure `incx` which increments the atomic leaves of a simple expression tree:

```
  incx: rd[@:;1+;1_;{(*y),x}]

  incx (`plus;(`minus;1;5);8)
(`plus;(`minus;2;6);9)
```

We can always distinguish between times `rm` or `rd` is appropriate by considering the valence of *combine*. As a matter of tidiness, let's write a single procedure which captures both:

```
  @:'({x};{x+y};+/;1;2.0)
100 100 107 -7 -9h
  ambiv: {$[99<@x;x[y];x]}/
  ambiv[{-x};3 5]
-3
  ambiv[{x,2*y};3 5]
3 10

  ra: {[p;f;d;c;x]$[p x;f x;{$[99<@x;x[y];x]}/[c;(.z.s[p;f;d;c]'d x;x)]]}
```

Another refinement is to observe that the base case represented by *finalize* often consists of returning a constant value such as `0` or `()`. We can simplify the use of our combinator by dispatching on the type of this argument instead of requiring constant nilads. Bringing everything together, we have `rec`:

```
  {$[99<@x;x 10;x]}'(2*;5)
20 5

  rec: {[p;f;d;c;x]
        $[p x;$[99<@f;f x;f];{$[99<@x;x[y];x]}/[c;(.z.s[p;f;d;c]'d x;x)]]}
```

## Strategies

By taking apart the components of recursive procedures using `rec`, we can begin to make observations about their properties. *Predicate* and *divide* capture everything about the way we traverse the concrete or implied recursive tree embodied in our algorithm. We can refer to this pair of functions as a *traversal strategy*, leaving *finalize* and *combine* as a *reduction strategy*.

If we write several procedures which operate on the same data structure, we will often be able to reuse the traversal strategy. In the following examples we define a procedure `countx` which counts the nodes of an expression tree and a procedure `razex`, which accumulates the leaf values of a similarly structured expression tree. Both routines share code for their traversal strategy, so it is only necessary to specify what to do with leaves and what to do to nodes.

```
  walkx:  rec[@:;;1_]
  countx: walkx[0;1++/]
  razex:  walkx[::;?,/]

  s: (`times;(`plus;1;2);(`divide;`x;9))
  countx s
3
  razex s
(1;2;`x;9)
```

Since the reduction strategy of these procedures does not depend on the structure of the tree (as would have been the case with `incx`), we can replace the traversal strategy and apply our procedures to a different structure:

```
  walkd:  rec[{~|/99 98=\:@x};;{x`args}]
  countd: walkd[0;1++/]
  razed:  walkd[::;?,/]

  d: `op`args!(`times;+`op`args!(`plus`div;(1 2;(`x;9))))

  countd d
3
  razed d
(1;2;`x;9)
```

Furthermore, with a traversal strategy established, we can conveniently build up reductions incrementally, in an interactive manner. Fixing *finalize* and *combine* as identity functions we see the structure of leaves. Plugging in a more elaborate definition of *combine* shows the structure of nodes. This is a very pleasant and intuitive way to develop tree-manipulating programs:

```
  walkd[::;::]d
(1 2
 (`x;9))

  walkd[::;{(y`op),x}]d
(`times
 (`plus;1;2)
 (`divide;`x;9))

  walkd[();{(y`op),x};d]
(`times
 (`plus;();())
 (`divide;();()))

  walkd[();{?,/(y`op),x};d]
`times `plus `divide
```

## Further Taxonomy

Let's look at a few more examples which illustrate additional design patterns. A *skeleton vector* is a flat representation of a tree's structure. Every item of a skeleton vector contains the index of its parent item in the original tree, and the root contains -1. This representation is highly amenable to vector algorithms- a parallel breadth-first traversal can be performed using dyadic *over*. `skel` produces a skeleton vector for a simple list of nested lists. `vals` extracts leaf items in a flat list corresponding to the skeleton:

```
  skel: rec[0>@:;,-1;::;{x,0,(#x)+1_ y}/[,-1;]]
  vals: rec[0>@:;::;::;`,,/]

  t: (`a;`b`c;(`d;`e`f);`g`h)
  skel t
-1 0 0 2 2 0 5 5 7 7 0 10 10
  vals t
``a``b`c``d``e`f``g`h
```

The traversal strategy `rec[0>@:;;::;]` appears frequently, and for simple trees is equivalent to the conforming behavior of atomic monads like `-:`. If a K interpreter had special knowledge of `rec`, it would be relatively simple to recognize common patterns of arguments and use faster specialized implementations of the combinator in these situations, much like J's <cite>Special Combinations</cite>[\[2\]](#ref2). Such an implementation need not even be recursive if it used intermediate data structures instead of the stack. An iterative implementation of `rec` could allow for traversal of much larger structures than stack limits ordinarily permit.

Even with more complex problems, decomposing your algorithm into small parts makes them easier to test (and thus develop) independently. Consider the following implementation of a recursive mergesort- The recursive parts of the algorithm are trivially easy to express, leaving the hard parts as well-isolated components.

```
  merge: {&/c@'+{y+t=&/t:x@'y}[c:x,\:0W]\[#1_,/x;&,#x]}
  msort: rec[2>#:;::;2 0N#;merge]

  2 0N#1 5 7 2 3 8
(1 5 7
 2 3 8)
  merge (1 5 7;2 3 8)
1 2 3 5 7 8
  msort 1 9 2 3 4 8 0
0 1 2 3 4 8 9
```

The `merge` function combines a pair of sorted lists to create a larger sorted list in linear time. It works by performing two passes. First, a *scan* which walks indices along the input lists, advancing the index pointing to the minimum element at each step. The second pass uses these indices to select the appropriate elements from the input list in sorted order. Input is padded with `0W` (positive integer infinity) so that indices do not advance past the end of any input lists before all other input values have been consumed. Note that the implementation of `merge` makes no assumption that it deals with two input lists- if *divide* is tweaked to divide the input list into smaller segments the algorithm will still work correctly.

If you're doing enough work during a recursive procedure, you might benefit from making your code parallel. Given a parallel implementation of *each* (`peach`), you could create a variant of rec which fans out in parallel for the first few layers and then falls back to the ordinary implementation:

```
  prec: {[n;p;f;d;c;x]$[p x;f x;c@($[n>0;.z.s[n-1];rec])[p;f;d;c]peach d x]}

  prec[2;2>;::;-2 -1+;+/]'!10
0 1 1 2 3 5 8 13 21 34
```

Another possibility worth exploring is memoization. If all the components of our recursive algorithm are pure functions, we can safely cache intermediate results in a dictionary and avoid recomputing them. Here's a simple example of an alternate implementation of `rm` which behaves this way:

```
  rmm: {[s;n;p;f;d;c;x]$[~n~s x;:s x;]
        @[s;x;:;$[p x;f x;c@.z.s[s;n;p;f;d;c]'d x]]
        s x}

  mem: (0#0)!0#0
  fm:  rmm[`mem;0N;2>;::;-2 -1+;+/]

  fm'!10
0 1 1 2 3 5 8 13 21 34

  mem
0 1 2 3 4 5 6 7 8 9!0 1 1 2 3 5 8 13 21 34

  \t fm 1000
9

  \t {$[2>x;x;+/.z.s'x-1 2]}'!30
3278
```

## Conclusions and future work

We have constructed and explained `rec`, a combinator which captures a wide variety of useful recursive algorithms. Decomposing these algorithms reveals ways to abstract and reuse code which walks or transforms data structures. Abstracting recursion also frees an implementation to evaluate recursive procedures in a specialized manner, permitting speedup of operations on common structures or given other constraints.

Nothing about `rec` is uniquely applicable to K; it could be readily adapted to a variety of other languages. K's flexible support for projection, concise tacit function trains and adverbs do, however, allow many examples to come out nicely. Compare the K definitions of `rec` and `fib2` to the equivalent in ECMAScript 6:

```
var rec = (p,f,d,c,x) => p(x)?f(x):c(d(x).map(x => rec(p,f,d,c,x)));

var fib = x => rec(
  x => 2>x,
  x => x,
  x => [x-2, x-1],
  x => x.reduce((a,b) => a+b),
  x
);
```

This discussion only scratches the surface of abstract recursion. Some recursive functions are very clumsy to express using `rec`- consider the mutual recursion of a recursive descent parser. If `p` and `f` must identify and handle a variety of possible base cases, they will often end up repeating logic. Furthermore, the Ackermann function is an example of an irregular recursive pattern which `rec` cannot capture:

```
  ack: {$[0=x;y+1; 0=y;.z.s[x-1;1]; .z.s[x-1;.z.s[x;y-1]]]}
```

There may be many other variations on the ideas in this paper, as well as totally novel combinators for describing other patterns, waiting to be found.

## References

1. <a id="ref1"></a>The Joy Programming Language [http://www.kevinalbrecht.com/code/joy-mirror/joy.html](http://www.kevinalbrecht.com/code/joy-mirror/joy.html)
2. <a id="ref2"></a>Special Combinations [http://code.jsoftware.com/wiki/Vocabulary/SpecialCombinations](http://code.jsoftware.com/wiki/Vocabulary/SpecialCombinations)
