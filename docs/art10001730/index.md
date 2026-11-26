---
vid: '10001730'
title: 'Gérard Langlet: A Man of Distinction'
authors:
- Sylvia Camacho
byline: by Sylvia Camacho
volume: '22'
issue: '2'
page: '56'
source: trad/v222/lang222.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- mathematics and statistics
- APL in perspective
---

by Sylvia Camacho
{ .byline }

We take as given the idea of distinction and the idea of indication, and that we cannot make an indication without drawing a distinction.

G. Spencer Brown,1969

Gérard Langlet (1940-1996) was a scientist by profession, a polymath by inclination and an expert user of computers from necessity. He published only in specialist French and English journals and died before he was sixty. Consequently his ideas have been little known and difficult to access but the growth of the Web has now offered his admirers an easy remedy. This paper is therefore addressed to curious browsers who may happen upon it in a search among the many fields which captured Langlet’s imagination. Readers are warned that he was never overawed by the authorities that he challenged and students would be wise to toe the party line until their examinations are safely passed before exploring the unconventional ramifications of his research.

Langlet questioned the mathematics which is used to underpin the sciences of physics, chemistry and biology, particularly for description and analysis at the smallest scale \[[2](#ref2) p119\]:

> Continuous fields (and their equations) are in fact an approximation, they would have no meaning at quantum scale, since the simple idea of a quantum should imply that no infinitesimal quantity should ever been *(sic)* thought of, for any plausible mathematical model in physics or biology.
>
> It is impossible to understand a process such as vision without trying to understand the action of one quantum on another quantum.

He began his career as a chemical engineer but then moved into crystallography and biochemistry and was married to and collaborated with a theoretical microbiologist. The organisation for which he worked, near Paris, has in its extensive description, atomic energy, materials science, condensed state matter, molecular chemistry and information research. In the nature of their business they make wide use of computers and are well-informed about software and Langlet took a close interest in both the architecture of the machines and the various programming languages available for them. During the 1970s he was introduced to the alternative mathematical notation invented by Kenneth Iverson in the late 1950s and 1960s and implemented as a computer language by IBM under the acronym *APL* (<i>A P</i>rogramming <i>
L</i>anguage)<i>.</i> It was Iverson’s additions to the scope of conventional mathematical notation that especially attracted Langlet’s attention. He says that he had studied the impact it made across his wide field of interest for over ten years before, in 1992, he made his major presentation to the international *APL* conference in St Petersburg. However it is quite unnecessary to have experience with *APL* as a programming language to appreciate the aspects of Iverson’s approach that suggested a resolution of Langlet’s mathematical problems, which he frequently summarised in this quotation:

<b>We
only perceive differences.</b>  
Christian Huygens, <b><i>Traité de la Lumière,</i></b> 1678

Digital means for control or calculation were used well before the Twentieth Century. Jacquard looms and pianolas are hole/no-hole devices. Charles Babbage called his first calculator a Difference Engine and telegraph and telephones made use of on/off relays. Valves, later to be replaced by transistors, came into their own as fast switching devices only after the Second World War but from that time digital technology has become increasingly favoured and when Iverson became involved with the design of IBM computers no one doubted that they would be fundamentally digital machines. This posed some problems for scientists and engineers for whom mathematics was almost synonymous with calculus and continuity. Circuit designers therefore turned to George Boole’s algebra for concepts to match on/off switches. This explains how ‘or-gates’and ‘and-gates’ became symbols in the ‘logic-blueprints’drawn up by manufacturers of pulse-driven devices. However computers are primarily required to do mathematics not formal logic and a way had to be found to describe how they could achieve it, starting from simple yes/no choices. Kenneth Iverson, although a mathematician, had been closely involved with the design of digital computers as they were becoming a commercial proposition in the United States in the late 1950s and early 1960s. He realised that conventional mathematical notation was not a suitable medium in which to describe the fundamental operations of such machines and over a few years he invented an alternative; different in three major respects. Firstly, the rules for using any of the conventional notations he chose to incorporate were recast to a rigorous consistency suited to the unintelligent rigidities of a machine. Secondly, the mathematical objects upon which the notation operated were conceived as arrays, of arbitrary dimensions, with respect to which a single number or character is merely an item. Thirdly, he subsumed in his mathematical notation the arithmetic of George Boole’s algebra.

Anyone who has to incorporate mathematical expressions within documents created on a modern computer can sympathise with the problems which Iverson faced. In addition to its invented symbols and wide-ranging plunder of several alphabets, conventional mathematical notation is not written along a line but is disposed on a plane, making no attempt to respect font size. Added to this are obscure rules of precedence for the order in which mathematical expressions should be scanned and resolved and all had to be subject to constraints imposed by the limited number of computer codes available for symbol assignment. Iverson used non-alphanumeric characters for the mathematical operations he needed to define and called them ‘primitive functions’. These fall into three groups; first, those with conventional notation equivalents; second, new characters used in the manipulations of arrays; third, characters specific to computer implementation. Computers are imperative machines for which all inputs are commands demanding a response. With Iverson’s notation the response to any expression immediately follows the input instruction. This is known as the interactive approach and is characteristic of languages implemented by interpreted as opposed to compiled software. It is more expensive in computer resources but is ideal for teaching and for experiments in mathematics. Iverson was above all a teacher who had a lifetime interest in natural language and its grammatical structure and he considered mathematics to be a language open to the same analysis. It is hoped that the following paragraphs will give some idea of the power and elegance of his notation although it is not in any sense a comprehensive description of *APL*, being only that small part needed to appreciate Langlet’s thesis.

For the four arithmetic operations *plus, minus, times and divide*, Iverson accepted the conventional symbols ’<b>+ - x ÷’</b> but, to distinguish between a negative value and a subtraction instruction, he used a high minus immediately preceding and forming part of the number to be signed negative for, as Cornelius Lanczos says \[[3](#ref3) p65\]:

To use the minus sign with two different meanings is obviously confusing and damaging to the conceptual understanding of mathematics.

When Iverson came to add *raise to a power* and *extract a root* he made a significant simplification. He used the asterisk ‘<span class="APLU">\*</span>’ for the primitive function, which takes two arguments; to the left the value, to the right the power to which it is to be raised and thus <span class="APLU">2\*5</span> gives 16. What then should he use for &amp;sqrt; or *root*? To Iverson the answer was obvious; roots are fractional powers and the asterisk will serve for both: the value is still the left argument with the requisite fraction to the right and thus <span class="APLU">16\*0.5</span> gives 4, the square root. The fraction of course might itself be a result returned by an expression on the right. Precedence must always be unambiguous and this is achieved by a rule that the righthand argument to any function is the result of the entire expression to its right. This makes unnecessary the conventional rules about the order in which evaluations are made and greatly reduces the need for parentheses, although these can be used to force the evaluation of expressions so enclosed, causing the result to be substituted for the contents of the parentheses in the overall working. Functions requiring two arguments he called ‘dyadic’and then went on to define ‘monadic’ uses: asterisk ‘\*’ with only a right argument he defined as *exponential* so that <span class="APLU">\*1</span>, for instance, returns *e*, the base of the natural logarithm. Iverson delighted in emphasising such relationships by his choice of symbols. For example *logarithm* is a circle surrounding an asterisk, the left argument being the base (10 for school logarithms) but it defaults to *e,* the natural logarithm. *APL* has many such elegancies but only selected examples can be illustrated here because many use special symbols in fonts that may not be easily available to everyone. Fortunately most of Langlet‘s ideas can be explained using common fonts. For  curious readers, sources of free *APL* interpreters are listed below.

Iverson generalised the idea of a variable to make the fundamental concept for all data that of an array. He left to the code implementing the interpreter the task of recording the size and shape of all data variables and for tracking any changes ordered by the program, allocating and releasing storage dynamically, thus relieving the programmer of the onerous task of reserving appropriate memory for each variable and releasing it for other uses when no longer needed. The programmer can control this if he wishes, finding the shape of any array with command ‘<span class="APLU">⍴</span>’ (Greek *rho*) to ask for the *shape* of the right argument or, with a left as well as right argument as in ‘<span class="APLU">2 4 5r</span>’, to *reshape* it (here to two matrices of four rows and five columns).

Character data is distinguished by opening and closing quotes. Numeric character sequences enclosed in this way can be converted to computable numbers with the function *execute*. Conversion the other way, from numeric to character form is done with function *format* (the symbols for these are not in this font). Alphabetic material is thus easily handled and integrated with the mathematical formalism.This flexibility required a large number of primitive functions to be devised for creating, changing, shaping and interrogating multi-dimensional arrays, which can themselves be arguments to most of the primitive functions. We shall only illustrate in this paper those needed to explain Langlet’s work. This took its inspiration from Iverson’s inclusion among his primitive functions, of notation to operate with binary data, expressed using only the digits zero and one.

In propositional logic Boole’s formalism operates with values *true* and *false*, the values of propositions which, in his algebra, are combined using symbols ‘<span class="APLU">^∨~</span> ’for *and, or, not.* However, Boolean logic can also be understood as the arithmetic of the set 0,1 where 0 replaces *false* and 1 replaces *true*. Iverson realised he could enhance the mathematical significance of Boole’s logical notation by including as primitive functions the conventional symbols ‘<span class="APLU">=</span>, <span class="APLU">≠</span>, <span class="APLU">&gt;</span>, <span class="APLU">≥</span>, <span class="APLU">&lt;</span>, <span class="APLU">≤</span>’, which return the true (1) or false (0) response according to the values of their left and right arguments. This is an exceptionally powerful tool when the arguments to which the tests are applied are arrays of any size and shape.

Iverson included four *operators* to modify the action of his functions. An operator has one or two functions as its arguments and produces a modified function as a result; the modified function then acts on the argument or arguments to the right (and maybe also to the left) of the operator expression. The operators are *inner product (<span class="APLU">.</span>)*, <i>outer product (</i><span class="APLU">∘ .</span>), *reduction* (<span class="APLU">/</span>) and *scan* (<span class="APLU">\\</span>). For example the *plus dot times* *inner product* (written *<span class="APLU">+ . </span>* <span class="APLU">×</span>) is conventional matrix multiplication. Iverson allowed an operator’s arguments to be any compatible primitive functions. The arguments of the expression containing the operator have to be compatible; since, for instance, in matrix multiplication the values to be multiplied must be in pairs, the first argument must have as many values in its columns as the right argument has in its rows and the number of rows in the left argument must be the same as the number of columns in the right argument. (If they do not the APL interpreter halts with an error.)

The second operator, *outer product* ‘<span class="APLU">∘ .</span>’(<span class="APLU">∘</span> is called ‘jot’) takes a dyadic function *f* to produce a new dyadic function <span class="APLU">∘ . f</span> (jot dot *f*). For example, the following expression produces a multiplication table:

```
      1 2 3 4 5 ∘ . ×1 2 3 4 5
 1   2   3   4   5
 2   4   6   8  10
 3   6   9  12  15
 4   8  12  16  20
 5  10  15  20  25
```

The third operator ‘reduction’ is monadic. Its single argument is the function to its left and its effect is as if the function were repeated in between each pair of elements of the right argument. So <span class="APLU">+/</span> with a row of numbers as its argument returns the sum of the row.

```
      +/ 1  2  3  4  5
 15    (i.e. 1+ 2 + 3 + 4 + 5)
```

Operators apply equally to Boolean primitive functions and Boolean arrays and it was this facility in particular that attracted Langlet’s attention. As he played with *APL*, he realised that, for his investigations, a particular expression was overwhelmingly significant. This concerned the fourth operator ‘<span class="APLU">\\</span>’called *scan.* Scan, like the other operators can be used to modify any compatible function (so <span class="APLU">×\\ 5
9 6.4 100</span> yields <span class="APLU">5 45 288 28800</span>, the cumulative products). Langlet was fascinated by the uses of operators with Boolean logic functions and particularly <span class="APLU">≠\\</span> (not-equals scan). This, used on a row of binary digits, performs modulo-2 parity propagation. Integers are said to have the same parity if they are either both odd or both even. As we have seen <span class="APLU">+\\1 1 0 0 0 1 0 1</span> will return <span class="APLU">1 2 2 2 2 3 3 4</span>, the cumulative sum, but <span class="APLU">≠\\1 1 0 0 0 1 0 1 </span>returns <span class="APLU">1 0 0 0 0 1 1 0</span>, the cumulative parity or in other words, the 2-modulus of the cumulative sum, a fundamental of binary arithmetic.

However, Langlet had in mind a more primitive concept particularly applicable to the atomic level physics, chemistry and biology with which he was professionally concerned. In his own words \[[2](#ref2) p120\]:

> <span class="APLU">≠\\</span> is the correct mathematical formulation of *the least–action principle* (expressed in decision theory as well as in genetics). This mechanism never introduces noise into the data it processes and operates on a constant volume (global shape, size) of data, therefore it represents the conjuction of an *adiabatic system* – which never produces heat i.e. entropy i.e.disorder – and of a system whose evolution would progress at *constant volume* in thermodynamics. With the usual criteria of classical thermodynamics – which does not take information into account as the essential factor, but postulates continuity, simultaneity, numeric additivity of actions together with some randomness, however modulated by mysterious statistical distributions, the 2<sup>nd</sup> principle of thermodynamics cannot be respected by living organisms!  <span class="APLU">≠\\</span> now brings a magnificent counter-example.

In 1991 he expressed his ideas in a paper called, *Paritons and Cognitons Towards a New Theory of Information* \[[5](#ref5)\] but it is perhaps easier to approach his thinking from a later paper (1993) which he called by a typically provocative title, *The Axiom Waltz or When 1+1 make 0* \[[4](%27ref4)\]<i>.</i>

Langlet postulated a Principle of Conservation of Information analogous to the conservation of momentum in mechanics. Information is only expressed in bits and cannot be either averaged or smoothed. He displayed a typical joking slide at a conference:

**Déclaration des Droits du Bit  
 (Antwerp 1994)**

> > 0\. All bits contain information, so they deserve respect.  
> > 1\. Every small bit is a quantum of information.
>
> Nobody has the right to crunch it.  
> Nobody has the right to smooth it  
> Nobody has the right to truncate it.  
> Nobody has the right to average it with it(s) neighbour(s).  
> Nobody has the right to sum it, except modulo 2.   

Only reversible processes conserve all the information contained in a given system and, as this is what he wanted to do, he needed a Boolean model comparable with linear algebra. In his paper first published in 1993, he defined the axioms for such a calculus. He favoured a matrix mathematics because a self-inverse matrix without numeric approximation would be the ideal operator for transforming information. He intended to show how this is possible for Boolean matrices and he began with order 2 as follows.

Conventional matrix multiplication is defined:  
matrix a b  multiplied by matrix a b  results in matrix  a<sup>2</sup>+bc  ab+bd  
             c d                                        c d                                 ac+cd  bc+d<sup>2</sup>  
 therefore to give a Boolean matrix of 1  0  the following equations must hold:  
                                                                   0  1              
                          a<sup>2</sup>+bc=1  (eq.1)   ab+bd=0 (eq.2)  
                          ac+cd=0  (eq.3)   bc+d<sup>2</sup>=1  (eq.4)   
  
Equations (1)&(4) imply: a<b><sup>2</sup>-</b>d<b><sup>2</sup></b>=0  (eq.5)  
therefore a<sup>2</sup>=d<sup>2</sup> & either a=d or a=<b>-</b>d  
  
A determinant equal to 1 is expressed:  ad-bc=1                         (eq.6)  
(eq.2) is resolved by either: b=0  or  a=<b>-</b>d  
(eq.3) is resolved by either: c=0 or a=<b>-</b>d  
The solution a=d is only possible if b=0 & c=0  
Thus, according to (eq.6) ad=1  
Thus a & d are both equal to 1 or <b>–</b>1

Two matrices are therefore self-inverse:    1  0   and   <b>-</b>1  0  
                                                                          0  1              0  <b>-</b>1  
Thus with a=<b>-</b>d (eq.6) becomes:   <b>-</b>a<b><sup>2</sup>-</b>bc=1                                  (eq.7)

But adding (eq.7) to (eq.1) would force the resolution  **0 = 2**

This result might seem absurd but in fact it indicates that we are dealing with a finite algebra in which these numbers are congruent modulo 2.

Langlet held that information can always be expressed by zeros and ones because our perceptions rely upon differences for sensing our universe. Biological receptors operate in discrete mode just as, at the fundamental level, computer components have only two states. Therefore the most elementary operation is not addition, it is <span class="APLU">≠</span> *not-equal*.

Langlet’s conclusion is that physical properties and natural algebra are isomorphic at the level of parity which is the logic of 0 and 1, the only elements of the set Z/2Z, his abbreviation for modulo-2 integer algebra. This is expressed in *APL* as <span class="APLU">0 ≡ 1 ≠ 1</span> (the *APL* name for <span class="APLU">≡</span> being *match*) which translated is, ‘it is false to say one does not equal one’. The equations (1), (2), (3), (4) can thus be written in Z/2Z, for which sum is equivalent to *<span class="APLU">≠</span> not-equal* and product is equivalent to <span class="APLU">^</span> *logical-and*. So, as a=d and a=<b>-</b>d are the same, they must equal 1 if bc is zero. By equation (6), b or c or both must then be zero. But ad=0 with a=d also allows bc=1 (b=1 *and* c=1). Thus in Z/2Z there are four matrices (G stands for *Geniton* by analogy with Hamiltonian, a typical Langlet play on words!):

       Identity/anti-id matrices           horizontal Geniton          vertical Geniton  
             (I)         (anti-I)                                 (Gh)                               (Gv)  
            1  0          0  1                                      1  0                                 1  1  
            0  1          1  0                                      1  1                                 0  1  
These are the only transformational matrices in Z/2Z that are not trivial (anti-I is almost trivial because its square <i>anti-I</i><span class="APLU"> ≠.^</span> *anti-I* yields *I*).

Condition (6)  ad-bc=0 can be rewritten ad <span class="APLU">≠</span>bc in binary algebra (for which multiplication is <span class="APLU">^</span>, so b <span class="APLU">^</span> c <span class="APLU">≡</span> b=1 *and* c=1). This means that the product in Z/2Z has become the function which only returns 1 if both arguments are 1 and otherwise returns zero (that is the meaning of <span class="APLU">^</span>). This is equivalent to a return of the *minimum* of its arguments. So ad=0 entails the other two possible matrices which, while not invertible, are reverses of each other.

 They are:  
           (G)          (Gd)   for both of which a<span class="APLU">^</span>d=0. Each is the rotation of the other  
           1  1           0  1    about the second (bottom left to top right) diagonal  
           1  0           1  1  
Together with the four matrices above, this set of six forms a multiplying group for the matrix product in Z/2Z, as defined by Cornelius Lanczos \[3 p60\]:

 A . . . characteristic feature of a group is that an *operation* is given which involves two elements of the group. . . .  A fundamental condition for the existence of a group is that the result of this operation must again be an element of the group.

The expression for matrix multiplication, *matrix* <span class="APLU">+.×</span> *matrix* has as equivalent for Z/2Z  <i>matrix</i><span class="APLU">≠.^</span><i>matrix</i>, (<span class="APLU">≠</span> being the equivalent of plus and <span class="APLU">^</span> the equivalent of times). For instance:

            1  2  <span class="APLU">+.×</span>  1  2      returns     7  10        
            3  4             3  4                      15  22                 while

            (G)             (G)      returns    (Gd)            
            1  1  <span class="APLU">≠.^</span>   1  1                       0  1  
            1  0             1  0                       1  1                     and

            (G)             (Gd)    returns    (I)  
            1  1  <span class="APLU">≠.^</span>    0  1                      1  0  
            1  0              1  1                      0  1          and  likewise Gd <span class="APLU">≠.^</span> G returns I

which is the identity (unit) matrix. So G and Gd are each a cubic root of I and as transformational matrices they are analogous to j and j<b><sup>2</sup></b>, the complex cubic roots of 1. In complex classical algebra this cannot be achieved without error generated by approximation to *e* and p and *i*. 

Langlet explains that he chose ‘G’ to stand for ‘Geniton’ because it can be used to create matrices of higher orders \[[4](#ref4) p109\]:

 Are there any matrices in the same algebra, that have the same properties as G and Gd (the inverse of their square), Gh and Gv (self-inverse), and of which the matrices of rank 2 are the sub-matrices, for every value of row R?

If G, being order 2, is renamed G2, we can then create a matrix of order 4 by replacing the three ones in G2 by G2 and the zero by the order 2 zero matrix Z2:  
       G2  G2                       1   1   1   1  
                            thus      1   0   1   0     which being order 4 we may call G4  
       G2   Z2                      1   1   0   0  
                                          1   0   0   0

Given that Gd is the inverse of G, what is the inverse of G4?

        G4 <span class="APLU">≠.^</span> G4 gives    0   0   0   1  symmetrical with G4 about the second diagonal  
                                          0   0   1   1  This could be called Gd4  
                                          0   1   0   1  
                                          1   1   1   1

 while G4 <span class="APLU">≠.^</span> Gd4 gives the order 4 unit matrix I2  Z2    1   0   0   0  
                                                                                       Z2  I2    0   1   0   0  
                                                                                                     0   0   1   0  
                                                                                                     0   0   0   1  as expected.  
This reasoning can be extended to a matrix of order infinity. Moreover these properties are preserved after removal of the last line and first column of matrices G4, G8, G16 . . . etc. to give matrices of odd orders. Langlet’s conclusion from these procedures is \[[4](#ref4) p119\]:

 We will have both a construction and a rigorous representation, from 2 to infinity, of the rotation operators j and j<b><sup>2  </sup></b> in a complex plane. There will be no need to define, as is usual as a precondition, any number other than 0 and 1. Nor will it, above all, be necessary to define <b><i>i</i></b>, the imaginary root of <b>–</b>1, which first required the definition of negative numbers. (A well-organised head being worth more than a well-filled head, Montaigne.)      **<i> </i>**

An outstanding example of the application of this reasoning is his analysis of the hexagonal arrangement of the hundred million rods in the human retina \[2\].

*The Axiom Waltz* paper formalised the research Langlet had presented two years previously in *Paritons and Cognitons.* There he reminded his readers of the traditional use of parity checking for computer error detection and went on to demonstrate the intriguing object generated by appending iterations of<span class="APLU"> ≠\\</span> parity propagation onto a Boolean vector of any length, creating a matrix\[[5](#ref5) p98\]:

 Let us define the PARITON as the Boolean MATRIX containing as rows, the differing bit sequences of successive parity integrals of an original bit sequence.

 For instance:  
       <span class="APLU">≠\\</span>0 1 1 0 0 0 0 1 0 1 1 1 0 0 0 0 0 1 1 0 1 1 0 0    gives  
            0 1 0 0 0 0 0 1 1 0 1 0 0 0 0 0 0 1 0 0 1 0 0 0

and, of course, the result of *not-equal reduce*  
<span class="APLU">≠/</span>0 1 1 0 0 0 0 1 0 1 1 1 0 0 0 0 0 1 1 0 1 1 0 0  
of the same vector returns its parity, 0, the last item.

Langlet is drawing an analogy between the use of *not-equals scan* in his Z/2Z algebra and integrals and differentials within conventional mathematics. He points out that <span class="APLU">+\\</span> *plus scan* can be used to plot the integral of any function if computed at frequent intervals in the discrete domain. Therefore <span class="APLU">≠\\</span>, *not-equals scan,* can be considered a *parity integral.* Moreover it can be applied iteratively to obtain the *parity integral* of the *parity integral* and this has some very interesting consequences. In particular a periodicity will appear after a number of iterations and, if each iteration is held as a next row, the full cycle will result in a square matrix which has the characteristic that the entire matrix can be retrieved by successive iterations of any row. The number of iterations in the cycle will be that power of 2 greater than or equal to the length of the initial vector. Such a cyclic matrix can be considered the topological equivalent of a cylinder in which case the final column can be viewed, end on, as a polygon or circle; to which Langlet gave the name *cogniton*. It contains, of course, the cumulative parity of each row. The inverse of parity integration, *parity differentiation*, is calculable from this last column or *cogniton*. The procedure is as follows:

calculate the penultimate column using *cogniton* <span class="APLU">≠</span><i>a one-bit
right rotation of cogniton</i>  
from the penultimate calculate the ante-penultimate column by the same procedure  
iterate until the square matrix is complete.

He points out that the periodicity of the left half of the matrix is one-half of the whole and that of the left quarter is one quarter of the whole, and so on. Thus the sequence of the first 4 bits repeats at the 5<sup>th</sup> row, the sequence of the first 8 bits repeats at the 9<sup>th</sup> row, the first 16 at the 17<sup>th</sup> and so on. The pariton resulting from an initial sequence containing only Boolean ones gives rise to the fractal known as *Sierpinski’s Gasket*. This is self-similar at all scales and has the interesting quality that if each row is understood as the base two representation of a single number, the set has no duplicates and its maximum value grows with the size of the matrix. Langlet noted, without drawing any conclusions, that these numbers if the right-hand digit is taken as the most significant, are similar to the *paroxistic series* which is exhibited by various natural phenomena \[[6](#ref6) p130\]. For instance a 32-bit matrix converts to:

 1 3 5 15 17 51 85 255 257 771 1285 3855 4369 13107 21845 65535 65537 196611  
327685 983055 1114129 3342387 5570645 16711935 16843009 50529027 84215045  
252645135 286331153 858993459 1431655765 4294967295

More generally, paritons, when encoded this way, easily allow to mimic observed spectra, especially EPR (electronic paramagnetic resonance) ones: some specialists (physico-chemists, biologists), who did not know that the spectra resulted from a short *APL* one-liner with no arithmetics . . . – were often able to “identify”the chemical products which could have produced the spectra!

There is another property of the *pariton,* to which Langlet gave the name *helical transform* because a diagonal of the matrix, when it is being viewed as a cylinder, constitutes a helix. He touches briefly on some references connected with characteristics of the main (bottom right to top left) diagonal but is more interested in the diagonal which runs bottom left to top right. He finds \[[5](#ref5) p103\]:

*<span class="indent"> In any
pariton, if any sequence I produces a diagonal D, a sequence equal to D
produces a diagonal which contains I.<br>
</span>* <span class="indent">So D will be named <b>–</b>I from now on (no
confusion can arise from the minus sign because the pariton involves no
arithmetic).<br>
<i>It means that –I is the helix inverse or helix transform of D. . . . <br>
As a corollary, any row of the matrix has its corresponding </i>helix inverse<i>
on the cylinder.</i></span>

Writing just two years before the successful decoding of the human genome, his drawing attention to these helical mechanisms is no accident. Langlet was fully aware of the tension between the theories of physics and the apparently high immunity to entropic degradation of living systems. He points to the asymmetry of the *pariton* and suggests \[[5](#ref5) p108\]:

Maxwell would have been fully satisfied to observe that the pariton does his hypothetical demon’s work. A rapid glance at any pariton shows that the organisation of information is ‘nil’ on the left side and maximum on the right side (the cyclic memory).

 The binary integral mechanism is a *self-associative auto-organiser.*

Langlet is referring to James Clerk Maxwell’s thought experiment, in which he imagined a demon selecting faster among slower molecules and so creating a hot box from the contents of a cooler box in defiance of entropy. Maxwell’s demon is generally thought to have been thwarted by the requirement that it itself must expend energy in obtaining its information about the speed of particles but Ilya Prigogine pointed out that what may apply on average to the universe as a whole and over cosmological time is not true at all scales \[[7](ref7) p175\]:

Often biological order is simply presented as an improbable physical state created by enzymes resembling Maxwell’s demon, enzymes that maintain chemical differences in the system in the same way as the demon maintains temperature and pressure differences. . . .

In the context of the physics of irreversible processes, the results of biology obviously have a different meaning and different implications. We know today that both the biosphere as a whole as well as its components, living or dead, exist in far-from-equilibrium conditions. In this context life, far from being outside the natural order, appears as the supreme expression of the self-organizing processes that occur.

In parts of this early paper (first published in Belgium in 1991) Langlet hints at applications of his calculus which he developed more fully over the five years left to him. *The APL Theory of Human Vision* \[2\] which he presented in Antwerp in 1994 is a good example of his attempts to show how his mathematics could be applied at the molecular level in biochemistry. The references below detail where this and other of Langlet’s work quoted here has been published. It is hoped in due course to make everything that he wrote as easily available.

## References

<a id="ref1"></a>\[1\]   G. Spencer Brown, *Laws of Form*, Geo. Allen & Unwin, 1969, ISBN 04 510028 4

<a id="ref2"></a>\[2\]   Ed. Alain Delmotte, Gérard Langlet, *The APL Theory of Human Vision*, Conference Proceedings Antwerp 1994, APL Quote Quad, Vol.25, No.1, ISBN 0-89791-675-1

<a id="ref3"></a>\[3\]   Cornelius Lanczos, *Numbers Without End*, Oliver & Boyd, Edinburgh, 1968

<a id="ref4"></a>\[4\]   Gérard Langlet, The Axiom Waltz<i>,Vector</i>, Vol.11 No.3, 1995, ISSN 0955-1433

<a id="ref5"></a>\[5\]   Gérard Langlet, Paritons and Cognitons<i>,Vector</i>, Vol.19 No.3, 2003, ISSN 0955-1433

<a id="ref6"></a>\[6\]   Ed. Lynne Shaw, Gérard Langlet, <i>Towards
the Ultimate APL-</i>TOE, ConferenceProceedings St. Petersburg 1992, APL Quote Quad, Vol.23, No.1, ISBN 0-89791-477-5

<a id="ref7"></a>\[7\]   Ilya Prigogine & Isabelle Stengers, *Order out of Chaos*, Fontana Paperbacks, London, 1985, ISBN 0-00-654115-1

***
