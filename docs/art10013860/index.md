---
vid: '10013860'
title: News from Sustaining Members
volume: '15'
issue: '1'
page: '18'
source: trad/v151/susnews.htm
converter: vecrescue 0.1.0
issue_tab: n1-august-1998
tags:
- implementations
- APL community
---

## Sustaining Members’ News

### Dyadic Systems Ltd.

Dyadic is pleased to announce Dyalog APL/W Version 8.2. Although this version is not scheduled for general release until 4th quarter 1998, it was exposed to hands-on use by APL98 workshop attendees and came through with flying colours. Version 8.2 contains two highly significant major enhancements, namely multi-threading and the ability to write ActiveX controls in APL, as well as many other new features and extensions.

<b>Multi-Threading</b>  
Multi-threading allows you to execute more than one APL function (or indeed any expression) at the same time. This allows you to perform background processing, such as printing, database retrieval, database update, calculations, and so forth while at the same time perform other interactive tasks.

Multi-threading may improve throughput and responsiveness and is an essential ingredient of an APL web server.

A thread is created by calling a function *asynchronously*, using the new primitive operator *spawn:* `&`.

Any thread can spawn any number of sub-threads, subject only to workspace availability. This implies a hierarchy in which a thread is said to be a child thread of its parent thread. Each thread has a unique thread number, with which, for example, its presence can be monitored or its execution terminated.

Threads are implemented by the following new language elements:

- A primitive operator called *spawn:* `&`.
- System functions: `⎕TID, ⎕TNUMS, ⎕TKILL, ⎕TSYNC`.
- An extension to the GUI Event syntax to allow asynchronous call-backs.
- A control structure: `:Hold`.
- System command:  `HOLDS`.

<b>APL ActiveX Controls</b>  
The new *ActiveXControl* object allows you to package a Dyalog APL application as an ActiveX Control. To run your application, all the user needs is your control and a copy of the Dyalog APL dynamic link library.

Packaged as an ActiveX control, your Dyalog APL GUI application may be included in a program written in Microsoft Visual Basic, Microsoft Excel, Borland Delphi, or in any other host application that supports ActiveX.

This feature also allows your Dyalog APL application to be run *inside a web page* hosted by Microsoft Internet Explorer, or by any other web browser that supports ActiveX. With Version 8.2, it is now possible to implement and distribute applets, written in Dyalog APL, that are accessible to any user with Internet Explorer on the World Wide Web. This capability offers enormously expanded potential for the exploitation of APL in application development.

<b>Other Enhancements</b>  
Other enhancements include the following:

- Additional APL2 language features
- Permanent session log
- `:With` control structure
- CoolBar object
- Splitter object
- Calendar object
- TabControl object
- Colour printing of APL functions
- Improved Bitmap support
- PNG and GIF support
- Web Server workspace

<b>More Information</b>  
Further details are available at [http://www.dyadic.com](http://www.dyadic.com)

### APL2000 Inc.

### Major Features of APL+Win version 3.0

**COM Support:** Allows an APL application to be called from other Windows development tools using Microsoft's component object model (COM). A developer can have an APL calculation engine driven by a front-end written in Visual Basic, or call APL from Excel or Java. Allows one APL session to manage multiple other APL sessions.

**Network Interface:** TCP/IP support allows Internet enabled APL applications. Also included are sample workspaces that demonstrate: FTP client and server; ITalk – Internet chat facility; HTTP server and utilities; Simple chat/draw across network; ECHO client and server; SMTP email client. ODBC Server: Allows workspace variables in an APL application from native or component files to appear as an ODBC-compliant database to any ODBC client. Included with APL+ODBC are the client driver and a sample server workspace.

**Media Control:** A media player control for AVI, ANI, WAV, and MIDI files. Revised On-line Help System: Easier access to any object's properties, methods, and events.

### Control Improvements

Improved support for ActiveX controls; embedded objects in RichEdit controls; new seltext property for Edit controls; tooltips on disabled objects; tooltips on labels; tooltipwidth – allows multi-line tooltips; onMouseEnter and onMouseLeave events; mouse events on labels; in-place editing of Treeview items; in-place editing of Listview items; order items and columns in Listview; place pictures in Listview.

### Other Improvements

Squad – INDEX function; Iota and epsilon faster with nested or heterogeneous arguments; open the function editor at a given line; Ctrl-T toggles between editor and session.

**New APL Utilities:** New workspaces with functions to: operate the Audio CD player and Multimedia Player (.avi, .mpg, .mid, etc.); print a tree or listview (in colour); print APL functions in colour; decode Windows error messages; find all Windows API calls in a workspace with their aliases and parameters; find DLL dependencies in a module; clean up INI and ADF files.

**Miscellaneous:** MS Word 6 .DOT file templates for easier insertion of APL characters into Word documents.

### Causeway Graphical Systems Ltd.

The first thing we must do is send our best wishes for the future to Duncan and Jon, who have moved on to join Adaytum Software’s growing team at York. From being Causeway developers, they are now full-time Causeway users – anyone who heard Morten Kromberg’s plenary session at Rome will know that the core of Adaytum Planning is rapidly being rewritten around the Causeway technology. The Causeway product set is now fully documented, stable and readily available from our web site. The *CausewayPro* manual and tutorial have been fully revised and distributed to existing customers. *RainPro* has been updated to include:

- horizontal bar charts
- bitmaps and metafiles placed anywhere on a chart
- graphical editing and drill-down capabilities

*NewLeaf* (for Dyalog APL) has also gained many new features, for example:

- automatic horizontal folding of wide tables, with key columns retained on each page
- ‘groups’ which define collections of text and charts and prevent unhelpful page-breaks
- html and csv output in parallel to the report production
- ‘overlays’ to allow arbitrarily complex master pages

Serial numbers after 97042 entitle you to a free upgrade, so please download the software (including updated help) from the [web site](http://www.causeway.co.uk) and contact us or your local dealer for the password and an updated user manual. Users with serial numbers prior to this may upgrade for a fee of £50 per product.

In common with most software companies, we are now up to our ears in work! Marc is in Basel fully occupied with TCP/IP sockets and SQL, Adrian and JoHo are busy with project work for Adaytum and SAP, and we have a major mainframe-to-Dyalog conversion job to start on from October. Please use email where you can for support calls, and try to help us out with clear and simple diagnostic information. If we are out of the office when you phone, leave us a short message and someone will get back to you as soon as they can.

***
