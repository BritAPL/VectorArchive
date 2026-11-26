---
vid: '10500290'
title: A genetic algorithm primer in APL
authors:
- Romilly Cocking
byline: by Romilly Cocking (romilly@cocking.co.uk)
volume: '24'
issue: '2'
page: '135'
description: Article submitted to vector.org.uk
abstract: Abstract This primer shows how you can implement and experiment with genetic algorithms simply and efficiently in APL.
source: content/printed/242/cocking/cocking.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- programming techniques
- expert systems and AI
---

by Romilly Cocking  
(romilly@cocking.co.uk)
{ .byline }

**Abstract** This primer shows how you can implement and experiment with genetic algorithms simply and efficiently in APL.
{ .abstract }

## Introduction

A genetic algorithm (GA) is a particular process that searches for a good solution to an optimisation problem. GAs use a technique that is based on Darwinian Natural Selection. They work well on problems that are too complex for an exact (analytic) solution, and are used in engineering and finance.

This primer shows how you can implement and experiment with GAs simply and efficiently in APL.

First, a disclaimer. GAs are very useful, but they are just one member of a large family of optimisation techniques. For example, optimisation problems can sometimes be solved by ‘hill-climbing’. This process repeatedly improves approximate solutions by varying them in the direction which ‘goes up-hill’.

Hill-climbing is simple to implement but it can easily become trapped by local optima. If you try to find high ground by always turning up-hill, you may get stuck on top of a low hill and miss a mountain.

If the function that you want to optimise is complex and has many hills, hill-climbing will not work well. In this case your best option may be to use a GA.

Genetic algorithms were introduced in a book by John Holland[\[1\]](#ref1). GAs are widely used and well-documented in the literature. Since GAs are inspired by biological systems they are described in biological terms.

GAs are based on the idea of reproducing populations. Throughout this paper, we will ignore sex, on the grounds that it only serves to make life more complicated. Instead we will assume that reproduction is asexual, as it is in bacteria.

Here are some simplified biological definitions (extended and adapted from Wikipedia):

*Chromosomes* are organized structures of DNA found in cells. A chromosome is a singular piece of DNA.

A *gene* is a locatable region of a chromosome that corresponds to a unit of inheritance. A chromosome contains many genes. One gene might determine your hair colour, while another might determine the shape of your ear lobe.

An *allele* is one member of two or more different forms of a gene. Thus the hair colour gene might have alleles for fair hair, brown hair, red hair and so forth.

A *locus* (plural *loci*) is a fixed position on a chromosome which defines the position of a gene.

We shall use the (non-standard) term *locus* set to mean the set of alleles that can exist at a particular locus.

## Representation

We can represent the full range of genetic possibilities for an organism by listing the locus sets. If an organism is characterised by hair colour, eye colour and height we might set

```
      localSets←('short' 'tall')('fair' 'brown' 'red')
                              …('blue' 'green' 'brown' 'blue-green')
```

and evaluate

```
      ⍉↑localSets
short  fair   blue
tall   brown  green
       red    brown
              blue-green
```

While this notation is easy to read, it is fiddly to implement. Most GAs do not use names to represent alleles. In this primer we will use integers to represent alleles; we will represent an allele by its origin - index within its locus set. (Note to purists: sets have no order, but we will represent locus sets by vectors, which do.)

In this notation, we can specify the chromosome of a tall individual with fair hair and brown eyes by the vector `2 1 3`. We can represent a population of individuals by a matrix. Each row then represents the chromosome of an individual in the population.

Since we are using integers to represent alleles, we can easily generate a random population. We just need to know how many individuals it should contain and the size of each locus set.

```
      5 populate sizes ← 2 3 4
1 2 4
2 3 4
1 3 4
2 2 4
1 3 1
```

In Dyalog APL `populate`  is a trivial dfn: `populate←{?⍵⍴⍨⍺,⍴⍵}`

## Scoring

Recall that a genetic algorithm is an optimisation technique. To implement a GA we need to know how to calculate the value we are trying to optimise. In other words we need a fitness function which we can apply to an individual or a population to find out how good a solution each individual represents.

In our case, we might envisage a ‘Mr/Ms World’ competition where each (sexless) participant gets a score based on their height, their hair colour and the colour of their eyes.

We’ll assume that the judges have given us a scoring function which represents the value to be optimised. I’ve chosen a trivial evaluation function; it is just the sum of the allele indices. The winner is thus a tall redhead with blue-green eyes. Interesting real world problems have so many solutions that exhaustive search is impractical. Let’s pretend that is true for our example.

Of course, the definition of the evaluation function is trivial. It is

```
      fitness←+/
```

Let’s do a little exploratory programming. First we’ll generate a population

```
      pop ← 6 populate sizes
      pop
1 1 3
2 1 2
1 3 2
2 2 1
1 2 1
1 2 1
```

How fit are the individuals in the population?

```
      fitness pop
5 5 6 5 4 4
```

## Breeding

Now we breed the next generation by combining three biologically inspired processes – selection, recombination and mutation.

### Selection

Selection takes the result of our evaluation function and uses it to favour the fitter (more valuable) members of the population. Fitter members get a higher chance of being selected for breeding.

Let’s see what happens when we apply selection. First we’ll generate a larger population, so that we have a good mix of individual types.

```
      pop ← 10 populate sizes
      pop
1 1 4
1 1 1
2 3 1
2 3 1
2 2 1
1 2 1
1 1 3
2 2 4
1 2 2
1 1 3
      fitness pop
6 3 6 6 5 4 5 8 5 5
```

The highest possible fitness is actually 9 (corresponding to the gene 2 3 4); our initial population includes a good individual with a score of 8, but no one who is perfect.

Let’s apply selection:

```
      ng ← select pop
      ng
2 3 1
1 1 4
1 1 3
1 1 3
2 2 4
2 3 1
1 2 1
1 2 2
2 3 1
2 3 1
```

I’ve defined `select` so that it uses the terms of the domain. Here it is with the utilities which it uses:

```
      select←{(size ⍵) cull sort ⍵ copied(size ⍵)×normalise fitness ⍵}
      size←{1↑⍴⍵}
      cull←{(⍺,1↓⍴⍵)↑⍵}
      sort←{⍵[⍒fitness ⍵;]}
      copied←{⍺⌿⍨⌈⍵}
      normalise←{⍵÷+/⍵}
```

What effect did selection have on the fitness of the next generation? Let’s use an `average` function to find out:

```
      average←{(+/⍵)÷⍴⍵}
      average fitness pop
5.3
      average fitness select pop
6.2
```

As we might hope, selection has increased the fitness of the population.

### Recombination

Selection is pretty effective, but not enough on its own. In our example it will increase the number of good individuals, but it won’t invent ones that are any better than the best we’ve currently got. We can improve things if we follow selection by recombination.

In recombination, we mate pairs of individuals and mix up their chromosomes. More specifically, we take a pair of chromosomes and cross them as shown below:

before:

```
1111
2222
```

after crossing over at the third locus:

```
1112
2221
```

If we consider longer chromosomes, we might have more than one-crossover.

before:

```
1111111111111111111111111111111111111
2222222222222222222222222222222222222
```

after a double cross-over:

```
1111111222222222222221111111111111111
2222222111111111111112222222222222222
```

To recombine, we need to specify a recombination probability as well as a population.

```
      0.3 recombine ng
2 3 1
1 2 1
1 1 1
2 3 1
2 1 4
2 2 1
1 1 4
1 1 3
2 3 1
1 3 1
```

If you compare this with `ng`, you will see that it has affected most of the individuals but not all. That’s because we specified a recombination probability of 0.3, which is the probability of recombination at any locus; so the probability of at least one recombination is just over 0.75.

Here’s the definition of `recombine` and its sub-functions:

```
      recombine←{(⍴⍵)⍴pairs⊖⍨parity ⍺ of 1↓⍴pairs←mate ⍵}
      mate←{(⌈1 2 1÷⍨2,⍴⍵)⍴⍵}
      of←{1000≥?⍵⍴(⌈1000÷⍺)}
      parity←≠\
```

### Mutation

Recombination and selection are pretty powerful, but there is one trick left up our sleeve: mutation. We may be unlucky; it might happen that a particular allele does not occur in any of the individuals in our starting population. In the GA implementation that we’ve chosen, neither selection nor recombination will ever generate a new allele. If the allele that’s missing from our starting population is required for the optimal solution, we will never get there.

We can be rescued by mutation. Mutation replaces alleles at random; if we evolve our population for long enough, each possible allele will be injected. It may take a while but we’re no longer trapped in some non-optimal subset of the search space.

Here’s the definition of `mutate`, which needs to know the probability of mutation, the size of each locus set, and the population to mutate.

```
      mutate←{
        (sizes pop)←⍵
        i←i/⍳⍴i←⍺ of×/⍴pop
        v←,pop
        v[i]←?(⍴i)⍴sizes[1+(⍴sizes)|i-1]
        (⍴pop)⍴v
      }
```

We can combine all these into a single breeding function, and define a `generate` function which applies it repeatedly.

```
      breed←{0.3 recombine 0.001 mutate ⍺(select ⍵)}
      generate←{((locusSizes∘breed)⍣⍵)⍺}
```

Let’s evolve for two generations:

```
      pop generate 2
2 2 4
2 2 4
2 3 4
2 3 4
2 2 4
2 3 4
1 3 4
1 2 4
2 3 1
2 3 1
```

We’ve now got several copies of the optimal solution. Of course the algorithm doesn’t know it’s optimal, just that it’s the best we’ve seen so far.

Let’s run two, three and four generations checking the average fitness.

```
      average fitness pop generate 2
7.2
      average fitness pop generate 3
8.6
      average fitness pop generate 4
9
```

By the fourth generation the whole population is made up of the optimal solution.

Genetic algorithms work well. That’s not surprising. The blind watchmaker[\[2\]](#ref2) has been using them with excellent results for quite a while.

<a id="references"></a>

## References

1. <a id="ref1"></a> Holland, John H. *Adaptation in Natural and Artificial Systems* 1975
2. <a id="ref2"></a> Dawkins, Richard *The Blind Watchmaker* 1986
