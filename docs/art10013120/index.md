---
vid: '10013120'
title: Jot-Dot-Floor ...or Jot-Dot-Min?
authors:
- Ian Clark
byline: by Ian Clark, . (earthspot2000@hotmail.com)
volume: '11'
issue: '3'
page: '999'
source: trad/v113/clark113_14.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- humour
- education
---

by Ian Clark, <span class="affil">.</span> <span class="email">(<a href="mailto:earthspot2000@hotmail.com">earthspot2000@hotmail.com</a>)</span>
{ .byline }

Here’s a quote from the June 1994 editorial:

“...my first innovation, a column of jottings on rock-bottom educational matters. Since it’s simply got to have a techie APL title, what better than ‘Jot-Dot-Floor’?”

I didn’t really want an answer to this. But I got one. A week or so ago the following flame from cyberspace tracked me down, node-by-node, like King Tut’s ghost:

“This has been bugging me for a while, so I thought I’d better fix it now. Your column title ‘jot dot floor’ is catchy, but wrong. For someone doing education stuff you’re misleading the public. Floor is a monadic verb. Min is a dyadic verb. Both inner and outer products take dyadic verbs as their right operands. Hence your column should properly be titled: ‘jot dot min’ This is not only correct, but looks nicer \[3 characters of 3 each\].

```
jot
dot
min
```

– Bob (Bernecky)”

Well, what can I say? Bob’s right, of course. I could point to the absence of arguments altogether, which makes it niladic, sort of, but that only draws attention to its being syntactically as well as semantically wrong, besides making people wonder if there are any valid arguments in the body of the text, let alone the title.

I could hide behind a symbolic rendering: `∘.⌊` but that’s obscurantist and just bemuses the public. The Editor-In-Chief was no comfort. He said I should have consulted the ISO standard which gives the proper English names for all the primitives (now he tells me!).

But I was thinking of a plan to digitise myself enunciating the names of the primitives in my beautiful BBC English (mummy used to leave me alone in the house with the radio on). My son’s already done it in broad Werdle. I could find others to read them for me in Brummie, ’Merkin, perhaps even Strine. Then onto French, and other languages.

I thought some more about it. Do the Finns have standardised names for the APL verbs – and can you type them on a single line? Do the Russian names for the verbs have perfective and imperfective aspects? Bearing in mind who hosted APL’94, do the Belgians – and do they have twice as many standard names as everybody else – one set in Walloon and one in Flemish?

Who else can I think of? What’s Spanish for Floor, or Min? Is it the same in South America? Do Californian schools have to teach three names for every APL verb, in English, Spanish and Vietnamese like their public signs? Have the French expelled the last remaining soupon of Franglais from their APL nomenclature? Is APL usage governed by the Acadmie Franaise? What about APL in Hebrew – do the verbs decline and the nouns conjugate? Has the Islamic world even begun to think of names for the contents of `⎕AV`? – or were their scholars calligraphing them from right to left in flowing Naskh during the 11th century? Did the Crusaders actually bring APL back from the Holy Land, only to have it branded as heretical by the official dogma?

Do the Chinese use the same written names as the Japanese, but pronounce them differently? Do the Eskimos have 127 different names for Rho? And what, oh what, are they doing to APL on the Pacific Rim? Do the Ozzies care an `××××`?

Let’s come nearer home. Do the APL primitives have names in Welsh, and why not, man? Would the acceptance of Gaelic names by the whole Irish people help or hinder the peace process? North of the Border, would the SNP demand different names on the PC and the Macintosh? If Cornish is an extinct language, would Cornish-spoken APL bring it to life again, or might the other thing happen?

I began to fantasise about touring the world on an APL scholarship, armed with a tape recorder and a copy of I-APL, discovering how different primitive tribes pronounced the APL primitives and release my findings into the public domain just in time for APL 2000. The talking part’s very easy on the Mac (I’ve already got one that speaks numbers) and Windows says “Me-Too” nowadays – if you install a Sound-Blaster – but you’ve spotted the snag, of course. It would need a built-in syntax analyser just to determine whether ‘?’ is Roll or Deal, or just a plain query inside a message string. It might be one of those recursively-unsolvable problems when taken across the whole ensemble of possible APL interpreters. In Dyalog APL you can define a new function like this:

```
MYFUN←∘.⌊
```

\- so what’s the machine to say when it reaches the end of the expression and finds no right argument?

Other mathematico-philosophical movements have foundered on their nomenclature, especially when you supply not just one, but two or more new names for things your audience already has names for, like good old question-mark. I spent half the seventies trying to get people to call their files “relations” and their records “tuples”. Not singlets, doublets and triples, mind, but 1-tuples, 2-tuples, 3-tuples, 4...

Needless to say, our band of high fliers ran into a lot of flak, even from academics, who really ought to have known better. Isn’t the whole of academic life all about learning to call everything by its right name (Augustine of Hippo, I believe)? The Company cherished us, like the Mikado, as a source of innocent merriment, but I chucked it in and spent the rest of the seventies researching why people found computers so difficult.

Eh, what’s that? Did I discover the reason? Well... no, not entirely. But there are things you can do to be helpful, and things which hinder. Introducing a lot of new names and new concepts with no apparent one-to-one mapping between them is not one of the helpful things to do.

Yes, read my lips. What I’m saying is that the strange characters of APL aren’t the problem with the language – that’s if you accept there is a problem. It’s the names for them. Who complains about code-page 437, I ask you? Yet everyone uses it, everyone still using DOS that is, and it’s full of the most bizarre glyphs – Wingdings comes nowhere by comparison. And they all have names, every last jot and sigil of them.

So I think I’ll stick with the present title for now, until I can think of a better one. Or a Spanish one, perhaps? Or in one of those Tintin-esque East European languages. It would be fun to see the actual names of the primitives decorated with slashes and backslashes, jots and dots, tildes and carets, all liberally laced with each-pepper.

(webpage generated: 18 October 2006, 03:52)
