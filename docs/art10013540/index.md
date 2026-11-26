---
vid: '10013540'
title: 'Editorial 18.1: Nightmare Programming– Nightmare Data'
authors:
- Anthony Camacho
byline: by Anthony Camacho (acam@tesco.net)
volume: '18'
issue: '1'
page: '3'
source: trad/v181/ed181.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-2001
tags:
- development practice
- APL in perspective
---

*by Anthony Camacho ([acam@tesco.net](mailto:acam@tesco.net))*
{ .byline }

Once upon a time, when the first computers were still being invented, there was a distinction between a calculating machine and a computer. Both could do more or less any task that would fit in the very small memories of such machines. The essence of the difference was that a calculating machine’s program was unchangeable (maybe held on punched cards) whereas a computer had a stored program and the data in the program store could be amended in just the same way as any other data. One could program a branch by storing a different instruction in the sequence next to be obeyed.

Such programs soon became a nightmare of complexity; debugging was *difficult.* When the program kept changing and had thousands of different valid states, a program listing could not represent the program adequately. (APL was invented to solve the problem of representing a multi-million state machine.)

Some of us were outraged when it was suggested as a matter of policy or good programming style that programs should not modify themselves. If a program were made read-only then its state could be fixed and the tested version could be relied on not to change. Of course the problem of testing was still insoluble in the sense that it was not possible to prove that there was no bug left in any program. This soon became received wisdom and is now the rule to such an extent that there are few programs that cannot be run directly from read-only memory (most commonly CD ROM). We used to be able to back up just the data directories.

The total separation between programs and data did not last long.

Perhaps it was the invention of the macro that breached the dyke (or the macro that called Visual Basic). Perhaps it was the foolish introduction of the *object* paradigm where programming could be included with the data. The Dynamic Data Exchange Management Library (about 1990) led the way.

It is, in a way, a surprise that computer people who had assented to the proposition that programs should not be treated as data; not be dynamically changed eagerly accepted the proposition that data should contain programs.

The consequences are legion. The worm and the virus for one. Surely this one consequence is bad enough to justify re-introduction of the rule *programs and data shall be rigorously separated*. Maybe we could even have simple back-ups of just the data directories again.

***
