---
vid: '10500730'
title: Sharing code – the APLTree project
authors:
- Kai Jaeger
byline: by Kai Jaeger (kai@aplteam.com)
volume: '25'
issue: '3'
page: '79'
received: '2011-12-23'
online: '2012-01-01'
editor: Stephen Taylor
description: The APLTree project is an attempt to make general tools available to the Dyalog programmer. This article discusses the motivation of the project and its details.
keywords:
- APL
- Libraries
- Code sharing
- Utilities
- Tools
- Code Management
abstract: The APLTree project is an attempt to make general tools available to the Dyalog programmer. This article discusses the motivation of the project and its details.
source: content/printed/253/apltree.htm
converter: vecrescue 0.1.0
issue_tab: n23-march-2012
tags:
- development practice
- APL community
---

by Kai Jaeger (kai@aplteam.com)
{ .byline }

![](featuredImg_sharing-code-the-apltree-project.jpg){ .lead-image }

The APLTree project is an attempt to make general tools available to the Dyalog programmer. This article discusses the motivation of the project and its details.
{ .abstract }

## Overview

Sharing code is something that never gained popularity in the APL community, and for good reasons: without having a proper tool for modularising code (classes or at the very least namespaces) sharing code is virtually impossible in APL due to the danger of name clashes.

With the introduction of namespaces in Dyalog APL things got better but there were still some obstacles. Namespaces don’t allow hiding implementation details: dotting into a namespace means you see everything that’s contained in that namespace. In a complex namespace that could mean you see a hundred functions although only, say, 10 of them are the actual “public interface”. So in order to use the functionality provided you need to worry about just 10 functions while looking at a hundred.

There is also the problem of how to prevent your dear colleagues from calling functions they are not supposed to call. If like me, you worked on teams who were supposed to follow such rules then you know how difficult a task that is.

Classes to the rescue: one advantage of the object-oriented paradigm is that you can see only the public interface when you dot into a class or an instance of that class. In other words, while namespaces allow you to modularize code and therefore to structure it, classes offer true encapsulation: a programmer only interested in using a class will see just the fields, properties and methods that constitute the public interface.

That does not mean that all the rest which is called implementation details cannot be investigated: You can still edit the class script, which allows you to investigate everything, not only the public interface. The code editor’s new tree structure emphasizes whether fields, properties and methods are public or not, and it allows fast navigation, too. Last but not least you still can trace into a class or even an instance, and you can look at everything you want.

Of course it is a big advantage when the implementation details are hidden: it makes using a class much easier. That one cannot call functions which are not supposed to be called from outside is very important, too.

Since the introduction of classes in Dyalog APL Version 11 there is no technical reason why developers should not share tools and utilities. However, this has not become popular so far. One reason is that many APLers still think along the lines: “If I can write it myself in a couple of hours why should I bother to learn the public interface of somebody else’s code?” Or in other words: if the amount of time it takes to master somebody else’s class is equal to the amount of time it takes to implement it yourself there is no point in sharing code, right?

Even if we ignore the simple fact that programmers tend to underestimate the time it needs to implement something by at least a factor of two this is also a short-sighted view: in total it needs much more time than simply implementing it: you also need to spend time on documentation and, even more important, on test cases. In fact these two tasks quite often take longer than writing the code in the first place.

It is also an important question who is providing the tools. Are they maintained regularly? Are bugs published openly? Can you buy support? These are all questions one might well ask when considering using third-party tools. But when all these questions are answered satisfactorily then you should remind yourself that there is no point in reinventing the wheel.

## Documentation

If you are a one-man-band you might be able to abandon the idea of spending time on documentation. You know how to use your tools, don’t you? However, things are different in a team. Since software development is normally done in teams these days you can’t escape spending time on documentation.

In practice old APL hands sometimes find themselves mixed into a team with other old hands at the start of a project. All APLers have their own set of tools. None is actually documented; they all wrote them for their own purposes. So from the start you have as many toolsets as there are team members, none of them documented. Not a good idea of course.

## Test cases

Test cases are considered mandatory nowadays in the IT industry despite the fact that the amount of resources consumed by the implementation of test cases is significant. Occasionally it exceeds the time spent on the implementation of the software.

Software projects become more and more complex. We know from experience that changing a complex system has all sorts of unforeseen and unwelcomed consequences. All of us have said more than once “I don’t understand why it’s crashing: I haven’t changed anything near this code.”

There is only one way to escape this problem: after a change, before making it available to the other team members, let alone the customer, run test cases.

## Improved quality

The most important reason for having test cases is that they are designed to improve the quality of any piece of software. This is always an important issue of course, but it is particularly important in projects which evolve dynamically, with a large number of software releases. By changing code you will inevitably break code but if you have a full-blown test suite ready to run against your changes you have a much better chance of discovering these problems.

That alone is a very good reason to spend the resources on implementing test cases. In the long run this will actually save time and money for that very reason.

## Test cases as examples and documentation

An implicit advantage of well-written and documented test cases is that they provide examples of how to use the software. That is a real time-saver for anybody supposed to understand and use a class.

This is more important for APLers than for other people. The reason is readability.

As a matter of fact APL is harder to read than code written in other languages. This is mainly owing to the fact that in one line of APL there is much more going on than in one page of COBOL, but it’s also due to the usually higher abstraction level.

Now APLers often argue that one can always work out easily what a function is doing by using a debugger and simply watch what the lines are doing with the data. Due to the outstanding debugging facilities of modern APLs this is certainly true. However, by definition it can be true only if correct data is provided to the function in question.

Now there’s a problem: when you get a bug report on your plate it’s quite likely to be caused by unexpected data, ill-formed data or data the application was never intended to deal with. With improper data you might have a hard time to find out what the code is supposed to do. It might be very costly or even virtually impossible.

By definition test cases provide valid data, so it is not a big deal to find out what the code is actually doing.

The big problem with written documentation is that nobody debugs it. Implicit documentation gathered from test cases is actually both, debugged and up-to-date. What an asset that is!

The idea of test cases has been around for a long time but only with the Agile Software Development paradigm did the idea really take off. The obvious reason is that in agile projects software is constantly updated in production, often every week or so. Whenever a developer has finished a task she is supposed to check in the updated code into the code repository. Before she is allowed to do so she must run the test cases. Only then does she stand a chance not to break the code.

## Conclusion

So, in total, when considering writing a utility which is already available somehow, one has to compare the time it needs to implement the software, write proper documentation, implement appropriate test cases and the time spent on making the software stable and reliable with the time it takes to master the interface of an already existing piece of software.

In total it’s fair to say that software coming without a proper documentation and/or without test cases should not even be considered a potential source.

There are no technical reasons any more which prevent Dyalog programmers from sharing code. It makes sense to provide solutions for typical every-day problems APL programmers are likely to face. With the APL Wiki the APL community has the platform to host such tools and utilities.

## The APLTree project

The APLTree project was introduced on the APL Wiki in 2009 without any effort to “sell” it: the idea was to get some flesh on the bones first.

APLTree is a pure Dyalog project: The OO features implemented by the different APL vendors are incompatible: something that was written for, say, Visual APL cannot be used in either Dyalog APL or APLX.

In July 2011 the project consisted of 10 utility classes and 4 tools. Note that all members of the APLTree project…

- come without any sort of copyright
- are documented at length
- come with test cases

These are in fact preconditions for becoming a member of the APLTree project. There is a page listing all of them: http://aplwiki.com/CategoryAplTree

There are two different types of tools available:

- Those supporting an APL programmer in the development process
- Those designed to be used within an application

All members of the APLTree project belong to one of these two groups. The page is reflecting this by dividing them into Tools and Utilities.

## I. The Tools

The tools are designed to support a programmer during her work somehow, so I am going to give a short overview about what the tools are actually doing.

### a) ADOC

ADOC is a self-contained class which extracts information from scripts, compiling an HTML page with all the pieces of information found.

### b) APLCode2HTML

APLCode2HTML is a simple tool which takes either a line of APL or the name of an APL function or operator or script and compiles an HTML page in order to display that code.

### c) Compare

Compare offers methods designed to compare functions, operators, namespaces and scripts or even workspaces with other functions, operators, namespaces and scripts located either in the workspace or on file. It can also be used as a merge utility.

It makes use of the brilliant third-party-software CompareIt! [\[1\]](#ref1) if available.

### d) ScriptManager

ScriptManager deals with scripts only. It allows loading, saving, comparing, editing and updating scripts. ScriptManager can deal with SALTed scripts and supports SubVersion [\[2\]](#ref2) as well as acre [\[3\]](#ref3).

ScriptManager’s GUI shows ...

- which scripts are different from their file version
- which scripts point to an invalid location
- which scripts differ from their SubVersion base version
- which scripts have SALT backup files, and how many

The most important actions you can perform from within ScriptManager are:

- Compare a script with the file version with CompareIt!
- Compare a script with any SALT backup files
- Compare a SubVersioned script with its SubVersion base file
- Compare a script managed by acre with the workspace version or an older version in the repository.
- Update all or selected scripts in the workspace with their file version
- Loading and saving scripts
- Delete a script from the workspace and/or from file
- Edit a script
- Manage scripts with the built-in Favorite Manager.

## II. The Utilities

All utilities in the APLTree project solve every-day problems almost all APLers come across during their daily work, at least potentially:

- Deal with the Windows Registry with `#.WinReg`
- Deal with files and directories with `#.WinFiles`
- Deal with INI files with `#.IniFiles`
- Get information closely related to Windows with `#.WinSys`
- Start programs with `#.Execute`
- Write log files with `#.Logger`
- Manage key-value pairs with `#.Hash`
- Display compiled help files (\*.CHM) with `#.ShowChmHelp`
- Write to the Windows Event Log with `#.WindowsEventLog`
- Create CHM files from APL with `#.APL2XML`; requires the third-party-software "HelpAndManual"[\[4\]](#ref4).

I was asked many times why #.WinFiles was implemented at all; after all we have .NET at our finger tips. Well, create a folder with 90,000 files in it and then try to get a directory listing first with #.WinFiles and then with .NET and you know the answer.

Documentation can be created by the means of ADOC; that makes ADOC the most important of the tools. That’s the reason why ADOC has its own article in this issue of Vector.

### The code repository

All APLTree projects are saved in two or maybe three different locations. The URL is: http://download.aplwiki.com/APLTree/

This page offers links to three sub pages:

#### Latest stable version

This is the latest production release. It only contains the script(s).

#### History

Every version which was once a “Latest stable version” is supposed to be represented here. However, because some members of the APLTree project are older than the project itself they are not represented with all their versions in the “History” folder.

Note that here the full project is saved, including all test cases.

If you want to get hold of the full project, in particular the test cases, this is what you want to download.

### Development

As soon as any development is carried out on a certain project it’s supposed to be saved in the development folder. Again the full project is saved. Note that only code that has passed the test cases successfully is allowed to be saved. That also means that no matter what you have changed you are supposed to execute the full test suite first.

As soon as a development branch is promoted to the “Latest stable version” folder all files belonging to that project are deleted from the “Development” folder.

## Contributions

Any kind of contribution is welcome, even if it’s just fixing a typo. However, for the time being it is only possible for a restricted number of administrators to save anything directly in the folders mentioned above. If you want to contribute please download the current branch or, if there is no such branch, the latest file from the “History” folder. When you’ve made a change somewhere and the test cases still execute fine than send your changes via email to kai@aplteam.com

I would love to get overworked by dealing with a wealth of contributions but right now I can handle it.

## References

1. <a id="ref1"></a> CompareIt! is an outstanding comparison utility. See [http://www.grigsoft.com/wincmp3.htm](http://www.grigsoft.com/wincmp3.htm) for details
2. <a id="ref2"></a> SubVersion is a widely used version control system. See [http://en.wikipedia.org/wiki/Apache\_Subversion](http://en.wikipedia.org/wiki/Apache_Subversion) for details
3. <a id="ref3"></a> *acre* is a version control system written in Dyalog for Dyalog by Phil Last (phil.last@ntlworld.com)
4. <a id="ref4"></a> HelpAndManual is an excellent help and documentation tool. See [http://www.ec-software.com/](http://www.ec-software.com/) for details.
