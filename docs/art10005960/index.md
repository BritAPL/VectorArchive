---
vid: '10005960'
title: 'Meeting: Business Graphics'
authors:
- George MacLeod
volume: '10'
issue: '1'
page: '70'
status: not online
issue_tab: n1-july-1993
---

George MacLeod
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 70](../10/1/VOL.10-NO.1-JULY-1993.pdf#page=72)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.1</p>
<p>Business Graphics A Second Look by George MacLeod</p>
<p>This is a summary ofthe presentation given by George MacLeod at the BAA meeting in March 1993,</p>
<p>Thepresentation discussed MacLeod s views on business gtaphics, how his ideas had been implementedin the graphics partof the Impetus product and how these ideas had been further developed by prototyping a new business graphics package (code named Trellis) which will run under Windows and is being implemented in Dyalog APL/W.It is hoped to provide similarfacilities under APL*PLUSII in the future.</p>
<p>The underlying principles of Impetus Graphicsare threefold. 1, It mustbe easy to produce simple graphs. 2. It must be possible to produce elaborate graphs when needed.</p>
<p>3, The graphs whenprinted should beof a high quality.</p>
<p>Thethird item is based on the belief that most graphics Programsconcentrate on the presentation on the screen rather than on the printed graph. Although many executives use Personal Computers thereis still a need to produce regular high quality printed graphical outputfor distribution to senior management.</p>
<p>There were considerabledifficulties in implementing these aims in Impetus as the GSS graphics drivers supplied with most DOS-based APLs were not bug-free. This was particularly true when use was made of variable-pitch fonts. This resulted in graphs which could take several hours to print, and which werestill incomplete. A paper on this implementation was presented to the BAA in 1991. Since that time there have been new releases of the graphics drivers and the problemsare nowresolved.</p>
<p>In the Windows world there is a wide variety of graphics applications programs, but most of them are really drawing programs that also have some ability to produce business graphics. The modern spreadsheet offers graphs tightly integrated into the spreadsheet data, and it is possible to produce graphs of considerable elaboration although many ofthe features of a good business graphics system arenot available.</p>
<p>70</p>
<p>VECTOR Vol.10 No.1</p>
<p>Trellis graphics has the same underlying aims as did the earlier Impetus graphics take but is intended to offer a more complete range of graph types and to advantage of the Windows Graphical User Interface . As with Impetus graphics, the data and the graph properties are kept separate. Fundamentally Trellis works complicated on three variables. Thefirst is the Graphics Description which is a nested variable containing many properties. The data is contained in a simpler nested variable known as the Data Source. on, and more A Data Source may be used with more than one Graphics descripti more than one Data importantly, a Graphics Description can be used with Source. to The third variable is the Script. This is an editable variable which is equivalent a batchfile in DOS.It uses a very simple andlimit ed language , and facilitate s the production of regular batches of graphs. MacLeod s ownintroduction to business graphics camevia the use of the Boeing runs Executive Information Services (EIS) product in the late 1970s. Boeing EIS has onan IBM mainframe andhasextensive business graphicsfacilities. The user the command great control over the graphs that are produced, but the syntax of able on structure is complex and hasto be learned and wouldbe totally unaccept a modern PC. and At the heart of Boeing EIS is the recognition that most business manufacturing data is time-related. For example, salaries may be paid monthly, VAT may be paid quarterly and rent every six months. A capital purchase is will made at a point in time. We call this data Time Spread data and Trellis support daily, weekly, monthly, quarterly, semi-annual and annualtime spread data. Such data is defined by three properties. The periodicity (monthly, weekly, values. etc.), the start date (day, month and year) and a simple vector of</p>
<p>Time-spread data has a number of advantages. Firstly you do not have to providethe labels for the time dimension on a graph the labelling is inherent defined, for in the data. Secondly the total duration of the data is clearly example, there is no ambiguity aboutany data before the start date. Thirdly it is possible to plot a graph of data with different periodicities, for example, monthly data can be plotted with quarterly data by automatically summing the months into quarters. Cumulated datais readily available to produce cumulative graphs, will andfinally it is possible to have a time window set to limit the data that appearin particular graph.</p>
<p>71</p>
<p>VECTOR Vol.10 No.4</p>
<p>The underlying architecture of a Graphics Description is as follows: Page Level Properties Graph Level Properties (Graph 1) Plot Properties (Piot 4) Plot Properties (Plot 2} Plot Properties (Plot 3) etc. Graph Level Properties (Graph 2) Plot Properties (Plot 41) Plot Properties (Plot 2) Plot Properties (Plot 3) etc. etc.</p>
<p>Pagelevel properties are concerned with such things as should there be a box aroundthe page, should there be a time stamp, whatis the background colour,ete.</p>
<p>Graphlevel properties relate to matters such as graphtype, description of the axes, grids, etc. These properties can be specified for each graph on a page,</p>
<p>Most graphs have more than oneplot. For example a Line Graph will often show morethan oneline of Plot. Plot properties relate to such matters asline types, bar patterns, where the data comes from, etc. As it require a large number of Properties to draw a graphit is important that most properties take default values and are only changed whenthe user wishes to change them.Trellis will be delivered with a complete set of defaults for all of the graph types. When a Graphics Project is first created a complete set of the system defaults will be placed in it.</p>
<p>The concept of heredity will be used in much the same way as with Object Oriented Systems. A Plot is the child of a Graph, which in turn is the child of a Page which is the child of a Project. For example,if we are producing a line graph and we are aboutto draw thefirstline Trellis will look for the properties of the line at the Plot Level. If it finds nothing it will look at the Graphlevel, if it again finds nothing thenit looks at the Page level and if nothing is specified there then it looks at the Project level, where everythingis specified.</p>
<p>This means that properties for the first line to be drawn in line graph may be found at a numberoflevels. Notall properties can be treated in this way. For example thelocation of a graph on the pagehasto be specified at the graph level, If it was specified at the page level then all graphs on a page would be superimposed. (It may be useful to define the size and aspect of a graph at the pagelevelso that several graphs on a page will have the same size and aspect.)</p>
<p>VECTOR Vol.10 No.1</p>
<p>Trellis will use thefacilities of the Windows GUI to provide users with interactive facilities to enter data and to organise the look of a page of graphs. Provision will be made for the actual display and printing of graphs to be driven using Dynamic Data Exchange (DDE) from other Windowsapplications.</p>
<p>MostTrellis users will be unaware thatTrellis is written in APL and most users will not have heard of APL! However, Trellis will be especially convenient for users writing application programs in Dyalog APL/W. They will be able to produce a Data Source in their program and then share this with Trellis. A definition of the structure of a Data Source will be available together with code that can be used to check that a Data Source produced by an application program is structurally correct.</p>
<p>Addendum:Getting that 3D Look</p>
<p>George showedtheresults of a numberof experiments he had been making with the look and feel of the Windows GUIinterface. Oneeffect that wassurprisingly effective, yet very easy to achieve was the generation of three dimensional effects in a window with a grey background. The example above showsa pairs of raised and recessed effects created by drawing white and dark greylines on the light grey background.</p>
<p>73</p>
</details>
