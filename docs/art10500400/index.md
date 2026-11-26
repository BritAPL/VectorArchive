---
vid: '10500400'
title: Understanding font embedding
authors:
- Kai Jaeger
byline: by Kai Jaeger (kai AT aplteam DOT com)
volume: '24'
issue: '4'
page: '83'
received: '2010-04-29'
online: '2010-08-29'
description: How to embed the APL385 Unicode font into a web page.
keywords:
- APL
- font
- embed
- EOT
- TTF
- Embeddeed Open Type
- TrueType
- CSS
- HTML
abstract: Explains how to embed the APL385 Unicode font into a web page. Adapted from an article in the APL Wiki.
source: content/printed/244/jaeger.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- user interfaces
- system interfaces
---

by Kai Jaeger (kai AT aplteam DOT com)
{ .byline }

Explains how to embed the APL385 Unicode font into a web page. Adapted from an [article](http://aplwiki.com) in the APL Wiki.
{ .abstract }

At the BAA London meeting in February 2010 my friend Stephen Taylor showed me his latest gadget, a brand-new iPhone. It’s a fascinating piece of hardware. I tried this and that and finally told it to go to the [FinnApl Idiom Library](http://aplwiki.com/FinnAplIdiomLibrary) on the APL Wiki. [\[1\]](#ref1)

I got so excited about what I saw that a fellow APLer asked me what was wrong. I told him that nothing was wrong at all: I was simply thrilled that the iPhone not only showed the web site properly with APL characters, it actually used the [APL385 Unicode](resource/apl385.ttf) [\[2\]](#ref2) font for this. “Well, it’s Unicode, so what?” was all my colleague had to say.

True, the APL Wiki is a Unicode web site, but that does not explain why the iPhone used the APL385 Unicode font. One thing is certain: that font is not installed on an iPhone by default. The magic that makes this work is called *Font Embedding*, and that’s what this article is about. I expected all modern browsers to support font embedding – but not an iPhone!

Font embedding means that when a web site is in need of a particular font to display a particular piece of text then this font is downloaded and installed temporarily by the browser in the background. This technology was demanded by web designers who want full control over their page layouts, but as APLers we are benefitting from this as well. It’s a chance to display APL code correctly on any machine even if that machine does not have an APL Unicode font installed. This article provides background information about this exciting technique as well as hints and tips regarding the usual obstacles. You need a basic understanding of [HTML](http://www.w3.org/html/) [\[3\]](#ref3) and [CSS3 font rules](http://www.w3.org/TR/css3-fonts/) [\[4\]](#ref4) in order to take advantage of this.

## History

Font embedding was introduced by Microsoft with Internet Explorer 4 a long time ago. This was never adapted by other browsers, and there was a reason for that: IE accepts only *Embedded Open Type* (EOT) fonts.

Now this format, although it has its merits, was owned by Microsoft, and Microsoft never even tried to make it a standard. For that reason other browser vendors kept away from this format. They had to.

In 2009, two things changed. Firstly, almost all other browsers started to implement font embedding, although in a different way. Secondly, Microsoft made the EOT-format public and at the same time started to make it a W3C-accepted standard.

That’s good news, although for the time being it means that one has to write different CSS rules for IE and all the other browsers.

## IE and Embedded Open Type fonts

EOT addresses two important issues:

- The font file can be compressed, meaning that it contains not all but only those characters of a particular font used on a particular web site.
- It can be bound to a particular URL. In other words, only when the font is downloaded from a URL contained in the EOT file itself will IE make use of it. This approach was taken to address licensing issues.

The way Microsoft has implemented it in terms of CSS rules is unfortunately different from all the other browsers. You cannot use `local` to avoid a download when a suitable font is installed locally, nor the `format` hint to restrict a rule to a specific font format.

## TrueType fonts

TrueType fonts (TTF) are used by the rest of the world for embedding fonts. Unfortunately there is no way to address licensing issues. This is bad news because it means that, strictly speaking, one may embed only free TTFs. The good news is that thanks to Adrian Smith, the owner and originator of APL385 Unicode, everybody may distribute the APL385 Unicode TTF freely, so we don’t have licence issues.

## Converting a TTF font into EOT

In order to use the APL385 Unicode font for font embedding with IE we need to convert it into the EOT format.

If you are interested in how to do this, details are available on the [APL Wiki](http://aplwiki.com/UnderstandingFontEmbedding#Converting_a_TTF_font_into_EOT). [\[5\]](#ref5)

But you don’t have to. Instead you can download a [zipped version of the EOT file](http://aplwiki.com/UnderstandingFontEmbedding?action=AttachFile&do=view&target=apl385.eot.zip). [\[6\]](#ref6)

## The CSS

For the time being one needs write special CSS rules in order to make font embedding work with all modern browsers. Since this is likely to change over time, it is not covered in this article. For details see the APL Wiki [\[5\]](#ref5). In this article I am going to discuss the CSS on a general level.

Note that the link `http://misc.aplteam.com/apl385.eot` used in the examples is a real one but of course you should not use it in your own CSS files.

```
@font-face {
  font-family: "APLFont";
  src: url("http://misc.aplteam.com/apl385.ttf");
}
```

Now `APLFont` can be used in the same way as any other font-family in CSS. An example:

```
pre {
  font-family: APLFont, monospace;
}
```

This tells the browser to use `APLFont` for any `pre` tags. If that fails for any reason (eg if the URL produces a 404 Not Found error), then the browser will use a local monospaced font instead.

A second example:

```
@font-face {
  font-family: "APLFont2";
  src:
    local("APL385 Unicode"),
    local("APLX Upright"),
    local("Courier APL2 Unicode"),
    local("SImPL"),
    local("SiMPL medium"),
    url("http://misc.aplteam.com/apl385.ttf") format("truetype");
}
```

This example uses `local` and `format` as well. These are defined by the CSS3 standard, but not implemented by IE. (IE9 is expected to implement it fully.)

Note that in this example the browser is told to use a locally installed font, APL385 Unicode. Only if this fails will the browser continue to the next statement. If none of the specified fonts is actually available on the current system, it finally will download the APL385 Unicode font and try to use it as a TrueType font according to the `format` specification.

The advantage is that the font is downloaded only when no local font is found. It simply saves time.

## Who will take advantage?

Under Windows, these browsers are fine:

- Chrome 4.0 and better
- Firefox 3.4 and better
- Internet Explorer 6, 7 & 8
- Opera 10.0 and better
- Safari 4.0 and better

Note that using NoScript with Firefox stops font embedding from working. Reason is that downloading anything is potentially a dangerous operation, so by default NoScript blocks the CSS font-face rule. This can be changed on the "Embeddings" tab in the "Options" dialogue of NoScript: untick the "Forbid @font-face" box and it will work.

However, if you declare, say, http://aplwiki.com as to be trusted which is probably a good idea anyway then not only JavaScript but also font-embedding will work.

This is new stuff, so make sure that you always use the latest version of your preferred browser.

## Strange effects

Browsers use different strategies to display a page which comes with embedded fonts. Safari, for example, does not show anything until it is absolutely clear how the page is going to look. As a result some people complain that the browser is slow. That’s not exactly true, because it’s a strategic decision made by Apple.

Firefox on the other hand tries to display something as soon as possible, even if the final layout might be different. Therefore Firefox comes up with something quite fast. However, for an embedded font Firefox uses one that is locally available. As a result the page changes more or less dramatically when the embedded font finally becomes available. Some people criticise this as distracting. Again this is a strategic decision.

Since APL glyphs might be completely unreadable with any font chosen by chance I prefer Safari’s approach, although Firefox is my preferred browser.

## Help! It still does not work for me!!

The way forward is to install a piece of software on your preferred browser that allows you to check the http headers. Chrome, IE and Firefox all have tools available. They will tell you exactly what the browser is requesting and what the server is delivering.

It might be helpful to see  [a working example](http://misc.aplteam.com/APLCharTestUnicode.html) [\[7\]](#ref7) – an HTML page with some background information. It makes use of font embedding by referring to this [style sheet file](http://misc.aplteam.com/fontface.css) [\[8\]](#ref8)

To test font embedding you need either a machine which never saw an installed APL font or a Virtual Machine with the same property. De-installing all APL fonts might not work: sometimes strange caching effects take place by both the underlying Operating System and the browser(s).

## The future

We can expect that

- Internet Explorer 9 will process both `local` and `format`.
- EOT will become a W3C-recognized standard and sooner or later all other browsers will accept EOT fonts as well.

Mozilla has proposed WOFF (Web Open Font Format) which will be supported by Firefox 3.6. This new format addresses licensing issues and compresses fonts heavily. For details see [hacks.mozilla.org/2009/10/woff/](http://hacks.mozilla.org/2009/10/woff/) [\[9\]](#ref9)

The future is bright.

Latest updates, working CSS files and technical details are to be found on the APL wiki [\[10\]](#ref10)

## References

1. <a id="ref1"></a> Example page on the APL wiki with APL chars  
    [http://aplwiki.com/FinnAplIdiomLibrary](http://aplwiki.com/FinnAplIdiomLibrary)
2. <a id="ref2"></a> APL385 Unicode font file  
    [http://www.vector.org.uk/resource/apl385.ttf](resource/apl385.ttf)
3. <a id="ref3"></a> HTML  [http://www.w3.org/html/](http://www.w3.org/html/)
4. <a id="ref4"></a> CSS3 font rules  [http://www.w3.org/TR/css3-fonts/](http://www.w3.org/TR/css3-fonts/)
5. <a id="ref5"></a> Converting a TTF font into EOT  
    [http://aplwiki.com/UnderstandingFontEmbedding  
    \#Converting\_a\_TTF\_font\_into\_EOT](http://aplwiki.com/UnderstandingFontEmbedding#Converting_a_TTF_font_into_EOT)
6. <a id="ref6"></a> APL385 Unicode in EOT format  
    [http://aplwiki.com/UnderstandingFontEmbedding  
    ?action=AttachFile&do=view&target=apl385.eot.zip](http://aplwiki.com/UnderstandingFontEmbedding?action=AttachFile&do=view&target=apl385.eot.zip)
7. <a id="ref7"></a> Sample UTF-8 web site  
    [http://misc.aplteam.com/APLCharTestUnicode.html](http://misc.aplteam.com/APLCharTestUnicode.html)
8. <a id="ref8"></a> CSS file illustrating font embedding  
    [http://misc.aplteam.com/fontface.css](http://misc.aplteam.com/fontface.css)
9. <a id="ref9"></a> Web Open Font Format  
    <a href="http://hacks.mozilla.org/2009/10/woff/">
    			    http://hacks.mozilla.org/2009/10/woff/
    			  </a>
10. <a id="ref10"></a> Latest updates, working CSS files and technical details:  
    [http://aplwiki.com/UnderstandingFontEmbedding](http://aplwiki.com/UnderstandingFontEmbedding)
