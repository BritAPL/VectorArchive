---
vid: '10500510'
title: A commentary on the formulator
authors:
- Neville Holmes
byline: by Neville Holmes (neville.holmes@utas.edu.au)
volume: '24'
issue: '4'
page: '72'
received: '2009-11-01'
online: '2010-12-06'
editor: John Jacob
description: More on the author’s scheme for a commodity calculator
abstract: A design for a commodity device called a formulator was outlined in a recently published essay.[1] Just as the calculator extended the abacus beyond basic arithmetic, the formulator is designed to extend commodity computation beyond arithmetic to algebra. This article examines aspects of the proposed formulator in more detail than given in the essay, and relates its capabilities to those of APL/J interpreters.[2]
source: content/printed/244/holmes.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- language design
- APL in perspective
---

by Neville Holmes (neville.holmes@utas.edu.au)
{ .byline }

A design for a commodity device called a *formulator* was outlined in a recently published essay.[\[1\]](#ref1) Just as the calculator extended the abacus beyond basic arithmetic, the formulator is designed to extend commodity computation beyond arithmetic to algebra.
{ .abstract }

This article examines aspects of the proposed formulator in more detail than given in the essay, and relates its capabilities to those of APL/J interpreters.[\[2\]](#ref2)
{ .abstract }

## The formulator

Calculators are widely familiar devices that are used to do basic arithmetic. They come in various forms but the simplest, the commodity calculator, is very cheap and apparently simple and reliable. The commodity calculator is also emulated on other handheld devices.

One of the potentially most important uses of calculators is in the teaching of arithmetic in early schooling. They are commonly used for simple quantitative problem solving but could also be adapted easily to teaching basic arithmetic skills.

What is there that goes a bit beyond calculator technology, either in daily life or in later education? Well, there are spreadsheets, but they are more a data management tool than a mathematical tool.[\[3\]](#ref3)

Interpreters like those for APL varieties are of course genuinely and ardently mathematical tools, but they go way beyond what is needed for everyday or school use. This makes them daunting for beginners and dabblers.

What is needed is an intermediary capability, one that combines versatility and simplicity. The formulator[\[1\]](#ref1) has these properties and also has enough in common with APLs that it would make it easier for users to move up.

## The exact calculator

The first step in developing a formulator design is to fix the problems of the commodity calculator. After all, these problems are arguably the reason behind the banning of calculators from primary schooling in countries such as Japan.

There are several significant problems, and these and fixes for them are given in my essay “Truth and Clarity in Arithmetic”[\[4\]](#ref4) in more detail than is appropriate here.

The source of falsehood in the commodity calculator is the adoption of inexact arithmetic pretending to be exact. Exact arithmetic is the cure.

Two measures underpin exactitude: computation with variable length numbers and the use of fractions with a non-decimal denominator.

To make variable length numbers practical, control over the entry and display of such numbers is necessary. For example, scaling is needed on numbers both being keyed in and being displayed. Very long numbers need to be able to abbreviated, but their inexactitude must be clearly shown when this is done.[\[5\]](#ref5)

To make fractions of all kinds practical, numbers with non-decimal denominators must be distinctively represented through use of both a decimal point and a fraction point.[\[6\]](#ref6)

To complete the design of an exact calculator, a variety of very simple functions needs to be added to the miserly ‘normal’ quota. At the very least, quotient and remainder functions are absolutely necessary, both for practical and educational use.

At the same time, inexact values such as multiples of π and inexact functions such as fractional powers need to be excluded, though there are ways that π could be handled symbolically. Simple multiples of π and roots of integers, however, can be approximated by fractions.[\[7\]](#ref7)

One important source of obscurity in the commodity calculator is the disappearance of the numbers being used.

Calculators work on more than one number at a time. Typically there are three numbers immediately relevant to the user of a calculator: two arguments and a result, not to mention the symbol of the function yielding the result. All should be displayed to the user, who should also be able to manipulate them.[\[8\]](#ref8)

## Towards the formulator

The commodity calculator is used to fiddle with numbers one at a time, constructing them by editing and by applying the basic functions provided.

The commodity formulator is designed to be used to fiddle also with functions, constructing them by editing and substitution. Of course the functions are used for numeric calculation, but the focus is on manipulating the functions, that is, on algebraic calculation.

The formulator is necessarily more complex than the calculator. It needs a richer keyboard and character set and more powerful basic functions.

The exact calculator serves as a very appropriate starting point. The published design assumed a conventional numeric keyboard with a sign symbol on either side of the zero key, and a column of four function keys.[\[4\]](#ref4) The signs were used as prefixes to the function keys to modify the four straight functions.

With the larger keyboard needed for the formulator, the function keys could be completely independent of the numeric keys, so that better symbolism could be used.[\[9\]](#ref9)

It is not appropriate here to consider in any detail just what the formulator keyboard should be like, just what basic functions should be provided, and just what symbols should be used. Rather the skeletal structure of the formulator is the focus. However, Ken Iverson’s Turing Award essay[\[10\]](#ref10) is an excellent guide for designers of notation, and the original mathematical symbols should be practical by now.

## Truth and inexact arithmetic

Overtly putting inexact arithmetic onto the exact calculator has two aspects; the use of numbers and the use of functions.

The numbers are entered, edited, and displayed.

The user needs to be able to key numbers in as inexact and, in case of need, to specify just how inexact. Similarly, the user needs to easily see whether the number is exact as shown, or exact but shown inexactly, or inexact with all of the exact part shown, or inexact with the exact part going beyond the end of the number shown.[\[11\]](#ref11)

The functions are selected, modified, and applied.

The basic traditional arithmetic function symbols, such as the saltire × and the obelus ÷, are used, as they are in APL.[\[12\]](#ref12) Variations on those functions are denoted by diacritics (though not shown thus here as it seems such diacritics are not provided by HTML/Unicode[\[13\]](#ref13)), for example the tilde signifies argument commutation: <code><sup>~</sup>÷</code> for *divide into*. Numbers – positive, zero, or negative – are used as diacritics or superscripts of a function to have it repeatedly applied, or ignored, or repeatedly inverted.[\[14\]](#ref14)

With inexact arithmetic come various basic functions, such as the trigonometric ones, that normally yield inexact results. Inversion functions, like the square root, even yield more than one result, and, even though it’s popular to ignore all but one, it’s more strict and informative, especially in mathematical education, to produce them all as a set of results.[\[15\]](#ref15) The root function also produces complex numbers, so they need to be handled alongside other kinds of value.[\[16\]](#ref16)

## Breadth and list processing

Adding inexact to exact arithmetic broadens the calculator without changing its basic nature as a calculator that applies functions one at a time to one or two arguments. But it does make it rather lopsided with a richness of calculation that involves producing one value from at most two values. The only argument widening described so far is the ability to use a set of numbers as a single value.

The logical next step is therefore to allow lists of values as arguments and results.[\[17\]](#ref17) Again this step has two aspects: handling list arguments and providing structural functions to derive values from such arguments.

A list argument is simply a sequence of numbers separated by spaces.[\[18\]](#ref18) Each item in the list, and each number in a set item, has its exactitude independent of other items or numbers in the list or set.

Because a numeric display of a list could be lengthy, the list calculator offers a variety of ways to view a list as a graph.[\[19\]](#ref19) List arguments can be produced by editing – selecting, joining, truncating, or otherwise fiddling with lists already in the argument stack.

Dyadic functions handle argument lengths as in APL/J, except that a single item list is treated as a scalar. However, a diacritical diple (`<` or `>`) can be used to point to the argument whose items are to be dribbled in so that items of the result are sets of the result from the entire other argument. Used monadically, a function with a diple is reflexive so that the result is the same whichever diple is used.

Much of what is done structurally by functions in APL/J can be done by editing commands in a commodity list calculator.[\[20\]](#ref20) Quite otherwise are those functions that do arithmetic dyadically within the list, collapsing subsequences to single items in various ways. The operators of APL/J are used here as the diacritical modifiers *acute* ´ and *grave* \`, corresponding to the two solidi / and \\.[\[21\]](#ref21)

## Clarity and formulae

The list calculator couples a richness of arithmetic with provision for its application to lists of numbers. The arithmetic functions can be applied by the user to the extent needed and understood, and the lists can be entered, inspected, changed and selected as needed.

The richness has merely been suggested here, with some discussion in the appended notes. Users of APL/J will not be at a loss to fill in the detail in various ways.

However, the design is still of a calculator. The functions are applied one at a time. The next two stages are very simple, but they are the essence of the formulator and the basis for algebraic clarity.

The formulator has a function stack in addition to, and separate from, the argument stack. Just as for the calculator, one or two arguments are set up for the next function to use. The next function is either keyed into the bottom slot in the stack or chosen from the function stack and possibly edited before being used.

A function in the stack can be either a basic function, with or without modifiers, or a composite of such functions, or a train of functions.

A simple composite function is like an integer – notationally juxtaposed functions that build their results up progressively from right to left, from least significant to most significant. Thus only the rightmost function has a choice of being used monadically or dyadically; the others are all monadic since they get their argument from the function to their right. So `÷-` is the reciprocal of either the negation (monadic) or the difference (dyadic) of the argument(s).

However a composite function can also have a dyadic point just like the decimal point of a number. Dyadically, the basic functions to the right of the dyadic point are applied monadically to each argument and the two results become the arguments for the functions to the left. Monadically, the basic functions to the right are applied similarly to produce the second argument at the dyadic point, while the original argument becomes the first argument at the point. So <code>-<span class="small"><sub>∆</sub></span>÷</code> is the difference of either the reciprocal of two arguments or of the single argument and its reciprocal.[\[22\]](#ref22)

A train of functions is like a list of numbers – a sequence of basic or composite functions separated by spaces. A train serves to repeatedly use its argument(s) while passing results from right to left along the train and combining those results arithmetically with the original argument(s) on the way.

The first, third, and so on functions from the right in a train of odd length are fed the argument(s) of the train as a whole and their results are fed to the intervening dyadic function. A train of even length is in effect made of odd length by prefixing an identity function, <code>+<sup>0</sup></code> say.[\[23\]](#ref23)

Parentheses may be used within formulae for various effects. For example, modifiers can be applied to an enclosing parenthesis to affect the enclosed function, and a train within parentheses can be used within a composite function.

## Depth and algebra

Composing formulae is the basic level of algebra. The ability to work with patterns of formulae is the next level, and is needed to give depth to the formulator.

A formula pattern or metaformula or template is a functional expression with place holders for substitution of meta-arguments. Templates are keyed in or edited in a template stack alongside the argument and formula stacks.[\[24\]](#ref24)

Once a template is selected for use, functions are selected in the function stack or are keyed in and they replace corresponding place holders in the template wherever they occur and as a single function.[\[25\]](#ref25) When replacement is complete the resulting function is placed in the function stack whence it can be used on selected arguments.

## Rationale

The formulator is not intended to compete with programming systems such the APL/J family. Indeed it is purposefully and drastically simpler, and not only because of the absence of name assignment and the restriction to list arguments.[\[26\]](#ref26)

Rather, the formulator is intended for everyday general use in work and in education – even in the home. In education it opens a wide field beyond simple arithmetic, the arithmetic that the commodity calculator has so badly betrayed, so that it has the potential to support teachers in greatly improving the development of numeracy in their students.

The design of the formulator embodies a contrast between computation and programming, a contrast I pointed out in a published essay on APL of more than thirty years ago.[\[27\]](#ref27) The formulator uses the symbolism of APL in rejecting use of the Roman alphabet, but ideas like trains come from J.

Advanced programming systems like the APL/J family present a forbidding prospect to anyone simply wanting to do some particular ad-hoc calculation. Arguably, this prospect explains why spreadsheets became so much more popular than APL.

Adoption of the formulator would provide an intermediate step between school arithmetic and APL/J systems that would make it easier for people to shift up.[\[28\]](#ref28) Implementation of the formulator should be a relatively easy adaptation of an APL/J interpreter, and it could find prompt use on various handheld digital devices.[\[29\]](#ref29)

The APL/J community would be well placed to produce for teachers and students the material needed for teaching and learning the use of the formulator.[\[30\]](#ref30) Properly done, and with conversion packages that bring formulator stacks into APL/J systems, these could lead to the much wider use of the programming systems favoured by the readers of *Vector*.

## Notes and references

1. <a id="ref1"></a> Holmes, W.N. (2009) “Truth and Breadth, Clarity and Depth in Algebra”, *Computer*, 42(11), 112, 110-111 (PDF in archive: [eprints.utas.edu.au/9474](http://eprints.utas.edu.au/9474))

2. <a id="ref2"></a> This article is named a commentary. Its body is meant to be a fairly straightforward description of the design of the formulator slanted to readers familiar with the APL family of systems. Citations and further comments are relegated to this trailing section so that a first reading need not be distracted by comments, speculations and other kinds of rant. The hyperlinks are also in this section for convenience of use in the online version of this article. Any reader who thinks my use of footnotes excessive should for perspective read Anthony Grafton’s delightful book, *The Footnote: A Curious History*, (Harvard University Press, 1997, ISBN 13: 978-0-674-90215-2; see [www.hup.harvard.edu/catalog/grafoo.html](http://www.hup.harvard.edu/catalog/grafoo.html)).

3. <a id="ref3"></a> A spreadsheet program I used through an IBM 2741 typewriter terminal in the early ’70s was written in APL\\360. The author told me that he did it for an insurance firm in New England (USA, not NSW) and that spreadsheets were very widely used in the insurance industry, though many actuaries in Australia used APL bare. Interestingly, this was well before VisiCalc ([bricklin.com/history/saiidea.htm](http://bricklin.com/history/saiidea.htm)).

4. <a id="ref4"></a> Holmes, W.N. (2003) “Truth and Clarity in Arithmetic”, *Computer*, 36(2), 108, 106-107 (PDF in archive: [eprints.utas.edu.au/1573](http://eprints.utas.edu.au/1573))

5. <a id="ref5"></a> Numbers need to be shown in their shortest form, for example by choosing between a decimal and a vulgar fraction. Long numbers can be shortened on display by scaling, using an italic scale to show that an exact value is available to the user. Also, it might be worthwhile to use the traditional integer superscript to enter and display exact powers, at least as an option.

6. <a id="ref6"></a> The so-called decimal point separating the integral part of a number from its fractional part needs to be keyed in even for vulgar fractional parts like two thirds. However, simple fractional parts could be displayed with a solidus and without the decimal point, for example: 2¾.

7. <a id="ref7"></a> Better still, exact functions that do for exponentiation what quotient and remainder do for division would be a very interesting capability that would allow exact approximations for inexact roots and powers to be investigated. Two pairs of functions would be needed. With, say, 2 and 12 as arguments, one pair would yield 3 and 1½ as results, the other 2 and 1<sup>1</sup>/<sub>3</sub>. (Why, oh why, doesn’t HTML/Unicode provide `&frac13;`?)

8. <a id="ref8"></a> The three numbers on display straight after tapping a function key would best be displayed like

    ```
      12
    × 34
     –––
     408
     ===
    ```

    which is the traditional format used in teaching elementary arithmetic. This is in effect a stack and the user would key in the next number as the second argument for a dyadic function. The stack could be extensive so that the user can select numbers previously stacked for editing and use as a new argument.

9. <a id="ref9"></a> Using the two special point symbols as basic function modifiers in the exact commodity calculator was motivated by the idea of imitating the ordinary commodity calculator. Otherwise, clarity is greatest if the numeric symbols are all distinct from the function symbols. A shift key to switch between the two sets, for instance with a touch pad on a screen, would keep the keyboard simple.

10. <a id="ref10"></a> Iverson, K.E. (1980) “Notation as a Tool of Thought”, *CACM*, 23(8), 444-465 (HTML in the Web: [jsoftware.com/papers/tot.htm](http://jsoftware.com/papers/tot.htm))

11. <a id="ref11"></a> While the distinctions can be made typographically, the user needs control over display of the number so that the entirety of a long number is accessible.

12. <a id="ref12"></a> For clarity, all simple or primitive functions should have a distinct non-alphabetic symbol in the spirit of “Notation as a Tool of Thought”, though Greek letters are used in mathematics and engineering as though they aren’t alphabetic.

13. <a id="ref13"></a> Tragically, present day typographical encoding doesn’t allow the freedom to put diacritics over any character of choice, a trivial matter in Donald Knuth’s old-time T<small>E</small>X (see [tug.org](http://tug.org)). For rants on this topic, see my archived essays on general ([eprints.utas.edu.au/2005](http://eprints.utas.edu.au/2005)) and sortemic ([eprints.utas.edu.au/1564](http://eprints.utas.edu.au/1564)) text encoding, on Unicode ([eprints.utas.edu.au/1527](http://eprints.utas.edu.au/1527)), and more recently on cultural theft ([eprints.utas.edu.au/1806](http://eprints.utas.edu.au/1806)). In the original essay on the formulator, getting the production artist to put the diacritics above the function symbols was so protracted that changes I would have liked to make to the draft essay went by the board. For example, I was interested in the effect of using special encoding for the non-decimal values of dates, times, and angles (a date can be added to a time but not to anything else) though the examples were both distracting and inconsistent.

14. <a id="ref14"></a> In the calculator with both exact and inexact arithmetic, literal numbers can be used as superscripts also to numbers, typically as a short way of representing a longish number. However, since in neither the calculator nor the formulator are arguments specified symbolically, such superscripts aren’t seen as specifying calculation, merely representation. In function modification, an interesting notational simplification is to use a numeric subscript to mean the same as its negated superscript. A subscript of one then signifies inversion. A zero superscript can be for the identity function for its left or only argument, and a zero subscript for its right or only subscript.

15. <a id="ref15"></a> A set of values is clearly and conveniently represented as such by enclosing them, systematically sequenced and without duplication, within parentheses. The empty set `( )` is particularly useful for missing values in a sequence. In respect of inexact values, this idea raises the question of whether intervals, which are sets and are also what inexact values strictly are, should be the way inexact values are handled within a formulator. See  [en.wikipedia.org/wiki/interval\_arithmetic](http://en.wikipedia.org/wiki/interval_arithmetic) .

16. <a id="ref16"></a> The representation of complex values presents quite a challenge, especially since each component can independently have the different kinds of exactitude and inexactitude. The traditional mathematical convention of expressing a complex value as a sum is clearly flawed and confusing to learners. An imaginary point like the decimal and fraction points is needed, but using `j` as in J is inconsistent. The best prefix point for the imaginary part of a complex number is surely the `$` symbol. Another aspect of roots in inexact arithmetic is the problem of representation of multiple determinate or even indeterminate angles but determinate magnitude, which implies that, at least as an alternative, a polar representation using the traditional ∠ symbol should be provided.

17. <a id="ref17"></a> It might be argued that tables of values and value arrays of higher rank should be introduced in the same step. However, for ad-hoc calculation or algebration this goes much too far. Apart from the problem of displaying and editing such arrays on a small screen, the extra basic functions needed for dealing with them are a severe complication. In any case, much of what is needed of tables can be handled by using a list of sets.

18. <a id="ref18"></a> A notational abbreviation for simple sequences would be useful for both entry and display of arithmetic progressions. For example, `5[6]78` would go up from `5` in steps of `6` but not go beyond `78`.

19. <a id="ref19"></a> The graph of a list would usually show the value or values of each item vertically in item sequence along the horizontal axis. If complex values are involved, options are given to graph the real or imaginary part, the magnitude or the angle, or the values plotted on the complex plane and connected by lines showing their sequence.

20. <a id="ref20"></a> For an ad-hoc or learner user, it’s easier and clearer for joins, merging, sequencing, rotations, reflections, substitutions, and the like to be done by editing within the argument stack because they don’t involve any arithmetic. Keeping such functions separate also simplifies the function set by keeping it for those that do work on values.

21. <a id="ref21"></a> Clearly the formulator needs a keyboard that provides directly and systematically for the kind of typographical versatility that traditional mathematical notation has exploited to great effect. The American Mathematical Society has built its own mathematical text formatter, AMS-LaTeX ([ams.org/tex/amslatex.html](http://ams.org/tex/amslatex.html)), on top of Donald Knuth’s T<small>E</small>X, but the formulator doesn’t need to be so sophisticated. Nonetheless, the shortcomings of HTML/Unicode present a most unfortunate barrier.

22. <a id="ref22"></a> This monadic use of a pointed composite function was not described in the original formulator essay, but it adds a very simple and useful capability, in particular that of using more than one dyadic point in a composite function.

23. <a id="ref23"></a> Functionally, this is just the same as the trains of J, but in the formulator the items of the train must be spaced out with blanks.

24. <a id="ref24"></a> In the original formulator essay[\[1\]](#ref1) the templates were described as sharing the formula stack. Second thoughts suggest that it would be simpler in operation to keep templates separated from formulae.

25. <a id="ref25"></a> Having two distinct place holders is simple and consistent. However, it is appropriate not to handle templates in quite the same manner as functions in their stack, because replacement needs to be done either from the function stack or directly from the keyboard. Note also that a place holder in a template stands for a single function, so that selecting a train for placing into a template will in effect enclose it in parentheses. Furthermore, a number could be used as a function within a template, or for giving to a place holder, by using a diacritic to convert it to a function that returns the value itself regardless of any arguments it might be given during evaluation of the resulting function.

26. <a id="ref26"></a> Of course, it might be useful to be able to name a trio of stacks when saving them for later restoration or for use in documents, but it would be better to use a generated name that could be changed outside the host formulator. This would keep the alphabet out of the way.

27. <a id="ref27"></a> Holmes, W.N. (1978) “Is APL a Programming Language?”, *The Computer Journal*, 21(2), 128-131 (PDF in the Web:  [comjnl.oxfordjournals.org/cgi/content/abstract/21/2/128](http://comjnl.oxfordjournals.org/cgi/content/abstract/21/2/128))

28. <a id="ref28"></a> An important aspect of the formulator, in particular in easing transition to the APL/J family, is the restriction of formulae to compositions and trains. This inculcates the idea of results passing from right to left between functions, and postpones any questions about the traditional function precedence as seen in expressions like `2×3+4`, but given up in APL/J.

29. <a id="ref29"></a> In a tacit programming thread on a J programming forum it was suggested that the ‘WolframAlpha computational knowledge engine’ ([wolframalpha.com](http://wolframalpha.com)) would provide a formulator. However I had a look at it and it seemed to be very complex and in a completely contrasting style to the formulator described here.

30. <a id="ref30"></a> J users will have noticed that I use the traditional APL terminology, viz., function/argument and operator/operand, rather than J’s noun, verb, &c. I find it simpler. However, for teaching simplicity there is in the formulator a J-like possibility that would call basic function symbols *letters*, compositions *words*, and trains *phrases*. This linking of numeracy and literacy could help teachers.
