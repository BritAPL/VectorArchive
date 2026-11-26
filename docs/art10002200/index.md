---
vid: '10002200'
title: APL Unicode Font – extended
authors:
- Phil Chastney
byline: by Phil Chastney (philip_chastney@yahoo.com)
volume: '17'
issue: '3'
page: '139'
source: trad/v173/simp2.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2001
tags:
- standards
- graphics
---

*by Phil Chastney ([philip\_chastney@yahoo.com](mailto:philip_chastney@yahoo.com))*
{ .byline }

## A New Release

A new version of SimPL.ttf is now available from the Vector website.  
*\[Ed: Phil has sent an even newer version as of 29th January – about 202K [download](simplttf.zip).\]*

The original [article](trad/v161/phil161.htm) appeared in Vector 16.1.

## What’s Changed

The objective remains the same: to provide the full set of APL glyphs, plus enough characters to display text in all modern European languages, and anything else that looks like it might be useful. The principal differences from the earlier versions of the font are those required to bring it in line with Unicode version 3.0, plus some of the elements of the proposed extensions to Unicode version 3.2. First among these is that the APL Quad now has its own codepoint at U+2395, which makes uniformity of appearance much easier to achieve in a large font.

The domain names \[these are shown here in bold, but are actually “fancy” characters from the Unicode set. <i>Ed</i>\] **H, N, Z, Q, R, C and P** are already part of the Unicode standard (although **H** and **P** are not given any semantics), so it seems sensible to continue to use double-struck characters for domain names. A double-struck zero, **Ø**, is such an obvious choice for the empty domain, that it has been retained but moved from the “empty set” codepoint (U+2205) to the Private Use Area, where it keeps company with  **2, D, F, S, U** and some other characters whose semantics we have yet to define.

The move is necessary because the Unicode Consortium have mandated (or, more exactly, appear to be about to mandate) that the empty set shall be represented by the circle-slash. This is a difficult area for Unicode, whose stated objective is to concentrate on semantics, not appearance. In practice, mathematic symbology works the other way, appropriating whatever symbols are available: attaching semantics to these symbols is a second, separate, stage. Maybe the Unicode consortium should just define the appearance, and leave the rest of us to interpret them as we choose? (Also, what do they mean by “slash”? Most slashes are inclined at an angle of about 24º, whereas every “empty set” symbol I have seen uses a diagonal slash, at an angle of 45º. And if I think the result looks ugly, and totally out of sympathy with the surrounding notation, that is best dismissed as just a prejudiced personal reaction.)

## What’s New

Glyphs (some better than others) are provided for all the Latin and Latin-based characters in Unicode 3.0 – that is the whole of Latin 1, Latin Extended A, Latin Extended B, and Latin Extended Additional. The resultant character set covers not only the modern European languages, as per our objective, but also covers many African, North American and Asian languages as well.

- Bearing in mind that this is intended as a programmer’s font, an attempt has been made to better distinguish upper-case letter “O” from the digit zero, and lower-case letter “l” from the digit “1”.
- The double accents used in Vietnamese are not well done, particularly the circumflex-tilde combination on uppercase letters. This is the result of a wrong decision early in the design stage, and backing out of that mistake is going to be a long job, so the font is being released *as is*, warts and all.
- The presence of the long-tailed “r” and the addition of the Tironian Et, mean that most recent renderings of Irish are now possible, provided you can accept the “gaelic g” as simply an alternative glyph for the lower-case Latin letter “g”.
- The Latin character set may not yet be complete: I believe proposals for Sami<sup><a href="#note1">1</a></sup> are still outstanding, and these will be included in the font when they have been included in the Unicode standard.
- The characters necessary for Romanised Sanskrit and Pin-Yin Chinese are now all available within Unicode 3.0, and so some of the stuff previously in the Private Use Area has been shifted out.

There are a few additional characters in the Greek area – nothing major.

Coverage of Cyrillic is much improved. Coverage of Church Slavonic is incomplete, but (I believe that) all the glyphs currently in non-liturgical use are provided, giving 100% coverage of those languages formerly known as “Soviet minority languages”, with the exception of Komi<sup><a href="#note2">2</a></sup>.

Unicode never promised to include pre-composed forms for all accented characters, so that they cannot really be criticised for most of the omissions listed<sup><a href="#note3">3</a></sup> at

[http://www.eki.ee/letter/chardata.cgi?ucode=e000-f8ff](http://www.eki.ee/letter/chardata.cgi?ucode=e000-f8ff)

but some of these characters cannot be formed from the existing repertoire of base characters and non-spacing modifiers, so I have included the lot. This should give complete coverage for Tagalog<sup><a href="#note4">4</a></sup> and mediaeval Luxemburgish, for those who need it. If not, let me know what is missing.

The Unicode 3.2 proposals include extended (broken, multi-line) parentheses, brackets and braces in one of the Mathematical Symbols areas. Their proposals for multi-line braces look unworkable, so that the characters previously included in SImPL.ttf remain in the Private Use Area. Whether these characters connect vertically appears to vary from printer to printer (or does it vary from driver to driver?).

## Using the Font

Apart from that unfortunate exception, the font appears to print OK at 12pt, 24pt and 36 pt, on lasers and inkjets.

The font is still based on the Adobe standard of an M-square of 1000 units. This has caused problems in the past, with early versions of Word 2000, but no problems have been encountered so far with either Word 2000 or Word 97, even though Microsoft still “strongly recommend” an M-square of 2048 units.

By maintaining one version of the font for development, and one for font generation, it has been possible to produce a font with a better screen display, which is particularly noticeable in those areas where NT’s screen rendering has always been a bit off-hand: in Font Preview, CharMap and Insert/Symbol. (Hinting is still done automatically.)

There are 101 small things to clear up, mostly in the area of Extended Cyrillic. As these things improve, they will be made available via the Vector website, but new releases will not be announced, unless something major changes.

***

<b>Footnotes</b>

<a id="note1"></a>

<ol>
<li>“Sami” is now the preferred term for Lappish, the language of the Laplanders. “Perhaps 30,000 speakers”, according to the Dictionary of Languages. The Ethnologue site at <a href="http://www.sil.org/ethnologue/ethnologue.html">http://www.sil.org/ethnologue/ethnologue.html</a> lists eight dialects, with a total of over 40,000 speakers. This should serve as an indicator of how uncertain our knowledge is – this is no place for dogmatism:  the two references do not even agree on how to spell the name of the language. One dialect, Kildin, uses the Cyrillic script.

<a name="note2">
<li>“Komi is spoken in the Komi republic in north-eastern European Russia along river valleys that drain into the Barents Sea”. (The Dictionary of Languages, again.)  Somewhat surprisingly, given the distance between them, their language is closely related to Sami. The Soviet administration appears to have taken a liberal view of minority languages that would shame most Western governments.

<a name="note3">
<li>This list is maintained by the Estonian Standards Bureau, and provides language-related information not available from the Unicode site.

<a name="note4">
<li>Now officially renamed “Pilipino”, Tagalog, with 10,500,00 speakers, is just one of eight major languages of the Philippines, but now appears to be the dominant one. Its Latin rendition is unique in its use of double-width diacritical markings – something of a problem for those brave souls trying to implement Unicode text interpreters. The double-width tie and tilde are provided in SimPL.ttf – laying out the page is your problem. The Constitution of the Philippines provides for a new, synthetic, language to be known as Filipino.
</li></a></li></a></li></a></li></ol>

Copyright © Phil Chastney 2000
