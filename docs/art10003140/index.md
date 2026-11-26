---
vid: '10003140'
title: 'NED: a Nesting Editor for APL*PLUS/PC'
authors:
- Olle Evero
volume: '7'
issue: '4'
page: '116'
status: not online
issue_tab: n4-april-1991
---

Olle Evero
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 116](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=118)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>A Nesting Editor for STSC s APL*PLUS/PC by Olle Evero (Evestic AB)</p>
<p>Though nested variables were first introduced in 2nd generation APL, nested functions had beenthere all the time. The conceptof nesting is actually central to control flow in APL. Typical APL code contains functions calling functions, calling yet other functions - all the activity that is monitored by the State Indicator.</p>
<p>This boxes-within-boxes approachis of course absolutely trivial, almost a natural law of computing. Maybe thetriviality accounts for its lack of support in the average APL developmentplatform.</p>
<p>However, do notfear, rescue is at hand... I give you (drumroll, please): VED, the Nesting Editor! NED is a simple cover to the STSC OFDIT command,that will help you explore and maintain an application in a natural, nested way.</p>
<p>Picture yourself using OZDIT? to browse through the main function of a system, called MAINMENU. Suddenly(in the middleof line 13, say), you stop dead in yourtracks. Before you is a reference to another function FOO44. Of course, you need to know whatit contains.</p>
<p>Normally, you would haveto exit the editor and start another editing session on FOO4u4 to find out. But since you are using NED, you just place the cursor over Foou4 and pressctrl-E (or ctrl-Q). This takes you immediately into DEDIT with Foouu, After finishing browsing FOO44, simply place the cursor on an empty space in the function andpressctrl-E once more. This returns you to MAINMENU, at the exact spot where youleft (line 13, remember?).</p>
<p>Well, there you have the whole idea: navigating the code structure of an application in a fashion similarto the application itself. Here follows the APL:</p>
<p>116</p>
<p>VECTOR Vol.7 No.4</p>
<p>VA NED 8;C;D;8;F;G;H;X;N;DELX ;DWINDOW</p>
<p>C1] [2] a WNesting Editor for APL objects. Example cal NED 'MAINMENU' Ca] a A = ambivalent argument, unassigned at invocation Ca] a B = valid APL object name cs a O70 assumed to be i Olle Evero, Evestic AB [6] ee [7] GELX«'OS0UND 140 2000+LX' a beep and exit at error ce OWINDOW+ 0 0 24 80 A set window C9] &gt;(2=DNVC 'A'}/L0 a skip init status string if A exists C10] ACB oO sbi a init status string, skip append £11] LO:AeB,' ',A A append status string if A assigned C12) Li: 24 0 1 a0 UOWPUT a0tdA A put status string onto status line £13) +(O40NC B)/L1i5 o QINEUF 262 70 ,~1+0AViB a new object changed to fn (414] LAS:DEDIT Bo +(0 2 3 =DNC B)/EX,L2,L3 a edit and branch acc to type C15] L2:C+eB 0 C#(~24+ 11 ,pC)pC o +L4u a if variable,make matrix £16] bar:cecR Ba if function, canonize (17) Lu:D+1+(GPEEK 191 192), 25610PEEX 194 193 a last cursor pos in DEDIT C18] DBC11+D(1)-6xQPEEK 179 A subtract 6 if line numbers [13] Ne" it+4+/D[2 3] a calculate row in object [20] +(N&gt;1tp@)/L¥ a if out of bounds, exit [21] E+! A_ABCDEFGHI JKLMNOPQRSTUVHXYZ0%23456789' a char candidates [22] EE, 'abcdefghijkimnopgrstuvwxyza' a for APL name (23) Fec{W;] ¢ G+FeE a produce row, and bit vector of char candidates [24} K+(9=0NG B)a~(F(iJ]=!'a')vi:te1t(~G)/F a nonlabel,noncomment rows in fn [25] G+(Xp0),G a allow for indentation of nonlabel,noncomment fun rows [26] H+Kt (Oa\GD(LI 4G), Lea\("14+DE1] 4G a find name string (27) AcH/F 0 +(QcpH)/EX a if no name, exit [28] A NED Ha else recursive edit session [29] OINBUF{~1+D[3 2 1])/ 421 420 389 @ buffer cursor moves [30] ahi a return to 'old' edit session C31] EX: a exit</p>
<p>Some Usage Notes: + NED is recursive; you can follow a whole chain of functioncalls from top to bottom, then return to the top again.</p>
<p>+ In the absence of programmable function keys in DEDI, NED operation is controlled by wherethe user puts the cursor.If there is a character underthe cursorat exit, this character and the adjacent characters will be interpreted as a valid APL name, and a new (EDIT session on this namewill be invoked.If there is no character underthe cursorat exit, you will returnto the calling environment. This protocol may take some getting used to.</p>
<p>+ You can use NEDto build a system from scratch.Afterfilling your main function with references to subfunctions, just place the cursor on any one of them,press ctrl-E andstart codingit. No need to memorize a plethora of names- andit saves typing too!</p>
<p>117</p>
<p>VECTOR Vol.7 No.4</p>
<p>+ Screenline 25 is used to line up the namesthat are passed to NED. Think of this as OST raveled,if you will.</p>
<p>+ The inputbuffer (used by JINBUF) does not normally hold more than 256 characters. This could be a problem if you tend to write functions exceeding 200 lines or so. On the other hand,if you write such huge functions, this may well be the least of your problems...</p>
<p>- NED operates on functions and text matrices alike.</p>
<p>+ Peeks and pokes are used as indicated in documentation pertaining to the somewhatobsolete version 7.1 of APL*PLUS/PC. You might want to check their applicability to the version you have got.</p>
<p>At Evestic, we have compiled a library of utilities related to the STSC APL*PLUS/PC product. The focus is on novel ideas and items otherwise hard to come by. There is guaranteed to be no assembler or locked code, just short stand- alone APL functions. Please drop a line to the following address, if you want more information on this.</p>
<p>Evestic AB, Attn: Olle Evero Frejgatan 6/B-133 114 79 Stockholm SWEDEN</p>
<p>Ed: This is a super idea, and in most cases it works very well, however there is one interesting snag:ifany lines in the edit stack are wrapped by QEDIT,itfails to return to the right place! This is a particular problem if you write functions with lots of local variables, and hence long headerlines.</p>
<p>Resolution of thislittle dilemmais left as an exercise for Vector readers ... answers on the back of a postcard please in time for 8.1, and we will print the best in the conference special. Thanks.</p>
<p>118</p>
</details>
