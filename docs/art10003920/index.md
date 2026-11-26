---
vid: '10003920'
title: A Lifetime of Working with Ken
authors:
- Roger Hui
byline: from Roger Hui
volume: '22'
issue: '3'
page: '94'
source: trad/v223/hui222.htm
converter: vecrescue 0.1.0
issue_tab: n3-august-2006
tags:
- APL in perspective
- language design
---

from Roger Hui
{ .byline }

I am indebted to Chris Burke, Eric Iverson, Eugene McDonnell, Donald McIntyre, Roland Pesch, Joey Tuttle, and Arthur Whitney for reading earlier drafts of the paper.

<a id="sec0"></a>

## 0. Beginnings

I first met Ken Iverson early in 1981 in Toronto, when he invited me to dinner, along with Mrs. Jean Iverson, and Eugene McDonnell, Lib Gibson, David Keith, and Jane Minett of I.P. Sharp Associates. I believe the dinner happened because Eugene mentioned me to Ken, mainly due to *The N Queens Problem* paper [\[0\]](#0) I had submitted to the Recreational APL column that he wrote and edited.

So now not only was I to meet the great Ken Iverson, inventor of APL, but I was also to get a free dinner. The latter was a consideration: at the time I was a graduate student at the University of Toronto. At the appointed hour I went up to Ken’s apartment on the 47th floor of the Manulife Center. I was so in awe of the great man that I took scant notice of the spectacular view – from the apartment one can see Niagara Falls, a 90 minute drive away – until Ken directed me to do so.

We then went for dinner at a restaurant on King Street. I don’t remember much about the dinner, except for the way Ken left the tip. The change came back from paying the dinner check, in the form of the dingiest looking bills you ever saw. Ken took out his wallet, extracted some crisp clean bills, and crumbled them up before leaving them on the tray.

In another sense, I had “met” Ken Iverson years before, when I learned APL during my undergraduate studies at the University of Alberta, from reading the *APL\\360 User’s Manual* [\[1\]](#1) and from working at the computer terminal. Since I did not have direct access to Ken, I read his papers carefully, principally *The Design of APL* [\[2\]](#2), *The Evolution of APL* [\[3\]](#3), and *Notation as a Tool of Thought* [\[4\]](#4). I also further acquainted myself with APL during the summers of 1975 and 1976, when Lib Gibson hired me to work as a summer student at I.P. Sharp in Calgary.

<a id="sec1"></a>

## 1. From APL to J

The late 70’s and early 80’s were an exciting time for APL. APL was a commercial success. APL was taught in schools and universities. APL conferences were well-attended. (I attended my first APL conference in 1979 in Rochester, and my recollected impression is that the parallel sessions at the conference each had audiences of over a hundred.)

Ken had published his seminal paper *Operators and Functions* [\[5\]](#5) in April 1978. He went from IBM to I.P. Sharp in Toronto in 1980, and there collaborated with Arthur Whitney on *Practical Uses of a Model of APL* [\[6\]](#6) in 1981-82, leading to *Rationalized APL* [\[7\]](#7) in January 1983, multiple editions of *A Dictionary of the APL Language* between 1984 and 1987, and *A Dictionary of APL* [\[8\]](#8) in September 1987. Within I.P. Sharp, the phrase “dictionary APL” came into use to denote the APL specified by *A Dictionary of APL*, itself referred to as “the dictionary”.

At that time, the main APL vendors were IBM, STSC, and I.P. Sharp, and all were active in developing and extending the language. IBM had APL2, based on the work of Trenchard More and Jim Brown \[[9](#9), [10](#10), [11](#11)\]. Work on APL2 proceeded intermittently for 15 years [\[12\]](#12), with actual coding starting in 1971 and APL2 becoming available as an IUP in 1982. STSC had an experimental APL implementation called NARS [\[13\]](#13). NARS and APL2 differed in fundamental respects from dictionary APL (and differed from each other).

I.P. Sharp implemented the new APL ideas in stages, complex numbers, enclosed (boxed) arrays, match, and composition operators (on, over, under) in 1981 \[[14](#14), [15](#15)\], determinant in 1982 [\[16\]](#16), and the rank operator, lev (left), dex (right), and link in 1983 [\[17\]](#17). However, the domains of operators were restricted to the primitive functions or subsets thereof. I.P. Sharp also had SHARP APL/HP [\[18\]](#18), principally the work of Arthur Whitney with the assistance of Rob Hodgkinson.

An important stepping stone from APL to J was SAX [\[19\]](#19), SHARP APL/Unix, written in C and based on an implementation by STSC. An alpha version of SAX became available within I.P. Sharp around December 1986 or early 1987. From that time to August 1989, when the first J source line was written, I had access to SAX, and in the later part of that period used SAX on a daily basis.

SAX implemented dictionary APL in part, and was upward-compatible with mainframe SHARP APL in order to run applications ported from there. It permitted derived or user-defined functions as arguments to operators, and had the special APL characters, workspaces, and the del-form function definition. However, it did not permit functions and operators to be named using the assignment arrow, as in the phrase <span class="APLU">sum←+/</span>, precluding what later came to be called tacit definition. (The dictionary specified such assignment.) It also did not have hooks and forks [\[20\]](#20), for those were not yet invented. For Ken, an important drawback to SAX was that it was not widely and freely available.

From a personal perspective, another important stepping stone from APL to J was the writing of my APL87 paper *Some Uses of \{ and \}* [\[21\]](#21). At the time both Ken and I were working for I.P. Sharp in Toronto, and the ideas in the paper evolved over many conversations with Ken. Writing the paper refined and sharpened my understanding of dictionary APL.

Arthur Whitney, from collaborating with Ken on *Practical Uses of a Model of APL* [\[6\]](#6) in 1981-82, inventing the rank operator while on the train ride to the APL82 conference in Heidelberg in 1982 \[[22,](#22) [23](#23)\], and implementing SHARP APL/HP in 1986 [\[18\]](#18), went on to Morgan Stanley in 1988 and there invented A [\[24\]](#24). He later invented k [\[25\]](#25) in 1993 and q [\[26\]](#26) in 2003. (Arthur is a classmate from the University of Alberta; over the years our paths have crossed many times, and each time I had benefitted. But that is a story for another time.)

In the wider world, the PC revolution was in full swing. It was now possible to have a computer (for example, the AT&T 3B1 UNIX PC or the IBM PC/AT) at home that was a reasonable platform for developing an APL system. Not too long ago, such a platform would be under lock and key in a room with a raised floor, inaccessible to mere mortals and not readily available for experimentation.

The conditions were ripe for...

<a id="sec2"></a>

## 2. J

The story of how J began has been told in detail by Donald McIntyre [\[27\]](#27). Ken himself wrote in *A Personal View of APL* [\[28\]](#28):

> Work began in the summer of 1989 when I first discussed my desires with Arthur Whitney. He proposed the use of C for implementation, and produced (on one page and in one afternoon) a working fragment that provided only one function (+), one operator (/), one-letter names, and arrays limited to ranks 0 and 1, but did provide for boxed arrays and for the use of the copula for assigning names to any entity.
>
> I showed this fragment to others in the hope of interesting someone competent in both C and APL to take up the work, and soon recruited Roger Hui, who was attracted in part by the unusual style of C programming used by Arthur, a style that made heavy use of preprocessing facilities to permit writing further C in a distinctly APL style.
>
> Roger and I then began collaboration on the design and implementation of a dialect of APL (later named J by Roger), first deciding to roughly follow “A Dictionary of APL” and to impose no requirement of compatibility with any existing dialect. We were assisted by suggestions from many sources, particularly in the design of the spelling scheme (E.B. Iverson and A.T. Whitney) and in the treatment of cells, items, and formatting (A.T. Whitney, based on his work on SHARP APL/HP and on the dialect A reported at the APL89 conference in New York).
>
> E.E. McDonnell of Reuters provided C programs for the mathematical functions (which applied to complex numbers as well as to real), D.L. Orth of IBM ported the system to the IBM RISC System/6000 in time for the APL90 conference, and L.J. Dickey of the University of Waterloo provided assistance in porting the system to a number of other computers.

It’s funny, but my recollection is that at the time I thought *I recruited Ken*. Whoever recruited whom, I won a great prize when Ken decided he and I should work together.

The final impetus that got J started was the one-page interpreter fragment that Arthur wrote, recorded in Appendix A of *An Implementation of J* [\[29\]](#29) and also reproduced in [Appendix A](#atw) below. My immediate reaction on seeing the page was recoil and puzzlement: it looked nothing like any C code I had ever seen. (“Is it even C?”) However, Ken counselled that I should reserve judgment. As recounted in *An Implementation of J*, it then happened that:

> I studied this interpreter for about a week for its organization and programming style; and on Sunday, August 27, 1989, at about four o’clock in the afternoon, wrote the first line of code that became the implementation described in this document.

The name “J” was chosen a few minutes later, when it became necessary to save the interpreter source file for the first time.

I consciously designed and built the system around “Table 2: Parsing Process” in *A Dictionary of APL* [\[8\]](#8), which I had studied carefully and modelled at least two different ways between 1986 and 1989. The C data and program structures were designed so that the parse table in C corresponded as directly as possible to the parse table in the dictionary. I set as an objective of being able to show Ken the C source for the parser, and have him verify that the syntax being implemented was correct. We never did carry out the exercise (of Ken reading the C source), but I reckon I met my objective, because eventually Ken replaced the table in the dictionary with the parse table from the C source [\[30, section II E\]](#30). Other effects of trying to meet the objective were entirely beneficial.

Years after the initial implementation, I happened to re-read Ken’s APL87 paper *APL87* [\[31\]](#31). In retrospect, the paper, in five pages, prescribed all the essential steps in writing an APL interpreter, in particular the sections on word formation and parsing.

Ken and I had in mind to implement *A Dictionary of APL* [\[8\]](#8) together with hooks and forks (phrasal forms) [\[20\]](#20). For years, Ken had struggled to find a way to write<span class="APLU"> f+g </span>as in calculus, from the “scalar operators” in *Operators and Functions* [\[5, section 4\]](#5), through the “til” operator in *Practical Uses of a Model of APL* [\[6\]](#6) and *Rationalized APL* [\[7, p. 18\]](#7), and finally forks. Forks are defined as follows:

<table>
    <tr>
      <td class="APLU">   (f g h) y </td>
      <td>↔</td>
      <td class="APLU"> (f y) g (h y)</td>
    </tr>
    <tr>
      <td class="APLU"> x (f g h) y</td>
      <td>↔</td>
      <td class="APLU"> (x f y) g (x h y)</td>
    </tr>
  </table>

Moreover, <span class="APLU">(f g p q r</span><span class="APLU">)</span> ↔ <span class="APLU">(f g (p q r))</span>. Thus to write <span class="APLU">f+g</span> as in calculus, one writes<span class="APLU"> f+g </span>in J. Ken and Eugene McDonnell worked out the details on the long plane ride back from APL88 in Sydney, Australia, with Ken coming up with the initial idea on waking up from a nap.

The choice to implement forks was fortuitous and fortunate. We realized only later [\[32\]](#32) that forks made tacit expressions (operator expressions) complete in the following sense: any sentence involving one or two arguments that did not use its arguments as an argument to an operator, can be written tacitly with fork and<span class="APLU"> @: </span>(compose) and<span class="APLU"> \[ </span>(left) and<span class="APLU"> \] </span>(right) and constant functions. If<span class="APLU"> @: </span>were replaced by the equivalent special fork<span class="APLU"> \[: f g</span> ,<span class="APLU"> </span>then a sentence can be written as an unbroken train (sequence of forks).

Because explicit definition (the analogue of del-form definitions in APL) was relatively complex, it was not implemented until some months had elapsed. As well, the restriction in APL that only nouns (arrays) can be assigned never made it past the first day, for there was no way other than by assignment to name verbs, adverbs, and conjunctions (functions and operators). Therefore, for some months *all* verbs were tacit; that is, all functions were defined as operator expressions. These circumstances forced us to explore tacit definition extensively.

Meanwhile, Ken was concerned about the usefulness of forks, and worked hard at finding examples of forks beyond those in *Phrasal Forms* [\[20\]](#20). After a while, it seemed that *everything* was a fork. The explanation lies in the proof of completeness for tacit definition [\[32\]](#32): if the root (last) function in a sentence is applied dyadically, then a fork is required to write the sentence tacitly. Since we wrote tacit definitions exclusively, it was no wonder that we ran into forks constantly. As well, we had little trouble expressing computations tacitly because of the completeness.

Eric Iverson founded Iverson Software Inc. in February 1990 to provide an improved SHARP APL/PC product. It quickly became obvious that we had shared interests and goals, and in May 1990 Ken and I joined Iverson Software Inc. The company soon became J only. The name was changed to Jsoftware Inc. ([www.jsoftware.com](http://www.jsoftware.com)) in April 2000.

Eric adapted the session manager from SHARP APL/PC for J in time for the first release. Over the years, he was responsible for everything that was not the interpreter proper – the integrated development environment, the Windows interface, and all other interfaces – and made contributions to the language itself, including locatives, control structures, and memory-mapped files. Eric also handled product packaging and release details. Chris Burke joined us in early 1994 and wrote the application libraries, in particular the plot package, the debugger, the project manager, and the performance monitor.

Work proceeded rapidly from August 1989. The first public presentation took place on 1990-02-26 when Ken and I gave a talk complete with a live demo to the Toronto APLSIG at the Toronto Board of Trade. The first release of the software was at APL90 [\[33\]](#33) in Copenhagen in August 1990, when J was made available as shareware to the Software Exchange. (You can still get this version from various archives.) The first J conference was held on 1996-06-24 to -25 at the Koffler Institute of the University of Toronto. There were 123 attendees and 12 contributed papers in the proceedings [\[34\]](#34).

In working with Ken, I soon realized that Ken had a flexibility of mind that is breathtaking. Two examples illustrate this point.

Example 0: For the first few months, the special APL characters and the ASCII spelling co-existed in the system. It was Ken who first suggested that I should kill off the special APL characters. I myself resisted for a few weeks longer, until the situation became too confusing, for reasons described in *J for the APL Programmer* [\[35, p. 11\]](#35).

Example 1: It came time to implement<span class="APLU"> \\</span> (“scan”). On that day (in the first half of 1990), during lunch at Josie’s Cafe on Yonge Street, I asked Ken innocently, “Have you ever wanted to scan something other than reduction?” The question was not even well-formed, because the APL paradigm is so powerful that it usually pushes aside alternative thoughts. Nevertheless, the discussion that then proceeded was productive, and quickly led to the realization that [\[35, p. 16\]](#35):

> Some APL expressions apply reduction implicitly. For example, APL scan applies its left argument reduction to successive prefixes of the right argument. The use of reduction ensured that computations such as sum scan can be effected by primitive function arguments to the operator, and that the overall result could be assembled in APL\\360.
>
> With J’s more permissive assembly rules (and the use of boxed arrays), reduction is no longer necessary, and in many cases, inappropriate. Therefore in J, if a reduction is required, it must be specified. Thus<span class="APLU"> +/\\ </span>in J computes *sum scan* and<span class="APLU"> +/\\. </span>computes *suffix sum scan*. The following J example does not use reduction, and is therefore not readily expressed using APL scan:
>
> ```
>    <\ 'abcdef'
> +-+--+---+----+-----+------+
> |a|ab|abc|abcd|abcde|abcdef|
> +-+--+---+----+-----+------+
> ```

Much recent work involved “recognizing” phrases and implementing them with special code. The expressive power of tacit expressions makes this a fruitful approach. For example, if<span class="APLU"> f </span>is a primitive proposition, then:

<table>
<tr><td><span class="APLU">f i. 0:</span></td><td>   </td>
 <td>first place where not<span class="APLU"> x f y</span></td><td></td></tr>
<tr><td><span class="APLU">f i. 1:</span></td><td> </td>
 <td>first place where<span class="APLU"> x f y </span></td></tr>
<tr><td><span class="APLU">f i: 0:</span></td><td> </td>
 <td>last place where not<span class="APLU"> x f y </span></td></tr>
<tr><td><span class="APLU">f i: 1:</span></td><td> </td>
 <td>last place where<span class="APLU"> x f y </span></td></tr>
<tr><td><span class="APLU">+ /@:f</span></td><td> </td>
 <td>number of places where<span class="APLU"> x f y</span></td></tr>
<tr><td><span class="APLU">+./@:f</span></td><td> </td>
 <td><span class="APLU">x f y </span>anywhere?</td></tr>
<tr><td><span class="APLU">*./@:f</span></td><td> </td>
 <td><span class="APLU">x f y </span>everywhere?</td></tr>
<tr><td><span class="APLU">I. @:f</span></td><td> </td>
 <td>indices where<span class="APLU"> x f y</span></td></tr>
</table>

are supported by special code. A primitive proposition is one of the relational atomic verbs (scalar functions)<span class="APLU"> = ~: &lt; &lt;: &gt; &gt;: </span> or the verbs<span class="APLU"> E. </span>or<span class="APLU"> e. </span> (respectively, “substring match” and “member of”; a legend of the J words used in the text is available in [Appendix B](#jwords) below.) For example:

```
   'boustrophedonic' (e.i.1:) 'aeiou'     NB. index of first vowel
1
   'boustrophedonic' (e.i:1:) 'aeiou'     NB. index of last  vowel
13
```

If the target is found near the beginning of the search, the result is computed instantaneously (and in time independent of the lengths of the arguments for atomic verbs<span class="APLU"> f</span>). Even if the target is found only at the end of the search, the improvement in time is by a factor of two or more. In either case the space used is constant for atomic verbs<span class="APLU"> f</span> .<span class="APLU"> </span>

Another recent development is a port to the Linux and Windows XP 64-bit operating systems on the AMD64 and Intel EM64T processors [\[36\]](#36). J64 has 64-bit integers and a 64-bit address space.

Ken had started to write a companion to the Abramowitz and Stegun classic, *Handbook of Mathematical Functions* [\[37\]](#37). Such writing was the motivation behind the extended-precision integer and rational number facilities; to the same end, extended-precision floating-point numbers will be also added. The following examples illustrate extended-precision integers and rational numbers:

```
   (+%)/\ 10 $ 1
1 2 1.5 1.66667 1.6 1.625 1.61538 1.61905 1.61765 1.61818
   (+%)/\ 10 $ 1x
1 2 3r2 5r3 8r5 13r8 21r13 34r21 55r34 89r55
   0j40 ": (+%)/ 100 $ 1x
1.6180339887498948482045868343656381177203
```

The hook<span class="APLU"> x(+%)y </span>is<span class="APLU"> x </span>plus the reciprocal of<span class="APLU"> y</span> ,<span class="APLU"> </span> and so<span class="APLU"> (+%)/ </span>is the continued fraction. The first phrase computes convergents to the golden ratio phi in 64-bit IEEE floating-point numbers; the second computes rational convergents to phi; and the third computes phi to 40 decimal places.

<a id="sec3"></a>

## 3. Remembering Ken

![Ken Iverson](kenlast.jpg){ width="202" height="255" }

<a href="kei20040905.jpg"></a>The last photograph of Ken Iverson, with Rachel Hui, Jean Iverson, and Nicholas Hui, on Sunday, 2004-09-05, at the roof-top garden of the Manulife Center, Toronto, Canada. \[photo by Stella Hui\]

Ken and I worked on J in our homes in Toronto, I in my apartment in the High Park area and Ken in his apartment on Erskine Avenue and subsequently in the Manulife Center (a different apartment from the one in 1981, with an even more spectacular view). The arrangement placed me uniquely under Ken’s influence. Oftentimes I found myself at Ken’s house, delivering the latest J version or working on a computer, and many times Ken would invite me to stay for lunch or dinner, explaining that “thou shalt not muzzle the ox when he treadeth out the corn”. Little did I imagine in 1981 that the dinner I had with Ken and Jean then would be the first of many.

It is said that if you keep playing chess with a grandmaster, your skills can not help but improve. I had been in almost daily contact with the master since late 1986, and the following are a few of the things I have learned.

### Credit

When Ken was at Harvard a fellow student habitually stamped his papers “Copyright” or “Confidential”. Howard Aiken, head of the Comp Lab and Ken’s thesis supervisor, advised, “Don’t worry about people stealing your ideas. If it’s any good, you’d have to ram it down their throats!” (A less picturesque version of this story is found in [\[38, p. 240\]](#38).)

### Blame

Ken and I sometimes kidded each other about whose fault it was when something went wrong. He complained in jest that I as implementor had the attitude of “just tell me where to pour the concrete”. And I, in turn, complained in jest that he as designer had the attitude that he was never to blame: If it’s a mistake in the implementation, then it’s obviously my fault; and if it’s a mistake in the design, well then I should have caught it during the implementation.

In truth, I am profoundly grateful to Ken for not complaining in the times when I failed him, even when I failed him the same way more than once. I believe that if necessary, he would have forgiven me for seventy times seven times (although even I would probably catch on before too long).

### Secret to Success

In January 1992, I was in the final struggles of writing my book *An Implementation of J* [\[29\]](#29), having difficulty and under time pressure. Around that time, Ken was polishing off several books and papers, seemingly effortlessly. I asked him what his secret was. His reply was, basically, “First, write 500 papers.”

(Ken subsequently gave me some more immediately useful advice about writing.)

### Reading and Writing Carefully

In the early days I did not have direct access to Ken, and therefore read his papers carefully. Another example is Arthur Whitney’s [“one page thing”](#atw), which was the final thing I studied (for one week) before writing the first line of source code for J. Over the years, I have benefitted enormously from such careful reading.

I later realized that Ken wrote carefully, in the expectation that what he wrote would be read carefully. An exemplar of such writing is the description of “cut” in *Rationalized APL* [\[7, p. 19-20\]](#7). It annoyed Ken to no end when accused of being “too terse”, for example as in “the dictionary is too terse”, when “terse” means “effectively concise”.

### Words

As others have recounted, Ken was deeply interested in words, their use and their etymology. He indeed did read the dictionary, and kept a copy of the American Heritage Dictionary [\[39\]](#39) (along with other dictionaries) by his easy chair for ready reference. He especially encouraged me to consult the section on Indo-European roots in the back of the AHD, which makes deep and uncommon connections between words.

Many of Ken’s relatives and friends received from him the AHD as a present. I myself did not because I’d already owned one years before I met Ken. In fact, *I* gave *him* the third edition of the AHD as a present.

I think nothing I had ever done impressed Ken quite as much as when I found the word “rhematic” (meaning, pertaining to the formation of words), a word he had been searching for for some time. Thus the phrase “rhematic rules of J” made its way into the J dictionary [\[30\]](#30).

### Books

Before long, Ken and I discovered that we shared a love of books. We often recommended reading material to each other. A sample of our recommendations:

I to Ken: *The Language Instinct* by Steven Pinker; *Climbing Mount Improbable* by Richard Dawkins; *Lincoln at Gettysburg* by Garry Wills. Ken to me: *From Dawn to Decadence* by Jacques Barzun; [*Calvin and Hobbes*](http://www.ucomics.com/calvinandhobbes); [*The New York Review of Books*](http://www.nybooks.com).

And, of course, both of us had read *The Book of J* [\[40\]](#40).

### Politics

Unsurprisingly, Ken stood up for what he believed in. He once spent a few hours in jail after being arrested during a protest against the Vietnam War. In the same cell was Garry Wills, the historian and author.

### Respect for People

Ken liked to tell the story of the stoic service agent: At an airport counter an irate traveller was berating the service agent over something or other, which the agent took with stoic forbearance. After the traveller went on his way, the next person in line told the agent, “I am amazed at how well you took that abuse.” The agent smiled thinly and replied, “Oh, the gentleman is flying to Chicago, but his luggage is going to Moscow.”

Ken also suggested, “Be kind to someone trying to make a living.” I try to remember this every time a taxi or truck driver cuts me off on the road.

### A Questioning and Flexible Mind

The flexibility of mind that Ken brought to his work (noted above) was also demonstrated in his everyday life. For example, it astounded me how often and how thoroughly Ken rearranged the furniture in his home. His explanation was, our needs have changed, and we rearranged our surroundings to meet those needs.

<a id="ref"></a>

## References

<table>

<tr><td valign="top"><a name="0"></a>[0]</td><td> </td><td>
Hui, R.K.W., “The N Queens Problem”, <i>APL Quote-Quad</i>,
Volume 11, Number 3, 1981-03.</td></tr>

<tr><td valign="top"><a name="1"></a>[1]</td><td> </td><td>
Falkoff, A.D., and K.E. Iverson, <i>APL\360 User’s Manual</i>,
IBM Corporation, 1968-08.</td></tr>

<tr><td valign="top"><a name="2"></a>[2]</td><td> </td><td>
Falkoff, A.D., and K.E. Iverson, “The Design of APL”,
<i>IBM Journal of Research and Development</i>, Volume 17, Number 4, 1973-07.</td></tr>

<tr><td valign="top"><a name="3"></a>[3]</td><td> </td><td>
Falkoff, A.D., and K.E. Iverson, “The Evolution of APL”,
<i>ACM SIGPLAN Notices</i>, Volume 13, Number 8, 1978-08.</td></tr>

<tr><td valign="top"><a name="4"></a>[4]</td><td> </td><td>
Iverson, K.E., “Notation as a Tool of Thought”, <i>Communications
of the ACM</i>, Volume 23, Number 8, 1980-08.</td></tr>

<tr><td valign="top"><a name="5"></a>[5]</td><td> </td><td>
Iverson, K.E., <i>Operators and Functions</i>, Research Report #RC7091,
IBM Corporation, 1978-04-26.</td></tr>

<tr><td valign="top"><a name="6"></a>[6]</td><td> </td><td>
Iverson, K.E., and A.T. Whitney, “Practical Uses of a Model of APL”,
APL82, <i>APL Quote-Quad</i>, Volume 13, Number 1, 1982-09.</td></tr>

<tr><td valign="top"><a name="7"></a>[7]</td><td> </td><td>
Iverson, K.E., <i>Rationalized APL</i>, I.P. Sharp Associates,
1983-01-06.</td></tr>

<tr><td valign="top"><a name="8"></a>[8]</td><td> </td><td>
Iverson, K.E., “A Dictionary of APL”, <i>APL Quote-Quad</i>,
Volume 18, Number 1, 1987-09.</td></tr>

<tr><td valign="top"><a name="9"></a>[9]</td><td> </td><td>
Brown, J.A., “A Generalization of APL”,
Ph.D. Thesis, Department of Computer and Information Sciences,
Syracuse University, 1971.</td></tr>

<tr><td valign="top"><a name="10"></a>[10]</td><td> </td><td>
Brown, J.A., <i>The Principles of APL2</i>,
Technical Report 03.247, IBM Santa Teresa Laboratory, 1984.</td></tr>

<tr><td valign="top"><a name="11"></a>[11]</td><td> </td><td>
<i>APL2 Programming: Language Reference</i>,
SH20-9227, IBM Corporation, 1988.</td></tr>

<tr><td valign="top"><a name="12"></a>[12]</td><td> </td><td>
Falkoff, A.D., “The IBM Family of APL Systems”,
<i>IBM Systems Journal</i>, Volume 30, Number 4, 1991-12.</td></tr>

<tr><td valign="top"><a name="13"></a>[13]</td><td> </td><td>
Smith, R., “Nested Arrays, Operators, and Functions”,
APL81, <i>APL Quote-Quad</i>, Volume 21, Number 1, 1981-09.</td></tr>

<tr><td valign="top"><a name="14"></a>[14]</td><td> </td><td>
McDonnell, E.E., <i>Complex Numbers</i>,
SHARP APL Technical Notes 40,
I.P. Sharp Associates, 1981-06-20.</td></tr>

<tr><td valign="top"><a name="15"></a>[15]</td><td> </td><td>
Iverson, K.E., <i>Composition and Enclosure</i>,
SHARP APL Technical Notes 41,
I.P. Sharp Associates, 1981-06-20.</td></tr>

<tr><td valign="top"><a name="16"></a>[16]</td><td> </td><td>
Iverson, K.E., <i>Determinant-Like Functions Produced by the Dot-Operator</i>,
SHARP APL Technical Notes 42,
I.P. Sharp Associates, 1982-04-01.</td></tr>

<tr><td valign="top"><a name="17"></a>[17]</td><td> </td><td>
Bernecky, R., K.E. Iverson, E.E. McDonnell, R.C. Metzger, and J.H. Schueler,
<i>Language Extensions of May 1983</i>, SHARP APL Technical Notes 45,
I.P. Sharp Associates, 1983-05-02.</td></tr>

<tr><td valign="top"><a name="18"></a>[18]</td><td> </td><td>
Hodgkinson, R., “APL Procedures”, APL86,
<i>APL Quote-Quad</i>, Volume 16, Number 4, 1986-07.</td></tr>

<tr><td valign="top"><a name="19"></a>[19]</td><td> </td><td>
Steinbrook, D.H., <i>SAX Reference</i>,
I.P. Sharp Associates, 1986.</td></tr>

<tr><td valign="top"><a name="20"></a>[20]</td><td> </td><td>
Iverson, K.E., and E.E. McDonnell, “Phrasal Forms”, APL89, <i>APL Quote-Quad</i>,
Volume 19, Number 4, 1989-08.</td></tr>

<tr><td valign="top"><a name="21"></a>[21]</td><td> </td><td>
Hui, R.K.W., “Some Uses of { and }”, APL87, <i>APL Quote-Quad</i>,
Volume 17, Number 4, 1987-05.</td></tr>

<tr><td valign="top"><a name="22"></a>[22]</td><td> </td><td>
Whitney, A.T., private e-mail, 2004-11-11.</td></tr>

<tr><td valign="top"><a name="23"></a>[23]</td><td> </td><td>
Pesch, R.H., private e-mail, 2004-11-11.</td></tr>

<tr><td valign="top"><a name="24"></a>[24]</td><td> </td><td>
Whitney, A.T., “A”, plenary session, APL89, 1989-08.</td></tr>

<tr><td valign="top"><a name="25"></a>[25]</td><td> </td><td>
Orth, D.L., <i>K Reference Manual</i>, Kx Systems, 1998.</td></tr>

<tr><td valign="top"><a name="26"></a>[26]</td><td> </td><td>
Whitney, A.T., <i>Abridged Q Language Manual</i>,
Kx Systems, 2004.</td></tr>

<tr><td valign="top"><a name="27"></a>[27]</td><td> </td><td>
McIntyre, D.B., “A Tribute to Roger Hui”, <i>APL Quote-Quad</i>,
Volume 27, Number 1, 1996-09.</td></tr>

<tr><td valign="top"><a name="28"></a>[28]</td><td> </td><td>
Iverson, K.E., “A Personal View of APL”,
<i>IBM Systems Journal</i>, Volume 30, Number 4, 1991-12.</td></tr>

<tr><td valign="top"><a name="29"></a>[29]</td><td> </td><td>
Hui, R.K.W., <i>An Implementation of J</i>, Iverson Software Inc.,
1992-01-27.</td></tr>

<tr><td valign="top"><a name="30"></a>[30]</td><td> </td><td>
Hui, R.K.W., and K.E. Iverson,
<i>J Introduction and Dictionary</i>, Jsoftware Inc., 2004.</td></tr>

<tr><td valign="top"><a name="31"></a>[31]</td><td> </td><td>
Iverson, K.E., “APL87”, APL87, <i>APL Quote-Quad</i>,
Volume 17, Number 4, 1987-05.</td></tr>

<tr><td valign="top"><a name="32"></a>[32]</td><td> </td><td>
Hui, R.K.W., K.E. Iverson, and E.E. McDonnell, “Tacit Definition”,
APL91, <i>APL Quote-Quad</i>, Volume 21, Number 4, 1991-08.</td></tr>

<tr><td valign="top"><a name="33"></a>[33]</td><td> </td><td>
Hui, R.K.W., K.E. Iverson, E.E. McDonnell, and A.T. Whitney,
“APL\?”, APL90, <i>APL Quote-Quad</i>, Volume 20, Number 4, 1990-07.</td></tr>

<tr><td valign="top"><a name="34"></a>[34]</td><td> </td><td>
<i>J User Conference Proceedings</i>, Iverson Software Inc.,
1990-06-24.</td></tr>

<tr><td valign="top"><a name="35"></a>[35]</td><td> </td><td>
Burke, C., and R.K.W. Hui, “J for the APL Programmer”,
<i>APL Quote-Quad</i>, Volume 27, Number 1, 1996-09.</td></tr>

<tr><td valign="top"><a name="36"></a>[36]</td><td> </td><td>
Iverson, E.B., “J64 Beta Available”,
J Forum message, 2004-07-30.</td></tr>

<tr><td valign="top"><a name="37"></a>[37]</td><td> </td><td>
Abramowitz, M., and I.A. Stegun, <i>Handbook of Mathematical Functions</i>,
National Bureau of Standards, 1964-06.</td></tr>

<tr><td valign="top"><a name="38"></a>[38]</td><td> </td><td>
Cohen, I.B., <i>Howard Aiken: Portrait of a Computer Pioneer</i>, MIT Press, 1999.</td></tr>

<tr><td valign="top"><a name="39"></a>[39]</td><td> </td><td>
<i>The American Heritage Dictionary of the English Language</i>,
Third Edition, Houghton Mifflin Company, 1982.</td></tr>

<tr><td valign="top"><a name="40"></a>[40]</td><td> </td><td>
Bloom, H., and D. Rosenberg, <i>The Book of J</i>,
Vantage Books, 1990.</td></tr>

</table>

***

<a id="atw"></a>

## Appendix A. Incunabulum

From *An Implementation of J* [\[29\]](#29)

One summer weekend in 1989, Arthur Whitney visited Ken Iverson at Kiln Farm and produced – on one page and in one afternoon – an interpreter fragment on the AT&T 3B1 computer. I studied this interpreter for about a week for its organization and programming style; and on Sunday, August 27, 1989, at about four o’clock in the afternoon, wrote the first line of code that became the implementation described in this document.

Arthur’s one-page interpreter fragment is as follows:

```
typedef char C;typedef long I;
typedef struct a{I t,r,d[3],p[2];}*A;
#define P printf
#define R return
#define V1(f) A f(w)A w;
#define V2(f) A f(a,w)A a,w;
#define DO(n,x) {I i=0,_n=(n);for(;i<_n;++i){x;}}
I *ma(n){R(I*)malloc(n*4);}mv(d,s,n)I *d,*s;{DO(n,d[i]=s[i]);}
tr(r,d)I *d;{I z=1;DO(r,z=z*d[i]);R z;}
A ga(t,r,d)I *d;{A z=(A)ma(5+tr(r,d));z->t=t,z->r=r,mv(z->d,d,r);
 R z;}
V1(iota){I n=*w->p;A z=ga(0,1,&n);DO(n,z->p[i]=i);R z;}
V2(plus){I r=w->r,*d=w->d,n=tr(r,d);A z=ga(0,r,d);
 DO(n,z->p[i]=a->p[i]+w->p[i]);R z;}
V2(from){I r=w->r-1,*d=w->d+1,n=tr(r,d);
 A z=ga(w->t,r,d);mv(z->p,w->p+(n**a->p),n);R z;}
V1(box){A z=ga(1,0,0);*z->p=(I)w;R z;}
V2(cat){I an=tr(a->r,a->d),wn=tr(w->r,w->d),n=an+wn;
 A z=ga(w->t,1,&n);mv(z->p,a->p,an);mv(z->p+an,w->p,wn);R z;}
V2(find){}
V2(rsh){I r=a->r?*a->d:1,n=tr(r,a->p),wn=tr(w->r,w->d);
 A z=ga(w->t,r,a->p);mv(z->p,w->p,wn=n>wn?wn:n);
 if(n-=wn)mv(z->p+wn,z->p,n);R z;}
V1(sha){A z=ga(0,1,&w->r);mv(z->p,w->d,w->r);R z;}
V1(id){R w;}V1(size){A z=ga(0,0,0);*z->p=w->r?*w->d:1;R z;}
pi(i){P("%d ",i);}nl(){P("\n");}
pr(w)A w;{I r=w->r,*d=w->d,n=tr(r,d);DO(r,pi(d[i]));nl();
 if(w->t)DO(n,P("< ");pr(w->p[i]))else DO(n,pi(w->p[i]));nl();}

C vt[]="+{~<#,";
A(*vd[])()={0,plus,from,find,0,rsh,cat},
 (*vm[])()={0,id,size,iota,box,sha,0};
I st[26]; qp(a){R  a>='a'&&a<='z';}qv(a){R a<'a';}
A ex(e)I *e;{I a=*e;
 if(qp(a)){if(e[1]=='=')R st[a-'a']=ex(e+2);a= st[ a-'a'];}
 R qv(a)?(*vm[a])(ex(e+1)):e[1]?(*vd[e[1]])(a,ex(e+2)):(A)a;}
noun(c){A z;if(c<'0'||c>'9')R 0;z=ga(0,0,0);*z->p=c-'0';R z;}
verb(c){I i=0;for(;vt[i];)if(vt[i++]==c)R i;R 0;}
I *wd(s)C *s;{I a,n=strlen(s),*e=ma(n+1);C c;
 DO(n,e[i]=(a=noun(c=s[i]))?a:(a=verb(c))?a:c);e[n]=0;R e;}

main(){C s[99];while(gets(s))pr(ex(wd(s)));}
```

***

<a id="jwords"></a>

## Appendix B. J words used in the text

Full reference in *J Introduction and Dictionary* [\[30\]](#30).

<table>
<tr><td valign="top"><table>
<tr><td><span class="APLU">=</span></td><td> </td><td>equal</td></tr>
<tr><td><span class="APLU">&lt;</span></td><td> </td><td>box • less than</td></tr>
<tr><td><span class="APLU">&lt;:</span></td><td> </td><td>less than or equal</td></tr>
<tr><td><span class="APLU">&gt;</span></td><td> </td><td>greater than</td></tr>
<tr><td><span class="APLU">&gt;:</span></td><td> </td><td>greater than or equal</td></tr>
<tr><td><span class="APLU">+</span></td><td> </td><td>plus</td></tr>
<tr><td><span class="APLU">+.</span></td><td> </td><td>or</td></tr>
<tr><td><span class="APLU">*.</span></td><td> </td><td>and</td></tr>
<tr><td><span class="APLU">%</span></td><td> </td><td>reciprocal</td></tr>
<tr><td><span class="APLU">$</span></td><td> </td><td>reshape</td></tr>
<tr><td><span class="APLU">~:</span></td><td> </td><td>not equal</td></tr>
<tr><td><span class="APLU">/</span></td><td> </td><td>insert (reduce)</td></tr>
<tr><td><span class="APLU">\</span></td><td> </td><td>prefix (scan)</td></tr>
<tr><td><span class="APLU">\. </span></td><td> </td><td>suffix</td></tr>
</table></td>
<td width="88"> </td>
<td valign="top"><table>
<tr><td><span class="APLU">[</span></td><td> </td><td>left (the left argument)</td></tr>
<tr><td><span class="APLU">[:</span></td><td> </td><td>cap (special fork)</td></tr>
<tr><td><span class="APLU">]</span></td><td> </td><td>right (the right argument)</td></tr>
<tr><td><span class="APLU">":</span></td><td> </td><td>format (thorn)</td></tr>
<tr><td><span class="APLU">@:</span></td><td> </td><td>at (compose)</td></tr>
<tr><td><span class="APLU">E.</span></td><td> </td><td>member of interval (substring search mask)</td></tr>
<tr><td><span class="APLU">e.</span></td><td> </td><td>member of</td></tr>
<tr><td><span class="APLU">I.</span></td><td> </td><td>indices (from boolean)</td></tr>
<tr><td><span class="APLU">i.</span></td><td> </td><td>index of</td></tr>
<tr><td><span class="APLU">i:</span></td><td> </td><td>index of last</td></tr>
<tr><td><span class="APLU">0:</span></td><td> </td><td>constant function 0</td></tr>
<tr><td><span class="APLU">1: </span></td><td> </td><td>constant function 1</td></tr>
</table></td></tr>
</table>
