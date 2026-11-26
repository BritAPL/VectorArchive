---
vid: '10004850'
title: An APL Library
authors:
- Stefano Lanzavecchia
byline: proposal by Stefano Lanzavecchia
volume: '19'
issue: '4'
page: '8'
source: trad/v194/lib194.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2003
tags:
- APL community
- programming techniques
---

*proposal by Stefano Lanzavecchia*
{ .byline }

*“O Deep Thought computer,” he said, “the task we have designed you to perform is this.  
We want you to tell us…” he paused, “the Answer!”  
“The Answer?” said Deep Thought. “The Answer to what?”  
“Life!” urged Fook.  
“The Universe!” said Lunkwill.  
“Everything!” they said in chorus.  
Deep Thought paused for a moment’s reflection.  
“Tricky,” he said finally.*  Douglas Adams – *The Hitchhiker’s Guide to the Galaxy*

If day after day your typical task is to delete leading and trailing blanks in simple character vectors, invert small numeric matrices, and calculating the average of planes in arrays representing sales, you’ll certainly find that a naked APL interpreter coupled with the FinnAPL idiom list is more than adequate. But if your customers or the business requirements demand distributed applications, idiot-proof graphical interfaces, connectivity and the most trivial of the tasks you are faced with is to programmatically download a web page, parse part of it, calculate its CRC, store it in a database, zip it, and attach it to an e-mail, then you might feel let down.

APL interpreters over the years have developed into general purpose development environments with many different interfacing capabilities. So why are the support libraries still lagging behind so badly? Let’s take for example another language, a modern and popular one: Perl. Well hidden in the acronymic name are the origins of the language: a Practical Extraction and Report Language, that the creator used to do manipulation of text files. How comes that despite its modest origins and its admittedly sub-optimal syntax, it’s become one of the most popular languages ever? It’s interpreted but it does not offer a development environment like an APL session. It’s cross-platform but most of us program on one platform at the time anyway. It is free and open-source but professional developers don’t go looking in the bowels of their interpreter every day. The features of the language in itself do not seem to be enough to justify its popularity. We must look somewhere else.

Fire up your favourite web browser, input the URL [http://www.cpan.org/](http://www.cpan.org/) and check the list of library utilities available. Even if you have a slow modem it’s still worth letting the seemingly endless list download. There are more modules to connect to Database Engines than you thought there were databases. There are XML parsers, file compressors, e-mail management tools, web scrapers, and more, much more. Not only that: they are all free and well documented, since these are requirements to make it into CPAN. We can direct our attention to other similarly popular languages, featuring a not particularly exciting syntax, like Python or PHP, and find the same wealth of free supporting useful utility libraries. Even for more obscure but classy languages like ML or Haskell seem to be better supported than our beloved APL.

I believe this to be a serious limiting factor and I expressed my thoughts already in the editorial for Vector’s issue 19.1. Because of the positive feedback I have received, the BAA has decided to start a library project. Since the APL community is relatively small and we cannot expect a large amount of support, especially at the beginning, we will proceed in small steps.

At the moment we see the library as a repository for freely available code, decently documented. It would be good if all the different APL (and J and K) interpreters could be supported, and that’s definitely a goal, although it could take a while to fill all the gaps. As we begin the collection of what’s already available on the world wide web, we will need librarians, each one expert in a different field (networking, protocols, file and file system manipulation, databases, user interface, internationalisation, core utilities, and so on), to sort the code, rationalise the documentation, disentangle the dependencies and finally publish the polished work on a new website. Once the code is available online, we expect people to download it, find it more or less immediately useful, adapt it to their needs, improve it and hopefully make their contribution returning the improved versions to the library. This is how CPAN was born and grew, so we know it’s possible, even if it’s difficult.

The website hosting the library (what about *http://www.apl.ws/*, (I think all the www.apl.\* domains were gone, how about www.apls.ws? domain still available for purchase?) could grow overtime in a information portal, with bug-tracking features, a forum that supports the exchange of messages as well as APL snippets in the full beauty of their symbolic representation, newsletters and more. It could be really cool, as they say.

But to shoot so high now would be foolish, so let’s stick to more modest expectations in order not to waste our limited energies and resources. What we need now is a few volunteers to dirty their hands and start the collection and classification of available code before we even start producing new one. Morten Kromberg has offered his services as our Librarian, and he would love to hear from any volunteer “assistant librarians”, who should contact him ([mkromberg@insight.dk](mailto:mkromberg@insight.dk)). If you feel that an APL library is a worthy goal, have code to contribute or want to share your expertise in a field by contributing with your time, feel free to contact me at [stf@apl.it](mailto:stf@apl.it) and let the fun begin.

*“Tricky,” he said finally.  
“But can you do it?”  
Again a significant pause.  
“Yes,” said Deep Thought, “I can do it.”  
“There is an answer?” said Fook with breathless excitement.  
“A simple answer?” added Lunkwill.  
“Yes,” said Deep Thought, “Life, the Universe, and Everything. There is an answer.  
But,” he added, “I’ll have to think about it.”* Douglas Adams – *The Hitchhiker’s Guide to the Galaxy*

***
