---
vid: '10001240'
title: The Language, the Mind and the Man
authors:
- Fred Brooks
byline: by Fred Brooks
volume: '22'
issue: '3'
page: '72'
source: trad/v223/fb223.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- APL in perspective
- language design
---

[by Fred Brooks](http://en.wikipedia.org/wiki/Fred_Brooks)
{ .byline }

## Introduction

I was probably Ken’s first student, because officially [Howard Aiken](http://en.wikipedia.org/wiki/Howard_Aiken) was my thesis supervisor, but *de facto*, Ken had more influence I think than Aiken. I arrived at Harvard in the fall of 1953 and Ken was finishing his dissertation that year, and I’ll remark on the dissertation later, because it was an important piece of work in its own right.

Towards every afternoon at five o’clock if Aiken was in town, everybody in the Comp Lab gathered in the machine room for coffee and conversation with and around the boss. Towards spring the boss turned to Ken, who was finishing and would start a faculty appointment in the fall, and said “I want you to put together a course on the business applications of computers, for teaching in the fall.” Well nobody had ever taught a course like that, anywhere in the world. And so immediately I said to Ken, “May I be your teaching assistant?” because I had been especially interested in the business application of all kinds of primitive machines before I ever encountered computers. He said Yes. Well the Comp Lab was a small place and this meant that Ken and I were put together in the same office starting that summer and for the next two years. So consequently most of my professional development was managed by Ken.

I want to talk tonight about the language, the mind and the man.

## The APL Language

APL came about in this way. We had taught the course the first year. When we were getting ready to do the second year we felt like we had it under control and started writing material which later became the book *Automatic Data Processing*. Ken started trying to analyze sorts. This was before [Don Knuth](http://en.wikipedia.org/wiki/Don_Knuth)’s really great work on sorts had appeared and in the process Ken, being a mathematician, said he was going to use a mathematical notation for analysing sorting algorithms. He came to the conclusion that there were many different notations but they were inconsistent. Ken set about then to unify the mathematical notations. We quickly encountered the problem that, whereas in ordinary algebra the letters are names and the values are numbers, as soon as one starts doing business data processing, the values start becoming letters, names, all kinds of character strings, and in machines in which you’re dealing with numeric addresses, the numbers are names. All of this had to be generalised and solidified. That’s how APL started: strictly as a thinking language, a way of getting your mind around these applications and mathematising what had been done before with words. The *Automatic Data Processing* book got fatter and fatter, and fatter and fatter, and eventually we sent it off to John Wiley. They sent it out to reviewers, one of whom was Bob Ashenhurst, and two of the reviewers, including Bob, came back with a recommendation to cut it into two books. So we did that and it became the *A Programming Language* book and the *Automatic Data Processing* book.

I remember two incidents in the development of APL especially. A lot of what happened at Harvard happened at night after work. As soon as Aiken finished with coffee and went home, then the graduate students and the junior faculty started real work, and we worked and then went to supper somewhat later. One night we were wrestling with the question of: do you really want a floor function or do you really want a ceiling function, because each mathematical notation we found had only one of these and some had one and some had the other. We kept encountering cases where we needed first one then the other in the same treatment. As the night wore on we said, “Let’s ask John Wiley to take a square bracket and cut off one serif at the top and one serif at the bottom and make us two new characters.” Originally we used them as parentheses as well, but Ken recognised that was inconsistent and switched to using them in a monadic fashion.

The other was the question of how APL got its name. One night we wrestled with names. I think it was two hours we wrestled with names. Finally we were tired and we were hungry and one of us, and I don’t know which, said, “Why don’t we just call it *A Programming Language*?” And so that’s how APL came to be known as A Programming Language. It was in desperation after all the other names had failed.

Some years later I asked Ken, I think it may have been at the Association for Computing Machinery ACM History of Programming Languages conference, “what is the touchstone to making an elegant programming language?” He said, <q>“The secret is, it has to do what you expect it to do.”</q> And if you stop and think about APL, and if you think about J, and if you think about Ken’s work generally, it is that high degree of consistency, which is the product of an exceptionally clear mind and a fierce determination not to invent any new constructs until you have to. So many of the arguments I’m sure you all engaged in, when someone came in and wanted to put so-and-so into the language – Ken would say, “Yes that’s an interesting idea” and a little while later he would say, “You can do it this way using the tools and components already at hand”. And again and again that was the solution: it’s already in the language, it’s just a question of being a little more ingenious in the way in which one approaches things.

The other story I want to tell about the language is that Ken didn’t get tenure at Harvard. He did his five years as as an assistant professor and the faculty decided not to put him up for promotion. I asked him what went wrong and he said, “Well the Dean called me in and he said, ‘The trouble is you haven’t published anything but one little book.’ ” That one little book later got the Turing Award. So I think that says more about academic procedure and Harvard than it does about Ken.

## Ken’s Mind

I never saw anybody who thought as clearly. Again and again, he would get down to fundamentals. Again and again he would teach and explain, or even wrestle verbally, until clarity emerged from his very articulation..

His dissertation was the development of a program for doing the input-output model of Harvard economics professor [Wassily Leontief](http://en.wikipedia.org/wiki/Wassily_Leontief). It used a floating vector notation that had one exponent and then a whole set of mantissas in the vector, which worked well on the Mark IV, and had an advantage that depended on the mathematical fact that you’re going to pivot your matrices anyway. And Leontief’s matrices were big: fifteen hundred by a thousand. Doing these on the Mark IV, which had 230 words of core memory and a thousand words of drum, and space for ten thousand instructions, was an accomplishment indeed. Leontief later got one of the early Nobel Prizes in economics for the work for which Ken provided the mathematical tools for the computation. The concepts were in fact Leontief’s, of course.

The clarity and the consistency of Ken’s thinking was, I think, very important. For me personally, the biggest contribution to my life was that Ken taught me to write, and he wanted things very concise. <q>When in doubt, leave it out. Do not put runways for your prose to take off or to land at the end of a chapter. When you’re finished, quit.</q> As a matter of fact, the first chapter of *Automatic Data Processing* is far and away the densest because we edited it, and we edited it, and we edited it. Ken taught me some very useful productions. <q>If it’s a clause, turn it into a phrase. If it’s a phrase, turn it into an adjective or an adverb. If it’s an adjective or an adverb, omit it.” And you apply these recursively.</q> In fact one of the things that hampered the adoption of *Automatic Data Processing* is the first chapter got over-concisified and students found it very difficult to get through the mathematical background, which is expressed with great clarity but also great conciseness in the first chapter – and they never got to the easy chapters.

Let me share with you one page of pure Iversonian writing as an example of the kind of clarity. This is from the chapter on translators. The chapter has already dealt with assemblers, with generators and other kinds of translators, and now we come to section 8.6 on compilers.

> A compiler is the most general type of translator. It differs from the assembler in possessing the following facilities to make the argument language easier to use.
>
> 1. Each elementary operation of the argument language may correspond to a multi-statement program, a subroutine in the function language. This permits the definition and use of a virtually arbitrary set of operations in the argument language. Functions such as sine(*x*) and log10(*x*) for example can be incorporated as the elementary operations `sine` and `log`.
> 2. The individual statements of the argument program may be compound in the elementary operations of the argument language. The programmer may use directly a compound algebraic statement such as Z is assigned to the value of u(*v*)…. The burden of analysing this into its elementary components is assumed by the compiler.
> 3. The representation used for the operands is not constrained to a single pattern but may vary widely. Consequently the argument language must include declarative statements or conventions to specify the representation of the variables. Each imperative statement refers to each operand by a single name. The particulars of the representations are automatically considered in the analysis of the statement. Furthermore, separation of the representation from the processing descriptions permits easier modification of each.
> 4. The imperative statements of the argument programs are not constrained to be independent or independently translatable. The type of interdependence is usually limited. Certain control statements specify the sequence of execution of other statements, the values of certain parameters of the formats of their operands. The scope of a control statement may include other control statements. This facility simplifies the writing of loops.
> 5. Since argument languages become more complex as they become more powerful, compilers typically include elaborate diagnostics, which bring real or apparent syntactic and schematic mistakes to the programmer’s attention.

Now, just for sheer clarity, elegance and generality, think about each one of those things and all the cases that it covers of the distinctions between a compiler and the simpler classes of translators. I think that shows such a clean mind.

The third thing I want to talk about is the man. I found him exceptionally, unselfishly helpful and giving of himself. If a box needed to be toted, he was ready to take one end of it. If a car needed to be started, well let’s run out and start it.

He taught me to take a nap every day after lunch. We need to put our heads down on the desk and zonk out and I’ve been doing that ever since graduate school and it’s one of the most useful things anybody ever taught me. I just stretch out on the rug in my office now. My secretary’s instructed to say, “He’s out”, which is accurate.

Ken’s manner was always honest and straightforward. I think also he was very brave. In World War II he left the Home Guard and transferred to the Royal Canadian Air Force because he wanted to be nearer where the action was and the Home Guard could not be assigned overseas. And Ken stood up to Howard Aiken, and Aiken stood six feet three and had Spockean ears and pointed tufts of hair and when he rared up and looked down at you, you thought you were looking at the Devil. And when he growled at you, you thought that too. Never mind: Aiken had the characteristic that if you stood up to him he respected that, and if you didn’t he would run you down, just tread you underfoot. Ken stood up to him. Ken’s manner was always argumentative, but argumentative in what I thought was an effective expression. “Perhaps one ought to think about it this way...” This was in contrast with Aiken’s “Goddamit it’s gotta be so-and-so, all right?” Ken was firm in his views but he was reasonable. I think he satisfied the old Latin motto that said, never uncertain, always open. *Nunquam incertus, semper apertus.*

These characteristics made him a very warm friend and a person one had to admire as a mentor, a person from whom one learned so much and the reason why we named our eldest son for him. A very great man and we shall miss his conversations; a very great man, and we shall miss his character.

## References

1. Iverson, K. E., *A Programming Language*, New York: Wiley, 1962, 286 pp.
2. Brooks, Jr., F. P., K. E. Iverson. *Automatic Data Processing*, New York: Wiley, 1963, 494 pp.; System/360 Edition, 1969, 466 pp.
