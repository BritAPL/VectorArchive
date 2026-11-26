---
vid: '10011360'
title: 'J-ottings 48: Perming and combing'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '22'
issue: '4'
page: '99'
source: trad/v224/jot224.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2006
tags:
- programming techniques
- mathematics and statistics
---

by Norman Thomson
{ .byline }

No, not an accidentally misdirected submission to *The Hair Stylist*, but rather an extension of one of Eugene McDonnell’s token reduction examples (see Vector vol. 22 no.3) which involves generating systematic lists of permutations, where a permutation of order n is to be understood as a list of integers i.n in some order, for example<span class="aplu"> 3 0 2 1 4 </span>.

## Permutation Lists

A permutation list is a list of lists in which all of the <span class="aplu">!n</span> possible permutations occur once and once only. The most interesting lists of this kind are those in which the ordering is in some way systematic, the most obvious being the lexical order which Eugene discusses, and which is readily obtainable using <span class="aplu">A. </span>(Anagram Index) as shown below. This ordering is also the result of Lehmer’s algorithm, so <span class="aplu">Llist</span> can be taken to denote either lexical or Lehmer.

```
   Llist=: i.@! A. i.           NB. Lehmer/lexical listing

   |:Llist 4
0 0 0 0 0 0 1 1 1 1 1 1 2 2 2 2 2 2 3 3 3 3 3 3
1 1 2 2 3 3 0 0 2 2 3 3 0 0 1 1 3 3 0 0 1 1 2 2
2 3 1 3 1 2 2 3 0 3 0 2 1 3 0 3 0 1 1 2 0 2 0 1
3 2 3 1 2 1 3 2 3 0 2 0 3 1 3 0 1 0 2 1 2 0 1 0
```

(Note: In many of the illustrations which follow, transposition appears as the leftmost verb in the J line for the purpose of compactness of display. In these cases individual permutations should thus be read as columns reading from top to bottom.)

## Factorial Digit Bases

Eugene defines a factorial digit base (that is, number base in the J sense) as:

```
   fdb=.}:@(>:@i.@-)          NB. factorial digit base
   fdb 4
4 3 2
```

Each digit from <span class="aplu">0</span> to <span class="aplu">n-1</span> has a unique representation in this base, as shown by:

```
   fact=.fdb#:i.@!
   |:fact 4                   NB. 0 to 23 as factorial digits
0 0 0 0 0 0 1 1 1 1 1 1 2 2 2 2 2 2 3 3 3 3 3 3
0 0 1 1 2 2 0 0 1 1 2 2 0 0 1 1 2 2 0 0 1 1 2 2
0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1
```

A permutation can be transformed to corresponding factorial digits by taking its items from the left and, for all but the last, recording the number of smaller digits which appear to its right:

```
   nld=.+/@:({. > }.)                   NB. number of lesser digits
   ptof=.}.`(nld,ptof@}.)@.(1&<@#)      NB. perm -> fact digits

   ptof 3 0 2 1
3 0 1

   each=.&>
   |:ptof each <"1 Llist 4
0 0 0 0 0 0 1 1 1 1 1 1 2 2 2 2 2 2 3 3 3 3 3 3
0 0 1 1 2 2 0 0 1 1 2 2 0 0 1 1 2 2 0 0 1 1 2 2
0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1
```

The inverse of<span class="aplu"> ptof </span>is also obtainable recursively, this time by joining each factorial digit to the immediately prior permutation list, copying any lesser digits and adding one to the others. To those who know J, this is probably described more clearly in J than in English!

```
   incgte=.] + <:           NB. increment if gtr or eq, else copy
   ftop=.(, -.)`({. , {. incgte ftop@}.) @.(1&<@#)
                            NB. fact digits -> perm
   |:ftop each <"1 fact 4
0 0 0 0 0 0 1 1 1 1 1 1 2 2 2 2 2 2 3 3 3 3 3 3
1 1 2 2 3 3 0 0 2 2 3 3 0 0 1 1 3 3 0 0 1 1 2 2
2 3 1 3 1 2 2 3 0 3 0 2 1 3 0 3 0 1 1 2 0 2 0 1
3 2 3 1 2 1 3 2 3 0 2 0 3 1 3 0 1 0 2 1 2 0 1 0
```

The above array demonstrates that ordering by factorial digits is equivalent to lexical ordering.

Factorial digits used with<span class="aplu"> ftop </span> resemble the left arguments of <span class="aplu">b.</span> inasmuch as each digit in the appropriate range represents a function. For example, a factorial digit of integers descending to 1 such as<span class="aplu"> 3 2 1 </span>represents *rotate* (<span class="aplu">|.</span>), a string of all 1s represents *1-shift*, a string of 2s followed by a 0 represents *2-shift*, a string of 3s followed by two 0s represents *3-shift*, and so on.

## Other Systematic Orderings of Permutations

Another systematic ordering for permutation lists involves using a recursive method to generate what is known as the Tompkins-Paige ordering, first published in 1960:

```
   TPlist=.3 : 0
if.y.>1 do.
  r=.,/(i.y.)|."1 each<(TPlist<:y.),.<:y.
else. r=.1 $0 end.
)
   |:TPlist 4
0 1 1 0 2 2 1 0 2 2 0 1 2 2 0 1 1 0 3 3 3 3 3 3
1 0 2 2 0 1 2 2 0 1 1 0 3 3 3 3 3 3 0 1 1 0 2 2
2 2 0 1 1 0 3 3 3 3 3 3 0 1 1 0 2 2 1 0 2 2 0 1
3 3 3 3 3 3 0 1 1 0 2 2 1 0 2 2 0 1 2 2 0 1 1 0
```

In the above array, focussing on how the 3s are blended with repetitions of the next lower order permutations list is the easiest way to see how the Tompkins-Paige ordering is formed. In time efficiency terms <span class="aplu">TPlist</span> and <span class="aplu">Llist</span> are broadly similar.

There are obvious ways in which orderings such as <span class="aplu">Llist</span> and <span class="aplu">TPlist</span> can be used to generate further systematic orderings, for example

```
   |:3 - TPlist 4
3 2 2 3 1 1 2 3 1 1 3 2 1 1 3 2 2 3 0 0 0 0 0 0
2 3 1 1 3 2 1 1 3 2 2 3 0 0 0 0 0 0 3 2 2 3 1 1
1 1 3 2 2 3 0 0 0 0 0 0 3 2 2 3 1 1 2 3 1 1 3 2
0 0 0 0 0 0 3 2 2 3 1 1 2 3 1 1 3 2 1 1 3 2 2 3
```

Another means of generating systematic orderings is to use TPlist n as an index to any one of its component permutations, for example:

```
   |:(TPlist 3){1 0 2
1 0 0 1 2 2
0 1 2 2 1 0
2 2 1 0 0 1
```

More generally, the component permutation can be randomised as in the following tacit verb train:

```
   rlist=.TPlist { ?@! { Llist   NB. list based on random perm
   |:rlist 4
2 3 3 2 0 0 3 2 0 0 2 3 0 0 2 3 3 2 1 1 1 1 1 1
3 2 0 0 2 3 0 0 2 3 3 2 1 1 1 1 1 1 2 3 3 2 0 0
0 0 2 3 3 2 1 1 1 1 1 1 2 3 3 2 0 0 3 2 0 0 2 3
1 1 1 1 1 1 2 3 3 2 0 0 3 2 0 0 2 3 0 0 2 3 3 2
```

## Parity of Permutations

A basic property of permutations is that of parity, which means the oddness or evenness of the number of switches required to bring the permutation back to i.n . This property is related to factorial digit representations in the following way:

```
   parity=.2&|@+/@ptof                  NB. rem((sum of fact digs),2)
   parity each<"1 TPlist 3
0 1 0 1 0 1
   parity each<"1 TPlist 4
0 1 0 1 0 1 1 0 1 0 1 0 0 1 0 1 0 1 1 0 1 0 1 0
```

Parity can also be obtained by considering the cycle form of a permutation as given by <span class="aplu">C. </span>which returns a list of boxed lists (cycles) whose lengths are given by <span class="aplu">#eachC. </span>:

```
   C.t=.6 4 2 7 1 3 0 5
+-+---+---+-----+
|2|4 1|6 0|7 5 3|
+-+---+---+-----+
```

A cycle of even length has odd parity and a cycle of odd length has even parity so

```
   #each C.t                            NB. cycle parities are E O O E
1 2 2 3
```

Parities combine according to the ‘not-equals’ rule, so in the above case the overall parity is

```
   par=.~:/@:-.@(2&|)@(#each@C.)        NB. parity by cycles
   par t
0
```

## Minimising Switches Between Adjacent Permutations

For <span class="aplu">rlist 3,</span> the permutation list parity pattern is either <span class="aplu"> 0 1 0 1 0 1 </span>or <span class="aplu"> 1 0 1 0 1 0 </span> which means that parity alternates between successive permutations. For <span class="aplu"> rlist 4 </span>the parity pattern is always one of

```
0 1 0 1 0 1 1 0 1 0 1 0 0 1 0 1 0 1 1 0 1 0 1 0
```

or

```
1 0 1 0 1 0 0 1 0 1 0 1 1 0 1 0 1 0 0 1 0 1 0 1
```

where in both cases, there are some consecutive permutations with the same parity. In programs which loop extensively through permutations computed in advance, the efficiency gained by minimising the changes between successive permutations can sometimes be worth considering. In 1963 S.M. Johnson of the RAND Corporation showed how to achieve such an ordering. A recursive technique is based on taking a single permutation at the next lower level, and creating<span class="aplu"> n+1 </span> further permutations by inserting<span class="aplu"> n </span>into all possible gaps starting either from the right or the left :

```
0 1 2 3                 3 0 1 2
0 1 3 2                 0 3 1 2
0 3 1 2                 0 1 3 2
3 0 1 2                 0 1 2 3
```

In array terms, the incoming value n is stitched to a skewed array of multiple copies of each of the next lower order permutations followed by a reverse skew which brings n onto the diagonal:

```
   rskew=:i.@-@# |.&> <"1              NB. right skew
   lskew=:i.@# |.&> <"1                NB. left skew
   mcopy=:$~ (,~ >:)@#                 NB. multiple copies
   agr=:lskew@(rskew@mcopy@[ ,. ])     NB. start inserts from right
   agl=:rskew@(lskew@mcopy@[ ,. ])     NB. start inserts from left
```

Now choose between right and left on the basis of parity:

```
   ag=.agr`agl@.(parity@[)             NB. all gaps (right or left)
```

and finally bring everything together in

```
Jlist=:3 : 0
if.y.~:2 do.
   r=.,/(<"1 Jlist <:y.)ag each <:y.
else. r=.0 1,:1 0 end.
)
   |:Jlist 4
0 0 0 3 3 0 0 0 2 2 2 3 3 2 2 2 1 1 1 3 3 1 1 1
1 1 3 0 0 3 2 2 0 0 3 2 2 3 1 1 2 2 3 1 1 3 0 0
2 3 1 1 2 2 3 1 1 3 0 0 1 1 3 0 0 3 2 2 0 0 3 2
3 2 2 2 1 1 1 3 3 1 1 1 0 0 0 3 3 0 0 0 2 2 2 3
```

This ordering not only involves just one switch between consecutive permutations, but additionally all such switches are between immediately neighbouring elements. However, the time to compute a Jlist is orders of magnitude greater than that required for a <span class="aplu">Tplist</span> or an <span class="aplu">Llist</span> , and so as, noted above, this method only pays off when there is a considerable amount of looping through pre-computed permutation lists.

## Combinations and Permutations of r from n

To find ordered permutations of <span class="aplu">r</span> items from <span class="aplu">n</span>, make every possible selection (that is combination) of <span class="aplu">r</span> elements from <span class="aplu">i.n </span>, and then transform each of these into a list of its <span class="aplu">!r</span> possible permutations using any of the permutation list methods. Obtaining systematic combination lists is not quite as tidy a matter as finding permutation lists. One possible technique is to express <span class="aplu">i.n </span> as binary numbers and select those whose digit sum is <span class="aplu">r </span>:

```
   cnos=.i.@:(2&^)@]              NB. integers from 0 to 2^n -1
   bins=.#:@cnos                  NB. binary nos from 0 to 2^n -1
   mark=.|.@((= +/"1@bins) # cnos) { bins
                                  NB. marks for bins with digitsum = x.
   combs=.mark #"1 i.@]           NB. transform marks to combinations
   |:2 combs 4
0 0 0 1 1 2
1 2 3 2 3 3
```

Now obtain a permutation list appropriate to every combination:

```
   perm=.,/@(<@TPlist@[ {&> <"1@combs)     NB. r from n
   |:2 perm 4
0 1 0 2 0 3 1 2 1 3 2 3
1 0 2 0 3 0 2 1 3 1 3 2
```

## An Alternative Method of Obtaining Combinations of r from n

The following technique is another way of obtaining the defining binary array. <span class="aplu">mk</span> is <span class="aplu">mark</span> with its component binary number lists in a different order. The method is based on the fact that the combinations of r from n consist of all the combinations of r from n-1, together with n added to all the combinations of r-1 out of n-1. In binary number terms:

r <span class="aplu">mk</span> n is (r <span class="aplu">mk</span> (n-1) stitched with 0) joined to ( (r-1) <span class="aplu">mk</span> (n-1) stitched with 1)

For largish values the double recursion makes this method less appealing without adaptations which could obscure the fundamental principle. The double recursion also leads to the two stopping values embodied in the first two lines:

```
mk=.4 : 0
if. x.=y. do. r=.(1,y.)$1 return. end.
if. x.=1  do.r=.=i.y. return. end.
r=.((x. mk<:y.),.0),((<:x.)mk<:y.),.1
)
   coms=.mk #"1 i.@]
   |:4 coms 6
0 0 0 0 1 0 0 0 1 0 0 1 0 1 2
1 1 1 2 2 1 1 2 2 1 2 2 3 3 3
2 2 3 3 3 2 3 3 3 4 4 4 4 4 4
3 4 4 4 4 5 5 5 5 5 5 5 5 5 5
```

## ... and Finally a Bang !!

Anyone who has delved into such matters must know the phrase ‘combinatorial explosion’ which describes how soon space and time boundaries are exceeded for even quite modest values of the parameters. This article has outlined the basic arithmetic of permutations and combinations en masse for whose exposition J is particularly suitable. Skilful tinkering and use of parallel processing can extend these boundaries, usually at an understandable loss of some clarity – that’s where programming ingenuity kicks in ...
