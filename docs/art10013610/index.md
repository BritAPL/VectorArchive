---
vid: '10013610'
title: 'Guest Editorial: Unicode Revisited'
authors:
- Adrian Smith
byline: Adrian Smith
volume: '19'
issue: '3'
page: '3'
source: trad/v193/ed193.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2003
tags:
- standards
- implementations
---

*Adrian Smith*
{ .byline }

*One Font to rule them all  
One Font to find them  
One Font to bring them all  
And in the darkness bind them.*

Here is something that seemed like a distant theoretical dream, which has suddenly become very urgent, which is why I asked Stefano if I could hijack the editorial slot for this Vector.

In the past, the different APL vendors have solved in different ways the problem of mapping `⎕AV` to the small number of available ASCII characters, which has meant that APL code often cannot not be easily exchanged between interpreters, let alone represented in email messages or newsgroups.

Surely, Unicode (although a distant dream) would be the final definitive answer to the cross-mapping between interpreters? Indeed, it should eventually enable APL code to be traded as easily in newsgroups as J and K code. Now that Windows Notepad fully supports Unicode, one can see newsgroup readers following along, and email software like Outlook should make it as easy to use APL in email messages as it already is to use Korean and Cyrillic.

*But first we must all agree on where the symbols go in the font.*

Can this still be an issue? IBM have worked hard behind the scenes to get APL firmly embedded in the unicode standard, and there is the official **Unicode Reference** available for us all to read. (See  [http://www.unicode.org/charts/PDF/U2300.pdf](http://www.unicode.org/charts/PDF/U2300.pdf) for a typical section.) So what’s the problem?

In the rush to ship the beta of .NET (and hence of Pocket APL) Dyadic mapped several of the APL symbols to the *Greek* section of the font, rather than the (visually identical) symbols in the *Miscellaneous Technical* section. To my shame, I did not pick this up until I had finally got so annoyed with the appearance of the APL symbols in Arial that I spent a few days dusting off my FontLab skills and made a fully Unicode APL font in the style of APLSans. Of course, I just followed the spec (as I understood it) and sent it off to Jonathan to test with Dyalog’s IME for V10. About 10 seconds later I got a phone call “Your font doesn’t work ....”. As you would expect, the very first thing he tried was `⍳12` which (being mapped to the Greek iota) failed dismally.

OK, not a big issue. Dyadic fixed these immediately (to the dismay of the PocketAPL testers who had terrible trouble getting the hunt-and-tap keyboard driver re-installed) but it was pure luck that the error was spotted in time. If this had got out of the door as a real shipped product, it would have been much harder to change. Just think of all those scripts that would suddenly stop working!

Nailing this one down turns out to have been the easy bit. The Unicode Standard may be fixed in stone, but there is (unfortunately) some scope for different interpretations. It turns out that three characters are still open to dispute:

- **Quad**. Unfortunately, when the standard was first drawn up, the Unicode people took the rather harsh view that if there was a matching glyph already defined, then oddballs like APL should use it. I can see the logic of this for symbols like `∩∪` where the APL function directly implements its mathematical precursor, but the only vaguely quad-ish thing was in the *Geometric Shapes* section at 25AF. After much protest, the APL cause was won, and something much more like a proper APL quad was added as a late runner to the *Misc Technical* block, and documented in the standard as such. The snag is that it never made it into Arial Unicode MS, so it looks very odd on screen.
- **Diamond**. There is one in *Geometric Shapes* at 25CA and one in *Mathematical Operators* at 22C4. They both look the same, so take your pick. Of course, APL2 and Dyadic picked different ones. Such is life.
- **Tilde**. Now this is a real annoyance. It is actually documented in the standard (in the maths section at 223C) as ‘APL tilde’ but it is visually the same as the ASCII tilde. For that matter there is a perfectly good 5-pointed star at 22C6 which is also noted as ‘APL’ in the standard. Now as far as I know, everyone uses the ASCII ‘\*’ as the APL ‘power’ so probably we should also use the ASCII ~ as our ‘without’ symbol. The snag is that anyone coming in from the cold and trying to do the right thing will read the standard and will choose the ‘correct’ one. In this case IBM chose ASCII and Dyadic chose maths.

## What Needs to Happen

We have been very fortunate in the co-operation between two (now 3) of the vendors of Gui APL systems in the near convergence of the mapping from `⎕AV` to the characters you see on screen. I could have typed that little fragment of APL into Dyalog, APL+Win or APLX and hit Ctrl+C to put it on the clipboard. Then I could have pasted it into either of the others and it would have worked correctly.

What we need next is a rapid agreement between all the vendors on these obvious danger points, and a published and accepted consensus on the entire font. Lee Dickey has already done sterling spadework which is well documented at:  
<i><a href="http://www.math.uwaterloo.ca/~ljdickey/apl-rep/tables/set3.html">http://www.math.uwaterloo.ca/~ljdickey/apl-rep/tables/set3.html</a></i>  
so perhaps he could look after the ‘definitive’ statement on his web-site?

There remains the interesting issue of how many alternatives an interpreter should accept as inbound mappings. There is already a precedent here – Dyalog APL will accept either variety of | as the ‘remainder’ symbol, so we could be relaxed about this and have interpreters map (for example) either `~` or ~ to the ‘without’ function. Somehow, I strongly dislike the idea. For one thing, it should be perfectly OK for a Greek APL user to type a`←⍳⍴`i as the a and the rightmost i are perfectly reasonable one-letter variable names in Greek! My instinct is to promote an APL Unicode font which has something resembling a particularly aggressive ‘NO ENTRY’ symbol at the ambiguous points which we have chosen not to use. Where there are valid Greek characters, they should be included (along with Cyrillic) but should be visually as different as possible. What do you think? You can check what it might look like on page 122, which has a full sample of the font and some more notes on the characters. Of course, the finished font will be available on the Vector website and will be freely redistributable. \[<i>Ed: This will probably be on the <a href="resource/index.htm">Resources</a> page within a couple of weeks.</i>\]

***
