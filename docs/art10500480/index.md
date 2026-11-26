---
vid: '10500480'
title: The APL Phrasebook Project
authors:
- Phil Last
byline: Phil Last
volume: '24'
issue: '4'
page: '37'
online: '2011-01-15'
editor: Stephen Taylor
description: The APL Phrasebook Project is a collection of cross-vendor idioms and phrases
keywords:
- idioms
- phrases
- library
abstract: Various collections of APL ‘idioms’ have been published over the years and the possibility of a cross-vendor phrasebook has been mooted a number of times.
source: content/printed/244/last.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- programming techniques
- APL community
---

Phil Last
{ .byline }

Various collections of APL ‘idioms’ have been published over the years and the possibility of a cross-vendor phrasebook has been mooted a number of times.
{ .abstract }

About a year ago Dick Bowman suggested to the BAA-London group in one of its regular monthly meetings that we might consider taking up the mantle of a pioneering effort that had been made some twenty years before to produce a phrasebook for as many APLs as would cooperate. After gauging initial interest Dick brought along an inch-thick sheaf of papers that were the only tangible evidence of that other project.

Chris Hogan scanned the whole lot and placed them on a web-site, a wiki, provided by Kai Jaeger.

Ellis Morgan then added cross-links and new pages to the wiki where phrases within a category were incorporated into a table. Provision was made for each phrase to be shown in a different version for a number of different APL implementations.

At a later meeting we made the decision to take a slightly different approach using the inbuilt wiki facilities to link individual pages, each of which centres on a single phrase. The phrase is accompanied by a full description including alternative codings but the main phrase is coded in such a way that it should work on as many platforms as possible including the *de facto* standard of APL2 and, as far as possible, the ISO Extended APL Standard[\[1\]](#ref1).

A page template assists in the making of a new page. This is complete and flexible enough to allow individual contributors to build a new phrase page within a short time, that will be published immediately.

The template includes headings for examples, how not to do it, conforming variants, specialities, compatibility and test cases.

The author succeeded in creating a new page from scratch well within a half an hour albeit without any test cases. He was also able to leave a preformed “Page under construction” notice on the page temporarily to excuse this shortcoming.

The hope is that the standardised headers will enable someone at some time to write software to link to the wiki as an extra resource for his or her own training material.

Wiki categories are used to group pages that share some arbitrary feature. A page is added to a category simply by naming the category at the bottom of the page. Clicking on a category name brings up that page with a list of all pages that name it. The phrasebook project has many categories but until there are many more phrases the most useful so far is `CategoryListAllPhrases`, to which all phrase pages belong.

The phrasebook project differs in several aspects from its predecessors such as the FinnAPL library:

## ⎕IO

The phrasebook uses `0` as the default value of `⎕IO`. The main reason for this is that dealing with .NET can be quite confusing otherwise. That does not mean that a phrase cannot use `1`, but `⎕IO` should then be set explicitly.

## APL2 syntax

APL2 is considered to be the general standard for modern APLs. However, this is of significance only in Dyalog, which is syntactically different by default, at least for the time being. One can force Dyalog closer to the APL2 syntax by setting its `⎕ML` (Migration Level) to the highest possible value, which is three at the time of writing.

## Test cases

Every phrase in the phrasebook is expected to come with proper test cases. The test cases not only ensure a high level of code quality, they also prove useful for compatibility reasons. Imagine someone writing a certain phrase in, say, Dyalog which you would like to use in, say, APLX: by copying and executing the test cases in your interpreter you can make sure that the phrase is doing what it is supposed to do.

Note that there is a button *Execute test cases* available on each phrase page. You may wonder what’s happening when you press this button. Well, quite a lot:

1. The link is sent to a particular port on the APL Wiki server where not Apache but MildServer[\[2\]](#ref2), a web server written in Dyalog APL, is listening.
2. MildServer then analyses the page, extracts the code and executes it. The rules are explained on the APL Wiki[\[3\]](#ref3).
3. MildServer then creates an ordinary HTML page, reporting either success or failure, which is then sent back to the client. To some extent this is arguably a dangerous thing to do; bad guys can easily write test cases which could bring MildServer down, or keep it busy forever. But then there are no bad guys in the APL community. (Phew. *Ed.*)

### Contributions

If you are tempted to contribute to the project but feel a little depressed after looking at the template offered: try to take the test cases as far as you can and then save the page. One of the administrators will sooner or later come along and fix any problems. By comparing your version with the newer one you can easily figure out what was changed.

The last thing to emphasise is that the project exists and is open for business. So far it contains only sixteen phrase pages, but we hope that will increase rapidly with more than the few current contributors.

We hope many of you will have a go and ‘get stuck in’. You need only an APL wiki account and a bit of time. If you have your own favourite phrase that you want to leave to posterity, or a whole library of them, all well and good. If not, there is a To-Do list that links back to sources for phrases if you just want to help.

Remember, it is a wiki and so no-one should be too precious about having his or her efforts ‘improved’ by others though the “Page under construction” notice will hold other contributors off for a while if you have temporarily to leave a page unfinished, while a similar “Please will someone improve this page” notice invites the vultures immediately.

## References

1. <a id="ref1"></a> [http://www.iso.org/iso/iso\_catalogue/catalogue\_tc/catalogue\_detail.htm?csnumber=22817](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=22817)
2. <a id="ref2"></a> [http://aplwiki.com/MildServer](http://aplwiki.com/MildServer)
3. <a id="ref3"></a> [http://aplwiki.com/PhraseBook/TestCasesGuidelines](http://aplwiki.com/PhraseBook/TestCasesGuidelines)
