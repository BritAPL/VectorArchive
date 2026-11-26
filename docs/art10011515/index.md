---
vid: '10011515'
title: The APL Wiki
prefix: News
authors:
- Kai Jaeger
byline: Kai Jaegerkai@aplteam.com
volume: '23'
issue: '1'
page: '19'
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
source: trad/v231/wiki.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- APL community
---

Kai Jaeger  
[kai@aplteam.com](mailto:kai@aplteam.com)
{ .byline }

27 Oct 2008

Wikis are widely accepted and used nowadays, although not everybody is necessarily familiar with the concept of a wiki. Before telling you about the APL Wiki, it might be a good idea to review what a wiki is.

Basically, a wiki is a website which can be changed by everybody. It’s really that simple!

Change means: edit, add and remove pages. But why should one create or edit a page at a particular web site? To add a statement, fix a typo or contribute or improve content!

The word *wiki* is a Hawaiian word meaning <em>quick</em>\[[1](#ref1)\]. Wikis succeed in attracting content from a wide range of people, in part because they can be edited so quickly. Wiki interfaces are designed to be understood and mastered easily. They focus on the essentials, and nothing else. That is what has made wikis such an overwhelming success.

## Problems

What about spammers and malicious hackers? This is surely a problem, but the success of [Wikipedia](http://en.wikipedia.org/)\[[2](#ref2)\] tells us that wikis work anyway. Every day thousands of pages in the Wikipedia are deleted or flooded with rubbish. It works nevertheless.

A wiki keeps track of all changes. That means a good guy can restore a destroyed page with a single click. Nevertheless the problem is a large one, and almost all wikis nowadays require contributors to log in before editing. That makes it much harder for our beloved spammers to flood a wiki with rubbish.

In the APL Wiki you can freely add attachments, but this might not last. (Many other wikis are flooded with porn attachments.)

## Why an APL Wiki?

A wiki is a perfect environment to let people collaborate. Since Jsoftware Inc. introduced the [J Wiki](http://wiki.jsoftware.com)\[[3](#ref3)\] it has become a valuable source of information for the J community. By the way, the J Wiki was a model for the APL Wiki. One of the administrators of the J Wiki, Chris Burke, helped me to install and configure MoinMoin – thank you very much Chris!

My hope is that the APL Wiki becomes a recognised source for valuable information about APL: articles, examples, tasks, papers, solutions, libraries, frameworks

## Getting help

Especially for beginners it is important to get a helping hand with problems. The APL Wiki uses [MoinMoin](http://moinmoin.wikiwikiweb.de/)\[[4](#ref4)\], which offers a large number of help pages. The names of all help pages start with `HelpOn`. So entering `helpon` in the search box and pressing Enter returns a list of all the help pages.

Please note that pressing Enter triggers a title search. Clicking on the `Text` button has MoinMoin perform a full text search across all pages. Understandably, this type of search can take a while.

## Unicode and Fonts

The APL Wiki is encoded in UTF-8. That means that all modern browsers should display APL characters. As usual, Internet Explorer might cause a problem. The behaviour of IE5.5 and IE6 is unpredictable, as is version 7; but in general version 7 is much better. However, on some machines even IE7 does not display UTF-8 correctly, and nobody understands why.

Strictly speaking, you do not need a particular font: any Unicode font will do. The APL Wiki will use either the [APL385 Unicode](resource/apl385.ttf "download") font\[[5](#ref5)\] or the [APLX Upright](http://www.microapl.co.uk/download/aplx_unicode.ttf "download") font if one of them is installed on your machine, otherwise Arial Unicode MS or Arial is used. Note that any of these fonts should display APL characters. If this is not the case, it is likely to be a browser problem - try a different one. But few fonts have been designed to display APL *attractively*: you will see the best results by downloading and installing the APL385 Unicode font or the APLX Upright font.

Most of the advice on *Vector*’s help page “[Displaying *Vector* pages](display.htm)” applies to the APL Wiki.

## APL Code: Edit, Copy, Paste

Why is Unicode so important? Because you can copy and paste code between platforms supporting UTF-8! For example, you can copy APL code from an edit window in Dyalog APL or APLX and insert it into an article on the APL wiki. Put `{{{` and `}}}` around it and you are ready.

That also works the other way around: copy APL code from the APL wiki, insert into the session of Dyalog or APLX and it will run.

Google Mail optionally supports UTF8, too. So you can exchange APL code between sessions, edit windows, the APL wiki and Google Mail.

If you have installed Dyalog APL/W on your machine, you can use its Input Method Editor to edit APL code in the APL wiki. From within Microsoft Word or the MoinMoin article editor you can change the keyboard layout to APL and then type APL just as you would in Dyalog.

## Gaining experience

Of course you should get some experience before starting serious contributions to any wiki. For that, the APL wiki provides a [Test Page](http://aplteam2.com/aplwiki/TestPage).

Note that the content of the Test Page and any pages associated with this page will not persist.

## Creating an account

Even in the test wiki you need to create an account first. (Otherwise the test wiki would soon become a porn site.) It is recommended you use CamelCase for your username: e.g., `JohnDoe` rather than `John Doe`. This way you can easily add links pointing to your internal ‘home page’. (You might not wish to create such a page right now but there is a good chance you will later.)

Note that passwords and usernames are case sensitive.

In case of a forgotten password you can ask the wiki to send you an email with your password hash, which MoinMoin will accept instead. Then it would be smart to change your password! (MoinMoin does not save passwords but hashes, so it can tell you only the hash. A hash can be generated from a password, but there is no way to recover a password from its hash.)

## Editing

To edit a website, normally you need to know HTML. Although HTML is simple enough, wikis make editing even easier with an extremely simplified markup. For example, to mark up a top-level header one would say:

```
= Heading =
```

A second-level header would look like this:

```
== Subheading ==
```

Here you see the markup for an unordered list:

```
 * a topic
 * another topic
```

An example for an ordered list:

```
 1. First item
 1. Second item
```

It does not matter which number is used – any number will do. Note that for list definitions the first character on a line must be a blank.

Finally an example for a bold word:

```
This is a '''bold''' word
```

Besides lists and headers, you need to know how to create a link. In general, there are two different kinds of links: *internal* and *external*. The easiest way to create an internal link is to use CamelCase syntax. Words written in CamelCase are automatically created as an internal link. There are two possible problems:

1. Sometimes you want to use CamelCase without creating a link. For example, mentioning the most popular version-control software, SubVersion, results in an internal link. Fortunately, there is an easy way to tell MoinMoin that you do not want such a link: `!SubVersion` will do the trick.
2. Sometimes you want a link something like `DyalogAPL`. That does not work, but `DyalogApl` looks ugly. In those cases you can use some of the more sophisticated markup to create a link. Refer to `HelpOnLinking`.

External links can be created with expressions like this:

```
[http://www.dyalog.com Dyalog APL Version 11]
```

`Dyalog APL Version 11` then becomes a hyperlink.

Since we are talking about an APL Wiki, we need to know how to enter code. Everything between `{{{` and `}}}` gets a box with a distinctive background color. (All whitespace is preserved.)

The syntax we’ve discussed so far should already be good enough to start your career as an author on the APL Wiki. Don’t hesitate!

## How to find an article

Apart from the search facilities offered by any Wiki, linking is of course very important. A page that no other page links to is hard to find. MoinMoin will identify those pages easily: enter `Orphan` in the search box and you get a list of pages with no inbound links.

MoinMoin also offers *Categories*. The idea is to put a string like `CategoryFoo` at the end of a particular page. Since this is a CamelCase word, a link is automatically created from this. If there is a page with the name `CategoryFoo`, it is supposed to create a list of all pages that have the word `CategoryFoo` on them. That makes it easy to keep pages together that share something. But even more important, the mechanism needs no attention: as soon as a new page is created which contains the word `CategoryFoo`, the CategoryFoo page will automatically include this page in its list.

Of course you can create new categories, but please be careful when doing this. Having too many categories is clearly counterproductive. And when you create a new category, you are supposed to create the category page itself, of course. (You can copy the content from an existing one.)

## Conclusion

We now have a collaborative platform in place. Let’s start to use it. For those of us making our living with APL it is also a way to share tools and frameworks.

My hope is that the APL Wiki will become a valuable source of information and code, attracting old hands and newcomers. It clearly has that potential – but only the APL community can make it a success. It is up to you!

### References

1. Ward Cunningham, The WikiWikiWeb, <a id="ref1"></a>[http://c2.com/cgi/wiki](http://c2.com/cgi/wiki)
2. Wikipedia, <a id="ref2"></a>[http://en.wikipedia.org/](http://en.wikipedia.org/)
3. Jsoftware Inc., The J Wiki, <a id="ref3"></a>[http://wiki.jsoftware.com/](http://wiki.jsoftware.com/)
4. MoinMoin, The MoinMoin Wiki Engine, <a id="ref4"></a>[http://moinmoin.wikiwikiweb.de/](http://moinmoin.wikiwikiweb.de/)
5. Adrian Smith, <a id="ref5"></a>[APL385 Unicode](resource/apl385.ttf "download")
