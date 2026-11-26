---
vid: '10009300'
title: 'Review: “APL Notes” by Jim Weigang'
authors:
- Walter Spunde
volume: '11'
issue: '1'
page: '57'
status: not online
issue_tab: n1-july-1994
---

Walter Spunde
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 57](../11/1/VOL.11-NO.1-JULY-1994.pdf#page=59)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.11 No.1</p>
<p>Jim Weigang s APL Notes reviewed by Walter Spunde</p>
<p>The learning curve for APL,they say,is very steep:it is hard to get going, to feel competentusing APL, and it seems to be true. Students doing the University of South Queensland mathematics course built on the use of APL (admittedly a course that throwsthem in at the deep end with ° . * and ~ in thefirst week)all say the same thing:it was hard at the beginning.For instructors who wantto use APL as a tool for teaching something else, there is a need for a book that provides not too much, but enough, information about APL an easy reference, to satisfy the curious, to stimulate the adventurous, but one that won t daunt the slow or divert the insecure. Jim Weigang s APL Notes seemstofit the bill, but not without somereservations.</p>
<p>Its 164 pages of uncluttered A5 in a three-ring binder(after the style of early APL*PLUSreference manuals) are split into seven chapters dealing with: APL expressions and mathematical functions; building arrays; indexing and reduction; programs and workspaces; comparisons and selections; branching, looping and debugging; and finally, the remaining APL primitives and operations withfiles. There are a further 35 pages of appendices and a useful index which includes page references to APL primitives dealt with in the text. Like Dalois Introduction to APL*PLUS/PC the book comes with a copy of Manugistics student version APL*PLUS/SE, where OWA in a clear workspaceis 180K. Alan Mayer's review ofthis book in Vector 10.2 pages 19-20, describes this system.</p>
<p>Weigang s style is conversational, enthusiastic and easy to read. His care with edge conditions in writing functions is meticulous. There is a nice explanation of why MC 31) and MC ;,13 give different answers and the implications of writing ME ;~1tpM] or MC ; (pM)02]1]. The discussion of line-labels and local variables explains simply whatwill be happening in a function thatcalls other functions. His explanation of UC makes clear the difference between a=5 and 0=a~b, Equally pleasing is his discussion of identity elements for various operations illustrated by +/10 and x/10, with the corollaries that x/pA gives the correct numberof elements for any array and that L/10 returns the largest numberthe computer can represent. Unfortunately, he describes this numberas infinity, having promised, in the introduction to the chapter, to display infinity. The reviewer did hope he would notsay it three times [Ed: WhatI tell you three times is true] but, on page 147, he did. It will not endear him to mathematics teachers, who mayfind the usage unforgivable. After all, calculus and indeed the</p>
<p>57</p>
<p>VECTOR Vol.11 No.1</p>
<p>real number system is based on a concept ofinfinity and infinite processes that cannot accommodate the constraints of a computer.</p>
<p>Mathematics instructors may also find his treatmentof functions disappointing. He prefers to use the term program andintroduceshis first program with a niladic example using global variables, and then progressively improves the program. This is probably the correct approach for those who cometo the study with the concept of a program as something which runs when invoked,butit avoids the emphasis on input*output that is so helpful in reinforcing the mathematical conceptof a function.</p>
<p>It is not clear whether Weigang sees his reader as someone from a business or from a scientific background. Certainly he has included functions for solving quadratic equations andcalculating integrals by Simpson's Rule.</p>
<p>A discussion of OWIN and associated system functions is one of those areas that must be passed over in a mathematics course but that students, once introduced to it, seek further information on, andit would surely give a lot of satisfaction to those from a business background. Unfortunately, this information is not there. A good case may well be made for teaching only language features and leaving system features alone, but when the book comes with a disc that places so many powerful facilities easily at the reader's fingertips, it seems a pity to pass over them without mention. Coupled to this, one may find advice on how to cope with older APL systems, the V editor, 0 and @, and some of the supplied functions obviously written for these older systems, somewhat irrelevant and perhapsa little confusing for the student. With the APL*PLUSfull screen Editor at hand,the discussion of system functions for native files will only be useful to the student when (s)he needs to manipulate file data from within a function.It seemsan unlikely task for a beginner.</p>
<p>For those who use the standard keyboard, there are ample references to the location of particular symbols (which of course will be annoying to those instructors who use the APL*PLUSfacility to re-define the keyboardsothat, e.g., &lt; appears on the &lt; key,etc.).</p>
<p>The text has very few typographic errors, leaning APL being the exception. The index inexplicably omits the function ? which nevertheless is used as roll on page 143 and mentioned in conjunction with deal (which is not explained) on page 155. One might have wished for a more detailed discussion of matrix multiplication (important both for business and mathematics students) and more of the sort of examples that make Helzer s Encyclopedia so rewarding to peruse. However, the language basics are there, and, if one wants a short and quick guide, one hasno rightto ask for too much.It is certainly a useful addition to the</p>
<p>58</p>
<p>VECTOR Vol.11 No.1</p>
<p>literature and, despiteits failure to exploit and describe the APL*PLUS system as fully as it might, it has been well enough received at the University of South Queensland to haveit set as a text, together with study notes, for a diploma course in APL.</p>
<p>Comments on Review of APL Notes by Jim Weigang</p>
<p>Editor s note: | sent Walter Spunde's review to Jim Weigangfor comment and to ensure it contained no misunderstandings. Below is an edited version ofJim s reply:</p>
<p>Jim feels that his use ofinfinity to meanthe largest representable number would be quite understoodin computercircles, thoughstrict mathematiciansfindit incorrect. Jim aimed to makethe bookrelatively neutral betweenthescientific or business audience. When someonelearns APL, the book should be combined with problemsfrom the reader's speciality. I am not generally qualified to providethese, and I mostly wantto step out of the way and avoid supplying inappropriate examples. He continues:</p>
<p>Walteris right about it being a pity that most of the APL*PLUS quadfunctions are omitted from the text. This was done partly to keep the book simple, and partly because I don t expectthatall readers will use the APL*PLUS interpreter included with the book. But the next edition should have examples of the most useful APL*PLUS quad functions anda brief summary of all of them.</p>
<p>A few minorcorrections: Althoughtheinterpreteris the same as the one included with Dalois Introduction to APL*PLUS/PC,most ofthe other programs, includingthe utility workspace, user commands, and full-screen debugger, are unique to APL Notes. The value of DWA in a clear workspace depends on your computer's configuration.I get 300,000 bytes free using DOS6 with a few resident programs. The deleditor is mentioned only in a single sentence which saysit is obsolete. (A brief summary of del editor commands appears in the reference card.) The descriptions of how to enter APL symbols are for the Unified keyboard, not the standard APL layout as Walterstates. Yes, the question mark symbol(?) was inadvertently omitted from the index, although entries are providedforroll and random numbers.Rollis used in an example on page 141, not 143, butit is introduced on page 28.</p>
<p>The booksells for $29 postpaid in the U.S. and Canada, $37 postpaid elsewhere. Discountsare available for resellers and quantity purchases. Payment should be madeby postal money order or a check drawnon a U.S. bank. Order from the author, Jim Weigang, at 68 Plateau Circle, West Springfield, MA 01089, U.S.A. Inquiries may be sent via e-mail to jimw@math.umass.edu.</p>
<p>59</p>
<p>RENAISSANCE DATA SYSTEMS Enlightenment Thru information Processing</p>
<p>Specializing in Books and Software on APL, J and other Curiosities of merit.</p>
<p>HAVE YOU GIVEN A COPY OF IAPL OR J TO A TEACHER OR STUDENT?</p>
<p>For a copyof our most recentcatalog, please send a self-addressed, legal- Name ates sized envelope (stamped if from U.S.) Street or mail this form to:</p>
<p>Postal Code. Renaissance Data Systems State: Park West Finance Station City: Telephove Country, oO P.O. Box 20023 New York, NewYork 10025-1510</p>
<p>[beam APL } [ APL Shareware/Demos and Related Publications |</p>
<p>ngs APL and Mathematics APL AS ATOOL OF THOUGHTProceedi</p>
<p>[ Other Important Sources of APL Information. |</p>
<p>] [ APL References and Techniques | f. APL History</p>
<p>| [ APL Interpreters and Software | { Special Subjects in APL</p>
<p>[_ Je: A Powerful Dialect ofAPL } [- Other Software for Use with APL ]</p>
<p>[To Russia with Love and for APL92 ] [_ Other Curiosities of Merit |</p>
<p>|-APL: An international voluntary project of individuals and companies seeking to share their love of APL.</p>
</details>
