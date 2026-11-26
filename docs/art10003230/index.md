---
vid: '10003230'
title: Matrikx Reloaded
authors:
- Simon Garland
byline: by Simon Garland (simon@kx.com)
volume: '20'
issue: '1'
page: '49'
source: trad/v201/matrx201.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-2003
tags:
- implementations
- education
---

*by Simon Garland (simon@kx.com)*
{ .byline }

Although the array language K is immediately accessible to programmers familiar with APL or J, comparatively few have spent any time looking into it. For anyone who wants to add a new language to their toolkit this short article describes how to download and install a trial version – and points to further information for those who would like to learn more.

## Installation:

Download and run a copy of `kdbsetup.exe` from the download page [\[2\]](#note2) of the Kx website [\[1\]](#note1), this 200K file will install the array language K and the database Kdb. It will install on any dialect of Windows, although NT or XP are more comfortable to use as the 95/98/ME command prompt is so clunky.

The trial version has restrictions on the amount of data it will work with, and the number of messages that can be sent between K sessions – but otherwise is a full version.

## Try K:

Open a command window, and type

```
k
```

and you’ll be in immediate execution mode in a K session, try a few familiar things:

```
C:\k>k
K 2.95t 2003-04-03 Copyright (C) 1993-2003 Kx Systems
Evaluation. Not for commercial use.
\ for help. \\ to exit.
  2+2
4
  2+2 3 4
4 5 6
  +/2 3 4
9
```

to get out just type `\\` in immediate execution mode.

K includes a built in reference, access it by typing a single `\` followed by a return in immediate execution, that will give you a list of pages available. Enter the particular one you want to see (\\+ to list the verbs for example).

If you type something complicated and get suspended you can clear the execution stack with a single `\` at the suspension prompt. Here a nonsense function `foo` is defined to add its argument to a string and cause an error:

```
  foo:{"string"+x}
  foo 17
type error
{"string"+x}
         ^
>  \
```

If you just want to disable suspensions while looking around enter the command `\e 0` on a new line – the error will still be displayed but the stack will be cut back:

```
  \e 0
  foo 17
type error
Try Kdb:
```

Get out of your previous K session if you still have it open by entering `\\` and then start a Kdb session by entering:

```
k db
this time you’ll be in a kdb session as shown by the ksql> prompt:
C:\k>k db
K 2.95t 2003-04-03 Copyright (C) 1993-2003 Kx Systems
Evaluation. Not for commercial use.
\ for help. \\ to exit.
KDB 2.5t 2003-06-25
ksql>
```

as Kdb is a database we’ll load in a sample database to make things more interesting, at the `ksql>` prompt type in:

```
ksql>load 'trade.t'
ksql>
Note the single quotes, strings are delimited by double quotes in K, single quotes in Kdb – that makes it easier to embed KSQL strings in K code.
Now try some simple KSQL:
ksql>tables
`stock `trade
ksql>vars stock
`stock `industry
ksql>count stock
6
ksql>select from stock where industry='auto'
.((`stock
   `xx `yyyy
   .,(`K;1;))
  (`industry
   `auto `auto
   ))
ksql>count trade
100000
ksql>vars trade
`stock `price `amount `date
ksql>select avg price by stock from trade
.((`stock
   `aaa `bbb `ccc `dd `xx `yyyy
   .((`T
      ,`stock
      )
     (`K;1;)))
  (`price
   74.93937 74.93437 74.89528 74.95792 74.73562 74.71044
   ))
ksql>
```

Notice that the table columns are just vectors: At last – a database that you can work with not against/despite! You can open a simple viewer on the database by typing in:

```
ksql>show ''
```

or you can view the results of a query by prefixing a show to the constraint:

```
ksql>show select avg price by stock from trade
```

you can escape from Kdb to K by entering a single \\ at the ksql> prompt

```
ksql>\
  2+2 3 4
4 5 6
```

and you return from K to Kdb by entering `` .d.l` ``

```
ksql>\
  count stock
value error
count
^
parse error
  .d.l`
ksql>count stock
6
ksql>
```

exit from a Kdb session with `\\` as you would from a K session.

Summaries of KSQL, and the Kdb SQL92 implementation can be found in the two files `s.txt (SQL92)` and `t.txt (KSQL)` in the directory `c:\k\db` created when you installed kdb earlier.

## Documentation:

If the 5-10 minutes it took to try this out has caught your interest the best way to learn the basics of K and Kdb is to read through the Kdb manual *Getting started with Kdb* available from the Documentation page [\[3\]](#note3) on the Kx website. Although its focus is on Kdb it takes you through the basics of K as well.

Apart from the other manuals on the Documentation page [\[3\]](#note3) there is a K reference card available from Mike Powell’s directory on the User Contributions page [\[4\]](#note4).

When learning a new language it’s often helpful to have other people’s code to read, there is a large library of K and Kdb code on the Kx site in Arthur Whitney’s directories [\[7\]](#note7).

More general articles about K and Kdb are indexed on Eberhard Lutz’s Kaylang page [\[8\]](#note8)

## More Information and Help:

If you have questions about K or Kdb the Kxforums [\[5\]](#note5) are open to anyone, or the K and Kdb listboxes can be searched from the archive page [\[6\]](#note6)

### Links:

> <a id="note1"></a>\[1\] [http://kx.com](http://kx.com) – home of k and kdb  
> <a id="note2"></a>\[2\] [http://www.kx.com/download/download.htm](http://www.kx.com/download/download.htm) – download k and kdb  
> <a id="note3"></a>\[3\] [http://www.kx.com/download/documentation.htm](http://www.kx.com/download/documentation.htm) – manuals  
> <a id="note4"></a>\[4\] [http://www.kx.com/download/contributors.htm](http://www.kx.com/download/contributors.htm) – user contributions  
> <a id="note5"></a>\[5\] [http://kxforums.com/kxf/](http://kxforums.com/kxf/) – user forums  
> <a id="note6"></a>\[6\] [http://www.kx.com/listbox/index.html](http://www.kx.com/listbox/index.html) – k and kdb listbox archives  
> <a id="note7"></a>\[7\] [http://kx.com/a/](http://kx.com/a/) – sample code  
> <a id="note8"></a>\[8\] [http://www.e7l3.com/kaylang.html](http://www.e7l3.com/kaylang.html) – extensive list of k articles

***
