---
vid: '10001520'
title: Another Use of Not-Equals-Scan
authors:
- Anthony Camacho
byline: by Anthony Camacho, . (acam@blueyonder.co.uk)
volume: '10'
issue: '4'
page: '136'
source: trad/v104/camacho104_136.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- programming techniques
- data and files
---

by Anthony Camacho, <span class="affil">.</span> <span class="email">(<a href="mailto:acam@blueyonder.co.uk">acam@blueyonder.co.uk</a>)</span>
{ .byline }

Donald McIntyre and I wanted to put together a disk of all his J writings. He regretted that he did not have disk versions of his APL 91 paper Mastering J or his article for the IBM Systems Journal Language as an Intellectual Tool: from Hieroglyphics to APL. I said I had a Logitech Scanman color and CorelDraw 4 which has an OCR-trace facility and I would see what they could do.

I did, and found that the scanner would produce a black and white bitmap which CorelTrace would read. CorelTrace produces an encapsulated PostScript file as scanner output. It uses the same filename as the file loaded and writes a .EPS file that begins thus:.

```
%!PS-Adobe-2.0 EPSF-1.2
%%Creator: CorelTRACE V4.0 in Adobe Illustrator 88(TM) format
%%Title: CorelTRACE Output
%%BoundingBox: 0.0 0.0 178.0 617.7
%%TileBox: 0.0 0.0 178.0 617.7
%%CreationDate: Thurs Sept 10 14:01:30 1992
```

When I looked at the rest of the file I was most impressed. I thought the fact that it specified the typeface as Helvetica meant that it recognised the font. Later I discovered that its output is always Helvetica. It doesn’t even distinguish between roman and italic or bold. Unfortunately I have no software that will read a .EPS file and extract the text from it. CorelDraw itself will import the .EPS file, but it comes in as a group of objects which cannot be edited until they are ungrouped. Each object is one line of text and I have not yet found any way to join them into a single string.

Adrian Smith suggested to me that, in PostScript, every text character will be in parentheses. It appears to be true although there are characters which are not text in parentheses too. The PostScript specification (PostScript Language Reference Manual, Addison- Wesley ISBN 0-201-10174-2) says that: “A string in PostScript is delimited by balanced parentheses... (Strings may include special characters \*-&\}^% and balanced parentheses ( ) (and so on).) . . .Within a string the ‘\\’ (backslash) character is used as an ‘escape’ for various purposes such as including unbalanced parentheses, non-printing characters and the ‘\\’ character itself.”

I thought that I would be able to write some APL to read the .EPS file and write the ASCII text to another file quite easily. I read the file into a text vector. I relied on the parentheses being balanced and did a not-equals scan on a boolean that marked them, to mark all the text within the parentheses. (Gérard would be proud of me.) It didn’t work because CorelTrace does not mark the unbalanced parentheses. I suppose it may assume that unbalanced parentheses are probably the result of scan failure. I wish it had marked every parenthesis within the text as unbalanced. That would have been within the specification and much easier to deal with. My reading of the specification is that CorelTrace does not meet it.

“Surely I can cope with that.”, I thought. I then wasted a day trying to find elegant methods. In particular I felt sure that subtracting a plus-scan of a boolean marking the closing parentheses from a plus-scan of a boolean marking the opening parentheses ought to be the right start. It gives a numeric vector where the values rise as each opening parenthesis is passed and reduce at each closing parenthesis. The trouble is that unmatched parentheses within the text ensure that there are non-zero values for characters outside the text strings. I couldn’t find a good way to reduce the values for the non-text characters reliably to zero.

Instead I tried another tack which appears to work. Below is the result. I have commented it enough for anyone to follow its working and have not saved time or space by combining lines which ought to be combined. It starts by collecting the positions of all the left and right parentheses separately. It aims to find each opening parenthesis, and remove any other opening parenthesis before the next closing parenthesis. and then to find each closing parenthesis and remove any preceding closing parenthesis closer than the next opening parenthesis to the left. I realise this really ought to be done recursively until there are no unbalanced parentheses to remove, but I chose instead to put in a test to see if the result is balanced and I’ve now scanned quite a few pages and never had a failure.

I had to recognise the carriage returns, which CorelTrace always renders as (\\r), and remove the () pairs if any which would cause the not-equals scan to flip and mark the non-text characters from () to the next (). It is written in APLIWIN and is more or less instantaneous on my 486 given the that largest file it can process is one page, or one column.

To my delight CorelTrace recognises most italic text almost as well as roman. It seems to be quite tolerant of noise. I have scanned dirty newsprint successfully. Of course there are severe limitations. It fails on a lot of Amstrad word processor print. It will not recognise subscripts or superscripts – the latter come out as quotes. It doesn’t recognise APL, however beautifully printed, or the sort of mathematical symbols Donald McIntyre is liable to use such as square root, infinity or sigma. But it does quite often read a paragraph without a single erroneous character.

### Appendix: the Code

```
      outfile extractasciifrom infile;tv;boo;posl;posr;mat;boomat;posrn;
                                      poslp;adj;posn
[1]  ⍝ Extracts text from CorelTrace .EPS infile and writes it to outfile
[2]   tv←⎕toapl ⎕hfread infile
[3]   posl←(tv='(')/⍳⍴tv ⍝         positions of left parens
[4]   posr←(tv=')')/⍳⍴tv ⍝         positions of right parens
[5]   mat←posr∘.⌈posl ⍝            larger of each posr and each posl
[6]   boomat←((⍴mat)⍴posl)≥mat ⍝   1 marks mat positions not to right of posl
[7]   posrn←⌊⌿mat+boomat×⍴tv ⍝     select smallest posr to right of each posl
[8]   posl←(nubsieve posrn)/posl ⍝ pick leftmost posl for each posrn
[9]   mat←posl∘.⌊posr ⍝            smaller of each new posl and each posr
[10]  boomat←((⍴mat)⍴posr)≤mat ⍝   1 marks mat positions not to left of posr
[11]  poslp←⌈⌿mat-boomat×2×⍴tv ⍝   select largest posl to left of each posr
[12]  posr←⌽(nubsieve⌽poslp)/⌽posr ⍝  pick rightmost posr for each poslp
[13] ⍝      There ought to be as many posr entries as there are posl and each
[14] ⍝      posr ought to be larger than the corresponding posl. So test!
[15]  →((⍴posl)≠⍴posr)/err ⍝       if they aren't the same length
[16]  →(~^/posl<posr)/err ⍝        if every posr is not larger than its posl
[17]  adj←~posr=1+posl ⍝           adj keeps all but adjacent ()
[18]  posr←adj/posr ⋄ posl←adj/posl ⍝ remove adjacent pairs
[19]  boo←(⍴tv)⍴0 ⍝                make an empty boolean
[20]  boo[posr,posl]←1 ⍝           put 1s in it
[21]  boo←≠\boo ⍝ ≠\ ⍝   switch on bits between ( and ) (guaranteed in pairs)
[22]  'There are ',(⍕+/boo),' characters between parentheses.'
[23]  posn←boo/⍳⍴boo ⍝             note start of each string in posn
[24]  'There are ',(⍕⍴posn),' strings of letters to extract.'
[25]  posn←(('\'=tv[posn+1])^'r'=tv[posn+2])/posn
[26] ⍝                             set tv to CR LF where group begins '\r'
[27]  tv[posn+1]←CR ⋄ tv[posn+2]←LF
[28]  'There are ''\r'' pairs at positions ',⍕posn
[29]  tv←(boo^0,¯1↓boo)/tv ⍝       select the characters required
[30]  (⎕toascii tv) ⎕hfwrite outfile
[31]  →0
[32] err:'It went wrong! ⍴posl= ',(⍕⍴posl),' and ⍴posr= ',(⍕⍴posr)
[33]  'posl:',(⍕posl) ⋄ 'posr:',(⍕posr)
```

(webpage generated: 28 March 2006, 06:57)
