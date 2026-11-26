---
vid: '10501040'
title: My favourite APL symbol
authors:
- Roger Hui
byline: by Roger K.W. Hui
volume: '26'
issue: '1'
page: '36'
received: '2013-01-23'
online: '2013-02-02'
description: The implementer of J explains what his favourite APL symbol is – and why.
source: content/printed/261/myfavourite/myfavourite.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- language design
- mathematics and statistics
---

by Roger K.W. Hui
{ .byline }

<div style="font-size:2em;margin-bottom:.3em;text-align:center;"><code>⍟ ○*</code></div>

One of the distinguishing characteristics of APL is its unique character set, containing 150-200 symbols. My favorite is `⍟`, the symbol for logarithm. Originally, the log symbol was formed by ‘overstriking’ `○` (circle) and `*` (exponential or power). At present, `⍟` is Unicode[\[1\]](#ref1) code point `0x235F`.

## Reasons for liking `⍟`

<ul class="bullet">
		<li>
			It’s kind of cute, possessing a radial symmetry.
		</li>

		<li>
			It denotes a function for which conventional mathematical
			notation <a href="#ref2">[2]</a> does not have a good symbol:
<pre>
        ⍟y   ←→ ln <em>y</em> or log <em>y</em>

        x⍟y ←→ log<sub><em>x</em></sub> <em>y</em>
</pre>
		</li>


		<li>
			It alludes to <code>0=1+*○0j1</code>, the most beautiful
			equation in all of mathematics <a href="#ref3">[3]</a>,
			relating in one short phrase the fundamental quantities 0,
			1, <i>e</i>, π, and <code>0j1</code> and the basic
			operations plus, times, and exponentiation.
		</li>

		<li>
			It is a visual pun – the symbol looks like the cross-section
			of a felled tree, i.e. a log <a href="#ref4">[4]</a>.
		</li>
	</ul>

## Chronology

1962-03
:   In *A Programming Language* [\[5\]](#ref5), logarithm, exponential, and power were not assigned symbols.

1966-03
:   In <cite>Elementary Functions</cite>[\[6\]](#ref6), exponential and power were denoted `*y` and `x*y`, their definitions to this day. Natural logarithm was denoted `*'` and base-`x` logarithm was denoted `(x*)'`. (In the book, `f'` is the inverse of `f`.)

1966-11-27 15:53:58 (GMT-7)
:   Initial implementation of APL\\360 [\[7\]](#ref7).

1967-10-17
:   Natural logarithm was denoted by `⍟y` no later than the publication of *The APL\\360 Terminal System* [\[8\]](#ref8). The dyadic case `x⍟y`, base-`x` log of `y`, was undefined; instead, it was computed by a defined function in the public library workspace `1 utility` [\[9\]](#ref9).

1968-08
:   Finally, natural logarithm was denoted `⍟y` and the base-`x` logarithm of `y` was denoted `x⍟y`, their definitions to this day, no later than the publication of *APL\\360 User’s Manual* [\[10\]](#ref10).

## References

1. <a id="ref1"></a> Unicode Consortium, *Unicode Standard 6.2*, 2013  
    [www.unicode.org/charts/PDF/U2300.pdf](http://www.unicode.org/charts/PDF/U2300.pdf)
2. <a id="ref2"></a> Abramowitz, Milton, and Irene A. Stegun, *Handbook of Mathematical Functions*, US National Bureau of Standards, 1964; Chapter 4  
    [people.math.sfu.ca/~cbm/aands/page\_67.htm](http://people.math.sfu.ca/~cbm/aands/page_67.htm),
3. <a id="ref3"></a> Hui, Roger K.W., *Euler’s Identity*, J Wiki Essay, 2010-02-04  
    [www.jsoftware.com/jwiki/Essays/Euler's\_Identity](http://www.jsoftware.com/jwiki/Essays/Euler's_Identity)
4. <a id="ref4"></a> McDonnell, Eugene E., *The Story of `○`*, APL Quote-Quad,  
    Volume 8, Number 2, 1977-12 [www.jsoftware.com/papers/eem/storyofo.htm](http://www.jsoftware.com/papers/eem/storyofo.htm)
5. <a id="ref5"></a> Iverson, Kenneth E., *A Programming Language*, Wiley, New York, 1962  
    [www.jsoftware.com/papers/APL.htm](http://www.jsoftware.com/papers/APL.htm)
6. <a id="ref6"></a> Iverson, Kenneth E., *Elementary Functions: An Algorithmic Treatment*, Science Research Associates, Inc., Chicago, 1966-03  
    [www.jsoftware.com/jwiki/Doc/Elementary\_Functions\_An\_Algorithmic\_Treatment](http://www.jsoftware.com/jwiki/Doc/Elementary_Functions_An_Algorithmic_Treatment)
7. <a id="ref7"></a> Hui, Roger K.W., (ed.), *APL Quotations and Anecdotes*, 2010-09-18  
    [www.jsoftware.com/papers/APLQA.htm#APL\_birthday](http://www.jsoftware.com/papers/APLQA.htm#APL_birthday)
8. <a id="ref8"></a> Falkoff, Adin D. & Kenneth E. Iverson, *The APL\\360 Terminal System*, Report RC-1922, IBM, 1967-10-16  
    [www.jsoftware.com/papers/APL360TerminalSystem.htm](http://www.jsoftware.com/papers/APL360TerminalSystem.htm)
9. <a id="ref9"></a> Conroy, C.A., Editor, *APL\\360 Newsletter Number 1*, IBM, 1967-07  
    [bitsavers.informatik.uni-stuttgart.de/pdf/ibm/apl/APL\_Newsletter\_1\_Jul67.pdf](http://bitsavers.informatik.uni-stuttgart.de/pdf/ibm/apl/APL_Newsletter_1_Jul67.pdf)
10. <a id="ref10"></a> Falkoff, Adin D. & Kenneth E. Iverson, *APL\\360 User’s Manual*, IBM,  
    1968-08; Table 3.2 [bitsavers.informatik.uni-stuttgart.de/pdf/ibm/apl/APL\_360\_Users\_Manual\_Aug68.pdf](http://bitsavers.informatik.uni-stuttgart.de/pdf/ibm/apl/APL_360_Users_Manual_Aug68.pdf)
