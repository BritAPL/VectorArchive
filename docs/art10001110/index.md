---
vid: '10001110'
title: APL Trivia – Wimbock APL
authors:
- Dick Bowman
volume: '3'
issue: '4'
page: '111'
status: not online
issue_tab: n4-april-1987
---

Dick Bowman
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 111](../3/4/VOL.3-NO.4-APRIL-1987.pdf#page=113)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.3 No.4</p>
<p>APL Trivia Wimbock-APL Application Notes</p>
<p>by Dan Wimbock Someofthe technical features ofthis litde-known implementation were describe d in an earlier edition of VECTOR (April 1985); regular readers will be pleased to know that Dan hasnotbeenidle overthe past two years ~ indeed he was unable to exhibit at APL86 due to pressure of other commitments.</p>
<p>The majorthrust of developmenthas been not so much in advancing technologicalfeatur es of the interpreter (although the airborne generalised array implementation is an advance on both floating and grounded which is worth anarticle in its own right ~ maybe next year); rather it has been in emphasising useability and the pragmatic nature of this particular incarnation of APL. Aninstanceof this combines Dan s interest in both Expert Systems and a new approach to help facilities within applications. When we look at the conventional help panel approach we find a number of problems whichlimit the possibility of actually providing the assistance which our user really wants.</p>
<p>a) Information displayedis in a quite rigid format - once seenit can only be re-seen; ifit fails to create understandingthereis rarely an alternative phrasing which could shed light. b) Analysis of the user s problem is the user s responsibility the system doesn t attemptto gain insightinto why help is needed. Dan s view of the solution to this problem alsofalls into two parts, a) Theability to construct meaningful and grammatically-correct sentences in the user s ownlanguage. b) A diagnosticfacility which can analyze underlying problems.</p>
<p>Takingthefirst, consider a sentence encountered very early in the education process of most English-speaking humans yet one which is only uttered under the rarest of circumstances by the most advanced computing hardware. . . The cat sat on the mat Grammatical structure is subject-verb-object; a structure which is able to tolerate considerable distortion in termsof additionalqualifiers (adjectives and adverbs), plurality ( the cats sat on the mats ) and abstraction ( a cat sat on a mat ). This single sentence structureis capable of embracing an immense diversity of subject matter (cats, dogs, mats, rugs, even computers) and the most advanced of technological concepts. . . The X-195A computer architecture is based on 73-bit Gallium Arsenide VLSI technology</p>
<p>lil</p>
<p>Vol.3 No.4 VECTOR</p>
<p>eneration-facility GHMGF)which is Dan has therefore implemented a help-message-g Vocabulary (AV), with based on this expert-system-paradigm (ESP) using Appropriate on (PRPS) as a method ology for percept ual diagonalisation pseudo-random phrase selecti the same stimulus success ively under of the underlying message (invocation of the facility evel before ion of the messag e, nor doesit take the user to a deeperl does not generate repetit understanding ofa first-order message is attained). . . Taking as an example the sub-phrase structure illustrated below.</p>
<p>J-. ober | | ote inze} ta verr) |} jorrr} loLpl</p>
<p>This example uses the APL2-emulation mode of Wimbock-APL: QAPL+'APL2" DPYis from Brown (1986) The PRPSalgorithm is simplicity itself:</p>
<p>Ca} Calex/73t0rs-0 QZ+PRPS TEXT;ORL</p>
<p>[2] Zev( Pp" TEXT) &gt; TEXT y</p>
<p>Hence: PRPS GOBLIN THE SMALL ELF PRPS GOBLIN A VERY OLO ELF create a full and The system designer needsonly feed ina suitable vocabulary in orderto at Wimbock future extensions planned varied selection of relevant help messages; automatic extraction ofvocabulary from system design documentation Enterprises include customisation and utilisation of the (NLT system variable to create national-language may require some work on the Generalised Grammar Interface GGI). (although thislatter the )SI stack to determine Theother half of the solution entails straightforward analysis of s is combine d in the full implementation with a the contextof the user s problem, andthi Thisis presently the pseudo-ELIZAdialoguing technique toelicit the user s state of mind. Wimboc k Enterpris esare unable to divulgef ull details of subject ofa patent application and averagely diligent the techniques and algorithms used; however Dan feels that the quickly (it is unlikely VECTOR reader should be able to devise a suitable solution fairly would be longer that using thefull capabilities of Wimbock-APL the necessary function than &lt;PRPS&gt; above).</p>
<p>Reference: Algorithms for AI in APL2;Brown, APL86 Tutorials (Editor: Gettem while we gottem).</p>
<p>112</p>
<p>VECTOR Vol.3 No.4</p>
<p>Recent Enhancements to Wimbock-APL: Danhasjust announced a newrelease, containing a numberof enhancements among them being... DIAPL System Function Ensures application portability from other environments; (APL maybe set to values APL2 , IPSA , STSC , VSAPL , APL68000 , DYALOG in additionto default setting WIMBOCR . When set all symbols are interpreted in the context of the parent implementation. Attempts to set JAPLto an unrecognizedvaluewill result ina DOMAIN ERROR, e.g. OAR LAPEt DOMAIN ERROR OAPL*" TAPE a Monadiciota domain extended Negative integers: _ - e. v5 172 73 4 75</p>
<p>Complex integers: 1252 1J1 Ww? 271 22</p>
<p>Further extension to non-integer argumentsare anticipated soon .</p>
<p>And So Forth Operator This is a new niladic operator intended to improve productivity at the function definition stage; it is only available on the dedicated Wimbock-APL Machine andtakes advantage of the advancedAI featuresof the machine. At any point during function definition whenitis obviousto the programmerhow to continuethe andsoforth operator may be invoked, for example: [12] Bost 2 1exy2 113) xyaeg 10710 Tia) a. {21) Orexyz Control of function definition is regained by the programmerat-the explicit line number entered. This feature has been incorporated to make Wimbock-APL more attractive to conventional systemsanalysts and Dan wishesto expresshis gratitudeto his supervisors in earlier years for making him awareof the usefulness ofthis notation. Circle reclassified as an operator This is an enabling move for a further generalisation of array structure (away from Tectangularity to a more general ovoid form); in the present implementationits usefulness is limited, but for example 90002 p14 12 34</p>
<p>Notethatthis assumesprior use of the JANGLESsystem variable which allowsuser choice between radians and degrees as measureofrotation. Notealso that domain is severely restricted with presentarray limitations 8702 2014 DOMAIN ERROR 87602 2p 14 A 113</p>
</details>
