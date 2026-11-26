---
vid: '10009800'
title: 'Guest Editorial: Time for a New Name?'
authors:
- Stephen Taylor
byline: by Stephen Taylor
volume: '20'
issue: '4'
page: '3'
source: trad/v204/ed204.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2004
tags:
- APL community
- APL in perspective
---

*by Stephen Taylor*
{ .byline }

At our Annual General Meeting on 21 May we agreed to consider changing the Association’s name, and to vote sometime in the coming year on a motion to do so. We did not agree on what the new name might be, but the gist of the idea is that a new name would drop ‘British’ and ‘APL’ and probably include ‘Vector’.

Some members are concerned about this, quite rightly. Observers will be tempted to see this as resignation on our part to a long decline in the use of APL, and the abandonment of any hope of a recovery. They might suppose we seek a new bandwagon, or wish to disguise our old one. After two decades of commitment to APL, why change now?

Nevertheless, there are reasons to contemplate this change. Nouns depend upon the distinctions they are used to mark, much as opinions depend upon facts. Challenged once on changing his opinion, Lord Keynes famously replied, “When the facts change, I change my opinion. What do you do, sir?”Arguably, what ‘APL’ distinguishes has changed in the last twenty years, and is no longer quite the distinction we wish to draw.

In some ways, this is obvious. We publish in VECTOR as much about J as we do about APL, and expect to publish more and more about K. You can think of ‘APL’ as a kind-word, and thus J and K as kinds of APL, just as APL2 and Dyalog APL are kinds of APL. You could call on authoritative support: I have heard both Ken Iverson and Arthur Whitney refer to these languages as APLs. But you would not have the support of usage, and usage is decisive. Within the APL, J and K communities, ‘APL’ is used to distinguish APL2, Dyalog APL, APL2000 and their like from J and K. Theory and their authors claim they are APLs; usage has them as something else.

There used to be some debate as to whether dictionaries are descriptive or prescriptive. That debate is largely over. It is accepted – outside the Académie Française – that lexicographers record usage, not dictate it. Perhaps ‘APL’ will one day serve as a kind-word that includes J and K. But it does not today, and a journal does better to follow usage than try to set it.

If ‘APL’ is no longer quite satisfactory for distinguishing the scope of our interest, it remains to see if we can find a better term. And this is the more interesting part of the whole question. What is distinctive about our languages? Our charter is to promote their usage. What do they offer the world? The answers to these questions are not what they were two or four decades ago.

## Rise and Fall

APL offered personal computing before personal computers. APL appeared as other systems were starting to share mainframe services between multiple users. It offered three distinct advantages.

- Its interpreter removed the distraction of saving, compiling, loading and running programs.
- It reacted to runtime errors by handing the user the stopped environment, with complete access to code and variables, and the ability to resume execution.
- APL’s concise notation made it economical for non-programmers to write programs.

APL quickly became an underground hit among managers with access to IBM mainframes. And while APL never had a comfortable place in IBM’s product range, for most of the last four decades, IBM had more employees using APL than anyone else did. Originally intended for teaching, APL spread through university programmes both as a language for programming computers, and in a variety of disciplines as a tool for modelling and exploring theories.

Two advances in personal computing, and a new movement in software development, brought this growth to a halt. The first was the appearance of VisiCalc on personal computers. With VisiCalc, Dan Bricklin presented, with Bob Frankston, a visual metaphor for the arrays he'd earlier worked on in APL. VisiCalc was a more suitable tool for the limited needs of most personal computing. Moreover, it pointed the way to the second, even more profound, advance.

The graphical user interfaces (GUIs) developed at Xerox PARC and popularised by Apple’s Macintosh computers produced a dramatic shift in the economics of personal computing. A rich set of conventions, learned for the most part unconsciously by users, removed most of the cost of learning to use software. For the first time it became possible to start using new software without studying a manual or training.

The economic importance of GUIs posed a challenge to software developers. Programming GUIs is inherently difficult, blending complex conventions about the use of controls with graphic design, psychology and insight into the user's conceptualisation of the task. Formally, it requires the management of complex states, reacting to complex and unpredictable sequences of events initiated by the user. Contemporary programming languages were inadequate for describing the solutions, and APL was no exception to this.

Xerox PARC’s solution was the Smalltalk language, which describes a world of mutually opaque programmable objects communicating through messages and reacting to events. This – the ‘object-oriented programming’ (OOP) paradigm – is the best framework anyone has yet devised for producing GUIs.

Smalltalk had shown that the OOP paradigm could be used to address not only the essential GUIs but all computing tasks. New OOP languages such as Java and C# appeared to exploit the paradigm. Other languages adopted it, with C and BASIC becoming the C++ and Visual Basic of today.

The economic importance of GUIs has made the OOP paradigm dominant. These days computer-science graduates trained in Java and C++ can be surprised to discover that paradigms other than OOP exist.

APL is most closely associated with the Functional Programming (FP) paradigm. FP approaches programming as the application of functions to data structures. Purists eschew even naming data and have demonstrated variable-free FP solutions to complex computing tasks, much as Smalltalk programmers once demonstrated the generality of an OOP approach. However, FP has nothing to offer the GUI programmer and FP languages are uncommon in industrial programming. While the (by-now various) APLs also added object-oriented extensions for programming GUIs, APL was sidelined too.

The Software Engineering (SE) movement emerged in parallel with OOP languages. SE sought to reduce risk in software development projects by drawing on an analogy with civil engineering. It appealed to managers by offering precise information on the progress of planned work, and appealed to developers like me by proposing to impose on the chaotic development process rules as orderly as those we wrote for our machines. The most elaborate expression of SE was in the Capability Maturity Model, a fine illustration of my Latin master's description of the subjunctive-the pious wish.

Sadly, SE's analogy with civil engineering was deeply flawed, and led to the wrong conclusions. While the processes added to software development hugely improved status reporting, and generated impressive artefacts-specifications, designs, object models-they did little for productivity. Criticisms of this were met with further development processes. Now the IT divisions of many large organisations flounder in a swamp of restrictive processes. At their worst, SE processes sacrifice productivity for a specious predictability-all that can be predicted is that the result cannot be afforded.

The heyday of Software Engineering is now perhaps passed, but in its prime, SE deprecated and expelled the fast and informal development associated with APL, or masked its productivity in expensive development procedures. When coding represents such a small fraction of the effort, why worry about programmer productivity?

## Where next?

Our charter commits us to promote the use of APL. To promote its use we must be able to say what to use APL for, which is not the same as to say what it can do. In particular, we have to identify where its benefits in commercial and industrial programming outweigh the costs of including it in the tool set. If our case applies to J and K as well, then we should adopt a term – Vector languages? – -that denotes them too.

I see two applications.

### Beyond Objects

In a casually incisive remark, a J tutorial observes that OOP and FP are complementary: most complex applications consist of an OOP layer on top of an FP layer. Separated from the black art of managing GUIs, APL productivity kicks butt. Recent developments such as .NET and ASP.NET have significantly lowered the cost of connecting APL processes to GUIs managed by OOP software.

Last week I was working on a web form with an ASP.NET developer when we needed to populate a drop-down list with options representing the 24 hours of the day divided into quarter hours. I had the satisfaction of writing the solution in front of him in a few minutes, testing fragments in immediate execution. (Format the hours with colons, use a catenate function table to join to the quarter hours, then ravel.) He gasped that he would have taken most of a day out to write a reusable C++ class to get the same result.

K’s dictionaries and Dyalog APL’s namespaces make it easy to map to and from states represented by collections of objects. OLE servers and .NET assemblies use the OOP model to let APL software plug and play with everyone else’s. K’s ODBC interfaces allow ASP.NET web controls to bind them.

In short, OOP tools for managing GUIs have now developed to the point where the dominance of OOP languages needs no longer be assumed. This changes the economics of FP programming in general, and of the Vector languages in particular.

### Fast and Furious

One of SE’s fundamental flaws is the misidentification of programming with construction. In SE's view, smart workers break down an initial high-level design into simple coding jobs for keyboard navvies, just as architects and engineers show where to put concrete and steel. The paradigm is guaranteed to produce large teams with lots of internal communications and logistics, and inflexible projects. The software is anything but soft. In pursuit of predictability, SE cedes the real-world game of productivity from the very start.

This is not the place to expand the theme, but the Agile Development movement, of which Extreme Programming (XP) is the best-known exemplar, challenges all this. Agile Development uses small teams of smart people and fast user feedback in a work style hauntingly familiar to APL programmers. And they do it largely in C++ and Java, bless them.

In development projects where most of the effort is coding, APL productivity again becomes decisive. As team size shrinks, communication overhead drops geometrically. (Simon Garland: “A K team with two programmers is huge!”) The time is ripe for combining agile methods and Vector languages.

## Conclusion

APL usage has declined substantially over the last two decades. In my view, the conditions that caused this are now passing or have already passed. There are exciting new opportunities for exploiting our languages, and the implementations have never been better. The opportunities are similar for J, K and the APLs; so we will do best under a single flag. For this reason, I propose the British APL Association, a specialist group of the British Computer Society, should adopt a new name – the *Vector Languages Group*.

A copy of this article has been posted at *www.vector.org.uk*, where you can post comments.

## References

**VisiCalc:** http://www.bricklin.com/visicalc.htm  
**Functional Programming:**  http://homepages.inf.ed.ac.uk/wadler/guide.html  
**Capability Maturity Model:** http://www.sei.cmu.edu/cmmi/

***
