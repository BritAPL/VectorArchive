---
vid: '10500650'
title: Tables with calculated columns
prefix: No Stinking Loops
authors:
- Stevan Apter
byline: by Stevan Apter
volume: '25'
issue: '1'
page: '74'
received: '2011-02-05'
online: '2011-05-07'
editor: Stephen Taylor
description: K4 has a quite different way of representing tables from that used in K3. This article describes how to simulate in q the column dependencies that K3 supported.
keywords:
- K3
- K4
- q
- table
- calculated
- column
- dependency
- dependent
abstract: K4 has a quite different way of representing tables from that used in K3. This article describes how to simulate in q the column dependencies that K3 supported.
source: content/printed/251/apter.htm
converter: vecrescue 0.1.0
issue_tab: n1-june-2011
tags:
- programming techniques
- data and files
---

by Stevan Apter
{ .byline }

K4 has a quite different way of representing tables from that used in K3. This article describes how to simulate in q the column dependencies that K3 supported.
{ .abstract }

## 0. Dependencies

In K3, tables are pseudotypes, dictionaries of vectors. The ‘columns’ of a ‘table’ are first-class variables. Thus, using dot-notation:

```
  t.f:10 20 30
  t g:40 50 60
```

The ‘records’ of a table are the corresponding elements of the ‘column’ variables.

A variable is a data-structure with three components:

- a simple symbolic name, e.g. `` `f ``
- a value, e.g. `10 20 30`
- a recursive dictionary of attributes

The `d` attribute is used to define the variable as a functional dependency. For example, using double-dot-notation:

```
  t.h..d:"f+g"
  t.h
50 70 90
```

Thus, `h` is a variable in the dictionary `t`, having name `t`, value `50 70 90`, and an attribute dictionary containing a single variable with name `d`, value `"f+g"`, and empty attribute dictionary:

```
  t
.((`f
   10 20 30
   )
  (`g
   40 50 60
   )
  (`h
   50 70 90
   .,(`d;"f+g";)))
```

The evaluation of `t` recursively evaluates the columns `f`, `g`, and `h` of `t`. `t.h` depends on `f` and `g`. If either `f` or `g` changes, `h` is marked ‘invalid’. If `h` is invalid, then on reference it recalculates in `t` using `f` and `g`.

The K3 workspace is a tree of dictionaries rooted in the nameless dictionary. We can capture the entire workspace by evaluating the empty symbol; or, as one wag put it, “The value of nothing is everything”:

```
  .`
.((`k
   .,(`t
          .((`f
                 10 20 30
                 )
                (`g
                 40 50 60
                 )
                (`h
                 50 70 90
                 .,(`d;"f+g";)))
          )
   )
  (`t;-7.584544e+008;))
```

In q (i.e. K4) a table is a list of records – structurally identical dictionaries. The columns of a table are the corresponding entries of the records. (N.B. internally, q stores the table as a structure of column-lists.)

How then can we simulate in q the column dependencies we get for free in K3?

## 1. Tables and views.

In q we have tables:

```
q)t:([]f:10 20 30;g:40 50 60)
q)t
f  g
-----
10 40
20 50
30 60
```

and we have views:

```
q)v::select from t where g<60
q)v
f  g
-----
10 40
20 50
```

`v` depends on `t`. When `t` is modified, `v` becomes ‘invalid’. `v` will be recalculated (‘validated’) the next time it is referenced:

```
q)t+:10
q)t
f  g
-----
20 50
30 60
40 70

q)v
f  g
-----
20 50
```

## 2. Calculated columns

We can use `update` to add calculated columns to a table:

```
q)update k:f+g,l:neg f from t
f  g  k   l
-------------
20 50 70  -20
30 60 90  -30
40 70 110 -40
```

but q won’t allow us to add columns unless they depend strictly on existing columns in the table:

```
q)update k:f+g,l:k*2 from t
'k
q)update l:k*2,k:f+g from t
'k
```

Columns must be added as a correctly-ordered sequences of separate updates:

```
q)update l:k*2 from update k:f+g from t
f  g  k   l
-------------
20 50 70  140
30 60 90  180
40 70 110 220
```

## 3. Automating calculated columns

Let’s divide up the parameters to the problem and assign them to distinct variables:

<table class="left">
		<tr>
			<td class="code">t</td>
			<td>is a table</td>
		</tr>
		<tr>
			<td class="code">f</td>
			<td>
				is a data-structure containing the names and definitions of
				calculated columns
			</td>
		</tr>
		<tr>
			<td class="code">v</td>
			<td>
				is a view which depends on <code>t</code> and <code>f</code>
			</td>
		</tr>
	</table>

For example, `f` might be defined through a GUI, in which users specify the calculated columns.

`v` will be a view which functionally depends on `t` and `f`:

```
v::willbe[t;f]
```

The function `willbe` takes `t` and `f` as arguments and returns a table containing the columns of `t` plus the defined columns, or ‘willbes’, specified by `f`.

## 4. Implementation 1: update over

To start, let’s use q’s native parsing primitive to analyze the structure of the successful update from section 2:

```
q)parse"update l:k*2 from update k:f+g from t"
!
(!;`t;();0b;(,`k)!,(+;`f;`g))
()
0b
(,`l)!,(*;`k;2)
```

The functional form of `update` is:

```
![t;a;b;c]
```

where `t` is the table to be updated, `a` is the constraint, or ‘where’ clause, `b` is the group, or ‘by’ clause, and `c` is the dictionary of names and definitions of the columns to be added.

In the problem at hand we have no constraints and no grouping, so by convention `a` and `b` are `()` and `0b`.

The column definitions are unit dictionaries. Each one gives the name and definition of a single field. A unit dictionary maps a one-element symbolic vector – the name – to a one-element parse of the definition. So, for example:

```
q)enlist[`k]!enlist parse"f+g"
k| + `f `g
```

The form of the successful update is:

```
![![t;();0b;kdict];();0b;ldict]
```

where `kdict` and `ldict` are unit-dictionaries which define `k` and `l` respectively. Generalising, we see that in order to add columns `c1`…`cn` to `t` we have to construct an expression of the form:

```
![..![t;();0b;c1]..;();0b;cn]
```

Let’s simplify this by defining a function which eliminates the constants:

```
col:{![x;();0b;enlist[y]!enlist z]}
```

`col` takes three arguments: `x` is a table, `y` is a symbol, and `z` is the parse of a definition. `col` returns `x` updated with the new column `y`. So:

```
col[..col[t;`c1;def1]..;`cn;defn]
```

We know what to do with this pattern: express it as the application of `col` over `t`, a vector of names, and a list of definitions:

```
col/[t;`c1..`cn;(def1;..;defn)]
```

`col` executes n times. Initially, `t` is updated with `c1` to produce `t1`. Then `t1` is updated with `c2` to produce `t2`.

```
q)col/[t;`k`l;parse each("f+g";"k*2")]
f  g  k   l
-------------
20 50 70  140
30 60 90  180
40 70 110 220
```

## 5. Implementation 2: column references

q still requires that we order the definitions correctly:

```
q)col/[t;`l`k;parse each("k*2";"f+g")]
{![x;();0b;enlist[y]!enlist z]}
'k
```

Let’s start by having `f`, our dictionary of column definitions:

```
q)f:`l`k!("k*2";"f+g")
q)f
l| "k*2"
k| "f+g"
```

Then parse each expression:

```
q)p:parse each f
q)p
l| * `k 2
k| + `f `g
```

We want to order `p` by column-reference, but first we have to extract these from each parsed definition. The algorithm is recursive: descend the parse tree looking for symbol atoms:

```
ref:{$[-11=t:type x;x;t;();.z.s each x]}
```

We read the conditional `$[..]` as follows:

- if `x` is a symbol, return `x`
- else if `x` is not a list, return `()`
- else `self each x`

For example:

```
q)ref p
l| () `k ()
k| () `f `g
```

Since each recursion adds a level of nesting, we need to flatten the result. The q primitive *raze* (K: `,/`) takes a list of sublists and returns the catenation of the sublists. The raze of `x` reduces one level of nesting. And since references can occur more than once in an expression, we need to compress out duplicates with `distinct`.

Since the result of `ref` is a tree it will often contain more than one level of nesting. For example:

```
q)ref parse "(a+b)*c-a*b"
()
(();`a;`b)
(();`c;(();`a;`b))
```

But `distinct raze` reduces just one level:

```
q)distinct raze ref parse "(a+b)*(a*b)-c"
()
`a
`b
(();`a;`b)
`c
```

To flatten a list of arbitrary depth, we keep applying `raze` until the result converges. That is, until the result cannot be any flatter:

```
flatten:distinct raze over

q)flatten ref parse "(a+b)*(a*b)-c"
`a`b`c
```

So the final form of our function for extracting references from a parse-tree is:

```
refs:flatten ref@
```

Thus:

```
q)refs each p
l| ,`k
k| `f`g
```

## 6. Implementation 3: column order

Now that we have the references of `p` just one piece remains: re-order `p` so we can add columns to `t` in correct order.

Let’s define f as follows:

```
q)f:`h`j`k!("j+k";"f+g";"j*100")
q)f
h| "j+k"
j| "f+g"
k| "j*100"
```

```
q)p:parse each f
q)p
h| + `j `k
j| + `f `g
k| * `j 100
```

```
q)r:refs each p
q)r
h| `j`k
j| `f`g
k| ,`j
```

```
q)k:key r
q)k
`h`j`k
```

Now if we index `r` by `k`:

```
q)r k
`j`k
`f`g
,`j
```

we get the references of each definition. Notice that some symbols in the result are indices of `r` (`` `h`j`k ``) and some are not (`` `f`g ``). Indexing `r` by one of the latter returns an empty list:

```
q)r r k
(`f`g;,`j)
(`symbol$();`symbol$())
,`f`g
```

```
q)r r r k
((`symbol$();`symbol$());,`f`g)
(();())
,(`symbol$();`symbol$())
```

In this process, we are drilling down in parallel to the ultimate constituents of each definition in `r`. Eventually, we bottom out in a nest of empties, since the ultimate constituents (`f` and `g`) are columns of `t`:

```
q)r over k
((();());,(();()))
(();())
,(();())
```

To capture the sequence of intermediates, we use `scan`:

```
q)r scan k
h                                  j                       k
`j`k                               `f`g                    ,`j
(`f`g;,`j)                         (`symbol$();`symbol$()) ,`f`g
((`symbol$();`symbol$());,`f`g)    (();())                 ,(`symbol$();`symbol$())
((();());,(`symbol$();`symbol$())) (();())                 ,(();())
((();());,(();()))                 (();())                 ,(();())
```

Reversing the result, we get the constituent analysis of `f` in calculation order:

```
q)reverse r scan k
((();());,(();()))                 (();())                 ,(();())
((();());,(`symbol$();`symbol$())) (();())                 ,(();())
((`symbol$();`symbol$());,`f`g)    (();())                 ,(`symbol$();`symbol$())
(`f`g;,`j)                         (`symbol$();`symbol$()) ,`f`g
`j`k                               `f`g                    ,`j
h                                  j                       k
```

The nesting and the empties are irrelevant, so we flatten the analysis:

```
q)flatten reverse r scan k
`f`g`j`k`h
```

This gives us a valid calculation order: the ultimate constituents first (in no particular order), followed by `j (f+g)`, `k (j*100)`, and `h (j+k)`.

For complex `f`, `flatten` over `reverse r scan key r` produces ever-larger and more complex intermediates. By moving the flattening operation into the scan loop we keep the intermediate results as simple as possible, thereby reducing the complexity of the indexing:

```
q)(flatten r@)scan k
`h`j`k
`j`k`f`g
`f`g`j
`f`g
`symbol$()
()
```

```
q)flatten reverse(flatten r@)scan k
`f`g`j`k`h
```

Finally, we want to exclude non-calculated constituents from the result:

```
q)flatten[reverse(flatten r@)scan k]inter key k
`j`k`h
```

So our final function is:

```
order:{flatten[reverse(flatten x@)scan key x]inter key x}
```

## 7. Synthesis

Putting it all together:

```
willbe:{[t;f]
 p:parse each f;        / parse of expression
 r:refs each p;         / references
 o:order r;             / ordered by reference
 col/[t;o;p o]}         / create view
```

```
flatten:distinct raze over
ref:{$[-11=t:type x;x;t;();.z.s each x]}
refs:flatten ref@
col:{![x;();0b;enlist[y]!enlist z]}
order:{flatten[reverse(flatten x@)scan key x]inter key x}
```

Let’s run through an example:

```
t:([]f:10 20 30;g:40 50 60)
f:`h`j`k!("j+k";"f+g";"j*100")
v::willbe[t;f]
```

```
q)t
f  g
-----
10 40
20 50
30 60
```

```
q)v
f  g  j  k    h
------------------
10 40 50 5000 5050
20 50 70 7000 7070
30 60 90 9000 9090
```

```
q)t:update g:g+1 from t where f<50
q)t
f  g
-----
10 41
20 51
30 61
```

```
q)v
f  g  j  k    h
------------------
10 41 51 5100 5151
20 51 71 7100 7171
30 61 91 9100 9191
```

## 8. Partitioned calculations

Our implementation allows us to define new columns by applying their definitions to existing columns as wholes. For example, `h` is all of `j` plus all of `k`. Suppose we add a grouping column `e` to `t`:

```
q)t:([]e:1 1 2;f:10 20 30;g:40 50 60)
q)t
e f  g
-------
1 10 41
1 20 51
2 30 61
```

Then we may want to define a new column whose values are computed for each `e`-partition of `t`: for the subtable where `e=1` and the subtable where `e=2`.

Notice that our definition of `col` supplies the constant `0b` to the third position of `!`, so new columns are always computed on the ungrouped input table `x`:

```
col:{![x;();0b;enlist[y]!enlist z]}
```

Let’s add a parameter to `willbe` which controls grouping:

```
q)f:`h`j`k`l!(’j+k";"f+g";"j*100";"k%sum k")
q)g:`h`j`k`l!(0b;0b;0b;enlist[`e]!enlist`e)
```

The new parameter `g` is a dictionary of ‘group by’ clauses, corresponding to the definitions of the calculated columns `f`.

We revise our suite of functions accordingly:

```
willbe:{[t;f;g]
 p:parse each f;        / parse of expression
 r:refs each p;         / references
 o:order r;             / ordered by reference
 col/[t;g o;o map'p o]} / create view
flatten:distinct raze over
ref:{$[-11=t:type x;x;t;();.z.s each x]}
refs:flatten ref@
map:{enlist[x]!enlist y}
col:![;();;]
order:{flatten[reverse(flatten x@)scan key x]inter key x}
```

So that:

```
q)t:([]e:1 1 2;f:10 20 30;g:40 50 60)
q)f:`h`j`k`l!("j+k";"f+g";"j*100";"k%sum k")
q)g:`h`j`k`l!(0b;0b;0b;enlist[`e]!enlist`e)
q)v::willbe[t;f;g]
```

```
q)v
e f  g  j  k    h    l
------------------------------
1 10 40 50 5000 5050 0.4166667
1 20 50 70 7000 7070 0.5833333
2 30 60 90 9000 9090 1
```

You can find this source code at [www.nsl.com/q/willbe.q](http://www.nsl.com/q/willbe.q)

## Acknowledgements

Thanks to Attila Vrabecz and Michael Rosenberg for corrections and improvements.
