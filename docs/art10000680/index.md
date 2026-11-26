---
vid: '10000680'
title: Update on APLIWIN
authors:
- Jonathan Barman
byline: by Jonathan Barman
volume: '10'
issue: '1'
page: '55'
source: trad/v101/barman101_55.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1993
tags:
- reviews
- implementations
- user interfaces
---

*by Jonathan Barman*
{ .byline }

I received review copies of ISI’s latest offering of APLIWIN and JWIN just before the copy deadline for Vector, so this is a very brief update on the new facilities.

### Session Manager

The Session Manager (version 2.0) has been greatly improved. There is a standard Windows menu with the normal entries of File Edit and Help together with a Font option. The menu cannot be accessed by the normal accelerator keys as ISI have commandeered all the Alt key combinations for the entry of APL characters. Alt+F, for example, gives a `⋄`, so this cannot be used to access the File menu entry. However, the Alt key on its own highlights the first menu item as is usual in Windows.

The File entry just contains an Exit option, so you still have to use the normal `)lib` and `)load` for workspaces. The Edit option contains the standard cut and paste entries, and shows that the standard accelerator keys of Ctrl+C, etc, can be used, including Ctrl+Z for undo.

The Font option allows you to change the size of the APL font, or to switch to any other font of your choice. The APL font can be changed to italic, and/or bold. Switching to a non-APL font might be useful as a temporary measure when running old DOS-based applications. If APL characters are on the screen the results are quite horrid!

Help now includes a really compact keyboard layout which you can pop up with Ctrl+F1. The window is quite small and is very clear. It stays on top of the APL session window, but disappears behind other windows when you switch applications.

The boxed entry area at the bottom of the screen has been removed, but the input area is still at the bottom of the session. The input line is the only line on which you can execute an APL expression. If you place your cursor on any other line on the screen and press the Enter key the text is automatically transferred to the input area and you then have to press the Enter key again to execute it. The text on the screen can be altered in any way you like. Cut and paste are character oriented, so that you can clean up the output if you need to.

There is now a scroll bar along the bottom of the window so that if `⎕PW` is set to a large value you can easily scroll to the right hand portion of the output.

The JWIN session manager is identical except that the APL keyboard is missing. You can switch to any font, but anything other than the standard font wreaks havoc on the J display of verbs and boxed nouns.

I was pleased to see these changes, particularly as they are CUA compliant. The overall effect makes for an easy environment in which to work. I particularly liked the pop-up keyboard layout.

### Windows Interface

The major additions to the Windows interface are the DDE and OLE interfaces. The list of Windows features is considerably longer, but I have not had time to investigate what they all do. One of the problems with the APLIWIN and JWIN Windows interface is that the only documentation is in the Help file which is decidedly terse. At least ISI have now provided the whole of the Help as a file in Write format so one does not have to go through the help screens printing them out.

The OLE demonstration workspace takes one through some simple but stunning examples of OLE. Linking with Microsoft Graph, which is included with Word for Windows, allows one to produce graphs from APL and J with the minimum of effort. There is a lot of power here which is waiting to be exploited.

I do not have Excel, so could not run the DDE demonstration directly. I spent a few minutes trying to do the equivalent job with Quattro Pro for Windows, but found that I needed to study the manuals and get better acquainted with DDE before I could proceed. The ISI documentation recommends that you start by linking data between two APL or J applications to get the feel of the thing. As Adrian showed in his article in on Shared Variables and DDE, in Vector 9.2 page 128, the DDE interface is a powerful way of exchanging data between applications. I look forward to playing with the ISI version of DDE.

There is a new picture class of object which allows one to display bitmap files (.BMP), Windows metafiles (.WMF) or icon files (.ICO). I was able to display .BMP files easily, but Ray Cannon’s sample bridge picture came out in black and white in a little box. More investigation is needed to see if I can get it to display in the full 256 colours at its proper size.

A whole bunch of commands has been added. I noticed that one can now invoke the standard Windows font selection dialog box. A rectangle with rounded corners can be drawn in a graphics window. Colours can be set in a dialog box. There is more control over events that will end wait commands.

All these new features are beginning to look as though one will be able to create some reasonable Windows applications with APLIWIN and JWIN.

### Conclusion

For a total of $70 (US or Canadian) you can get APLIWIN and the APL reference manual, which is an unbeatable price. This is definitely the best buy APL which is ideal for production systems as well as for home use. On a 486 the APL performs at a perfectly adequate speed, and you have the tools to build quite respectable Windows applications.

(webpage generated: 5 December 2005, 18:50)
