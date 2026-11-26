---
vid: '10008810'
title: 'Meeting: Eric Iverson at the BAA'
authors:
- Adrian Smith
volume: '15'
issue: '2'
page: '49'
description: 'VECTOR: The Journal of the British APL Association'
keywords:
- APL
- Array Languages
- Array Processing
- Rapid Development
source: trad/v152/eric.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1998
tags:
- conference reports
- APL community
- data and files
---

*reported by Adrian Smith*

## Introduction

The BAA would like to thank Eric for taking the trouble to break his journey back from APL98 to talk to us in London. In spite of the best efforts of *Computing* magazine, which mistook J for Java and triggered several phone calls from intrigued Java hacks, it was not a well- attended meeting, with only 30 or so able to make the trip.

Those who came enjoyed a splendid talk, with some entertainment – J running on a palmtop under Windows CE – and some solid education in the inner workings of virtual-memory – disguised as a demo of J and memory-mapped files.

The subject of memory-mapped files is an important one for the future of both J and APL, and will be treated in much more depth in the next *Vector*. For now, I will just focus on what Eric showed us on the day.

## How J is Marketed and Sold

Eric showed how ISI’s marketing strategy is now aimed at the Maths, Physics, Chemistry fraternity, with J being promoted as:

1. an array language (of course!)
2. a maths language, in direct competition with MATLAB etc. They get asked questions like “How does it do derivatives?”.
3. a “permissive” language (unlike Java) where people can solve problems in their own way, and 10 programmers are likely to return with 30 different answers when given a problem to solve.
4. a “functional” programming language that’s only as pure as you want it to be.
5. a real “object oriented” language, but you don’t have to restrict your ideas to this.
6. a very high-performance language (vs MATHEMATICA etc.) when operating on large amounts of data.
7. a possible component in a hybrid application.

ISI are working on selling $6,000-$10,000 site agreements for “all the J they can eat” as the basis for their expansion. The CD release has all the web-site content and all back versions of J (an installation only takes 60Mb) which is also a really useful distributed archive for ISI!

## J on the Palmtop (under Windows CE)

This is a complete (except for OpenGL which is not available on CE) implementation of J “in your hand”. It has met with an overwhelming response at maths and science conferences, where the portability is very attractive. You can program on the train – particularly handy if you need to hide the fact that you are writing programs!

One useful bonus for ISI is that “Pocket Excel” has no graphics capability, so J can be sold quite effectively as a simple graphing add-in for spreadsheets.

Eric was extremely enthusiastic about the machine – all the 16Mb of RAM is for you, as Windows is in ROM – and also about the CE operating system which he noted is extremely stable, very tightly coded and generally feels better engineered than anything else that has recently come out of Microsoft. Personally speaking, I would not be at all surprised to see it sweep the home market over the next few years – the benefits of a 1-sec boot-up time and the impossibility of damaging it will surely give it a massive advantage over NT when Windows 98 comes to the end of the road.

## *Lapack* – for Linear Algebra

We used to think APL was the best at this game, but one consequence of visiting the mathematics circuit was that Eric became aware that there are lots of very competent (and specialised) packages out there which embody some very much better code than we can produce. If you want eigenvalues of a 100 x 100 matrix on your palm-top, you need *Lapack*, directly interfaced with J.

## Memory-mapped Files

This is one of the most sigificant things which has happened to J in the business and finance community. The surprising thing – which was noticed by A+ and K a long time ago – is how simple it is. As Eric pointed out, it is not new stuff! All the hooks were there in NT 3.5, so why have C++ and VB been so long in catching on to the possibilities?

The important thing to realise is that under NT and Windows95, all memory is actually mapped to disk all the time.

When a process starts it is has a 4GB address space. It can address bytes from 0 up to 2^32, each process has its own address space and initially all addresses are invalid. An address space byte is ONLY given a value by mapping it to a byte in a file - every byte value in an address space comes from a byte in a file.

<b>Physical memory</b>

On-chip cache memory, off-chip cache memory, and real memory are all simply performance mechanisms to connect an address space byte with its mapped byte in a file. *Real memory is just another level of cache!* It has little to do with the underlying architecture of mapping address space bytes to file bytes.

<b>Mapping a file – 3 steps</b>

1. Create a file object (open a file).
2. Create a mapping object from the file object that describes which bytes in the file are to be used (offset and length) and how they can be accessed (readwrite, readonly, etc.).
3. Create a view from the mapping object. The view connects the bytes in the file described by the mapping object with an address in the address space.

**J memory mapped files**

The J programmer has complete control over mapping files into the J address space. The J programmer can create a J array header that describes the memory of a mapped file and access the file data directly as a J noun.

Open and load mmf utilities.

```
   open 'user\mmf.ijs'
   load 'user\mmf.ijs'
   load 'files'
```

Set a variable that will be used as the name of a file:

```
   fn=: 'c:\t.mmf'
   (50$'abcdef') fwrite fn
50
```

Steps to map file bytes to a memory address:

1. openfile calls Win32 Createfile to create the file object

    ```
    a=:fn;GENERIC_READWRITE;0;OPEN_EXISTING
       [fh=: openfile a
    8
    ```

2. mapfile calls CreateFileMapping to create a mapping object. By default all bytes in the file are mapped.

    ```
       [mh=: mapfile
    fh;PAGE_READWRITE
    9
    ```

3. viewfile calls MapViewOfFile to map the file bytes represented by the mapping handle into the address space. The result is the address of the file bytes in the address space.

    ```
       [fad=: viewfile
    mh;FILE_MAP_WRITE
    _2100400128
       memr fad,0,50
    abcdefabcdefabcdefabcdefabcdefa...
    ```

Release resources:

```
   UnmapViewOfFile fad
_2122891228
   CloseHandle mh
1
   CloseHandle fh
```

### Mapping the file to a J noun

`xmap` maps a file, creates a J array header that describes it, and creates a symbol table entry to address the header. The left arg is the name of the noun. The right arg is the file, readonly flag, trailing shape, and type.

<pre>   'abc' xmap 
fn;0;5;JCHAR
+---------+--------+-----------+--------+-+-+
|abc_base_|c:\t.mmf|_2100400128|26736252|8|9|
+---------+--------+-----------+--------+-+-+
<p>   $abc
10 5
   abc
abcde
fabcd
efabc
defab
cdefa
bcdef
abcde
fabcd
efabc
defab</p>
<p>   abc=: 'zxcvb' 1} abc
   abc
abcde
zxcvb
efabc
defab
cdefa
bcdef
abcde
fabcd
efabc
defab
</p>
<p>   unmap &lt;'abc'
0
   abc
|value error: abc
|[-1]
   fread fn
abcdezxcvbefabcdefabcdefabcdef... 
</p></pre>

Eric pointed out that this implements ‘persistent data’ remarkably easily. It also gives you access to really huge datasets which you could never hope to handle in the workspace. He showed an example with a CD of 650Mb of trading data from the New York stock exchange. With these mapped as J nouns ( ... pause for the CD ROM to get up to speed) you now apparently have 7 x 100Mb numeric arrays to manipulate:

```
    NB. map 7 files (600MB) to 7 nouns
   nysemapall''
   $ctidx
199602 22
   $ctbin
11332740 22
   */$ctidx
4391244
   */$ctbin
249320280
```

Of course you need to be careful what you do, but being able to run a grid control on 250Mb of data sure beats Excel!

So – what does it do for you? Well it obviously moves the boundary of the direct array-processing language much further out. You can map parts of files to different nouns, maybe in different processes, which is just like shared variables, except that this really is the *same* array.

The key difference from the Dyalog `quad-XT` implementation is in the separation of the array data – on the CD – from the array header – in the workspace. The data behaves as a real variable, but with certain restrictions, for example assignment is only possible ‘in place’, although catenation may be possible.

## Summary

Seeing this from the point of view of an APL programmer, I would say that the most important lesson was in the need to break out of the workspace and have limited, but convenient, access to very high volumes of data without having to run with enormous workspace sizes.

J already has a clear advantage here, in that it uses the standard Windows mechanisms to allocate and release memory. If APL can hook into some of these ideas on access to external data, it will greatly streamline many existing and future applications.

The potential of the CE machines was also brought home to many of the audience for the first time. Before long we will have access to this kind of computing power on something not much bigger than the £4.95 calculator you can buy at Dixons. The very cryptic nature of APL and J (remember APL was developed on 10cps typewriter terminals) is an important selling point when you are trying to do maths on a small keyboard with limited display area! Watch this space!

*\[Editor’s note: some details in the implementation of the supplied scripts for memory-mapped files are slightly different in the production release, which is now available.\]*

***
