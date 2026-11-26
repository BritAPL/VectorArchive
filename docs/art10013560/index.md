---
vid: '10013560'
title: 'Editorial: Vector 18:3'
authors:
- Stefano Lanzavecchia
byline: Stefano Lanzavecchia
volume: '18'
issue: '3'
page: '3'
source: trad/v183/ed183.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2002
tags:
- data and files
- system interfaces
---

*Stefano Lanzavecchia*
{ .byline }

“Bert, what utter nonsense! Why do you always  
complicate things that are really quite simple?”  
“Kindly do not attempt to cloud the issue with facts”  
“Supercalifragilisticexbealidocious”  
*Mary Poppins, the movie*

XML (eXtensible Mark-up Language): did we really need yet another TLA (Three Letters Acronym)? I doubt I’d be able to give a crash course on XML, since despite its immense popularity, like many of you, I am still trying to understand what it is all about. First of all, a few facts: it is a language, that is, a verbal description of something formalised by a grammar; it has a simple, unambiguous syntax, that, admittedly, should be easily read by humans and parsed in a relatively easy way by computer programs; it’s used to describe tree-structured documents in which the nodes can be given a kind and be decorated with named attributes; its natural form is a flat text string.

When seen from the distance, an XML document looks like the source of an HTML page, with all those angular brackets, those double quotes and those slashes. Not surprisingly so, since they are both children of SGML. In fact, I dare say that the success of XML owes much to the success of the World Wide Web and its original Hyper-Text Mark-up Language. While HTML has a well-defined niche of application, to me it is still unclear what kind of data really benefits from being represented in XML.

A quick survey on the Web shows that people are trying to put XML clothes on just about any form of data. One simple use of XML is to give structure to configuration files. Another popular application is the encoding of messages between applications: from the simple XML-RPC to the all-inclusive newborn SOAP (Simple Object Application Protocol) which permits two peers to send to each other function calls with the associated data and receive structured results in a way that is general, flexible, cross-platform and, if needed, firewall-friendly when using HTTP as the transport layer. Text documents, spreadsheets, slide-presentations can be saved in smart XML format by most of the modern Office program suites. Entire relational databases are exchanged in XML shape. In fact the flexibility of XML is such that the limit seems to be only in the fantasy of the developers involved.

But there are technical limitations. The one I am particularly wary of is that an XML document is flat text: after having been parsed by a program, it can be kept in memory as a tree of pointers, so that document traversal as well updates in place can be as efficient as they need to be. But, by definition, when serialised for exchange the document must be represented by a simple string. This means that even to modify a small leaf, it’s necessary to read, parse, validate the entire document, and then rewrite it all once the changes have been applied. In a context where XML is used to exchange messages, or encode a small set of configuration options this is clearly not an issue. On the other hand, XML cannot be used to persist in an efficient way a multi megabyte relational database. To make the situation a bit worse from this same point of view, there’s the fact that XML is quite verbose. Not only binary data must be converted to text (for instance using Base64 encoding) before it can be inserted in an XML stream, but even simple text and numbers seem to drown in a swamp of tags, somehow defeating the original intent of human readability of the document.

Another technical limitation is that parsing XML, despite the deceiving look, is not trivial, especially in languages such as APL, where character-by-character analysis of a large string is a performance monster. Simple subsets of XML can be parsed efficiently by vector algorithms, as shown by Arthur Whitney’s ultra-concise parser coded in K, but the handling of all the subtleties of the XML specs is better left to experts and to fast scalar-oriented languages. Once again, modern APL programmers, instead of re-inventing the usual squared wheel, can resort to tools written in other languages. Microsoft’s XML parser, to mention one, is a COM component and it’s completely free: Jonathan Barman showed in a recent Vector issue (17.4) how simple it is to use it from Dyalog APL. In the Unix world, many Java parsers exist and Sharp APL’s Java can probably interface to them via its Java front-end.

Many grand unifying theories have been proposed since the dawn of modern computing, in the field of cross-platform, inter-process, inter-application exchange of information. Every time the developers were promised that it was only a matter of years before the new paradigm conquered the whole world and washed away all the previous troubles, finally concluding the chaotic middle-age of custom solutions. If you are preparing a presentation to the board with the future plans of your development team, don’t forget to spice it up with a bit of XML and some extra HTTP, XSD, XSLT, SOAP, WSDL, UDDI, SAX, XPath, XInclude (such wonderful acronyms!), WebServices and all the other related technologies. They are being advertised as the brand new ultimate magic tool to solve all the problems in computing: they play a fundamental role in Microsoft’s .NET platform, in Sun’s JavaOne, and other big players such as IBM are also funding research in the field. I will suspend the verdict until I know more and I have experienced pros and cons, but, nevertheless, I will continue to spend time investigating, experimenting, understanding, reporting on, and, whenever possible, having fun with XML.

***
