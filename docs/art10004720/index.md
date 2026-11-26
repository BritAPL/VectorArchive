---
vid: '10004720'
title: The Axiom Waltz – or When 1+1 make Zero
authors:
- Gérard Langlet
byline: 'by Gérard LangletAPL-CAM Journal, Vol 15, No 4, 16 October 1993,pp601-609, (c)1993: BACUS Translated by Diane Whitehouse and Gil Smith.'
volume: '11'
issue: '3'
page: '101'
source: trad/v113/langlet113_101.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- mathematics and statistics
---

by Gérard Langlet  
APL-CAM Journal, Vol 15, No 4, 16 October 1993,  
pp601-609, (c)1993: BACUS  
Translated by Diane Whitehouse and Gil Smith.
{ .byline }

Editor’s note: I am very grateful to the two translators. Gérard’s French is very difficult to translate, as it is packed tight with various kinds of word play, not to mention obscure French proverbs and sayings. Such great brilliance sometimes makes it harder to follow the train of thought; with Gérard there are probably three simultaneous trains of thought anyway. I would be glad to hear whether readers enjoy reading our best efforts at rendering Langlet into English. If so the latest issue of Les Nouvelles d’APL has a further two articles!

### Summary or Introduction

One of the most famous axioms in the history of mathematics is undoubtedly Euclid’s: *“Through a point situated outside a line, one can only draw a single line parallel to that line.”*

The abandonment of this dogma, fixed in the mind by centuries of teaching Euclidean geometry based on the aforesaid axiom, happened many centuries after Euclid, with Riemann and Lobatchevski, and led to a tremendously rich new mathematics (from which relativity ensued). Yet at the same time the whole world is convinced that *two and two make four*...

On a little reflection one realises that it is now possible to break down the **Information Wall**, like the Berlin Wall, without a great effort. This is largely thanks to APL with its wonderful properties, such as a pure mathematical notation, and the possibilities this language offers for experimentation in new ideas using any kind of modern microcomputer and then expressing these ideas concisely. In mathematical terms, this comes down to overhauling fundamental axioms, to exploring new routes – routes neglected not only by pure mathematical theory but which are, above all, missing *ipso facto* from models in physics and biology. It is unfortunate that these have, up till now, relied mainly on considerations of mass and energy, while the opportunity of research into **Information** remains wide open; yet it contains the seed of fruitful discoveries.

Physicists or biologists always gather Information (and very little energy) in their experiments. The computer, now a familiar tool to both, can be stretched to an unlimited extent to handle whatever Information one gives it. The DNA which programs us all is more Information than matter. So, let us destroy the Wall and try to revise the axioms. In principle, when one travels up the course of a river to find its source (whether that river is the Nile, the Seine, the Amazon, or the Mississippi), the widest branch at each confluence is not always the longest: one must explore every branch of every fork one after the other to get to know it all (which can’t be done all at once, unless you can use a satellite).

### Information’s Natural Processes

We know very little about the processes that lead a man to think, nor of those that govern the development of a human being starting from a chemical program (a metre and a half long if one unravels the wonderful double helix of the DNA sequence – the *helical* spring of our being wound up in the nucleus of our cells and invisible to the naked eye, with a “listing” of 23 pairs of sub-programs called chromosomes). When a system is self-organizing, the appearance of observable order would go against the laws of thermodynamics, for entropy seems to decrease in such a process; now entropy is, by definition, tied to energy: it is itself defined as its “quality”. On the other hand, if one considers the information in a self-organized system, one can postulate that this information must be preserved without any deterioration in quality, for example from one generation to the next, or during **metamorphoses**: the *caterpillar* contains all the information of the chrysalis which itself holds all the information of the *butterfly*.)

The **Principle of Conservation of Information** can replace, if not be identified with, that of conservation of momentum in mechanics, a principle that is well known to billiards players. But information is only expressed in bits or in pixels. One cannot either average it or smooth it without degrading its content. One can never increase the quantity of information contained in a system by either interpolation or extrapolation. Only reversible processes conserve all the information contained in a given system: these exchanges should produce a constant volume of information to remain optimal. If a system grows in size and adds to its information, it is because it has captured information from somewhere outside itself, to the detriment of another system that it has destroyed (naughty!); or, it has copied, aped, or cloned the information from another system without necessarily destroying it (dodgy but kind).

In mathematical terms, beginning with linear algebra for instance, we are led to seek plausible models for a simple and above all correct formulation (so as to make the least number of mistakes, we will put forward the smallest possible number of axioms.

### Matrix Mathematics

An inverse matrix (with no numeric mistakes) is an ideal operator for transforming information. Indeed – though in theory only – this matrix and its inverse offer us the possibility of transforming a problem’s data into results, but also of recovering the data from the results. In practice, it is impossible, except in particular very rare, if not trivial, cases, given any matrix M, to calculate the inverse matrix M<sup>-1</sup> such that the inverse matrix of the latter (M<sup>-1</sup>)<sup>-1</sup> is strictly identical to M. Readers with a knowledge of APL can try to disprove this more easily than others, for in APL any comparison of real magnitudes takes place while taking into account a relative *epsilon* called the **comparison tolerance**.

However, to model a **reversible** process, we need a self-inverse matrix, with M by definition identical to M<sup>-1</sup>. Hence, it seems useless to refine the numeric algorithms of matrix inversion. It would seem much wiser to use pure reasoning to investigate the required self-inverse matrices. To do so, we shall start small and then explore the trivial example of a one-row, one-column matrix necessarily containing 1 (in APL `1 1⍴1`), and then two-by-two matrices. Afterwards, let us extend our reasoning to a greater number of rows, going, if possible, right up to infinity to see whether by chance we might have forgotten some fundamental options, just as by failing to explore the pathways corresponding to branches of reasoning and perhaps to the toppling of parity in the positing of the initial axioms, the little black or white pebbles of Perrault’s Little Tom Thumb, lost in the combinatorial forests of a Game of GO, from “go to” to GOTO, the great Japanese theoretician of decision-making.

### Self-inversion of Matrices of Rank 2

For every matrix of rank 2, we can write a system of four equations (1) (2) (3) (4). This will allow us to search for the possible values for the four terms `a b c d` if the matrix is self-inverse. (The period symbol “`.`” on its own shows the generalized matrix product expressed by the inner product in APL in the form `+.×` for the numerical arguments.)

```
ab . ab -> a²+bc  ab+bd <--> 1 0 thus: a² + bc = 1(1) ab + bd = 0 (2)
cd   cd    ac+cd  bc+d²      0 1       ac+cd = 0  (3)   bc+d² = 1 (4)

Equations (1) and (4) imply: a² - d²  = 0                         (5)
then a² = d²  therefore either a = d or a = -d

A determinant equal to 1 is expressed by: ad-bc = 1               (6)
Equation (2) is resolved by either: b = 0 or with a = -d
Equation (3) is resolved by either: c = 0 or with a = -d
The solution a = d is only possible if b = 0 and c = 0.
Thus, according to (6) ad equals 1, thus a and d are both equal to 1 or -1.
```

Two matrices are therefore self-inverse. They are:

```
1 0  and  -1  0
0 1        0 -1

Thus a = -d, then equation (6) becomes: -a² - bc = 1              (7)

Adding (7) to (1) would force the equation to be resolved as:


0 = 2                                                   (8)
```

This resolution is especially sensitive in the context of axioms that are particularly well established. This kind of result constitutes an **Information Wall** that is generally impossible to climb. Unless ...

### A Change in Algebra or a Very Natural Algebra?

When we announce that *“two and two make four”*, we presuppose that *and* means *plus* and that *“one and one make”* is already *“two.”* In reality, if one human being and another human being make two humans, a man and a woman form a couple, and make a child. Counting purely numerically is therefore an abstraction of reality; or, more precisely, it is an abstraction of our judgement of reality (which occurs with the aid of our senses and our “understander”, the brain). If Huygens had already announced, in his *Treatise on Light* (1671) – and in French – *“We perceive ONLY the differences,”* he had already understood the puzzle’s fundamental axiom. However, Huygens had neither the APL nor a suitable computer to make any kind of progress except with the help of continuous functions.

All our biological receptors are discrete, and no measurements of anything can be made other than through sampling (and this assertion was already true before the time of Poincaré and Shannon) and we can never analyse nor model the universe itself: we can ONLY try to interpret, understand, and possibly calculate, our perceptions of the so-called universe.

The information we perceive can always be expressed by 0 and 1. We have found no other effective means of handling information other than as a series of 0s and 1s. We can rightly enquire, “Why?”

Both conceptually and naturally, all information can be reduced to 0s and 1s. Why therefore do we try, on a day-by-day basis, to shape that information into other forms which are much more difficult to digest and to manage by computers which, by definition, only know how to handle 0s and 1s? The simple inversion of a more or less large matrix executes thousands of millions, even billions, of useless and perfectly short-circuitable, conversions between various series of bits with a floating decimal point called code, and the numbers we are accustomed – through education – to use for counting and reasoning.

A new-born computer (without software) is, a priori, only able to transform 0s into 1s and 1s into 0s. However, it is already more intelligent than one might believe. The only arithmetic it can use is a natural arithmetic, the result of the physical processes which allow it to function: the quantified leap of a recognized state (electronically or magnetically symbolised either by + or by -) to another recognized state (electronically or magnetically symbolised either by – or by + respectively). Before we teach it something by stuffing it full of software, all it knows of floating-point and/or universal arithmetic is the **changing of the sign**. And the *rule of signs* applies:

```
+ and + is +, 0 + 0 is 0
+ and - is -, 0 + 1 is 1
- and + is -, 1 + 0 is 1
- and - is +, 1 + 1 is 0
```

The word “and” has become “plus”, but in MODULO 2. It is then to our advantage to replace the functional symbol + with the APL sign `≠`. Thus, we can understand to what extent, just how, and why Huygens was right.

Modulo 2 algebra was not yet known in the Enlightenment; it was studied by Galois at the beginning of the nineteenth century, and later by Boole (around 1860) who, thanks to some famous polynomials, codified its isomorphism with logical algebra. Even today, mathematicians still use the ⊕ symbol for + modulo something (among others, 2), as if the definition of exclusion or logical difference were derived from addition, and not the other way round. Unlike the symbol `≠` (which symbolises a true primitive when handling information), addition – which we still class as an “elementary operation” – is not one. It is impossible to make it so in a physical system in a simple way, and it is highly unlikely that we will one day be able to teach a molecule the necessary algorithms. On the other hand, a simple chain of molecules containing alternating single and double chemical bonds is already an organism which can carry out an operation like `≠\` by itself, like a rope hooked to one of its ends.

To understand the effect of `≠\` and its consequences, it is better to learn APL and start experimenting... Thus, we notice that `≠\` induces a genuine, waving, periodic process, chaotic in the extreme, and very similar to electrostatic, magnetic or gravitational effects. When we consider 0 as an empty space, and 1 as either a material or magnetic mass or as a charge, effectively the 1 entities are repelled only a short distance and are attracted over the long distance across the empty space (0 0 0 ... 0 0 0) without there being any need to understand the mechanism or to create equations of any sort. And `≠\` also represents the ideal model for an optimal decision-making chain, which in other programming languages is expressed laboriously with interwoven loops like:

```
IF a THEN BEGIN b: = NOT b; IF b THEN BEGIN c: = NOT c; IF c THEN BEGIN... ... END; END; END;
```

over several pages ... But, if `a` is a watchful neuron, and the sequence `b,c,d...x,y,z` a chain of sleeping neurons (all equal to 0), where `z` is the release mechanism for other processes, `≠\a,b,c`...is quite sufficient to model the moment that `z` wakes up as soon as `a` jumps to 1. It is only if there are *inhibitions* (equal to 1) between `b` and `y` that the chain will stop spreading the wake-up process. `≠\` `V` is sufficient in APL if `V` already contains the twenty-six bits necessary for the model. We can extend this to several million bits using one of today’s microcomputers.

The computer’s new-born arithmetic is a result of the natural physical properties of its constituents, which involve either opposing charges and “magnetic masses” that attract or identical charges that repel, as all physicists know perfectly well. The paradox 0 = 2 from equation (8) is resolved effortlessly. Two electrons with a negative charge (by convention, `-` or 1), which one unfortunately wants to be in the same place, are going to leap elsewhere, leaving behind a void which will be seen and recorded as an absence of charge (in reality, 0) but always noted down as `+`, a positive charge (which it is not really) purely by convention. In an empty space (0), we can put nothing (0), or put something (1). We can take nothing away (0) from a full space (1), or we can empty it, setting it back to 0, by taking out the something (1) that was inside. But we certainly cannot fill the space up again, because it would not let itself be pushed around like that.

This waltz of the monopoles and electrons follows an identical logic to that of the information waltz it manages: physical properties and natural algebra are then isomorphic at the level of parity, the logic of the two numbers 0 and 1, the only elements of the set Z/2Z.

### Forgotten Matrices

Actually the algebra of the set Z/2Z is the **only** known algebra in which 1+1 MODULO 2 indeed equals 0. With the help of *programmable APL*, we can express and calculate this more easily in binary algebra as `0≡1≠1`.

So, if one returns to the impossible equation 0 = 2, the reasoning we have followed so far has also constructed a mathematical proof that there is NO other choice. Of course, the equations (1) (2) (3) (4) with which we started can also be posed entirely in modulo 2 algebra in which sum and product exist, the sum identifying with the difference.

So, a = d and a = -d are a single, same solution: a and d must equal 1 if bc is zero. According to equation (6), b or c must therefore be 0, and both can be zero. But the solution ad = 0 with a = d also allows the finding that bc can equal 1. In Z/2Z, this leads to b = 1 and c = 1. Thus, in Z/2Z, there are four solutions to the problem of self-inverse matrices of rank 2, of which three are not trivial:

```
(I)   (anti-I)   (Gh)   (Gv)
1 0     0 1      1 0    1 1
0 1     1 0      1 1    0 1
```

In terms of transformational matrix operators that are neither trivial nor almost trivial (as is the case of anti-I), only Gh and Gv are left. They are the only possible involution operators in Z/2Z of an algebra that is not signed and is necessarily linear, to describe any transformation that is supposed to be reversible.

In Z/2Z there are four square roots to I, the matrix-unity of rank 2. But other matrices can be reversed, because condition (6) `ad-bc = 0` can be rewritten as `ad ≠ bc` in binary algebra, where multiplication becomes the logical function of logical AND `^`:

```
b ^ c = 1 leads to b = 1 and c = 1.
```

In Z/2Z, the product `bc` becomes the function `MINIMUM (b,c)`. In APL it is `b ⌊ c`.

So, we must have `ad = 0` since, if a is equal to 0, d is equal to 1, or vice-versa. As a result, the only other two possible reversible matrices are:

```
(G)        (Gd)
1 1   and  0 1
1 0        1 1
```

These two matrices are not part of the set of four matrices listed above. They cannot be self-inverse; they must, however, be the inverse of each other. Besides, it is necessary that the set of six matrices which combine them forms a multiplying group for the matrix product in Z/2Z.

The product of these two matrices one multiplied by the other, through symmetry, must be commutative and if the two matrices are symmetric the result must be that one of the matrices is itself symmetric, so (I) or (anti-I). The simple scalar product MODULO 2 of 1 1 by 0 1 has 1 as a result, which eliminates (anti-I). Consequently, the matrices (G) and (Gd) have (I) as a product. So, if the product of the two inverse matrices is the matrix-unity (I), the matrices will each be the cubic root of (I). As transformational operators, they will have the properties of j and of j<sup>2</sup>, the complex cubic roots combined with unity.

Without particularly looking for any other examples, these properties appear in all their simplicity, like Botticelli’s Venus, where the enigma 0 = 2 provides the key to the electrical and magnetic fields which cause Venus to rise so discreetly from the waves.

Our reasoning showed, before we sought to extend it to matrices of any rank R (R varying from 2 to infinity), that there are no other matrices than those listed here for rank 2. In an entire algebra in modulo 2, which is isomorphous to logical algebra, we know how to define a coherent set of “spinors”, that **generate a three-dimensional space**. In fact, we can define a three-sided object with orthogonal axes and three orthogonal planes. This implies that we can at least identify the rotating operator of a third of a turn around the diagonal of this trihedron. It needs a rotation matrix equivalent to j (and so, necessarily, the inverse matrix equivalent to j<sup>2</sup> to allow the inverse).

In complex classical algebra, for rank 2, this is physically impossible; it cannot be achieved without error or approximation in a device trying to put these calculations physically into effect. In no other programming language than APL can we define j (alias G) more simply – and in a rigorously exact manner, in four bits – and then use it.

The symbol G identified with j means “Geniton”.

A Geniton generates symmetry in a topological space of parities which are either 0 or 1. The choice of identifying G with j and Gd with j<sup>2</sup>, rather than the opposite, is a convention. It is like the trigonometric direction in a complex plane, or the *minus* sign designating the charge on an electron, or 0 referring to False in Logic, or the sign for heat being emitted (in Chemistry, this is a positive sign, but negative in Physics where the system is considered as losing energy). We can see that a system retains its total information during a reversible phase-change; for example when water freezes, it keeps all its information since it returns to its former structure when it melts – as many times as we wish.

### Extension to Higher Ranks or Orders

Are there any matrices, in the same algebra, that have the same properties as G and Gd (the inverse of their square), Gh and Gv (self-inverse), and of which the matrices of rank 2 are the sub-matrices, for every value of row R?

The symmetry in relation to the second diagonal, in G2, can replace the geniton in rank 2: this is at the same time, the operation of matrix inversion and that of the elevation to the square. (It is the equivalent of the symmetry in relation to the horizontal axis of the complex plane, called conjugation, which changes the sign of the imaginary part of a complex number.) By auto-similarity, let us replace every 1 in G2 by G2 and every 0 by Z2, the null matrix of rank 2. We obtain G4:

```
1 1 1 1                                                 0 0 0 1
1 0 1 0     of which the symmetrical expression         0 0 1 1
1 1 0 0     in relation to the second diagonal is       0 1 0 1
1 0 0 0                                                 1 1 1 1
```

The matrix product can be found from the products of the sub-blocks of rank 2 (I2 is the unified matrix of rank 2):

```
G2 G2 . Z2 G2
G2 Z2   G2 G2
```

with the result

```
 I2 Z2
 Z2 I2
```

of which the unified matrix is I4.

Through repetition, this reasoning can be extended to infinity. It shows that, by replacing each 1 in G4 directly by G4 and each 0 directly by Z4, G4, then G8, then G16, and so on, (or even directly, G16 from G4), are always the symmetrical inverses about the second diagonal d. Their product, that is, the elevation to the square of each matrix thus obtained, always has as a result – at all its orders – this same inverse.

So, this property will also be preserved if we remove the last line AND the first column of all the matrices G4, G8, G16, etc. The example below of G3, sub-matrix of G4, shows this:

```
1 1 1
0 1 0
1 0 0
```

The symmetry, in relation not to the second diagonal but to the centre, is:

```
0 0 1
0 1 0
1 1 1
```

If we continue suppressing both the last line and the first column, in every matrix G of row R equal to the power of 2 (it will eventually be very large), the characteristic must be maintained. We will have both a construction and a rigorous representation, from 2 to infinity, of the rotation operators j and j<sup>2</sup> in a complex plane. There will be no need to define, as is usual as a precondition, any number other than 0 and 1. Nor will it, above all, be necessary to define *i*, the imaginary root of -1, which first required the definition of negative numbers. (A well-organised head being worth more than a well-filled head, Montaigne.)

These verifications and demonstrations are child’s play for those who know APL and its marvellous primitives which allow one to study all these constructions, and even enjoy it (and why not?).

The union of the sets G and Gd, forms the “bi-compound” set. The set is so called because it is double, and contains all the compound axial rotation operators in both senses of rotation. It can also, if we use a computer, be expressed through binary algebra (thus, in bits) without there being any need to use whole, real, or above all complex, arithmetic.

Similarly, we can also easily show, by starting from the sub-blocks G2, G2h, G2v, and Z2, then G4 and so on, and successively suppressing lines and columns, that the horizontal rotations Gh, and/or, via symmetry, the vertical rotations of the “bi-compounds” G or Gd, for each row R from 2 to infinity, will be self-inverse. As a result, they represent the only matrix operators able to carry out direct orthogonal transformation on information sequences (which are, by definition, coded in bits and able to sample every modulation and form all imaginable computer programs) without error or approximation. They will also model both physical and biological changes while conserving the information of the systems – which are, by definition, reversible.

If, as Jacques Brel sang, the \[axiom\] waltz has taken some time, the blows from its battering ram will get the better of the (Information) wall, little knock by little knock, which can also be modelled by `≠\`.

(webpage generated: 25 March 2007, 02:10)
