---
vid: '10014230'
title: News from Sustaining Members
authors:
- MicroAPL Ltd
volume: '21'
issue: '2'
page: '9'
description: VECTOR news article
source: trad/v212/sm212ma.htm
converter: vecrescue 0.1.0
issue_tab: n2-february-2005
tags:
- implementations
- APL community
---

## MicroAPL Ltd

**April 2005** MicroAPL is pleased to announce the release of **Version 3 of APLX**, available for Windows, MacOS and Linux.

The new version is designed to extend the APLX philosophy of providing the easiest-to-use, most productive <acronym>APL</acronym> development environment on all the major desktop platforms. Of particular interest will be the new built-in charting, networking, image-manipulation, and database access facilities.

Key features of the upgrade include:

- A built-in, very powerful, but easy-to-use facility for producing business, statistical, and scientific graphs. At its simplest, you can use a new right-click menu item to bring up an instant chart of an <acronym>APL</acronym> variable. The new `⎕CHART` system functions allows the APL programmer to produce quick charts with almost no programming, and the `⎕WI` Chart object provides a full-featured way of placing graphs in your APLX applications. The types of chart which can be displayed include Line, Area, Scatter, Bar, Stacked Bar, Horizontal Bar, Horizontal Stacked Bar, High-Low-Open-Close, and Candlestick graphs.
- The new system function `⎕SQL` provides a built-in, extensible, cross-platform interface to standard relational databases such as Oracle, SQL Server, IBM DB2, MySQL, or PostgreSQL, using Structured Query Language (<acronym>SQL</acronym>). It can also be used (via <acronym>ODBC</acronym>) to access data from spreadsheets, Microsoft Access, db2 files, and many other data sources. You can use it to retrieve data from relational databases as nested arrays in the workspace, or to update a database directly from within <acronym>APL</acronym>.
- A new `⎕WI` Web-browser object allows you to incorporate web pages in your APLX application.
- New `⎕WI` networking and internet access facilities. The SendMail and GetMail objects allow you to send and receive e-mail under program control, and the HTTPClient object fetches pages and other files from web sites into APL variables. There is also a new Socket object for low-level networking.
- A new Image object. This is an interface to ImageMagick, allowing you to do image conversion to/from a wide range of graphics formats, as well as a huge range of image manipulation operations. You can use the Draw method (or a Chart object) to create the image to be manipulated or saved, or you can read it from file or from the clipboard.
- Other `⎕WI` enhancements include a ToolButton control, for buttons with images and for toolbars, a new tooltip property for hover-help, and new methods for coordinate conversion.
- New system functions `⎕TR` (translate text to/from APLX internal representation), `⎕SYMB`, `⎕WSSIZE`, `⎕LIB` In addition, the release incorporates a number of bug-fixes, and a major speed-up for the Linux version of APLX.

As with previous versions of APLX, all these facilities have been implemented so that, as far as possible, they work in the same way on all platforms, but take advantage of the strengths of particular systems. For example, the new Web Browser object is based on Internet Explorer under Windows, and Safari on MacOS, but presents the same programming interface in <acronym>APL</acronym>.

Time-limited but fully-functional demonstration copies of APLX Version 3 can be downloaded free of charge from our website <a href="http://www.microapl.co.uk/apl">www.microapl.co.uk/apl</a>.

***
