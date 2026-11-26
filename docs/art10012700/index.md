---
vid: '10012700'
title: 'Hacker''s Corner: Flying Windows'
authors:
- Jonathan Barman
byline: by Jonathan Barman
volume: '10'
issue: '4'
page: '108'
source: trad/v104/barman104_108.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- system interfaces
- user interfaces
---

by Jonathan Barman <span class="affil"></span> <span class="email"></span>
{ .byline }

A little while ago I was shown how to edit the file that produces the screen saver Flying Windows. Windows is supplied with Flying Windows displaying the Windows logo which is actually defined as the very last character in the WinDings font `(¯1↑⎕av)`. The screen saver program file SSFLYWIN.SCR includes this character just before the word DISPLAY. It is very easy to edit SSFLYWIN.SCR using Write with the no conversion option, and to paste the character of your choice from Character Map. I got fed up doing it this way because there was always the chance that I might muck up the file, and I found it difficult to visualise what the WingDings characters looked like at large font sizes. For a bit of fun I wrote an APLIWIN function to allow me to select the character from a list of big WingDings, and eventually came up with the following:

```
    ∇ wingdings;a;s;C;V;⎕io
[1]   ⎕io←0
[2]   C←⎕av[156] ⋄ V←⎕av[255]
[3]   a←'pc menu;pn Wingdings;'
[4]   a←a,'xywh 30 0 35 200;'
[5]   a←a,'cc l1 listbox ws_border ws_vscroll lbs_notify;'
[6]   a←a,'cfont -35 0 0 0 400 0 0 0 2 3 2 1 2 "Wingdings";'
[7]   a←a,'ctext ',V,(,(⎕toapl 33↓¯1↓⎕av),[0.5] C),V,';'
[8]   a←a,'xywh 10 200 30 15;'
[9]   a←a,'cc Ok Button;cn "&Ok";'
[10]  a←a,'adjw 10;cci Cancel;cn "&Cancel";'
[11]  a←a,'csel l1;cfocus;'
[12]  a←a,'pmove 0 0 100 230;'
[13]  a←a,'pcenter;pshow;ptop;wait;'
[14]  s←¯1↓⎕wd a
[15]  �⎕wd 'pclose;'
[16]  →(s[6 7]≡' 7')↓0
[17]  →(0=(⎕toascii ¯1↑s) ⎕hfwritex '\windows\ssflywin.scr 11762')/0
[18]  'File write problem'
    ∇
```

I wanted this to be a completely self contained function, so there are no sub-functions, nor any global variables. Also, it is tailored for my screen size of 800 by 600 pixels. I had to muck about a bit with the numbers on lines \[4\], \[8\] and \[12\] to get the list box about the right size on the screen. To get the numbers for the argument for cfont, on line \[6\], I ran `'*data' wdg wd 'mbfont;'`, and selected a suitably sized WingDing font. I let the results splat out onto the APLIWIN session manager, and then just cut and pasted them into my function. I found the magic number on line \[17\] by reading in the entire file and searching for the word DISPLAY:

```
     ⎕io←0
     a←⎕hfread '\windows\ssflywin.scr'
     ¯1+('DISPLAY'�⎕toapl a)/⍳⍴a
11762
```

I quite like the snowflakes, but there are lots of other possibilities.

(webpage generated: 28 March 2006, 06:49)
