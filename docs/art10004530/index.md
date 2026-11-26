---
vid: '10004530'
title: 'Letter: PROPER Capitals'
authors:
- Adam Kertesz
volume: '16'
issue: '3'
page: '125'
source: trad/v163/adam163.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2000
tags:
- programming techniques
---

<table width="100%">
<td>From: Adam Kertesz</td><td align="right">Nov 1999</td>
</table>

Here is a little utility which capitalizes the first letter in a text string and any other letters in text that follow any character other than a letter.

```
    ∇ R A PROPER B;ALLC;BOOL;FIRST;IND;K;LC;N;P;UC;X; IO
[1]   ⍝ Adam Kertesz, 03/20/1998+05/21/1999
[2]   ⍝      To change text into ¨Proper¨ format
[3]   ⍝      B is a character vector to change
[4]   ⍝      A is a two part exception list, both vector of vectors
[5]   ⍝      If not exists: empty
[6]   ⍝      If exists: [1] remain as is , e.g. APL APL, WfWG WfWG
[7]   ⍝                 [2] should be all lowercase, e.g. AND and
[8]     IO 0
[9]    :IF 2 NC 'A' ⋄ A 2⍴ ''
[10]   :ENDIF
[11]   (A N) 2 A ⋄ B ,B
[12]   UC  AV[( AV⍳'A')+⍳26] ⋄ LC  AV[( AV⍳'a')+⍳26] ⋄ ALLC UC,LC
[13]   BOOL B∊ALLC ⋄ FIRST 1 ⋄ R ''
[14]   :WHILE 0<⍴B
[15]     X +/^\BOOL=BOOL[0] ⋄ K X B ⋄ B X B
[16]     :IF BOOL[0]
[17]     :ANDIF ~( K)∊A
[18]       IND ALLC⍳K ⋄ P (( K)∊N) FIRST ⋄ K[0] UC[26|IND[0]] ⋄
            K[P ⍳⍴K] LC[26|P IND]
[19]     :ENDIF
[20]     R R,K ⋄ FIRST FIRST>BOOL[0] ⋄ BOOL X BOOL
[21]   :ENDWHILE
    ∇
        PROPER 'this is a TITLE'
This Is A Title

        PROPER '76budGet'
76Budget

        PROPER 'ATHENS, georgia'
Athens, Georgia

        LIST1 ''
        LIST2 'THIS' 'IS'  'A'
        (LIST1 LIST2) PROPER 'THIS IS A TITLE'
This is a Title

        LIST1 'APL'  'COBOL'
        LIST2 'ARE'  'AND'
        (LIST1 LIST2) PROPER 'APL AND COBOL ARE LANGUAGES.'
APL and COBOL are Languages.

        (LIST1 LIST2) PROPER 'ARE APL AND COBOL LANGUAGES?'
Are APL and COBOL Languages?
```

***
