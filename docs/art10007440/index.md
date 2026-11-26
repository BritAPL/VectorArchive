---
vid: '10007440'
title: A Skeleton Key to Windows Performance Monitoring
authors:
- Bill Rutiser
byline: by William Rutiser (a short talk given at the APL2000 Meeting, 2003)
volume: '21'
issue: '3'
page: '141'
source: trad/v213/rut213b.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- performance
- system interfaces
---

by William Rutiser (a short talk given at the APL2000 Meeting, 2003)
{ .byline }

These notes are appropriate to Windows 2000 Professional. Things are likely to be more or less different on other versions of Windows. Yes, the title is misleading; perhaps it should be “A Crowbar and a Roadmap to Some Starting Points….”.

Begin with the “Start” Menu then select “Settings” then “Control Panel” then “Administrative Tools” then “Performance” Subsequently this will be condensed to select “Start | Settings | Control Panel | Administrative Tools | Performance” This will start the “Performance Tool” The Performance Tool is a snap-in for the Microsoft Management Console or MMC. MMC is a general-purpose facility; this tool is just one instance. It starts with two panes. The left pane contains a page named “Tree” that shows a tree-view starting at “Console Root” Select “System Monitor” This opens a promising looking graphical display in the right pane. Selecting “Help | Help Topics | Index | System Monitor” will bring up the MMC Help viewer with a list of sub-topics. “Overview” and “interface” may be good starting points. Also see “settings reference”, “toolbar reference”, and “configurations, setting up”

What’s going on here? The kernel keeps track of many different statistics of interest when analyzing performance. Current values of the statistics are available in counters. The System Monitor can display a graph of selected counters changing with time. There are a lot of counters to choose from and a lot of options to twiddle. There appears to be a way to save display configurations for future use.

We need to add some counters to the display.

Select “Help | Help Topics | Index | adding performance counters | System Monitor” to read how to do it. As an example try:

- Select the “New Counter Set” icon (leftmost in the System Monitor toolbar) to clear any selected counters.
- Right click the System Monitor details pane and select “Add Counters…”
- Wait patiently for the dialog.
- Select “Performance Object | Memory”
- Select “Select counters from list ”.
- Select “% Committed Bytes in use”
- Select “Explain” This shows the official definition of the counter.
- Select “Add” to add this counter to the displayed counter set.
- Repeat the last three steps for “Page Reads/sec” and “Page Writes/sec”
- Now select “Close”

You should now see an updating display with three coloured lines. At the bottom of the pane is a control with a row for each counter in the display set. Just above this are some statistics for the selected counter. Select different rows.

If your computer is not doing much, you should see relatively flat lines and generally low numbers. Start some applications to stir things up. Open a version 4.0 APL interpreter and clear to a large number. Add a version 5.0 interpreter. Try a large slow one liner. Watch the graphs and numbers change. You should now be able to experiment with different configurations. Explore.
