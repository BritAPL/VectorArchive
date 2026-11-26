---
vid: '10004580'
title: R.I.P. OOF
authors:
- Morten Kromberg
volume: '17'
issue: '2'
page: '91'
source: trad/v172/ripoof.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2000
tags:
- system interfaces
- programming techniques
---

## **(At the Hand of Dyalog v9.0)**

*by Morten Kromberg (mkromberg@insight.dk)*

## Introduction

In July 2000, Vector published an article I had written in April, describing a toolkit called “OOF” (OLE Object Functions), which I had developed in order to make it easier to use ActiveX or OLE Servers, such as those provided by Microsoft Excel. In this article, I concluded:

> *Dyalog APL provides most of what you need to use OLE Servers from APL, but in my opinion, they could be significantly easier to use. The OOF tool allows APL developers to use expressions, which are closer to that which they can find in documentation and other literature, which is usually aimed at Visual Basic or Visual C developers.*
>
> *However, I can’t help feeling that I should not have had to build this tool – it should have been part of my APL system! It will soon be time to re-launch APL to a new generation of developers. The people with an ‘attitude’ to APL (good or bad) are moving on, and I think we will soon have an opportunity to try to sell APL again. This will not be possible if the new audience feel that the mechanisms we provide for inter-application communication are significantly inferior to the environments they are used to. On the other hand – if we have it, there is no reason why we should not be able to go out and sell APL to people who need to compute something; all the other languages are still far behind APL in this respect.*
>
> *If the vendors are not already working on it, this seems to be the next big challenge, which we need to address as a community. I think I would go as far as to say that the long term survival of APL depends on support for objects at the language level.*

Unknown to me at the time I wrote these lines, Dyadic had already done most of the work required to instantly obsolete OOF. At APL Berlin 2000, they presented version 9, which provides improved OLE/ActiveX support. Thanks to Dyadic, this has turned into a perfect project for me: I had all the fun of learning a bit more about objects, designing it and getting it to work, but was spared the grief of packaging, rollout and support.

## What’s New?

A couple of new features in version 9 make the difference. Firstly, version 9 now loads all Type Information associated with an object when the first top-level object is created. This is done much faster than before and the results are saved in the workspace<sup><a href="#fn1">1</a></sup>.

Secondly, you can now use the “industry dot syntax” to refer to the properties of GUI and OLE objects. So where you previously had to write:

```
'F' ⎕WS 'Caption' 'Hello World'
```

… You can now get away with:

```
F.Caption←'Hello World'
```

When resolving a “dotted name”, each segment in the name is an expression, which is evaluated in the context of the object resulting from the evaluation of the segments to the left. Unless it is the last (rightmost) expression, it must return a single namespace<sup><a href="#fn2">2</a></sup>. This allows the use of expressions in the form:

```
      XL.Workbooks.(Open 'MyBook.xls').Sheets
```

We can now avoid the explicit creation of “intermediate” objects, which in version 8 was required for every level of the hierarchy. To illustrate, let us take a look at the code example required to open a workbook and retrieve data from a range within it. In Dyalog APL version 8.x, you had to do something along the lines of:

```
     'XL' ⎕WC 'OLEClient' 'Excel.Application'
     'WBS' ⎕WC 'XL' ⎕WG 'WorkBooks'
     'BOOK' ⎕WC 3 ⎕NQ 'WBS' 'Open' 'MyBook.xls'
     'SHEETS' ⎕WC 'BOOK' ⎕WG 'WorkSheets'
     'SHEET1' ⎕WC 'SHEETS' ⎕WG 'Item[1]'
     'RANGE' ⎕WC 'SHEET1' ⎕WG 'Range[A1;D3]'
     ⍴'RANGE' ⎕WG 'Value'
  3 4
```

Quite a mouthful! With OOF, this was reduced to:

```
     #.OOF.Add '#.XL' 'Excel.Application'
     #.OOF.Add '#.BOOK' '#.XL.Workbooks.Open' 'MyBook.xls'
     ⍴#.OOF.Prop '#.BOOK.Sheets[Sheet1].Range[A1;D3].Value'
  3 4
```

In version 9, all you need is:

```
     'XL'⎕WC'OLEClient' 'Excel.Application'
     ⍴XL.Workbooks.(Open 'MyBook.xls').Sheets.(Item 'Sheet1').(Range 'A1:D3').Value
  3 4
```

## More Examples

To ensure that nobody has any illusions that OOF might still have any use (unless you are unable to upgrade to v9), here is a quick run through most of the examples in the July article.

### Open a Workbook

We need this for all the subsequent examples:

OOF:

```
        #.OOF.Add '#.book' '#.XL.Workbooks.Open' 'MyBook.xls'
```

V9.0:

```
        book←XL.Workbooks.Open 'MyBook.xls'
```

### Invoke a Method

OOF:

```
        #.OOF.Invoke '#.XL.InchesToPoints' 1
```

V9.0:

```
        XL.InchesToPoints 1
```

### Get Property of an Item in a Collection

OOF:

```
        (1 #.OOF.CollProp 'Name') '#.book.Sheets'
```

V9.0:

```
        book.Sheets.(Item 1).Name
```

### Set Property of an Item in a Collection

OOF:

```
        'Demo' (1 #.OOF.CollProp 'Name') '#.book.Sheets'
```

V9.0:

```
        book.Sheets.(Item 1).Name←'Demo'
```

### Get Property from all Items in a Collection

OOF:

```
        names←('*' #.OOF.CollProp 'Name') ditto
```

V9.0:

```
         :With book.Sheets
                names←Count⍴⊂''
                :For i :In ⍳⍴names ⋄ (i⊃names)←(Item i).Name ⋄ :EndFor
        :EndWith
```

OOF seems to be doing a bit better here, but `OOF.CollProp` is a monster function containing lots of ugly code. You could write a very simple cover-function in v9, if you want to. Unfortunately, despite the fact that …

```
        3=⍴sheets←book.Sheets.(Item¨⍳Count)
```

and

```
        'Demo'≡sheets[1].Name
```

… Dyadic have decided to spend a bit more time pondering theoretical issues before they let us<sup><a href="#fn3">3</a></sup> access the properties of an array of objects using notation like:

```
        book.Sheets.(Item¨⍳Count).Name
```

### Enumerated Types

Object hierarchies often make use of Enumerated Types, in which an index or constant must belong to a declared set of named values. OOF contained a utility function to retrieve the “Index Set” of a collection:

```
        #.OOF.GetIndexSet '#.book.Sheets'
1 2 3
```

And if “range” is a range within a sheet:

```
        #.OOF.GetIndexSet '#.range.borders'
 xlInsideHorizontal    12
 xlInsideVertical      11
 xlDiagonalDown         5
 xlDiagonalUp           6
 xlEdgeBottom           9
 xlEdgeLeft             7
 xlEdgeRight           10
 xlEdgeTop              8
```

With OOF, you could use the names (like xlInsideHorizontal) or the numeric ids (12) to refer to an item in a collection.

In version 9:

```
        3 2⊃range.Borders.GetPropertyInfo 'Item'
 XlBordersIndex
```

If this returns ‘VT\_VARIANT’ (as it would for book.Sheets.Item), the argument to Item is not an enumerated type and the list of valid item identifiers is simply:

```
        ⍳book.Sheets.Count
```

Otherwise, you can easily extract the type information:

```
        ↑range.Borders.GetTypeInfo 'XlBordersIndex'
xlInsideHorizontal    12
xlInsideVertical      11
...etc...
```

It is important to note that version 9 exposes enumerated type constants as variables in the namespace corresponding to the object. So you can simply refer to:

```
        range.Borders.xlInsideHorizontal
12
```

And use it as follows:

```
        range.Borders.(Item xlInsideHorizontal).Color←231232
        range.Borders.(Item xlInsideHorizontal).LineStyle←1
```

Or even better:

```
        :With range.Borders.(Item xlInsideHorizontal)
         Color LineStyle←231232 1
        :EndWith
```

## Conclusion

With version 9, Dyadic have come close to making it as easy for APL developers to use OLE/ActiveX components as it is for developers using Microsoft development tools. The challenge of understanding the various object models remains, but it is now much easier to re-use a code fragment found in a magazine, on the web or in documentation. As a result, APL is much more competitive with respect to Visual Basic and other mainstream development tools for Windows; our chances of selling APL to a new generation of developers is dramatically improved.

Under the covers, APL is doing exactly the same job that OOF was doing before: creating temporary objects on your behalf, in order to reach the level of the object hierarchy you are reaching for. Where OOF only deleted these “temps” when you explicitly deleted the parent object (which you had explicitly created), APL (like Visual Basic) deletes them immediately unless you assign them to a variable. This means that, although it is now very easy to reach into deep object hierarchies in fantastic one-liners, it may be wise to create temporary variables in order to prevent wholesale creation and destruction of objects, which may sometimes have a high initialisation cost. For example:

<pre>   :For i :In ⍳book.Sheets.(Item 1).(Range'A1:J25').Count
       (book.Sheets.(Item 1).(Range'A1:J25').Item i).Formula←'rand()*100'<sup><a href="#fn4">4</a></sup>
   :EndFor</pre>

... takes three times as long to execute as:

```
    range←book.Sheets.(Item 1).(Range'A1:J25')
    :With range
       :For i :In ⍳Count
         (Item i).Formula←'=rand()*100'
       :EndFor
    :EndWith
```

The above code is all a bit stupid anyway, it is a workaround for what looks like a bug in Excel. You should be able to get away with:

```
    book.Sheets.(Item 1).(Range'A1:J25').Formula←25 10⍴⊂'=RAND()*100'
```

… which takes no time at all, but for some reason Excel treats this as if you had put the formula in quotes (as a text constant), and displays the formula without executing it. If you visit each cell in turn, click on the formula as if to edit it and hit enter, it corrects the problem<sup><a href="#fn5">5</a></sup>.

Although it seems that Microsoft’s imagination may have failed in this particular case, it is becoming common for object models to contain array properties. For example, ADO database interfaces return matrices of data. Life is getting easier for the array-minded developer!

### Footnotes

<ol>
  <li><a name="fn1">I’m not sure what happens if you then try to use your workspace with a different version of Excel – I assume that COM interface versioning forces a reload of the relevant information?

  <li><a name="fn2">The ability to return namespaces as the result of functions is also new in version 9.

  <li><a name="fn3">I hope. Please call John Scholes or Peter Donnelly and tell them you must have this feature, and you want it yesterday!

  <li><a name="fn4">Note the parentheses starting at the left and ending after “Item i”. If the parentheses were around (Item i), i would be evaluated in the cell namespace, and give a VALUE ERROR. In the second example, i is a variable inside range.

  <li><a name="fn5">A beer at the next APL conference for anyone who has found a way to convince Excel to accept an array of formulas.
</a></li></a></li></a></li></a></li></a></li></ol>

***
