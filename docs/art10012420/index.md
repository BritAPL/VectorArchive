---
vid: '10012420'
title: 'Hacker''s Corner: A Windows Task Killer'
authors:
- Duncan Pearson
byline: by Duncan Pearson
volume: '10'
issue: '2'
page: '126'
source: trad/v102/pearson102_126.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- system interfaces
---

by Duncan Pearson <span class="affil"></span> <span class="email"></span>
{ .byline }

Adrian Smith came to me the other day and said “If I start a Windows program from Dyalog with `⎕CMD` how can I kill it again from within APL ?”. So we dived for the manuals and found out a few interesting things.

First of all the `⎕CMD` function, when used to start a Windows task, returns a number.

```
      +⎕CMD 'notepad.exe' 'minimised'
12345
```

“Aha !” say we, “this looks like a task handle. Why not send a Windows ‘QUIT’ message to the task and see if it kills itself.” If you are a Windows task then getting a `WM_QUIT` is like being locked in a room with a bottle of whiskey and a loaded revolver and being told to get on with it. It looked like a good bet.

So we did. We used `⎕NA` to associate the `PostAppMessage` windows function

```
⎕NA 'I user.P16|PostAppMessage U U I I4'
```

and we posted the task a `WM_QUIT` (message number 18)

```
PostAppMessage 12345 18 0 0
```

Hello DOS prompt, goodbye Windows - "bummer!" said Pooh!

Apparently `⎕CMD` returns something called an `hinstance` which is not a task handle but an *instance handle*. So is there a way from one to the other ? As a matter of fact there is but you have to look for it. There is a pair of functions in `toolhelp.dll` with which one can go round all the tasks in the Windows event-queue collecting information from them. One of the bits of information is the `hinstance` and another is the task-handle.

We associate them thus.

```
⎕NA 'I toolhelp.P16|TaskFirst ={I4 U U U U I I I I I I U C[10] I U}'
<PRE class=aplu>
⎕NA 'I toolhelp.P16|TaskNext ={I4 U U U U I I I I I I U C[10] I U}'
</PRE>
```

They are pretty much the same. Each one takes a horrible structure called a `TASKENTRY` and returns it (the `=` means take and return the argument). We pass `TaskFirst` an empy structure and it fills it with details of the first task in the queue. We pass the filled structure to `TaskNext` and it replaces the information about the first task with information about the next task. We carry on calling `TaskNext` until we find a task with the hinstance that was returned, get the task handle of that task from the structure and send it a `WM_QUIT`.

What I omitted to say was what the horrible structure meant. We get back twelve numbers, a ten character text vector and two more numbers. The text string is the task name, the second number is the task handle and the fourth is the `hinstance`. I‘m sure that the rest is very useful, but not right now so I will ignore it.

\>>>>>>>>>> taskfns + examples of Kill\_Handle and Kill\_Name

## ![Figure 1](pearson102_126-fig1.gif)

(webpage generated: 6 December 2005, 20:40)
