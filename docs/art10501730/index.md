---
vid: '10501730'
title: AntLang – A modern APL system
authors:
- Anthony Cipriano
byline: Anthony Cipriano
volume: '27'
issue: '1'
received: '2016-09-14'
online: '2016-11-01'
editor: John Jacob
description: A new APL on the block. The author invites you to explore AntLang
source: ../../php-site/content/printed/271/antlang/antlang.htm
converter: vecrescue 0.1.0
issue_tab: n1-20162022
tags:
- implementations
- language design
---

Anthony Cipriano
{ .byline }

![](featuredImg_a-modern-apl-system.jpg){ .lead-image }

## <a id="why-antlang"></a>Why AntLang?

Because they can’t be enough APLs!

OK, there is a bit more to it: AntLang is intended to be practical.

It does multi-threading, system-interaction, easy File-I/O and many many more.

## <a id="syntax"></a>Syntax

Most of the syntax is inspired by K and Q, because they are very convenient and compact languages.

However, they do some steps in the false direction, like making a difference between verbs and functions.

```
q)2+2
4
q)3{x+y}5
’type
```

In AntLang, verbs are functions and functions are verbs, so both of the above examples execute correctly.

```
    a)2+2
a1:4
a)3{x+y}5
a2:8
```

AntLang does not allow juxtaposition, which allows statically parsing AntLang code without knowing the type of things.

```
a)sin 2
Syntax Error
a)sin[2]
a1:0.9092974268256817
a)sin@2
a2:0.9092974268256817
```

It makes source code a bit longer sometimes, but it implementing AntLang very easy compared to K or J, which is why there already exist multiple implementations (hsAntLang, minAntLang, tryAntLang, `....)` .

APL is the only language I know, that does not allow writing classic BNF parsers, so for me, this is a big improvement.

However, it does not allow something like “atom atom atom”, since it would be interpreted as “atom function atom”.

```
a)1 2 3
Domain Error: 1 2 3
a)<1;2;3>
a1:<1;2;3>
```

## <a id="semantics"></a>Semantics

Arrays make it really hard to talk about deeply nested things, like trees or so, which is why AntLang uses sequences instead.

However, you can still think in arrays using sequences.

```
a)$|<<1;2>;<3;4>;<5;6>> / "$" is mingle, "|" is apply, "$|" is transpose
a1:<<1;3;5>;<2;4;6>>
```

It only has two concisely chosen adverbs: reduce and pair each.

```
a)1*/iota[5]
a1:120
a)<1;2;3>+/:<4;5;6>
a2:<<5;6;7>;<6;7;8>;<7;8;9>>
```

Most other APL adverbs are implemented as classic verbs, which is allowed in terms of static parsing, so that functions can be easily passed to other functions.

## <a id="conclusion"></a>Conclusion

It is definitely a good idea to try out AntLang, becuase it brings static parsing and functional programming to APL, which is definitely something you have to try.

You can get more information and even read its source code on  
GitHub: [github.com/ac1235/AntLang](https://github.com/ac1235/AntLang)
