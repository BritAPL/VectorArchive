---
vid: '10000780'
title: SHARP APL Tasks
authors:
- Dan Baronet
byline: by Dan Baronet (danb@dsuper.net)
volume: '16'
issue: '4'
page: '75'
source: trad/v164/dan164.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2000
tags:
- implementations
- language design
---

*by Dan Baronet (danb@dsuper.net)*
{ .byline }

## Introduction

Following Soliton’s announcement of their intent to unleash their APL into the Linux community I decided to review the differences between SHARP APL and APL2 **style** APLs (IBM’s APL2, Dyalog, APL+Win). I have divided the report into several articles, each with a different topic.

This article is the third of the series. In the first one I showed the differences between the languages and enclosed arrays handling. In the second one I wrote about operators. This one is specific to the multi-task capabilities of SHARP APL. Not much comparison can be made with the other APLs since this is fairly unique feature of SHARP APL.

When reading this text keep in mind the date it was written (Nov 1999) as languages change and may render this information incorrect in the future.

## Conventions

In the following text *SAPL* refers to SHARP APL, including *SAM* which is SAPL under MVS and *SAX* which is SAPL under Unix. SAX is the one also running under Linux and compared to APL2 here.

Variables and workspace names are enclosed in ‘quotes’. Functions and files and enclosed in &lt;angle brackets>.

The term *atom* refers to a numeric or character scalar only. The term *item* refers to any scalar. The term *string* refers to a list of characters.

*Italicized* words usually precede their definition.

I use the terms ‘list’ and ‘vector’ in the same way. The terms ‘table’ and ‘matrix’ are also used interchangeably.

## A bit of history

SAPL came to life in the 70s. It is a mainframe product running under MVS and providing a multi-user, multi-tasking environment to the APL programmer. System functions were created early to provide access to these aspects of the system environment.

## Task spawning

There are two types of tasks in SAPL: those controlled from a *Terminal*, called T-tasks, and with no associated terminal, called N-tasks. The latter are also known as background tasks.

A multi-user environment such as SHARP APL/MVS allows a user, with proper access, to control tasks under another user’s ID.

Other multi-user environments may not allow a user to control tasks under another user’s ID. Under single user environments<b>,</b> such as NT, this concept does not exist<sup><a href="#1">1</a></sup>.

In all cases, a task started by another task is said to be its *child* task. The task that started the other one is said to be the *parent* task.

## N-tasks (non-interactive tasks)

These tasks do not require user interaction. They are typically batch tasks processing large amounts of data, or servers interacting with other sources such as Aps (auxiliary processes) or file systems.

An N-task is initiated by the `⎕run` system function. This function takes a string as argument with the following fields:

- user account:password or ‘:’ if same as parent
- workspace name to start including library number if needed
- continue workspace<sup><a href="#2">2</a></sup> upon termination
- CPU limit (0=none)
- time limit (0=none)

`⎕run` returns an error code and task ID if successful. Ex:

```
   ⎕run ': myws mywsEnd 0 0'
0 1234
```

Here we requested an N-task to be started on our own account (:) using ‘myws’ (which must have a valid `⎕lx` in order to run). When the task completes we want the workspace to be saved under ‘mywsEnd’. We have no specific CPU or time constraints<sup><a href="#3">3</a></sup>. APL responded with a “OK, new task id is 1234”. A non-zero return code hints at reasons why the task was NOT started. For example a return code of 13 means that the `⎕lx` was not executable.

### Renaming the “continue” workspace

It is possible to change the name under which the current active workspace will be saved upon task termination, or to prevent such a *save* from occurring.

To change the name, call the system function `⎕twsid` with a string argument. For example, to change its name to ‘crash’ do

```
⎕twsid 'crash'
```

APL returns the name previously in effect.

### Preventing a *save*

To prevent APL from saving the workspace upon N-task termination (or unexpected T-task termination) either call `⎕twsid` with `¯`1 (1 is “allow saving”, 0 is “report current value” and -1 is “disallow saving”) or ensure the workspace is empty at save time by inserting

```
-⎕trap←'$d clear'
```

which creates a domain error which is trapped and <b>d</b>oes **clear** the workspace.

### Splitting the execution of the current code

It is possible to spin off a copy of the current workspace as an N-task by replacing the initial workspace name in the `⎕run` argument by a colon. `⎕run` then returns a result in each task, the parent and the child. The parent’s result includes the child task id and the child’s result is 0 0.

Here is a simple function to execute an expression in the background:

```
    ∇dobehindmyback this;tid
[1] →(0^.=tid←⎕run ': : crashed 0 0')/child
[2] 'task rc/id: ',⍕tid ⋄ →0
[3] child: ⍎this
[4] ⍝ avoid crash workspace if OK
[5] -⎕trap←'$d clear' ∇
```

Note that you cannot split the workspace and have it run under a different account in this version of SAX. The account field MUST be ‘:’.

#### *Arguments to N-tasks*

It is possible to give “arguments” or startup information to an N-task by assigning values to `⎕sp`<sup><a href="#4">4</a></sup> BEFORE starting the task. An N-task’s *initial* `⎕sp` is always the same as the parent’s `⎕sp` at the time of the `⎕`run.

#### *Reporting on running tasks*

`⎕runs` will report all tasks running under or started from the current account. `⎕runs` reports the task ID, its owner, its parent task ID, its type (1=N-task), its CPU and time usage and its CPU and time limits. The result is an 8 column table, one row per task.

## S-tasks (shared-variable tasks)

In SAPL, the interpreter receives commands through an interface that varies from one machine to another. For example, on a mainframe, users at IBM3270 type terminals will communicate with APL through a *session manager* responsible for performing terminal I/O. The interpreter itself is not concerned with terminal management details.

The bare bones interpreter is available for use through an auxiliary processor (AP) in SAPL. AP1 is an interface AP which permits users to submit requests to the interpreter and receive its output. AP1 does only that. It is not responsible, for example, for inter-user security issues. This means that a task must go through the regular signon procedure to start another APL task.

In SAM this means a user task can start another task on another user account. Under Unix (Linux) this is not possible. Yet<sup><a href="#5">5</a></sup>. SAX does not allow a user to start tasks under a different account number.

#### *Usage*

To start an S-task simply share a variable with AP1:

```
1 ⎕svo 'cmd'
```

An S-task request requires a prefix to distinguish normal APL statements from, say, an interrupt request. The “normal APL statement” prefix is `4⍴0{⎕av`. For example to ask ‘2+2’ to be processed in the previous S-task we use:

```
cmd←prefix,'2+2'
```

APL processes the requests and puts the output of the result line by line, including the final APL prompt, if any, into ‘cmd’. In the example above ‘cmd’ should contain '4' followed by a CR `(⎕av[13])`, followed by 6 spaces (the APL prompt).

Each line has a 6 byte descriptor which includes the length of the line and the state which your task is in (immediate execution mode, quote-quad input, etc.). You need to parse the result to make some sense out of it:

```
   ∇r←read var;s;len;prefix;⎕io
[1] r←⍴⎕io←0 ⋄ s←⍎var
[2]get1section:len←256⊥⎕av⍳2↑s
[3] prefix←2↓6↑s ⋄ r←r,6↓len↑s
[4] →get1section⍴⊂0<⍴s←len↓s
   ∇
```

Here the prefix is not used but a more serious example would take it into account. The following line should read in the answer from the previous statement and return '4',CR,`6⍴' '`

```
  read 'cmd'
```

Most SAPL installations come with a workspace (found in library 1) containing a set of functions to start and use one S-task at a time. SAX for *linux* may too.<sup><a href="#6">6</a></sup>

#### *An example of use*

Other APLs often have the ability to get input from a source other than the keyboard. This can be useful when simulating situations or testing cases.

SAPL does not have this ability. It does, however, allow S-tasks to be used the same way with the added bonus of being able to modify the input stream.

For example, suppose we want to run a test suite on a program. With APL\*PLUS II we can redirect the input by using the command

```
⎕in 'source'
```

and test our program with it. If something goes awry midstream and the program stops, the input continues to flow, possibly generating further errors. Here is a simple case where &lt;add> is undefined:

```
    ∇test
[1]  add input 'Your name: '
[2]  add input 'Your address: '
    ∇
test
```

<pre>Your name: <b>danb</b> (← provided by the input stream)
value error
test[1]  add input 'Your name: '
           ^</pre>

It is now futile to provide further input. In such pre-determined source mode there is little we can do.

Under S-tasks the output generated by the error can be analyzed and acted upon. In this case the program could detect and report the error and continue doing something else like another test.

#### *An example: multi-task manager*

In 1986 I was part of a large two-man<sup><a href="#7">7</a></sup> team in charge of a large company’s data on I.P.Sharp’s machine in Toronto. Our daily tasks included firing up many tasks using typewriter type terminals and answering fairly straightforward prompts. We needed one terminal per task to do this. We would monopolize up to 5 terminals at once. Our popularity fell below norms when we did this as terminals were a scarce resource. This is when I decided to look into S-tasks.

It turned out to be as simple as starting one task which fired up the 5 others through five shared variables, each responsible of issuing answers to prompts and shutting normally if all went well.

#### *An example: multi-task manager (cont.)*

A few years later I started remote consulting via a single asynchronous line. Much of my work involved signing on to several accounts at once and juggling them. With only one physical line it was difficult. I then wrote a simple multi-task handling program that would be responsible for starting S-tasks and switching between them. I still use this program to this day.

The entire program relies on quote-quad for input and output. I added a few “system commands” of my own to allow to return to the parent task (the workspace that started all the S-tasks) and also to allow transfer of objects between tasks. For example, if I wanted variable ‘z’ in S-task 3 to be also in S-task 5, I would issue the “system command”

```
)xfr z /from=3 /to=5
```

and my program, detecting the ‘)xfr’ would switch to S-task 3, store variable ‘x’ somewhere<sup><a href="#8">8</a></sup>, switch to S-task 5 and read it.

This turned out to be so useful that I would also use it with an IBM3270 terminal when I was at my client site even though we could use several 3270 sessions at once (sometimes all lines would be busy and we could not start new sessions).

I also added an )edit command such that when I was using a 3270 I could mimic APL2’s edit command by transferring the object to edit to the parent task (using the same transfer code as above), use the full screen editor in the parent task and return the modified object to the S-task.

#### *An example: user commands*

Still later on, I was working with STSC<sup><a href="#9">9</a></sup> mainframe APL (like SAPL in many ways) and fell in love with user commands. I wrote several commands and worked with them until I left the now defunct company and went back to SAPL.

User commands are specific to the APL+ family of APLs. These allow a programmer to create and use utilities as commands invoked like “system commands” by substituting ‘\]’ for ‘)’ as in

```
]mycmd {arguments}
```

You can create a user command to, say, scan the workspace for specific strings or whatever you might find useful.

A `⎕ucmd` system function also exist to call user commands under program control.

All STSC APLs (APL/PC, APL\*PLUS II, APL+Win) offer this feature.

The mechanics are fairly simple and can be mimicked easily under any APL.

But why would I want to do this?

When I went back to remote SAPL consulting I realized how much work I had put into those user commands and how much I missed them. And how useful that S-task based multiple task interface was.

I ported the STSC files to SAPL<sup><a href="#10">10</a></sup> and created a &lt;UCMD> function to mimic STSC’s `⎕ucmd` system function. I then modified my S-task interface program to call my function whenever an immediate execution statement started with ‘\]’. It would first verify that my &lt;ucmd> function was in the workspace (by )<i>copy</i>ing it if necessary) and then call it with whatever was to the right of ‘\]’.

Of course I’ve ported all this code to SAX under *linux*.

#### *Reporting on running S-tasks*

These are reported by `⎕run` with a type of 0 (like T-tasks) in column 4.

## Conclusion

SAPL is definitely different from other APLs in how it handles multiple tasks. Both S-tasks and N-tasks are extremely useful. The ability to run programs unattended can be done easily in SAPL while remaining IN APL.

Both N & S-tasks are useful in their own way. To me, S-tasks are more like little children tasks, in a sense that you must provide input and steer them all the time. N-tasks are more like teenager tasks. You have little control over them.

Report questions/remarks to [danb@dsuper.net](mailto:danb@dsuper.net). Utility workspaces for SAX are available at [www.dsuper.net/~danb/milinta/](http://www.dsuper.net/~danb/milinta/).

Dan Baronet  
Montreal  
Canada  
Email: [danb@dsuper.net](mailto:danb@dsuper.net)

### Footnotes

<a id="1"></a><sup>1</sup> The closest that we can come to is the ability to multi-thread, a concept used by Dyalog APL

<a id="2"></a><sup>2</sup> Under SAPL, workspace names are limited to 11 in length

<a id="3"></a><sup>3</sup> They are not honoured in this version of SAX

<a id="4"></a><sup>4</sup> `⎕sp` is the session parameter, a system variable which survives ‘)load’. This variable and other SHARP APL specific language elements were reviewed in my first article

<a id="5"></a><sup>5</sup> I am told it will be possible in the future

<a id="6"></a><sup>6</sup> if it doesn’t see Milinta’s site for multiple S-task version

<a id="7"></a><sup>7</sup> the other man was a woman

<a id="8"></a><sup>8</sup> It could have been on file but I used the SVP instead

<a id="9"></a><sup>9</sup> alias Manugistics, alias APL2000

<a id="10"></a><sup>10</sup> of course a few commands never made it because of incompatibility between the two APLs but for the most part all went well

***
