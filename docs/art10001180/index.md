---
vid: '10001180'
title: Full-screen Methods with APL2
authors:
- Peter Branson
volume: '7'
issue: '4'
page: '126'
status: not online
issue_tab: n4-april-1991
---

Peter Branson
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 126](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=128)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>Full Screen Methods with APL2 by Peter Branson</p>
<p>Introduction This is mostly concerned with APL2 on the mainframe and,although there is a widespread tendency to move APL applications down to PC s or the RS 6000, ete., there is still plenty of mainframe code out there. In most cases, APL mainframe systemsare likely to have been around a while, and the full screen techniques well established, possibly with a set house-style. Nevertheless, mainframe APL developmentdoesstill go on, and a review of someofthe full screen methodsavailable maybeofinterest to relative newcomers.</p>
<p>Myprimary interest is alphamerics for menus, reports, tables, etc. but graphics will get some mention. I will cover four approaches: (1) Character Matrix, (2) GDDM (AP126),(3) ISPF and (4) AP124 (yes, AP124!). As always, everythingis in origin-1. ] use TSO, but most of whatI haveto say also applies to VM.</p>
<p>Character Matrix For simple menu systems,this is the oldest, the easiest and the cheapest method. Obviously, all you need to do is clear the screen (AP100 and TSO CLEAR), construct and display the matrix, havea little prompt/ response function and use OPc(2]. By the way, APL2 provides a rather neat wayfor that promptfunction:</p>
<p>vVIPX(Oiv Col Z+IPX MSG;0PR C1] a APL2 prompt/response fn C2] OPR+' [3] f+mse (4) Zege")</p>
<p>Z+QWGET 1</p>
<p>(This won't work on APL2/PC since DPR is not yet implemented)</p>
<p>I can anticipate screams of horror at the thought that a professional APL-er mightstill do full screens this way, butit does actually work! Of course, a prime objection is thatit is too easy to interrupt back to raw APL, so there are many systems where you should NEVERuseit. In some of our systems, though, for</p>
<p>126</p>
<p>VECTOR Vol.7 No.4</p>
<p>historical reasons we still have menus created this way, and with several hundred users over many years J am not aware of many complaints. I think the essential pointis that our usersarerelatively sophisticated and would not be put off by accidentally hitting a wrong key. Of course, you shouldn t nest these simple menustoo deeply since this makes backingoutfrustrating.</p>
<p>Graphical Data Display Manager I suppose that GDDM(assessed via AP126) is regarded as the main IBM tool for full screen work, both text and graphics. However, it is expensive, and buffering of the GDDMcalls where possible should be used to minimise CPU time. There is a good paper by Dick Bowman on howtoset aboutthis (Bo89).</p>
<p>As Dickpoints out, the APL2-supplied GDMX function (in workspace 2 GDMx) has very complex internal logic and seems to be designed on the basis thatit will still work and access any new GDDMcalls that IBM mightinvent in the future. However, I live in the here and now, and Dick s simpler type of code is far preferable. For seriousfull screen work a design tool is essential. Manysites will still have [BM s 2 FSDESIGN which doesthis, but this is obsolescent; in any case it does not support buffered GDDMcalls. Dick Bowman s paper, though, does give the bonesofan alternative design tool which works quite well. Be warned however! If you start copying his code, by the time you have nearly finished you comeacross that most helpful of all APLlines:</p>
<p>FIELDPOS(26] etc, etc, etc.</p>
<p>Yes, you need about another 60 lines here, so you will have to understand the code afterall - which is always a good idea anyway!</p>
<p>FIELDFOS doesall the work to add, delete, move, resize, etc. screen fields and uses a couple of unexplained functions (VOTOVER and PMAP) which are not listed. Their purpose, though,is pretty clear if you remember that you cannot overlap screen fields in GDDM. The way FIELDPOS works is to generate the new format row and columndetails each time a change is made and put these with PFMT which holds the parameters for the fields already set. PMAP starts with a screen-size map of 0 s and goes round PFMT adding a 1in each position of eachfield. If we end up with only 0 s and 1 s it s O.K., otherwise an overlap so throw it away andstart again. This is a much simpler method than trying to detect entry errors and putting up messages, ete. and the user soon learns to be careful with cursor positioning.</p>
<p>127</p>
<p>VECTOR Vol.7 No.4</p>
<p>For whatit s worth, my origin-1 code for those functions (combined together) is given at the end. (I wonder if anyone will complain about that loop?), Finally, before leaving FIELDPOS, thereis a nicelittle idiom there for turning a vector into a 1-column matrix, which I don t remember seeing before:-</p>
<p>» CxO] VECTOR ++ ({(p VECTOR) ,1)pVECTOR</p>
<p>Interactive System Productivity Facility Or ISPF for short. Many people routinely use this withoutrealising thatit is a full screen design system and that you can use it with APL2. Useful documentation is, however, scattered about.</p>
<p>The APL2 manual(IBM1 pp 72, 73) is not very helpful, although it does tell you how to use the ISPF editor to edit APL2 functions. For long functions (and, yes, they are sometimes necessary, viz. FIELDPOS) this is much better than JEDITOR 2 becauseof its powerful insert, move, copy, delete etc. facilities for both single lines and blocksoflines. Worth looking at even if you don t use ISPF for panels. Minorrestrictions: (1) You can t ring-edit - a pity! (2) You cannot have function lines longer than around 230 characters - but who wants to, anyway?</p>
<p>Where next then? You will need up-to-date ISPF manuals (IBM2), (IBM3) and there is a reasonable amount of APL2 there, including some example code (more on whichlater). You can learn how to write ISPF panels from these sources (and it is not difficult) butit is still not easy to see howtoset up a reliable APL2-ISPF bond.</p>
<p>The best starting reference that I have come across is the paper by Loren Mayhew (Ma86) which givesa very clear and detailed account (with code) of one method of proceeding, including the sort of logic needed to incorporate ISPF split-screen mode. (You mustnotallow APL2 to be called twice! It has, as they say, unpredictable results !) The key to the success of this methodis the use of an AP101 stack to do a )RESET and then re-call the original (QLX) calling function.</p>
<p>One small point; the full-stops (periods) don t always showclearly in the code as printed and there are nameslike ,HELP , whichis not the same as HELP . Look for suspicious indentations if in doubt. All in all, I recommendthis paper as a goodplaceto start.</p>
<p>One thing the above paper doesn t touch on is ISPF tables, but (IBM3) is quite good here and has a worked telephone book example with two APL2 functions</p>
<p>128</p>
<p>VECTOR Vol.7 No.4</p>
<p>TELEBK and TELESY. This code can be tidied up somewhat butit is worth coding up to learn about ISPFtables. One thing to be awareofis thatit is not fully robust.If, for example, you try from the panelto delete a report before you have created it, you will have a nasty ABEND. There are a few like this, all sequence dependent, but they are easily covered byinitialising some flags in TELEBK and trapping/switching them in TELESV to enforce correct sequencing.</p>
<p>Thetelephone book and (Ma86) between them cover almost everything you need to know about APL2 with ISPF. One exception is perhaps dynamic table building, a useful techniquefor large tables. (IBM2) has a selection on this with a worked PL/1 example. Even if you don t know PL/1 this is easy to translate into APL and gives a good insight, including how to use the excellent supplied scrollingfacilities e.g. PF7, PF8. Also things like DOWN 500 or M for MAX on the commandline, then PF8 to jumpstraightto the bottom ofthe scrolled data, etc.</p>
<p>With a properly set up ISPF system,there is no need to back out of menus; you can jump from almost anywhere to almost anywhereelse. For example, =3.4 will go straight to option 3.4, and =X will get you home wherever you are. Tf you wantto do this sort of thing yourself, perhaps using GDDM,the logic is not exactly trivial and with ISPFit is already donefor your.</p>
<p>Amongst the things I like about ISPFis the excellent help tutorial; you may need to write some application-specific help panels, but all the stuff about scroll commands,etc. is already there. Another thing is toggling the PF key display. A beginner can type PFSHOW ON onthe commandline to get them displayed and, whenthey are familiar, the PFSHOW OFFwill get rid of them. Apart from the default PF key settings, application-specific ones can readily be set dynamically.</p>
<p>One piece of advice is to never waste time typing in a complete ISPF panel; instead, copy one of your own, or someoneelse s, and changeit. For example, for yourfirst primary panel, pick the ISPF primary panel itself and modify a copy of that. If you see a panel you like somewhere just type PANELID on the command line to get its member name, then go look in the SYS...ISPPLIB libraries, or wherever, to grab a copy.</p>
<p>Someone mentioned to me a while back that a possible problem with ISPF-APL2 was competition for attention interrupts. Our relations with our end users are such that we can simplytell them notthe use the attention key and they meekly obey. However,if this matters to you, I think it can be handled with an attention exit routine in the calling CLIST; this is discussed in (IBM2) but I have not tried it yet.</p>
<p>129</p>
<p>VECTOR Vol.7 No.4</p>
<p>I like ISPF panels. The APL code is a little messy because everything (even numbers) has to be passed as a characterstring, but the panels are easy to set up (almost WYSIWYG)and a wholeraft of excellent data validation routines comes ready-supplied.</p>
<p>ISPF is also available on the PC, I gather, but I have not tried it because you cannotlink it to APL2/PC. IBM tell me that I am the first person to requestthis option, and T hopethat they can find timeto buildit in.</p>
<p>AP124 This was IBM s precursor to AP126 and allowed full-screen panels withoutthe GDDM overhead. Well, IBM don t supply this any more, so why bother to mentionit? Firstly, old VSAPL systems converting to APL2(if there are anyleft who haven't done so already) either have to rewrite the full screen work or simulate this, perhaps even using GDDM (Yuk!). Fortunately I don t havethis problem. What I am moreinterested in are commercial packages like FSM124 from Interprocess Systems which simulates AP124 without using GDDM,andis reportedly cheaper to use. I am hoping to test run this in the not too distant future.</p>
<p>Coming down from the mainframe for a moment, I am also interested in APL2/PC which doesn t have AP126 but does have AP124 so I am also getting involved there. The supplied workspace for AP124 (called AP124) does have a modestfull-screen design function, FSDEF, but this requires all the individual field parameters to be hard-codedinside application functions, which is not very satisfactory for serious design work. Also, the code has been brought down from VSAPLandis distinctly APL1, with a plethora of globalvariables, etc.</p>
<p>If you want an AP124 full-screen design kit, which itself exploits the full-screen capabilities, then it s (almost) all there in Adrian Smith s book (Sm82), if you can beg,steal or borrow a copy. You will need some modifications for the PC (see the IBM PC manual) and, interestingly, you can now use overlapping fields for things like adding a uniform backgroundcolour. Finally, you will need to get rid of the embedded [JA¥ code, and(I'm sure Adrian would agree now that APL2 is available) might nowadays prefer using a nested array to avoid those global names.</p>
<p>Alternatively, if you prefer Dick Bowman s GDDM approach, it is not too difficult to modify this for AP124.</p>
<p>130</p>
<p>VECTOR Vol.7 No.4</p>
<p>Graphics Of the above four methods, only GDDM will do graphics on the mainframe, but as one keeps repeating, it is expensive. If you need mixed text and graphics GDDMcan do this but, if your work is mostly text with a small graphics content, consider using ISPF and calling GDDM from there, which is possible with the latest release of ISPF.</p>
<p>I think, though, that these days many people would recommend downloading the data to a PC and using one of the excellent PC graphics packages like Harvard Business Graphics or Lotus Freelance, etc. This is the approach that we now use. Is GDDM obsolescent?</p>
<p>References [Bo89]_ Bowman,R. APL and GDDM - A High Performance Toolkit APL89 Conference Proceedings pp 43-53</p>
<p>[Ma86] Mayhew,L. Increasing Productivity with ISPF/APL2 APL86 Conference Proceedings pp 243-251</p>
<p>[IBM1] APL2 Programming: System Services Reference IBM Publication SH20-9218-2 (Nov87) [IBM2] ISPF: Dialog Management Guide: MVS" IBM Publication SC34-4112- 00 Jun 87)</p>
<p>[IBM3] ISPF: Dialog Management Services and Examples: MVS IBM Publication SC34-4113-00 (Jun 87) [Sm82] Smith, A. APL - A Design Handbook for Commercial Systems (1982) - Out of print</p>
<p>131</p>
<p>VECTOR Vol.7 No.4</p>
<p>Appendix</p>
<p>VOVER(OIV fo] R+TL_NM OVER PFMT;FF3M;I;F;R8;CS C4] a Check for overlapping fields C2] Ben on no on on nnn en er eee nee a cad aM - 'Bitmap' matrix of screen. M[J;K] is :- Ca] a 0 if unused, 1 if OK fld posn, &gt;1 if overla, Cs] q--------------------------~--------------+ [6] FF+(G+£2}1402]PFM?),C1]7L_NM a Add new fld C7] M+(SS- 0 1)90 a SS is global screen size [8] I+0 C9] BLP:&gt;((AtpFP)&lt;I+*I+1)/OLAP [10] F*FF(I;] [11] (RS CS)+(~14+F01]+1F 03] ) (W1+P C21 +1 F 64] ) C12] MURS; CS]+1+MERS;CS] a Note addition £13] +LP C44] A~- - C18] OLAP:R+lei&lt;,M a R - 4 if overlap, 0 if OK</p>
<p>132</p>
</details>
