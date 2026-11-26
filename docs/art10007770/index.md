---
vid: '10007770'
title: 'D: a functional subset of Dyalog APL'
authors:
- John Scholes
byline: by John Scholes (john@dyadic.com)
volume: '17'
issue: '4'
page: '93'
received: '2013-01-23'
online: '2013-12-24'
description: An introduction to the D functional subset of Dyalog APL
source: trad/v174/schol174.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2001
tags:
- language design
- programming techniques
---

by John Scholes (john@dyadic.com)
{ .byline }

Dynamic functions and operators appeared in Dyalog APL/W Version 8.1 in early 1997. Since that time, experience has shown that their field of application is at least as wide as expected and ‘D-Fns’ have started appearing in many operational workspaces. A number of applications [\[1\]](#ref1) have been written entirely in D-Fns and D-Ops and so we can consider them to constitute a small, complete sub-language within Dyalog APL. As the letter is one of the remaining few, not currently assigned to a programming language, we could call it: D.

D functions and operators are lightweight, faster but richer alternatives to the traditional ‘header-style’ container for the collection of APL expressions. D is described in detail in [\[2\]](#ref2) but the following little ‘reference card’ is a summary:

<table class="code">
		<tr>
			<td>{⍺ Function ⍵}</td>
			<td>{⍺⍺ Operator ⍵⍵}</td>
			<td> :   Guard</td>
		</tr>

		<tr>
			<td> ⍺ Left argument</td>
			<td> ⍺⍺ Left Operand</td>
			<td>::   Error-Guard *</td>
		</tr>

		<tr>
			<td> ⍵ Right argument</td>
			<td> ⍵⍵ Right Operand</td>
			<td>⍺←   Default larg</td>
		</tr>

		<tr>
			<td> ∇ Self reference</td>
			<td> ∇∇ Self Reference</td>
			<td>1:s← Shy result</td>
		</tr>
	</table>

\* Error-Guards are due to appear as an enhancement to Dyalog Version 9 and will be released initially via the web-based Dyalog Subscription Service.

The following example shows some of the look and feel of D:

```
      display ⎕cr'display'
┌→─────────────────────────────────────────────────────────────────────┐
↓display←{⎕IO ⎕ML←0                          ⍝ Boxed display of array. │
│                                                                      │
│    ⍺←1 ⋄ chars←⍺⊃'..''''|-' '┌┐└┘│─'       ⍝ ⍺: 0-clunky, 1-smooth.  │
│                                                                      │
│    tl tr bl br vt hz←chars                 ⍝ Top left, top right, ...│
│                                                                      │
│    box←{                                   ⍝ Box with type and axes. │
│        vrt hrz←(¯1+⍴⍵)⍴¨vt hz              ⍝ Vert. and horiz. lines. │
│        top←(hz,'⊖→')[¯1↑⍺],hrz             ⍝ Upper border with axis. │
│        bot←(⊃⍺),hrz                        ⍝ Lower border with type. │
│        rgt←tr,vt,vrt,br                    ⍝ Right side with corners.│
│        lax←(vt,'⌽↓')[¯1↓1↓⍺],¨⊂vrt         ⍝ Left side(s) with axes, │
│        lft←⍉tl,(↑lax),bl                   ⍝ ... and corners.        │
│        lft,(top⍪⍵⍪bot),rgt                 ⍝ Fully boxed array.      │
│    }                                                                 │
│                                                                      │
│    deco←{⍺←type open ⍵ ⋄ ⍺,axes ⍵}         ⍝ Type and axes vector.   │
│    axes←{(-2⌈⍴⍴⍵)↑1+×⍴⍵}                   ⍝ Array axis types.       │
│    open←{(1⌈⍴⍵)⍴⍵}                         ⍝ Expose null axes.       │
│    trim←{(~1 1⍷^⌿⍵=' ')/⍵}                 ⍝ Remove extra blank cols.│
│    type←{{(1=⍴⍵)⊃'+'⍵}∪,char¨⍵}            ⍝ Simple array type.      │
│    char←{⍬≡⍴⍵:hz ⋄ (⊃⍵∊'¯',⎕D)⊃'#~'}∘⍕     ⍝ Simple scalar type.     │
│                                                                      │
│    {                                       ⍝ Recursively box arrays: │
│        0=≡⍵:' '⍵(⎕FMT ⍵)⍵(⊃⍵ ⍵∊⎕AV)⊃' -'   ⍝ Simple scalar.          │
│        1 ⍬≡(≡⍵)(⍴⍵):'∇' 0 0 box ⎕FMT ⍵     ⍝ Object rep: ⎕OR.        │
│        1=≡⍵:(deco ⍵)box ⎕FMT open ⍵        ⍝ Simple array.           │
│        ('∊'deco ⍵)box trim ⎕FMT ∇¨open ⍵   ⍝ Nested array.           │
│    }⍵                                                                │
│}                                                                     │
└──────────────────────────────────────────────────────────────────────┘
```

### Functional programming

D facilitates, but does not impose, writing in a purely functional style. To do justice to exactly what this means is beyond the scope of this note but see [\[3\]](#ref3) and [\[4\]](#ref4) for an introduction.

An *indication* that a piece of code is in the *structured programming* style is that it contains no branches. Likewise, an *indication* that a piece of code is in the pure, functional style is that it contains no variables. In this context, there is a distinction between a variable and a definition. A definition is a name, associated (just once) with an object, whereas a variable may be re-assigned any number of times. Pure functional programming does not admit this ‘destructive’ assignment or any of its variations: indexed, selective or modified assignment.

If we adhere to this restriction, we are entitled to pronounce the symbol `←` as *is*, rather than *gets*, *becomes*, etc. The `display` function, above, is an example of such code.

There is a (possibly apocryphal) story that a great advance in the field came when some computer scientists were trying to teach programming to a group of geography students. The students couldn’t understand the meaning of: `I=I+1`, and on reflection, the teachers realized that they weren’t too sure either!

### Enumeration of all possible D functions

If we were to list all possible D functions in order of complexity, each of the first six would be useful:

```
sink←{}         ⍝ Discard unwanted value.
lev←{⍺}         ⍝ Left argument.
dex←{⍵}         ⍝ Right argument.
const←{0}       ⍝ Return constant.
dup←{⍵ ⍵}       ⍝ Duplicate.
link←{⍺ ⍵}      ⍝ Left arg, right arg, pair.
```

`const¨` provides the shortest (but not quickest) way to return an array of values, the same shape as an arbitrary array: `{'woof'}¨102⍴'K'9`.

### Tail recursion vs iteration

D’s guard construct is sufficient to replace *all* the usual control structures. People find this unsurprising for `:If :Else` and `:Select`, but sometimes have difficulty with the iterative controls.

`:For` splits into two cases. If there is no communication among the passes of the loop, it maps immediately onto `{…⍵}¨`. The construct is recognised by the interpreter and special-cased to re-use the D function stack frame, which gives it the same slick performance as a tail call. Otherwise, if there is some ‘accumulation’ of values by successive passes, it maps onto `↑{…⍵}/`.

`:While` and `:Until`, each contain an implicit test for termination. In D, this test maps onto a guard at the top or bottom of the function, where the ‘loop’ becomes a tail-recursive call on the function. In this context, tail recursion has been described as ‘looping with parameters’. Look at the following equivalent versions of a function for the sum of the items of a vector. (There are of course, easier ways to achieve this.)

<table>
		<thead><tr><th>Traditional APL</th><th>D</th></tr></thead>
		<tr>
			<td>
<pre class="left">
    ∇ rslt←sum nums
[1]   rslt←0
[2]   :While 0¨⍴nums
[3]       rslt+←⊃nums
[4]       nums←1↓nums
[5]   :EndWhile
    ∇
</pre>
			</td>
			<td>
<pre class="left">
  sum←{
      ⍺←0
      0=⍴⍵:⍺
      (⍺+⊃⍵)∇ 1↓⍵
  }
</pre>
			</td>
		</tr>
	</table>

We sometimes seem reluctant to use recursion. To be fair, we may be worried by a *perceived* cost of stack allocation and at first find it difficult to distinguish between a ‘tail call’ and a ‘stack call’. In Dyalog, a tail call is by a small margin, the fastest way of ‘branching’! Look at the following two versions of factorial.

<table>
	<thead><tr><th>Stack Call</th><th>Tail Call</th></tr></thead>
	<tr>
		<td>
<pre class="left">
  fact←{
      ⍵=1:1
      ⍵×∇ ⍵-1
  }
</pre>
		</td>
		<td>
<pre class="left">
  fact←{
      ⍺←1
      ⍵=1:⍺
      (⍵×⍺)∇ ⍵-1
  }
</pre>
			</td>
		</tr>
	</table>

## Techniques

The rest of this note is devoted to a variety of techniques that have been discovered while programming in D. References such as \[`max parse`\], show \[workspace function\], where examples of the technique can be found in sample workspaces supplied with Dyalog Version 9.

### Avoiding name clashes

APL’s ‘name clash problem’ can be largely avoided in D. Traditional functions that do workspace administration tasks such as function cross-referencing, must be careful that their own local names do not obscure the global names in which they are interested. This leads to the choosing of ‘nasty’ local names: `∆_∆_fns __cr_list`, etc. (The author has been tempted to use obscene words for such names in locked functions, reasoning that if a name clash did occur, the victim would be unlikely to call to complain – but decency prevailed).

Names in D are localised dynamically. In the following function, `names` and `source` are localised *after* the evaluation of `⎕CR` and `⎕NL` and so do not obscure global homonyms.

```
wsdoc←{
    names source←↓⍉↑{⍵(⎕CR ⍵)}¨↓⎕NL ⍵
    ...
}

[dfns attrib]
```

This technique is fine for simple tasks such as the above, but can become unwieldy for more complex ones, as it requires that all ‘external work’ be done *before* the first assignment. More generally, we can preserve an outer name-free environment as a function operand to an inner operator. This is one advantage of D’s *static* scope rules.

```
find←{
    {⍎⍵}{                ⍝ 'external' operand.

        local names←…    ⍝ local names
        …
        vars←⍺⍺'⎕nl 2'   ⍝ name list via operand.
        vals←⍺⍺¨↓vars    ⍝ values via operand.
        …
    }⍵
}

[dfns find]
```

### Function pipelines

It is sometimes convenient to break up a large function into a ‘pipeline’ of smaller ones, where the result of each is passed as argument to the next. In particular, this gives fine control over the amount of workspace used, as large temporary definitions may be localized more accurately. In some cases, the indentation in a ‘raw’ pipeline of functions shows the structure of the code better than does a succession of function names: `foo goo …`.

```
{
    … ∇ …
}∘{
    … ∇ …
}¨  …

[dfns factors] [dfns stamps] [dfns wsdiff]
```

### Initial left argument

One way to provide an initial (as opposed to default) left argument, as an accumulator for a function, is to bind it at definition time using composition. This delivers a slight performance benefit, particularly in a recursive function, as the body of the code no longer needs to test (`⍺←…`) for the existence of the left argument. The `sum` example above would become:

```
sum←0∘{
    0=⍴⍵:⍺
    (⍺+1↑⍵)∇ 1↓⍵
}

[max.parse]
```

### ‘Call me’

The `∇` or *self* token refers to its nearest enclosing brace pair. If we want a recursive call that ‘reaches’ out more than one level, it seems we are obliged to name the target function, solely in order to use its name for the call:

```
foo←{
    {
        foo ⍵   ⍝ recursive call on outer function.
    } ⍵
}
```

An alternative is for the target function to pass a reference to itself as an *operand* to the inner function, which then becomes an operator:

```
{
    ∇{
        ⍺⍺ ⍵    ⍝ recursive call on outer function.
    }⍵
}
```

The technique can be used to pass the function reference through an arbitrary level of nesting. This means that we are not forced to invent an artificial name for a function just in order to recur. We name things only for elucidation and to avoid repeated evaluation. Perhaps for this reason, defined operators seem to occur more frequently in D than in traditional APL. \[`min parse`\]

### Complex guards

Finally, an obvious point: there is no limit to the complexity of the guard expression to the left of the `:`. In particular, it can include a D function call.

```
{
    …
}⍵:{
    …

[max unify]
```

## References

1. <a id="ref1"></a> `min` and `max` are sample workspaces supplied with Dyalog Version 9.0.
2. <a id="ref2"></a> *Vector*, Vol. 13.2, 88 “Dynamic Functions in Dyalog APL”  
    Available from [dyalog.com/download/dfns.zip](http://www.dyalog.com/download/dfns.zip).  
    This rich text format file uses font “Dyalog Std TT”, from [dyalog.com/download/dlogttst.zip](http://www.dyalog.com/download/dlogttst.zip) \[<em>Ed: compatible with APL2741 as used on the Vector site.</em>\]
3. <a id="ref3"></a> *The Computer Journal*, Vol. 32.2, April 1989.
4. <a id="ref4"></a> Bird, R. & Wadler, P. *Introduction to Functional Programming*, Prentice Hall, ISBN 0-13-484197-2
