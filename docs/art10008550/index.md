---
vid: '10008550'
title: 'Hacker’s Corner: Native Files in Dyalog APL (Without Tears)'
authors:
- Adrian Smith
volume: '11'
issue: '1'
page: '126'
status: not online
issue_tab: n1-july-1994
---

Adrian Smith
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 126](../11/1/VOL.11-NO.1-JULY-1994.pdf#page=128)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.11 No.4</p>
<p>Hackers Corner: Native Files in Dyalog APL (without Tears) by Adrian Smith</p>
<p>Introduction Oneof the things you get used to in APL*PLUS/PCisfast, reliable access to DOS nativefiles; indeed many developers have tended to use these in preference to componentfiles for function filing (see for example my notes on Apack in Vector 8.2 pp.125-126) because the APL*PLUS componentfiles are somewhat slow by comparison.</p>
<p>In Dyalog/W,the opposite is true! Componentfiles go like greased lightning and the NFILES auxiliary processoris (in my opinion) somethingofa dog.It has the unpleasant habit of locking out completely if the AP cannot find space to load, andit litters your workspace with a bunch of unhelpfully-named functions like open, close etc. If you wouldlike the pleasure of wiping NFILES.EXE for ever from your hard disk, read on.</p>
<p>Raw Input and Output This investigation began when I got interested in making noises with APL! I discovered that the Windows .WAVfiles are simply a short header, followed by a stream of byte values (say 11000 per second) in the range 0-255 (where 128 represents the midpoint) which outline the waveform. APL has these funny circle functions, which are adeptat generating sine waves, so a suitable numeric vector will generate tones and harmoniesvery easily indeed. Exponential decay superimposed on random numbersgives a very authentic gunshot!</p>
<p>In orderto investigate the header structure, I clearly needed to readthefile as a stream of untranslated byte values, so turning to the SDK help-file I found enoughto hack together the folowing:</p>
<p>vV r+rget name;_lread;_Iclose;_llseek;_lopen;fh;size; sinks; blk;ct;bytes;get [1] a Get Raw file contents as a string of bytes, C2q a Returns 6 if file not found. [3] ONA'E kernel.Pi6{_lopen &lt;0? I' [sj ONA'US Kernel.P16|_liseek I gu I! [5] OWA'I kernel.P16|_Iclose I'</p>
<p>126</p>
<p>VECTOR Vol.11 No.t</p>
<p>[6] ONA'U4 kernel.P16|_lread IF &gt;U1[} U2' [73 r+@ 0 fhe_lopen name 0 o +("1=fh)to [8] Size+_llseek fh 0 2 [9] sink«_llseek fh 0 6 [10] a Maximum length is 64K per read, so get blocks ... [11] blk+64000 size o r+@ o sink«QWA {12] a Don't read lots of garbage with the last block (faster) 13] More:get&lt;«blklsize-pr C14) ct bytes+_Iread fh get get cas] +(cet=0}tDone a should never happen! C16] c,+cetthytes C17] a Have we got enough yet? C18) +(size&gt;pr)tMore [19} Done:sink+«_Iclose fh v</p>
<p>The functions are ANSI-standard C, and closely match the syntax offered by nfiles. Because theyare built in to Windows, they appearto run rather fast hereis a trivial example: rget '\apl\dyalog\serial' 48 48 48 49 539 57 10</p>
<p>.. note that this has a line-feed (10) but no carriage-return (13), indicating its Unix heritage! Theother half of the transactionis filled by:</p>
<p>Vv {r}+name rput bytes;_Iwrite;_lclose;_lcreat;fh;size; sink;bik;ct;bytes;put;len {1] A Put a string of bytes to file (no translation). [2] a Returns 1 if OK; 0 if failed. C3] OVA'I kernel.Pi6|_icreat &lt;o?7 I! cay OWA'I kernel.P16|_iclose I! 5) ONA'U4 kernel.P16|_lwrite I &lt;U1£] 2! (67 r+0 0 fh&lt;«_lereat name 0 o +(~41=fh)t0 C7] a Maximum length is 64K per write, so make blocks ... ca) b1k+64000 C9] More:len+bixipbytes 9 put+lentbytes [103 et+_lwrite fh put len [143 bytes«lentbytes [C12] a Have we finished yet? (13) +(O&lt;pbytes)tMore [44] Done:sink«_iclose fh 9 rei v</p>
<p>.. which will happily write out several K of meaningless noise in less time thanit takes to blink. Note that you will need to sort out the CR/LF pairs if you want the file to appear to DOS as records.It is sensibly behaved, in thatit will open (and truncate) an existing file, or start a completely new file without complaining.</p>
<p>127</p>
<p>VECTOR Vol.11 No.1</p>
<p>Text Files Having mastered the raw case, it occurred to me that I could probably use the same meansto read and write text files as well sure enoughI canstill beat the NFILES AP for speed, even though I am doing the ASCII translation in APL! Hereare the two functionsthat do thejob, without further comment:</p>
<p>vo renget name; _Iread;_Iclose;_liseek;_lopen;fh;size; sink: bik;ct;bytes;get;asc;020 [1] a Get Native ASCII file contents as a vector of vectors (2) a Returns '' if file not found. [3] Uva'I kernel.P16|_lopen &lt;oT I? ca) OWA'US kernei.Pi6|_llseek I v4 I! cs] OWA'T kernel,Pis|_Iclose I' [6] OWA'US kernel.P16| lread I &gt;U1L] Y2' t7] a Make translate table te) asc+256plet 9] asc(82t.32}e! 1°HSKR''()aet, ./04234567892; a&gt;7t £10) asc(64+132}+' @ABCDEFGHIJKLMNOPQRSTUVWXYZL\)A_' £44] asc[964+132]+' abcdefghijkimnopgrstuvvxyz{|}~ ' £12) ascl9 11 18]&lt;0T¢ [13] ree o fhe_lopen name 0 © +(71=fh)+0 C14) size+_liseek fh 0 2 C1s] sink«_liseek fh 0 9 (163 a Maximum Jength is 63K per read, so get blocks ... C173 blk«64000Lsize o re'' o sink«QWA o QI0+o [i8] a Don't read lots of garbage with the last black (faster) [19] More:get«biklsize-pr [20] ct bytes+_Iread fh get get [21] +(ct=0)+Done » should never happent [22] bytes+cttbytes £23] a Pransiate and append ... £24) r,easc[bytes] £25] a Have ve got enough yet? £26) +(size&gt;pr)tMore (27) Done:sink+_iclose fh o O10+1 C28] « Eliminate LF and partition on CR ... C29) re7igr-UTCes) (30) reae(r=Dre(z))er v</p>
<p>v (r}+name nput txt;_ivrite;_iclose;_icreat;asc:fh;size; sink: bik:ct;bytes;put:len C1] a Put a vector of vectors to file (ASCII translation). [2] a Simple vectors are tolerated! cal a Returns 1 if OX; 0 if failed, C4] OWA'I kernel.Pi6|_Jcreat &lt;oT I! [5] OVA'I kernel.P16|_Iclose I' 6) OWA'UY kernel.P16j_iwrite I &lt;U¥it] U2" £7] r+0 © fh+_Icreat name 0 ¢ +{(~1i=fh)t0 [8] 8 Make translate table c9] asc*2S6p'e' © asc{9 1% 1a]&lt;Drc Cio) asc(32tra2]e! PE" H$Z8''()et, ./02123056789;;&lt;=&gt;7! 4ad asc(64+132)+' GABCDEFGHI JKLMNOPQRSTUYWXIZ(\}4_' C12] asc(96+t32]+! abcdefghijkimnopgrstuvvxyz{t}- ' (133 «a Look up each element and add CR/LF pairs 14) a(2&gt;|stxt)/'txtecext!</p>
<p>128</p>
</details>
