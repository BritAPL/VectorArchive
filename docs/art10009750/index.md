---
vid: '10009750'
title: Three Principles of Coding Clarity
authors:
- Stephen Taylor
byline: by Stephen Taylor (sjt@lambenttechnology.com)
volume: '18'
issue: '4'
page: '113'
description: Principles of writing software that is clear to read, and some practices derived from them
keywords:
- clarity
- code
- coding
- program
- programming
- style
source: trad/v184/clar184.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2002
tags:
- development practice
- programming techniques
---

by Stephen Taylor ([sjt@lambenttechnology.com)](mailto:sjt@lambenttechnology.com)
{ .byline }

For most of my professional life I’ve heard that 80% of a system’s lifetime costs are spent on maintenance. More strictly, on modifying and extending what it does. I don’t know any references for this; I’m just going to take it as so.

The immediate implication is that the clarity of the software we write is probably its most important property, after accuracy and acceptable performance.

I’ve read more articles and heard more lore on APL coding practice than I care to remember. I’ve found many useful rules, but none so useful that I haven’t also found an occasion when it seemed right to break it. So in this article I’m looking *behind* the rules-of-thumb for underlying *principles*, principles of coding clarity.

I’m not attempting here to derive practices from first principles. Instead I’ve been looking for a while at what I do that promotes clarity, and what I avoid that weakens it, and to induce from that some underlying principles. This is a venerable method. The mathematical forms of symbolic logic emerged not from *a priori* reasoning, but were induced and formalised from our intuitions about whether an argument is valid or not.

So with programming clarity. We all recognise clear code, and obscure code, and degrees of clarity in between. Discovering principles of clarity would look like this: we’d recognise that our good practices are informed by them, our bad practices contravene them, and they would also suggest further practices for writing clearly.

To my surprise, I’ve identified only three principles. I’d expected more. But I find them powerfully productive of ways to improve the clarity of my code. Here’s an idea of the results I get from clear coding.

My present practice looks like this. I’ll write an entire module of the system I’m working on, for example, a module that produces a report. During this time I’ll occasionally test individual expressions, to ensure that some dense piece of array manipulation works the way I think it does. Mostly I just write. At a certain point, I’m satisfied that I’ve written a complete draft: I’ve written code for all the system’s function. None of it is tested. This is about my half-way point. I now read all the code, looking for ways to clarify to the human reader what’s going on. I might make several passes, reorganising, renaming, labelling fragments of code as ‘macro’ functions – I’ll come back later to these practices. I spend as much time reading and revising the code as I do writing it in the first place.

Now I’m nearly done. I’ll run the code. Typically I’ll encounter 2-5 typos I’ve overlooked, fix them and keep going. One or two test runs eliminate these oversights. Now I’m done. The code does what I think it does. Any remaining problems reflect my misunderstanding of the specification. They are not coding problems.

What’s distinctive about this is that my practice ensures that the code does what I think it does. How can I know this? This is a point of some subtlety. I have seen work over the years on code-proving mathematics and software to demonstrate that software matches its specification. Nothing useful seems to have resulted. I can run tests to show that the code executes and that the output is as required. But in most cases the combinations of possible inputs are too large to test exhaustively. In the end my best assurance that the code is correct is that I can *see* that it is so. What would it take to be able to *see* that code is correct?

Now there is a level at which any APL programmer can see directly that a piece of code does what he thinks it does. To take an extreme example, I see, as you do, that the expression

```
2+2
```

adds 2 and 2 and will return 4. As we write code of greater and greater complexity, how can we retain, and how *much* can we retain, of this quality of clear, direct vision?

Before we proceed to look at that, let me make a disclaimer. I don’t write perfect code. Even with careful reading and revising I miss typos. Sometimes, aspects of a specification elude me and I have to rethink and rewrite. My point is not that I write perfect code but that I know what my code does. ‘Debugging’ reveals and removes typos, that’s all.

A couple of other disclaimers. Some people, horrified by obscure ‘one-liners’, have made a virtue of breaking all code up into ‘easy pieces’. Not me: I love dense, fast, and viciously economical expressions. This is where we get to use APL’s power. My practice is to write code so that where such expressions occur, *what* they do can be clear to the reader independently of *how* they do it. I’ll have more to say on this under the heading of semantic domain.

The other disclaimer: it will sound in what follows as if I don’t pay much attention to performance. That’s true, I don’t. PCs are fast enough these days that most code runs acceptably fast. I optimise code only when performance is a problem. But I notice that clarifying and revising almost always reduces execution time.

## The first principle: shorten lines of communication

Fantasise a moment. Imagine that you’re a law-enforcement agent investigating a drug gang. What are its members up to? How do they take decisions, and how do they communicate them? When and where does money change hands? Where are goods moved and exchanged? Who knows what, and when and how do they know it?

In reading someone else’s code, you make a similar investigation. Lord help me, in reading my *own* code I’m doing it – I can’t remember from one week to another how everything works.

Now suppose you are a member of such a gang, under surveillance, and want to obscure all these matters from an investigator. What would you do? Why, you’d do what everyone knows to do. You’d use code words to disguise what you talk about. When you have to gather with your colleagues in a meeting room, while you talk, you’d also communicate covertly, passing notes beneath the table, hidden from unseen cameras. You’d use cutouts, so that you never exchange money for drugs, but had other people do that in places far from you. You wouldn’t have important messages delivered to your home in the post; you would have them left in obscure places for covert collection by someone else, who would bring them to you discreetly.

Why the fantasy? In his book <cite>Visual Explanations</cite>[<sup>1</sup>](#fn1), Edward Tufte describes how, to examine what makes a process *clear* to an observer, he looked first at the practices of those dedicated to making it *obscure*. So in order to distinguish practices to promote clarity, he looked at what conjurors and illusionists do to defeat it.

The imaginary drugs gang has similar lessons for us. The practices distinguished above all have their analogues in coding. We can look at them to learn something about what makes code clear, what obscure.

The first analogue is between a defined function and the meeting room, wired for sound and vision. As the investigator, I’d like all communication in the room to be clearly expressed, to be about a single matter, and to be in plain sight – no notes under the table or left in hollow tree in the car park outside!

Central to all this is a notion of ‘place’. We intuit that matters handled in the same APL function are being handled in the same ‘place’. Implicit in ‘place’ are concepts of closure and distance. This is encouraging: these are powerful concepts for theory construction [<sup>2</sup>](#fn2).

Distance is our second analogue. Here are some examples to check against your own intuitions. Data passed between functions as arguments and results are like money or drugs passed from hand to hand: the distances – in time and space – are short. Data assigned to a global variable to be read sometime later by some function is like a message left in a ‘drop’ in a hollow tree, to be picked up sometime later by someone: the separation of parties – in time and space – is great.

Naming a function or value saves it for use later and further away. It’s a way to communicate across ‘distance’ – time and space. The nearer and sooner we use it, the more clearly our investigator sees what’s going on. We have an interest here in shortening our communication paths, even if it’s still unclear exactly what is shorter than what.

Let’s try to formalise our intuitions about distances within the workspace by ranking different instances of distance.

The expression being executed is where the action is. So our first metric is distance within an expression. Consider the following two equivalent function lines.

```
[1] rows←2⊃⍴format ⋄ selection←(rows⍴0 1)/⍳rows
[2] selection←(rows⍴0 1)/⍳rows←2⊃⍴format
```

It’s clear that the distance in the second expression between setting and reading the value *rows* is less than in the first expression. Now consider a third equivalent.

```
[3] selection←{(⍵⍴0 1)/⍳⍵}2⊃⍴format
```

Here an unnamed function is used, carrying the value in its argument <var>⍵</var>, which is local to its function, and thus clearly extinguished when the function has been evaluated. Because it’s clear that no further use will be made of this stored value, \[3\] is clearer than \[2\], where *rows* might have had some later use.

Our second intuition of ‘distance’ is between lines in a function. The more function lines separate the naming and the using of an object, the further apart they are, and the longer is the communication path. Any two name references in the same line of code are closer together than any two references on different lines.

Our third intuition of distance is between levels on the state indicator, or execution stack. References on different levels of the state indicator are further apart than references in the same function or operator.

These intuited distances seem to combine according to discernible rules. Consider: function *F* calls *G*, then *H*. *G* sets a variable *a*, and *H* reads it. (I do *not* recommend this design.)

```
    r←F y
[1] G y
[2] r←H
```

The distance between naming *a* in *G* and reading it in *H* is something like the sum of the distance in *F* between lines \[1\] and \[2\], and the 1-level differences in the state indicator between *G* and *F*, and *H* and *F*.

Shortening the line of communication increases clarity. Having *G* return *a* as a result, and *H* take it as an argument gives us:

```
    r←F y
[1] r←H G y
```

And even better:

```
F←H∘G
```

where the distances between symbols *F*, *G* and *H* are even shorter. Here we see that the principle of shortening lines of communication at least recommends to us practices we already know promote clarity.

Shortening lines of communication underlies several well-known rules-of-thumb. We’ve already noted the good practice of passing values between functions as arguments and results.

We can also see that where named values are to be shared by functions that cannot pass them as arguments and results, we do well to localise those names as near as possible to where they are used. For example, if the functions *G* and *H* have to communicate through variable *a*, then it were better to localise *a* to *F* than to define it as a global variable. Thus *a* would be external to both *G* and *H*, but local to *F*. Our first principle explains why. Localising *a* to *F* doesn’t itself shorten the line of communication, but it shows the reader that communication through *a* goes no deeper into the stack, and ends when *F* has been evaluated. That equates to an increase in clarity.

There is no difference in principle here between assigning names to data and assigning names to functions. We shall see some practices suggested by this insight after we have looked at the second principle.

## The second principle: semantic consistency

Consider our investigator again. Let’s suppose that he’s a newcomer to the world of drugs, arms-smuggling and international finance. He could easily get confused listening to a conversation that switched between these subjects. Here’s a danger: in the middle of a discussion of planned drug movements, someone wants to settle questions regarding the use of bank accounts in the Caribbean. Our poor agent could miss some important detail.

Luckily for him the gang bosses send aides into another conference room to work out the banking details. Our agent has a separate tape recorder in there; he will be able to review their conversation later. Both conversations will be clearer and he won’t get confused between them.

The analogy with writing code is obvious, and the practice of ‘subordinating detail’ is well known. One has an intuitive sense of what is the ‘main story’ of a function. But here is a formalisation of it that yields new practices.

Consider the concept of *semantic domain*. Words belong to the same semantic domain when they are about the same subject.

A function has complete semantic consistency when all its referents (what its names refer to) belong to the same semantic domain. For example:

```
    document←Report record
[1] document←CoverLetter record
[2] document,←Page1 record
[3] document,←Page2 record
[4] document,←Page3 record
```

The function’s referents — all but one — share a semantic domain. The referents are parts of a document that reports facts from *record*, which is the exception. The exception, the argument *record*, belongs to a different semantic domain. It contains facts about someone or something; it is not a part of a document.

The function *Report* describes a relationship between two domains, the facts and the document reporting them. As a rule-of-thumb, functions which relate two semantic domains are more interesting than functions with one. But adding semantic domains pays in clarity for any gain in interest.

There is something slightly misleading about the above. Semantic domains do not exist in nature, nor are they precisely individuated. The concept is useful only as a way to marshall and sharpen your intuitions about the organisation of your code.

Here is a powerful practice generated by the principle of semantic consistency: use names that relate to the semantic domain. That seems obvious, but Ray Cannon proposed a test of it which I have found an extraordinary source of clarity.

### The Cannon Test:

> Are there numeric constants in the body of the function?

If the answer is Yes, consider assigning the constants to local names that show the meaning of the value in the relevant semantic domain, then using the name instead.

For example (in a function that generates RTF code for tables in Microsoft Word documents) I set up a rank-3 control array of cell properties. Planes of the array correspond to properties like alignment, width, background shading and so on. The third plane specifies background shading, so

```
[10] format[3;((2⊃⍴format)⍴0 1)/⍳2⊃⍴format;]←5
```

specifies a 5% background shading on alternate rows. Applying the Cannon Test led me to declare local names for the planes:

```
align width shading←1 2 3
```

which changed line 10 to

```
[10] format[shading;((2⊃⍴format)⍴0 1)/⍳2⊃⍴format;]←5
```

with a rise in clarity. *Shading* shares a semantic domain with *format*. But numeric constants remain. The reshape of 0 and 1 refers to a stripe effect I want on alternate rows of the table. Declaring a variable in the ‘set up’ of the function makes this clear:

```
alternateRows←((2⊃⍴format)⍴0 1)/⍳2⊃⍴format
```

enjoys a semantic consistency of its own: *alternateRows* is a function of the size of *format*. We can clarify it by removing repetition:

```
alternateRows←{(⍵⍴0 1)/⍳⍵}2⊃⍴format
```

Now line 10 gets even clearer:

```
[10] format[shading;alternateRows;]←5
```

The last numeric constant is 5. Should that go too? Absolutely. The 5 denotes a 5% background shading. But in the world (semantic domain) of report appearances we don’t make a hundred distinctions of shading. What we mean here is pale grey. So a declaration in ‘set up’ of

```
paleGrey←5 ⍝ 5% background shading
```

leaves us finally with

```
[10] format[shading;alternateRows;]←paleGrey
```

Function code and web pages have something in common: they are rarely read, mostly skimmed. The kind of clarification achieved above aids skimming. Skim-reading the assignment to *alternateRows* without analysing it, one sees that its value is a function of the size of *format*, which seems right. On line 10, we can see what is specified for alternate rows (shade background 5%) without being distracted by how alternate rows are selected. (Compare the separation of the gangsters’ two conversations.) Should we ever be concerned about how alternate rows are selected, we can examine it as an isolated proposition.

Here’s an even more extreme example. In working with RTF, I convert between inches, half points, points and twips (twenty twips to a point; 12 points to an inch). To a programmer immersed in this subject, the numbers 12, 20, 2, 72 and 1440 are clear signs of units being converted. To a casual reader, or the next programmer struggling to master the subject, they’re just numbers. So functions that convert units declare some local names:

```
insToTwips←1440∘× ⋄ twipsToPts←÷∘20 ⋄ ptsToHalves←2∘×
```

Even the humble *double* function `2∘×` here gets assigned a local name to clarify its semantics! Humble 1s and 0s suffer even more semantic ambiguity, which we can helpfully clarify:

```
do dont←on off←true false←1 0
```

Compare this practice with the alternative of adding comments. Comments are not always maintained with the code they relate to, and can get out of step with it. I systematically prefer to clarify code than to comment on it.

In the examples above, semantic consistency led us to reduce code density slightly: we added symbols and lines. This is not a general argument for reducing code density! If I seem to be belabouring the obvious, it is because the principle of semantic consistency leads me to vary code density in ways that are otherwise not at all obvious.

For example, in another function I combine specifications for the row properties of an RTF table with specifications for the cell properties. We’re here manipulating arrays of RTF code strings with the sort of speed that makes us glad to have APL:

```
    TABULATE←{
[1] ⎕ML←1
[2] rProps←PageWidth rowProperties ⍺
[3] cProps←(cellProperties ⍺){0=⍴⍺:'' ⋄ ⍺,GROUP ⍵,'\cell '}¨⍵
[4] ∊rProps tableRow∘∊¨⊂[2] cProps
    }
```

The original code is a 1-liner. I’ve relineated it here for *Vector*’s page width. Semantic consistency is complete and suggests no changes to improve clarity. The relineation has lightened the burden of parsing a long line, but at the cost of slightly lengthening the lines of communication. I doubt the 5-line version is any clearer than the 1-line version.

In this example, the code is just dense, that’s all. It describes some moderately fancy array manipulations. Any difficulty in reading is the difficulty inherent in reading and envisaging the array transformations. The semantics of what is being done is already clear: table row properties are being combined with table cell properties. Style has nothing further to add here to clarity.

Would *TABULATE* be clearer if laid out in loops? It would — to a programmer who thinks in loops. To a programmer who thinks in arrays, a looped description is laborious and less clear. We write in APL because machines are good at manipulating arrays, and we gain mastery of their power by thinking and writing in arrays. A looped version of *TABULATE* would reduce both machine and programmer efficiency.

If this works for you, then the principle of semantic consistency can help you to use dense code without losing clarity.

## The third principle: don’t repeat yourself

People who repeat themselves bore us and lose our attention. We then risk losing important detail in a flood of repetition.

Don’t do that to others! This is such a general principle of programming that it hardly bears including. All programmers do this; arguably, it is what programming *is*.

I’ve included it as our third principle, because, combined with our first two principles it produces coding practices I value but have not seen in common use. One of these is extensive use of localised code fragments or ‘macros’.

## Macromania

Wherever I see a pattern repeated in code, I consider defining it as a function, named for semantic consistency and localised to minimise the communication path. Direct definition and the *composition* and *each* operators are powerful tools here.

We’ve already seen above the examples of the unit conversions and the pre-selection of alternate rows of a table. Even for one-time use these were worth giving local names to, if only as a substitute for comments.

The same trick is good for any recurring code fragment. For example, it’s a simple piece of code to format a number as a sterling amount to include in text:

```
'up to ',({(∨\⍵≠' ')/⍵},'P⊂£⊃CI2'⎕FMT max),' a year'
```

I would declare at the beginning of my function:

```
stg←{(∨\⍵≠' ')/⍵}∘,∘('P⊂£⊃CF12.2'∘⎕FMT)
```

and use the local function *stg*

```
'up to ',(stg max),' a year'
```

So my application functions often begin by assigning names to some local functions and constants. (Note that this practice is as often indicated by semantic consistency as by avoiding repetition.)

Compare this practice with defining, naming and using a ‘global’ utility function for the whole workspace. The global utility will have more complex arguments to handle many more cases, will thus be less efficient, and will have some name that distinguishes it as belonging to the class of global utilities. In contrast, the macro has just enough code for the job at hand and is named for semantic consistency.

## Control structures

Control structures like If-Then-Else and Select-Case lengthen communication paths by adding function lines. I use them rarely and happily to group blocks of code which are not minor variations of each other. But keeping communication paths short leads me to get If-Then-Else handled in a single line where possible.

Here are three handy macros and illustrations of their use:

```
IF←/⍨ ⋄ BUT←{×⍴⍵:⍵ ⋄ ⍺} ⋄ WITH←{⍺}
→0 IF 0=⍴argument
→(0 WITH r←'') IF 0=⍴argument
z←34 45 56 BUT 25 50 75 IF argument=3
herhim herhis←'her' 'her' BUT 'him' 'his' IF sex='M'
txt,←'to consult ',herhim,' about ',herhis,' pension'
```

## Shy arguments

Supplying a default for an omitted shy left argument seems too small a job to warrant branches or control structures. Try this D-function instead:

```
defaultsTo←{×⎕NC ⍺:⍎⍺ ⋄ ⍵}
```

```
left←'left' defaultsTo 0
```

The use of *defaultsTo* has the virtue of making an explicit assignment to *left*, clear to be marked by an automatic code analyser.

## Conclusion

Notice that our three principles often tug is in different directions! For example, naming a code fragment in a ‘set up’ section at the start of a function *lengthens* communication paths by separating definition from use. It is finally perhaps only an æsthetic judgement how to weigh the clarity gained by semantic consistency against that lost by introducing a new ‘path of communication’, even a short one, within a single function.

This, in my view, is why the many practices I’ve seen proposed as rules for clear code are fragile guides at best. In æsthetics it is helpful to distinguish principles, and futile to legislate.

These three principles are offered for sharpening your intuitions about the clarity of code. APL shares this with the natural languages: there are useful guides to writing clearly, but no formula for it. <q lang="la" style="font-style:italic">De gustibus non disputandem est.</q>

## Notes

1. <a id="fn1"></a> Edward R. Tufte, *Visual Explanations: images and quantities, evidence and narrative*, Graphics Press, Cheshire, Connecticut, 1997
2. <a id="fn2"></a> Enclosure: <q>Distinction is perfect continence,</q> wrote G. Spencer-Brown as his first assertion in *Laws of Form* (Crown, 1972) and went on to derive the fundamentals of mathematics. Distance: we’re unable to even think about cause-and-effect without using distance as an analogue for time. Julian Jaynes: <q>history is impossible without the spatialization of time 
    		that is characteristic of consciousness.</q> *The Origin Of Consciousness In The Breakdown Of The Bicameral Mind*, Houghton Mifflin, 1982, p. 251.
