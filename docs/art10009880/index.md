---
vid: '10009880'
title: 'Industrial FP: a Case Study'
authors:
- Stephen Taylor
volume: '21'
issue: '3'
page: '52'
description: VECTOR article
source: trad/v213/ifp.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- applications
- programming techniques
- language design
---

Jars of spring water are no longer enough;  
take us down to the river.  
<span class="small">— Rumi</span>

<acronym>APL</acronym> and [Functional Programming](http://www.vector.org.uk/?submit=page&area=discover&page=linguistics "Discover: Programming Linguistics") are kissing cousins. Long before <acronym>FP</acronym> was elaborated, <acronym>APL</acronym> was focused on the transformation of arrays of data by primitive, defined and derived functions. In turn, seminal work on Functional Programming influenced the development of D in Dyalog APL and tacit programming in J.

What does this mean to a working programmer? Substantial software has been written in pure <acronym>FP</acronym> form, including compilers. But does <acronym>FP</acronym> remain an academic fad, a cunning trick — look, Ma, no variables? What can we take and use from <acronym>FP</acronym>, and how might it help us in everyday programming?

Elsewhere in this issue (“How To Write A Computer Program”) John Scholes offers his view of the nature and value of <acronym>FP</acronym>. In this article I will take a single substantial example drawn from my own programming practice, explain how it works and how it was developed. I’ll also review what differences doing it in <acronym>FP</acronym> made, and what debugging and maintaining it have been like.

This example has been in production use at a large <acronym>UK</acronym> pension provider for the last year. It is fast, reliable and ran about 10,000 times in the last three months.

## The problem

As part of our development style with this customer (I would blush to use so elevated a word as ‘methodology’) we minimise our use of their <acronym>IT</acronym> people. We are often reminded how scarce their time is. So when we were offered an interface to mainframe data records and asked in what format we would like the data arranged, we said: <q>Anything, really.</q> We didn’t want to add to their already heavy burdens. <q>Just do it,</q> we said, <q>in any way that suits you, and tell us afterwards what you did. We’ll figure out how to read it. We just want you to do it whichever way is easiest for you.</q> They looked at us as if we were mad.

Perhaps we were. When we got the file formats, tables were nested inside tables within tables within… Perhaps this is a common layout for mainframe files. Would I know? Anyway the data had to be unpacked from this, and in the future from files of similar structure. I did not want to revisit this problem; I wanted a general approach.

Brief thought distinguished the syntax from the semantics of the file. The files alternate definition lines with one or several data lines. (See [Exhibit 1](ifpex1.htm) or [complete query file](query.txt "download query file (100Kb)").) The definition lines name and describe the fields represented in the data lines that follow. The semantics of the file involve type conversions, discarding padding and so on. The syntax was a matter of picking the data fields out.

The first lines in the file are encouragingly easy. For example:

```
D011 #PAIDTODATE(N5),1 #ISSUEDAY(N8),1 #STATUSCODE(A2) …
A01  1248        1220  1180   456  20  1248  1248GPSN ZS488133CYYN …
```

This is knockabout stuff. Definition line 1 (`D01`) specifies `PAIDTODATE(N5)` — a numeric field requiring 5 characters. Well, 6, as it turns out that `N` fields always get an extra character width. Still, they are consistent, no problem. It’s followed by 9 characters of `ISSUEDAY` and 2 (really 2) for an alphabetic `STATUSCODE`. This gets the first three fields of the line: `'1248' '1' '22'`. If only it were all that easy.

The second definition line introduces a complication.

<pre>
D021 <span class="highlight">RET-IND-INFO(1:700)</span>,<span class="red">2</span> #COVCONNECT(A2)
A020707080819
</pre>

`RET-IND-INFO` is not a field, but announces a 700-row table. The following fields all contain 700 values, alpha-fields padded with blanks, and numeric fields padded with zeros. So we get the `COVCONNECT` column as `'07' '07' '08' '08' '19'` and the blanks are presumably 695 unused rows.

We also notice the numeric prefix to the field name. In the first example, the fields were all prefixed with a `1`, locating them at the root of a ’nesting tree’. The columns of the `RET-IND-INFO` table are all prefixed with `2`s, indicating another level of nesting.

The `RET-IND-INFO` table gives another couple of blank-padded alpha columns, then hits its first numeric column.

```
D052 #DETAILEFFDATEMYC(N5)
A05  1180  1204  1180  1204  1237     0     0     0     0…
A06     0     0     0     0     0     0     0     0     0…
A07     0     0     0     0     0     0     0     0     0…
A08     0     0     0     0     0     0     0     0     0…
A09     0     0     0     0     0     0     0     0     0…
A10     0     0     0     0     0     0     0     0     0…
A11     0     0     0     0     0     0     0     0     0…
```

Once we realise the zeros are just the padding on the numeric column we can see there is no difficulty here. The table continues with further columns, then ends with a new kind of field declaration.

```
D261 #ERRORMESSAGE(A30/1:10)
A26
```

We know the table has ended because `ERRORMESSAGE` is prefixed with a 1, putting it back in the root of the nesting tree. If it had a `2` prefix, it would be part of the `RET-IND-INFO` table. The `A30` speaks for itself, but `1:10` looks like a table declaration. Almost: we could say this a list of ten 30-character strings. (We’re pleased to see the error message is blank.)

After the error message we plunge straight into a new table, `SVDATA`.

```
D011 SVDATA(1:2),2 #CASHVAL-1(N9.2),2 #SURRVAL-1(N9.2),2 #SURRCHG-1(N7.2)…
B01     46989.35     49766.26     46989.35     49766.26       0.00…
```

(We notice the data lines are now prefixed with `B`; it turns out we’re into part B of the query. And the definition line numbers have restarted at 1 again.) Lines `D01` and `B01` describe the left end of the 2-row table `SVDATA`:

```
CASHVAL-1       SURRVAL-1       SURRCHG-1
46989.35        46989.35        0.00
46989.35        46989.35        0.00
```

But just a little way further along the same definition line, things start to get more interesting.

```
2 #REASONCOUNT-1(N2),2 RETLPIENTRY-1(1:3),3 #LPISURRVAL-1(N7.2)
```

While still inside table `SVDATA` we’ve declared a new 3-row table `RETLPIENTRY`. Now further ’columns’ of `SVDATA` are no longer 2-lists, but 2×3 tables. To mark this, the contents of `RETLPIENTRY` are prefixed with a `3`. The first column of `RETLPIENTRY` is `LPISURRVAL`; this is now nesting level 3.

That’s pretty much it. The file has no more surprises. But it makes liberal use of the table trick. Before emerging from `RETLPIENTRY` it’s opened (line `D18`) the 3-row `RETPHLPIENTRY` table at level 4, and we can see (definition line `D02` in part C) that we reach level 5 at the deepest level of nesting.

If I have to unwrap query files like this, I want a general solution. I want a function that will read the definition lines and work out where the data is in the data lines and break it out and remember what shape and type it’s supposed to be. I’ll go nuts if I try to work this stuff out myself and I’m blowed if I want to repeat the exercise for the next kind of query file we use. (Hence the epigraph from Rumi at the head of this article.)

## Parsing the query file

I decided to parse the query file into a 4-list. The first element of the list will be a list of nesting stacks. For example, the field `LPISURRVAL` is within the 3-row table `RETLPIENTRY`, which is itself within 2-row table `SVDATA`. So the nesting stack for `LPISURRVAL` is 2 3. (Or 1 2 3 if you count the root of the nesting tree.) An <acronym>APL</acronym> way to say it: the first element of my result will be a list of the shapes of the fields.

The second element will be a list of the field names, and the third a list of their formats. The fourth and final shall be a list of the data ‘windows’, that is to say, the text (possibly truncated) from the query file that contains the data for the field.

Since each of the four lists will have an entry for each field, they will be of equal length. So we could equally well describe the result as a 4-column table.

## Strategy

The job is an obvious candidate for <acronym>FP</acronym>. All the information required is on the query file. Retrieve that as a single character string, and return the required 4-list. The query files run to about 100Kb, which can be easily handled within memory. There are no side-effects required. I set out to write `parseFile` as a D function using <acronym>FP</acronym> style.

The first thing to notice about `parseFile` ([Exhibit 2](ifpex2.htm)) is that it consists entirely of definitions. Three of these define data, which is to say names are assigned to constants. The remaining lines all define functions and operators, except the last line, which defines the result. It is also the only definition that involves the function argument. So we can see that, as `parseFile` is interpreted, not much actually happens until the last line, then a lot happens at once. For debugging this looks like a disaster, but I’ll return to that subject later. At any rate, it makes the strategy of `parseFile` very clear.

Looking at the last line, the `lineate` function is dyadic; `crlf` is its left argument. Functions `nstate`, and `separate` are monadic, and `nstate` and `separate` both return paired lists, which dyadic function `parse` zips together.

So we can see that the result is defined as something like the following: we `parse` between the two lists returned by `nstate` after separating segment headers and bodies from the segmented file string lineated by `crlf`. Well, it’s a start.

Read from right to left: we lineate the file string using `crlf`. That sounds as if we sensibly divide it into lines marked by `CR-LF`. Then we segment it, and a quick glance at the definition of `segment` confirms this involves testing for the initial `D` that we know distinguishes the definition lines. So we can guess `segment` too: it looks as if it groups the lines into segments of a single definition line followed by 1 or more data lines.

Ah, we can then guess what `separate` does. It turns a segment into a pair: a definition string (header) and a data string (body).

The `nstate` function does the tricky thing in the whole setup.

The file lines have been truncated, so the segments have to be parsed individually, lest we fail to add enough padding between segments. We have to calculate the data windows for each segment, overtaking to pad with blanks, before starting the next segment. So we need to get all that worked out while preserving the segment structure.

But there is not enough information in each segment to calculate its data window. (That is, how many characters the whole segment should occupy.) We need the whole nesting structure to calculate the windows of the individual fields. The `nstacks` function takes a list of segment headers and returns the nesting stacks for each field, renested into segments. (The `onflat` operator that does this came from a casual remark by Ray Cannon that defined operators are handy anytime you want to transform something, apply a function, and then reverse the transformation.)

The `nstacks` function works out the repetition (nesting) stacks for each field. That means running forward through all the definitions, working out the cumulative effect of the various table declarations and ends of tables. Back to this later. For now, `nstate` does this, and infixes the nesting stacks with the field definitions.

After that it’s all downhill. We’ve got the field names and type/widths and by now, the nesting stacks which tell us how many times to replicate the width to get the ‘data window’ for each field. Simple: multiply the field width by the product of its nesting stack. You can see this happening at the right end of the `parse` function.

```
     parse←{r f←⍺ ⋄ (⊂1↓¨r),ns,⊂⍵ fretby×/¨r,¨2⊃ns←nmshp f}¨
```

We’re nearly home and dry. We came out of `nstate` with the nesting stacks infixed in the field definitions. All that remains in `parse` is to derive the data window for each field and use it to fret the segment’s data string. (You don’t have to write J to learn from it.)

Broadly, `parseFile` has three roughly equal parts. The first part covers transforming the file string first into lines, then segments, then separating the segments into pairs of definition and data strings. Nothing very hard there. The second part (`nstate`) does the hard work of calculating the nesting stacks. The third part (`parse`) uses the field widths and nesting stacks to calculate the data windows, and so fret the data strings.

## Tactics

An undeserved pleasure of displaying code like this is leaving the impression that it just flowed off the pen, or in this case the keyboard. Well, some of it did. The first third was fairly obvious in two ways. It was fairly obviously a good way to start on the file string, and it was fairly obvious how to do it.

The last third required more fiddling to convert the various field definitions to widths, but the general line of it was clear from the beginning. I recall trying many ways of passing arguments between the functions before settling on this version.

Half the work went into the middle section, especially if you include lying on the sofa with a wet cloth on my head.

Of course, it’s by no means pure <acronym>FP</acronym>. Although `parseFile` defines no variables other than a couple of constants, three of the functions it defines do. Arguably their short length limits whatever damage this does.

My J is nowhere near good enough to attempt one, but I would expect a J version to look broadly similar with perhaps 50-70% of the code volume. D is not, I think, quite as compact, but still a fine vehicle for <acronym>FP</acronym>.

## Against utilities

The function is monolithic and has no external references. There are obvious opportunities to use utilities for familiar functions such as removing trailing blanks, partitioning a string. I used none. Why?

First, I was indulging a love of the monolithic, self-contained function. D functions are such an opportunity to write functions that are simultaneously monolithic and modularised, without importing the rubric of object-oriented code.

One of my principles of coding clarity is to minimise the distance between definition and use of a name. A reference to a term defined in the previous line is clearer than to one ten lines earlier. A reference to a term defined in the same function is clearer than a reference to one defined elsewhere. So I favour local definitions. I might think I know what a utility function does, but its definition is not right in front of my eyes, so there is some uncertainty.

Worse, utilities usually do too much. When I wrote utilities I wrote as if I were extending the set of primitive functions. I designed them to handle boundary conditions predictably and consistently, and never suspend. Ideally they would select the most efficient algorithm for their arguments. But reading later the code that uses them, you can’t see which parts of the utility are expected to be useful. Is that boundary condition the utility takes such trouble to handle actually expected to occur in this application?

These days utilities have a more limited place in my work, encapsulating pesky interfaces to other systems and a bunch of stuff I’d prefer not to know about. But for simple transformations, such as lineating a file string, I use the simplest D function I can write. Simplest, so I can read exactly what it’s intended to do.

Utilities handle boundary conditions I expect not to encounter. If I’m wrong about the argument I pass it, a utility is likely to do something clever and return an unexpected result to cause trouble further down the line. Better to use a simple D function that breaks in place. Think of this as a fuse, a circuit breaker; code that breaks when given unexpected arguments.

## Better off trad?

Did I do a good thing writing this in <acronym>FP</acronym> style? The middle third of `parseFile` took a lot longer to write than I care to remember. The whole function represents several days of work. The middle section, in particular, I could only work on when fresh and alert and able to stay ‘in the zone’. (I believe pair programming here would have slashed the development time. Ray, where were you?)

Perhaps I would have done better to write a less condensed, tightly-wrought solution? I haven’t tried the exercise for comparison, but I don’t think so. In the first place, it made a huge difference that I could get all the source code on a single screen. Eyeballs track closer to the speed of thought than fingers do.

Secondly, while developing I stopped `parseFile` on its penultimate line. (You can’t set a stop on a D function, but a judiciously placed period will do it.) At that point I had all its local functions and operators defined and could debug the last line in immediate execution. That was pretty much what I did. I got `lineate` working to my satisfaction, then `segment`, and so on.

The tiny D functions could all be edited until they did the right thing, then displayed in canonical representation, and the revised definitions pasted back into the suspended `parseFile` function. Nothing could be simpler. Despite the complexity of the whole problem, refining the components of `parseFile` was pleasantly like doing textbook <acronym>APL</acronym>.

When `parseFile` worked on my folder of sample query files I took it back to the office to play with live query files. Proudly I showed the source to my colleagues, who paled. Surely this was the infamous ‘write-only code’? It turned out not to be so. The live query files twice displayed characteristics I hadn’t originally seen or catered for. In each case I was able to identify and amend the relevant fragment of parseFile in under twenty minutes.

That was a year ago; never had to touch it since. Some time last year we got the second kind of query file, formatted on the same principles. We had five problems parsing it, all of them arising from errors in the query file format. Once they were corrected, `parseFile` handled everything smoothly.

## Refactoring

In fairy tales the good people live happily after after. Or something happens after a year and a day.

A year and a day later, while preparing this article, I noticed some unhappy-looking expressions. For example, the original function defined

```
      wdws←{×/¨⍵,¨1↓¨⍺}
```

Surely those three eaches might be refactored? Indeed yes, and `wdws` disappeared into `×/¨` and a `,¨`. And then a couple of defined operators looked vulnerable and had to go as I saw patterns I hadn’t been able to see a year earlier.

Two things to notice. One: I’m still learning how to write <acronym>APL</acronym>.

Second, refactoring was a snack. We have thousands of query files, but only two varieties. I took the file strings from an example of each and assigned them names `F1` and `F2`. Now

```
      R←parseFile¨F1 F2
```

gave me `R` as a model answer. Then it was time for substitutions. I opened `parseFile`, duplicated a line, commented out the first copy and edited the second. Exiting the editor,

```
      R ≡ parseFile¨F1 F2
```

told me whether the new line worked. Changing and testing in small steps like this, I was able to work confidently, never more than one change away from a working version. Code and test, code and test, with a cycle time measured in seconds — <acronym title="Extreme Programming">XP</acronym> on amphetamine!

## Alternative ending

Fairy tales can have alternative endings too. Reviewing the code for this paper, I was unhappy with a certain amount of shuffling I saw going on, with arrays being packed and unpacked primarily to keep them in the <acronym>FP</acronym> pipeline. Refactoring removed much of this, but you can still see some juggling going on in `nstate` and `parse`. I tried another round of refactoring: could I lose the juggling? Alternatively, if I did assign a (presumably minimal) set of intermediate results to variables, how bad would that now look?

You can see the result in [Exhibit 3](ifpex3.htm). (I’ve arranged the lines so that both functions start with all the common lines.) It’s a line or two longer. The last line expresses the result more clearly: four lists with the segment structure removed. The penultimate section is, I think, clearer about the tactics of the function, less clear about its strategy. You pays your money…

More soberly: <acronym>FP</acronym> seems not *the* way to write (any more than <acronym title="object-oriented">OO</acronym> is) but a Very Useful Style worth practising. Working functionally kept the code terse and free from distractions.

***
