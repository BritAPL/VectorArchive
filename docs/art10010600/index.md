---
vid: '10010600'
title: 'J-ottings 47: RU14 txt msgs? pls 1!:1 on'
authors:
- Norman Thomson
byline: by Norman Thomson
volume: '22'
issue: '2'
page: '131'
source: trad/v222/jot47.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- language design
- education
---

by Norman Thomson
{ .byline }

APL used to be described by its deriders as a system of hieroglyphics. Then around 1990 or so J was unveiled which used only ordinary keyboard symbols to achieve an even greater degree of shorthand without hieroglyphics, making it surely the earliest form of text messaging. Now that text messaging is commonplace, could J become more widespread if presented as a scientific text messaging *lingua franca*?

Of course for the newcomer, the J vocabulary in its entirety is far too great to be accepted in a single bite, or even many great swallows. But it could make sense to think in terms of a basic acceptable subset, leading to a gradual infusion of more advanced shorthands.

The arithmetic and logical verbs

```
           + -  *  %  ^  ! <  >  = >:  <:  ~:
```

pose no great problems since these symbols, or symbols like them, are already widely used in, say, e-mails, where no mathematical symbols are possible. The only unfamiliar form is monadic \* meaning signum.

The symbols derived from the above for double, halve, square and square root, together with increment and decrement, should be easily accommodated, viz.

```
           +:  -:  *:  %:  <:  >:
```

as should the di-grams <span class="APLU">+/</span> and <span class="APLU">\*/</span> for sum and product of lists (S and P in maths terms).

Series, that is lists, are an essential part of the discourse of mathematics, and operations to join (<span class="APLU">,</span>), tally (<span class="APLU">#</span>) and reverse (<span class="APLU">|.</span>) are a necessary element of any programming language which admits strings. Head/behead and tail/curtail are desirable additions to the basic set, albeit a degree of memorisation/indoctrination is required before it becomes ingrained that <span class="APLU">\{</span> means ‘take one’,  <span class="APLU">\}</span> means ‘take all but one’, dot means ‘first’ and colon means ‘last’.

Intelligent texting requires some appreciation of the special parsing rules of J, in particular the concepts of *hook* and *fork*, which lead to understanding why <span class="APLU">(+/%#)</span><i>v</i> is ‘mean of *v*’ and <span class="APLU">(\*&gt;:)<i></i></span><i>n</i> is *n(n+1)*. Also <span class="APLU">@</span> denoting ‘after’ is needed so that <span class="APLU">(-:@(\*&gt;:))</span><i>n</i> means ‘halve after *n* times *(n+1)* ’. The rule in text messaging terms is that <span class="APLU">@</span> stops on meeting the first verb to its right, so that <span class="APLU">(-:@\*&gt;:)</span><i>n</i> means ‘semi-multiply *n* by *n+1*’, giving the same result as before but by a subtly different route. As another example <span class="APLU">\*@+/%#</span> means <span class="APLU">(\*@+/)%#</span>, that is a single fork articulated as ‘(signum of sigma) divided by tally’, as opposed to <span class="APLU">\*@(+/%#)</span>, that is ‘signum after mean’, which is not the same thing:

```
   (*@+/%#)1 2 3            NB. (signum of 1+2+3) divided by 3
0.333333
   (*@(+/%#))1 2 3          NB. signum of mean of 1 2 3
1
```

Of course if a J interpreter is available grammar verification is always to hand!

These ideas can be tested on the formula quoted in Gene MacDonnell’s article “Metlov’s Triumph” in Vector [Vol. 21 no. 4](trad/v214/metlov.htm). The core problem involved counting numbers of triangles within triangles, which leads to the formula

```
           * -: @ * +
```

The rule that the right scope of <span class="APLU">@</span> is a single verb means that the unambiguous interpretation, parenthesised for clarity, is the fork

```
           * (-: @ *) +  
```

meaning ‘semi-multiply the sum by the product’. The same six symbols could be written in a different order

```
           -: @ * * +
```

meaning <span class="APLU">(-: @ \*) \* +</span> that is ‘semi-product multiplied by sum’, a different computation but leading to the same result.

A symbol for ‘repeat’ is a valuable text messaging addition. Suppose that ‘joddling’, that is ‘joining double the last’, turns up as an intermediate building block in a bigger application. This idea is expressed comprehensibly and unambiguously as

```
   jdl=.,+:@{:          NB. the verb ‘to joddle’
   jdl 1 3 6
1 3 6 12
```

The idea of the power conjunction as a shorthand for ‘repeat’ falls naturally into place:  

```
   jdl^:3(1 3 6)  N     NB. joddle three times
1 3 6 12 24 48
```

One of the most frequent ways in which lists turn up in mathematics is as coefficients of polynomials. <span class="APLU">#.</span> is a convenient shorthand for ‘polynomial evaluation’, so

```
   4 #.1 2 3            NB. coeffs. in descending power order
27
```

means evaluate the polynomial x<sup>2
</sup>+ 2x + 3<sup>    </sup>when x=4.

To convey the idea that a polynomial is a single entity which is represented by a coefficient list, ‘box’ it (<span class="APLU">&lt;</span>), and use the reverse shorthand ‘open’ (<span class="APLU">&gt;</span>) to bring the boxed polynomial back to coefficient list form. At the same time define <span class="APLU">&amp;&gt;</span> as a shorthand (or arguably a longhand!) for each – the intricacies of how it works do not require explanation at the text messaging level. Thus

```
   _1 0 1 2 #.each <1 2 3     NB. multiple evaluations of x^2+2x+3
2 3 6 11
```

The sum of the first four values of the series *u<sub>n</sub>=* <i>1
\+ 2n + 3n<sup>2</sup></i><sup>  </sup>is then 

```
   +/1 2 3 4 #.each <1 2 3    NB. +/ is sigma
62
```

A more substantial example of using text messaging techniques is to explain the terms “internal rate of return” (IRR) and “annual percentage return” (APR). The phenomenon of 5% compound interest growth can be described equivalently by any one of four numbers namely 5, 0.05, 1.05  and (1.05)<sup>-1</sup>. These differences are expressible in J terms as:

```
   p=.percentage rate                         (p>_100)
   r=.fractional rate  so r=.0.01*p           (r>_1)
   k=.multiplication factor so k=.>:r         (k>0)
   d=.discount factor so d=.%k or %>:0.01*p   (d>0)
```

To be meaningful, values must be in the ranges indicated. Negative growth, that is decay, is indicated by <span class="APLU">p &lt;0</span>,  <span class="APLU">r &lt;0</span>, k &lt;1 and <span class="APLU">d &gt;1</span>. Conversion from <span class="APLU">p</span> to <span class="APLU">d</span> is described above and is consolidated as

```
   ptod=.%@>:@(0.01&*)
   ptod 20
0.83333
```

The reverse operation <span class="APLU">d</span> to <span class="APLU">p</span> is given by <span class="APLU">dtop=.100&amp;\*@&lt;:@%</span> . Both <span class="APLU">ptod</span> and <span class="APLU">dtop</span> require the use of the conjunction <span class="APLU">&amp;</span> to specify the particular multiplication involved.

The underlying concept of IRR is based on the “worth” of a project, defined as the final return reckoned in present money. This is expressed by

```
   worth=.+/is*d^i.#is
```

where <span class="APLU">is</span> is an income stream with values after the first assumed positive, and <span class="APLU">d</span> stands for the discounted value of money. The IRR is obtained by taking worth = zero, that is by solving the polynomial <span class="APLU">0=d #.is</span>  . Thus for a project with an income stream<span class="APLU"> \_100 0 0
200</span>, that is an initial investment (outflow) of 100 in year 0, with a return of 200 in year 3 and no intervening inflows, the initial investment has increased by a multiple of 2. The cube root of 2 (<span class="APLU">3%:2</span> in shorthand) is 1.26, and so the IRR is 26%.

The significance of “internal” is that equating to zero creates a system in which ultimate worth is neither increased nor decreased for the solved value of <span class="APLU">d</span>, which is therefore that rate of continuous money decline which would need to occur to make the overall project do no more than break even. If the *actual* rate of money decline exceeds the IRR, then it would have been better not to have undertaken the project. IRRs should in general be used in a *relative* sense to compare alternatives, rather than as absolute measures – the higher the IRR of a project alternative, the more flexibility there is for its success compared with other proposals. For valid comparisons, project proposals should have approximately the same initial outlays and the same income stream lengths. If these conditions are not met further considerations concerning investment of surplus and realised funds should be taken into account.

Obtaining values for IRRs is a matter of solving polynomial equations for which income streams provide the coefficients. Now comes the *coup de grace* for those of your text messaging friends or colleagues who have not already been swept off their feet. The <span class="APLU">d</span> in  <span class="APLU">0=d #.is</span>  is merely a dummy denoting the form of the solution, the solution itself is supplied by just two keystrokes. Not only does this convey the message, but, given a J interpreter, the solution as well:

```
   is=._100 20 50 70 80      NB. an income stream
   p.is
+--+------------------------------------------------+
|80|_1.2196 _0.20558j1.1462 _0.20558j_1.1462 0.75579|
+--+------------------------------------------------+
```

from which the <span class="APLU">d</span> value of <span class="APLU">0.75579</span> is extracted and converted to <span class="APLU">p</span> form by

```
   dtop 0.75579
32.312
```

to obtain an IRR of 32.3%. The coefficients in the argument for <span class="APLU">p.</span> are in *ascending* power order, while those for the polynomial evaluator <span class="APLU">#. </span>are in *descending* order. Thus the value of 0.75579 is confirmed by

  

```
   0.75579 #.|.is
0.00046177             NB. effectively zero
```

It is always nice to have a simple case confirmed, so using the first example above

```
   p. _100 0 0 200
+---+-------------------------------------+
|200|0.7937 _0.3969j0.6874 _0.3969j_0.6874|
+---+-------------------------------------+
```

from which <span class="APLU">0.7937</span> leads to confirmation of the <span class="APLU">p</span> form obtained earlier.

```
   dtop 0.7937
25.99
```

Extracting the <span class="APLU">d</span> value above consists of opening the second box resulting from <span class="APLU">p</span>.

```
   roots=.>@{:@p.      NB. extract list of roots
```

and obtaining the single non-negative, non-complex value, the latter property being tested for by comparison with conjugates as provided by <span class="APLU">+</span> applied to complex numbers. From tally it is one small step to <span class="APLU">#~</span> as shorthand for ‘select’, which leads to two further building blocks

```
   real=.#~ (= +)      NB. select real values
   pos=.#~ >&0         NB. select positive values
```

The process of obtaining the internal rate of return then consists of four steps in sequence  :   

```
   irr=.dtop@pos@real@roots     NB. internal rate of return for an is
   irr is       
32.312
```

In words IRR is calculated as: convert to <span class="APLU">p</span> form the positive real root of the income stream regarded as polynomial coefficients.   

IRR calculations mirror those for fair loan repayment rates. The formula *(rk<sup>n</sup>)/(k<sup>n</sup>-1)* gives the fraction of an amount loaned which must be repaid in each of *n* periods when interest <i>p</i>% is paid on the declining balance.   The words “declining balance” reflect the fact that IRR calculations make an implicit assumption that the inflows are reinvested at the calculated compound interest rate, which can sometimes lead to a rosy-eyed picture of absolute IRR values. Using the input stream is as an illustration, the precise progress of each flow as it is reinvested is made evident by  

```
   k=.1.323121                  NB. extra precision used
   (20*k^3)+(50*k^2)+(70*k^1)+80-(100*k^4)
8.75549e_5  
```

Sometimes it can be convenient to enter an arithmetic formula such as *(rk<sup>n</sup>)/(k<sup>n</sup>-1)* by entering it more or less directly in the style of traditional programming languages. This option is readily available to text messagers through the words ‘monad’ and ‘dyad’ thus:

```
   PA=.dyad : '(x.*(1+x.)^y.)%_1+(1+x.)^y.'   NB.P(ayment)% A(mount)
```

The PA fraction, and the consequent repayment amount for a loan of 10000 are given by

```
   0.005 PA 12             NB. factor for monthly rate of 0.5% 
0.086066
   10000*0.086066          NB. monthly repayments
860.66
```

From the point of view of a lender the transaction is an income stream of \_10000 followed by 12 monthly payments of 860.66 whose IRR is therefore  

```
   irr _10000, 12#860.66
0.49992                    NB. i.e. monthly rate of 0.5%
```

  

illustrating that IRR matches exactly the fair rate of loan interest paid by equal instalments on a declining balance. ‘Matches exactly’ is the monadic meaning of the symbol <span class="APLU"> -:</span>, so in symbols

<pre><b>            r -: irr _1,n#(r PA n)</b></pre>

relates IRR succinctly to loan repayments rates. IRR is reduced if payments are deferred, whereas if a loan is paid off early using the same proportionate repayments, the IRR increases. These statements are confirmed by  

```
   irr _10000, (11#860.664),0,860.664       NB. last inflow delayed
0.49378
   irr _10000,6#2*860.664                   NB. inflow rate doubled     
0.92988
```

The related term APR takes into account that the equivalent annual rate is not 12\*0.005, corresponding to 6%, but rather this rate compounded over the 12 periods:

```
   1.005^12
1.06168             NB. APR is 6.17%
```

To describe APR, use the conversions to and from <span class="APLU">p</span>  to <span class="APLU">k</span> :  

```
   ptok=.>:@(%&100)
   ktop=.100&*@<:
   apr=.ktop@(ptok@irr ^ <:@#)
   apr _10000, 12#860.66
6.168
```

**Note on computational practicalities:**  <span class="APLU">irr</span> may fail if the income stream is too long, or if the data generates a pathological situation for <span class="APLU">p.</span>’s underlying root solver. In such circumstances, alternative numerical methods must be employed such as the Newton Raphson method, which can be expressed in the adverb

```
   Newton=.1 :']-x.%x.D.1'(^:_)("0)
```

Suppose that a loan of 1000 is repaid in 24 instalments of 45.27. The APR is worked out by  

```
   ms=.(24$45.27),_1000            NB. money stream reversed
   fn=.#.&ms                       NB. define a polynomial
   dtop fn Newton 0.9              NB. guess d=0.9 and apply N-R
0.67447                            NB. monthly rate of return = 0.6745%
   1.0067447^12                    NB. convert to annual rate
1.084                              NB. APR = 8.4%
```

By way of summary here is the proposed basic mathematical text messaging vocabulary:

```
   +  -  *  %  ^  !  <  >  =  >:  <:  ~:  +/  */
   +:  -:  *:  %:  <:  >:  ^:  {.  {:  }.  }:  @  &
   ,  #  |.  #~  #.  <  >  each=.&>  p. monad  dyad 
```

   Txt msg OK for sums?  LOL J

***
