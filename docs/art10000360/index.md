---
vid: '10000360'
title: Functional Programming in Joy and K
authors:
- Stevan Apter
byline: Stevan Apter sa@nsl.com
volume: '21'
issue: '4'
page: '31'
source: trad/v214/joy214.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- language design
- APL in perspective
---

Stevan Apter <a href="mailto:sa@nsl.com?subject=Functional%20Programming%20in%20Joy%20and%20K">sa@nsl.com</a>
{ .byline }

## What is Joy?

Joy is a pure, concatenative, functional, scalar programming language.

Joy is *pure*  because it does not contain assignment.

Joy is *functional*  because computation consists of the evaluation of expressions.

Joy is *concatenative*  (and not applicative) because

- The elementary well-formed expressions of Joy are monadic functions of a nameless data stack.
- If X and Y are well-formed expressions, then the concatenation of X and Y is well-formed.
- If Z is the concatenation of X and Y, then the value of Z is the composition of the values of X and Y.

For a rigorous exposition of Joy, and for examples of its use, the reader should consult the FAQ, language reference manual, tutorial, and related materials on the [official Joy website](http://www.latrobe.edu.au/philosophy/phimvt/joy.html).

In this note to my interview with Joy’s inventor, Manfred von Thun, I describe [tcK](http://www.nsl.com/k/tck/), a tiny concatenative language modelled on Joy and written in K.

## A tiny concatenative K

tcK is a pure, concatenative, functional, *array*  programming language.

tcK is a “tiny” version of [cK](http://www.nsl.com/papers/ck.htm): syntax and display are untranslated K, and the interactive environment is the plain K console.

The *primitives*  of tcK are those of K: the twenty dyads

```
          ~  !  @  #  $  %  ^  &  *  -  _  =  +  |  :  ,  <  .  >  ?
```

and their monadic counterparts

```
          ~: !: @: #: $: %: ^: &: *: -: _: =: +: |: :: ,: <: .: >: ?:
```

The *atoms*  of tcK are those of K, minus lambdas (defined functions): integers, floats, characters, symbols, null, dictionaries, and lists.

Since tcK is concatenative, everything – primitives, atoms, and lists – is a monadic function of the nameless data stack. For example, the number 12 is a function which takes a data stack and returns it with 12 as the new top element. The “stack diagram” showing the action of the 12 function is:

```
          -> 12
```

\+ is a monadic function which takes a stack whose top two elements are X and Y and returns it with X and Y replaced by X+Y:

```
          X Y -> X+Y
```

Evaluation in tcK uses two stacks, implemented as K lists. The data stack

```
          (..;Z)
```

has Z as its top element. The program stack

```
          (X;..)
```

has X as its next element.

Since the program stack is a concatenation of monadic functions, it denotes a composition. For example,

```
          (2;+;*)
```

is the composition *times of add of 2 of*  the data stack. Applied to

```
          10 20 30 40 50
```

it returns

```
          10 20 30 2080
```

That is,

```
          10 20 30 (40 * 50 + 2)
```

Computation consists of evaluating the program stack on the data stack to obtain a new data stack.

## Quotations and Combinators

A program in tcK is a list, or in Joy-speak, a *quotation*. Quotations are monadic functions of the stack (everything is), so, applied to the data stack, it returns that stack with itself as the top element.

A *combinator*  is a function which expects one or more quotations on the data stack, and applies those quotations in a particular way to the remainder of the stack. Combinators resemble APL operators, or K adverbs.

The simplest combinator is i, which expects a quotation as the top item of the data stack. The action of this combinator is to evaluate the quotation on the remainder of the data stack:

```
          (10;20;30;40;50;(2;+;*);i)
        10 20 30 2080
```

## Recursive Combinators

Joy contains several combinators which abstract common patterns of recursion. One such is *linear recursion*, which expects four quotations I, T, E, and F on the data stack. The combinator evaluates the predicate I. If it leaves ‘true’ on the stack, it evaluates T, else it evaluates E, recurses, and then evaluates F. For example, in Joy the factorial function can be written:

```
          [0 =] [1 +] [dup -1 +] [*] linrec
```

and in tcK:

```
          ((0;=);(1;+);(dup;-1;+);,(*);linrec)
```

## Definitions

Joy allows us to create associations between a name and the contents of a quotation:

```
          sqr == dup *
```

The effect of the definition is to add the word 'sqr' to the Joy vocabulary. Note that == is *not*  assignment.

The tcK analogue of Joy definition is the function-definition function d. A tcK definition is a *projection*  of d onto a quotation:

```
          sqr:d[(dup;*)]
```

The result is a monadic function of the stack which can be used in subsequent evaluations as though it were a primitive of tcK.

## An implementation of tcK

The tcK evaluator E is the following dyadic function:

```
          E:{*(a .)/(x;y)}
```

E is applied to a data stack x and a program stack y. It calls (a .) repeatedly, initially on (x;y), and thereafter on the result of the previous application, until that result either matches (x;y) or is the same twice in a row.

For convenience, evaluation on the empty data stack is defined as the projection

```
          e:E[()]
```

For example,

```
          e(10;20;30;+;-)
         ,-40
```

The application function a is:

```
          a:{:[~#y;(x;y);(4:*y)_in 4 7;(f[x;*y];1_ y);(x,1#y;1_ y)]}
```

Again, x is the data stack and y the program stack. If y is empty – all program elements have been processed – then a returns (x;y), which causes E to terminate evaluation and return the final data stack. Otherwise, if the next program element \*y is a function or a symbol, f is called to compute the new data stack and \*y is dropped from the program stack. Otherwise, the next program element is appended to the data stack and dropped from the program stack.

The function-evaluation function f is:

```
          f:{:[(#k)>i:k?y;(v[i]_ x),,y . v[i]#x;y x]}
```

where x is the data stack and y is a single program element to evaluate. k is a list of the forty K primitives, and v is a vector of the corresponding valences, negated for convenience. For example, the dyadic k primitive of equality is k 32, and v 32 is -2.

If y is a K primitive, then the new data stack is constructed by dropping valence-of-y-many elements from the data stack, and appending the result of applying y to those elements.

If y is not a K primitive, then it is either a tcK primitive or a tcK definition.

The tcK primitives are monadic K functions which model the stack operators and combinators of Joy, and the adverbs of K. For example, the Joy operator dup which duplicates the top element of the data stack is written:

```
          dup:{x,-1#x}
```

and the K adverb over is written:

```
          over:{(-2_ x),,{{*e(y;z),x}[y]/x}.-2#x}
```

The vocabulary of Joy is quite large. Since the purpose of tcK is primarily pedagogical, I've implemented only those operators required by the demonstration problems:

```
          dup       X -> X X                  duplicate top of data stack
          cons      X [..] -> [X ..]          insert X at head of [..]
          swap      X Y -> Y X                swap top two items of data stack
```

```
          i         [P] -> P                  evaluate P
          linrec    [I] [T] [E] [F] ->        if I then T else: E, recurse, F
```

```
          right     X Y [F] -> X [F]/:Y       X F/:Y, X F each-right Y
          over      X [F] -> [F]/X            F/X, F over X
          converge  X [F] -> [F]/X            F/X, R:F X, then R:F R until R ~ X or R ~ previous R
```

## Three problems

### Transitive closure<sup><a href="#note1">1</a></sup>

A K implementation of the classical ‘or . and’ APL solution:

```
          tc:{x|x(|/&)/:x}/
```

tc is the converge of the monadic function

```
          {x|x(|/&)/:x}
```

the “noun-verb-adverb”syntax of which is:

```
          nvn(vav)an
              –-
               v
```

That is, x is a noun, | and & are transitive verbs, / and /: are adverbs, and the expression (|/&) parses to a transitive verb. In keyword K, tc is:

```
          {x or x (or over and) right x} converge
```

We can easily implement tc in prefix form, where expressions involving adverbs are explicit projections of higher-order functions:

```
          or:|
          and:&
          over:{x/y}
          converge:{x/y}
          right:{y x/:z}
```

```
          tc:converge[{or[x]right[{over[or]and[x]y}][x]x}]
```

A concatenative language does not have variables. Instead, operators such as dup and swap are used to move items on the data stack into argument position:

```
          tc:d[((dup;dup;(&;,(|);over);right;|);converge)]
```

```
          e(3 3;0 0 0 1;#;tc)
        ,(0 0 0
          1 0 0
          1 1 0)
```

### Accumulator-Generator<sup><a href="#note2">2</a></sup>

Paul Graham [poses](http://www.paulgraham.com/accgen.html) the following problem:

> > *Write a function foo that takes a number n and returns a function that takes a number i, and returns n incremented by i.*

We cannot write foo in K, since K lambdas have no state. But tcK programs are lists, and lists have parts which *can*  be used to keep state:

```
          acc:d[((+;`acc);cons)]        / accumulator (recursive, must use `acc instead of acc)
          foo:d[(`acc`i;cons)]          / generator (can use acc and i instead of `acc and `i)
```

```
          e(3;foo)                      / generate a 3-accumulator
        ,(3;`acc;`i)
          e(3;foo;4;swap;i)             / and accumulate 4
        ,(7;+;`acc)
          e(3;foo;4;swap;i;5;swap;i)    / then accumulate 5
        ,(12;+;`acc)
```

### Quines<sup><a href="#note3">3</a></sup>

A *quine*  is a function which prints its own code.

The standard approach is to design a function which indirectly constructs a text-representation of its code. In K (and in many other languages) the ultimate constituents of text are *characters* . But the ultimate constituents of programs are *terms* , so we might expect that a language in which programs are directly available as lists of first-class terms would present the opportunity for a more direct solution.

In tcK, we can define the following quine:

```
          (`dup`cons;`dup;`cons)
```

Evaluation begins by pushing the program \`dup\`cons on the data stack:

```
          ,`dup`cons
```

Next, dup is evaluated, leaving two items on the stack:

```
          (`dup`cons;`dup`cons)
```

Finally, cons is evaluated, which inserts \`dup\`cons at the head of the list \`dup\`cons, leaving

```
          ,(`dup`cons;`dup;`cons)
```

which is the original program.

***

<sup><a name="note1"></a>1</sup> Adapted from code posted by Greg Heil on the K mailing list.  
<sup><a name="note2"></a>2</sup> Joy solution by Martin Young.  
<sup><a name="note3"></a>3</sup> Joy solution by Manfred von Thun

***

## [tck1.k](http://www.nsl.com/k/tck/tck1.k)

```
// tcK - 1 stack

/ verbs

k:,/+{.:'(x,":";x)}'"~!@#$%^&*_-+=|:<,>.?"                           / F1,F2 = (~:;!:;..;~;!;..)
v:-&0 20 20                                                          / valences

/ stack operators

dup:{x,-1#x}                                                         /      X -> X X
cons:{(-2_ x),,{(,x),y}.-2#x}                                        / X [..] -> [X ..]
swap:{(-2_ x),|-2#x}                                                 /    X Y -> Y X

/ combinators

i:{E[-1_ x;*-1#x]}                                                             / [..] -> ..
linrec:{{[x;i;t;e;f]:[E_[x;i];E[x;t];E[_f[E[x;e];i;t;e;f];f]]}[-4_ x].-4#x}    / t if i else: e, recurse, f

/ adverbs (k combinators)

right:{(-3_ x),,{x{*e(y;z),x}[z]/:y}.-3#x}                            / X Y f2 -> X f2/:Y
over:{(-2_ x),,{{*e(y;z),x}[y]/x}.-2#x}                               /   X f2 ->   f2/X
converge:{(-2_ x),,{{*e(,y),x}[y]/x}.-2#x}                            /   X f1 ->   f1/X

/ apply

a:{:[~#y;(x;y);(4:*y)_in 4 7;(f[x;*y];1_ y);(x,1#y;1_ y)]}            / apply if program-stack not empty
f:{:[(#k)>i:k?y;(v[i]_ x),,y . v[i]#x;y x]}                           / apply k or tck0 program

/ eval

E:{*(a .)/(x;y)}                                                      / evaluate y on x
E_:{*-1#E[x;y]}                                                       / last of evaluate y on x
e:E[()]                                                               / evaluate y on ()

/ trace

T:{(a .)\(x;y)}                                                       / trace evaluation of y on x
t:T[()]                                                               / trace evaluation of y on ()

/ define program P:d[(..)] (metalinguistic)

d:{E[y;x]}                                                            / evaluate x on y
```
