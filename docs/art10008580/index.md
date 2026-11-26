---
vid: '10008580'
title: 'Hacker’s Corner: Making Noises in APL'
authors:
- Adrian Smith
volume: '11'
issue: '2'
page: '118'
status: not online
issue_tab: n2-october-1994
---

Adrian Smith
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 118](../11/2/VOL.11-NO.2-OCTOBER-1994.pdf#page=120)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.11 No.2</p>
<p>Hackers Corner: Making Noises in APL by Adrian Smith</p>
<p>Introduction: What is Sound Anyway? Ignoring (for the moment) the odd fact that we have twoears, soundis a strictly one-dimensional phenomenon.All we hear whenlistening to a room full of people talking (or to Beethoven's Fifth in the Albert Hall) is a sequence of positive and negative variations about the normalair pressure in the room.It is easy to get confused and think of a sound wave as some kind of complex pattern of vibration going up-and-downand side-to-side, travelling towards us from each person in the room (or instrumentin the orchestra) as a separate channel of information. Wrong!It is the brain which somehowbuffers the simple vector of amplitude values and deduces whatthe source of each must be. You may be able to listen to the second flute over the background of strings and brass, but you certainly don t hearit!</p>
<p>Once you understand how simple sound wavesare, you will appreciate how easy they are to model in APL! Take a momentto think aboutit, and read on.</p>
<p>Howare Sounds made by Windows? This also turns out to be easier than expected! Windowscan play wave-forms held in files called WAV several are supplied with your system (such as DING.WAV) and many more turn up on various games disks. I would recommend buying the Microsoft Entertainment Pack Vol.4 just for the noises! Unusually, these files are held in a really simple format essentially a 44-byte headerstructure (which is pretty standard) and then a simple sequence of byte values in the range 0-255 giving the pressure variation around a normal value of 128. The rate these values are sent to your sound card (or PC Speakerdriver) is determined by the sampling frequency , typically set at 11,000 persecond.I use 14,567 for no good reason other than that I can hear frequencies up to near this value through my cheapo speakers (Goodman's Active 65 £34 the pair from Dixons, highly recommended). Younger readers with Hi-Fi may need to go abovethis, but WS FULL awaits the unwary!</p>
<p>Playing existing .WAVs from APL is really easy (Vector 10.3, page 99) ... but what we are about here is making new ones with only some basic physics to</p>
<p>118</p>
<p>VECTOR VoL11 No.2</p>
<p>guide us. Let s start with a pure tone (a sine wave) of constant amplitude and work up from there:</p>
<p>V {r}«fone arg;QI0;sam; freg;ampl;data;size [1] A Pure tone at given frequency (amplitude 0 * 128) [2] a Sounds for exactly 1 sec at Hi-Fi sampling rate. (3] OF0&lt;0 9 sam+14567 C4} freq ampl+2targ o ampl+128Lampl [5] data+lLamplxsamp loo2xfregx{isam)+sam t6J a Adjust zero point to 128 +/- data [7J r+i2e+data v</p>
<p>efone 200 128 14567 80tfone 200 128 128 139 149 160 171 181 191 200 209 217 225 232 238 243 247 251 253 255 255 255 254 252 249 245 240 234 228 220 212 204 195 185 175 165 154 143 132 121 110 99 89 78 68 59 50 41 33 26 20 14 10 631000 25 9 13 18 25 31 39 47 56 66 76 86 97 107 118 129 140 154 162 173 183 192</p>
<p>The nextstage is to append a valid header structureto the frontofthis:</p>
<p>Vv reAddHdr snd;hdr;sam;size;0I0 C13 a Add header structure to sound. [2] a R I F F {size} H AV E t m t bl fm ch sam bps blk bps data faJ fidr+82 73 70 70 6 192 0 0 87 65 86 69 102 109 1416 32 160001010 17 43001417 43 0010 8 0 100 97 116 97 226 191 00 [4] OI0+0 9 sam+14567 9 size+zpsnd t5] Ce] Adr{24 25 28 29]&lt;4p¢256 256Tsam [73 Adr{&gt;,/4 40+¢14]+8p99256 256 256 256Tsize Ca] [9] a Return completed structure, ready to be played ... £10] cehdr,snd v</p>
<p>If you really want to know whatis going ononline [3], you will have to read the SDK manuals! The commentonline [2] is a rough translation of the byte values directly below, and indicates where values must be inserted into the constant data. Thetricky bit is getting the encoding right as well as expressing our APL integers as short-integer pairs, we mustreverse the byte ordering (lines [6] and [7]). Here is the tone withits header stuck onthe front:</p>
<p>119</p>
<p>VECTOR Vol.11 No.2</p>
<p>ugtAddHdr Tone 200 128 B2 73 70 70 231 56 0 0 87 65 86 69 102 109 116 32 16 OO O10 10 231 56 0 0 231 560010 8 O 100 97 116 97 231 56 0 0 128 139 149 160</p>
<p>.. hopefully, you can see where the data gotinserted! All we need to do now is to write this lot to file (with a .WAV extension) and Windowswill playit like any other sound.I suggest using rput from Vector 11.1 page 127. A moreinteresting challenge is to play the sound directly from an image in memory this saves the overhead of file 1/O, and avoids cluttering your disk with lots of failed attempts.</p>
<p>9 PlaySound sound;GlobalAlloc;GlobalLock;GlobalUnlock; GlobalFree;handle;pointer;sink;hmemepy;sndPlaySound [aa na Add header to sound and play from memory image. C2] sound+Addidr sound C3] ONA'U kernel. P16|GlobalAlloc U U4! c4] OWA'I4&amp; kernel.P1is|GlobalLock U' c5] DNWA'T kernel.P16|GlobalUnilock U! [6] OWA'I kernel.Pis|GlobalFree U' C7] DWA'kernel.Pi6|hmemcpy I4 &lt;Ui[] Iu' [ej OWA'I mmsystem.P16|sndPlaySound I" I' C9] handle«GlobalAlloc 0(psound) [10] pointer+GlobalLock handle (11) sink«hmemcpy pointer sound{psound) (12] «9 Phe &lt;4&gt; is the flag to play from memory at &lt;pointer&gt; £13] sink+sndPlaySound pointer 4 [14} a Don't forget to free the memory!! [153 sink+GlobalUnlock handle [16] sink«GlobalFree handle v</p>
<p>Whata lot of Microsoft programmers it must take to changea lightbulb! All we are actually doing is moving the completed sound into memory, and telling Windowsto playit from there! If you were to execute: PlaySound Tone 500 128</p>
<p>You would hear a gentle but pure tone through your speakers.I just did!</p>
<p>Adding Texture and Realism Atthis point, I shouldreally stop and let you experiment on your own I have only spent an hour or two messing around with different functions to hear the effect. I would suggest over-driving the data by using an amplitude above 128 for the early values and clipping the tops to give a much squarer waveform, and</p>
<p>120</p>
<p>VECTOR Vol.11 No.2</p>
<p>using a simple decaying exponential to model die-away of the sound. Hereis a first attempt, plotted as a line-graph by the RAIN workspace:</p>
<p>chset'nomark' © chplot Sound 1200 128 o PG+chelose View PG</p>
<p>260</p>
<p>240. 220 200. 180-4] EEE 1604 . 140. F ABigot 120-444 100 80 60-4 40.</p>
<p>20</p>
<p>4 T T T T T T t 0 500 1000 1500 2000 2500 3000 3500 4000</p>
<p>Vv {r}+Sound arg;0I0;sam; freg;amp!l;dur;data;size;dieaway C41) a Reasonable electronic piano' sound. (23 freq amplearg o dur+2{400+freg o sam+14567 [33 Oro&lt;o C4 size+(durxsam csi dieaway+freg+1000000 C6] ampl«2xampix*-dieawayx.size [7] data+LampIxsizepio02x( fregxdur)x{isize)+size [8] a Kill the tail (fails to drive speakers) [9] data+(ampl&gt;2)/data (10] a Check range and adjust zero point ... [11] data«255{0[128+data [12] redata v</p>
<p>121</p>
<p>VECTOR Vol.11 No.2</p>
<p>Notice how the die-awaystarts by making the sound purer at the leading edge, andthen cuts in to reduce the amplitude until thereis solittle left we can trim off the tail completely.</p>
<p>You can improve this further (and the effect of this minor change is quite surprising) by adding random noiseto the very beginning:</p>
<p>¥ {r}+Square arg;DI0;sam;freq;ampl;dur;data;size;dieavay;vol C11 freq vol+arg ° dur+2|400+freg o GTO+0 © sam+14567 C2] size+Ldurxsam C3j dieaway«freq+1000000 cal ampi+1000x*-dieaway*1size cs] data+lamplxsizep1o02x{ fregxdur)*(isize}+size Ce] a Add random noise to leading edge (percussive effect) C7] (200tdata}++7200p1000 [8] data+|dataxvol+0.1*[/|data C93 data+(ampl&gt;2)/data [10] data+255(0[128+data taiJ redata v</p>
<p>.. which comes outas a pretty acceptable Xylophone. chset'nomark' o chplot 1200+Square 2000 128</p>
<p>.. which I don t have space to include, but if you try it you will see how the random noise messes upthe regularity of the waveform (even moreover-driven in this case) to give the effect of a hammerhitting an instrument.</p>
<p>That should be enough to get you started makingsilly noises is all good educational fun, but I can see a lot moreusesforthis stuff in analysing recorded sound, for example comparing Chaffinch dialects across the UK or making sense of Dolphin-speak. Once you know howto read the .WAV format, you can getthe data into APL andstart doing something seriously interesting with it.</p>
<p>122</p>
</details>
