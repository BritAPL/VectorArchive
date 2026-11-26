---
vid: '10011610'
title: Analysing CONTINUE workspaces
subtitle: A light-hearted look at the Dyalog CONTINUE workspace,and what to do with it
authors:
- Ray Cannon
byline: Ray Cannonray_cannon@compuserve.com
volume: '23'
issue: '1'
page: '104'
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
source: trad/v231/cannon.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- development practice
- implementations
---

A light-hearted look at the Dyalog <span class="smallcaps">CONTINUE</span> workspace,  
and what to do with it
{ .subtitle }

Ray Cannon  
[ray\_cannon@compuserve.com](mailto:ray_cannon@compuserve.com)
{ .byline }

This article contains annoying and exuberant references to obscure artefacts of British culture, inappropriate to a serious journal of international reputation. Our first scheme was liposuction: to edit them out as waste tissue. However, investigation revealed them inseparable from the skeleton and sinews of this piece. With the deepest regret, we print them as submitted. Ed.
{ .ednote }

They sought it with thimbles, they sought it with care;  
They pursued it with forks and hope;

I just tried googling for “continue.dws” and came up with a mere “7 English pages”, none of which appeared to be related to Dyalog APL. So, not many people out there in the Wild Word World of Googleland appear to know about Dyalog’s <span class="smallcaps">CONTINUE</span> workspace.

But *we* all know it is what you get when you cross a bug in a workspace with the Dyalog runtime interpreter.

## Drunk in charge

There are only two types of vodka, good vodka and great vodka. There is no bad vodka, just not enough vodka.

There are only two types of code, Untested Code and Part-Tested Code. There is no Fully-Tested Code, and there is never enough vodka.

Errors in code fall into three types:

1. errors that work, but produce the wrong answer; e.g. two items are added rather than subtracted;
2. errors that cause the code to crash; e.g. dividing by zero;
3. errors that trigger error trapping; e.g. dividing by zero under error trapping.

<span class="smallcaps">CONTINUE</span> workspaces only result from the second, and then only ever under runtime Dyalog APL.

## Hunting the Snark

He had softly and suddenly vanished away —  
For the Snark was a Boojum, you see.

Just like out-of-date comments in the code, I lied. There is also a fourth type, the System Error (<span class="smallcaps">SYSERROR</span>). Unlike <span class="smallcaps">CONTINUE</span> workspaces, the developer’s version of Dyalog supports them.

`aplcore` is the name of the file on disk containing the snapshot of the core memory used by Dyalog APL, at the point the interrupter discovered the system error.

A developer can cause <span class="smallcaps">APLCORE</span>s by incorrect calls using `⎕NA` or OLE links etc., to external features. Bugs in the interrupter can also result in a <span class="smallcaps">SYSERROR</span>. (Dyalog would like to know about the latter. In particular, they would like to know how to reproduce the <span class="smallcaps">SYSERROR</span> with the minimum of code.)

Since it cannot be trapped, or directly recovered from, it is often very hard to discover exactly where in the code the <span class="smallcaps">SYSERROR</span> was triggered. It may, however, be possible to extract data from the <span class="smallcaps">APLCORE</span> workspace, although sometimes this in turn results in another Boojum.

## Fit the First – The Landing

“Just the place for a Snark!” the Bellman cried,  
As he landed his crew with care;

The first problem that we (the APL developers) have with the <span class="smallcaps">CONTINUE</span> workspace is: How do I get hold of it? Because unlike “real programmers” (the Bellman and crew don’t eat quiche or Beaver but do run `dyalog.exe`), the “real users” only ever use `dyalogrt.exe`. So for us, the <span class="smallcaps">CONTINUE</span> workspace normally appears on someone else’s PC.

I gave up expecting users to send me their <span class="smallcaps">CONTINUE</span> workspaces merely because I asked them to. They will do so only if their jobs (not mine) depend on them doing so.

The question becomes “How do we make the <span class="smallcaps">CONTINUE</span> workspace into a homing pigeon or boomerang?” (Fade up Charlie Drake singing “My boomerang won’t come back”.)

## The Star Wars guide to the Galaxy

(In which Darth Vader and the Vogons meet Dr Doolittle and Arthur Dent, and they all get their Blue Peter Badges.)

The APL developer has the basic PushMePullYou choice of the “tractor beam” or the “bulldozer” approach:

- The tractor beam (driven by “Darth Vader, developer from hell”) locks onto a <span class="smallcaps">CONTINUE</span> workspace floating out there in the network of space but within range, and pulls it into the Death Star.
- The bulldozer (driven by the Vogon user acting as a sub-contractor), pushes the <span class="smallcaps">CONTINUE</span> workspace across the network (over Arthur Dent’s house) into the arms of the patiently waiting (property) developer Zaphod Beeblebrox.

I prefer the tractor-beam approach. The application workspace need know nothing about the developer, it needs no general error trapping, and it has to do nothing. It is powerless to stop itself being dragged into the clutches of the Evil Empire. It also works on <span class="smallcaps">APLCORE</span>s.

Alternatively, you have to build the bulldozer (or Vogon Constructor fleet) into every workspace, setting up complex Snark traps in the hope you won’t catch a Boojum.

The main drawback to the tractor-beam approach is that the target must be in range. That is to say, the <span class="smallcaps">CONTINUE</span> workspace must be visible (readable) by you, Darth Vader, across the network, and at a known location. Whereas, the Vogons, in their bulldozer, need know only Arthur Dent’s home planet address; i.e. your PC’s IP address or name on the network.

A third alternative is the “here is one I made earlier” Blue-Peter approach. The workspace at start-up, before normal processing, looks for an existing (earlier) <span class="smallcaps">CONTINUE</span> workspace and, if found, sends it across the internet back home. This has the advantages of both the tractor beam (no error trapping required) and the bulldozer (can work from an unknown location), but the downside that you only receive the <span class="smallcaps">CONTINUE</span> workspace the next time that user runs the system. Like the tractor beam, it also can work with <span class="smallcaps">APLCORE</span>s.

## A joint Spanish Inquisition/KGB production

You may threaten its life with a railway-share;  
You may charm it with smiles and the comfy chair.

The developer, having got the poor defenceless <span class="smallcaps">CONTINUE</span> workspace into his grubby hands, is now able to `)XLOAD` it into the development APL environment, and, using all the tools of the inquisitor, can extract all the information (who, what, how, when, where, etc.) contained within. (Film: Think *Marathon Man*, Dustin Hoffman, Laurence Olivier. Oh *no* – not the dentist’s chair!)

The two most important pieces of information contained in the <span class="smallcaps">CONTINUE</span> workspace can be found in `⎕DM` and `⎕SI`. However, if you have used the bulldozer, you may need to defuse the error trapping – before the booby trap blows up in your face. You can then safely cut the stack back to the Snark.

Other useful information about the (real) user and their environment is, available from:

```
⎕AN
⎕WSID
GetCommandLine
⎕WG 'AplVersion'
```

Knowing the IP address, the version of Microsoft Word and the operating system version etc., would also be nice. But from the `)XLOAD`ed workspace can be read only information about the developer’s environment on *your* PC, not the user’s runtime environment on *his* PC.

So I suggest, again in true Blue-Peter fashion, that you get this information using the “Here’s one I made earlier” approach…

Go back in your time machine, and either:

- Following the error triggering the error trap, before starting up your bulldozer, save all the information you need;

or do as I do and:

- Get your workspace’s start-up function to save *all* the environment information you can lay your hands on into a global variable.

Then, when your tractor beam captures a workspace, it already has this information nicely stored away. (This is also true even if your Boojum has turned into an <span class="smallcaps">APLCORE</span>.)

## Errorbot

As he wrote with a pen in each hand,  
And explained all the while in a popular style

If your users are bent on producing lots of errors you may wish to automate the documentation of them, via an “errorbot” which, without your intervention, quietly extracts the end-user environment and error information, and then (with a pen in each hand) writes it to log file, printer, web page and email.

Under Dyalog APL it is possible, but not straightforward, to create an environment that can automatically extract the `⎕DM` and `⎕SI` as text from a <span class="smallcaps">CONTINUE</span> workspace. I will leave that as an exercise for the advanced user to work out. (I believe it is also now much simpler under Version 11 to extract this info from an <span class="smallcaps">APLCORE</span> workspace.)

Alternatively, your bulldozer error trap can attempt to save this information at the point of the error into variables known to your pet errorbot, from where a simple `⎕CY` command can extract it.

## Code snippet

N.B. The last (fourth) line of APL is split onto several lines, as shown by ellipses. `⎕ML` is assumed to be `0` or `1`. The code is `⎕IO`-independent.

```
#.quaddm←⍬        ⍝ initialise ⎕DM store
#.quadsi←0⍴⊂''    ⍝ initialise ⎕SI store
continue←'''',(ExpandPath ⎕WSID,'..\..\continue.dws'),''''
⎕TRAP← (0 'E'(
  …  '⎕TRAP←(0 1000) ''S''
  …  ⋄ #.quaddm,←⎕dm
  …  ⋄ #.quadsi←↑⎕XSI,∘{''['',(⍕⍵),'']''}¨⎕LC
  …  ⋄ ⎕save ',continue,'
  …  ⋄ ⎕off '))
```

## Restarting and Resetting

“Just the place for a Snark! I have said it twice:  
That alone should encourage the crew.  
Just the place for a Snark! I have said it thrice:  
What I tell you three times is true.”

If you `)XLOAD` a <span class="smallcaps">CONTINUE</span> workspace, it may be possible to correct the problem and then restart the workspace to test if the fix has worked.

**File ties** are independent of saving or loading workspaces. So an `)XLOAD`ed <span class="smallcaps">CONTINUE</span> workspace will need any files retied before restarting.

I have found that by:

1. using `)RESET` to clear the stack;
2. running the workspace to a stop placed near where the error occurred;
3. without untying the files, `)XLOAD`ing the <span class="smallcaps">CONTINUE</span> workspace again

we are almost ready to restart the workspace.

Unfortunately, the <span class="smallcaps">CONTINUE</span> workspace does not contain the **`⎕PATH`** at the point of the error. So before restarting, you may need to set the `⎕PATH`.

<span class="smallcaps">CONTINUE</span> workspaces preserve even **OLE links** to Microsoft Word, etc.!

Now activate **Debug Mode** via Ctl-Enter, and you’re ready to roll. (Or at least roll over dead with the same error the user got.) If you can’t reproduce the error using his data, start by looking at the differences between his environment and yours.

A pre-Version 11 <span class="smallcaps">APLCORE</span> cannot be `)XLOAD`ed but data may sometimes be **copied** into the active workspace with `)COPY` or `⎕CY`. (An `)XLOAD` is preferable because it preserves the stack.)

To determine, without the stack, where the <span class="smallcaps">SYSERROR</span> occurred, I find it useful to `⎕CY` the whole of the <span class="smallcaps">APLCORE</span> into a clear workspace, and then look at all the **local variables**.

```
      )CLEAR
CLEAR WS
      ⍝ use ⎕CY because )COPY omits local variables
      ⍝ APLCOREs don't have .DWS extensions, so use full pathname
      ⎕CY 'C:\Dyalog\APLCORE.'
```

Since the local variables of functions on the stack at the point of the <span class="smallcaps">SYSERROR</span> are accessible, their names and values can be used to determine which functions are in the stack. Distinctive local names help this process; reusing similar names, (e.g. `x`, `y`, `r`, `data`, `text`, `mat`, `name`) make it far more difficult.

## Here is one I made earlier

“Two added to one – if that could but be done,”  
It said, “with one's fingers and thumbs!”  
Recollecting with tears how, in earlier years,  
It had taken no pains with its sums.

If you really want to know exactly which functions are active at the time a <span class="smallcaps">SYSERROR</span> occurred, try putting into every function a **label** that identifies the function’s name. For example, for a function `Foo`, name the label `Lab_Foo`.) Only the labels of functions in the stack will be `⎕CY` copied from the <span class="smallcaps">APLCORE</span>, so listing all variables starting with `Lab_` will list all the functions on the stack at the time of the <span class="smallcaps">SYSERROR</span>.

Unfortunately you can’t put a label in a D-function, but if the Dfn is *dynamically* created and localised, it is not a problem. Then the very fact that the Dfn has been copied by `⎕CY` proves it was on the stack at the time of the <span class="smallcaps">SYSERROR</span>.

## Rules of thumb

1. Untested code always fails in production.
2. Part-tested code that fails in production often does so on “edge conditions”.
3. That the code works with 2, 3 or 4 elements, but fails with a scalar, is a mistake typical of a newbie. A more experienced writer’s code might fail with *zero* elements. Well-tested code could still fail with twenty million elements (or fewer) due to `WS FULL`.
4. Virtually all code will fail at the extremes, or if the environment is changed.

## “Have you stopped beating your wife yet? A simple yes or no answer is all that is required.”

“The fourth is its fondness for bathing-machines,  
Which is constantly carries about,  
And believes that they add to the beauty of scenes–  
A sentiment open to doubt.

The following examples are all taken from the pensions industry.

A person’s sex, (ignoring sex changes, and other medical oddities) can simply be held as a Boolean, e.g. `0` for male and `1` for female. However there is a third possibility, “Not applicable”. The sex of someone’s spouse is N/A if there is no spouse. Hence the ‘3-way Boolean’: `M` for male, `F` for female and `N` for not applicable.

A frequent fault is caused by common code running off variable data.

One pension system I have worked on handles over 220 pension products from five former insurance companies, now all merged, but still having data held on the five original mainframe systems. Feeds from these differ by product and mainframe.

Failing to reconcile data codes resulted in errors. For example, when the code for a payment frequency (monthly, quarterly, half-yearly etc) was set to `A` for ‘annual’ on one mainframe and `Y` for ‘yearly’ on another, errors were encountered.

**Tip** When writing a case statement, list all known cases and let the default `ELSE` be an error. (Rival UK adverts come to mind: “Kills ALL known germs” “Kills 99.9% of all germs”.)

```
:Select payment_frequency
:Case 'M'
    …
:Case 'Q'
    …
:Case 'H'
    …
:CaseList 'A' 'Y'
    …
:Else
    ERROR
:End
```

Do not be tempted to let the `:Else` replace the `:CaseList 'A' 'Y'` That way, when the code gets a `F` (Fortnightly? Four-weekly?), it is rejected and does not default to the inappropriate Annual case.

“I said it in Hebrew – I said it in Dutch –  
I said it in German and Greek:  
But I wholly forgot (and it vexes me much)  
That APL is what you speak!”

Well that’s all I have time for now.

“What’s the good of Mercator’s North Poles and Equators,  
Tropics, Zones, and Meridian Lines?”  
So the Bellman would cry: and the crew would reply  
“They are merely conventional signs!”

(With apologies to Lewis Carroll, Douglas Adams, George Lucas, and Dyalog. However, I make no apologies whatsoever to the Spanish Inquisition or the comfy chair.)
