---
vid: '10002060'
title: 'Review: Dyalog APL CE (beta) – first impressions'
authors:
- Ray Cannon
byline: 'by Ray Cannon (email: Bob Hoekstra) 22nd September 2002'
volume: '19'
issue: '2'
page: '46'
source: trad/v192/pocket.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2002
tags:
- reviews
- implementations
---

*by Ray Cannon (email: [Bob Hoekstra](mailto:webmaster@vector.org.uk))* 22nd September 2002
{ .byline }

A new version of Dyalog APL was announced at the Dyadic Vendor forum at APL 2002 in Madrid. What makes it so special is that it is a FULL copy of Dyalog APL, it’s expected to “cost as little as $50” and runs on the latest Pocket PCs.

A “Pocket PC” is a Personal Digital Assistant (PDA) running Microsoft Pocket PC 2002 software (Windows CE 3.0, Pocket Outlook, Pocket Word and Pocket Excel).

So two weeks ago I bought myself a HP Jornada 568, a Pocket PC with a 206MHz StrongArm processor, 64 MB ram, 32 MB flash rom, a 320x240 16-bit reflective TFT colour display, and type 1 Compact Flash port.  I downloaded a beta copy of “Pocket APL” from Dyadic’s web site, and have been playing with it in my free time ever since.

## Keyboard

Most Pocket PCs use a stylus and virtual keyboard, but physical keyboards are available as add-ons for many makes, which allow one to “thumb type” (as against “touch type”).

I do not yet have such a keyboard for my Jornada, so cannot tell you if it works in conjunction with the APL keyboard. However, since the virtual keyboard, when displayed, takes up about 1/3 of the screen, it would be a real asset if it does work, even if I still type with the stylus!

Using the stylus takes me back 30 years to my “ROLF HARRIS STYLOPHONE” days. On the Stylophone, you could not play chords, only single notes. So it is with the Pocket PC, “computer keyboard chords“ ( such as “rho” = &lt;ctrl>r) are not possible, so are entered via a sequence of single strokes, &lt;ctrl> then &lt;r>.

(I wonder if I can turn my Pocket PC into a Stylophone via Dyalog APL?)

## Installation

Installation of the beta copy of Dyalog (and later, a beta upgrade) was simple and painless. After installing APL I was in such a hurry to use it, that I failed to notice that the newly installed “Pocket APL Keyboard” had not been selected. So I wasted half an hour wondering how to type in APL. Once I had realised that I needed to select this second “keyboard”, I was off. Moral – RTFM.

Having seen a demo of “Rain in Spain” I knew it would run many Dyalog APL workspaces, without problem. (Pocket APL will run the 1996 shareware version of Adrian Smith’s *Rain*, supplied as an “outer product” with Dyalog 7.2, as demonstrated by Dyadic on the last day of the 2002 APL conference in Madrid. )

However, I also knew that the “Windows CE API”, did NOT contain the same set of DLLs as the “Win32 API” as used by Win 95/98/Me/2000 etc., so, as an initial project, I decided to convert all my utility functions that used DLLs to also work under CE wherever possible.

## Chicken or Egg?

The first problem I had to overcome was “How does code know if it’s running under ‘Win32 API’ or ‘Win CE API’?”

The API function “GetVersionEx” that is available under both Win32 and WinCE returns suitable information so one can make this choice. However, the name of the DLL it resides in is different. So the ⎕NAcode needed to call it, needs to know which OS it is running, so one needs to call the API GetVersionEx…..

OK, a different approach is required, let’s see what APLVersion gives us.

```
      '.' ⎕wg 'APLVersion'
 Windows for Pocket PC  10.0.0  P Development
```

Note, the 3rd element of APLVersion is “P” compared to “W” for the Win32 API, and “M” (Motif) from Unix based machines. (In the first beta release, APLVersion returned “CE” rather than “P” but this was changed to prevent length errors in running existing code, that some beta testers have reported.)

```
     ∇PoW←{⍝ Pocketpc or Win32 API?
[1]   ⍝Returns  ⍵ if APLVersion is W else ⍺
[2]        ⎕ML←0
[3]        'W'=⊃⊃3⊃'.'⎕WG'APLVersion':⍵
[4]        ⍺   ⍝ Pocket PC or Motife etc }     ∇
```

Now please note that I have /am playing with BETA software, and have already received one update which fixed/resolved several issues with the original release. In addition to APL related issues, the update has greatly improved the input facilities available to the “developer” e.g. “Auto complete”. Dyadic are acting on the feedback from the beta testers. So be warned, the final production release may well have significant changes from the current software I am testing.

Now, on a full-sized PC, I find “Auto complete” a real distraction, and I turn it off whenever possible. However, when armed with only a “pointed stick” and a “virtual keyboard” the “Auto complete” feature is a real boon. You will do almost anything to save a few “key-strokes” or should I say “stick-taps”.

As implemented by Dyadic, the “Auto complete” works very well. I have adjusted its parameters so it does not guess until you have typed 2 characters. I found the default of 1 character just not worth the effort. (It’s a bit like that old TV quiz “I’ll name that tune in one”, sheer luck if it is correct.)

## The Core of the Kernel

Depending on platform, Windows APIs can handle text as either single-byte (ASCII) or multi-byte (normally 2-byte – UNICODE). To allow for this, many API functions which handle text can have two forms, and their names have either a trailing “A” (ASCII) or “W” (wide).  Windows CE 3 supports UNICODE only, hence the API functions end with “W” not “A”.

On a Pocket PC most of the available APIs are to be found in COREDLL. Whereas under Win32, the APIs there are spread out a lot more; in Kernel32, User32, Gdi32, Advpi32 to name just a few.

So here is a function to return the Windows version information:

```
     ∇r←GetVersion;get;val;dll;dos;build;ex;rc;str;txt;⎕IO
[1]    ⍝Return OS major, minor, build, platform/dos
[2]    ⎕IO
[3]    rc←0
[4]    'ex'⎕NA{
[5]       ⍵=1:'I kernel32|GetVersionExA ={I4 I4 I4 I4 I4 T[128]}'
[6]       ⍵=2:'I coredll|GetVersionExW ={I4 I4 I4 I4 I4 T[128]}'
[7]    }2 PoW 1
[8]    :If 3=⎕NC'ex'
[9]        str←148 0 0 0 0(128⍴'')
[10]       rc str←ex,⊂str
[11]   :End
[12]   :If rc
[13]       str[4]←16⊥(4⍴16)⊤str[4]
[14]       r←str[2 3 4 5]
[15]       txt←6⊃str
[16]       txt←(+/^\txt≠⎕AV[1])↑txt
[17]       r,←⊂txt
[18]   :Else
[19]       r←0 0 0 0 ''
[20]   :End
[21]   ⍝
[22]   ⍝INFO for 32 bit application (ie not 16bit win 3.x)
[23]   ⍝Win NT3.51      - 3 51  ?     ?
[24]   ⍝Win NT4 (sp6)   - 4  0  1381  2 Service Pack 6
[25]   ⍝Win 95  (OSR2)  - 4  0  1111  1 B
[26]   ⍝                - 4  0  1111  1 C
[27]   ⍝                - 4  0  1212  1 B
[28]   ⍝                - 4  0  1212  1 C
[29]   ⍝Win 98          - 4 10  1998  1
[30]   ⍝Win 98SE        - 4 10  2222  1 A
[31]   ⍝Win ME          - 4 90  3000  1
[32]   ⍝Win 2000 (sp3)  - 5 0   2195  2 Service Pack 3
[33]   ⍝Win XP   (sp1)  - 5 1   2600  2 Service Pack 1
[34]   ⍝Win CE 3.0      - 3 0  11171  3 ''
     ∇
```

So far, I have converted about 40 `⎕NA` calling functions, and have had no difficulty with the APL. That’s not to say that the Pocket PC is a great APL development environment, it’s not. But the problems are with the limited resources available on the Pocket PC, not the APL.

The way I have been working is to modify a function on my desktop PC, and after testing that the code works, saving the workspace. This workspace is then transferred across to the Pocket PC. After loading this workspace on the Pocket PC, I test it again. If necessary, I can fix minor bugs and typos in the APL code, on the Pocket PC, and then transfer the workspace back to my desktop.

Finally, here is the code to return the version number of a DLL/EXE. Be aware that Dyadic only recently introduced support for “VerQueryValue” so running DllVersion on Dyalog.exe will only return a result for version 9.0.3 with a patched date some time after 22nd March 2002. (I do not know exactly when Dyadic introduced it.)

```
     ∇ version←DllVersion file;Aloc;Free;Lock;Ulok;Size;Info;
       Valu;copy;size;hndl;addr;buff;ok
[1]    ⍝ Get version number of DLL
[2]    :If 0 PoW 1
[3]        'Aloc'⎕NA'u kernel32GlobalAlloc u  u'
[4]        'Free'⎕NA'u kernel32GlobalFree u'
[5]        'Lock'⎕NA'u kernel32GlobalLock u'
[6]        'Ulok'⎕NA'u kernel32GlobalUnlock u'
[7]        'Size'⎕NA'u versionGetFileVersionInfoSizeA <0T >u'
[8]        'Info'⎕NA'u versionGetFileVersionInfoA <0T u u u'
[9]        'Valu'⎕NA'u versionVerQueryValueA u <0T >u >u'
[10]       'copy'⎕NA'dyalog32.C32MEMCPY >u[] u u'
[11]   :Else
[12]       'Aloc'⎕NA'u coredllLocalAlloc u  u'
[13]       'Free'⎕NA'u coredllLocalFree u'
[14]       Lock←{⍵}
[15]       Ulok←{1}
[16]       'Size'⎕NA'u coredllGetFileVersionInfoSizeW <0T >u'
[17]       'Info'⎕NA'u coredllGetFileVersionInfoW <0T u u u'
[18]       'Valu'⎕NA'u coredllVerQueryValueW u <0T >u >u'
[19]       'copy'⎕NA'coredllmemcpy >u[] u u'
[20]   :End
[21]   :If ×size←⊃Size file 0                  ⍝ Size of info.
[22]   :AndIf ×hndl←Aloc 0 size                ⍝ Alloc memory.
[23]       :If ×addr←Lock hndl                 ⍝ Lock memory.
[24]           :If ×Info file 0 size addr      ⍝ Version info.
[25]               (ok buff size)←Valu addr'\' 0 0                                                             ⍝ Version value.
[26]               :If ok
[27]                   buff←copy(size÷4)buff size   ⍝ Copy info.
[28]                   version←⊃,/(⊂2/2*16)⊤¨2↑2↓buff                                                          ⍝ Encode version.
[29]               :EndIf
[30]           :EndIf
[31]           ok←Ulok hndl                    ⍝ Unlock memory.
[32]       :EndIf
[33]       ok←Free hndl                        ⍝ Free memory.
[34]   :EndIf     ∇
```

Points to note relating to Pocket APL:

- Dyalog32.DLL was not shipped with the beta version, but since “coredll” contains a both “memcpy” and “strncpy” (note lower case spelling), this is not actually a problem.
- Pocket PC does not support Global memory calls, so these are replaced by Local memory calls.
- Local memory does not and cannot be “locked” so these calls are replaced by simple function calls returning appropriate results.

## Conclusion

Dyalog Pocket APL is NOT a toy, nor a “cut down” version of their desktop product, but a real, state of the art, extremely cheap, full-blown APL, that runs in a PC that literally fits in your pocket. It can handle workspaces 3 times the size of the largest workspace I can use on the mainframe APL systems I still support, and the 256MB of Compact Flash memory I have compares well with the 300 MB of mainframe disk storage I have allocated.

With Pocket APL, the APL character set is NOT an issue, Pocket PCs are Unicode not ASCII based, and with the virtual keyboard, I can see APL characters as I type for the first time in 7 years!

I believe Pocket APL can give the APL community a new lease of life. It gives APL developers a flying start into the Pocket PC market. We can re-sell our existing products in a brand new environment. We can develop new products for a new target audience using our existing tools, with almost no learning curve.

To paraphrase the chancellor of the exchequer at the end of his budget speech:

> “I can commend this APL to the community.”

\[Ed: see also Adrian Smith’s [First Experiences with PocketAPL](cecil192.htm)\]

***
