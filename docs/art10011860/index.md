---
vid: '10011860'
title: 'In Session: "Under the Table"'
prefix: In Session
authors:
- Stephen Taylor
byline: Stephen Taylor sjt@dyalog.com
volume: '23'
issue: '3'
page: '106'
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
source: trad/v233/insession.htm
converter: vecrescue 0.1.0
issue_tab: n3-june-2008
tags:
- programming techniques
---

Stephen Taylor  
[sjt@dyalog.com](mailto:sjt@dyalog.com)
{ .byline }

APL is famously good at describing things using tables, and tables are an obvious way to represent the contents of a spreadsheet or the results of an SQL query. But tables don’t have to grow very wide before we face a writing problem. We commonly want to select rows based on expressions that refer to columns. How to refer to columns clearly?

‘Cannon’s Canon’<sup><a href="#ref1">\[1\]</a></sup> – no numeric constants in the code – steers us away from referring to them by number like this:

```
tbl←0,⍨'Orders'DB.FindM('Item' 'Price' 'Quantity')('')
tbl[;4]←×/tbl[;2 3]
html,←'TABLE'tag'{TR'tag'TD'∘tag∘⍕¨⍵}/tbl[;1 4]
```

(`DB.FindM` queries a database table `Orders` and `tag` marks up character vectors with <span class="smallcaps">HTML</span> tags). We can improve on this a little with local names for column indexes:

```
ITEM PRIC QUANT AMNT←⍳2⊃⍴tbl
tbl[;AMNT]←×/tbl[;PRIC QUANT]
html,←'TABLE'tag'{TR'tag'TD'∘tag∘⍕¨⍵}/tbl[;ITEM AMNT]
```

We can write something closer to the simplicity of our first textbook by representing the table as a namespace in which named vectors represent the columns:

```
tbl←'Item Price Quantity'DB.Tbl'Orders'
tbl.(Amount←Price×Quantity)
html,←'TABLE'tag'{TR'tag'TD'∘tag∘⍕¨⍵}/⍉↑tbl.(Item Amount)
```

Dot notation gives us even better opportunities when we come to tables with matching column names. Consider a table `CON` of contact information from a spreadsheet. We’ll use some of the records to update contacts already in our database. The others we’ll add as new contacts. So the first question is: which records in `CON` represent existing contacts? Our first step is to match email addresses, record the corresponding `ContactID` and write a bitmask to show which records were not matched.

```
C←'ContactID Email CompanyID Name'#.DB.Tbl'Contacts'
no←0=CON.ContactID←(C.ContactID,0)[⊃⍳/(C CON).Email]
```

Note

- use of `⍳/` with dot notation to look up a column from one table in its homonym in the other;
- the clarity of setting and testing for the ‘null’ value `0` in the same line.

But people use multiple email addresses, so we’ll test the unmatched records to see if we can recognise any others. Here we’ll look for matches on *both* CompanyID and Name.

```
(no/CON.ContactID)←(C.ContactID,0)[⊃⍳/1 no/¨↓∘⍉∘↑¨(C CON).(CompanyID Name)]
```

Note

- use of dot notation to specify the `CompanyID` and `Name` columns from *both* tables;
- use of `↓∘⍉∘↑¨` (*zip each*) to convert each pair of vectors into a vector of pairs;
- use of `1 no/¨` to mask only the `CON` pairs;
- use of `(no/CON.ContactID)←` to insert just the matched contact IDs, if any.

This is clearly not beginner’s writing, but the single line is easier to analyse and maintain than the same operations dispersed across several lines and temporary variables.

## References

1. <a id="ref1"></a> “Three Principles of Coding Clarity” Taylor, S. *Vector* <strong>18</strong>(4) pp113-124  
    [http://www.vector.org.uk/archive/v184/clar184.htm](../art10009750/)
