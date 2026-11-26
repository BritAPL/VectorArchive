---
vid: '10006500'
title: 'Editorial: Interesting Times'
authors:
- Duncan Pearson
byline: by Duncan Pearson
volume: '13'
issue: '1'
page: '3'
source: trad/v131/pearson131_3.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1996
tags:
- user interfaces
- APL in perspective
---

by Duncan Pearson <span class="affil"></span> <span class="email"></span>
{ .byline }

We are at an interesting time in the development of computer software. For the first time in ages the users of compiled languages such as C and Pascal have better facilities for the production of good user-interfaces than the users of APL.

It is frightening to compare the development of an environment such as Microsoft C++ with that of the competing APL products over the last three years. The reason it is frightening is that it implies an erosion of the advantage that APL developers have traditionally enjoyed over their counterparts in the mainstream of the software industry. To a certain extent our customers were prepared to put up with a product being a little slower and a little lacking in fancy features if it were delivered on time and if during its development it could change to reflect myriad changes in specification. These things are both valuable to customers, whose requirements change and who need the software for tasks today, and difficult to achieve. If at any stage it is seen that an ordinary developer using Visual Basic or Delphi can be as fast and as flexible in development and yet produce a faster and fancier system, then a number of APL developers will find themselves either out of jobs or using a different language – one more suited to the computer than to the problem solver.

Martyn Adams in an article in Vector 9.4 explained why he had had to abandon APL in favour of Visual Basic. That was over three years ago when VB was slower and offered a truly horrendous programming environment in which to organise a large system. When in April I looked at Visual Basic v.4 (as part of some work in developing a Causeway DLL for J) I was staggered by the range of controls available and the extent to which the developer could customise their behaviour. When as part of the same work I compiled and ran my first C++ project (created from a wizard and at that stage with none of my own code) I was in for more surprises. I had something that handled a menu, a docking toolbar, all the horrible file menu functionality and that even registered a file-type so that I could double-click one of my files on the Windows 95 desktop and start the application.

Why can’t we have these things in APL?

Should we want them anyway?

In his article in this issue about the use of Delphi from APL, Eric Lescasse examines ways of including some of this kind of interface functionality in an APL system but he also hints that there are ways in which a Delphi application can call routines in an APL server to do manipulations of data. Also for those who use J there is support for this kind of thing built into the language. As Eric Iverson demonstrated at the Association meeting in April it is possible to use the interface development tools of VB, Delphi or C++ and to call J in a dynamic link library to manipulate data. The resulting system appears as a small fast executable with a small J DLL in tow, is easy to install and uses the buzzword “OLE” technology which makes it even more attractive to clients. Furthermore it allocates memory dynamically so that one doesn’t have to guess the size of the user’s data in order to set the workspace size.

It is not clear whether the APL interpreter developers are challenged by what ISI have done with J. It is quite possible that the majority of APLers are happy in an entirely APL programming environment. That is clearly what the interpreter developers think and indeed the meeting of GSE (the European IBM user group – reported in this issue) seemed to bear this out. The users of APL2 said that they would prefer Windows features in the APL to the kind of DLL support I am describing. Only time will tell who is right but it is clear that the APL development environments have much that J does not offer. To those used to developing a system in a workspace the J method of defining functions and data in scripts comes as a surprise. In APL to change a function I double-click its name with the mouse, moving to any called function in the same way. In J I have to know in which script the “verb” was defined and then open that script file and find it. The APL session from either Dyadic or from APL2000 offers significant advantages over the J session, APL+Win in the area of syntax colouring and debugging facilities, and Dyalog in the area of workspace exploration (see the review of Dyalog 8 by Bob Byers).

There are challenges here for both APL and J. If half the time that is spent on developing the APL GUI facilities were put into providing the kind of OLE integration that J enjoys then I would never have considered changing to J. These facilities should be available to all APL programmers and if we were given them I believe that the community would be more productive as a whole. We would stop comparing the size of each other’s interfaces and get on with designing and talking about fast powerful server systems.

All this playing with J has made me realise that while the languages share a common heritage there is more to the difference than meets the eye. It is not in the spelling or character set, which is mere syntax, but is deeper in the semantics. To help others like me to approach J from the standpoint of an experienced APLer, and to give the pure J folk a flavour of what all these funny APL symbols mean, we have the first of what I hope will be a series of articles exploring common programming problems from the point of view of both languages. There is an article from Norman Thomson in which he applies some of the operator-based thinking of J to APL2 programming and there is an article by Chris Burke on the use of the Rank conjunction (operator) and how it simplifies some common APL problems.

Since the inception of J in 1990 there has been a debate within the APL community about whether J belongs or not. The annual conferences when they have included J have tended to split into two groups of delegates and I know many readers of Vector who completely ignore one or other half of the magazine. The users of one dialect have in general little or no interest in the use of or developments in the other. However I am a firm believer that APL and J belong together. The dialects share a great deal more than divides them. For a user of one dialect, who has already conditioned himself to think in a way that suits the array basis of the language, the main hurdle in the learning of the other dialect is over. I also value Vector’s position as being one of the few forums in which users of both J and APL can share opinions and read about developments in each other’s worlds. Long may it continue as such.

(webpage generated: 6 December 2006, 12:40)
