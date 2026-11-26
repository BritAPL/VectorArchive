---
vid: '10003340'
title: Standardisation Beyond the Language
authors:
- Martin Gfeller
- Morten Kromberg
byline: by Martin Gfeller and Morten Kromberg
volume: '10'
issue: '4'
page: '58'
source: trad/v104/gfeller104_58.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- standards
- implementations
---

by Martin Gfeller and Morten Kromberg <span class="affil"></span> <span class="email"></span>
{ .byline }

### Introduction

As APL professionals working with several APL and non-APL systems, we have become increasingly frustrated by the lack of standardization in some ancillary areas of the APL language and system. This lack of standardization leads to duplication of human effort and computer resources, sometimes to the point where more standard non-APL solutions become more feasible, despite APL being inherently easier for the task.

Standardization should increase the portability and inter-operability between different APLs, and between APL and non-APL software. The areas to standardize concern language pragmatics, rather than language fundamentals. Hence, they should be easier to standardize, since fewer design beliefs are involved.

While APL should adopt common industry standards established in its environment, there are two broad APL-specific areas that need special consideration: character set issues and array representation.

### Character Set Issues

APL’s use of special glyphs is an asset, but carries quite a high cost. Standardization attempts to minimize this cost.

### Character Set Encoding

Different APL products use different ways of superimposing the APL character set onto the operating system’s character set. A standard mapping would allow easy exchange of APL text files and fonts, as well as easier mixing and matching of language systems with session managers and user interfaces.

Three distinct mapping schemes are needed, according to the width of the underlying system’s character representation. The most important character sets of different width are 7-bit ASCII, 8-bit ASCII, and Unicode.

- 7-bit ASCII. 128 character positions cannot accommodate alphabets, numerals, punctuation, control characters, and the APL glyphs. APL uses overstriking to solve the problem, and represents the second alphabet by overstriking the first with the underbar character. It also discards some ASCII characters not used in APL. Fortunately, this encoding has been standardized, and hence most APL terminals or terminal emulators are usable with most ASCII based APL systems. The industry standard is called APL-ASCII Typewriter-Pairing \[2, in annex\].
- 8-bit ASCII. With 256 characters, all APL glyphs could be represented, in addition to all standard 7-bit ASCII characters. However, 8-bit ASCII was not invented for the benefit of APL; and most of the 128 additional positions have been used for additional European language glyphs, and for various useful symbols (such as smiling faces).
- APL vendors superimposed their character set on 8-bit ASCII by removing some ASCII glyphs. Some vendors occupied parts of the first 32 positions, which are defined to control devices. While this is acceptable on PCs, it makes handling of asynchronous devices difficult. Even more severely, most vendors implemented a different mapping scheme. This makes the transfer of APL text and the exchange of tools very difficult. For example, each APL product generally requires its own screen and printer font (plus different fonts for each printer type).
- Unicode. Multi-byte schemes such as Unicode (UCS-2 in ISO 10646) with more than 65000 positions allow the representation of all APL glyphs, including overstrikes used in any system, without affecting other character positions. The Unicode standard \[4\] contains all the APL characters, but products must become available to support it. An implementation for APL2 is proposed by \[1\], which also discusses many considerations and trade-offs related to these issues. Basically, the 8-bit ASCII character set should be a defined subsequence of Unicode, including all ASCII characters and APL glyphs. Conversion between 8-bit (short) characters and 16-bit (Unicode) characters must be handled automatically by the interpreters.

### Action Points

A standard for APL representation in 8-bit ASCII is most urgently needed. Vendors should incorporate the standard, and provide migration tools toward it. The standard should be defined by designating a subsequence of Unicode with 256 distinct elements.

APL products implementing Unicode are required for Unicode to unleash its potential benefit to APL.

### Keyboard

The variety of APL keyboards is bewildering. Some differences stem from the requirements of the underlying operating system (such as Windows reserving the use of the Alt-key to invoke menus), but some are plainly unnecessary.

The keyboard issue is largely independent from the character set encoding, but is complicated by different national keyboard layouts. We suggest that standardization should begin with the US keyboard, and national APL user groups should transform the US APL keyboard to their national layout. To allow this, the APL keyboard handlers must be flexible enough to handle problems like “dead keys”; table driven finite state translators fit this requirement.

The primary requirement for APL keyboard design is to leave the ASCII keyboard undisturbed. This means that all keys issue their standard glyphs, and APL glyphs are only generated with otherwise unassigned key states (Ctrl, Shift+Ctrl under Windows). Since all keys retain their default behaviour in the unmodified and shifted state, the alphabetics map to minuscule and majuscules. Since APL names should be enterable easily, without using Caps-Lock, this leads naturally to the union character set convention \[3\].

### Action Points

Design a standard US ASCII APL keyboard that works with Windows. Let national APL user groups adapt the keyboard to the layouts of their countries.

### Human-readable Keywords for ASCII

There will always be devices that cannot be used to enter or display APL glyphs. While they might not be used for extensive APL programming, there is no reason for not making them available for APL use. The most common solution for this problem is keywords, but again, there is a large variety of keyword schemes, and none of them is popular across APL implementations.

Keyword schemes should be designed for human readability. They do not need to mimic the terseness of symbols, because the result is often cryptic. They should be easy to remember or discover, because many users will resort to them infrequently, when no APL device is available. Longer words can be more descriptive and may be easier to remember; on input, they could be abbreviated as long as they remain unique.

Keywords should map one-to-one to glyphs; adoption of different keywords for monadic and dyadic use involves syntax analysis and makes the transliteration too complex.

### Action Points

Standardize on a simple, readable keyword scheme, and adopt it in all language implementations.

### Array and Code Representation

APL workspaces, functions, and arrays are more structured than objects in many other languages in that they cannot be represented as simple text files.

Workspaces need a static representation for transfers between systems, whereas arrays need a representation for dynamic exchange between processes.

### WSIS Transfer Standard

A Workspace Interchange Standard (WSIS1) exists as an annex of the ISO Standard APL \[ISO 86\]. It does not extend to derived functions and user defined operators (nor to APL2 or SHARP APL packages), and it is not widely supported.

An extended WSIS should be defined, encompassing at least nested arrays and user-defined operators. Precise rules on how the various dialects should import border-case nested arrays (transferring enclosed scalars from a grounded to a floating system) should be defined.

The WSIS standard should be based on the standardized 8-bit ASCII character set encoding, so a complex translate table mechanism will not be necessary (since the representation is static, it can always be passed through suitable reversible utilities such as uuencode to convert to a 7-bit or any other desired encoding).

### Action Points

Create an updated WSIS. All vendors should support it. A test workspace in WSIS should be provided for vendor conformance testing.

### DDE/OLE Interchange Formats

DDE (Dynamic Data Exchange) and OLE 2.0 (Object Linking and Embedding) are Windows standards for inter-application communication. Other standards exist or are emerging in other operating system frameworks, but many representation issues are common amongst them. (XDR is an important representation description standard in the UNIX environment; it is used to describe RPC parameters. OMG’s CORBA IDL is a new standard to describe objects.)

There are DDE and OLE format conventions for common structures such as bit maps, text strings and spreadsheet cells, but not for multi-dimensional nested arrays. Such a format would be useful for communication between APLs of different vendors, between APL and ancillary services (e.g. session managers, network shared variable processors, or array editors), or between APL and other applications using array structures.

There are two kinds of formats, a native format and a presentation format. The presentation format is a text representation of the information, and is equivalent to APL’s display form. The native format is useful for further internal processing, and can be used to exchange arrays between processes that understand their structure.

Both formats should be standardized. The descriptor part of the format (the “header”) must not make use of APL glyphs. The array contents may be character data based on standard 8-bit ASCII or Unicode, as indicated by a flag in the descriptor.

### Action Points

APL array DDE/OLE native and presentation formats should be defined. APL implementations under Windows should make them available through their respective interprocess communication mechanisms (Shared Variables, native function calls).

### Foreign Function Descriptor Format

Many APL interpreters are able to call functions written in another language. Often, this facility is implemented by an APL system function (`⎕na`) that associates an APL function with a description of a foreign function. However, the format used to describe the foreign function (its signature; e.g. parameters and their types) varies between dialects. The descriptor format should be standardized, and a format that can be automatically generated from C and C++ header files should be preferred.

Foreign functions may need to access APL workspace structures; however, this is an area that realistically cannot be standardized due to the different internal formats of workspaces for different interpreters and architectures. Portable applications should instead use the inter-application communication methods described above to access data in foreign APL workspaces.

### Action Points

Create a standard descriptor format for foreign functions. Build tools to transform C and C++ header files into this format.

Martin Gfeller  
Reuters SA  
Risk Management Software  
Kleinstrasse 6  
CH-8008 Zürich  
Switzerland  
  
Morten Kromberg  
Insight Systems ApS  
Nordre Strandvej 119a  
DK-3150 Hellebæk  
Denmark

### References

1. J. Brown et al. “Extending the APL Character Set”, APL 93 Conference Proceedings, p. 41, Toronto, August 1993.
2. IS8485, “Programming Language APL”, International Standards Organization, Geneva, 1986
3. Martin Gfeller, “Adding Lowercase to APL”, APL News, Springer Verlag, June 1986.
4. The Unicode Consortium, “The Unicode Standard”, Worldwide Character Encoding, Version 1.0, Vol. 1, Addison-Wesley, Menlo Park, CA, October 1991.

(webpage generated: 28 March 2006, 06:48)
