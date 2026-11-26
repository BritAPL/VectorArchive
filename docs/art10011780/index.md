---
vid: '10011780'
title: Alive and Well
authors:
- Morten Kromberg
volume: '23'
issue: '3'
page: '34'
description: Vector, the journal of the British APL Association
keywords:
- algebra
- APL
- array
- computation
- computing
- J
- K
- mathematics
- programming
- programming language
- programs
- Q
source: trad/v233/kromberg.htm
converter: vecrescue 0.1.0
issue_tab: n3-june-2008
tags:
- APL in perspective
---

> This article is discussed at  [comp.lang.apl](http://groups.google.com/group/comp.lang.apl/browse_thread/thread/627d50ec72abff24/)
> { .right }
>
> ## Editor’s Note
>
> I have reproduced this, originally posted at comp.lang.apl, in part to mark the end of a certain conversation. Many heads now gray or simply shiny believed when younger that one day all programming would be done in APL or something recognisably like it, and that software development would resemble what we do. Nothing like that happened.
>
> After years ‘in the wilderness’ there are signs Iversonian languages are once again attracting wider interest. There is nothing in this to wonder at. Programmers are not markedly less susceptible to fashion’s tide than other humans.
>
> There *is* a question as to how APL once lost respect, but it is not a very interesting conversation. It is a verbal wake, welcome neither in the pages of *Vector*, nor at meetings of the British APL Association.
>
> But a closely-related question is interesting. In what current roles are the APLs likely to enjoy respect? How should we describe their virtues, and to whom? *Vector* welcomes this conversation.
>
> [Dyalog Ltd](http://www.dyalog.com/) is a Sustaining Member of *Vector* one of the leading APL vendors today. Morten Kromberg thought long and hard about the future of the APLs before investing in the company and becoming its Chief Technical Officer. His post at comp.lang.apl kicks off a conversation *Vector* is happy to host.
>
> Stephen Taylor

## Alive and well

## Morten Kromberg [mkrom@dyalog.com](mailto:mkrom@dyalog.com)

It *does* seem to be true that:

1. Some APL vendors never successfully made the transition from the mainframe to the PC.
2. Some APL programmers with grey or white hair only see others with the same hair colour, and do not know any of the many young APL programmers who joined the community recently.
3. Unlike in 1980, it is very hard to find a consulting job where all you need to know is how to write APL programs.
4. It is almost impossible to interest Computer Science departments in APL.
5. If you work in a traditional software department you are unlikely to be allowed to start a new project in APL. Software engineers still don’t think APL looks like anything they learned at school and that it is ‘broken’. The fact that they can’t deliver software on time, on budget and to spec is only just starting to shake their faith that they are doing the right thing.

Over the past few weeks, we have seen a number of people post messages at comp.lang.apl, expressing the belief that APL is dying, due to various combinations of the above. Fortunately, it is simply not the case: APL and other array languages are well established in certain niches. The value of solutions based on these technologies is measured in billions of dollars per year, and their existence is *not* threatened.

It *is* a hard time to be an APL programmer. I think it is fair to say that most of us once chose APL because it offers a very high level of abstraction, allowing us to very quickly turn ideas involving data manipulation into executable code. But today, the platforms upon which we are expected to deliver software are changing rapidly, and we are expected to take interest in the very issues that we picked APL to avoid getting distracted by. In particular, user interfaces and databases now require extensive layers of toolkits, and these layers are shifting under our feet. XAML, Silverlight, Virtual Machines of one description or another. ODBC, JDBC, ADO, ADO.NET. We are expected to embed APL software in browsers which are not allowed to use local storage, even though this is an obviously bad idea for software which analyzes huge amounts of data. Remember CICS? It’s the same story all over again. Traditional web development techniques are another example of mainstream technology trying to beat APL into a mould created for lightweight transactional systems. Keep your data in an SQL database, look at one record at a time…

Architectures get more and more complex. If you are a computer scientist or software engineer, rewriting the software every few years in order increase the beauty of the code *is* what you love – why you chose the job. If you are an APLer, chances are you find all this depressing. Fortunately, while the higher layers of tools are still in a state of flux, the lowest levels of the new infrastructures *are* now stabilizing. We should soon see tools and interfaces from vendors that allow APLers to integrate solutions with popular platforms without all of us having to go out and learn the details of the latest advances.

The big APL vendors of old, staffed by people who loved APL and often had the same problem-oriented (rather than technology-oriented) attitudes, were slow to realize the impact of the technology shift. Consulting, timesharing and licensing revenues dropped sharply as the PC took over from the mainframe. Almost no marketing has been done since the late 1980s, and no new educational materials have been produced – with the notable exception of those written for J. The traditional vendors were caught in a negative feedback loop. Several did not pull out.

Accelerating the decline was the fact that much of the early revenue was a function of a huge but fundamentally temporary business in the 1970s and 1980s, where all large companies were building their own software for just about every purpose. APL was one of the best tools on the market for this pioneering work. However, the production of – and competition in the market for – the ‘shrinkwrapped’ software which replaced most of the APL systems written in this period is a different game entirely: key factors are a cheap and easily recruited/fired workforce, documentation, procedures, and marketing. I think that most APLers that I know would not want to work in these jobs, which the industry is trying to turn into outsourceable ‘unskilled’ jobs, even if APL were an acceptable technology to these businesses.

We can argue about whether this or that system which was converted would have been cheaper to maintain and more useful if the technology had remained APL. I have personally witnessed many extraordinarily bad business decisions to replace APL systems, made by management teams with absolutely no understanding of what they were doing (some of whom are now back in the fold as customers, I am happy to report). But in many situations, the business logic which caused managers to replace the use of APL by more ‘predictable’ workforces and technologies are ‘sound business decisions’. It is in the nature of APL to be used as an executable specification language: APL is often used as much to discover the specification as to implement the system. Having an executable spec makes ‘agile’ interaction with the user much easier than with many other technologies.

In many businesses (the financial industry being the prime example), the specifications never quite settle down. Fortunately, APL interpreters are highly efficient and capable platforms, which support the use of APL in high-volume production applications. Although APL is dynamic and interpreted, APL-coded solutions in this market often perform better than traditional solutions. I believe that this is because the layered approach of traditional software development tends to drive the development in the direction of reuse of overly general components, which need to be bent a bit too much and therefore perform badly. Too many layers in an object hierarchy lead to inefficient code, even after compilation.

So yes: there *are* many large APL (based) businesses which have died – but the family of APL languages will not die unless they are one day replaced by something *more capable* in the areas where APL is strong. This is quite simply because it provides unique value in a number of situations. The market that these situations represent is rapidly growing, and very poorly served by other technologies, so there is significant room for growth for APL, J, K and other new dynamic and array-oriented software development methodologies. The mainstream software industry is only just starting to shift its focus from administrative systems to analytical software, and we have a 40-year head start. If you look at trends in modern programming languages, they are heading in *our* direction: dynamic languages, inference in languages like C#, growing use of ‘Reflection’ (inspecting types and doing different things depending on what was passed to you), all of these are signs that mainstream technology is waking up to the fact that if you want to write code quickly, and especially if it needs to do anything analytical, you need the kind of flexibility that APL already has.

This would be a *very bad time* to start moving the language towards where everyone else is now!

I admit that:

1. The golden age of cushy (mainframe) consulting jobs will not return.
2. Trying to sell APL in head-to-head competition with Java and C# for traditional ‘software construction’ projects is almost impossible.
3. APL is pretty useless in a Computer Science department. It is not the goal of these institutions to teach the art of extracting specifications or the construction of working software so APL has little to offer them (OK, I admit have a chip on my shoulder: I dropped out of CS pretty early on after discovering that only 15-20% of the marks were given for arriving at a working solution).

However, unless you specifically want to achieve one of the above three goals, there is no reason to despair. The market for turning complex ideas into working software is growing rapidly, and all the brilliant ideas that Ken Iverson and the early APL developers put into the language are more relevant than ever before.

Today, the successful (growing) users of APL seem to be entrepreneurs (or entrepreneurial businesses) with novel ideas that they want to bring to the market ahead of the competition, or before conditions change. If you want to be an ‘APL consultant’ (internal or external), you must learn how to wrap APL as components to be embedded in – or communicate with – mainstream components. You must learn about SQL and XML and other emerging technologies, and how to help the domain experts who have the ideas and the skill to write APL for the application core.

In rare cases, one person can do all of the above (for a while), but generally we need to build teams combining domain- and technical experts. The vendors must do what they can to make the jobs of both these groups easier, and also explain the mechanics to managers who need to understand how to make this work.

At Dyalog, we firmly believe that we can safely ignore the decline of the APL market which followed the unnatural blip in the 70s and 80s, even though some old APL systems are still in their last throes. It is a false signal with respect to the fundamental value of the technology. The truth is that APL is actually still ahead of its time. It is our good fortune that we never had a mainframe business, as it means that for us, APL has been always been growing – and that growth now seems to be accelerating. We believe that we can further accelerate the growth through:

1. Working hard to improve integration with platforms (Microsoft.Net, Mono, perhaps Java). We’ve been working hard for the last several years, with the addition of the .Net interface, Object Orientation and finally Unicode support in V12.0, we feel we have made big steps in the right direction. Although it is one of the fundamental values of our product that people who are not software engineers have a complete package which allows them to build all the components of an application, we must also support large organizations who want to build multi-layered, and ‘multi-tiered’ applications using a combination of teams skilled in APL and other technologies.
2. Producing new books, training materials, ‘code patterns’ – and provide new tools for sharing code between APL users. Expect to see the first results of this work to appear during 2008.
3. Cultivating contacts in the dynamic/agile programming communities, where we believe we will soon see the first openings for marketing APL directly to developent teams (as opposed to going via the users first).

We don’t think it will be long before we’ll be back in the business of cold-calling certain types of development teams to sell APL as a modern technology. We are already working to start new academic programmes using APL. It is too early to say when we will succeed with this, but we are optimistic.
