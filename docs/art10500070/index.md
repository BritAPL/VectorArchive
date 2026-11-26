---
vid: '10500070'
title: 'In Session: Congratulations not in order?'
prefix: In Session
authors:
- Stephen Taylor
byline: Stephen Taylor (sjt@5jt.com)
volume: '24'
issue: '1'
page: '99'
source: content/printed/241/insession.htm
converter: vecrescue 0.1.0
issue_tab: n1-december-2008
tags:
- programming techniques
---

Stephen Taylor (sjt@5jt.com)
{ .byline }

The card popped out of its envelope bearing the single word *Congratulations* surrounded by what looked like anagrams of it.

This kind of thing preys on one’s mind. What *was* my sister thinking? I *don’t* have time on my birthday to stare at the phrases and decide whether they really *are* all anagrams or just *look* like they are, or just *some* of them are. Need to resolve this question quickly and get back to typesetting <cite>Vector</cite>…

Let’s see… if we alphabetise… and ignore spaces…

```
      {⍵[⍋⍵]~' '}
```

then all that’s left is to compare the results…

```
      'congratulations'{⊃≡/{⍵[⍋⍵]~' '}¨⍺ ⍵}'iron nuts catalog'
1
```

Gluing on the left argument gets me a monadic test function.

```
          test←'congratulations'∘{⊃≡/{⍵[⍋⍵]~' '}¨⍺ ⍵}
      test¨↓⎕←↑ANAGRAMS
nora taunts logic
iron nuts catalog
using cool tartan
caution long star
stout groin canal
so not a garlic nut
ration aunts clog
uncool giant rats
auto girls cannot
tuscan train logo
trust colon again
groan until ascot
snail conga tutor
1 1 1 1 1 1 1 1 1 1 1 1 1
```

There we go. Anagrams tested, male autism appeased.
