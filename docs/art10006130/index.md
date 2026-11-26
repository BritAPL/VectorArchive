---
vid: '10006130'
title: 'Up, Down and Sideways: Extending the Reach of APL'
authors:
- Richard Nabavi
byline: by Richard Nabavi Managing Director, MicroAPL Ltd Talk given at the “All APLs Day” at Naples Beach
volume: '22'
issue: '2'
page: '91'
source: trad/v222/nab222.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- implementations
---

by Richard Nabavi Managing Director, MicroAPL Ltd  
Talk given at the “All APLs Day” at Naples Beach
{ .byline }

APLX, MicroAPL’s cross-platform APL interpreter and development environment, was introduced in 2001, although its roots go back many years. One of our goals in developing the product was to extend the reach of APL by providing a solution which would cover a wide range of users’needs, and overcome some of the obstacles which have prevented people using APL. Four years later, it seems appropriate to review how we are doing in attaining this goal, and to look at how we intend to respond to recent changes in the marketplace.

## Extending sideways, by supporting all the major platforms

APLX was launched simultaneously on Windows and MacOS, and as a Server Edition on AIX and Linux. Subsequently, a desktop edition under Linux was introduced, so that all the main desktop environments are supported.

The key point here is compatibility. APLX behaves virtually identically on all the platforms. A workspaces saved on any platform can be<span class="APLU"> )LOAD</span>ed on any other – and will run almost always without change, even if it makes full use of the <span class="APLU">⎕WI</span> system function for programming user-interface. Component files can be shared across different platforms. In addition, APL code can be easily ported from other APL implementations, especially APL2 and APL+Win, further extending the reach of the product.

One important platform, however, was missing: Solaris. We are now announcing that APLX will be available on Solaris from the 1<sup>st</sup> quarter of 2006, making APLX a near-universal solution.

## Extending sideways, by making the product easy to use

.APL has always been a wonderful language for exploring data and experimenting with algorithms. Perhaps, indeed, that is its greatest strength. The original design of APLX aimed at a user interface which would provide very powerful development facilities, without any distracting on-screen clutter.

In the recently-launched Version 3 of APLX, we added an important feature which extended this still further: built-in charting. At its simplest, you can use a new ‘right-click’ menu item to bring up an instant chart of an APL variable. For example, if you type:

```
WAVE←(2○0.25×⍳100)×*-0.05×⍳100
```

and then right-click on the variable name <span class="APLU">WAVE</span>, there is now a new ‘Display as Chart’ item in the pop-up menu. Selecting that item brings up a window containing an instant graph of the data:

![](./nab223_files/image002.gif){ width="601" height="287" }

If the variable is a matrix, APLX will make an intelligent guess about which rows or columns are the Y values and which (if any) the X values, but you can use the Chart menu to change these parameters, as well as selecting logarithmic scales or changing the type of the graph (for example, to a Scatter or Bar chart). If the chart is a nested matrix of strings and numeric values, the strings are used as labels of a bar chart. You can change this to a pie chart using the menu. For example, if you type:

```
SALES←2 4⍴'Apples' 'Oranges' 'Bananas' 'Plums' 445 223 564 101
```

and then use the pop-up menu item Display As Chart, the default will be to draw a Bar chart. You can use the Chart menu to change this to a pie chart:

![](./nab223_files/image004.gif){ width="432" height="334" }

If you want to take this a bit further, the new <span class="APLU">⎕CHART</span> system function allows the APL programmer to produce quick charts with almost no  programming. The right argument is the data, and optional left argument allows you to specify how the data should be displayed. If you do not specify, APLX makes an intelligent choice for you. For example:

Draw a sine wave. Because no X values are specified, implicit X values of 0, 1, .... 99 are used:

```
   ⎕CHART 1○0.1×⍳100
```

Draw a sine wave with the X values specified in degrees. We provide a two-column matrix; APLX assumes the second column is the X axis, because it increments regularly:

```
   ⎕IO←1
   ⎕CHART (1○(10×0,⍳36)×○2÷360),[1.5]0,10×⍳36
```

Draw a bar chart:

```
   ⎕CHART X←2 4⍴'Apples' 'Pears' 'Oranges' 'Plums' 445 323 345 765
```

Draw the same data as a pie chart:

```
   'type=pie' ⎕CHART X
```

Draw some random data as two, named series, against implicit X values 0, 1, 2..:

```
   'x=implicit' 'seriesname=US' 'seriesname=EU' ⎕CHART ?2 8⍴100
```

For the most control over the appearance of the graph, you can use a new <span class="APLU">⎕WI</span> object class, the **Chart**. This is a control which can display a range of different business, statistical and scientific graphs. The actual data which is displayed in the graph is held in one or more **Series** objects.

Although you can customize many aspects of the display of a Chart object, it is designed so that it automatically selects defaults which should give a good appearance for the chart with minimal programming. For example, if a chart is re-sized, by default the font sizes are adjusted appropriately, and reasonable values for the chart scale, tick positions, and labels are automatically chosen.

Charts by default are displayed in color. You can select your own colors for all the major elements of a chart, or you can leave the Chart object to select defaults. You can also ask for the chart to be displayed in monochrome.

As well as being displayed within your <span class="APLU">⎕WI</span> windows, charts can also be printed, copied to the clipboard, and saved to file in various formats. You can also customize a chart by adding your own graphics elements using the Draw method.

## Extending sideways, by interfacing to external programs and data sources

To be useful, APL systems need to exchange data and interface with other software. APLX includes a comprehensive range of such interfaces, including OCX/ActiveX controls, built-in networking through Sockets, E-mail Clients, an HTTP Client, and a <span class="APLU">⎕WI</span> Web Browser control. All of these are very easy to use, and all (except the Windows-specific OCX/ActiveX interface) work identically on the different platforms.

New in Version 3 is the <span class="APLU">⎕SQL</span> system function. This provides a built-in, extensible, cross-platform interface to standard relational databases such as Oracle, SQL Server, IBM DB2, MySQL, or PostgreSQL, using Structured Query Language (SQL). It can also be used (via ODBC) to access data from spreadsheets, Microsoft Access, db2 files, and many other data sources. You can use it to retrieve data from relational databases as nested arrays in the workspace, or to update a database directly from within APL.

In this sequence, we are using the sample database supplied with IBM’s DB2:

Connect to the database. The return code 0 0 0 0 means no errors occurred:

```
      ⎕SQL 'Connect' 'aplxodbc' 'DB2'
 0 0 0 0
```

Retrieve the number of rows in the ‘emp\_photo’ table:

```
     (RC ERRMSG DATA)←⎕SQL 'Do' 'select count(*) from emp_photo'
     RC
0 0 0 0
     DATA
12
```

No errors occurred, and the result was 12. We could retrieve all 12 rows immediately using the ‘Do’ keyword, but instead we’ll prepare a database statement which allows more control over the retrieval, as well as allowing us to see the types and names of the columns:

```
      ⎕SQL 'PREPARE' 'CS1' 'SELECT * FROM EMP_PHOTO'
 0 0 0 0
      ⎕SQL 'EXECUTE' 'CS1'
 0 0 0 1   SQL_CURSH200C4
      ⎕SQL 'DESCRIBE' 'CS1'
 0 0 0 0     EMPNO PHOTO_FORMAT     PICTURE
             EMPNO PHOTO_FORMAT     PICTURE
            CHAR 6   VARCHAR 10 BLOB 102400
                 0            0           1
```

 We’ll fetch the first five rows of the result:

```
      (RC ERRMSG DATA)←⎕SQL 'Fetch' 'CS1' 5
      ⍴DATA
 5 3
      2 ↑[2]DATA
 000130 bitmap
 000130 gif
 000130 xwd
 000140 bitmap
 000140 gif
```

The third column of the result contains binary data (pictures of the employees). At this point, we could display the pictures  using a Picture object in <span class="APLU">⎕WI</span>.

Close the statement, and disconnect from the database:

```
     (RC ERRMSG DATA)←⎕SQL 'Close' 'CS1'
     (RC ERRMSG DATA)←⎕SQL 'Disconnect'
```

## Extending downwards, by making the product easily available

One of the obstacles to the use of APL has been the relatively high price of most APL interpreters. To help address this, we have kept the price of APLX competitive. However, that is not enough; we also give away an earlier (Version 1.1) Linux version of APLX, absolutely free of charge for personal use. This has proved very popular, and now has many hundreds of users all over the world.

We also make available time-limited but fully-functional demonstration copies of our Windows and MacOS APLs. These can be downloaded from our website, and operate for slightly over a month. This has proved very popular. Indeed, it has had an unexpected but very welcome effect; a number of universities include an introduction to APL in their undergraduate courses, and the students download one of our demonstration APLs to get some familiarity with the language.

## Extending upwards:  The new 64-bit world is here!

The launch by AMD of a 64-bit version of the x86 architecture, a couple of years ago, has opened the way for low-cost, 64-bit systems in the mass market. Today, you can buy a desktop machine – or even a laptop – with a 64-bit processor, for just a few hundred dollars. Intel have now followed AMD with a binary-compatible equivalent, and, over the next few months, we can expect 64-bit x86 systems to become commonplace. Microsoft has a full 64-bit version of Windows available to run on these systems, and Linux is of course also available. In addition, Apple has sold 64-bit Macintoshes, based on the PowerPC architecture, for some time. 64-bit Servers are, of course, well established.

Such machines can address extraordinary amounts of memory. 32-bit systems are limited to at most 4GB of memory address space, and usually this is not all available to user processes (Windows, for example, normally has a limit of 2GB). Today, XP Professional x64 Edition supports up to 128GB of RAM and 16 Terabytes of virtual memory. In the future, even more memory will be usable. And the cost of RAM has fallen to such an extent that it is already possible to configure desktop machines with tens of gigabytes of memory for a couple of thousand dollars.

APL traditionally has been at the forefront of making full use of available memory. Back in 1982, MicroAPL made a big splash with the 68000-based MicroAPL Spectrum computer, which could offer a 1MB workspace size or more, at a time when nearly all micro-computers were limited to just 64KB. Now we are talking about memory sizes several thousand times bigger, but we see the same benefits: direct manipulation, in the APL workspace, of data which other languages have to process piecemeal.

Of course, it is not just the size of workspace which matters; it is also the maximum size of arrays. 32-bit APL systems use 32-bit slots to hold array sizes and dimensions. Thus, even if the processor can address more than 32-bits, it does not follow that an application (such as an APL interpreter) can handle arrays which will take full advantage of this memory.

For this reason, we have decided to introduce a fully 64-bit version of APLX, which will overcome all limitations in workspace and array size for the foreseeable future. APLX64 is this new product. It is initially being made available for Linux and Windows, with a MacOS version following. In APLX64, all array dimensions are 64-bit. In addition, integers are also 64-bit (otherwise you would have to use floating-point numbers to index arrays!). Booleans remain as one bit per element, making it possible to handle huge Boolean arrays without excessive memory requirements.

We believe this product extends the reach of APL upwards to applications which previously were beyond its reach. These include modelling and simulation using huge data sets, and OLAP applications for analysis and aggregation of large volumes of transactional data. In this new world, you do not need to compromise: you can just load the entire database into your APL workspace (the <span class="APLU">⎕SQL</span> system function will be handy here!). In some ways, this is taking APL back to the kinds of application where it used to be strong, but where it lost ground because the amount of data became too big to fit in the workspace.

## Conclusion: Up, Down, and Sideways

Extending the Reach of APL means ensuring that it is appropriate to a whole range of users and different needs, from the student learning about APL on a low-cost PC, to a retired Professor of Economics analysing stock-market data, to commercial programmers writing cross-platform solutions, to big corporations analysing huge volumes of commercial data.

And the beauty is, all of this can be done with the same product, using the core language specification of APL2, with the same <span class="APLU">⎕WI</span>-based user-interface components, and running in the same way on a whole range of different processors and operating systems.

***
