---
vid: '10002360'
title: 'KPS: Beyond the Spreadsheet'
authors:
- Ian Clark
byline: by Ian Clark
volume: '12'
issue: '3'
page: '55'
source: trad/v123/clark123_55.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1996
tags:
- applications
---

by Ian Clark
{ .byline }

KPS stands for “Kunzle Planning System”, the brainchild of George Kunzle and the flagship product of Bristol-based Adaytum KPS Software, a small but growing company of currently under 30 employees. In mid-1995 Adaytum acquired a 100% holding in Insight Systems, a Danish software house well known in APL circles. As a result of the impetus this gave to its development plans, Adaytum was able to announce KPS for Windows at the SoftWorld Exhibition, Birmingham, England, in October, 1995, where it demonstrated a working prototype in a blaze of publicity.

At the time of writing, first customer shipment of the Windows product is still a week or two away, but the wraps are off and the shape of the product is there for all to see. What is less common knowledge is that Adaytum has been selling KPS as a DOS product successfully enough to turn it from a tiny startup into a £2M turnover company in the space of three years. In the UK alone there are now 200 KPS customer sites. KPS is almost entirely written in (Dyalog) APL, so it can already lay claim to being one of the highest earning APL applications ever written. But of course the customer couldn’t care whether it is written in APL, C, C++ or Esperanto. It looks (superficially) like any other Windows integrated spreadsheet package. (The development team happens to think that’s an enormous achievement.)

Why has it been so successful? KPS is a Business Management System designed to overcome the limitations of the spreadsheet when used for business planning and budgeting. Now a spreadsheet is a pretty blunt instrument, in spite of years of refinement from the original humble VisiCalc on the Apple II and the Commodore Pet to the dizzy heights of Excel and Lotus 1-2-3. It’s designed to provide quick solutions to simple problems. A complex business management system importing, consolidating and sharing substantial data across an enterprise is somewhat beyond their remit.

Yet this is how many enterprises use them, and hate themselves for it. Just how they got themselves painted into this corner awaits the definitive psycho-historical treatise, which long-serving operations research (OR), management information system (MIS) and data processing (DP, ADP, IT) specialists will covertly ponder in shocked silence. But the outcome is no secret. Many organisations rely on a mess of incompatible, inflexible, stand-alone spreadsheet applications; nobody but the original macro-writer knows how they work, and not even they can alter them reliably. Adding new brands, product codes or divisions typically takes weeks of re-programming, in a fairy-language which frankly won’t take the strain. (It’s rather fun being able to write this in an APL publication.)

So what do customers see in KPS? Quite simply KPS out-performs all state-of-the-art spreadsheets in its chosen area, which Adaytum calls “Budgetware”. It does this by offering many small features, and one or two great ones, well-adapted to the needs of financial managers, planners and cost accountants, in medium-to-large organisations. Quoting from its brochure KPS provides the ability to:

- Support a single integrated system across departmental and functional areas.
- Import and consolidate large volumes of data from multiple sources in minutes, not days.
- Build or change complex models quickly and easily without recourse to specialist support.
- Produce ad-hoc reports on demand.
- Perform comprehensive “what-if” and variance analysis by product, region, customer or any other category.
- Allocate overheads according to any criteria.

This last item may sound woolly, but it concentrates in six words one of the most impressive features of KPS, its ability to allocate an altered total across the contributing values, which may themselves be computed from other values.

For example, suppose you have the following row of figures, the end one being a total of the foregoing ones:

```
100     100      150      200      100      50      300      1000
```

Now overtype the total: 1000, making it, say, 1100. With a spreadsheet set up to do this straightforwardly, you would simply lose your formula and replace it by a constant, 1100. But with KPS (which does it even more straightforwardly) the following happens:

```
110     110      165      220      110      55      330      1100
```

Altering a total (in this case to increase it by 10%) causes the contributing figures to be altered in the right way (in this case increasing each of them by 10%) to add up to the new figure. If the contributing figures themselves are totals, then their contributors in turn get adjusted, all the way back to what KPS calls “detail” items, i.e. non-calculated. When you realise that what KPS calls a total can have a complex formula in plus, minus, multiply, divide, power, if-then-else and even complex functions like feeds, you realise that this is an enormously powerful facility, with mathematical depths hard to plumb.

The budgeting implications are easy to see, however. And to use. You, the forecaster, can look at a business plan as a multi-dimensional array (a d-cube, in KPS parlance), choosing whatever slice you care to portray as the rows and columns of a spreadsheet-like grid, then you can say things like “let’s increase this cell by 10% and see what changes”. If too many cells change (they change colour as well as contents), you can go back a step and “hold” the cells you don’t want to change.

You needn’t start with actual typed-in figures, either. There are quick ways of setting the grid to zero or a constant and filling planes of the grid with numbers which grow in a rich variety of ways. Even the allocation facility can be pressed into service to create convincing time-series from scratch. KPS understands times and dates. It can allocate values across a monthly timescale in proportion to the number of days in the month, getting the leap-year right, too. And all without you having to type a row of figures like this: 31 28 (or is it 29?) 31 30 ... (although you can do that too, if you like).

Some people think that using KPS is more like using a painting program than a spreadsheet, with figures pouring into their designated areas like colours. So much so that one major UK brewery could boast “it used to take nineteen people three months to produce one quarterly forecast. It now takes one management accountant half a day”. And of course you can generate respectable reports and graphs from the sets of figures when you’ve produced them, so nobody can accuse you of just having fun.

It’s remarkable how quickly you can set up a d-cube from scratch. Nobody likes to talk about hypercubes (nasty frightening things) or even multi-dimensions, if it can be helped. But tables of figures in an organisation tend to fall into a small number of patterns. There may be lists of product codes, or sales regions. There may be typical timescales consisting of days of the week, hours of the day, 5-year periods split up into quarters, or this year and next year split up into months. In addition to the raw-value columns there may be grand totals, totals for the year or half-year, and so forth. KPS lets you build and save each of these sets of column headings as a library entity, called a d-list (short for dimension-list). The calculation needed for a given total is attached to the d-list itself, whereas a spreadsheet attaches a calculation to each cell and replicates it (in some more-or-less perplexing way) when you ask it to. Therefore, for each “total” d-list item (aka column-heading or row-heading) there is a single formula, which applies to a whole column (or row) in a 2-dimensional d-cube, a whole plane in a 3-dimensional d-cube, and in general (since we’re talking to APLers) a whole (n-1) matrix of cells in the general case of an n-dimension d-cube. Never mind. If you’re an accountant, you simply define a new d-cube in terms of a collection of n d-lists and hey presto! – a d-cube appears full of zeros.

You can then feed data in from a variety of sources by marrying up relational domains with d-list items, ASCII file fields, in fact any data source for which there is an ODBC driver (and that’s most commercial products nowadays). Nor do you need to confine the “transfer link” as it’s called to (one number)–(one cell), but you can “consolidate” (many-to-one) or “allocate” (one-to-many). It’s particularly attractive to transfer data between different d-cubes sharing one or more d-lists in common, and quick to set up. In that way a large organisation can pass data up the managerial pyramid, consolidating or allocating data across subheadings at each stage. Activating a transfer link is as simple as pressing a button. KPS allows you to build a “radar map” – a layout of buttons to look like an organisation chart or a flow diagram. Some of the buttons can display given d-cubes, others can activate named transfer links.

This raises the question: “where did that number come from”? There’s another button with an electric drill icon. Pressing this will progressively “drill down”, opening windows into d-cubes or ASCII file images to show you the contributory numbers.

Curiously, there’s interest in KPS among organisations who have problems simply converting data from one form to another between different subsystems. The potential for sophisticated import and export offers a sort of user-friendly patchboard, without needing to pour the data into a d-cube at all. But of course, when you’ve got the Electric Light, why use it only when you’re trimming the candle wicks?

(webpage generated: 8 February 2007, 03:16)
