---
vid: '10009900'
title: Pair Programming With The Users
authors:
- Stephen Taylor
byline: by Stephen Taylor (sjt@5jt.com)
volume: '22'
issue: '1'
page: '128'
description: VECTOR article
source: trad/v221/sjt221.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- development practice
---

by Stephen Taylor (sjt@5jt.com)
{ .byline }

Written from the notes for my talk at the Dyalog Users’ Meeting at Elsinore in October.

We have two kinds of delegate at this meeting. Some of us have used <acronym>APL</acronym> to write software which we use to offer goods and services. A while back Stefano Lanzavecchia was quoted as saying while he didn’t think <acronym>APL</acronym> had anything still to teach computer science, it was still the best language to use if you wanted to get rich.

How many people do you know who have made a million dollars or more with software they have written themselves – and how many of them wrote in <acronym>APL</acronym>? The prospects for entrepreneurs like these remain as bright as ever. But not for the other kind of delegate here.

The other kind of delegate here is someone like me, who likes to be paid to write software in <acronym>APL</acronym>. For us, the decline in the popularity of the <acronym>APL</acronym>s jeopardises how we like to make a living. And that is what I want to talk about today.

Two years ago at the meeting in East Horsley I argued that our best prospects are closely aligned with the movement for agile systems development, sometimes called Extreme Programming or just <acronym>XP</acronym>. Today I want to report how that future is emerging in the work I’m doing with [Optima Systems](http://www.optima-systems.co.uk) at one of the <acronym>UK</acronym>’s largest life and pensions offices, and I’ll review what has and hasn’t worked in our systems development there.

I’m also going to discuss a new paradigm for developing software, an alternative to the analogy with civil engineering popularised by the Software Engineering movement; because how we talk and think about what we’re doing matters far more than we usually realise.

Finally, I’ll advance a new metric, *semantic density*, relating to the clarity of source code, and I’ll discuss its importance to the agility of our software development.

## Coming back

Two years ago, our rôle at the pension company had an unpleasant familiarity. From several mergers the company had inherited <acronym>APL</acronym> systems developed over the previous 15 years. Mergers are always predicated on economies of scale to be obtained from rationalising administrative work and the computer systems that support it. Such strategies require great faith that yet unidentified problems will be solved within the chosen technical strategy. In this world of faith, a technology that is not part of the solution is part of the problem.

Contemporary conversations about <acronym>IT</acronym> strategy provide no place for development in <acronym>APL</acronym>. It followed inexorably that our systems were integration problems and scheduled for replacement by ‘strategic technology’. Here we see the cost to us of ceding our claim to an important rôle for <acronym>APL</acronym> in the development of key systems.

Another familiar aspect of our position was that our systems proved particularly resistant to replacement. Manager after manager compared the cost of maintaining our systems with the cost of replacing them and concluded he had more pressing business.

As other legacy systems were wound up, the <acronym>APL</acronym> team languished in the exit lounge. We wanted to break out and win recognition as a useful resource for new systems development. But, as many other <acronym>APL</acronym> teams have found, a record of unmatched productivity is not enough to overcome the unblinking faith required by a corporate integration strategy.

We promoted ourselves as writers of tactical systems, interim solutions, stopgaps to be used until corporate strategy bore its long-awaited fruit. Eventually we found a senior manager with new responsibilities who needed results badly enough to give us a break.

The opportunity he gave us was to help him process pension claims. This is work of a complexity that Dickens would have admired. Pension funds have suffered decades of changes in products, legislation, regulation and tax law. When the time comes for a policyholder to retire and convert savings into a pension, someone has to unwind all this, ensuring that restrictions placed on rights vested in certain parts of the funds are respected. Two years ago this process took about an hour a policy. It took six weeks to train a new clerk to process the simpler claims, and as many months before she could process all the possible variations. Managers were largely unable to switch staff to tackle backlogs, because cross-training took three weeks. The senior clerks had improvised all kinds of tools to support their works: spreadsheets, Word document templates, checklists – at least the quill pens were long gone.

We approached the work in a way that will be familiar to <acronym>APL</acronym> programmers, writing code alongside the end users. Two years later we have halved the average processing time, can get very simple claims processed in under ten minutes, and we have become the primary tool for ‘business re-engineering’ in the pensions division. We are now winning jobs from the <acronym>IT</acronym> division’s Too Hard basket, and have doubled the size of our team – large by <acronym>APL</acronym> standards, tiny by <acronym>IT</acronym>’s – to handle the new work our customer is asking us to take on.

And for certain portions of its business, we are now the designated strategic solution.

## What worked, what didn’t

Something that failed from the beginning was analysis. You all know analysis. You have a conversation with the user, take notes, write a description of what the computer is supposed to do, have the user confirm it. Then you go write the program. None of this worked.

Our expert users are the senior clerks who train new clerks. They are the source for what is needed from the system; no one knows the work better than they do. They train new clerks to do the work, so you would suppose they can explain the requirements. Not so. They train new staff in procedures, not by explaining theoretical concepts, but with “do this, then do that” instructions. Staff do eventually work out for themselves the implications of what they’re doing. It’s just as well humans can learn this way, because the staff’s linguistic skills don’t allow them to talk and reason about the work with any precision or clarity. Over and over again our conversation floundered into self-contradictions and misunderstandings, unable to navigate through the many variables in play. I saw that even if I were able to write requirements I had confidence in, I had no hope of getting a useful review of them.

A radical change of approach was needed. We were working on the rules for determining whether a particular penalty was to be applied to a policy before benefits are taken. The checklist the clerks used to decide this covered five A4 pages. “Show me your first example,” I said. We worked through it, with me copying numbers from the mainframe screens and defining variables and rules. Eventually I had a function that used the same facts she did, and produced the same answer. I saved the cluster of facts as my first test case and started modifying the function as we worked through her second example. When I had a second right answer I retested the first case and reworked the function until it produced both answers correctly. By the end of a fortnight we had working code and a test suite with forty cases, and I was starting to see the principles involved.

Over time I was able to refactor these rules for clarity, using the test suite to confirm I was still getting the same results. The rules written in <acronym>APL</acronym> now cover about half an A4 sheet; that’s one order of magnitude smaller than the checklist in English. As far as I know it is the only formal specification of the penalty rules the organisation possesses. And – this is the kicker – it’s maintained jointly by the <acronym>APL</acronym> programmers and the clerks. It’s been some time now since we found a case for which we needed to modify the rules; but when we do so, we jointly trace execution, examining values, to identify where the machine’s logic varies from the clerk’s. We correct the rule – the programmer modifies it and the clerk confirms the new code appears to match her meaning – then execute it to confirm it now yields the right answer.

Extreme Programmers work in pairs to produce immediate feedback on code quality, and get user feedback on a 2–3 week release cycle.

We work in pairs with the users to get *immediate* feedback on the requirements. When the specification’s confirmed, the programming is finished, for the specification is executable. Collaborating with system users on an executable description of the system’s behaviour is a key breakthrough in our work. We call it ‘Pair programming with the user’.

Another hard-learned lesson concerned collaborating with the internal <acronym title="information technology">IT</acronym> division. No one in the business has been able to grasp the speed and flexibility our work requires except the people participating with us. Where conventional <acronym>IT</acronym> delivers in weeks and months, we deliver in hours and days. Usually we can write an executable description of system behaviour that solves the business problem (*i.e.* a program) faster than an analyst can write a system specification. That time includes generous rewriting as we get user feedback of detail, quantity and quality largely unavailable to the analyst. As a result, our programs are a better fit to the business needs than any formal specifications we’ve seen the organisation write. Ever.

In this work it makes little economic sense to plan ahead much. This makes our progress hard to predict. We rarely know how complex or simple a job is until we’ve finished it. This is madness, of course. The only justification for it is that we progress so much faster. Our customer’s managers naturally hate this unpredictability, but consistently prefer our rapid progress to predictable delay.

## Making movies

There are in this naughty world programs that have to run exactly right the first time – ask <acronym title="National Aeronautics and Space Administration">NASA</acronym>. Arguably, the programs we are writing for the pension company fall far away from those on a scale defined in terms of cost of errors, complexity of function, stability of requirements and cost of delay.

You might suppose that the <acronym>NASA</acronym>-style project best represents general systems development, and that the conventional ‘engineering’ approach of ‘plan it, then build it right’ is the best way to work. You might allow that what we are doing makes sense, but only in our unusual situation, and that few general lessons may be drawn from it.

For a moment consider instead the possibility that some of the more unusual features of our project merely shed strong light upon common aspects of software development that the engineering approach does not avoid but only masks.

That would be interesting if it were true, because we have other reasons to suspect the foundations of Software Engineering. <acronym>SE</acronym> was adopted in the 1980s in response to a widely-shared view that most software projects were out of control. Writers like Boehm<sup><a href="#1">1</a></sup> and deMarco<sup><a href="#2">2</a></sup> drew unfavourable comparisons between programmers and civil engineers and proposed we would do better if we could adopt engineers’ methods. Thus Software Engineering and the rise of the formal methodologies.

<acronym>SE</acronym> has had some success at making software projects more predictable; but the modesty of these successes has to be contrasted with the effort expended upon them. When intelligent people work long and hard at something for little result, we are entitled – perhaps obliged – to ask if the problem has not been misconceived. More recently, some writers have argued the analogy with civil engineering has been misapplied from the beginning.

It is a standard strategy of industrialisation to conserve expensive intellectual skills by defining simple and repetitive jobs that can be done with minimum skill. So, in building a bridge, expensive engineers develop plans, then cheaper workers pour concrete and weld steel. In the <acronym>SE</acronym> model, analysts and designers develop specifications and database designs, which are implemented by programmers. In <acronym>SE</acronym>, programming is the analogue of construction; and programmers the navvies of <acronym>IT</acronym>.

Ward Cunningham<sup><a href="#3">3</a></sup> points out important differences in the material properties of bridges and software. Erecting steel and concrete is slow, and expensive to modify. Compiling and copying software is not.

A computer program is an executable description of the behaviour of an imaginary machine. ‘Executable’ means that a Turing Machine can read the program and emulate the behaviour of the imagined machine. Given plenty of <acronym>PC</acronym>s and a program, construction of the imagined machine is nearly immediate and free.

Cunningham argues that compilation is the analogue of construction, not programming. The rest of the work – developing the software – is all design work. It can be done in pretty much any order (like drawing the bridge and unlike erecting it) and can be changed or redone relatively cheaply.

This view of software development casts doubt on the industrialisation project itself. No architect would try to analyse his architectural practice into simple and repetitive tasks. Perhaps industrialising the writing of software is similarly inappropriate?

What other processes could we use as a model? Another example of a collaborative writing project is making a movie. A producer friend of mine in Hollywood claims my software projects sound more like her movie projects than engineering work. Movie-making turns out to have important differences from writing software too, but also enough similarities to make comparison instructive.

A movie project starts with an initial idea and finishes with a film master, from which copies can be made without limit for distribution. The serious money is spent during ‘principal photography’. Until then the work consists of developing increasingly specific descriptions of the movie.

The first description is usually very short (“Godzilla Meets Bambi!”), and does not need writing down, much as a director might tell his board, “We need a new order-entry system!” More elaborate descriptions follow in succession: treatment, screenplay and shooting script. Sponsors work on these descriptions to satisfy themselves the imagined movie will meet their own demands. By the time the shooting script is ready it incorporates considerable feedback from participants concerned about finance and marketing.

In a shooting script all the scenes and dialogue have been defined. The director has what he needs, but may still make extensive changes during photography. (My friend: “A script? That’s what you use to get funding.”) Scenes the director has imagined in detail sometimes fail to work despite many takes, and get rewritten. Within the constraints of the shooting schedule and the actors’ availability, new problems get identified and solved, scenes get reshot to reflect changes made elsewhere. Then all the film goes to the cutting room for further revisions.

Movies are like our software project in incorporating lots of feedback during their making. Design continues into the cutting room and after first screenings, the ‘sneak previews’. They are unlike our software project in that design stops after release, while our and other agile projects release as little as possible as early as possible, and keep modifying it as long as people think the effort worth paying for.

Both movies and software projects start off by writing increasingly elaborate descriptions of the project. In movie projects this process ends with principal photography. The project continues to evolve in the director’s hands, watched anxiously by his producer, but no more descriptions are written.

In building a bridge the same moment arrives when the engineers stop drawing and the workers start pouring and welding. In the engineering view of software development, that moment arrives when coding starts. It’s the time to stop describing and start making. And it is the perennial complaint of the programmers that they are never left alone to write the described program. If you imagine yourself as a construction worker who finds himself repeatedly tearing down what he has just built to erect something like it nearby, you have a sense of the programmer’s frustration.

Why *aren’t* the programmers left alone to do their job? The comparison with making movies is instructive. In both fields the projects develop a succession of increasingly elaborate descriptions of the work, using them to imagine the end product in increasing detail and to communicate about it. But the movie people can understand their descriptions, right down to the shooting script, and that difference is everything.

Of course movie people can read a shooting script. They’re movie people! They can read a screenplay, envisage the result and write a reliable estimate of production costs on the back of an envelope. All the descriptions of the movie allow the sponsors to communicate about the end product.

Nothing like this is true in software. When the director tells his board, “We need a new order-entry system!” he knows what he is talking about. By the time a fat binder of ‘requirements’ has been through the ritual of ‘sign-off’, communication has stopped, for the project description has escaped the language of the managers. The requirements now consist of formal specifications they cannot understand, plus reassurances such as “user responses will be sub-second or better”. Unlike the movie producers, the system sponsors cannot read the description and reliably envisage the end result.

“Whereof I cannot speak, thereof I must be silent.”  
Ludwig Wittgenstein

There are Two Great Lies of Software Development, which sponsors and developers tell to each other. The first lie is “We can tell you what we need.” The second lie is “We can tell you what it will take to produce.”

Both lies enclose a kernel of truth. The sponsors *do* know a lot about what they need. The developers *do* have a lot of useful experience of writing. Sponsors and developers tell the Two Great Lies to each other because they think it shameful not to know these things completely.

The reality is that they don’t. Canny project managers allow for this and treat system requirements as a shooting script – something you use to get funding. Sponsors cheerfully accept breathtaking variation from the specification provided the needs of the business are met. The unspoken agreement that underlies the contract is that the developer will meet the needs of the business in something like the time estimated, and at something like the projected cost; and it is this unspoken agreement that provides the context for the project manager’s work.

Movie sponsors have feedback loops in their development of the movie that are missing from the initial development of a software project. Where does this feedback show up instead? It shows up when the software is first delivered. This precipitates the usual argument. The customer objects it doesn’t meet his business need, and the developer replies that it exactly fits the description the customer signed. This conversation proceeds to a discussion of what change can be made to save the day, and who will pay for it, either as an extension (customer) or correction (developer).

The hard fact here is that it is very difficult to envisage the behaviour of an imaginary computer system; it is also very hard to describe. Of course, it is also hard to imagine and describe a movie, but these are skills movie people cultivate. Business people do not cultivate skills in imagining and describing computer systems.

Suppose the near-impossible: you have a customer who can envisage in unlimited detail the behaviour of a computer system he needs – he can give consistent answers to all questions about it. How would he describe it to you? Not using formal methods, for he is a businessman, not a developer. He must use informal, ambiguous, flexible, elusive English. He might well have all the detail (*pace* Wittgenstein) ‘in his head’, but he will never communicate all of it to you. Unlike the movie people, you don’t share a common notation.

In this light we can begin to distinguish some interesting elements of our recent work on pension claims. In true <acronym>XP</acronym> style, we are always modifying a running system. We do not rely on imagination to carry us far beyond that. We generally discuss only small changes to observable behaviour.

My personal practice in this regard has become somewhat extreme. The clerks who guide our work no longer write even emails to me about changes they want. To get a change, they come and sit beside me. They used to begin by describing the change they want, until recently I noticed what a waste of time this usually is, and put a stop to it. Now they begin by ‘breaking’ the system, either by showing me some way they’ve found to crash it or by getting it to behave in a way they want to change. Then we talk a bit. I’ll use my knowledge of the system to locate where that behaviour is described and interrupt execution there. We trace execution and modify the code together as I wrote above.

Like the movie sponsors, we share a common notation for describing the end product. Our end product is not a movie, but the system behaviour. Our common notation is the source code. We are able to use the source code as a specification because of some special properties of the writing (which I’ll come to shortly) and because the system animates it for us.

‘Animates’ is a better word here than the usual ‘execute’. Recall that source code is nothing but a description of the behaviour of an imaginary machine. It differs from (*e.g.*) “an order-entry system” in being formal (so the machine can animate it) and more elaborate. (It also has a different semantics, but more of that later.) The same is true of the C code into which our source is translated, and arguably of the machine language into which that is compiled. This imagined machine is never built, only described; then that description is animated by the computer. It is the great virtue of computers that this is all we need: a description of a machine and a computer to animate it.

The computer animates our description, giving me and my user immediate feedback on what we have written, much as a movie director pores over ‘rushes’ at the end of a day’s shooting. When we are satisfied, I save the modified system where she can test it further.

From this you can see we have collapsed the stages of analysis, specification, design, coding and testing into a short, uninterrupted dialogue with our user. Communication is high-bandwidth, face to face; feedback is rich and immediate.

You might suppose that in these conditions, and having worked together for over a year, communication is very good. Not at all. Misunderstandings are rife. Mercifully, the feedback quickly exposes them.

When we contemplate doing the same work through descriptions written in English, we shudder.

Just such description writing lies at the heart of the ‘waterfall’ development methodologies. (Perhaps they are really named for the cascade of non-executable system descriptions they produce.) From our perspective, conventional waterfall projects look mostly like collaborative literary projects to produce documents that neither men nor machines can read effectively. Consider: a program is a machine-behaviour description that a computer can animate. How many descriptions do you want to produce that *can’t* be animated?

An engineer who knows a component is expensive and unreliable uses it as little as she may. Communication (in English or other natural languages, without machine animation) is unreliable and expensive. Yet conventional methodologies employ it as if it were reliable and free. Worse, it is repeatedly advocated as the remedy for software projects in difficulty. Meetings abound. “We just need to ensure everything’s documented and signed off,” as if everything important could be pulled out of the changing world and fixed on the page. Well, some things can; but there are important limits. Perhaps software developers should study poetry more.

## Semantic density

“I gotta use words when I talk to you.”  
TS Eliot

What can we use to replace expensive and unreliable communication in English?

Our answer is to shorten the cascade of descriptions that starts with “an order-entry system” and finishes with the source code. Most software development methodologies call for maintaining and synchronising the entire sequence, so that changes at one level are reflected in the others. Many waterfall projects that subscribe to this in theory avoid it in practice. This is widely known as ‘writing the documentation after the code’; and sometimes it is known as ‘writing the specification after the code’.

Keeping the non-executable descriptions correct and in step is a lot of maintenance work, especially for descriptions that cannot be run on the computer, and which are very hard to validate. We should avoid as much of this work as we can. We admire the <acronym>XP</acronym> practice of destroying design artefacts after use. This prevents both misuse (when no longer accurate) and maintenance. But destroying design artefacts is practical only to the extent that the source code is readable.

Put it the other way around: improvements in the readability of source code permit order-of-magnitude reductions in the development work. In this last section I shall discuss a source of coding clarity. This extends ideas I first advanced in “[Three Principles of Coding Clarity](trad/v184/clar184.htm)” (Vector 18.4)<sup><a href="#4">4</a></sup>.

Like many programmers seeing <acronym>APL</acronym> for the first time I was excited by seeing the expression

```
        SALES ← +/ PRICE × QUANTITY
```

where `PRICE` and `QUANTITY` were understood as table columns. No loops, no counters! I understood that their absence was important, but was unable to say why. People who felt the same way spoke of being able to “concentrate on the problem”, and Ken Iverson wrote of the importance of “[Notation As A Tool Of Thought](http://elliscave.com/APL_J/tool.pdf "download PDF 2.3Mb")”<sup><a href="#5">5</a></sup>.

We intuit that talk about sales, prices and quantities is distinct from talk about loops, counters and conditions. In “Three Principles…” I used the phrase *semantic consistency* to sharpen this distinction. The terms *sales*, *price* and *quantity* are drawn from the conversation of our customers. These terms refer to things our customers discuss: their referents are in the business world. The terms *loop*, *counter* and *condition* come from the conversation of programmers. These terms refer to things programmers discuss: their referents are confined to the world of program logic. We could say that if the tokens of a program refer to both the business world and the programming world, then it has two *reference schemes*, or **semantic domains**. And the exciting aspect of `SALES←+/PRICE×QUANTITY` is that it has only one.

One can quibble about this. The × function belongs to both domains. So does the + function, but +/ is known only to <acronym>APL</acronym> programmers; while *sum* would belong to both. Syntactic elements such as assignment, if/then/else and select/case turn out in practice to be easily assimilated into the language of non-programmers. Linguistic philosophy warns us that individuation of semantic domains is likely to bear only limited weight; it is a ‘loose and popular’ usage.

But in practical terms the distinction of semantic domains is clear and useful. We can now measure *semantic density*: the proportion of writer-defined tokens in the semantic domain of the system user.

Our experience is that at high levels of semantic density (<acronym>SD</acronym>) we are able to share source code and ‘pair program’ with our business users in the way I have described. <acronym>SD</acronym> does not have to reach 100% to get this effect, but misunderstandings decrease sharply as <acronym>SD</acronym> approaches it.

The effect does not seem to be much diluted by the use of primitive <acronym>APL</acronym> functions. The functions + and × are read as usual, and +/ is often read as sum. The assignment arrow is understood without explanation. Otherwise, most primitive functions and unnamed dynamic functions (Dyalog APL supports something like the [lambda calculus](http://en.wikipedia.org/wiki/Lambda_calculus "Wikipedia article")) are simply ignored.

This is consistent with a distinction between close reading and skimming. A close reading requires attention to every detail; our users need to see only that (eg) the tax-free cash allowance is derived from the tables of assets and previous transfers and from nothing else.

A moderate use of control structures is helpful. Our users read if/then/else more easily than expressions which return a value as a function of the condition. But nesting control structures appears to degrade readability, as to a lesser extent does verbosity. Using IF/BUT is often a useful compromise. <span class="small">\[See Phil Last’s more elaborate <a href="nap221.htm">treatment</a> of inline conditional structures in this issue. - <i>Ed.</i>\]</span>

```
        MinAge ← 65 BUT 60 IF Sex='F'
```

The <acronym>APL</acronym> glyphs for the primitive logical functions have not become familiar enough to be read easily; so for shared code we define aliases: and, or, not. Unused to formal logic, business users require the most careful phrasing to keep logical structures clear.

Balancing these considerations is a matter of writing style, calling for nice æsthetic judgement. Programmers traditionally regard writing style as of small consequence. Our development practice puts writing style in the front line; illiteracy costs too much.

Using a local, task-specific vocabulary in the user’s semantic domain…

```
        asIDN←{dft 3⍴#.IDNToDate ⍺ ⍺⍺ #.DateToIDN dtt ⍵}
        daysAfter←+asIDN
        daysBefore←-⍨asIDN
        oneOf←{(⊂⍺)∊⍵}

        all←∧/ ⋄ not←~ ⋄ before←< ⋄ after←>
        no←~∘(∨/) ⋄ and←∧ ⋄ or←∨ ⋄ any←∨/
```

we define some further terms in the business domain

```
        isDeferred←OrigNRD≠Nrd
        isBefore911←OrigNRD before 20040911
        termOver5←OrigNRD≥5 yrsAfter StartDate
        termUnder5←not termOver5
        deferredBy5←Nrd≥5 yrsAfter OrigNRD
```

with which we can write business rules…

```
     :If not InUWP
         answer←doesNotApply
     :Else
         :If termUnder5 and RetirementAge≥75
             answer←refer

         :ElseIf RetirementAge≥75
             answer←doesNotApply

         :ElseIf ExitDate after oneMonthBefore 75 yrsAfter Dob
             answer←doesNotApply

         :ElseIf ExitDate before 183 daysBefore OrigNRD
             answer←any MVRs>0

         :ElseIf IsVista
             answer←ExitDate before ThreePolAnniversariesBefore OrigNRD
```

NB. this is not actual production code, but merely resembles it.

## Three-Wish Limit

I have argued against distinguishing software design from software construction.

What would software development look like if construction were immediate and free? Imagine that as soon as you had described what you want, you had it. That would be the world of fairy tales. Genies grant wishes for immediate delivery.

This is our world. When we and our users have worked out our description of what is needed, it is there.

In the stories there’s a catch to the wishes, and it is always the same catch. You get what you ask for but it is never what you want. Often it is the very opposite. And there’s a Three-Wish Limit. You get only three chances to describe what you want.

It is almost as if the old stories set out to warn us about the unreliability of communication. Did we stop listening when we grew up?

Unlike genies, we get paid, so there is no Three-Wish Limit. That matters, because working out what is needed from the system is too complex to get right in three tries.

You can think of conventional software development imposing an effective Three-Wish Limit through slow feedback. It takes so long to feed mistakes in the specification back to the designers: much work gets redone once; very little a third time. It takes a brave user or developer to propose a fourth try. So Three-Wish Limit becomes an expensive game of poker played between client and developer.

Our development work looks very unusual. At the end of most days, my work is complete. I have no outstanding work remaining, nothing on which to report status. I usually have a good idea of what we will work on the next day, but the users are free to move something else to the top of the list, and from time to time surprise me. They control the course of development.

The business users, knowing the complexities of what they want automated, are better than we are at predicting the time required. They often consult us in case we can see complications they don’t; but we do not generally estimate task effort. Not having status to report, we don’t often attend project meetings: our users plan and prioritise the work ahead without us.

Here’s how that can play in practice. Fifteen specialist clerks in another part of the division had been processing pension claims in another city. Managers had speculated for some time that the work could be moved to our city and the <acronym>APL</acronym> system, but no resources could be spared from the company’s business re-engineering project for analysing what it would take and specifying the system changes.

Finally two of our users visited the other city one Monday, and discovered that, as the managers had suspected, the work could be done using the <acronym>APL</acronym> system provided a few program changes were made. During their visit they announced they would be ready the following Monday to process the new claims. Since such migrations normally take months and sometimes fail entirely, it is hard to suppose they were entirely believed.

Back at work the next day, our users worked with us to change the <acronym>APL</acronym> system to handle the new work. By the end of Tuesday they had finished testing the changes, and they went into production. The following Monday, four clerks in our city smoothly took over the new business. All done.

What I love most about this story is that, while visiting the other city, our users never even phoned us.

Our customers never hear that what they want is “out of scope”. We can’t produce everything instantaneously, but they know we can produce what they want about as fast as they can work out with us what that is. As in <acronym>XP</acronym>, we are always working on their top priority; and they can change priorities at any time.

In this work we depend on our ability, supported by <acronym>APL</acronym>, to write terse code we can share with our users.

## References

1. <a id="1"></a><cite>Software Engineering Economics</cite>, B. Boehm, Prentice-Hall, 1981
2. <a id="2"></a><cite>Controlling Software Projects: Management, Measurement and Estimation </cite>  T. deMarco, Prentice-Hall, 1982
3. <a id="3"></a>Keynote address, W. Cunningham, Fourth International Conference on Extreme Programming and Agile Processes in Software Engineering, May 2003 (unpublished)
4. <a id="4"></a>“[Three Principles of Coding Clarity](trad/v184/clar184.htm)”, S. Taylor, *Vector* 18.4
5. <a id="5"></a>“[Notation As A Tool Of Thought](http://elliscave.com/APL_J/tool.pdf "download PDF 2.3Mb")”, K. Iverson, 1979 <acronym title="Association for Computing Machinery">ACM</acronym> Turing Award Lecture, *Communications of the <acronym>ACM</acronym>*, Vol.23, No.8, August 1980
