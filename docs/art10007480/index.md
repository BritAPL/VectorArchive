---
vid: '10007480'
title: A Control Operator
authors:
- Denis Samson
byline: by Denis Samson
volume: '10'
issue: '3'
page: '113'
source: trad/v103/samson103_113.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- language design
---

by Denis Samson <span class="affil"></span> <span class="email"></span>
{ .byline }

Over the last 25 years, dozens of proposals have been made about adding control structures (of the if-then-switch-for-while-until variety) onto APL. At the last APL conference in Toronto, there were no less than three such papers!

Two things come to mind: first, there IS a need for some acceptable form of control structures in APL; second, no widely acceptable control structure scheme exists yet, since no APL system offers any.

The problem with all proposals so far is that either they do not extend the language, or they do extend it!

- if they do not extend APL, they use defined IF THEN ELSE functions that use quoted arguments: they are essentially slow and unreadable if there is more than one level of imbrication;
- if they do extend APL, they add on top of it meta-constructs that are too foreign in design to have a chance to mix with APL. For example, control structures are traditionally expressed in a redundant-prefix-and-often-postfix form – e.g. if &lt;condition> then &lt;statements> endif – whereas APL is a pure infix language: these mix like oil and water!

This paper should help solve that dilemma by offering a general-purpose operator that fits perfectly with APL syntax and gives control structures for free. The operator will be used in conjunction with a revised use of the colon and a new notation for defined functions.

### Operators

Operators, also called adverbs or conjunctions in Iverson’s “Dictionary of APL” \[Iv 86\] have been around in APL since the beginning, first as second-class APL citizens – you would call “+/” a reduction, but “/” was just part of the notation – then later \[Iv 79\] as distinct grammatical entities. Over the years, the syntax for operators – &lt;function> &lt;monadic operator> gives &lt;derived function> and &lt;function> &lt;dyadic operator> &lt;function> gives &lt;derived function> – has been refined, new operators – e.g. each, rank, and various forms of composition – have been added and the syntax widened a bit: &lt;value> &lt;operator> and &lt;operator> &lt;value> were also permitted.

In parallel, a healthy dose of retrofitting allowed us to turn specification into an operation: the left arrow in Dyalog’s V+É1 (for VÉV+1) and VïþÉ1 (for VÉ1ïV) is a (slightly anomalous) monadic-or-niladic operator that is the modern version of Burrough’s APL/700 pre-operator notation.

### Control Arguments

A distinction absent from APL syntax is that of “control arguments” to a function. APL does not provide any formal distinction between a “data argument” of a function and a “control argument” used to modify some behaviour of the function.

The only formal distinction was between scalar and mixed functions, where the latter’s left arguments were often concerned with structure. The term “mixed” even disappeared in later dialects with the advent of function ranks. Control arguments have been recognized as such in \[Iv 79\] and \[IPS 84\] as “custom, or variant parameter”, with a “variant operator” to take care of such arguments. This distinction did not surface in any implementation, however.

On the contrary, numerous ad-hoc techniques continue to be used in order to specify a variant of a function, or, in other words, to provide a function with a control argument. For example, they may use:

- a variant of the symbol: ®V
- an implicit argument:
- ŒCTÉ0 ¿ A=B
- ŒIOÉ1 ¿ ¼N
- ŒPWÉ5 ¿ •X
- an explicit “modifier”:
- ’S&lt;., .>BF12.2’ ŒFMT AMT
- brackets:
- A,\[ŒIO-0.5\] B
- an operator:
- ,ð0 M
- the left argument:
- 1X
- a global variable:
- FILLÉ’\*’ ¿5 TAKE ’ABC’ (gives ’ABC\*\*’)
- arrays:
- (é5 ’\*’) TAKE ’ABC’

This proposal is that control arguments are supplied to a function with a control operator. The symbol we will use here is “à”, for reasons that will become obvious later on. Syntax is &lt;control argument> à &lt;function>. A variant of the operator can be thought of, &lt;function> É &lt;control argument>, but maybe we’re getting a bit too recursive for now...

This use does not interfere with that of the “go to”, which could be considered as an – outdated, slightly anomalous, but unambiguous – monadic use of “à”. The expressions in the example above could be rewritten so that they use their control argument in a consistent way, for example: A 1à= B or 1à¼N or 1à\*X or 5 ‘\*’àî ’ABC’ or 5 ‘\*’àTAKE ’ABC’.

Now, while APL implementers can happily provide variants to primitives that will free our APL code of all those viciously hidden arguments – and certainly this is a worthwhile achievement – the real value of this operator lies in the answer to the question “how can a defined function, such as TAKE in the last examples, benefit from the variant specification?” Or, in modern APL speak, “how do I derive a function from ’\*’àTAKE ?” The mechanism provided here is definitely object-oriented: rather than tell the operator how to apply the control argument to the function argument, we pass, or “send”, the control argument to the argument function, which then decides by itself what variant it is going to turn into. This can be put in contrast with APL2’s use of defined operators for applying function argument(s) to (data) argument(s).

The notational mechanism used here is quite simple, and simply consists of prefixing statements with constants representing variants. APL implementations allowing named constants could certainly use them in that context. In the admittedly simplistic case of a monadic CIRCLE function that can emulate C\*X with CàCIRCLE X, our CIRCLE function would look like:

```
     ∇ R←CIRCLE X
[1]  0: R←(1-X*2)*0.5
[2]  1: R←1○X
[3] ¯1: ⎕ERROR (~X≡¯1⌈X⌊1)/'DOMAIN'
[4]     R←¯1○X
[5]  2: R←2○X
     ...
     ∇
```

And ¯1àCIRCLE would derive – at least conceptually – the function:

```
     ∇ R←CIRCLE X
[1]   ⎕ERROR (~X≡¯1⌈X⌊1)/'DOMAIN'
[2]   R←¯1○X
     ∇
```

As one can guess from the example, the rules are quite simple: “C à F derives, for each item of C, a function built by selecting only the statements prefixed with that item”. The appendix presents a more complete tentative syntax and semantics for prefixes, so we shall discuss only a few points here:

As can be seen in the example above, a prefix applies to all statements to its right and below until the next prefix. There is no reason a prefix would need to be at the beginning of a line: 1:RÉ1\*X ¿ 2:RÉ2\*X certainly looks quite reasonable!

One can also note that prefixes are remarkably similar to cases (provided by almost all other modern languages), except we do not need a special &lt;break> statement to move around “unselected” cases: the derived function does not even contain the unselected statements, and the prefixes themselves are gone!

Prefixes shown so far are all scalar. But there is no reason that an array language such as APL should be restricted to scalar variants. On the other hand, rank-2 and higher arrays can be formed only with functions – as in 2 3 ½¼6 – so we will restrict ourselves here to scalar and vector prefixes.

The prefix rank, or case rank, of a function is the maximum rank of all prefixes in the function. It gives the rank of items taken from the control argument and matched to prefixes of the function in order to derive the function or functions. For example, if FN has cases such as 0 0: , 0 1: , 1 0: and 1 1: we could get a derived function with (0¬ŒIO,ŒCT)àFN and a vector of 4 derived functions with (³ 2 2 ‚¼4)àFN

We won’t go any further here on the subject of function arrays, since our concern here is control structures – and we’re almost there! Nonetheless, we would like to note arrays of functions so created do fit very naturally with APL syntax and can express very powerful ideas: for example, X 1àFOOÜ Y applies 1àFOO to each pair of elements from X and Y.

By the way, all derived functions apply to the data argument or arguments, so the control operator is a form of outer product. And X (1 2 1àFOO) Y might be a nice notation for specifying three parallel executions of 1àFOO, 2àFOO and 1àFOO on X and Y!

### Control Structures

The astute reader probably has already noticed that a statement such as (A>0)àF with F defined as:

```
     ∇ F
[1] 1:'TRUE' ⋄ 0:'FALSE' ⋄ 2:'MAYBE'
     ∇
```

expresses a simple if then else in a very APL-looking style!

Further,we can even use:

```
     1→F               0→F            1 0 2 1→F
TRUE             FALSE          TRUE
                                FALSE
                                MAYBE
                                TRUE
```

The rightmost example being a “for case of”. Of course, function F could be monadic or dyadic:

```
     ∇ R←X F Y
[1] 1:R←X ⋄ 2:R←Y ⋄ 0:R←'NEITHER!'
     ∇
     'LEFT' (1 0 2 1→F) 'RIGHT'
LEFT
NEITHER!
RIGHT
LEFT
```

What we’ve got here in fact is a very general use of our control operator. Going just slightly further, we could also define two more uses of the operator:

- &lt;function 1> à &lt;function 2> is a &lt;while>,
- &lt;function> à ( 0 | 1 | 2 ) creates a derived function with the specified valence.

As in a &lt;case>, any result of &lt;function 1> in a &lt;while> is used to derive variant(s) of &lt;function 2>. The difference with the &lt;case> is that after the variants have been used, &lt;function 1> is called again to derive another (set of) variants of &lt;function 2> that are then used, and so on.

The cycle stops when no more variants of &lt;function 2> can be derived, that is when &lt;function 1> does not return any variant that can be matched to a prefix in &lt;function 2>.

This is actually pretty simple:

```
     ∇ F                          ∇ R←TEST
[1] 1:⍳N ⋄ N←N-1              [1] R←N>0
 ∇                               ∇

     N←3 ⋄ TEST→F
1 2 3
1 2
1
```

By the way, niladic functions used as arguments of a defined operator are treated differently in APL2: rather than being passed as functions to the operator and used by it, they are simply evaluated outside the operator and their result passed to it.

If this behaviour would be changed – I would say corrected – there would be no conflict with their treatment here as arguments of “à”. Besides, if-then constructs could be expressed with a defined IF operator and a niladic function.

### Bracket-Notation for Functions

So far, so good. Except that if we have to define a function each time we need to express even a simple if then else, things suddenly get less exciting, and readability improvements are not very evident. For example, replacing:

```
  →(A>0)↓ELSE ⋄ 'POSITIVE' ⋄ →END
 ELSE: 'ZERO OR NEGATIVE'
 END:
```

by:

```
0 0⍴⎕FX 'F' '1:''POSITIVE''' '0:''ZERO OR NEGATIVE'''
(A>0)→F
```

does not seem to buy us a big improvement in readability! Besides, function arrays \[BH 91\] \[Bk 84\] \[Bn 84\] \[Br84\] could also be used in a similar hard-to-read way: for example, if FA is a two-Function Array with an &lt;else> function as first element and a &lt;then> function as the second, then FA\[ŒIO+A>0\] would be quite as bad as our (A>0)àF above.

So what we really need to introduce here is at the notational level: we need to be able to express our constructs without having to use pre-defined functions. What we introduce is a variation on direct-definition of functions: one or more APL statements, on one or more lines, enclosed in braces \{\} \[Sa 84\]\[Bu 87\] is a function.

The form of function definition offered here: \{+/?\} is equivalent to +/ in tacit-definition \[HIM 91\]. It is intended for use in expressing more complex algorithms where the arguments and result cannot simply be glued to the left and right of a single expression. The +/ notation could of course be used instead of \{+/?\} for simple cases. Since there is no header, the valence of the function is not explicit. The arguments, if any, are accessed as ¸ and ?, and the function may refer to itself as ‘. The result is that of the last expression executed in the braces. The specific value matching a prefix (see the appendix) is accessible as #. (note that, since # is a national character, it’s probably not very well suited for such a use, so another Œvariant might be better).

The fact that a \{\}-function has no explicit valence allows it to be free of the anomalous treatment of niladic functions. If we need to assign a valence to a \{\}?function, perhaps another version of our “à” operator could be used: \{+/?\}É1 (or +/É1) would be the monadic variant. But this is outside the scope of this paper.

Also outside our scope here is function assignment. If a \{\}-function can be assigned to a name or used as a normal APL data entity, it becomes a first-class APL citizen, and can be used for example in arrays of functions. The only thing we need to sort out here is under what circumstances are the function or its variants applied (to their arguments, if any) and when are they referenced (as functions, not as results of their execution).

In other words, after FÉ\{...\} is F a function, or is it the result of the execution of \{...\}? In current APL, the “niladic-function syndrome” prohibits named-function assignment because its niladic functions are syntactic anomalies: while FÉF1 (with F1 a monadic function) could be allowed (which name would be used in the header of F?), would, on the other hand FÉF0 (with F0 niladic) turn F into a copy of F0 or assign it the result of its execution?

Of course, the interpretation of FÉF0 might depend on the syntactic class of F at the time of the assignment: &lt;function> É &lt;function> would assign a function, and &lt;not a function> É &lt;function> would execute and assign a result. Is the APL community ready to get that near to declarations?

Back to \{\}-functions, this anomaly does not exist, since \{\}-functions have no explicit valence (should they be called “anavalent functions”?). The rule assignment vs. execution is simpler: \{\}-functions are executed ONLY if they are given argument(s) -e.g. \{+/?\} V – OR they are sitting alone in a statement – e.g. \{’Hi!’\} – OR they are derived -e.g. (A>0)à\{1:’yes’\}.

Anything else is an assignment. Assignment of a variant can be expressed easily: VFÉ\{caseà\{...\}\}.

Now, since there’s no reason \{\}-functions shouldn’t use prefixes, we can use these to write much-easier to read statements, such as

```
(×A) → {1:'positive' ⋄ 0:'zero' ⋄ ¯1:'negative'} ⍝ (case)
```

or

```
Fib←1 1 ⋄ {10>⍴Fib} → {1:Fib←Fib,+/¯2↑Fib}    ⍝ (while)
```

A \{\}-function can of course spread over multiple lines, just like a current defined function. Immediate-execution mode should of course provide a reminder – a “\}” under the opening “\{”? – that a \{\}-function is still open.

### Name Scopes

\{\}-functions need local names, and they are free from the local-names list of the header, since there is no header at all. So we can – and have to – design appropriate name scope rules and notation. Here are a few ideas:

     \*     Declaration of a local name could be a side-effect of its initialisation: any statement in the function before the first prefix could be used for setting a name space shared by any derived function(s) and the left argument of the operator. For example,

```
     root← {Y>tol} → {tol←0.001 ⋄ X←Y←1 ⋄ F←{...} ⋄ 1:Y←F X←X+{...} X ⋄ X}
```

would have local variables tol, X and Y, plus a local function F. The rule for localisation here is that any name assigned before it is used is local. Further, if we have compilers in mind, we might enforce a rule essentially saying that locals cannot change rank or type after they are initialised.

The function derived from the example above would be equivalent to the following, expressed as a current defined function:

```
     ∇ result←derived;tol;X;Y;F
[1]   tol←0.001 ⋄ X←Y←1 ⋄ 0 0⍴⎕CR 'R←F X ...'
[2]  while: →(Y>tol)↓end
[3]   Y←F X←X+{...} X
[4]   →while
[5]  end:result←X
     ∇
```

     \*     Static localisation of names – that is, names visible only inside the braces, NOT from functions defined outside even when called from inside – could be the rule, ar at least available. This could provided with prefixes, e.g.=: MÉ0 0½’’ might declare and initialise a static local character matrix. Anyway, a good deal of experimentation with different schemes is probably needed before anything gets cast in concrete!

### Conclusion

In this author’s opinion, the time has come for APL to get a long overdue renewal and provide control structures before it’s too late: newcomers to APL have become a rare species in our community – the average age of participants at APL93 is very striking: we were a gathering of veterans from the Fortran wars! – so the language is threatened with extinction within the current generation.

APL is doomed if nothing is done soon to attract people who have been exposed to structured programming and are repelled by a spaghetti language like APL! Now that we can integrate control structures as first-class APL citizens, there is a window of opportunity the APL community can’t afford to miss! We are desperately looking for APL3!

### Appendix – Tentative Syntax and Semantics of Control Structures

### 1. Prefixes

Prefixes are used inside a named or unnamed defined function in order to select which statements are going to be part of a particular derived function. Since prefixes are – at least conceptually – absent from the derived functions, they need not be APL expressions and can have their own syntax. Examples of non-APL syntaxes are found in system commands, ’-editor, ŒTRAP or ŒFMT.

Alternatively, prefixes could be any APL statement, but this is probably not necessary if a simple notation can cover 90 percent of cases and the rest is taken care of by providing access to the actual case value with # or Œcase.

Each prefix is matched to variants provided by the left argument of the control operator “à” and, in case of a match, the statement(s) at the right and below the colon are included in the derived function.

The maximum rank of all prefixes is called the case rank of the function. This gives the rank of items taken from the left argument and matched with prefixes. So a prefix is either a scalar numeric or character descriptor or a vector of two or more descriptors. A descriptor is either a number, a range of numbers, a character, or one of the following special symbols:

? matches any number or character

\* matches a length-0 or longer sequence of numbers or characters

~ matches any number or character not previously matched in this variant.

\* and ~ reset the “not matched yet” attribute.

ç: and ’’: match numeric and character empty vectors, respectively.

For example:

```
 1 *: 2 *:
```

matches any variant # with #\[1\]¹ 1 2

```
 ~ 0:
```

then matches the others who also have #\[2\]=0

```
 *:
```

matches all

```
 ? ? *:
```

matches all length-two or more vectors

```
 'load':'save': matches these two character vectors
 1..3 4..5: matches 1 4, 1 5, 2 4, 2 5, 3 4 and 3 5.
```

### 2. Escapes

While we do not need to “branch around” unselected statements, we need to provide the following:

- ŒBREAK &lt;condition> exits this variant of the (derived) function.
- ŒEXIT &lt;condition> exits all variants. This is useful for a &lt;while>. (&lt;condition> ŒBREAK/EXIT &lt;result value> may be worthwhile as well)

### 3. Explicit names

In non-tacit \{\}-functions, names have to be provided for arguments, function, case value (more exactly, variant selectors) and perhaps result. This paper uses the following symbols, shown with possible Œ-variants:

¸ (ŒX) left (data) argument

? (ŒY) right (data) argument

‘ (ŒF) original function, allows recursion and use of other variants

\# (ŒC) case used to derive this variant

@ (ŒR) explicit result: may be useful (?) in cases where it is not convenient to use the last statement to provide a result.

### 4. Branches and Labels

These two archaic “features” of APL do not interfere with our control operator and prefix notation, so they can be tolerated, at least on the grounds of compatibility with existing APL functions. A few obvious restrictions are needed for controlling their fossil use, such as “branches are not allowed into a \{\}-function”.

Denis Samson  
Accès Informatique  
940, avenue de Manrèse  
Québec G1S 2X1  
tel/fax: (418) 688 9645

### References

     BH 91     R. Bernecky, R.K. Hui, “Gerunds and Representations”, APL 91 proceedings, APL Quote-Quad vol. 21 no. 4

     Bk 84     R. Bernecky, “Function Arrays”, APL 84 proceedings, APL Quote-quad vol. 14 no. 4

     Bn 84     J.P. Benkard, “Syntactic Experiments with Arrays of Functions and Operators”, APL 84 proceedings, APL Quote-quad vol. 14 no. 4

     Br 84     J.A. Brown, “Function Assignment and Arrays of Functions”, APL 84 proceedings, APL Quote-quad vol. 14 no. 4

     Bu 91     J. Bunda, “APL Function Definition Notation”, APL 91 Proceedings, APL Quote-Quad vol. 21 no. 4

     HIM 91     R.K. Hui, K.E.Iverson, E.E. McDonnell, “Tacit Programming”, APL Quote-Quad vol. 21 no. 4

     IPS 84     K.E. Iverson, R. Pesh, J.H. Shueler, “An Operator Calculus”, APL 84 proceedings, APL Quote-quad vol. 14 no. 4

     Iv 79     K.E. Iverson, “The Role of Operators in APL”, APL 79 proceedings, APL Quote-Quad vol. 9 no. 4

     Iv 86     K.E. Iverson, “A Dictionary of APL”, I.P. Sharp Associates, July 1986

     IW 81     K.E. Iverson, P. K. Wooster, “A Function Definition Operator”, APL 81 proceedings, APL Quote-Quad vol. 12 no. 1

     Sa 84     Denis P. Samson, “A Proposal for Control Structures in APL”, APL 84 proceedings, APL Quote-quad vol. 14 no. 4

(webpage generated: 23 February 2006, 20:34)
