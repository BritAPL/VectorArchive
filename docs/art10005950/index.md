---
vid: '10005950'
title: GSS Graphics with APL*PLUS/PC
authors:
- George MacLeod
volume: '7'
issue: '4'
page: '76'
status: not online
issue_tab: n4-april-1991
---

George MacLeod
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 76](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=78)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>The Trials and Tribulations of using GSS Graphics with APL*PLUS/PC talk by George MacLeod November 1990, notes by Jonathan Barman</p>
<p>George MacLeod gave us a heartfelt commentary on the problemsin getting decent graphs outof GSS graphics with APL*PLUS/PC.</p>
<p>First George explained why high quality charts are needed. APL Impetus Ltd develop and marketa financial planning package Impetus which is a descendant of the Boeing s TABAPL. Impetus is often used high up in companies andas a result the quality of the output is important, and high quality output must include high quality graphics.</p>
<p>The initial prototypes for Impetus used the 0G graphics available with the APL*PLUS/PCinterpreter. The functions mirror those available in ROM BASIC, and the facilities provided are quite simple, for example only one font is provided which is increased in size by pixel replication. Sizes increase by squaring the numberofpixels, so big characters look very crude. 0G graphics can be used witha relatively limited range of graphics boards and printers, but there are now a hugearray of graphics devices. It has become impossible for STSC to supportall the devices on the market.</p>
<p>GSS*CGI was introduced by STSC to solve these problems. GSS is a American company, Graphics Software Systems Inc, who concentrate on providing graphics drivers for every device on the market. CGI stands for Common Graphics Interface which is set of standardised graphics calls for programming languages such as C and FORTRAN.STSC have provided a set of functions in a workspace which reproduce each of thecalls provided by GSS*CGI. Usingthese functions forces the use of extensive looping, as the GSS functions are really designed for compiled languages which can only manipulatescalars.</p>
<p>Thefirst essential before using GSS*CGI is to buy the GSS Programmers Guide costing £50. The APL*PLUS/PC manual gives a simple list of functions and their titles, but not the details of the arguments and values required. For example, the function V_OPEN_WKST takes an eleven element numeric right argument which specifies default values for the device namedin the left argument. The exact numbers to be used are only given in the GSS Guide.</p>
<p>George showed a sample chart using GSS*CGI as supplied. The fonts are only marginally better than the [1G fonts, and the quality is quite low. To get better</p>
<p>76</p>
<p>VECTOR Vol.7 No.4</p>
<p>fonts you need the GSS Completer Kit at £70, which contains lots of device drivers, FONTDRIV.SYS, but nofonts! Bitstream supply fonts at £140. What you actually get is sets of bezier curves defining the characters, together with a font maker. To actually use the Bitstream fonts you have to buy the GSS Font Maker whichis part of the GSS Developers Toolkit costing £400. An apparently simple addition turns out to be quite expensive!</p>
<p>Nowyouhave everything the problemsreally start. Whilst screen display works well the major problem is printing in high resolution. George showed sample charts which took many hours each to produce, and someof them wereplainly wrong with headings missing! The problemsare almostcertainly to do with the way GSS manages memory, and GSS ownupto having a problem with bitmaps. Whatever memory is given to GSS it always seems to need more. The memory has to be below the 640K boundary, and any memory allocated to GSS cannot be used by APL.</p>
<p>At this point George &amp; Co. gave up andtried to use the Metafile Interpreter supplied by GSSat £225. The Metafile Interpreter consists offive diskettes full of subroutines, and you have to write a program in C or FORTRAN to produce what you want,So they had to buy a MicroSoft C compilerat £250.</p>
<p>Basically this is as far as George has got with high resolution graphics using APL*PLUS/PC and GSS*CGI. Other alternatives are being looked at such as APL*PLUS II and DYALOG APL. Both these versions of APL can run above 1 Megandcan let GSS run below 640K. APL*PLUSII and APL*PLUS/PC have essentially the sameinterface with about 120 APL functions matching each of the GSS commands. Dyadic have done rather better in that you can pass many GSS commands to the Auxiliary Processor in one nested array. Also, Dyadic have made somefunctions ambivalent so that settings can be passed as an optional left argument.</p>
<p>George summarised thesituation as follows:</p>
<p>The GSS Graphics system is unsatisfactory when used with APL*PLUSPC.</p>
<p>Very limited experience suggests GSS will worksatisfactorily with APL*PLUS ILand DYALOG APL.</p>
<p>Other PC products that offer graphics use GSS and they seem to work well. Perhaps most of the problemsare dueto poor cohabitation of APL*PLUS/PC and GSS.</p>
<p>GSS is probably the best PC Graphics System available.</p>
</details>
