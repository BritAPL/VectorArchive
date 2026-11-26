---
vid: '10006300'
title: 'Hacker’s Corner: How Big is that Doggie in the Window?'
authors:
- William Parke
byline: original from William Parke (wrp@parkenet.org), notes by Adrian Smith
volume: '17'
issue: '2'
page: '108'
source: trad/v172/hack172.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2000
tags:
- graphics
- system interfaces
- programming techniques
---

*original from William Parke (wrp@parkenet.org), notes by Adrian Smith*
{ .byline }

## Introduction

Being kind to web browsers is always a good idea, but particularly so when it comes to images. For readers who have been asleep for the last 5 years, you include a picture in a web page with an HTML fragment like:

```
<img src="mydog.jpg">This is my dog!
```

... and when the browser hits the tag, it beetles off back to the server and asks for the image file. In fact if you have lots of pictures, sensible browsers start a whole series of threads running to get the pictures in parallel. Of course the whole of the text has usually been loaded long before the last image arrives, which leaves the browser with a big problem – how can it lay out the page until it knows how big all the images are?

There are two possible solutions here:

1. wait for the last picture to come, compute the wraparounds and gaps, then lay out the text
2. slam the text down as soon as you can and rewrite it as many times as you have to, as information about the pictures trickles in.

Option 1 is good if you have ADSL or are working on a LAN, option 2 is actually not as bad as it sounds on a slow modem. However, you can *tell* the browser exactly what to expect by including some extra information in the tag:

```
<img src="mydog.jpg" height=240 width=128>This is my dog!
```

Now the page can be rendered correctly and immediately, and the pictures will slot neatly into the gaps. Everyone deeply content, all pigs fed and ready to fly etc. But .... how does your APL web-site builder find out how big a picture is so it can include the information in the HTML stream that it writes out?

This was an issue I had been quietly ignoring for some time, as my HelpStuf workspace only does Webpages as a by-product, being much more focussed on Help files and manuals. However it is rapidly becoming essential to do good web-based documentation too, so I was delighted when Bill Parke took up the challenge, took on the research and came up with a serious piece of APL+Win code which will find the size of more or less any file you care to name.

## SizeofIMG in APL+Win

Bill keeps up-to-date copies of his code on his own website, and the original is a shade over 300 lines long, so it would be pointless to list it here. However the references are (very sensibly) included in the leading comments:

```
⍝∇([fileformat width height depth hres vres units] or [attributestring])←[returnformat] SizeofIMG filename -- find size information in BMP, JPG, GIF and PNG image files
⍝∇ver 2000.06.09 wrp <wrp@parkenet.org>
⍝∇ file ←→ [path\]imagefilename
⍝∇ rf   ←→ 0 = return raw data (default)
⍝∇         1 = return string formatted for HTML <IMG> tag
⍝∇ z    ←→ raw data: fileformat width height depth Hresolution Vresolution resolutionunits
⍝∇      or format string: 'width="xxx" height="yyy"'

⍝∇ JPEG information adapted from:
⍝∇  the Independent JPEG Group's JPEG software, release 6b
⍝∇  Copyright (C) 1991-1998, Thomas G. Lane.
⍝∇   specifically jdmarker.c
⍝∇  ftp.uu.net/graphics/jpeg/jpegsrc.v6b.tar.gz
⍝∇  ftp.simtel.net/pub/simtelnet/msdos/graphics/jpegsr6b.zip
⍝∇
⍝∇ BMP, GIF and PNG information from:
⍝∇  Miano, John. "Compressed Image File Formats"
⍝∇  Copyright (C) 1999, ACM Press (Addison-Wesley)
⍝∇  ISBN 0201604434
```

He returns the size information, and quite a lot of other interesting stuff about the file. Calling it is as simple as it looks:

```
     1 SizeOf 'd:\digipix\cars\mazda.jpg'
width=1852 height=1239
```

Here is my translation into Dyalog APL, much reduced in size as I only needed the file-size information and could ignore bitmap format as these are never used in my webpages. I have cut the leading comments, which just repeat Bill’s notes.

```
     ∇ size←SizeOf file;fileformat;units;height;width;nsize;ntie;
            byte;nread;length;data;⎕IO
[1]   ⍝ Get size info (GIF,JPG,PNG) and return in HTML format
 ...........
[14]  ⍝ Translated from Bill Parke's APL+Win code by ACDS - July 2000
[15]  ⍝ See http://parkenet.org/wrp/apl/SizeofIMG.html for his notes
[16]
[17]   size←'' ⋄ ⎕IO←0
[18]
[19]  ⍝ Initialize data variables
[20]   fileformat←units←'' ⋄ height←width←0 ⋄ ntie←¯1+⌊/0,⎕NNUMS
[21]
[22]   :Trap 0 ⍝ Open file with error trapping
[23]     file ⎕NTIE ntie 0
[24]   :Else
[25]     Log'Could not open ',file,' no size information will be included'
[26]     :Return
[27]   :End
[28]
[29]   nsize←⎕NSIZE ntie
[30]
[31]   :If 255 216≡256|⎕NREAD ntie,83,(nread←2),0   ⍝ is JPEG file
[32]     fileformat←'JPEG'
[33]     :While nread<nsize   ⍝ read byte-by-byte, look for FF (255) markers
[34]       :Repeat    ⍝ read until marker found or end of file
[35]         byte←256|⎕NREAD ntie,83 1
[36]         →(nsize=nread←1+nread)↑END
[37]       :Until 255=byte  ⍝ marker found, look for marker code
[38]
[39]       :Repeat  ⍝ read marker code, skip any FF padding
[40]         byte←256|⎕NREAD ntie,83 1
[41]         →(nsize=nread←1+nread)↑END
[42]       :Until 255≠byte  ⍝ length includes itself, so subtract here
[43]
[44]       length←¯2+256 256⊥256|⎕NREAD ntie,83 2 ⋄ nread←2+nread
[45]
[46]       :Select ⊃byte
[47]       :CaseList (192+⍳16)~196 200 204
[48]         data←256|⎕NREAD ntie,83,length
[49]         nread←nread+length
[50]        ⍝ image size is contained in next 4 bytes
[51]         height←256 256⊥data[1 2] ⋄ width←256 256⊥data[3 4]
[52]         →END
[53]
[54]       :Else   ⍝ not interested -- skip this parameter segment
[55]         :If 0<length
[56]           byte←⎕NREAD ntie,83,length
[57]           nread←nread+length
[58]         :End
[59]
[60]       :EndSelect
[61]
[62]     :EndWhile    ⍝ -- end JPEG section -------------------
[63]
[64]   :ElseIf 'GIF'≡⎕NREAD ntie,82 3 0
[65]     fileformat←'GIF',⎕NREAD ntie,82 3   ⍝ GIF subtype ('87a' or '89a')
[66]     (width height)←⎕NREAD ntie,163 2
[67]
[68]   ⍝ -- end GIF section ----------------------------------------
[69]
[70]   :ElseIf 137 80 78 71 13 10 26 10≡256|⎕NREAD ntie,83 8 0
[71]     length←323 ⎕DR⌽⎕NREAD ntie,82 4
[72]     :If (13=length)^'IHDR'≡⎕NREAD ntie,82 4
[73]       fileformat←'PNG'
[74]       width←323 ⎕DR⌽⎕NREAD ntie,82 4
[75]       height←323 ⎕DR⌽⎕NREAD ntie,82 4
[76]     :End
[77]   ⍝ -- end PNG section ----------------------------------------
[78]
[79]   :End
[80]
[81]  END:
[82]
[83]   :If ^/0<width,height  ⍝ only supply valid values
[84]     size←' width=',(⍕width),' height=',(⍕height)
[85]   :End
[86]
[87]   ⎕NUNTIE ntie
     ∇
```

Notice the trailing 0 on the call to `⎕NTIE` on line\[23\] – this is something I almost always forget (including the above function until I pasted it into Vector 5 minutes ago) and it is essential if you are accessing a read-only network drive. Notice the effect of correcting the problem ...

```
      SizeOf '\\della\d\pix\berlin\bbgate.jpg'
Could not open \\della\d\pix\berlin\bbgate.jpg no size information will be included

      SizeOf '\\della\d\pix\berlin\bbgate.jpg'
 width=523 height=360
```

I think APL+Win defaults to opening the file read-only (option=0) but Dyalog defaults to read-write mode which is a very silly idea if you keep your pictures on a network drive, a CD ROM or a read-only ZIP drive.

## Summary

If you are using APL to build web-pages, go and get Bill’s code and just use it. If you really want to know about the details, follow up his excellent references. Either way, he has made a really useful contribution to the corpus of publicly available APL code, and deserves all our thanks.

***
