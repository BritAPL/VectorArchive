---
vid: '10013440'
title: Editorial 16.2
authors:
- Stefano Lanzavecchia
byline: by Stefano Lanzavecchia (stf@apl.it)
volume: '16'
issue: '3'
page: '3'
source: trad/v163/ed163.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2000
tags:
- performance
- APL in perspective
---

*by Stefano Lanzavecchia (stf@apl.it)*
{ .byline }

“*The pace of microchip technology change is such that the amount  
of data storage that a microchip can hold doubles every year.*”  
*Moore’s Law*

I doubt any computer professional these days ignores the so-called *Moore’s Law*, yet it was worth quoting it again in its original form. In 1965 when preparing a talk, Gordon Moore noticed that up to that time, microchip capacity seemed to double each year. The pace of change having slowed down a bit over the past few years, the definition has changed (with Gordon Moore’s approval) to reflect that the doubling occurs only every 18 months. Moore’s law is often quoted in terms of general computing power doubling at least every 18 months, forgetting that raw power is, in general, not enough. Last summer, my attention was caught by the web site of a researcher working for Microsoft, professor Todd Proebsting. It’s not my intention to sponsor the work of the professor, who seems to be doing quite well without my help, at least looking at the projects he’s been involved with so far, but I would like to quote here what he calls *Proebsting’s Law*, which is a paraphrased version of Moore’s law, with a rather more depressing conclusion: “*Advances in compiler optimisations double computing power every 18 years.*”

The evidence shown to support this assertion is a simple experiment. I will quote some more directly from its web site for those of you who are reading this text without a cellphone connection to their favourite palmtop to browse the web even during the distressing trip by train from York to Manchester Airport. *Run your favourite set of benchmarks with your favourite state-of-the-art compiler. Run the benchmarks with and without optimisations enabled. The ratio of those numbers represents the entirety of the contribution of compiler optimisations to speeding up those benchmarks. Let’s assume that this ratio is about 4x for typical real-world applications, and let’s further assume that compiler optimisation work has been going on for about 36 years. These assumptions lead to the conclusion that compiler optimisation advances double computing power every 18 years. QED.* Another way to put it is that, compared to the increase in hardware performance, theoretical studies and practical applications of research on optimising compilers, despite the large amount of resources they consume, contribute only marginally to the effective increase in computing power available to users. Proebsting’s conclusion is that Programming Language Research should concentrate on more effective topics than compiler’s optimisations. *Perhaps programmer productivity is a more fruitful area.*

Since APL is not a compiled language, how does this law relate to APL programmers? Let me throw another external contribution in, before I come to some conclusions of my own. This morning, while I was collecting my thoughts to write this editorial, I came across another very interesting article, which not only is written in good English, but which puts in a nicely organised way opinions that I mostly share with the author. The article’s title is “Are Very High-Level Languages Really High-Level?”. Please check the appendix for a reference to the article on the web. In the five dense pages of text, the author basically expresses the opinion that, while end-users’ tools have progressed immensely since the days of teletypes, programmers’ tools, that is tools used by programmers and meant for programmers, have not. Many developers are convinced that the simple tools they use allow them to be more productive than if they used anything new, developed since they last learned how to use a text-editor. Good for them. I don’t buy into that. The development environment provided by Microsoft Visual C++ is amazingly more powerful than anything that we do-it-yourself hardcore APLers ever even imagined could be done. Yet, more complex is not necessarily better, but this is not the point. In the early 90s a whole lot of new programming languages, known as Very High Level (to distinguish them from the first High-Level ones, like COBOL or Basic or even Pascal) were introduced, to simplify the job of quickly prototyping concepts, but they have not evolved a lot since. In some ways they have regressed by becoming too complex without a good redesign to incorporate all the new ideas.

If I put together the thoughts of the two authors, I can come to these conclusions: a compiler producing optimised code historically does not seem to have helped a lot programmers (who in general pay for the optimisation job with extra wait time) nor end-users; APL is not compiled, and its being interpreted is, in my opinion, a major bonus for a software developer. It allows short code-debug cycles because the developer can at any time not only check the state of the data, but interact with the data, experiment with the data, while running the application. A symbolic debugger like the one offered by Microsoft’s Visual Studio permits some of this, but the way in which both code and data can be inspected and modified on-the-fly in an interpreter like APL is still unmatched. Yet, this is not enough to make the life of a developer easy. The develop-ment environment of all the commercial implementations of APL is so retro (not to say worse) that any improvement would be well accepted, like the new debugger in APL+Win.

And we haven’t even started tackling the issue of language features. In short, APL was no less than two decades ahead of many today’s very high-level languages. Somehow the slow pace of improvement of APL interpreters has now put the language back in the group. Are we all really so happy of the way we coded (I wasn’t to be sincere, I was more likely trying to learn how to read and write) in the 70s? I will never claim that a fancy GUI makes life easier, but it’s true that programmers’ productivity is always hardly taken into account when new features are added to a language, and this is also because they don’t seem to care. What if they cared?

## References

Moore’s Law: [http://www.eco.utexas.edu/undergraduate/Forum/ML/index1.htm](http://www.eco.utexas.edu/undergraduate/Forum/ML/index1.htm)  
Todd Proebsting’s homepage: [http://www.research.microsoft.com/~toddpro/](http://www.research.microsoft.com/~toddpro/)  
Are VHLLs Really High-Level?: [http://www.oreilly.com/news/vhll\_1299.html](http://www.oreilly.com/news/vhll_1299.html)

***
