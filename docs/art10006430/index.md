---
vid: '10006430'
title: 'Hacker’s Corner: A Windows Task Killer'
authors:
- Duncan Pearson
volume: '10'
issue: '2'
page: '126'
status: not online
issue_tab: n2-october-1993
---

Duncan Pearson
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 126](../10/2/VOL.10-NO.2-OCTOBER-1993.pdf#page=128)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.2</p>
<p>Hackers Corner: A WindowsTask-Killer for APL by Duncan Pearson</p>
<p>Adrian Smith came to methe other day and said If | start a Windows program from Dyalog with DCMD howcan kill it again from within APL? . So we dived for the manuals and found out a few interesting things. First of all the CMD function, when usedto start a Windowstask, returns a number.</p>
<p>+01CMD 'notepad.exe' minimised! 12345</p>
<p>Aha ! say we, this lookslike a task handle. Why not send a Windows QUIT message to the task and see if it kills itself. If you are a Windowstask then getting a WM_QUIT is like being locked in a room with bottle of whiskey and a loaded revolver and being told to get on withit. It looked like a goodbet.</p>
<p>So we did. We used (WA to associate the PostAppMessage windowsfunction OWA 'I user. P16|PostAppMessage U U I Iu!</p>
<p>and weposted the task a WM_QUIT (message number 18) PostAppMessage 12345 18 0 0</p>
<p>Hello DOS prompt, goodbye Windows bummer! said Pooh!</p>
<p>Apparently OCMD returns something called an hinstance whichis not a task handle but an instance handle, So is there a way from oneto the other? As a matter of fact there is, but you have to look for it. There is a pair of functions in toolhelp.dll with which one can go roundall the tasks in the Windowsevent- queue collecting information from them. One of the bits of information is the hinstance and anotheris the task-handle.</p>
<p>Weassociate them thus: OVA 'I toolheip.Pi6|faskFirst ={Iu4 VUUUIIII 862 8 clic] © uy GWA 'I toolhelp.P16|TaskNext ={I4 UUUUIIIII I U clio} © oy</p>
<p>They are pretty much the same. Each one takes a horrible structure called a TASKENTRY andreturns it (the = means take and return the argument). We pass fPaskFirst an emptystructure andit fills it with details of thefirst task in the</p>
<p>126</p>
<p>VECTOR Vol.10 No.2</p>
<p>queue. Wepassthefilled structure to TaskNext andit replaces the information aboutthe first task with information about the next task. We carry on calling TaskNext until we find a task with the hinstance that was returned, get the task handle of that task from the structure and send it a WM_QUIT.</p>
<p>WhatI omitted to say was what the horrible structure meant. We get back twelve numbers, a ten-character text vector and two more numbers. The text string is the task name, the second number is the task handle and the fourth is the hinstance. I m suretherestis very useful, but not right now soI will ignoreit.</p>
<p>¥ taskfns C1] 8 Associate some useful Windows fns t C2] 'pam'OWA'I user.exe.P16|PostAppMessage UU I Iu C3] first! QNA'I toolhelp.Pi6}faskFirst ={T4 U UY UIIIIIIV efio] © uyt cal tnext'OWA'l toolhelp.Pi6|faskWext ={T4 UU UY I rl ruc Cio] I gv}! v</p>
<p>¥ list+Task_List;qqipam;first;next C12 a List handles and names of all extant tasks {2) taskfns 0 list+'! £3] qg+2arirste (12440), (cide! 1),0 0 Ca] Nextinme13ogqg 0° am+("1+nm DAVC1)) tam cs] dist;+cnm Ce] &gt;('DYALOG'=6tnm) to C71 qq+2anext&lt;qg C8] aNext</p>
<p>¥ Kilj_Name name;qq;pam;first;next [1] a Kill task by name C2] taskfos tal gqgr2zfirstc(12t60},(c10p' 1),0 0 Cay Next:+(namex(pname)}t13ogq)tKill cs) +('DYALOG!=64213 gg) to ced qq+2enextegg C77 aNext Cal Kill:gg+pam(2&gt;gq}18 0 0 ¥</p>
<p>Vv Xill_Num hinstance;qq;pam;first;next (1) a Kill by instance handle (returned by dyalog on UCMD) C2] a W,B. It had better be therelt Ca] taskins Cs] gge2ofirstc(12tu0),(e10p' 17,0 6 Cs] Next:&gt;(hinstance=42qq)tKiil Cel qq+zenextegg C7] wNext Ce] Kill:gq+pam(2&gt;gqg)1iB 0 a y</p>
<p>Kill_Name 'CLOCK* &amp; Harmless enough Kill_Name 'PROGMAN' a Less sensible ... Kili_Name Task_List a... for a very clear desktop!</p>
<p>127</p>
</details>
