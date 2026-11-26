---
vid: '10008910'
title: 'Hacker’s Corner: Compressing with GZIP'
prefix: Hacker’s Corner
authors:
- Adrian Smith
byline: Experiments by Stefano, notes by Adrian Smith
volume: '18'
issue: '1'
page: '84'
description: Vector, the journal of the British APL Association
keywords:
- algebra
- APL
- array
- computation
- computing
- J
- K
- mathematics
- programming
- programming language
- programs
- Q
source: trad/v181/hack181.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-2001
tags:
- data and files
- system interfaces
---

Experiments by Stefano, notes by Adrian Smith
{ .byline }

OK, we all have 256Mb of memory, so workspace is infinite and disks are huge. Why would you want to bother taking a perfectly good APL vector and reducing it in size by a factor of 10 or so? Just so you can make it bigger again. Well, the IT world is moving in some very mysterious ways at the moment and just as we are learning to use the infinite power of the desktop, along come SOAP and .NET and suddenly we are expected to create ‘interoperable’ applications with a really slow network separating the components.

What is more, we are supposed to exchange data using a wonderfully redundant scheme called XML (Jonathan Barman’s article in Vector 17.4 is an excellent introduction) which can describe pretty well anything in plain ASCII text but takes huge numbers of extra bytes to do it.

OK, various components along the way attempt to compress things, but if you want to really make this stuff fly, you probably should consider compressing the data yourself as you generate it, and saving things like timeseries and budgets (which tend to be full of zeros) in their compressed state.

## Disk Speed vs CPU Power

The major performance bottleneck on a modern machine (particularly a portable) is usually the disk access time. I was quite surprised to notice on my old P90 Dell laptop that when we compressed the disk to make some space, everything started running faster! Even with a P90 the time taken to uncurl a compressed ‘dyalog.exe’ was so small that it was easily beaten by the saving in time to get the (much smaller) file off the disk. If you are storing big timeseries on component files, then the chances are that the Redstone shares didn’t move much last week and your data will compress really well. Of course you may not want to compress the entire file (like me you probably keep the first few components as documentation) so a couple of easy-to-use functions that compress and decompress vectors might come in handy.

## Maintaining Portability

Sometimes you only need to do the compression – if you are publishing a report in a portable format like Adobe’s PDF you can leave it up to the ‘reader’ application to decompress the data – but only if you have used a compression scheme that it knows about. The same applies if you are trying to share data with (say) a PERL application on a Unix-based web-server. For some time now I have been avoiding APL component files in favour of a simple ‘reverse polish’ array representation which is entirely character-based and can be written to a text file. This was specifically designed to be easy to parse by non-APL applications, but of course it is a little wordy and could also be selectively compressed very nicely. As long as I use a compression technology which is readily available to PERL and Delphi programmers, then I am much better insulated against future change.

## What is this thing called ‘Flate’?

I think all you can say here is that lots of very clever people have been working on recognising redundancy in data and transforming large redundant representations into minimal non-redundant equivalents. The algorithms used in PKZIP are very typical, and the .gz format used by Unix systems is another example. These schemes (and the patented LZW algorithm used in GIF pictures) all give very similar results, and the good thing about the public domain Deflate/Inflate algorithm is that it fractionally outperforms all of them, being an intelligent synthesis of all the best ideas. Basically it looks for repeating patterns in the inbound data (so a vector of random integers will compress rather badly), makes a table of patterns and replaces the pattern by a pointer into the table. Typically the compression algorithm will be quite slow and the decompression lightning fast, which is fine for your average PDF (written once) or dyalog.exe (written once a week) or a corporate budget (written once a year) but is not so sensible for a market tracker that is noting prices every few minutes.

### So how do I make it work?

Basically you need a single DLL (freely redistributable) and a bunch of functions to call it. A copy of the C++ header file is rather necessary to work out how the calls should be built, and what all the defined constants are. You can find the DLL and the Dyalog workspace which covers it in the resources section of the Vector web site as a 98K [zip file](resource/zlib.zip). All the code is by Stefano – I have added a few comments as notes to myself as I was reading through it.

Firstly – copy zlib.dll somewhere Dyalog can find it (mine is in /winnt/system32) and load the supplied workspace. This has a couple of examples and an Abstract variable in root, and a ZLIB namespace to do the serious work. The kind of thing it can do is nicely illustrated by:

```
      qq←32768 ZLIB.DeflateIV 2345⍴⍳5
      ⍴qq
31
      qqq←32768 ZLIB.InflateIV qq
      ⍴qqq
2345
      qqq≡2345⍴⍳5
1
```

Obviously this is a very artificial example, but it is surprising how often planning and scheduling systems throw up huge arrays of mostly zeros….

```
      pmat←234 106⍴0  ⍝ 2 years planning for 234 products
      pmat[18;5 6 7 67 68 69 70]←12
      pmat[28;98 99]←14
      qq←32768 ZLIB.DeflateIV ,pmat
      ⍴qq
64
       qqq←32768 ZLIB.InflateIV qq
       ⍴qqq
24804
      pmat≡234 106⍴qqq
1
```

Well, it may be a one-trick pony but it plays this trick pretty well. Let’s see if the buffer size has any noticeable effect on a big array:

```
       tt←⎕AI[2] ⋄ qq←1024 ZLIB.DeflateIV ,pmat ⋄ ⎕AI[2]-tt
4166
       tt←⎕AI[2] ⋄ qq←4096 ZLIB.DeflateIV ,pmat ⋄ ⎕AI[2]-tt
1161
       tt←⎕AI[2] ⋄ qq←32768 ZLIB.DeflateIV ,pmat ⋄ ⎕AI[2]-tt
331
       tt←⎕AI[2] ⋄ qq←(2*20) ZLIB.DeflateIV ,pmat ⋄ ⎕AI[2]-tt
201
    ⍴,pmat
641680
      2*20
1048576
```

Basically, we are timing the speed of the APL interpreter, and the catenation to the intermediate result vector. I would be inclined to round up the buffer size to the nearest 1k block above the data size and take it all in one lump always!

Just for interest, how do these times compare with the reconstruction phase:

```
       tt←⎕AI[2] ⋄ qqq←1024 ZLIB.InflateIV qq ⋄ ⎕AI[2]-tt
2965
       tt←⎕AI[2] ⋄ qqq←4096 ZLIB.InflateIV qq ⋄ ⎕AI[2]-tt
932
       tt←⎕AI[2] ⋄ qqq←32768 ZLIB.InflateIV qq ⋄ ⎕AI[2]-tt
291
```

So you can begin to judge the trade-off. Round about 300ms to reduce rather over a megabyte of integers to 64 bytes, but of course we need to keep note of the shape of the original array somewhere. Something which would be very nice here would be for Dyalog to implement `⎕OR 'qq'` such that it returned the internal representation of any array as a vector of bytes. Then we could compress anything and (with something equivalent to reconstruct the array from its `⎕OR` form) put it back again. The only way I know to do this is to connect back-to-back TCP sockets, set one to ‘APL’ and the other to ‘RAW’ and throw the array around the loop! I seem to remember that something very like this made it into +Win 3.6, so maybe someone can follow this up and convert the internal calls appropriately.

## Let’s Have a Look at the Code

```
     ∇ Init;z;strncpy;DLL;ZSTREAM
[1]   ⍝ Set up basic DLL calls and find out the version info
[2]    DLL←'zlib.dll.C32'
[3]    ZSTREAM←'{I I I I I I I I I I I I I I}'
[4]   ⍝ We will need the version to pass to the other calls!
[5]    ⎕NA'I4 ',DLL,'|zlibVersion'
[6]    'strncpy'⎕NA'dyalog32|STRNCPY >0T I4 U4'
[7]    VERSION←strncpy 256 zlibVersion 256  ⍝ VERSION←'1.1.3'
[8]
[9]    ⎕NA'I ',DLL,'|deflateInit_ =',ZSTREAM,' I <0T I'
[10]   ⎕NA'I ',DLL,'|deflate =',ZSTREAM,' I'
[11]   ⎕NA'I ',DLL,'|deflateEnd =',ZSTREAM
[12]
[13]   ⎕NA'I ',DLL,'|inflateInit_ =',ZSTREAM,' <0T I'
[14]   ⎕NA'I ',DLL,'|inflate =',ZSTREAM,' I'
[15]   ⎕NA'I ',DLL,'|inflateEnd =',ZSTREAM
[16]
[17]  ⍝ Standard utilities to reserve/free memory
[18]   ⎕NA'I4 kernel32|GlobalAlloc U4 U4'
[19]   ⎕NA'I4 kernel32|GlobalLock I4'
[20]   ⎕NA'U4 kernel32|GlobalFree U4'
[21]
[22]  ⍝ Use the Dyalog32 DLL to move stuff to/from reserved memory
[23]  ⍝ MEMCPY(destination-address, source-address, number-of-bytes).
[24]   'put'⎕NA'dyalog32|MEMCPY I4 <I1[] U4'
[25]   'get'⎕NA'dyalog32|MEMCPY >I1[] I4 U4'
     ∇
```

The outer layer…

```
     ∇ r←bufs DeflateIV data;handle;availIn;stream;out;status
[1]   ⍝ Deflate an integer vector in units of <bufs>
[2]   ⍝ Moves data into memory buffer and compresses it
[3]
[4]    handle←DeflateInit Z_DEFAULT_COMPRESSION bufs bufs
[5]    availIn←0
[6]    r←⍬
[7]
[8]    :While 1
[9]      :If 0=availIn
[10]       stream←(bufs⌊⍴data)↑data
[11]       :If 0=⍴stream
[12]         :Leave
[13]       :EndIf
[14]       data←(⍴stream)↓data
[15]     :EndIf
[16]     (status availIn out)←handle Deflate((0=availIn)/stream)Z_NO_FLUSH
[17]     'Invalid stream'⎕SIGNAL(~status∊Z_STREAM_END Z_OK)⍴11
[18]     r,←out
[19]   :EndWhile
[20]
[21]   :While Z_STREAM_END≠status
[22]     (status availIn out)←handle Deflate ⍬ Z_FINISH
[23]     r,←out
[24]   :EndWhile
[25]
[26]   status←DeflateEnd handle
     ∇
```

Basically, this sets up a ‘session’ by creating a temporary namespace and initialising the DLL with an appropriate ‘zstream’ which is a deceptively simply array of values and flags. It then uses the session progressively to perform the deflation and cleans up at the end.

Have a look at the workspace on the website for the low-level calls – these do some nice stuff with defined operators to keep the interface clean. Improvements and enhancements welcome – I will probably be using this for image data in PDF files (these are excellent subjects for compression, as are NewLeaf tables) so look out for more on the same theme in the future.
