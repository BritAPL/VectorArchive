---
vid: '10005860'
title: 'J: A First Lesson'
authors:
- Donald McIntyre
byline: by Donald B. McIntyre
volume: '10'
issue: '4'
page: '18'
source: trad/v104/mcintyre104_18.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- language design
- education
---

by Donald B. McIntyre <span class="affil"></span> <span class="email"></span>
{ .byline }

It is not surprising if on your first encounter with J you should think it difficult; this is true of most new things. As A.N. Whitehead said: “Of course nothing is more incomprehensible than a symbolism we do not understand” \[1\]. The Editor of The Education Vector says: “I find J very difficult. ... Even Donald McIntyre admitted to coming close to despair before he could write J expressions which did what he wanted” \[2\]. It is true that I met formidable difficulties; but then I was one of J’s first guinea pigs (August 1990) and had few examples to follow. Nearly four years later things are different: bugs are eliminated; new features and modified old ones make J easier; and, thanks in great part to Vector, the literature on J is very much expanded.

The Editor went on to say that “the articles in Vector are imperfect as the early ones were written in early versions of J which have been superseded”. That is not wholly true; at J workshops at APL93 I distributed disks \[3\] bringing all eight of my papers on J up to date through Version 6.2. The script files also execute in Version 7, though not at this time taking advantage of V7’s new features. The disk includes other scripts used in my J workshops. Together they constitute a book-length manual supplementing the publications of Iverson Software, Inc. Because the scripts are extensively annotated, they execute as dynamic tutorials. All necessary changes from the published texts are explained, and my paper on Amendment \[4\] explains the last major changes to J syntax.

No reader of Education Vector should think that undue difficulty is any longer a reason for putting off the study of J. Even more than APL, this is an executable mathematical notation of great elegance and power. You will enjoy it.

Let’s take a concrete example: the computation of a similarity table, or the classification of objects by how close they are in a space spanned by their measured properties. This is the basis of Cluster Analysis and is Herbert Hellerman’s first example in his classic texts of 1967 (using Iverson Notation) and 1973 (using executable APL) \[5\]. His data are in a table:

```
1 4 3 1
2 1 3 5
1 1 4 1
3 8 2 4
2 3 3 2
0 2 4 2
```

Each row represents an item; e.g., a student, a patient, or some object. The columns represent measured properties; e.g., the results of tests. In this case six items are interpreted as points in 4-dimensional space, and the geometrical distance between two points is taken as a measure of similarity between two items. Following Pythagoras, the square of the distance between two points is obtained by taking the sum of the squares of the differences between corresponding co-ordinates.

In J we begin by entering the data by linking the rows (with ;), which gives a string of six boxes. When these boxes are opened (by >) the result is the required table, which can be thought of as a collective noun. It can be given a name (using =.) for later reference, and it is then displayed simply by entering the name. As you read, enter what follows in bold type into J (Version 7). What you enter will be indented and J’s response will be flushed left.

```
  1 4 3 1; 2 1 3 5; 1 1 4 1; 3 8 2 4; 2 3 3 2; 0 2 4 2
┌───────┬───────┬───────┬───────┬───────┬───────┐
│1 4 3 1│2 1 3 5│1 1 4 1│3 8 2 4│2 3 3 2│0 2 4 2│
└───────┴───────┴───────┴───────┴───────┴───────┘
hh=. >1 4 3 1;2 1 3 5;1 1 4 1;3 8 2 4;2 3 3 2;0 2 4 2
```

Space does not allow us to give Hellerman’s distance functions in both Iverson notation and APL, or mine using APL’s Direct Definition, but here are three alternative versions in J. Spaces are for readability only; none is necessary.

```
   DSQE=. '+/"1 *: -"1/~ y.':''
   dsq=. [: +/"1 [: *: -"1/~
   dsqt=. +/"1@*:@(-"1/~)
```

The distance-squared matrix for Hellerman’s data is:

```
      dsq hh
 0 26 10 30  3  7
26  0 18 52 13 15
10 18  0 66  7  3
30 52 66  0 31 53
 3 13  7 31  0  6
 7 15  3 53  6  0
```

To those not already familiar with J these expressions will appear strange and perhaps even forbidding. My object is to help dispell the strangeness by showing how to read and write this much J.

We can consider the expression 2+3 to be a sentence in which 2 and 3 are nouns and + is a verb (function) that operates on its noun arguments to give a noun result. Each of the verbs DSQE, dsq, and dsqt operate on the noun hh to give the similarity table. Each says: sum the squares of the differences. If you can guess that +/"1 is sums, \*: is squares, and -"1/~ is differences, then the symbolism is already less obscure.

DSQE is an explicit definition of the distance-squared verb in which y. is the explicit place-holder of the argument. The definition for the monadic case (one argument) is in quotes. It is followed by a colon and an empty string. Had a dyadic case been needed, its definition would have replaced the empty string and x. would have been the place-holder for the left argument.

dsq and dsqt are tacit definitions, with no explicit reference to arguments.

Before addressing these directly we need to consider some fundamentals. The sentence 2+3 includes two kinds of objects; namely, a dyadic verb (+) and its two noun arguments (2 and 3). Verbs like + apply also to lists and tables; i.e., to collective nouns:

```
      0 1 2 3 + 3 2 1 0
3 3 3 3
```

Atoms (single numbers) are extended for compatibility. Here are examples of the dyads minus, times, divide, power, and the monads double, halve, square, and square root:

```
      0 1 2 3 - 2
_2 _1 0 1
      0 1 2 3 * 2
0 2 4 6
      0 1 2 3 % 2
0 0.5 1 1.5
      0 1 2 3 ^ 2
0 1 4 9
      +: 0 1 2 3
0 2 4 6
      -: 0 1 2 3
0 0.5 1 1.5
      *: 0 1 2 3
0 1 4 9
      %: 0 1 4 9
0 1 2 3
```

J uses ASCII symbols, either alone or as digraphs consisting of an ASCII character followed immediately by either a period (.) or a colon (:). Negative numbers are preceded with an underbar, and numbers less than 1 are preceded by zero as well as a decimal point.

An adverb modifies the verb to its left; e.g. the passive adverb (~) commutes or interchanges the arguments:

```
      0 1 2 3 ^~ 2
1 2 4 8
```

A monadic verb has a single argument, to its right:

```
      - 0 1 2 3
0 _1 _2 _3
```

The reflexive adverb (~) supplies a copy of the right argument as the verb’s left argument:

```
      +~ 0 1 2 3
0 2 4 6
```

The verb i. generates lists and tables of integers starting at zero, and the tally is the number of items.

```
      y=. i.4
      y
0 1 2 3
      #y
4
```

The insert adverb (/) inserts its verb between the items:

```
      0+1+2+3
6
      +/0 1 2 3
6
```

Verbs, like nouns, can be named and displayed:

```
      sum=. +/
      sum
┌─┬─┐
│+│/│
└─┴─┘
      sum y
6
      (sum % #) y
1.5
```

Because parentheses control the order of execution, the enclosed expression must first be evaluated. It is an entity – a train of three verbs defining a new (in this case, unnamed) verb. How to interpret it is not difficult to guess if you know that the arithmetic mean is defined as the sum divided-by the number of items. Name it for future use:

```
      mean=. sum % #
      mean y
1.5
```

It is essential to note that a very different result is obtained if we enter the expression for immediate execution:

```
      sum % # y
0.25
```

The reason is that no new verb is produced. Three monadic verbs are applied in succession and the result is the sum of the reciprocal of the tally of y, which is of no obvious use.

Display the boxed representation, or use the fix adverb (f.) to display the verb in terms of J primitives:

```
      mean
┌───┬─┬─┐
│sum│%│#│
└───┴─┴─┘
      mean f.
┌─────┬─┬─┐
│┌─┬─┐│%│#│
││+│/││ │ │
│└─┴─┘│ │ │
└─────┴─┴─┘
```

Define the verb tree (its explanation is beyond the scope of this first lesson) and use it for a tree display:

```
     tree=. 5!:4@<
     tree 'mean'
  ┌─ sum
──┼─ %
  └─ #
```

The display shows why trains of 3 elements are called tridents. A train of 3 verbs is a special case, called a fork. If f, g, h are verbs and x and y are nouns, this is how J interprets (f g h):

```
    (f g h) y  <-->    (f y) g   (h y)    Monadic fork
  x (f g h) y  <-->  (x f y) g (x h y)    Dyadic fork
```

Note that the root verb (here g) is always dyadic.

J has other ways of building tables:

```
      0 1 2 +/ 0 1 2 3
0 1 2 3
1 2 3 4
2 3 4 5
```

A train of adverbs defines an adverb

```
      +/~ 0 1 2 3
0 1 2 3
1 2 3 4
2 3 4 5
3 4 5 6
      ]y3=. i.2 3 4
 0  1  2  3
 4  5  6  7
 8  9 10 11
12 13 14 15
16 17 18 19
20 21 22 23
```

Right (\]) is a verb that returns its (right) argument. It is used here as a convenient way of displaying a named noun. When working with collective nouns (multi-dimensional arrays) we need verbs that give the shape ($), the tally or number of Items (#), and the rank (the tally of the shape):

```
      $y3
2 3 4
      #y3
2
      #$y
3
```

The verb I call rank combines two verbs with the atop (@) conjunction:

```
      rank=. #@$
      rank y3
3
```

The verb head (\{.) gives the first item:

```
      ]y2=. {. y3
0 1  2  3
4 5  6  7
8 9 10 11
      rank y2
2
      ]y1=. {. y2
0 1 2 3
      rank y1
1
```

Because Sum and tally give the sums over the items and the number of items, the mean of y3 is a 3 by 4 table of means across the two 3 by 4 tables. If y3 is a budget report covering 2 years with 3 line-items and 4 quarters; then the means over two years (3 line-items and 4 quarters) are:

```
      mean y3
 6  7  8  9
10 11 12 13
14 15 16 17
```

Without changing the definition of mean, we can use the rank conjunction (") to apply mean to the rank-2 cells; i.e., over the 3 line-items for each of the 2 years and 4 quarters (down the columns):

```
      mean"2 y3
 4  5  6  7
16 17 18 19
```

Similarly the means of the rank-1 cells are over the 4 quarters for each of the 2 years and 3 line items (along the rows)

```
      mean"1 y3
 1.5  5.5  9.5
13.5 17.5 21.5
```

Use a fork to catenate the means to the report:

```
      f=. ] , mean
      f y3
 0  1  2  3
 4  5  6  7
 8  9 10 11
12 13 14 15
16 17 18 19
20 21 22 23
 6  7  8  9
10 11 12 13
14 15 16 17
```

Apply this to the rank-2 cells:

```
      f"2 y3
 0  1  2  3
 4  5  6  7
 8  9 10 11
 4  5  6  7
12 13 14 15
16 17 18 19
20 21 22 23
16 17 18 19
```

Apply it to rank-1 cells:

```
      f"1 y3
 0  1  2  3  1.5
 4  5  6  7  5.5
 8  9 10 11  9.5
12 13 14 15 13.5
16 17 18 19 17.5
20 21 22 23 21.5
```

We can reproduce these results by a hook, i.e. a train of 2 verbs. A hook modifies a set of data by a function of the data (the monadic case), or it modifies one set of data by a function of another set:

```
        (g h) y  <-->  y g (h y)
      x (g h) y  <-->  x g (h y)
```

Note that g is always dyadic and h is always monadic. The required hook is:

```
      h=. , mean
```

Using match (-:),a fork can be used to show that f and h give the same result; i.e., execute (f y3) -: (h y3)

```
      (f -: h) y3
1
      (f -: h)"2 y3
1 1
      (f -: h)"1 y3
1 1 1
1 1 1
```

Although the arguments of minus must agree, compatibility is not necessary for difference tables:

```
      2 4 -/ 1 2 3
1 0 _1
3 2  1
```

The first step in creating a similarity table is applying minus to the rows (rank-1 cells). Parentheses are used here for readability but are not needed for execution.

```
      1 2 (-"1) i.3 2
 1  1
_1 _1
_3 _3
```

The rank conjunction bonds minus and 1, creating a new verb that applies to the rank-1 cells. Test this with a simple set of data; 4 points in 2-space, the columns being the x and y co-ordinates:

```
      ]y=. 1 1 2 3 ,. 1 2 1 3
1 1
1 2
2 1
3 3
      -"1/~ y
 0  0
 0 _1
_1  0
_2 _2
  ...
 2  2
 2  1
 1  2
 0  0
```

Now use a fork to square (\*:) these differences. Because cap (\[:) caps a branch of a fork, (\[: g h) makes g monadic.

```
      ([: *: -"1/~) y
0 0
0 1
1 0
4 4
...
4 4
4 1
1 4
0 0
```

The squares of the distances are the sums of the squares of the differences. This is true also in multi-dimensional space. Take the sum over the rank-1 cells (rows), using cap to complete another fork. The result can be checked by hand.

```
      dsq=. [: +/"1 [: *: -"1/~
      dsq y
0 1 1 8
1 0 2 5
1 2 0 5
8 5 5 0
```

To read the definition of dsq, first identify the classes of the elements: there are four verbs (\{: + \*: -); two adverbs (/ ~); and one conjunction.

The parsing rules specify that: (a) adverbs and conjunctions are executed before verbs; (b) an adverb modifies the verb (or verb phrase) to its left; (c) and a conjunction has long left-scope and short right-scope. Consequently the parser sees the elements grouped as a train of 5 verbs:

```
      [: ((+/)"1) [: *: ((-"1)/~)
┌──┬───────────┬───────────────────────┐
│[:│┌─────┬─┬─┐│┌──┬──┬───────────────┐│
│  ││┌─┬─┐│"│1│││[:│*:│┌───────────┬─┐││
│  │││+│/││ │ │││  │  ││┌───────┬─┐│~│││
│  ││└─┴─┘│ │ │││  │  │││┌─┬─┬─┐│/││ │││
│  │└─────┴─┴─┘││  │  ││││-│"│1││ ││ │││
│  │           ││  │  │││└─┴─┴─┘│ ││ │││
│  │           ││  │  ││└───────┴─┘│ │││
│  │           ││  │  │└───────────┴─┘││
│  │           │└──┴──┴───────────────┘│
└──┴───────────┴───────────────────────┘
```

dsq gives an identical display. The pattern is more obvious in a simpler case; if f g h i j are verbs:

```
      f g h i j
┌─┬─┬───────┐
│f│g│┌─┬─┬─┐│
│ │ ││h│i│j││
│ │ │└─┴─┴─┘│
└─┴─┴───────┘
```

The parser works from right to left. Successive elements from the tail-end of the queue (initially the input string) are moved to the front of a stack. Executions in the stack are confined to the first four elements, eligibility for execution being determined by reference to the parsing table. The executions in the stack are confined to the first four elements; i.e., it is sufficient to know that the four elements g h i j are verbs in order to decide that (h i j) is a fork. Had g been a conjunction it would have grabbed h and prevented it from participating in a fork with i and j. The J Dictionary must be consulted for full details of parsing and execution \[6\].

A train of 2 verbs is a hook and a train of 3 verbs is a fork, and meaning is assigned to longer trains by repeated resolution. Thus a train of 5 verbs is a fork whose right-hand tine is itself a fork. The tree display is another way of viewing the structure:

```
      tree 'dsq'
  ┌─ [:
  ?     ┌─ / ─── +
  ├─ " ─┴─ 1
──?
  ?     ┌─ [:
  ?     ├─ *:
  └─────?                 ┌─ -
        └─ ~ ─── / ─── " ─┴─ 1
```

Finally take the square root (%:) to determine distances between points. The result is then a train of 7 verbs:

```
      dist=. [: %: [: +/"1 [: *: -"1/~
```

An alternative formulation is:

```
      dist1=. %:@(+/"1@(*:@(-"1/~)))
```

The conjunction atop (@) bonds the whole verb phrase on its left to the first element to its right (which may be an expression in parentheses).

Unnecessary parentheses can be dropped without affecting the result:

```
      dist2=. %:@+/"1@*:@(-"1/~)
```

Though dist, dist1, and dist2 give the same results, the parser treats dist2 as if it were written:

```
      ((((%:@+)/)"1 )@*:)@((-"1)/~)
      dist2
┌─────────────────────────┬─┬───────────────┐
│┌──────────────────┬─┬──┐│@│┌───────────┬─┐│
││┌────────────┬─┬─┐│@│*:││ ││┌───────┬─┐│~││
│││┌────────┬─┐│"│1││ │  ││ │││┌─┬─┬─┐│/││ ││
││││┌──┬─┬─┐│/││ │ ││ │  ││ ││││-│"│1││ ││ ││
│││││%:│@│+││ ││ │ ││ │  ││ │││└─┴─┴─┘│ ││ ││
││││└──┴─┴─┘│ ││ │ ││ │  ││ ││└───────┴─┘│ ││
│││└────────┴─┘│ │ ││ │  ││ │└───────────┴─┘│
││└────────────┴─┴─┘│ │  ││ │               │
│└──────────────────┴─┴──┘│ │               │
└─────────────────────────┴─┴───────────────┘
```

Read the expression from left to right. Because @ has short right-scope we first have (%:@+). But the conjunction " has long left-scope and therefore takes the verb phrase ((%:@+)/) as its left argument; its right argument is 1. Everything to the left of the next @ is now a unified verb phrase, which is the left argument of @, while \*: is its right argument. Everything to the left of the last @ is its left argument, while the parenthesised expression (-"1/~) is its right argument. Lastly the verb -"1 is modified by two adverbs (/~). Parentheses prevent (-"1) being taken as the right argument of @ and the two adverbs being left to apply to the entire expression.

### Concluding Remarks

Omitting essential parentheses is a common error. My advice is: (a) use forks where possible; (b) scan from left to right looking carefully at the right argument of each conjunction (especially any atops) and remembering that conjunctions have short right-scope; (c) write fully parenthesised sentences and use boxed displays to make sure that the grouping is what you intend; (d) only then cut down the number of parentheses, always verifying that the result remains unchanged. With practice you will find that you are beginning to think like the parser! If you check all points in this paper by reference to Iverson’s J Introduction and Dictionary \[6\],it will become easier to use the Dictionary as the essential tool it is.

All J expressions in this paper execute with J Version 7.

### Bibliography:

1. A.N. Whitehead, An Introduction to Mathematics, Home University Library, New York and London (1911).
2. Alan Mayer, Editorial Comment, Vector, Vol.10, No.3 (January 1994)
3. Donald B. McIntyre, Disk with J script files and texts, APL93, Toronto (August 1993). Available from I-APL Ltd., 56 The Crescent, Milton, Weston-super-mare, Avon, BS22 8DU.
4. Donald B. McIntyre, Amendment: “A change for the better”, Vector, Vol.9, No.3 (January 1993)
5. Herbert Hellerman, Digital Computer System Principles, McGraw-Hill Book Company, New York (1967) p.55 and (1973) p.53.
6. Kenneth E. Iverson, J Introduction & Dictionary, Iverson Software Inc., Toronto, J Version 7 (1993)

### Appendix:

On a DOS based machine, load the J interpreter and execute the sentences:

```
     script=. 0!:2&<
     'session.log' script ''
```

The session log will be captured as an ASCII file that can be edited with any word processor. If desired the revised file can be read and executed as an input file Enter the name of the input file (in quotes) as the right argument of the verb script. The name session.log is not a privileged name and any name (and path) consistent with DOS is acceptable.

(webpage generated: 4 April 2006, 18:26)
