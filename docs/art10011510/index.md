---
vid: '10011510'
title: Building OO applications in Dyalog version 11
prefix: Course
authors:
- Stephen Taylor
byline: Stephen Taylorsjt@lambenttechnology.com Gilgamesh Athorayae9gille@googlemail.com
volume: '23'
issue: '1'
page: '17'
description: Vector, the journal of the British APL Association
keywords:
- algebra
- APL
- array
- computation
- computing
- J
- K
- mathematics
- programming
- programming language
- programs
- Q
source: trad/v231/objects.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- development practice
- language design
- applications
---

Stephen Taylor  
[sjt@lambenttechnology.com](sjt@lambenttechnology.com)  
Gilgamesh Athoraya  
[e9gille@googlemail.com](e9gille@googlemail.com)
{ .byline }

We were privileged to be invited to help Dyalog prepare Version 11 for release. And we abused that privilege. Roundly. Instead of trying to make the interpreter misbehave, we misbehaved. We used the release candidates to rebuild the major application we were developing that year for a pension company.

Like any developers working on a mature system, we itched for a new start to exploit the insights we’d won into the application. And, like good APL programmers, we were keen to see how *lightly* the system could be written, especially using the new tools.

We were already making extensive use of the object model in a subsystem that marks up plain text to produce RTF files, that Microsoft Word is happy to open, edit and print. But our crude object model, implemented with Dyalog namespaces, made instance objects simply by taking deep copies of the class object. This is inefficient. Worse, changes to instance methods don’t propagate automatically back to the class. But primarily, we were excited about seeing how OO might clarify and simplify our code, what our GUI code might look like, and what our code management would be like using scripts. (Could we collaborate using Subversion, for example, as so many writers of other languages do?)

Stephen got our first release candidate on Kefalonia in May 2006 and, to his partner’s horror, sat up nights under the stars writing the first version of what came to be called Surrender to the sound of goat bells tinkling over the mountainside. This looked very exciting when brought back to England. Looking for maverick shortcuts, and inspired by A+, he’d derived the object model from GUI classes. Every instance had a native GUI representation – no mapping to be done! Objects had their own Browse methods, which would let you edit them.

Exhilarating as this was – the code had just melted away – we discarded this as a dead end. (It supports only single views of each object’s contents; worse, each GUI object consumes limited Windows GUI resources. It would be inefficient in batch – i.e. non-GUI – use, and unlikely to scale to arrays of objects.)

We then set out on a more conventional path, separating the presentation layer from the object model. Because we thought we would probably want to deploy the system over an intranet from behind IIS, we used a structure isomorphic to ASP.NET, reasoning that an eventual port to ASP.NET would require rewriting only the modules we had modelled on ASPX pages. But for ease of development, and because we thought Remote Terminal a plausible alternative, we wrote all our GUI in Dyalog. Focused on progress rather than testing, we didn’t often stop to investigate what we couldn’t make work, more usually simply finding another way around.

By September, working in what we pleased to call our spare time, we had a simple version of the application creating, saving, opening XML claim files using object seralisation and deserialisation, interpreting and importing data from mainframe extract files, browsing the claims and producing correspondence in RTF and XML formats. All the code was held in UTF-8 `.dyalog` script files, using <acronym title="Simple APL Library Toolkit">SALT</acronym>, and assembled at runtime with a `.dyapp` file. We hadn’t reported many bugs, but we were able to show Dyalog that their release candidate would support an application using the new extensions.

We took this system to the Dyalog conference in Elsinore, where we attempted to share what we had learned with those attending Dan Baronet’s workshop on the OO extensions. We considered that unsuccessful, largely because Dan’s students had come simply to learn what the OO extensions were about; we were able to share very little of our experience.

Nonetheless we thought it would be of value to anyone else setting out down the same path, and offered to run a longer course, for students who had already worked through Morten’s [tutorial](trad/v221/oop1221.htm) on the OO extensions. This eventually turned out as a 5-day workshop in January this year, generously hosted by The Carlisle Group in Scranton, Pennsylvania.

Stephen always gets a kick out of visiting Scranton. Our Californian students were politely but explicitly dismayed at spending days in the heart of the American rustbelt. But for fans of 1920s provincial architecture, Edward Hopper paintings or Sinclair Lewis novels, Scranton is full of delights.

Gauging the schedule after Elsinore was challenging. By the third day of the workshop we saw how the remaining material could be reorganised around a practical task: building an address-book application, with a browser, images for contacts and so on. We negotiated with the students that we would start that after lunch that day, dividing into pairs to work. Everyone agreed it would be a useful way to work, even if no application got completed before we finished. In the event, everyone had a simple but working OO address-book application by five that afternoon.

Next time we run this we will offer it as a 3-day course, with an optional one or two days coaching on students’ own applications, to follow either directly afterwards, or after an interval. If you think this course might be of value to you, please contact the authors.
