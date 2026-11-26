---
vid: '10012940'
title: 'Hacker''s Corner: Native Files in Dyalog APL (Without Tears)'
authors:
- Adrian Smith
byline: by Adrian Smith
volume: '11'
issue: '1'
page: '126'
source: trad/v111/smith111_126.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- data and files
- system interfaces
- implementations
---

by Adrian Smith
{ .byline }

### Introduction

One of the things you get used to in APL\*PLUS/PC is fast, reliable access to DOS native files; indeed many developers have tended to use these in preference to component files for function filing (see for example my notes on `∆pack` in Vector 8.2 pp.125-126) because the APL\*PLUS component files are somewhat slow by comparison.

In Dyalog/W, the opposite is true! Component files go like greased lightning and the NFILES auxiliary processor is (in my opinion) something of a dog. It has the unpleasant habit of locking out completely if the AP cannot find space to load, and it litters your workspace with a bunch of unhelpfully-named functions like open, close etc. If you would like the pleasure of wiping NFILES.EXE for ever from your hard disk, read on.

### Raw Input and Output

This investigation began when I got interested in making noises with APL! I discovered that the Windows .WAV files are simply a short header, followed by a stream of byte values (say 11000 per second) in the range 0-255 (where 128 represents the midpoint) which outline the waveform. APL has these funny circle functions, which are adept at generating sine waves, so a suitable numeric vector will generate tones and harmonies very easily indeed. Exponential decay superimposed on random numbers gives a very authentic gunshot!

In order to investigate the header structure, I clearly needed to read the file as a stream of untranslated byte values, so turning to the SDK help-file I found enough to hack together the following:

```
     ∇ r←rget name;_lread;_lclose;_llseek;_lopen;fh;size;
       sink;blk;ct;bytes;get
[1]   ⍝ Get Raw file contents as a string of bytes.
[2]   ⍝ Returns ⍬ if file not found.
[3]    ⎕NA'I kernel.P16|_lopen <0T I'
[4]    ⎕NA'U4 kernel.P16|_llseek I U4 I'
[5]    ⎕NA'I kernel.P16|_lclose I'
[6]    ⎕NA'U4 kernel.P16|_lread I >U1[] U2'
[7]    r←⍬ ⋄ fh←_lopen name 0 ⋄ →(¯1=fh)↑0
[8]    size←_llseek fh 0 2
[9]    sink←_llseek fh 0 0
[10]  ⍝ Maximum length is 64K per read, so get blocks ...
[11]   blk←64000⌊size ⋄ r←⍬ ⋄ sink←⎕WA
[12]  ⍝ Don't read lots of garbage with the last block (faster)
[13]  More:get←blk⌊size-⍴r
[14]   ct bytes←_lread fh get get
[15]   →(ct=0)↑Done ⍝ should never happen!
[16]   r,←ct↑bytes
[17]  ⍝ Have we got enough yet?
[18]   →(size>⍴r)↑More
[19]  Done:sink←_lclose fh
     ∇
```

The functions are ANSI-standard C, and closely match the syntax offered by nfiles. Because they are built in to Windows, they appear to run rather fast – here is a trivial example:

```
     rget '\apl\dyalog\serial'
48 48 48 49 53 57 10
```

... note that this has a line-feed (10) but no carriage-return (13), indicating its Unix heritage! The other half of the transaction is filled by:

```
     ∇ {r}←name rput bytes;_lwrite;_lclose;_lcreat;fh;size;
           sink;blk;ct;bytes;put;len
[1]   ⍝ Put a string of bytes to file (no translation).
[2]   ⍝ Returns 1 if OK; 0 if failed.
[3]    ⎕NA'I kernel.P16|_lcreat <0T I'
[4]    ⎕NA'I kernel.P16|_lclose I'
[5]    ⎕NA'U4 kernel.P16|_lwrite I <U1[] U2'
[6]    r←0 ⋄ fh←_lcreat name 0 ⋄ →(¯1=fh)↑0
[7]   ⍝ Maximum length is 64K per write, so make blocks ...
[8]    blk←64000
[9]   More:len←blk⌊⍴bytes ⋄ put←len↑bytes
[10]   ct←_lwrite fh put len
[11]   bytes←len↓bytes
[12]  ⍝ Have we finished yet?
[13]   →(0<⍴bytes)↑More
[14]  Done:sink←_lclose fh ⋄ r←1
     ∇
```

... which will happily write out several K of meaningless noise in less time than it takes to blink. Note that you will need to sort out the CR/LF pairs if you want the file to appear to DOS as records. It is sensibly behaved, in that it will open (and truncate) an existing file, or start a completely new file without complaining.

### Text Files

Having mastered the raw case, it occurred to me that I could probably use the same means to read and write text files as well – sure enough I can still beat the NFILES AP for speed, even though I am doing the ASCII translation in APL! Here are the two functions that do the job, without further comment:

```
     ∇ r←nget name;_lread;_lclose;_llseek;_lopen;fh;size;
       sink;blk;ct;bytes;get;asc;⎕IO
[1]   ⍝ Get Native ASCII file contents as a vector of vectors
[2]   ⍝ Returns '' if file not found.
[3]    ⎕NA'I kernel.P16|_lopen <0T I'
[4]    ⎕NA'U4 kernel.P16|_llseek I U4 I'
[5]    ⎕NA'I kernel.P16|_lclose I'
[6]    ⎕NA'U4 kernel.P16|_lread I >U1[] U2'
[7]   ⍝ Make translate table
[8]    asc←256⍴'∘'
[9]    asc[32+⍳32]←' !"#$%&''()*+,-./0123456789:;<=>?'
[10]   asc[64+⍳32]←'@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_'
[11]   asc[96+⍳32]←'`abcdefghijklmnopqrstuvwxyz{|}~ '
[12]   asc[9 11 14]←⎕TC
[13]   r←⍬ ⋄ fh←_lopen name 0 ⋄ →(¯1=fh)↑0
[14]   size←_llseek fh 0 2
[15]   sink←_llseek fh 0 0
[16]  ⍝ Maximum length is 64K per read, so get blocks ...
[17]   blk←64000⌊size ⋄ r←'' ⋄ sink←⎕WA ⋄ ⎕IO←0
[18]  ⍝ Don't read lots of garbage with the last block (faster)
[19]  More:get←blk⌊size-⍴r
[20]   ct bytes←_lread fh get get
[21]   →(ct=0)↑Done ⍝ should never happen!
[22]   bytes←ct↑bytes
[23]  ⍝ Translate and append ...
[24]   r,←asc[bytes]
[25]  ⍝ Have we got enough yet?
[26]   →(size>⍴r)↑More
[27]  Done:sink←_lclose fh ⋄ ⎕IO←1
[28]  ⍝ Eliminate LF and partition on CR ...
[29]   r←¯1⌽r~⎕TC[3]
[30]   r←1↓¨(r=⎕TC[2])⊂r
     ∇
```

```
     ∇ {r}←name nput txt;_lwrite;_lclose;_lcreat;asc;fh;size;
       sink;blk;ct;bytes;put;len
[1]   ⍝ Put a vector of vectors to file (ASCII translation).
[2]   ⍝ Simple vectors are tolerated!
[3]   ⍝ Returns 1 if OK; 0 if failed.
[4]    ⎕NA'I kernel.P16|_lcreat <0T I'
[5]    ⎕NA'I kernel.P16|_lclose I'
[6]    ⎕NA'U4 kernel.P16|_lwrite I <U1[] U2'
[7]    r←0 ⋄ fh←_lcreat name 0 ⋄ →(¯1=fh)↑0
[8]   ⍝ Make translate table
[9]    asc←256⍴'∘' ⋄ asc[9 11 14]←⎕TC
[10]   asc[32+⍳32]←' !"#$%&''()*+,-./0123456789:;<=>?'
[11]   asc[64+⍳32]←'@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_'
[12]   asc[96+⍳32]←'`abcdefghijklmnopqrstuvwxyz{|}~ '
[13]  ⍝ Look up each element and add CR/LF pairs
[14]   ⍎(2>|≡txt)/'txt←⊂txt'
[15]   txt←⊃,/txt,¨⊂⎕TC[3 2]
[16]   bytes←¯1+255|asc⍳txt
[17]  ⍝ Maximum length is 64K per write, so make blocks ...
[18]   blk←64000
[19]  More:len←blk⌊⍴bytes ⋄ put←len↑bytes
[20]   ct←_lwrite fh put len
[21]   bytes←len↓bytes
[22]  ⍝ Have we finished yet?
[23]   →(0<⍴bytes)↑More
[24]  Done:sink←_lclose fh ⋄ r←1
     ∇
```

... obviously, Scandinavian readers will need to patch the translate table, depending on where in their `⎕AV` they buried `∇` and other exciting things.

As a final bonus, I found that I could write stuff direct to the printer (by using ‘prn’ as the file name) without getting jumped on by Windows – just what my RAIN graphics needs to get its PostScript past the Print Manager queue!

(webpage generated: 14 October 2007, 18:02)
