---
vid: '10501500'
title: 'Q Tips: Fast, Scalable and Maintainable Kdb+'
authors:
- Yang Guo
byline: by Yang Guo (yang_guo@mfe.berkeley.edu)
volume: '26'
issue: '4'
page: '107'
received: '2015-06-06'
online: '2015-07-09'
description: 'Book review of Q Tips: Fast, Scalable and Maintainable Kdb+, author Nick Psaris'
source: ../../php-site/content/printed/264/qtips/qtips.htm
converter: vecrescue 0.1.0
issue_tab: n4-march-2016
tags:
- reviews
- programming techniques
---

by Yang Guo (yang\_guo@mfe.berkeley.edu)
{ .byline }

For most people who approach a new language, the first question asked usually isn’t about syntax, features and functionality, but rather “what can it do”. Q, being as terse as it is, has relied on snippets, anecdotes and reference guides to try to answer this question, however there so far has not been a very satisfying answer. Whereas other languages have tutorials that showcase its strengths, q has not had a comprehensive, clear answer to “what can it do”. Nick Psaris’s book Q Tips aims to be one answer for this question, with a very obvious focus towards quantitative analysts. The overall premise of the book is already grand in its nature – the user should have a fully working complex event processor (*CEP*) by the end of the chapters, however the most impressive aspect is the seamless interweaving of usable code snippets, introductions to basic language features and building the codebase up in a well-paced and disciplined fashion (not to mention a github full of examples from the book).

Another noteworthy aspect of the book is that it doesn’t treat q purely as a database language but rather a data analysis language. This is a big shift from common perception, as kdb+/q is often compared to SQL and NonSQL databases rather than data processing languages like R and Python. Q Tips tries to show this often-neglected side of the language, giving examples that belong both in a library for data and quantitative analysts in addition to database administrators and developers performing operational support. Not only will you learn how to create and manage various database types available in kdb, you will also know how to create frameworks for aggregating data, multiple ways of summarizing results and a very sophisticated method of creating simulated time series.

## Getting started and generating data

Even from the first steps, Q Tips makes no assumption that the reader has expertise in running Q and diligently walks through every aspect of starting an instance. From the detailed explanation of reserved keywords to an explanation of the banner itself, the book leaves no stones unturned. Nick goes through common shortcuts seasoned users of the language take (such as creating aliases), and even shows some tips that even seasoned q users may not know, such as piping input into the Q instance on startup. The basics of the language are introduced clearly and concisely; Nick clearly observes the “show not tell” philosophy as after each brief description is an example that clearly shows each language feature.

Even in this section, before the language has been formally introduced, Nick is already presenting usable examples. For example, in showing the difference between q and k, he shows how easy it is to write a percentile function, something that will be used later on in developing the CEP. Although the example is simple, it clearly showcases the difference between q and k, as well as giving the user a taste of the power of the language.

This method of concise explanations with extremely helpful code examples carries on to the introduction of basic data types of the language. Again, with the quant emphasis in mind, Nick’s examples go beyond toys and introduce practical, usable code. A particularly interesting example in this chapter deals with random selection with and without replacement – in two lines of code we see how easily it is to define a deck of cards as well as deal it out. The concept itself is useful (e.g. a poker simulator), and can be extended to any problem that requires creating a set of unique items and dealing them.

## Functions, code organization

With the basics of the language out of the way, Nick shifts gears and starts delivering on very common functions needed in almost any type of simulation based setting. While introducing q as a functional language, he uses two methods of generating normal random variables (Box-Muller and Beasley-Springer-Moro) from uniform random variables as an example of q functions. In this chapter, we also start to see core programming concepts such as error handling, vital in creating robust production code.

This is carried on into the chapter on code organization, where Nick goes into detail about code organization, libraries and how q interacts with its outside environment. These details are very important when it comes to deploying and maintaining code, and until now I have not really found a resource that describes it so comprehensively. From this chapter it’s very apparent that the book is written by someone with experience deploying production q code. One particularly clever example for namespace scoping is the `.util.use` function, which allows the user to move a function from a namespace into global scope.

After these chapters, the user has already seen ample examples of the basics of the language, and has multiple working statistics functions and utilities in their toolbox!

## A Random walk and tables

One of the powers of q is treating data sets as tables, and allowing for easy creation, updating and manipulation of these tables, far beyond what traditional SQL can offer. In these chapters, Nick goes through the most important aspects of creating lists of data and converting them into tables. Q Tips take an extremely practical approach again with these chapters, showing off the language using examples that seasoned q users have used on the job often. More importantly, he focuses on small but critical parts of data manipulation, often overlooked but significant pain points in data analysis. From reversing a table to rearranging columns, these examples are explained intuitively and builds upon the previous chapters.

The chapter on large tables is probably the most ‘traditional’ chapter in the book, dealing with the bread and butter of q. In this chapter Nick goes through two core concepts of the language – eaches and attributes. While these might not matter to much to a new user, they are absolutely critical when writing optimized q code for large tables, where the difference in algorithmic efficiency is clearly visible.

## Trades and quotes and CEP engine components

Even though we have already learned a massive amount, and have been exposed to some very critical code pieces in the previous chapters, this is technically the first non-introduction chapter – which is a feat in itself as we already have enough working knowledge of the language to go do something useful. This chapter is extremely valuable for anyone who has ever wanted to run sensitivity analysis or a strategy backtest, as we literally build a full quote and trade time series using the knowledge we acquired in the previous chapters. The chapter describes generating a very realistic quote and trade table, and shows off some very cool (and fairly unknown) language features. Two particularly interesting features of the tables Nick creates are varied tick sizing depending on security price, using a clever language feature that overloads the sorted attribute, and randomly delaying simulated trades based on quotes that is more in line with what is actually observed in the market. As with the rest of the book, these features are outlined clearly and the code to take the function from an idea to full implementation is laid out step by step. By the end this chapter the reader has a fully simulated and extremely extendible quote and trade table, with timestamps down to the nanosecond, something that is extremely hard to do in most other data processing language!

The next chapter switches back to more of the developer operational pieces by defining a very flexible timer, adding in logging and finally parsing command line arguments. Again, these are absolutely critical in a production environment, and Q Tips is really showing the user how a production level product could and should be built. By the end of this chapter we have all the raw components for running a CEP engine.

## Running a CEP engine, security and debugging

The culmination of the previous chapters all lead up to this – combining the skills we have gained in starting/stopping a q process, creating modular functions and creating simulated high frequency data, we can now build our own CEP. As with many other learning experiences, I feel that the journey is more important than the destination, but it is very satisfying to see all the previous chapters combined into one very tight and robust system. This chapter also includes information on loading data from files and the various ways to do so, which is useful for any practical data analysis.

## Q-SQL

As I have mentioned before, it is very obvious that Nick sees q as much more than just a query language – the first mention of Q-SQL being in chapter 14 of the book is testament to this. That being said, this is probably one of the best treatments of Q-SQL I have seen so far as well. As with the rest of the book, Nick takes the practical route with the introduction of the syntax and examples, and identifies pain points that many new users have as they approach the language. This goes beyond the normal treatment of Q-SQL, as exemplified in the section about `exec` as well as pivot tables. In here, Nick really digs into how `exec` works, as well as the relationship between a `select` statement and an `exec` statement. His examples on creating a simple `ohlc` table using `exec` versus `select` truly shows off the language’s power, as it’s easy to see how easy it is to generalize summaries across arbitrary data sets and with any arbitrary metrics in an extensible fashion.

In addition, his section on pivots is probably my favorite section in this book. For many areas in data analysis, a pivot table is one great way to generate a fast set of results to ingest before doing more complex analysis. Many languages have built-in pivot functions, however since they tend to be fairly rigid in functionality you are locked into whatever functionality they provide. Nick shows off the flexibility of q by going through multiple iterations of building a pivot function, and then finally creating one that is simple to use and easily extensible, giving the user far more control than any stock function allows.

The final section on joins shows off the various ways that kdb allows you to join data. While this chapter is as well written as the previous ones, I feel this is the most covered topic for kdb+/q documentation. One item to note is the emphasis on time-based as of (`aj`) and window (`wj`) joins as opposed to the more regular ones (union, left and each join). For time series analysis I find myself using `aj` more so than any other type of join, and a detailed coverage of this is very welcomed.

## Big data and beyond

The rest of the chapters talk about more domain specific problems such as partitioning, compression, remote access and custom aggregation. These chapters round off the knowledge needed to create something in q, and give the background knowledge for making an informed decision of how to build out a database as the need for larger data sets arises

## Conclusion

By the end of the book you will have taken a giant leap towards being a seasoned q expert. Not only will you be now well equipped to deciding how to process and store data, but you will also know many intricate details of the language such as how to best use exec statements (giving you far more flexibility than SQL), how to combine data to make analysis efficient and how to write modular statistical functions to help in statistical analysis and simulations. Most importantly, however, you will know how to think in the ‘q way’ – with transparent, terse code, modularity and flexibility through projections and each statements as well as very purposeful decisions in terms of data attributes. You will find that when you ask yourself “what can it do” when it comes to q, you can answer this question not just once, but in multiple ways. Just remember to keep it terse.

Q Tips: Fast, Scalable and Maintainable Kdb+  
Published by: Vector Sigma, Hong Kong SAR (2015)  
ISBN: 978-988-13899-0-9  
[http://q-tips.net](http://q-tips.net/)
