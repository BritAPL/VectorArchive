---
vid: '10014150'
title: News from Sustaining Members
authors:
- MicroAPL Ltd
- Causeway Graphical Systems Ltd
volume: '19'
issue: '2'
page: '9'
source: trad/v192/sus192.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2002
tags:
- implementations
- APL community
---

## MicroAPL Ltd

The new version 1.1 of *APLX*, which was previewed at APL2002 in Madrid, is now shipping on all platforms (Windows and MacOS, plus server editions for Linux, AIX, and Windows servers). The full list of new features is too long to detail here, but highlights include:

- Support for the ‘structured control’ programming keywords (`:If` etc).
- Support for using the double-quote as an alternative to the traditional single-quote character for delimiting APL character vectors.
- `⎕DISPLAY` and `)DISPLAY`, as well as a new Display window which shows you the structure of an APL array with just a click of the mouse.
- Powerful graphics drawing, bitmap and picture display, and animation facilities, using the `Draw` method.
- Support for Windows OCX/ActiveX controls, embedded OLE documents, and OLE servers. For example, you can control and exchange data with Excel, or include a Word document inside a `⎕WI` window and program it from APL. There is also a Control browser which allows you to browse through the external classes installed on your machine.
- The `maxsize` and `minsize` properties allow you to put constraints on `⎕WI` window sizes.
- The RichEdit control is now available under MacOS as well as Windows.
- A new keyboard-customisation dialog allows you to edit your own keyboard layout by dragging symbols on to the keyboard picture.
- Speed-ups to various primitives, especially matrix divide under Windows.
- Improved documentation, with fully searchable HTML help now available under MacOS as well as Windows. Context-sensitive help (using F1 or the Mac Help key) is also improved, recognising `⎕WI`keywords and not requiring the word at the cursor to be selected.

In addition, we have included the promised ‘packager’ feature which bundles your workspace with a free runtime edition of the interpreter. To make an executable application from your APL workspace, just choose Save As… from the File menu, and select ‘Executable (packaged) ws’ as the file type. (Under Windows, you also need a DLL, downloadable free of charge from our website).

APLX Version 1.1 runs on all Windows operating systems from Windows 95 through to XP, and on all MacOS versions from 8.6 onwards, including the new MacOS 10.2 ‘Jaguar’.

To find out more, you can download documentation on the new features, or the complete new manuals in PDF form, from [http://www.microapl.co.uk/apl](http://www.microapl.co.uk/apl). Time-limited evaluation versions of the full product are also available.

Many of the version 1.1 improvements have been in response to the excellent feedback we have received from APLers around the world. Please keep the suggestions coming – the next major release will be version 2.0, which will include some unique new development-environment features as well as important language enhancements and new `⎕WI`classes.

## Causeway Graphical Systems Ltd

At Madrid, APL2C and Causeway agreed that it would be good for both of us to work more closely together. The APL2C engine is ideal for delivering Causeway’s *GraPL.net* server product as a single lightweight DLL on the Windows platform, and also allows us to ship a command-line EXE file on any of the common Unix systems. The RainPro source can be automatically converted to run in APL2C, and then compiled and linked to make a COM server, which is much lighter to load than a full APL system with interpreter and workspace.

We also see the potential to offer APL developers a service which will take existing VS APL or APL2 code and wrap this as a single-file DLL with the appropriate COM wrapper automatically constructed. Before we can do this, we would like to prove the technology as thoroughly as we can, and the best way of doing this is to ship a good subset of RainPro and NewLeaf in this format. We will also be quietly working through the test examples in the ISO APL Standard (and Extended APL Draft Standard) which will ensure that the service we offer can reliably compile any standards-conforming (Gui-free) APL code. Please do not expect any quick results from this project. We already have a single-file DLL version of RainPro which runs all our test charts correctly (and fast) but there is a long way to go before APL2C will be available as a fully-supported Causeway product for general-purpose use in compiling arbitrary APL2 code. We need to design the structures which will automatically generate the COM interfaces from your public functions, and test and retest the engine before we feel we can offer it for use to the APL community.

***
