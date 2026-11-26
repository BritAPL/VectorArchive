---
vid: '10008890'
title: 'Hacker’s Corner: Using Watchpoints in APL+Win 3.6'
authors:
- Adrian Smith
volume: '16'
issue: '4'
page: '95'
status: not online
issue_tab: n4-april-2000
---

Adrian Smith
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 95](../16/4/VOL.16-NO.4-APRIL-2000.pdf#page=97)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.16 No.4</p>
<p>Hacker s Corner: Using and Abusing Watchpoints in APL+Win 3.6 some experiments by Adrian Smith</p>
<p>Background Those of you who build applications in Causeway (shareware or Pro) will know that its main design objective was to take away from the programmer(me) the responsibility of remembering which Gui objects needed to be updated when some underlying data changed. Now I think it is fair to have the programmer state explicitly the dependencies ( this field will need fixing when any of the following change... ) although the screen design software can usually make a fair guess just by stripping the valid namesout of the object s data expression; whatis notfair is to have to rememberto notify the change to some variable when you run a callback that re-assigns it. Surely the interpreter knows that you assigned some new data to user_name - whycan t it justtell the program?</p>
<p>Where do Watchpoints comeinto it? The concept of watchpoints has been around in real development environments for years, and several old mainframe APLs (Xerox, Burroughs?) had something similar. Dyalog actually demonstrated ) watch in an early version of Dyalog/386 at a BAA meeting (to general acclaim) but somehow it never madeit into the Windowsinterpreter. J has a related idea in the 4!:5 conjunction which reports all the names which changed since youlast called it. For debugging, watchpoints are just amazingly useful. For example in my Rain graphics engine, I build the description of the chart as a simple character vector of PostScript code. This involves a lot of APLlike...</p>
<p>R+R, C ',(CHSFMT XPOS,TLF,XPOS,YL),' ] ',ATT,' pspL ',nl</p>
<p>... to draw a polyline (1 think this may be a Y-axis). Quite often, my fingers get ahead of my brain andI forget to format the co-ordinates. APL is horribly tolerant of this kind of misdemeanour, so everything carries on until I hit WS FULL (because I now have somehorrid nested heterogeneous miscegenation lurking in my workspace) or until I try to view thechart, saveitto file etc.</p>
<p>95</p>
<p>VECTOR Vol.16 No.4</p>
<p>Nowat the end of each block of code, the temporary variable R gets catenated toa global buffer with an expression like: CHAPG+CHAPG, ps .. Just suppose we could watch this buffer and pull up short the momentit ceased to be a pure character vector? Well now wecan, andit is amazingly easy to set it up:</p>
<p>Qwatchpoints+1 2p'CHAPG' ~a/CHaPGelav' chPlot 112 WATCH POINT TRIGGERED ~a/CHaPGeDav A si DwArcHPOrINT[i} CHacatle] chPlot{22]</p>
<p>So what happened, and where am I? Every time 1 make an assignment to the buffer, the expression ~,/CHAPGe(av is evaluated.If it is true (non-zero) then mycode is immediately suspended in a phantom function called QWATCHPOINT, and I can lookat the stack to see which line of my code caused the problem. Note that the help file is misleading here - it states Thefirst column holds the nameof the variable whose imminent change in value will trigger evaluation of the expression in the second colunin; ... which implies that the suspension will occur before the damage is done.In fact it does the assignmentfirst, but at least you have the smoking gun to hand.</p>
<p>Now,you can call functions in that right-hand column (whinge #2 ~ the help file states If the function returns a zero or no result, execution will not be suspended, but whatever code the function contains will have been executed. but in fact you must return a result) and you can run as many expressions as you like against the same variable...</p>
<p>Qwatchpoints+3 zp'x' 'QO+''x has changed'' x! X*12 x has changed 12 X has changed 12 x has changed 12</p>
<p>... but don t think you can abuse it with thingslike....</p>
<p>Owatchpoints+i 2p'x' 'Ok''x has changed'' (x+x+i)?</p>
<p>96</p>
<p>VECTOR VoL16 No.4</p>
<p>While we are in the mood, whinge#3 - the help file is quite explicitly wrong in stating Notealso thatthe single character variable name Z must be raveled to a vector before assignment; scalars are not accepted. - scalars are accepted, as in the example on the previous page.</p>
<p>This all begins to feel rather useful, not just as a debugging tool but quite possibly as part of a real application. Suppose we are running a +Win OLE server using Excel as the Gui interface. Maybethis is running somefancy statistical model, and it might maintain some intermediate variables to show how it is getting on. Perhaps the spreadsheet could display these variables as they change? Nowall we have to do is to watch the variables in our server and use the Notify method on rootto tell Excel that something changed, and probably whatit is and the new value. Which brings meto...</p>
<p>A 1-Line Causeway Engine Remember, Causeway is all about things changing, and other things getting told aboutit. Suppose we have:</p>
<p>Aname Adrian Smith</p>
<p>v Flip Ca] a The famous Causeway 'Flipper' using watchpoints C2] fm' OWI 'Create! 'Form' ( where' 24 12 12 40) C3] fm.ed' OWI Create 'Edit' ( where! 2 4 1.2 24) C4] fm.ed' Qwi 'text' aname Cs] fm. bb' OWI Create Button' ('caption' 8FPlip')({'where' 6 4 2 12) (6) fm. bb' Qwi 'onClick' aname«¢aname' (7) C8] Owatchpoints+1 2p'aname' '''fm.ed'' Dwi ''text'' aname' Ca] [10] 'fm' OWL Waitt v</p>
<p>Then Bob, as they say, is your uncle. Whetherthis is sensible, and if so how eae 2,] one might set about implementingit i see for real, remain open questions. . = However it works so simply and reliably I can t help feeling that it has to be a technology worth exploiting. Watch this space!</p>
<p>97</p>
<p>Umbilic Torus NC by Umbilic Torus created in Jsoftware Helaman Ferguson, 1986</p>
<p>Por those who see the elegancein an algorithm, Jsoftware offers an OLAPprogramming language idealfor solving complexproblems.</p>
<p>JSsoftware 1s the modeling tool ofchotce tn the art ofprogramming. Jsoftware is an OLAPlanguage.It s highly optimized for operating on whole high performance interactive nature datasets at a time, allowing you to allows a programmer to concisely manipulate data far more easily than express algorithms when analyzing with conventional software. Also,sparse complex data sets. amay support provides an efficient form of storage for very large The core language is based data sets. on a small set of simple yet consistent rules with many Makesure to join the JForum powerful factlities to define new for a lively discussion on a operations. Built-in functions a variety of interesting J topics, ee: www,jsoftware.com/f « Strand Software,Inc. s J foram. htm 19235 Covington Ct. Shorewood,MN 55331 (612) 470-7345 i Fax (612) 470-9202 aaa software.com info@jsoftware.com :</p>
</details>
