---
vid: '10014100'
title: News from Sustaining Members
authors:
- MicroAPL Ltd
- APL2000 Inc.
- Causeway Graphical Systems Ltd
volume: '18'
issue: '2'
page: '18'
source: trad/v182/sus182.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2001
tags:
- implementations
- APL community
---

## MicroAPL Limited

After a long absence from these pages, MicroAPL is back, in a big way – with a major new range of APL products for the Macintosh, Linux, and Windows!

Our long-established APL for the Apple Macintosh was the world’s first APL interpreter for a modern-style windowing environment. With Apple’s introduction of Mac OS X, substantial changes had to be made if we were to continue marketing this product. After a careful review of the technical and commercial options, we decided that this was the time for a major re-write. We are pleased to announce that our new APL development environment, called *APLX*, is now available – and that it runs under Windows as well as MacOS.

### *APLX*: Cross-platform development

*APLX* comes in two forms, as a GUI application offering an advanced and elegant environment for developing APL, and as a server-based product for running large commercial APL applications and legacy systems. The GUI product is initially available for Windows 95, 98, ME, NT, 2000 and XP, as well as for Mac OS 9 and Mac OS X. In each case, *APLX* is a full native application, with the ‘look-and-feel’ appropriate to the host. For example, under Mac OS X, it implements Apple’s new ‘Aqua’ user-interface, whereas the Windows versions follow Microsoft’s style. All this is achieved using a common code base and advanced cross-platform technology, developed by MicroAPL, which will make it easy for us to continue to upgrade the product for multiple environments.

The server editions of *APLX* are available for Windows NT and 2000, for x86 Linux, and for the IBM RS/6000 under AIX.

All versions of *APLX* share the same workspace and component file layouts. This means that workspaces saved by any version can be loaded by any other, and component files can be shared across a network of different hosts. Where the two systems use different byte-ordering conventions, *APLX* transparently converts between them. Existing APL.68000 and APL Level II workspaces can also be loaded directly (*APLX* automatically converts them to the new format), and existing component files are also upwards-compatible.

For users migrating from other APL implementations, IBM-compatible APL transfer files can be read directly using the `)IN` system command, or via the File menu.

### *APLX*: Elegant development environment

MicroAPL’s long experience in developing APL windowing systems, plus our extensive exposure to ‘best-of-breed’ development environments for other languages, has been carefully applied to the design of *APLX* as a platform for writing APL code. The aim has been to provide very powerful development facilities, without any distracting on-screen clutter. Key features include:

- The Session window history can be as long as you like, subject to available system memory.
- Syntax colouring is available for Edit windows, with facilities for identifying and localising variables changed by a function.
- A pop-up menu is available at any time to edit an object, see its value or properties (including the syntax and first few lines of comment if it is a function/operator), or access the comprehensive on-line help system for the object or APL symbol at the cursor.
- The convenient Workspace Explorer window lets you browse quickly through the workspace contents.
- Watch windows allow you to monitor the contents of a variable or the result of any APL expression.
- The Debug window provides a user-friendly front-end for you to intervene when errors occur or a breakpoint is hit, displaying the stopped function and allowing you to browse the `)SI` stack. As well as using the pop-up menu to inspect variable contents, you can edit the stopped function directly in the debug window, set and clear breakpoints, and use the Step Over, Step In, Step Out and Go buttons to control execution. And if you want to resume execution from a different line, just drag the execution pointer there.

### *APLX*: Cross-platform APL GUI applications

Under *APLX*, the APL developer has access to a rich set of user-interface objects using the `⎕WI` system function. This includes all of the standard objects such as windows, buttons, list boxes and edit fields, plus more advanced ones such as rich-edit controls, splitters, bevelled outlines, and movie players (also known as ‘media’ controls). In order to assist in application migration, this implementation is designed for upwards compatibility with MicroAPL’s previous Macintosh APL, and is similar to the implementation in APL+Win. Enhancements include better geometry management (no need for tedious ‘onResize’ event handling!), and improved access to commonly-used facilities such as file-open and colour-selection dialogs.

Except for a few controls where the underlying operating system does not provide an equivalent capability, this functionality operates cross-platform. Your APL application written using *APLX for Windows* will run with little or no change under Mac OS X – and with the ‘Aqua’ look and feel.

### *APLX*: Language features

Like its predecessors, *APLX* is a high-performance interpreter closely modelled on IBM’s APL2 language standard, combined with the tried-and-trusted APL.68000 enhancements. Workspaces of up to 2GB are supported, subject to available system memory. APLX also implements the very powerful `⎕NA` system function, allowing you to call operating-system or DLL routines directly from APL, as though they were ordinary APL functions.

### *APLX*: Development roadmap

This first release of the new APLX interpreter is the start of a planned series of upgrades and new features. As well as increased functionality in `⎕WI` and even better development environment facilities, we plan to introduce a number of language enhancements to APLX in future releases. We believe that high priorities include the ‘structured control’ directives and hierarchical naming, but we want to hear from you, APL developers, as to what you want.

Also on the roadmap is support for GUI programming under Linux. This will extend *APLX*’s cross-platform capability both for APL development and for applications developed in APL.

### *APLX*: Competitive pricing

The APLX development environment for Windows and MacOS is competitively priced at £499 (+VAT) in the UK. Special terms are available for educational establishments and for users upgrading from previous MicroAPL products. For more information, visit our web site [http://www.microapl.co.uk/apl](http://www.microapl.co.uk/apl).

## APL 2000 Inc APL+Win Version 4.0

APL2000 Inc. is pleased to announce the availability of APL+Win version 4.0 for Windows 95/98/NT/ME and 2000.

The most remarkable enhancement to this version of the software is the addition of the APL.Grid object to the APL+Win class library. This ActiveX control provides for the display of APL derived data in a familiar spreadsheet format. An APL.Grid workbook can contain multiple pages with the customary tabbed interface, complete with row and column headings, fixed panes, cell decoration, and other features familiar to users of commercial spreadsheet programs.

While the grid object does not contain a calculation engine, direct transfer of array-oriented APL data into and out of the grid allows the interpreter to easily handle all computational needs. Customizable formatting for text, numbers, dates, times and currency data types is provided. Localization to native standards for dates, times, currency, and numerics is supported, as is specialized formatting based on value (*e.g.*, negative currency amounts).

Most all aspects of the grid are accessible under program control, but a standard user interface is provided as well. Cell content is easily edited by a user and verifiable by the program. Cell selection is accomplished with the usual actions, and multiple cell ranges are supported. The potential for a truly unique user interface is enhanced by several special cell types. Rather than data, a cell can contain a button, a combo box, or a check box. Tooltips can be provided to explain the content or function of specific cells, and overlay images can be set into cells to provide visual cues.

APL.Grid, as an ActiveX control in APL+Win, also benefits from those enhancements made to almost all controls in the class library. Chief among these are new properties which make the provision of truly context-sensitive help easier. Version 4.0 also enables use of the question mark button on form borders to allow for the implementation of “What’s This?” help. All visible controls now have a noredraw property which can help control screen flicker when updating complex forms, and the new imagelist operation of the Draw method allows easy visual customization of all controls.

Other areas of the system have been enhanced in this latest release. Interaction with printers has been augmented by better connection with the standard Windows print dialog boxes. Printer capabilities such as paper bins and collation are now easily available to your functions. You can easily find or set the default printer, and can now detect and use printer resources connected to your application’s station by a network. Specific controls have had their capabilities enhanced as well. The Combo box has been extended to support images and indents; the RichEdit control supports bullets, indents, and paragraph numbering; and the Selector (tab control) offers new options for tab appearance and behavior. More controls now benefit from enhanced tooltip support, and overlay images have been added to more controls’ display.

Enhancements for those in special circumstances round out the list of features in this release. Large applications may benefit from a greatly increased maximum file size for both component and native files, and a doubling of the file tie quota. Applications using ActiveX objects can exploit an expanded calling syntax which can chain method calls, name parameters explicitly, and reference defined constants. Your applications have better control of their INI files, both in accessing them and creating them, and can initialize themselves without displaying the APL+Win banner.

And last but not least, APL+Win now comes with a full complement of printed documentation. The three-volume perfect bound set contains a newly revised User Manual, Systems Functions Manual, and a Windows Reference manual. The text of the printed documentation is still distributed on the APL+Win CD, along with a comprehensive help system, practical utilities, and sample workspaces.

For the latest on this release, or to learn about our other products, please visit us at [www.apl2000.com](http://www.apl2000.com).

## Causeway Graphical Systems Ltd

Strange how you can stay in the same place and eventually the world comes around to you. I think the Guinness campaign slogan “All things come to he who waits” started with Badger in *Wind in the Willows* who just ‘remained’ while civilisations came and went around him.

Back in late 1989, I started recording rainfall data and wanted to plot it as a nice graph. The technology available was APL\*PLUS/PC and my HP DeskJet 500 and GoScript (a command-line PostScript engine). The result was a set of vector-graphics primitives which eventually formed the basis of *RainPro* and *NewLeaf*. There things have rested for over 10 years until the W3C published the specification for SVG, and Adobe released PDF into the public domain. Suddenly, there are two very PostScript-like engines out there, and anything drawn with these basic primitives will render easily on either of them.

Now, *RainPro* has always been about making simple, effective charts with minimum use of redundant ink. This keeps the bandwidth down (even as images, the charts compress very well) but somehow it lacks pizzazz. With SVG you can define the most spectacular gradient fills, 3D extrusions, and even lighting. Just imagine a piechart with gradient-filled 3D sectors, lit from a spotlight positioned a few cm in front of the monitor surface, angled to bring out the shadows.

No, on second thoughts, go and have a look at the examples on [www.grapl.com](http://www.grapl.com) in the “Fun with Filters” section. Then check the file sizes. Then read the ASP script that made them. I think you will see what *RainPro* has been waiting for.

***
