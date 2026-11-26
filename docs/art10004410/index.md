---
vid: '10004410'
title: A Note on the Match Function in APL
authors:
- Joseph De Kerf
volume: '7'
issue: '4'
page: '133'
status: not online
issue_tab: n4-april-1991
---

Joseph De Kerf
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 133](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=135)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>A Note on the Match Function in APL by Joseph L.F. De Kerf</p>
<p>Abstract In SHARP APL, empty arrays of the same structure always match. In APL2-like implementations, empty arrays only matchif they are of the same structure and prototype. As a compromise, the APL Working Group - designing the new standard ISO APL Extended - defines the match function such that when the arguments are empty arrays of the same structure a domain error is reported.It is suggestedin this note that the definition of the match function for arrays with the same structure should be consistent with the definition of the primitive scalar function equal.</p>
<p>Introduction In ISO APL 8485 (1), the dyadic scalar function equal Z+A=B is defined such that, if the arguments 4 andB are arrays of the samestructure, the explicit result Z is a booleanarray of the same structure as the arguments, containing a 1 where the corresponding elements of A and B are equal, and 0 otherwise. If one argumentis a scalar or one-item array, or if both arguments are one-item arrays, so-called scalar extension is applied. If the arguments A and B do not conform, an appropriate error message is reported. Comparison Tolerance [ICT is an implicit argument.</p>
<p>Apparently there is no problem with this definition. With the introduction of arrays of arrays however, a controversy has arisen about the new dyadic function match Z«A=B. In some implementations, empty arrays of the same structure always match. In some other implementations, empty arrays oftle same structure only match if they are of the same prototype. Confronted with this controversy, the APL Working group ISO-IEC/JTC1/SC22/WG3- designing the new standard ISO APL Extended - defines as a compromise the match function such that when the arguments are empty arrays of the samestructure a domain error is reported.</p>
<p>The SHARP APL Approach In SHARP APL(2), based on the groundedarray concept, the dyadic function Z&lt;A=B, if the arguments A and B are of the same structure and data, returns as</p>
<p>133</p>
<p>VECTOR Vol.7 No.4</p>
<p>explicit result Z a boolean scalar 1, and 0 otherwise. Two empty arrays of the samestructure such as the empty character vector '' and the empty numeric vector 10 match, as they have no elements; ''=10 «+ 1. The definition conformsto Ken Iverson s APL Dictionary (3).</p>
<p>The NARS Approach In APL*PLUS NARS(4) and IBM APL2-like implementations (6), based on the floating array concept, two arguments A and B match if they are of the same structure and data, and if empty, they are of the same structure and prototype. Two empty arrays of the same structure but different prototype, such as the empty character vector '' and the empty numeric vector 10, do not match: Tre1d &lt;&gt; 0.</p>
<p>Thebasic idea seemsto be that two arrays matchif and only if they behavein the same way. For example, the first function +B applied to the empty character vector '' gives a blank, while applied to the empty numeric vector 10 gives a zero, and as such they do not match. However, match doesn t guarantee the same behaviour. For instance (the example is from a private communication by Eugene McDonnell):</p>
<p>Ocr+1z°13 Ati B+0,999999999999999 A=B 1 (1-A)21-B 9</p>
<p>The reason is of course that, such as for the dyadic scalar function equal, comparison tolerance (CTis an implicit argument. If comparison toleranceis set to zero, or the match function is defined as independent of comparison tolerance, two non-empty arrays of the same structure which match, always behave in the same way. Andin the example given above, A and 2 no longer match:</p>
<p>o¢r&lt;o Atl B+0.999999999999999 A=B 0 (1-A)s1-B8 0</p>
<p>134</p>
<p>VECTOR Vol.7 No.4</p>
<p>Defining the dyadic function match as independent of comparison tolerance however would produce situations which do not conform with the definition of the dyadic scalar function equal and are unacceptable. For instance, the product of an integer with its inverse could give a result which never matches the value one:</p>
<p>A+100p1 B«Bx+B+1100 Ocr&lt;1E-13 A=B</p>
<p>ocr+o A=B 0</p>
<p>the discrepancies being dependentfrom the particular implementation used. For the implementation used in preparing this note (VAX APLVersion 3 - DEC):</p>
<p>Ae100p1 B&lt;100 Ocr&lt;o (A#Bx+B)/B 23°27 46 54 89 92</p>
<p>which means that for this implementation the identity Bx+B«+&gt;1 does not hold for the integers listed (23-27-46-54-89-92). Consistency of the concept match with the basic idea of assuring the same behaviour seemsnotto befeasible.</p>
<p>The ISO APL Approach In the meantime, the APL Working Group ISO-IEC/JTC1/SC22/WG3 is preparing a new APLstandard, provisionally called ISO APL Extended. Atits Copenhagen Meeting in August 1990, the APL Working Group decided that "APL Extended must include the generalized array facility by defining the functions enclose ¢ and disclose &gt; (as defined by APL2) . As far as the inclusion of the dyadic function match is concerned however, it was clear that it would be very difficult to come to a concensus aboutthe controversy described.</p>
<p>Finally, based on an earlier proposition from Eugene McDonnell (6), it was decided to define the dyadic function match in such a way that, when the arguments are empty arrays of the samestructure, a domain error is reported. This gives the implementor the opportunity to choose - as a consistent extension - whetherin this case the explicit is 1 or 0. This has no sense. In addition, as Jean-</p>
<p>135</p>
<p>VECTOR Vol.7 No.4</p>
<p>Jacques Girardot stated at the APL 90 Conference (7), "it means that what the standard defines as a conforming program should not use A=B without first checking the case of empty operandsof the same dimensions - an unacceptable situation.</p>
<p>Suggestions There is however a plausible solution. APL, as every higher level programming language, even assembler, contains a lot of redundancy. Redundancy increases the power of a language, but redundancy mustbe consistent. For the dyadic function match A=B for instance, this meansthat its definition must be consistent with the definition of the dyadic scalar A=B as given above;if two arrays A and B havethe samestructure, they should matchif and onlyif the conjunction 4 of the - enlist of A=B is 1 or A=B ++ A/ENLIST A=B (ENLIST? R being a monadic function that returns as explicit result a simple vector whose elements are all the single scalars in the right argument R taken in eventually nested row major order, as for instance the primitive function IR in some APL2-like implementations and the monadic system function QENLIS? R in VAX APL Version 3 of DEC). This gives for two empty arrays with the samestructure as for instance the empty character vector '' and the empty numeric vector 10:</p>
<p>A/DENLIST ''=10</p>
<p>which means that those empty arrays match. Maybe the APL Working Group ISO-IEC/JTC1/SC22/WG3_ could reconsider its decision taken at the Copenhagen Meeting of August 1990. and discuss this proposal of consistency which,as far as the authorof this note is concerned,is a must.</p>
<p>136</p>
<p>VECTOR Vol.7 No.4</p>
<p>References (1] ISO 8485; 1989 (E); Programming Language - APL;First Edition ~ 1989 11- 01; Edited by A. Morrow;International Organization for Standardization- ISO, Geneva, Switzerland, 1989. See also : ISO 8485 (F).</p>
<p>22] K.E. Iverson; Composition and Enclosure; SHARP APL Technical Note SATN-41; LP.Sharp Associates Limited, Toronto, Ontario, 20 June 1981.</p>
<p>BI K.E.Iverson; A Dictionary ofAPL; Publication No. 0402 8703 E3, LP.Sharp Associates Limited, Toronto, Ontario, March 1987. See also: APL Quote Quad, Vol. 18, No. 1, September 1987, pp.5-40.</p>
<p>i4] C.M. Cheney; APL*PLUS Nested Arrays System ~ Reference Manual; Publication No. P046-0381; STSC, Bethesda, Maryland, March 1981.</p>
<p>B] D. Rabenhorst; A PL2 Language Manual- Installed User Program 5798-D]B; Publication No. SB21-3015-0 ~ First edition; [BM Corporation, APL Development Department, San Jose, California, June 1982.</p>
<p>[6] E.E.McDonnell; Rationale for the Match Function - Proposed text for ISO Document; APL Working Group ISO-IEC/JTC1/SC22/WG3-N200; Dated 19 May 1988.</p>
<p>JJ. Girardot; Arrays and References; APL 90 Conference Proceedings, Copenhagen, Denmark, 13-17 August 1990; Edited by P Gjerlov; APL Quote Quad, Vol. 20, No.4, July 1990, pp.161-172</p>
<p>137</p>
</details>
