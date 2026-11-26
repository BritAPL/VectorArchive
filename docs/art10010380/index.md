---
vid: '10010380'
title: 'J-ottings 24: One Foot in the Grade'
authors:
- Norman Thomson
volume: '16'
issue: '4'
page: '34'
status: not online
issue_tab: n4-april-2000
---

Norman Thomson
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 34](../16/4/VOL.16-NO.4-APRIL-2000.pdf#page=36)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.16 No.4</p>
<p>J-ottings 24: One Foot in the Grade by Norman Thomson</p>
<p>A Vectorobituary preview column? Well not quite, or maybe the answer should be, of a sort, since that is what grade (used generically to refer to both the grade- up and grade-down verbs) is all about. Sorting a list (the equivalent of APL VLAV]) has a direct J equivalentin the following bridge hook : este 274 1247</p>
<p>The algorithm applies equally to characterlists:</p>
<p>({-/1)&gt;'Pred';'Joe'; Egbert! Egbert Fred Joe</p>
<p>For convenience give names to the verbs which sortlists up and down:</p>
<p>sortus: {-/: sortd=: {-\: sortd&gt;'Fred';'Joe';'Egbert' Joe Fred Egbert</p>
<p>Sort by rows and sort within rows are differentiated by using the rank conjunction : 23 4$10 4 2 6 ctu u)s;sortu"i u</p>
<p>sae won IO NEO Ono ous ana Oa</p>
<p>For sort by columns do a rowsort under (&amp;.) the transformation |: (transpose):</p>
<p>ortué. |}: u</p>
<p>VECTOR Vol.16 No.4</p>
<p>Flexibility in the choice of collating sequence (say all the odd numbersare to be prior to any of the evens)is achieved by using dyadic i. : oe=.1 3570 2 (/ioe t.v) {v= a 1724</p>
<p>Consolidate this in a verb:</p>
<p>csortu=,/:@i.{] NB. x. is collating seq. ee=.1 3579024 68 oe csortu 4 27 1 1724</p>
<p>Hereis a collating sequence which blurs the distinction between upper and lower case characters: , each=. &amp;&gt; Jes=.(,|]:65 97+each &lt;i.26){a. AaBbCcDdEeF fGgHhIid jKKLIMMNnNOoPpQqRrSsTtUuVvWwXxYyZz</p>
<p>m=,&gt;'bless';'This ';'house' (]isortu;csaécsortu)m</p>
<p>bless|This |bless This |bless|house house|house| This</p>
<p>Dyadic Grade Whereas dyadic grade in APL provides a means of changing the collating sequence, this is not the case in J where the following rule for dyadic grade applies :</p>
<p>x./:y. isequivalentto (/: y.){x.</p>
<p>so that x. mustbe atleast as long as y. . A special case satisfying this constraint is when x. and y. are equal, in which case y. /: y. (or equivalently /:~y.) is simply anotherdefinition of sortu. The definitions of sortu and sortd can therefore be shortened by onecharacterto</p>
<p>35</p>
<p>VECTOR Vol.16 No.4</p>
<p>Sorting by columns of a matrix other than thefirst requires dyadic grade. The following display demonstrates sorting on the 2nd and 4th columns. colsort=,] /: {"4 (];14colsort;3&amp;colsort)u</p>
<p>506 4%/5 06 4/7 972 7:97 21/073 65 06 4 0736/7 97 2/0736</p>
<p>It is worth while pausing to consider how this fork works, The from ({) at rank 1 onthe rightof the fork selects the chosen column as y.. ] selects the entire table as x.. Now apply the dyadic grade rule above. The indices for ordering the chosen column are obtained as /: y. and are used to select from all the columns in the table.</p>
<p>When items within matrices have complex structures as in data base tables the principle remains unchanged but some minor variations may be necessary involving the use of open (&gt;) and @: to take care of rank inheritance: selectc r&gt;@{ra NB. select x.th column of y. sortcs: :@:selecte { ] NB. order y. by column x.</p>
<p>dbase</p>
<p>Bristol England {400000</p>
<p>Aberdeen |Scotland| 200000</p>
<p>Edinburgh |Scotland|500000</p>
<p>York England |100000</p>
<p>(1&amp;4sortc,,2&amp;sortc) dbase NB. sort by country, then population</p>
<p>Bristol England |400000/ York England |100000</p>
<p>York England |100000/Aberdeen |Scotland|200000</p>
<p>Aberdeen |Scotland|200000/Bristol England /|400000</p>
<p>Edinburghj Scotland|500000|£Edinburgh | Scotland| 500000</p>
<p>36</p>
<p>VECTOR Vol.16 No.4</p>
<p>Ranking The potential confusion between rank in the J sense (as in rank conjunction) and rank in the sense of collating sequence ordering can be avoided by referring to the latter as ranking.</p>
<p>Tt might be expected that, by analogy with APL, /: /: v delivers the upward ranking of the elements of v_ . AYIA 7244</p>
<p>Whydoes APL intuition fail? The reason is that /: /: is a hook. The rightmost 7: obtains the upward ordering index vector of v which is 3 1 0 2, which, in the absenceof an explicit left argumentis both left and right argumentto the leftmost dyadic /+ . Followingthe rule above, a second gradeup is performed resulting in the intermediate generation of the rank vector 2 1 3 0 which is then applied as a selection vector (from) on the original right argumentleading to the result 7 21 4. Toblockthis last step it is necessary to use a different composition mechanism for the two /: s, viz</p>
<p>(rkusi/:@/:)4 2 7 4 2130 Downward ranking uses both grade up and grade down:</p>
<p>(rkd=:/:@\: 4 27 4 1204 Upward and downward tied rankings require slightly unwieldy verb combinations : vktu=.-1@(rku + \:@/:@|.) rktd=,-:@(ckd + \:@\:@1.) rktu vie.§ 33525 8 1.5 1.5 4% 04 6 rktd vi 24.5 4.5 26 2 0</p>
<p>A variation of rktd is schoolmaster s rank in which students with equal scores are each rated as highly as possible, a property inherent in dyadic i. . The work for the schoolmaster s ranking verb has largely been done and it can be achieved by a single fork :</p>
<p>sche.i.~{rkd &gt;rsch vi 2552721</p>
<p>37</p>
<p>VECTOR Vol.16 No.4</p>
<p>The grade verbs have applications outside the realm ofstrict sorting. In J-ottings 23 grade up was used to perform inverse permutations, that to reverse the effects of a shuffle. In technical terms, when the argument y. is a permutation, that is an arrangementofall the items in i. n where n is a positive integer, then /: isa self-inverse verb, and so in these circumstances /: /: y. isidenticalto y. -</p>
<p>Then in J-ottings 22 it transpired that the application of grade verbs to permutations matched exactly the transformations of rotations and reflections about axes in the plane applied to matrices which provide alternative representations of: the permutation in the sense that, for example, 1 0 2 3 is represented by</p>
<p>wld. wl</p>
<p>Specifically /: is a reflection in the line x+y=0 but \: is a clockwise rotation through 90 degrees. The reflection/rotation distinction underlines the non- symmetrical behaviour of /: and \: in the ranking algorithms given above. 7:@\: and /:@\: represent reflections in the x and y-axes, \: @\: @\: represents an anti-clockwise rotation through 90 degrees and \:@\: represents a half-turn.Finally /: @\: @\: represents a reflection in the line x=y.</p>
<p>Orderstatistics, such as median, require the use of grade. A median algorithm can be built up using three auxiliary verbs:</p>
<p>mindex=.-:@&lt;:@# NB. 0.5*(n-1) where n is length of vector y minmax=:&lt;. , &gt;. NB. Floor,ceiling of real number y. mean=.+/%# NB. Arithmetic mean</p>
<p>Now introduce sortu as the right prong of a fork, and use mean to average the values of the two (possibly identical) middle values:</p>
<p>median=:mean @ (minmax@mindex) { sortu median 4 2714 NB. mean of two middle values 2 and 4 3</p>
<p>Verbs defining other partition values, e.g. percentiles, use the same principle, although inevitably are more complex in detail.</p>
<p>In short grade is a very versatile verb for which you should rightly have grade expectations.</p>
<p>Havea grade day!</p>
<p>38</p>
</details>
