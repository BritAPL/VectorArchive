---
vid: '10013630'
title: BAA AGM Meeting – Vendor Presentation
authors:
- Adrian Smith
byline: Adrian Smith
volume: '20'
issue: '1'
page: '44'
source: trad/v201/vend.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-2003
tags:
- implementations
- APL community
---

*Adrian Smith*
{ .byline }

## John Scholes of Dyalog Ltd

John gave his usual laconic amble through the latest release of the Dyalog APL interpreter.

> “We’ve been short-changing you for years – we never used the right-hand column of the session! With Version 10 you get 2% more columns!!”

Dyalog have also followed +Win in adding the option of automatic `⎕PW` and have taken a small walk on the wild side by allowing deletion of unwanted material from the APL session. Personally, I still rate the total anarchy of APL\*PLUS/PC where you just typed over anything!

John showed how easy it now is to export your application as a bound executable (you also get to choose the icon) which is quite a step forward for serious developers. It makes it very much simpler to set up the ‘associations’ for your filetypes, allowing Windows to start you automatically on (for example) the user double-clicking one of your datafiles. Many of us have written ‘trivial’ launcher applications in C++ or Delphi which have little purpose other than providing Windows with a genuine .EXE to call. Hopefully, these can now head straight for the long grass where they belong.

In answer to a question from the audience, John gave us a brief outline of how the tokeniser treated idioms. Basically a ‘spare’ token **1B** acts as a marker “Here comes an idiom” and is followed by the token for the idiom. However the original code is left inplace so that version-9 can still run the V10 workspace (this was required as PocketAPL is effectively V10 but came out before the real V10 for general users). There is effectively no cost attached to the addition of more idioms, except that the interpreter gets a little bigger each time. All the recognition happens when the function is fixed, which is also why loading a V8 or V9 workspace into V10 takes noticeably longer. John recommended taking the time to resave applications destined for shipping in V10. Two more things to mention – memory mapped files are now implemented, and everything is around 10% faster simply as a result of switching to the Microsoft compiler.

***
