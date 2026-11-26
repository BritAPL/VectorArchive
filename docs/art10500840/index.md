---
vid: '10500840'
title: Introducing “Some Topics in Computing”
authors:
- Keith Smillie
byline: by Keith Smillie (smillie@cs.ualberta.ca)
volume: '25'
issue: '3'
page: '34'
online: '2012-04-09'
editor: Kai Jaeger
description: A survey of computing from antiquity to the present using J as an executable mathematical notation and as a programming language.
source: content/printed/253/smillie.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- APL in perspective
- education
---

by Keith Smillie (smillie@cs.ualberta.ca)
{ .byline }

History … interprets the past to understand the present and confront the future… . P. D. James, *The Children of Men.*

## Introduction

On my eightieth birthday – which is now longer ago than I care to admit - a physicist friend gave me quite unexpectedly a copy of Great Ideas in Physics by Alan Lightman[\[1\]](#ref1).. The two aims of the book according to the author who holds appointments in both Humanities and Physics at MIT are “to provide a grasp of the nature of science, and to explore the connection between science and the humanities”.

Using only elementary mathematics not including calculus, Lightman discusses the conservation of energy, the second law of thermodynamics, the theory of relativity, and quantum mechanics, and introduces illustrative excerpts from the writings of Newton, Kelvin and Einstein as well as many other scientists, poets, novelists and philosophers.

I read the book with great pleasure and finished it feeling envious of the students, most of whom were in their first year, who had taken the course from which the book was developed. I particularly remembered the first page of the Introduction which began with a brief account of a visit Lightman had made to the Font-de-Gaume prehistoric cave where the walls are covered with 15,000-year-old Cro-Magnon paintings. After describing one painting of two reindeer, he continues:

> ...The light was dim, and the colors had faded, but I was spellbound. Likewise, I am spellbound by the plays of Shakespeare. And I am spellbound by the second law of thermodynamics. The great ideas in science, like the Cro-Magnon paintings and the plays of Shakespeare, are part of our cultural heritage.

Unfortunately most of our university science courses regardless of the discipline are intended for students majoring or intending to major in the subject or who wish to use the subject as a tool in their own discipline. Very little time is spent on the historical, literary or other cultural aspects of the subjects.

These matters are often treated briefly in panels inserted in the text. There appear to be relatively few introductory courses intended for those persons with modest scientific and mathematical backgrounds wishing some knowledge of a scientific discipline as part of a liberal education. There are certainly very few such courses or books written by specialists which introduce computers and computing as part of our culture to the general reader.

Therefore, encouraged by Lightman’s book I decided to assemble a few chapters dealing with what I consider some of the more important ideas in computing as seen in historical perspective. To avoid having to define computer or computing science and to emphasize the undoubted incompleteness of the work I used the title “Some Topics in Computing”. As with Lightman’s book there is very little mathematics, and certainly no calculus. However, in addition to conventional mathematical notation J is used both as an executable mathematical notation and as a programming language. The first chapter gives a brief introduction to J and further aspects of J are introduced in subsequent chapters as they are needed and in an unobtrusive manner as possible. The Table of Contents is given as an Appendix to this paper, and the chapters are available as Web pages at http://goo.gl/39jq2 [\[2\]](#ref2).

The purpose of this short article is to give a very brief description of the topics presented on the Web in the hope that some readers will be encouraged to make use of some of the material in their own work and may extend it in directions and with emphases which they consider necessary or desirable.

## Topics

The first chapter introduces Kenneth Iverson and APL and then J as a “modern dialect” of APL. J is then illustrated by two short “dialogues”, annotated sessions with the computer, in which some of the J primitive verbs are introduced by means of simple examples, one of which finds the number of grocery items purchased in a shopping trip and the total cost of the items. This allows the introduction of both functional and explicit definition of verbs and a comparison with a BASIC program for the same problem. A final dice-rolling example allows the introduction of the very useful verb table /, and is the first of several dicing examples.

Number systems are the subject of Chapter 0 which begins with the additive number systems of ancient Egypt and Greece and the multiplicative systems of China and Japan. This is followed by a discussion of our positional number system which is illustrated with several recreational examples including the game of Nim and an example from Alice in Wonderland to be mentioned later in this paper. The chapter ends with an extension of the dice-rolling example of the first chapter.

The first two chapters give a somewhat leisurely introduction to J which it is hoped will prepare a reader new to the language for the following chapters on the development of computing. In brief, these chapters discuss the work of John Napier, Charles Babbage, George Boole, Alan Turing, and some of the pioneers in early computing. J is used to provide illustrative examples such as a Windows form for a Difference Engine simulator and a simulator for a Turing machine, and a brief introduction to Boolean algebra. The dice-rolling example appears again in the discussion of programming languages where dice-rolling programs are given in twelve different languages including a machine-language simulator written in J. The final chapter makes some remarks on solvable and unsolvable computational problems and ends with a brief discussion of the on-going work on polynomial- and exponential-time problems and the P vs NP problem.

## Computing and literature

Although we have taken an historical approach to the growth of computers and computing, we must not neglect other branches of the humanities as a source of examples which may illuminate and enrich computing topics. Indeed some of these examples may illustrate that we have been introduced to important concepts in computing and mathematics through books and stories we have known since childhood. In this section we shall give just a few examples from English literature.

We shall begin with number systems, as we did in Chapter 1 of Topics, with the following from Thomas Hardy’s The Trumpet Major which takes place during the Napoleanic Wars as an illustration of the problems presented by the decimal number system to those with a limited formal education:

> Behind the wall door were chalked addition and subtraction sums, many of them originally done wrong, and the figures half rubbed out and corrected, noughts being turned into nines, and ones into twos. These were the miller’s private calculations. There were also chalked in the same place rows and rows of strokes and open palings, representing the calculations of the grinder, who in his youthful ciphering studies had not gotten as far as Arabic figures.

Number systems to a varying base are illustrated at the beginning of Alice in Wonderland as Alice repeats the multiplication table shortly after having fallen down the rabbit hole:

> Let me see; four times five is twelve, and four times six is thirteen, and four times seven is – oh dear! I’ll never reach twenty at that rate!

A marginal note in Martin Gardner’s Annotated Alice[\[3\]](#ref3) refers to an explanation which justifies Alice’s arithmetic since four times five is twelve to base eighteen, four times six is thirteen to base twenty-one, … and four times twelve is nineteen to base thirty-nine. However, four times thirteen is not twenty to base forty-two so Alice does not reach twenty after all.

A character much loved by children is A. A. Milne’s Winnie-the-Pooh who early in the stories illustrates the difference between the exclusive- and inclusive-or:

> Pooh always like a little something at eleven o’clock in the morning, and he was very glad to see Rabbit getting out the plates and jugs; and when Rabbit said, ‘Honey or condensed milk with your bread?’ he was so excited that he said ‘Both,’ and then, so as not to seem greedy, he added, ‘But don’t bother about the bread, please.’

An excellent illustration of the indirect method of proof is given when Pooh is looking for companions to join himself and Christopher Robin in an Expedition to the North Pole. He meets Rabbit first and says:

> ‘Hallo, Rabbit,’ he said, ‘is that you?’
>
> ‘Let’s pretend it isn’t,’ said Rabbit, ‘and see what happens.’
>
> (Hello, Rabbit, is the number of primes infinite? ...)

Charles Dickens, who knew Babbage well, got some of his ideas in Little Dorrit for the Circumlocution Office, the government office that did nothing, from Babbage’s experiences with the government and modelled Daniel Doyce in part on Babbage.

Arrays with one or more dimensions of length zero occur in Douglas Adams’s The Hitch Hikers Guide to the Galaxy: “A hole had just appeared in the galaxy. It was exactly a nothingth of a second long, a nothingth of an inch wide, and quite a lot of millions of light years from end to end.”

The problems associated with recursive processes and their termination are illustrated in Arthur C. Clarke’s “The Longest Science-Fiction Story Ever Told” which tells of an editor who writes a letter which ends by referring to itself and thus starts over again and again … .

Many more examples could be cited but it is hoped that these few may suffice to show how fiction can provide a source of examples to illustrates important ideas in computing, and indeed in other fields of science. Moreover such examples in themselves may encourage the science student to put aside, if only briefly, the prescribed science texts and turn to literature for relaxation and renewal. The rewards of such actions are beautifully expressed in the Introduction to one of the several editions of Mrs. Gaskell’s Cranford:

> … in that pleasantest hour of all where the toils of the day are over and business cares are forgotten, the last hour of the day before retiring, one reads again one chapter; or forgetful of the morrow one reads on along the peaceful river of gently flowing prose, of effortless charm, and tranquil truth.

## References

1. <a id="ref1"></a> Lightman, Alan, 2000. Great Ideas in Physics. McGraw-Hill, New York.
2. <a id="ref2"></a> https://webdocs.cs.ualberta.ca/~smillie/RevisedTopics/TopicsRev.html
3. <a id="ref3"></a> Gardner, Martin, 1960. The Annotated Alice. Bramhall House, New York.
4. <a id="ref4"></a> Smillie, Keith, 1986. “Language, literature and the computer.” The Creating Word, P. Demers (ed.). The University of Alberta Press, Edmonton, Alberta.

## Appendix: Table of Contents

0. Introduction
1. Kenneth Iverson, APL and J
    - Development of APL and J
    - A very short dialogue with J
    - A longer dialogue with J
    - Grocery shopping again; Summary
    - Throwing dice
2. Positional Number Systems
    - Introduction
    - Additive systems
    - Multiplicative systems
    - Arithmetic tables
    - Positional systems
    - Guessing numbers
    - The binary clock
    - Down the rabbit-hole
    - The game of Nim
    - A genealogical problem
    - A closer look at multiplication
    - Rolling more dice
3. John Napier and Logarithms
    - Introduction
    - John Napier of Merchiston
    - Napier’s logarithms
    - Further development of logarithm tables
    - Napier’s rods
    - Slide rules
4. Charles Babbage and his Engines
    - Charles Babbage
    - The method of differences
    - The Difference Engine
    - A simulator for the Difference Engine
    - The Analytical Engine
    - Other Difference and Analytical Engines
    - Prime numbers and coffee tables
5. George Boole and Logical Design
    - Aristotelian logic
    - George Boole
    - Boolean algebra
    - Truth tables
    - Binary addition
6. Alan Turing and Computability
    - Mathematical foundations
    - Early life and education
    - Bletchley Park
    - National Physical Laboratory and Manchester
    - Turing Machines
    - Computability
    - A play and a novel
    - A Turing Machine simulator
7. Early Computers
    - Introduction
    - Electromechanical computers I. Konrad Zuse
    - Electromechanical computers II Howard Aiken
    - Eletromechanical computers III. The Bell Telephone computers
    - Electronic computers I. ENIAC and EDVAC
    - Electronic computers II. NPL, Manchester and FERUT
    - Electronic computers III. Cambridge and EDSAC
    - Nim-playing computers; A machine-language simulator
8. FORTRAN and Some Other Languages
    - Introduction
    - Before FORTRAN; FORTRAN; BASIC; ALGOL; Pascal; C, Java and Perl; MATLAB
    - Spreadsheets
    - Backus-Naur Form
    - Acknowledgements
    - Appendix - Example programs for dice frequencies
9. Some Overwhelming Numbers
    - Introduction
    - Bubble sort
    - A couple of legends
    - Some problems from the real world
    - A few notes on cryptography
    - Public key encryption
    - P vs NP
    - J programs
10. Appendix. J4.06 Script File
