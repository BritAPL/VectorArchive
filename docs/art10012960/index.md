---
vid: '10012960'
title: 'Review: "APL Notes" by Jim Weigang'
authors:
- Walter Spunde
byline: by Walter Spunde
volume: '11'
issue: '1'
page: '57'
source: trad/v111/spunde111_57.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- reviews
- education
---

by Walter Spunde
{ .byline }

The learning curve for APL, they say, is very steep: it is hard to get going, to feel competent using APL, and it seems to be true. Students doing the University of South Queensland mathematics course built on the use of APL (admittedly a course that throws them in at the deep end with °.\* and × in the first week) all say the same thing: it was hard at the beginning. For instructors who want to use APL as a tool for teaching something else, there is a need for a book that provides not too much, but enough, information about APL – an easy reference, to satisfy the curious, to stimulate the adventurous, but one that won’t daunt the slow or divert the insecure. Jim Weigang’s APL Notes seems to fit the bill, but not without some reservations.

Its 164 pages of uncluttered A5 in a three-ring binder (after the style of early APL\*PLUS reference manuals) are split into seven chapters dealing with: APL expressions and mathematical functions; building arrays; indexing and reduction; programs and workspaces; comparisons and selections; branching, looping and debugging; and finally, the remaining APL primitives and operations with files. There are a further 35 pages of appendices and a useful index which includes page references to APL primitives dealt with in the text. Like Dalois’ Introduction to APL\*PLUS/PC the book comes with a copy of Manugistics’ student version APL\*PLUS/SE, where ŒWA in a clear workspace is 180K. Alan Mayer’s review of this book in Vector 10.2 pages 19-20, describes this system.

Weigang’s style is conversational, enthusiastic and easy to read. His care with edge conditions in writing functions is meticulous. There is a nice explanation of why M\[ ;1\] and M\[ ;,1\] give different answers and the implications of writing M\[ ;¯1î½M\] or M\[ ; (½M)\[2\]\]. The discussion of line-labels and local variables explains simply what will be happening in a function that calls other functions. His explanation of ŒCT makes clear the difference between a=b and 0=a-b. Equally pleasing is his discussion of identity elements for various operations illustrated by +/¼0 and ×/¼0, with the corollaries that ×/½A gives the correct number of elements for any array and that ˜/¼0 returns the largest number the computer can represent. Unfortunately, he describes this number as infinity, having promised, in the introduction to the chapter, to display infinity. The reviewer did hope he would not say it three times \[Ed: What I tell you three times is true\] but, on page 147, he did. It will not endear him to mathematics teachers, who may find the usage unforgivable. After all, calculus and indeed the real number system is based on a concept of infinity and infinite processes that cannot accommodate the constraints of a computer.

Mathematics instructors may also find his treatment of functions disappointing. He prefers to use the term program and introduces his first program with a niladic example using global variables, and then progressively improves the program. This is probably the correct approach for those who come to the study with the concept of a program as something which runs when invoked, but it avoids the emphasis on inputàoutput that is so helpful in reinforcing the mathematical concept of a function.

It is not clear whether Weigang sees his reader as someone from a business or from a scientific background. Certainly he has included functions for solving quadratic equations and calculating integrals by Simpson’s Rule.

A discussion of ŒWIN and associated system functions is one of those areas that must be passed over in a mathematics course but that students, once introduced to it, seek further information on, and it would surely give a lot of satisfaction to those from a business background. Unfortunately, this information is not there. A good case may well be made for teaching only language features and leaving system features alone, but when the book comes with a disc that places so many powerful facilities easily at the reader’s fingertips, it seems a pity to pass over them without mention. Coupled to this, one may find advice on how to cope with older APL systems, the ’ editor, Œ and �, and some of the supplied functions obviously written for these older systems, somewhat irrelevant and perhaps a little confusing for the student. With the APL\*PLUS full screen Editor at hand, the discussion of system functions for native files will only be useful to the student when (s)he needs to manipulate file data from within a function. It seems an unlikely task for a beginner.

For those who use the standard keyboard, there are ample references to the location of particular symbols (which of course will be annoying to those instructors who use the APL\*PLUS facility to re-define the keyboard so that, e.g., &lt; appears on the &lt; key, etc.).

The text has very few typographic errors, leaning APL being the exception. The index inexplicably omits the function ? which nevertheless is used as roll on page 143 and mentioned in conjunction with deal (which is not explained) on page 155. One might have wished for a more detailed discussion of matrix multiplication (important both for business and mathematics students) and more of the sort of examples that make Helzer’s Encyclopedia so rewarding to peruse. However, the language basics are there, and, if one wants a short and quick guide, one has no right to ask for too much. It is certainly a useful addition to the literature and, despite its failure to exploit and describe the APL\*PLUS system as fully as it might, it has been well enough received at the University of South Queensland to have it set as a text, together with study notes, for a diploma course in APL.

### Comments on Review of APL Notes

by Jim Weigang

Editor’s note: I sent Walter Spunde’s review to Jim Weigang for comment and to ensure it contained no misunderstandings. Below is an edited version of Jim’s reply:

Jim feels that his use of infinity to mean the largest representable number would be quite understood in computer circles, though strict mathematicians find it incorrect. Jim aimed to make the book relatively neutral between the scientific or business audience. When someone learns APL, the book should be combined with problems from the reader’s speciality. I am not generally qualified to provide these, and I mostly want to step out of the way and avoid supplying inappropriate examples.

He continues:

“Walter is right about it being a pity that most of the APL\*PLUS quad functions are omitted from the text. This was done partly to keep the book simple, and partly because I don’t expect that all readers will use the APL\*PLUS interpreter included with the book. But the next edition should have examples of the most useful APL\*PLUS quad functions and a brief summary of all of them.  
A few minor corrections: Although the interpreter is the same as the one included with Dalois’ Introduction to APL\*PLUS/PC, most of the other programs, including the utility workspace, user commands, and full-screen debugger, are unique to APL Notes. The value of ŒWA in a clear workspace depends on your computer’s configuration. I get 300,000 bytes free using DOS 6 with a few resident programs. The del editor is mentioned only in a single sentence which says it is obsolete. (A brief summary of del editor commands appears in the reference card.) The descriptions of how to enter APL symbols are for the Unified keyboard, not the standard APL layout as Walter states. Yes, the question mark symbol (?) was inadvertently omitted from the index, although entries are provided for roll and random numbers. Roll is used in an example on page 141, not 143, but it is introduced on page 28.  
The book sells for $29 postpaid in the U.S. and Canada, $37 postpaid elsewhere. Discounts are available for resellers and quantity purchases. Payment should be made by postal money order or a check drawn on a U.S. bank. Order from the author, Jim Weigang, at 68 Plateau Circle, West Springfield, MA 01089, U.S.A. Inquiries may be sent via e-mail to jimw@math.umass.edu.”

(webpage generated: 2 October 2007, 05:08)
