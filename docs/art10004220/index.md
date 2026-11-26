---
vid: '10004220'
title: Thoughts on
authors:
- Maurice Jordan
volume: '7'
issue: '4'
page: '119'
status: not online
issue_tab: n4-april-1991
---

Maurice Jordan
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 119](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=121)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>Thoughts on +r g A by Maurice Jordan</p>
<p>Introduction These notesarise from ideas first stimulated by Iverson's presentation at APL88 on yoke (which allows amongotherthings infinite sets to be handled simply in APL), and function assignmente.g.</p>
<p>sumet/</p>
<p>Yoke has been superseded by the phrasal forms hook and fork (APL89) andthese are now implemented in J (APL90). These phrasal forms assign meaning to phrases such as</p>
<p>Ief gh</p>
<p>where f g and are functions. Powerful though the concepts are, are they the bestuse of this phrasal form?.</p>
<p>Experiments using Dyalog APL Function assignment has been available in Dyalog APL since their introduction of defined operators in 1986. When used with operators it becomes possible to build quite complex functionsin a simple sequenceofassigned functions.</p>
<p>As soon as I got my hands on a Beta-release version of Dyalog/386, I began experimenting with fork and hook. Hookis simply expressed as an operator:</p>
<p>VY Z+{A}(£ hook g)B a Elidable left args must be enclosed in {} av Iverson/McDonnell's hook phrasal form. na 2++A £ g B (dyadic); Bf g B (monadic} +(O#0QNC 'A')4E1 0 A+B Li:a*A £ g B v</p>
<p>Butthe trouble with fork is that it needs a mechanism for deriving an operatoror a way of passing more than two functions to an operator. I quickly foundthatI</p>
<p>119</p>
<p>VECTOR Vol.7 No.4</p>
<p>could use Bob Smith s NARS composition operator as implemented in Dyalog (analogousto &amp; inJ) to bind 3 functionsso that they lookedlike one</p>
<p>fegeoh</p>
<p>and thus fool the interpreter. Once inside the operator, the intemal representation can be examined:</p>
<p>EEE fght+/erep ADISP OCR 'fgh' a ADISP is a utility to display nested</p>
<p>and in simple cases (where f g and h are primitive) the original f g and h can be reconstructed easily:</p>
<p>VY Z2+{A} (fins fork)B;f;9;A;FNS a Iverson/McDonnell's phrasal forms idea. A fnse+fogeh; Z*({A} £B) g ({A} fh B) a8 £g h are primitives here, FNS&lt;«QCR'fns' f+s2&gt;FNS © g+2322FNS © A+ 3aFNS a Monadic &gt; is + in APL2 +(OSQNC'A')}+Monad o 2+{A £ B)g AA Bo +0 Monad:Z+(f B)g h B v</p>
<p>This allows</p>
<p>mean++/erep fork mean i234 5</p>
<p>I was able to experiment enough with these operators to realise that their combination with function assignmentleads to a powerful tool.</p>
<p>120</p>
<p>VECTOR Vol.7 No.4</p>
<p>A Better Way This representation of fork depends on a fudge. The fudgeis clumsy anddifficult to generalise so that it deals with defined, derived-from-defined, and assigned functions. Instead of the composition primitive, relying on undocumented internals (therefore liable to change), why not regain control and use a defined operator?</p>
<p>v Z+(f J_ g)B aV A Join operator to stack functions together for operator a calis and unstack them within the operator. +(B=0)4hi 9 Z+f o +0 L1:a+g v</p>
<p>This enables two (or more) functions to be joined together for syntactic binding in operator calls, and provides an easy means of unscrambling them again within the operator. This is used in the next generation of the fork operator:</p>
<p>Vv Z+{A)(fgh J_)B;fgsfigih f£j_g jl av Performs J's fork construct when fgh is of form 2(0=0NC 'A')/'Atright! fg+fgh 0 a fgh must be of form Lj_g fA ferg 0 grfg 1 hefgh 1 Z+(A f£ B)g(A h BY} az:a MHJ 075ep90 v</p>
<p>meanet+/ j_ + Joe IL mean i234 5 3</p>
<p>rightis simply the right identity from Dictionary APL:</p>
<p>VY B+(A}right B aV Right identity (+ in Dictionary APL) v</p>
<p>an identity as this is not a primitive in Dyalog APL. The idea of assigning in ambival ent function s and operator s comes function for elided left arguments from Phil Last. What a pity we can t write</p>
<p>121</p>
<p>VECTOR Vol.7 No.4</p>
<p>¥ Ze{Ae+}(fgh J_) B</p>
<p>to supply a default for an elided left argument and avoid clumsy conditional executes or computed gotos.</p>
<p>Even Simpler A simpler construction, requiring only onecall of j_ results from</p>
<p>V Ze{A} (fg Fork h)B;f3g av A simpler form of fork depending on the stack operator Je 2(O=0NC 'A')/'Atright! f+fg 0 a fg must be of form f j_ g g+ig 1 Z(A £ BYg AB az:a MHS 07Sep90 v</p>
<p>mean++/ j_ + Fork p mean it234 5 a</p>
<p>Other Formsof Fork Otherformsof fork can be derived from</p>
<p>¥ 2+{A}(f£ strand g)B £(OS0NC'A')/'Aeright! Z+(A £ B)(A g B) v</p>
<p>meantae(+/)e(+/ strand p) a rather unlovely</p>
<p>or</p>
<p>meantze(+/)e(+/ j_ p fnarray)</p>
<p>where fnarrayis an operator to simulate Benkard s function array ideas but lacks the elegance to be shown here.</p>
<p>122</p>
<p>VECTOR Vol.7 No.4</p>
<p>Back to the Yoke Although forks may be simulated in these waystheir usage lacks the elegance achieved by simply, but tediously, building a series of defined operators to cover the g functions inthe £ g A fork (as in the original yoke idea) e.g.</p>
<p>VY Z«{A}(L and g)B avY AND operator for proposition functions f and g #(O=QNCtA')/'Acright'! Z+(A £ B)sAA Gg B v</p>
<p>Vv 2+{A}(f£ or g)B av OR operator for proposition functions f and g 2(O=DNC'A')/'Atright | Z+(A £ BVA Gg B v</p>
<p>50 2.8.</p>
<p>integer_matrix+integer and matrix</p>
<p>This approach has the advantage that with very little added complication an early exit and/or error handling may be provided, allowing DOMAIN ERROR to be avoided in functions such as</p>
<p>probability+(se1) and (2°0) and numeric</p>
<p>Alternative Meaning for f g 2 The experimentation with fork, hook and function assignment thus made possible confirm the expressive powerof these constructs. However, muchis due to the composition operator, this operator very soonstarts to get in the way with simple functions such as matrix:</p>
<p>matrix*&gt;e2e=ep9p</p>
<p>which canbe applied to any array:</p>
<p>any_array+ieleft</p>
<p>123</p>
<p>VECTOR Vol.7 No.4</p>
<p>APLallows meto assign from theright in a simple expression:</p>
<p>Zelgha fer Yeg Xen A</p>
<p>Whenassigning functions, whycan t I write expressions such as</p>
<p>Z+(fgh+L (gheg h))A</p>
<p>In other words, why shouldn t APL allow meto write</p>
<p>matrix+&gt;2=pp any_array+¢i1a</p>
<p>In this context dyadic functions with left arguments are regarded as deriving monadic functions. Commute can be used for the rarer cases where we want a monadic function derived from a dyadic function andright argumente.g.</p>
<p>spete= a 'spe'+$'eps'</p>
<p>For example, rewriting some J assignments from Vector 7.1 p 72 with APL equivalents:</p>
<p>cos=. 20, A cos+20 Gr. #8180 A g+t5180 A or g+180+~ rld=. 0.9 a rideog cosd=. cos rfd A cosd+cos rfd Sind=, 10.0.(%8180) 9 sind+100180+=</p>
<p>This last expression was originally</p>
<p>Sind=. 180,.8(0.8(48180))} a sind+1ceo0(oe{+0180)) J</p>
<p>I have found that insertion of composition (and parentheses) into phrases for function assignmentis often non-trivial. Yet, with the exception of cases like the definition of g above whereanalternative is available through the commute operator, the expressions can be read simply if composition and parentheses are ignored. Why not provide the facility to write them without composition? Unfortunately this is in conflict with fork and hook. My experience so far is that</p>
<p>124</p>
<p>VECTOR Vol.7 No.4</p>
<p>fork and hook are a (notinsignificant) minority of useful function assignments. For my purpose, it is more useful to use the yoke concept for fork, even if it meanswriting a family of operators which could have been derived from yoke.</p>
<p>Conclusion Although composition (¢ in Dyalog and NARS,or &amp; and other operators in J) is the basis for a lot of useful assigned functions and other constructions, it quickly ends upin the way.It should be possible to reduceit to an elided operator with a suitable definition of APL syntax. My impression is that there is a language of exceptional beauty simplicity and powerstruggling to emerge. Which constraint on our current thinking must be brokento releaseit?</p>
<p>References and Further Reading [1] Iverson A Commentary on APL development Presentation at APL88 published in QuoteQuad 19.1</p>
<p>[2] Iverson, McDonnell Phrasal Forms APL89 Proceedings</p>
<p>[3] Hui,tverson,McDonnell,Whitney APL\? APL90 Proceedings</p>
<p>[4] Iverson ] Vector 7.1 [5] Cherlin APL Trivia APL90 Proceedings</p>
<p>[6] Benkard Nonce Functions APL90 Proceedings</p>
<p>[7] Benkard Structural Experiments with Arrays of Functions APL85 Proceedings Maurice Jordan 91 Coldershaw Road, WestEaling, London W13 9DU</p>
<p>125</p>
</details>
