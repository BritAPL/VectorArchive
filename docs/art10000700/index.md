---
vid: '10000700'
title: 'Hacker’s Corner: Flying Windows'
authors:
- Jonathan Barman
volume: '10'
issue: '4'
page: '108'
status: not online
issue_tab: n4-april-1994
---

Jonathan Barman
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 108](../10/4/VOL.10-NO.4-APRIL-1994.pdf#page=110)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.4</p>
<p>Hackers Corner: Flying Windows by Jonathan Barman</p>
<p>little while ago I was shown howto edit thefile that produces the screen saver Flying Windows. Windows is supplied with Flying Windows displaying the Windows logo which is actually defined as the very last character in the WingDings font ("1+0av).</p>
<p>The screen saver program file SSFLYWIN.SCR includes this character just before the word DISPLAY. It is very easy to edit SSFLYWIN.SCR using Write with the no conversion option, and to paste the character of your choice from Character Map.I got fed up doingit this way because there was always the chance that] might muckup thefile, and I foundit difficult to visualise what the WingDings characters lookedlike at large fontsizes.</p>
<p>Fora bit of fun I wrote an APLIWIN function to allow meto select the character from list of big WingDings, and eventually came up with the following:</p>
<p>VY wingdings;a:s;C;V;QDio taj Dio+o C2] C+lav£156} © V+fav([255] C3J a«'pe menu;pn Wingdings;! c4aj ata,'xywh 30 0 35 200;' Cs} ata,'ce 11 listbox ws_border ws_vscroll ibs_notify;' [6] ara,'cfont -35 09060 400000232212 Wingdings ;' C7) aea,'clext ',V,(,(Otoap! 3347isDav),[0.5] C),V,':! [8] a+a,'xywh 10 200 30 15;' £9] ata,'ce Ok Button;cn ROKk";* (10) a+a,'adjw 10;eci Cancel;cn "&amp;Cancel";! (11) ata,'csel !1;¢focus;' [12] ata,'pmove 0 0 100 230;' (13] a+a,'pcenter; pshow;ptop;vait;' (14) s+"asQwd a [15] «wd 'pclose;' (16) +(sl6 71¥#' 7')+0 [17] +(0=(Dtoascii its) DAfwritex 'Nwindows\ssflywin.ser 11762')/0 [18] File write problem' v</p>
<p>] wanted this to be a completely self-contained function, so there are no sub- functions, nor any global variables. Also,it is tailored for my screen size of 800 by 600 pixels. I had to muck about a bit with the numbers on lines [4], [8} and</p>
<p>108</p>
<p>VECTOR Vol.10 No.4</p>
<p>[12] to get the list box aboutthe right size on the screen. To get the numbersfor the argumentfor qT]</p>
<p>cfont, on line [6], lran '*data' wdg wd</p>
<p>Hay pore dO mbfont;', and selected a suitably sized WingDing font. I let the results splat out onto Ww the APLIWIN session manager, and thenjust cut and pasted them into my function.</p>
<p>I found the magic number on line [17] by reading in the entire file and searching for the word DISPLAY:</p>
<p>Dio+o atQhfread '\windows\ssflywin.scr! 1+('DISPLAY'eOtoapl a)/ipa 11762</p>
<p>I quite like the snowflakes, butthere are lots of otherpossibilities.</p>
<p>109</p>
</details>
