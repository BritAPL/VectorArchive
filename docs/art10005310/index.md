---
vid: '10005310'
title: 'Review: “Introduction to APL*PLUS/PC” by Maurice Dalois'
authors:
- Alan Mayer
volume: '10'
issue: '2'
page: '19'
status: not online
issue_tab: n2-october-1993
---

Alan Mayer
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 19](../10/2/VOL.10-NO.2-OCTOBER-1993.pdf#page=21)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>The Education Vector Vol.10 No.2</p>
<p>Introduction to APL*PLUS/PC by Maurice Dalois reviewed by Alan Mayer</p>
<p>This book is an excellent introduction for APL beginners. It comes with a floppy disk containing an educational version of APL*PLUS/PC.This is similar to the Manugistics commercial product, with some limitations and omissions, and is quite adequate for the stated purposeof learning APL. At $30 (plus shipping costs) for book and diskit is also ideal for many other educational purposes, such as teaching mathematics andstatistics. I will start with a brief review of the software.</p>
<p>Installation of the system is very simple. The only startup decision the user has to makeis the choice between three separate batch files for CGA, EGA and VGA. VGA and EGA work well. CGA is painfully slow, as the APL characters are produced in graphics mode. Only Epson compatible and HP Laserjetprinters are supported by the Educational Version of APL*PLUS, and this choice can be made at startup by specifying a parameter for the batch file. Object sizes are limited to 64K and dimensions of arrays to 32767. A character vector cannot exceed 32767 characters; a numeric integer vector cannot exceed 32763 numbers; a numeric floating point vector cannot exceed 8190 numbers.</p>
<p>The system function OMLOAD (usedto link up with non-APL programs) is not available. Terminal mode is not available (although a do-it-yourself transmit/receive program is described briefly in the text). Virtual workspace, extended memory usage by theinterpreter, andfile-sharing on networks are not available. Screen managementis all there, as is file-handling. 0G graphics are supported, but not the GSS interfaces. The user command processor works, but the documentation in the bookis limited, so access to the full documentationis desirable. Comingto the book itself, there are 190 pages of text, and a further 112 pages of appendices and an index. The text is divided into 9 chapters, which are easy to read, and take you rapidly but comprehensively through the main features of APL programming, from calculator mode to screen management, graphics and datafiles.</p>
<p>There are some typographical errors words like beyong instead of beyond. Nonethat I have found really matters: I mention them by wayof a minorprotest.</p>
<p>19</p>
<p>The Education Vector Vol.10 No.2</p>
<p>Weall have spell-checkers these days. J know they can be annoying, but used sensibly they should help eradicate these irritations. (Yes, I know this is dangerous stuff send meall the typos you find in Education Vector and 1 will humbly apologise for every one!) From chapter 3 onwards, various utility functions are introduced in the text. They are listed as they are introduced. Theyalso appear in Appendix C, and are available on the disk. You would be forgiven for assuming that functions with the same name, taken from any of these sources, would be identical but you would be wrong. In at least one case (a function called ASKN)all threelistings are different! Most ofthe functions perform correctly, so the inconsistencies are again little more than anirritation.</p>
<p>In the case of a function called DISPLAY, however, there are some problems. The disk version corresponds to the appendix, butnotto the text. The problem is that only the text version gives the results claimed in the text. Working through Chapter 4, on Screen Management, we are advised to copy DISPLAY from the workspace UTIL and perform a few simple tasks. The first task involves displaying a blinking statement on line 24 of the screen. Unfortunately line 24 is a Statusline, so the statementcannot beseen unless you scroll downtoit. Then you display other statements at various positions on the screen, and that is when the disk (and appendix) versions let you down.</p>
<p>It is sad that these flaws mar whatis otherwise an excellent text. Once you realise that when things don t work it may not be yourfault, you can benefit greatly from an exploration of Screen Managementin Chapter 4, Graphics in Chapter 5 and Data Files in Chapter 6. Chapter 7, entitled Miscellaneous Techniques, introduces Recursion and the User Command Processor. A very brief part of Chapter 7 also discusses communications with printers, other computers, plotters and digitizers sufficient information to whet the appetite and perhaps to set the determined explorer off on the right track. The last two chapters present miscellaneous problems (to which solutions are provided in an appendix, together with solutions to problems posed throughoutthetext) and a case study on processing election results.</p>
<p>Appendix A is particularly valuable as a concise reference manual. Entitled Definitions it includes a completelist of all primitive functions and operators, together with examplesof their various uses. This is followed by a complete list ofall system functions, variables and commands.</p>
<p>Book and disk are available from EducAPLInc, 1120 av du Parc, QUEBEC, Que, CANADA, G1S 2W7. They are undoubtedly an important addition to the APL educationallibrary.</p>
<p>20</p>
</details>
