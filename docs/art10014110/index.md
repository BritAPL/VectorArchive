---
vid: '10014110'
title: News from Sustaining Members
authors:
- Dyadic Systems Ltd
- MicroAPL Ltd
- APL2000 Inc.
- Causeway Graphical Systems Ltd
volume: '18'
issue: '3'
page: '9'
source: trad/v183/sus183.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2002
tags:
- implementations
- APL community
---

## Dyadic Systems Ltd

<b>Dyalog.Net</b>  
Dyalog.Net is now in the final Beta Test stage and will be ready soon after the availability of the Microsoft .NET Framework, which is imminent.

Dyalog APL for the Microsoft .NET Framework is the result of a collaborative effort between Dyadic and Microsoft that began in December 1999. Working alongside Microsoft as the .NET Framework code has been developed, Dyadic has integrated Dyalog APL with the .NET product so that all of the features and capabilities of this new computing platform will be available to Dyalog APL programmers the moment .NET is shipped by Microsoft.

The Microsoft .NET Framework is a highly object-oriented technology whose functionality is implemented in terms of .NET Classes. It comes with a huge number of built-in classes for performing all manner of tasks, including an object-oriented implementation of the Windows GUI and a set of classes for developing Internet applications. A third-party .NET class industry is already emerging.

Building upon the namespace technology and syntax that is central to Dyalog APL, Dyalog.Net provides syntax for using .NET classes from APL that closely follows the syntax used by other programming language. Dyalog.Net makes it as easy to use .NET classes from APL as from C# or VB. Furthermore, sample C# and VB code published in help files and manuals can readily be transposed into the APL environment.

<b>.Net Classes</b>  
Not only is it easy to use .NET classes in an APL application, it is also very simple to write new classes in Dyalog.Net which can then be exploited by other programming languages and development environments. For example, you could write a class that exposed a whole set of actuarial formulae and calculations (easily and efficiently expressed in APL) which can then be called directly by an application written in Visual Basic, or from an Excel spreadsheet.

<b>APLScript, DLLs and .EXEs</b>  
Dyalog.Net includes APLScript, a scripting version of Dyalog APL that supports the use of APL in character based ASP.NET Web Pages and Web Service script files. APLScript may be used with or without traditional APL workspaces, giving you a variety of options for implementation.

.NET Classes written in Dyalog APL are saved for you as DLL files and are indistinguishable from classes written in other languages. You deploy a Dyalog.Net class by simply supplying the DLL file together with DYALOG10.DLL, the Dyalog.Net engine.

Also new in Dyalog.Net is the ability to package a workspace as a .EXE that can be distributed as a stand-alone program depending only upon DYALOG10.DLL. This facility applies whether or not your application makes use of the .NET Framework.

<b>Internet Applications</b>  
The .NET Framework is not just about classes; it includes some major new technologies for implementing Internet and Intranet applications and Web Services. Through Dyalog.Net, all these new technologies are immediately available to the APL programmer.

ASP.NET is a new Microsoft technology for publishing web pages. ASP.NET supports server-side scripts, including APL scripts, that create web pages on the fly. Now you can very easily harness the power of APL to the Internet and generate pages that are not just simple database lookups, but contain the results of really complex calculations.

Web Services are classes that expose their functionality over the Internet. Typically, a Web Service contains methods that take arguments and return results. You can think of a Web Service as a set of remote procedure calls, but calls that are processed over the Internet. Dyalog.Net allows you to develop, debug, test and implement Web Services in APL. This means that, subject to security controls, you can quickly and easily make your APL functions directly accessible to any .NET program running on any computer that is connected to the Internet.

Whether implemented as Web Pages or Web Services, Dyalog.Net APL code is controlled and run by the ASP.NET engine inside Microsoft IIS. Dyalog.Net web applications are managed in the same way as other Internet applications and require no specialized knowledge for deployment.

Dyadic has surely removed the final barriers for the successful exploitation of APL on the Web; the rest is up to you!

## MicroAPL Ltd

Our new cross-platform *APLX* range of interpreters for Windows, MacOS, Linux and AIX - announced in Vector 18.2 - has received an enthusiastic response from APLers around the world. (Our favourite comment came from a user who described it as ‘my best Christmas gift’.) We thought we would focus in this issue of Vector on two areas which we are often asked about.

**Portability of APL code** The *APLX* interpreter itself is 100% upwards-compatible with our previous APL.68000 and APL Level II products. This means that the language specification is very similar to IBM’s APL2, with the addition of a number of commercially-oriented extensions (but without complex number support). IBM APL2 workspaces can be imported using <tt><font face="APL2741" size="+1">)IN</font></tt>. Users have also reported that converting code from APL+Win is generally straightforward. See our web page [http://www.microapl.co.uk/apl/spec.html](http://www.microapl.co.uk/apl/spec.html) for a summary of the language features.

Of course, the real compatibility issues arise not so much in the core APL language, but in the operating-system and GUI support. User-interface programming in *APLX* is based on the <tt><font face="APL2741" size="+1"> WI</font></tt> system function, and we have tried very hard to make this as compatible as possible across MacOS and Windows. This means that *APLX* applications can easily be written to be platform- independent, adapting automatically to the look-and-feel of Windows, MacOS 9 or Mac OS X.

We have also ensured that old APL.68000 and APL Level II Mac workspaces can be loaded and run with very little change, although in some cases it will be necessary to <tt><font face="APL2741" size="+1">)COPY</font></tt> new versions of some of the utility workspaces supplied with the product. The <tt><font face="APL2741" size="+1"> WI</font></tt> implementation of *APLX* is upwards compatible with our previous Mac products, with a few minor exceptions.

Existing APL+Win GUI code, written using the <tt><font face="APL2741" size="+1"> WI</font></tt> facility provided by APL2000, Inc, will require some modifications to run under *APLX*. However, we have tried to retain as much compatibility as possible, so that in most cases it is reasonably straightforward to take an existing APL+Win GUI application and convert it to run under *APLX* on Mac OS 9 or OS X (or Windows, of course). Dyalog APL applications are likely to require bigger changes.

<b>Runtime licences</b>  
MicroAPL pioneered the concept of freely-distributable runtime versions of APL, and many users have asked whether this facility will be available for *APLX*. The answer is that we will shortly be announcing an *APLX* *Packager* which takes your *APLX* workspace, and converts it to a standalone Mac or Windows application, which you can distribute without paying royalties to MicroAPL.

If you would like to be kept informed of this and other developments, please fill in the feedback form on our web site [http://www.microapl.co.uk/apl](http://www.microapl.co.uk/apl) .

## APL2000 Inc

<b>APL+Win Version 4.0</b>  
APL2000 Inc. is pleased to announce the availability of APL+Win version 4.0 for Windows 95/98/NT/ME and 2000 in the United States and Canada. The International Version will be available shortly.

The most remarkable enhancement to this version of the software is the addition of the APL.Grid object to the APL+Win class library. This ActiveX control provides for the display of APL derived data in a familiar spreadsheet format. An APL.Grid workbook can contain multiple pages with the customary tabbed interface, complete with row and column headings, fixed panes, cell decoration, and other features familiar to users of commercial spreadsheet programs.

While the grid object does not contain a calculation engine, direct transfer of array-oriented APL data into and out of the grid allows the interpreter to easily handle all computational needs. Customizable formatting for text, numbers, dates, times and currency data types is provided. Localization to native standards for dates, times, currency, and numerics is supported, as is specialized formatting based on value (*e.g.*, negative currency amounts).

Most all aspects of the grid are accessible under program control, but a standard user interface is provided as well. Cell content is easily edited by a user and verifiable by the program. Cell selection is accomplished with the usual actions, and multiple cell ranges are supported. The potential for a truly unique user interface is enhanced by several special cell types. Rather than data, a cell can contain a button, a combo box, or a check box. Tooltips can be provided to explain the content or function of specific cells, and overlay images can be set into cells to provide visual cues.

APL.Grid, as an ActiveX control in APL+Win, also benefits from those enhancements made to almost all controls in the class library. Chief among these are new properties which make the provision of truly context- sensitive help easier. Version 4.0 also enables use of the question mark button on form borders to allow for the implementation of "What’s This?" help. All visible controls now have a noredraw property which can help control screen flicker when updating complex forms, and the new imagelist operation of the Draw method allows easy visual customization of all controls.

Other areas of the system have been enhanced in this latest release. Interaction with printers has been augmented by better connection with the standard Windows print dialog boxes. Printer capabilities such as paper bins and collation are now easily available to your functions. You can easily find or set the default printer, and can now detect and use printer resources connected to your application’s station by a network. Specific controls have had their capabilities enhanced as well. The Combo box has been extended to support images and indents; the RichEdit control supports bullets, indents, and paragraph numbering; and the Selector (tab control) offers new options for tab appearance and behaviour. More controls now benefit from enhanced tooltip support, and overlay images have been added to more controls’ display.

Enhancements for those in special circumstances round out the list of features in this release. Large applications may benefit from a greatly increased maximum file size for both component and native files, and a doubling of the file tie quota. Applications using ActiveX objects can exploit an expanded calling syntax which can chain method calls, name parameters explicitly, and reference defined constants. Your applications have better control of their INI files, both in accessing them and creating them, and can initialize themselves without displaying the APL+Win banner.

And last but not least, APL+Win now comes with a full complement of printed documentation. The three-volume perfect bound set contains a newly revised User Manual, Systems Functions Manual, and a Windows Reference manual. The text of the printed documentation is still distributed on the APL+Win CD, along with a comprehensive help system, practical utilities, and sample workspaces.

For the latest on this release, or to learn about our other products, please visit us at [http://www.apl2000.com](http://www.apl2000.com)

<b>New APL2000 Distributor for the UK</b>  
APL Systems IDC SL of Madrid, Spain have been asked to provide sales and support services for APL2000, Inc., a COGNOS, Inc. company, in the United Kingdom. The Bloomsbury Software Company Limited, the former APL2000 distributor, have been merged with WebGain Ltd. Fred Honea, the contact at APL Systems, says he is looking forward to working with the APL users in the UK. Fred adds that APL will provide maintenance and support in the UK in addition to providing made to order courses of instruction. If anyone needs any information or has a question, please call or email Fred at +34 60 680 5949 or [fhonea@apl2000.net](mailto:fhonea@apl2000.net).

The Bloomsbury Software Company Ltd. have long been a strong supporter of APL in the United Kingdom. In response to changing market conditions, Bloomsbury have chosen to concentrate on the webcentric aspects of their business. This was accomplished by an alliance with WebGain Ltd. and a cessation of APL activities in the UK earlier this year. We are all fortunate that APL Systems IDC SL have been able to step into the breach and offer both sales and support to our APL users who have formerly depended on Bloomsbury.

<b>Trial Version of APL+Win</b>  
APL2000, Inc. are currently studying the possibility of releasing a trial version of APL+WIN. It would be based on Version 3.6 and would not include a Runtime version, adf files, nor hardcopy manuals. The price would be about one fourth that of the full version and, of course, the upgrade to the full version 4.0 would carry an attractive price. It could be downloaded or packaged and mailed as a CDROM.

## Causeway Graphical Systems Ltd

Perhaps the Internet really is changing the way some of us do business. The received wisdom has always been that you cannot “make a million dollars with ProductX” unless you first burn a huge amount of someone else’s money promoting it. Causeway is taking the “better mousetrap” approach which is a complete opposite - build your product, write about it on the internet, wait for the world to find you. It worked for Google, and it seems to be working for us. GraPL.NET really is just *RainPro* in a box, so it is first and foremost a programmer’s tool. Programmers just go trawling the internet (unlike managers who read the expensive promotions in glossy journals) and if they like what they see, they grab the demo, try it, then tell the manager to go and buy it.

This is good news for all you budding millionaires out there. APL is a way that a small company can build a better mousetrap and get rich selling it while staying small. APL lets you keep control of your codebase and your vision, and allows you to move quickly to respond to customer needs. What’s more you can test your product in a very demanding (but very tolerant) market segment; the APLers of the world will not only road-test it for you - they will most likely fix it when it breaks and mail you the fix!

This way, we get all the benefits of Open Source development (in a strictly limited domain) and at the end a product that knocks the socks off all those ‘real’ programmers out there who have never heard of APL. Thanks guys!

***
