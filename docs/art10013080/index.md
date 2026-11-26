---
vid: '10013080'
title: 'Hacker''s Corner: Making Noises in APL'
authors:
- Adrian Smith
byline: by Adrian Smith
volume: '11'
issue: '2'
page: '118'
source: trad/v112/smith112_118.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1994
tags:
- system interfaces
- programming techniques
---

by Adrian Smith
{ .byline }

### Introduction: What is Sound Anyway?

Ignoring (for the moment) the odd fact that we have two ears, sound is a strictly one-dimensional phenomenon. All we ‘hear’ when listening to a room full of people talking (or to Beethoven’s Fifth in the Albert Hall) is a sequence of positive and negative variations about the normal air pressure in the room. It is easy to get confused and think of a sound wave as some kind of complex pattern of vibration going up-and-down and side-to-side, travelling towards us from each person in the room (or instrument in the orchestra) as a separate channel of information. Wrong! It is the brain which somehow buffers the simple vector of amplitude values and deduces what the source of each must be. You may be able to listen to the second flute over the background of strings and brass, but you certainly don’t hear it!

Once you understand how simple sound waves are, you will appreciate how easy they are to model in APL! Take a moment to think about it, and read on.

### How are Sounds made by Windows?

This also turns out to be easier than expected! Windows can play ‘wave-forms’ held in files called .WAV – several are supplied with your system (such as DING.WAV) and many more turn up on various games disks. I would recommend buying the Microsoft Entertainment Pack Vol.4 just for the noises! Unusually, these files are held in a really simple format – essentially a 44-byte header structure (which is pretty standard) and then a simple sequence of byte values in the range 0-255 giving the pressure variation around a ‘normal’ value of 128. The rate these values are sent to your sound card (or PC Speaker driver) is determined by the ‘sampling frequency’, typically set at 11,000 per second. I use 14,567 for no good reason other than that I can hear frequencies up to near this value through my cheapo speakers (Goodman’s Active 65 – £34 the pair from Dixons, highly recommended). Younger readers with Hi-Fi may need to go above this, but WS FULL awaits the unwary!

Playing existing .WAVs from APL is really easy (Vector 10.3, page 99) ... but what we are about here is making new ones with only some basic physics to guide us. Let’s start with a pure tone (a sine wave) of constant amplitude and work up from there:

```
     ∇ {r}←Tone arg;⎕IO;sam;freq;ampl;data;size
[1]   ⍝ Pure tone at given frequency (amplitude 0 ⍲ 128)
[2]   ⍝ Sounds for exactly 1 sec at Hi-Fi sampling rate.
[3]    ⎕IO←0 ⋄ sam←14567
[4]    freq ampl←2↑arg ⋄ ampl←128⌊ampl
[5]    data←⌊ampl×sam⍴1○○2×freq×(⍳sam)÷sam
[6]   ⍝ Adjust zero point to 128 +/- data
[7]    r←128+data
     ∇

     ⍴Tone 200 128
14567
     80↑Tone 200 128
128 139 149 160 171 181 191 200 209 217 225 232 238 243 247 251 253 255 255 255 254 252 249 245 240 234 228 220 212 204 195 185 175 165 154 143 132 121 110 99 89 78 68 59 50 41 33 26 20 14 10 6 3 1 0 0 0 2 5 9 13 18 25 31 39 47 56 66 76 86 97 107 118 129 140 151 162 173 183 192
```

The next stage is to append a valid header structure to the front of this:

```
     ∇ r←AddHdr snd;hdr;sam;size;⎕IO
[1]   ⍝ Add header structure to sound.
[2]   ⍝    R  I  F  F {size}     W  A  V  E   f   m   t
bl          fm  ch  sam        bps     blk bps    d  a  t  a
[3]    hdr←82 73 70 70 6 192 0 0 87 65 86 69 102 109 116
32 16 0 0 0 1 0 1 0 17 43 0 0 17 43 0 0 1 0 8 0 100 97 116 97 226 191 0 0
[4]    ⎕IO←0 ⋄ sam←14567 ⋄ size←⊃⍴snd
[5]
[6]    hdr[24 25 28 29]←4⍴⌽256 256⊤sam
[7]    hdr[⊃,/4 40+⊂⍳4]←8⍴⌽256 256 256 256⊤size
[8]
[9]   ⍝ Return completed structure, ready to be played ...
[10]   r←hdr,snd
     ∇
```

If you really want to know what is going on on line \[3\], you will have to read the SDK manuals! The comment on line \[2\] is a rough translation of the byte values directly below, and indicates where values must be inserted into the constant data. The tricky bit is getting the encoding right – as well as expressing our APL integers as short-integer pairs, we must reverse the byte ordering (lines \[6\] and \[7\]). Here is the tone with its header stuck on the front:

```
     48↑AddHdr Tone 200 128
82 73 70 70 231 56 0 0 87 65 86 69 102 109 116 32 16 0 0 0 1 0 1 0 231 56 0 0 231 56 0 0 1 0 8 0 100 97 116 97 231 56 0 0 128 139 149 160
```

... hopefully, you can see where the data got inserted! All we need to do now is to write this lot to file (with a .WAV extension) and Windows will play it like any other sound. I suggest using rput from Vector 11.1 page 127. A more interesting challenge is to play the sound directly from an image in memory – this saves the overhead of file I/O, and avoids cluttering your disk with lots of failed attempts.

```
     ∇ PlaySound sound;GlobalAlloc;GlobalLock;GlobalUnlock;
        GlobalFree;handle;pointer;sink;hmemcpy;sndPlaySound
[1]   ⍝ Add header to sound and play from memory image.
[2]    sound←AddHdr sound
[3]    ⎕NA'U kernel.P16|GlobalAlloc U U4'
[4]    ⎕NA'I4 kernel.P16|GlobalLock U'
[5]    ⎕NA'I kernel.P16|GlobalUnlock U'
[6]    ⎕NA'I kernel.P16|GlobalFree U'
[7]    ⎕NA'kernel.P16|hmemcpy I4 <U1[] I4'
[8]    ⎕NA'I mmsystem.P16|sndPlaySound I4 I'
[9]    handle←GlobalAlloc 0(⍴sound)
[10]   pointer←GlobalLock handle
[11]   sink←hmemcpy pointer sound(⍴sound)
[12]  ⍝ The <4> is the flag to play from memory at <pointer>
[13]   sink←sndPlaySound pointer 4
[14]  ⍝ Don't forget to free the memory!!
[15]   sink←GlobalUnlock handle
[16]   sink←GlobalFree handle
     ∇
```

What a lot of Microsoft programmers it must take to change a lightbulb!! All we are actually doing is moving the completed sound into memory, and telling Windows to play it from there! If you were to execute:

```
     PlaySound Tone 500 128
```

You would hear a gentle but pure tone through your speakers. I just did!

### Adding Texture and Realism

At this point, I should really stop and let you experiment on your own – I have only spent an hour or two messing around with different functions to hear the effect. I would suggest ‘over-driving’ the data by using an amplitude above 128 for the early values and clipping the tops to give a much squarer waveform, and using a simple decaying exponential to model ‘die-away’ of the sound. Here is a first attempt, plotted as a line-graph by the RAIN workspace:

```
     chset'nomark' ⋄ chplot Sound 1200 128 ⋄ PG←chclose
     View PG
```

## ![figure\_1](smith112_118-fig1.gif)

```
     ∇ {r}←Sound arg;⎕IO;sam;freq;ampl;dur;data;size;dieaway
[1]   ⍝ Reasonable 'electronic piano' sound.
[2]    freq ampl←arg ⋄ dur←2⌊400÷freq ⋄ sam←14567
[3]    ⎕IO←0
[4]    size←⌊dur×sam
[5]    dieaway←freq÷1000000
[6]    ampl←2×ampl×*-dieaway×⍳size
[7]    data←⌊ampl×size⍴1○○2×(freq×dur)×(⍳size)÷size
[8]   ⍝ Kill the tail (fails to drive speakers) ...
[9]    data←(ampl>2)/data
[10]  ⍝ Check range and adjust zero point ...
[11]   data←255⌊0⌈128+data
[12]   r←data
     ∇
```

Notice how the die-away starts by making the sound ‘purer’ at the leading edge, and then cuts in to reduce the amplitude until there is so little left we can trim off the tail completely.

You can improve this further (and the effect of this minor change is quite surprising) by adding random noise to the very beginning:

```
     ∇ {r}←Square arg;⎕IO;sam;freq;ampl;dur;data;size;dieaway;vol
[1]    freq vol←arg ⋄ dur←2⌊400÷freq ⋄ ⎕IO←0 ⋄ sam←14567
[2]    size←⌊dur×sam
[3]    dieaway←freq÷1000000
[4]    ampl←1000×*-dieaway×⍳size
[5]    data←⌊ampl×size⍴1○○2×(freq×dur)×(⍳size)÷size
[6]   ⍝ Add random noise to leading edge (percussive effect)
[7]    (200↑data)+←?200⍴1000
[8]    data←⌊data×vol÷0.1×⌈/|data
[9]    data←(ampl>2)/data
[10]   data←255⌊0⌈128+data
[11]   r←data
     ∇
```

... which comes out as a pretty acceptable Xylophone.

```
chset'nomark' ⋄ chplot 1200↑Square 2000 128
```

... which I don’t have space to include, but if you try it you will see how the random noise messes up the regularity of the waveform (even more over-driven in this case) to give the effect of a hammer hitting an instrument.

That should be enough to get you started – making silly noises is all good educational fun, but I can see a lot more uses for this stuff in analysing recorded sound, for example comparing Chaffinch dialects across the UK or making sense of Dolphin-speak. Once you know how to read the .WAV format, you can get the data into APL and start doing something seriously interesting with it.

(webpage generated: 22 October 2007, 01:50)
