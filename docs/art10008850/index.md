---
vid: '10008850'
title: 'Hacker’s Corner: Making Waves in Win32'
authors:
- Adrian Smith
byline: by Adrian Smith (adrian@cgsl.demon.co.uk)
volume: '16'
issue: '2'
page: '110'
source: trad/v162/hack162.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1999
tags:
- system interfaces
- programming techniques
---

*by Adrian Smith (adrian@cgsl.demon.co.uk)*
{ .byline }

The one piece of luggage I invariably forget to pack is some kind of travelling alarm clock. This habit came home to roost in Austria on our eclipse-spotting outing, when we needed to be up early to catch the morning coach for a cross-border raid on the Czech Republic. Of course I had a computer with me, but Delia (being nearly 4 years old) has no sound card, so I regret to say I wrote my wake-up call in APL\*PLUS/PC using `⎕SOUND`. However this reminded me that several people had been having trouble getting WAV files to play properly on Windows95, and that I had never updated my little workspace that plays sounds from memory. I rather expected this to be easy, but good ol’ Microsoft had gone out of their corporate way to make things awkward, as usual ....

### Playing WAV Files in Windows95

Here is a simple translation of the old 16-bit code ...

```
     ∇ {r}←{flg}Play snd;sps
[1]   ⍝ Play a .WAV file (synchronous by default)
[2]   ⍝  flg←+/0(synch) 1(asynch) 2(no deflt) 8(repeat) 16(nostop)
[3]    snd←snd,(~'.'∊snd)/'.WAV'
[4]    'sps'⎕NA'U winmm.C32|sndPlaySoundA <0T U'
[5]    ⍎(0=⎕NC'flg')/'flg←0'
[6]    r←sps snd flg
     ∇
```

Of course they changed the name of the DLL and you need to add an ‘A’ to the function name, but so far so good. It makes a noise. However when you call it with a 1 as the optional left argument, silence. This is a real nuisance if you want sound effects in games, as you really want the noise to keep playing after control has returned to APL. Stef provided the answer to this one ...

```
     ∇ {r}←{flg}Play snd
[1]   ⍝ Play a .WAV file (asynchronous by default)
[2]   ⍝  flg←+/0(synch) 1(asynch) 2(no deflt) 8(repeat) 16(nostop)
[3]    snd←snd,(~'.'∊snd)/'.WAV'
[4]    'sps'⎕NA'U winmm.C32|sndPlaySoundA <0T U'
[5]    ⍎(0=⎕NC'flg')/'flg←0'
[6]    r←sps snd flg
     ∇
```

Spot the difference! That’s right, the boot’s bigger [\[1\]](#ref). Well actually the header line is smaller, by 4 characters – when the `⎕NA` call goes out of scope the sound dies, so you *must not localise it*. First problem solved, but Adrian just a little peeved.

### Playing Sounds from Memory

This is the real he-man stuff. The 16-bit code made heavy use of HMEMCPY to move bytes into a block of allocated, locked memory. Attempts to make the obvious translation to 32-bit calls met with nothing but VALUE ERROR as I tried kernel32, user32 and so on. Having nearly given up, I dug into the Windows documentation on the CD and found no trace of HMEMCPY in the Win32 API. However I did get some hits in Windows.H, but as a #DEFINE to something like RTLCopyMemory. OK, let’s look for this ... once again no joy so back to the header file to discover that a little further down, this is itself defined as RtlMoveMemory, with exactly the same syntax as before. Actually, it copies, and the significance of the Rtl is that the rightmost bytes are copied first which can matter if your memory blocks overlap. So, to play a sound from memory:

```
     ∇ PlaySound sound;GlobalAlloc;GlobalLock;GlobalUnlock;
       GlobalFree;handle;pointer;sink;RtlMoveMemory;sndPlaySoundA
[1]   ⍝ Add header structure to sound and play from memory image.
[2]    sound←AddHdr sound
[3]    ⎕NA'U kernel32.C32|GlobalAlloc U U'
[4]    ⎕NA'U kernel32.C32|GlobalLock U'
[5]    ⎕NA'U kernel32.C32|GlobalUnlock U'
[6]    ⎕NA'U kernel32.C32|GlobalFree U'
[7]    ⎕NA'kernel32.C32|RtlMoveMemory U <U1[] U'
[8]    ⎕NA'U winmm.C32|sndPlaySoundA U U'
[9]    handle←GlobalAlloc 0(⍴sound)
[10]   pointer←GlobalLock handle
[11]   sink←RtlMoveMemory pointer sound(⍴sound)
[12]  ⍝ The <4> is the flag to play from memory at <pointer>
[13]   sink←sndPlaySoundA pointer 4
[14]  ⍝ Don't forgat to free the memory!!
[15]   sink←GlobalUnlock handle
[16]   sink←GlobalFree handle
     ∇
```

Wow! Let’s try it ...

```
     ∇ MakeTSF
[1]   ⍝ Make Tonic Sol Fah as variables
[2]    (doh re mi fah soh lah ti dohtop)←{100,⍨440×(2*÷12)*+/⍵↑0 2 2 1 2 2 2 1}¨⍳8
     ∇
doh re mi
 440 100  493.8833013 100  554.365262 100
     ∇ {r}←Square arg;⎕IO;sam;freq;ampl;dur;data;size;dieaway;vol
[1]    freq vol←arg ⋄ dur←2⌊400÷freq ⋄ ⎕IO←0 ⋄ sam←14567
[2]    size←⌊dur×sam
[3]    dieaway←freq÷1000000
[4]    ampl←1000×*-dieaway׼size
[5]    data←⌊ampl×size⍴1○○2×(freq×dur)×(⍳size)÷size
[6]   ⍝ Add random noise to leading edge (percussive effect)
[7]    (200↑data)+←?200⍴1000
[8]    data←⌊data×vol÷0.1ח/|data
[9]    data←(ampl>2)/data
[10]   data←255⌊0⌈128+data
[11]   r←data
     ∇

     ⍴Square doh
13242

     ∇ Scale
[1]    {PlaySound Square ⍵}¨doh re mi fah soh lah ti dohtop
     ∇
     ∇ r←AddHdr snd;hdr;sam;size;⎕IO
[1]   ⍝ Add header structure to sound, assuming standard sampling rate.
[2]   ⍝    R  I  F  F {size}     W  A  V  E   f   m   t  bl          ... etc
[3]    hdr←82 73 70 70 6 192 0 0 87 65 86 69 102 109 116 32 16 0 0 0 1 0 1 0 17 43
            0 0 17 43 0 0 1 0 8 0 100 97 116 97 226 191 0 0
[4]    ⎕IO←0 ⋄ sam←14567 ⋄ size←⊃⍴snd
[5]
[6]    hdr[24 25 28 29]←4⍴⌽256 256⊤sam
[7]    hdr[∊4 40+⊂⍳4]←8⍴⌽256 256 256 256⊤size
[8]
[9]   ⍝ Return completed structure, ready to be played ...
[10]   r←hdr,snd
     ∇
```

OK, it sounds terrible, as the lower notes take a little longer to compute than the high ones! However for making happy beeping noises at the end of a long process – the sort of thing we always used `⎕SOUND` for – it works rather well.

### <a id="ref"></a>Reference

1. Jeremy Clarkson, *BBC Top Gear*, on the new Mazda MX-5.
