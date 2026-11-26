---
vid: '10003480'
title: Loops in APL2
authors:
- H. Haussmann
byline: by H. Haussmann, Universität Hohenheim. (haussman@ds0ruh11.bitnet)
volume: '10'
issue: '3'
page: '106'
source: trad/v103/haussmann103_106.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- language design
- programming techniques
---

by H. Haussmann, <span class="affil">Universität Hohenheim.</span> <span class="email">(<a href="mailto:haussman@ds0ruh11.bitnet">haussman@ds0ruh11.bitnet</a>)</span>
{ .byline }

I have been using APL for decades and I have been handling vectors and matrices for centuries! Of course the last statement cannot be true as I am only 54 years old, but it feels as long as that. In any case I don’t know anybody at my university who has done so much work with matrices. Why do I say this? I want to say that I do not need APL to make me use vectors and matrices! – I had read an argument, saying that the reason there is no loop mechanism in APL is to force people to think in arrays.

As I said already, I have been using APL for a long time. And all that time I have felt that APL is incredibly wonderful and that it is my great love! But I also felt that APL had some serious deficiencies. Later some of these deficiencies have been cured, again in a wonderful way, by APL2! – APL2 is wonderful! It is ten times more powerful than APL. But, of course, it is no longer quite so easy and it needs some experience to make use of this power – especially to know when you need a disclose more or an enclose less! In the meantime I have learnt: whenever your function doesn’t work and you search and search and you cannot find any error: check the depth of your variables and you will immediately find the reason for the trouble! – but one main deficiency still remains: the lack of a loop!

I have not used FORTRAN now for at least 10 years, but, whenever it comes to a loop, I sigh and remember my first love: O dear! How easy is it with FORTRAN to write DO 10 I = 1, 100 and then 10 CONTINUE! There is no need to waste a second of thinking and you can be absolutely sure, that the loop will be performed exactly 100 times! Why does my great love APL2 deny me such a caress?

To think in arrays and to use array handling functions is not trivial, and is not the natural approach of anyone or a schoolboy or schoolgirl nowadays, for dealing with numbers and characters stored in a computer. The normal approach to nearly every simple problem that can be tackled by beginners is the loop! And if it is easier to write a loop in BASIC or some other language, then it is no wonder that APL cannot become a generally accepted language. And this is what bothers me most: the most powerful language is not accepted by normal students. It is a pity that a 100% efficient tool is only nowadays used by .1% of computer users.

A few years ago my son was 13, now he is 16. How can I recommend him and his friends a programming language that doesn’t have a convenient loop? In spite of my enthusiasm for APL2, I just can’t honestly do it. Let’s say they have a text matrix and they are looking for a certain string in each line. There must be an easy way to go through the matrix row by row and to process each line separately! This is how a normal person looks at such a problem. Of course APL and especially APL2 provide a lot of elegant means to do it otherwise but a child of ten years is not yet interested in such crazy things. – a programming language is a tool and not a straitjacket. It has to enable things and not to prevent anything! The advantage of APL2 is that you don’t need to code a lot of loops all the time because you can use vectors, nested vectors and matrices, the each operator and so on. The disadvantage is that you can’t code a loop in an easy way when you need it or just want it.

Loops are necessary, in my work too, although I know to use arrays and avoid loops. But there are situations where a lot of loops have to be defined and where there is no other way! And furthermore: does it make sense to think for half an hour (or for three days!) and to manipulate an array by ten primitive functions if the alternative is just to apply a simple loop which goes from 1 to n? This may be a nice exercise for somebody trying to learn APL and to get familiar with arrays and array manipulation. It may also be a nice sport, like chess. (And it is even more like chess – chess problems – if you want to modify your function two months later!) But if you are a busy man and you want to get your work done and save your brain for problems that really need it, then you have to write a loop and you need a convenient way to do that.

Loops can be avoided by using the each operator and a subfunction. But the disadvantage of this solution is that you cannot see both functions, the calling function and the subfunction, on the editor screen simultaneously. This is rather inconvenient if several nested loops are dealt with in this way. Say, I have to maximize a likelihood function for an eight-dimensional parameter space and I want to do this the elementary way by varying all eight parameters in turn. So I have eight nested loops and it gets very impractical to develop the program with eight nested subfunctions! (I have tried it.)

I did FORTRAN when there were no THEN, ELSE and ENDIF statements, but recently I translated some FORTRAN programs with such structures into APL2. It was not trivial for me to understand them. My feeling is, that branching to different labels is not significantly inferior to using IF, THEN, ELSE and ENDIF. It may be even easier for beginners to use labels and branching. These control structures may be nice but I think they are much less essential than a convenient loop mechanism! Certainly they would clash with the concise character of APL code a bit.

The situation for these control mechanisms is different from loops: to do something in a loop is the elementary approach. And to branch to a different statement or block of statements is also the elementary approach of the beginner. And branches and labels have to be learnt in any case (perhaps there are some fundamentalists who would disagree). To formulate things with THEN, ELSE and ENDIF is more elegant, but it calls for some learning at the beginning.

I tried to create my own loop by the use of two functions, LOOP and STILL. The resulting technique was:

```
     LOOP  n  'variable name'  vector
     label: . . .
     . . .
     → (STILL n) / label.
```

I didn’t like it because it needed an integer (n) and a label for each loop, and both the integer and the label had to be mentioned twice – I got involved in other things and many months passed.

\* \* \*

Now I came across the above notes which you have just read and I had the idea to try if it would be possible to do it without using a label. It turned out that it was possible! By the use of `→⎕LC`. The two functions are now called DO and DOLOOP and the loop looks like this:

```
     DO  n  'variable name'  vector
      . . .
     → DOLOOP  n.
```

Isn’t this nice? As convenient as in FORTRAN and more flexible, as the loop variable can assume any values (defined in the vector). And it works! The advantage compared with FORTRAN, that no label is needed at all, is unfortunately balanced by the fact that a DOLOOP statement is always needed here whereas a CONTINUE statement is not always needed in FORTRAN.

Now I have my loop! I have just to copy two functions in my workspace whenever I need loops and the long-desired loop mechanism is there! You understand that being 54 years old there was not much hope that a wait for a built-in loop in APL would be rewarded – especially when you have already waited for 15 years. But now I don’t need to wait any more, I have it! – Another proof that in APL2 you can do everything yourself! Isn’t it wonderful? Examples of this DO loop and a formal description are given in the attached program DEMODO.

Of course there have been many attempts to develop user-defined control structures in the past (e.g. Giloi, 1976; Piper, 1992). But to my knowledge they were not widely accepted, although many APLers argue in favour of (built-in) control structures. There may be several reasons for this. Certainly such techniques are more difficult to construct in plain APL than in APL2. So the earlier products were probably not very elegant. Also the solution given above would not be possible in plain APL because the argument of the DO function is a nested vector (in the normal case). On the other hand, if perfection is aimed at and all the IFs, THENs etc. are simulated by functions, then there are a lot of control functions. The disadvantage is that they all have to be copied into each new workspace and that the list of functions is always mixed with functions which are not of interest. Two such functions, DO and DOLOOP, are tolerable, at least for me.

Of course the use of these functions makes programs a bit slower. Six lines of APL2 code have to be processed each time round the loop. But I write a lot of functions where speed is of no interest. And I have learnt that future computers will be so fast that the problem will be to give them jobs!

I will probably not use DO and DOLOOP in basic functions for reasons of speed and portability. With a built-in loop mechanism of course this restriction would not be necessary. And also the preparations needed with this solution – importing DO and DOLOOP, defining the vector LOOPS and localizing the name LOOPS – would be unnecessary with a language-immanent solution.

There are two versions of the DO statement as indicated in the program DEMODO. A third version, simulating a DO WHILE construction, would also be possible. But I decided to leave it without that in order to keep APL simple and (relatively) fast. A DO WHILE problem has to be solved by leaving the loop irregularly by a branching statement located within the loop.

Some additional thoughts which may be of no practical value: In APL versions with only one statement per line it should also be possible to write loops without the loop index n as each DOLOOP statement would be uniquely defined by its line number. Even with multiple statements per line it would be possible if all loops could be relied on to end regularly, i.e. without branching from within to outside a loop. But both of these conditions are too restrictive to be considered in practice.

APL2 is a powerful but no longer a simple tool. In my opinion it has become a language for specialists and for full-time programmers. Future developments, if necessary at all, should therefore not increase its complexity and its level but rather make it a tiny bit more elementary by introducing a simple loop and a simple way to read ASCII records. How can a language exist which cannot read an ASCII file? (I will come back to this question later.) It must necessarily remain an exotic business. My fear is that one day APL will be the most powerful language in the world but it will exist in books only and in archives.

### References

1. Giloi W. (1976): Programmieren in APL. Walter de Gruyter, Berlin
2. Piper D. (1992): DOIF Considered Harmful. Vector 9, 1, 133-142.

### Appendix: the Functions

```
--INSERT CODE--
(.......Gill, where is it, please?)
```

(webpage generated: 23 February 2006, 17:18)
