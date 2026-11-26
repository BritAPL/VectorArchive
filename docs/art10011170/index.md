---
vid: '10011170'
title: 'Review: J Release 2'
authors:
- Dave Ziemann
byline: by Dave Ziemann
volume: '11'
issue: '1'
page: '65'
source: trad/v111/ziemann111_65.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- reviews
- implementations
---

by Dave Ziemann
{ .byline }

By the time you read this, J Version 2 should be on the streets; I am reviewing an early beta-test copy of the product, which is at this time (mid-June) is still called J version 8.

For this reason, this review focuses on significant product features rather than on software deficiencies or apparent resource requirements, the characteristics of which are likely to change prior to release.

## ![figure\_1](ziemann111_65-fig1.gif)

### First Impressions

I had not looked at J since version 6.2 and was immediately surprised and impressed. J version 8 is a commercially oriented fully Windows-integrated product. The DOS version of J is included on the beta-test disk, but is clearly being de-emphasised. I don’t know if it will be included in the product, and I didn’t look at it. It looks as if ISI are concentrating their J development on Windows.

Installation is from Windows; you type ‘A:SETUP’ from the Run option in Program Manager – a reassuringly familiar activity. The standard Microsoft setup screen then takes over, and the files are decompressed and copied to the chosen target directory without fuss, and in only a few minutes.

The result of this is a new Program Manager Group which contains an icon for the J executable. Double-clicking this icon puts you straight into J8 – and what a difference!

What you see is an MDI parent window, which contains a single MDI child – a J execution window (with the label “1.JX”). Typing something into this window and then pressing the Enter key causes the entry to be executed as a J sentence, and the result displayed in the same window.

## ![figure\_2](ziemann111_65-fig2.gif)

### The MDI User Interface

The abbreviation MDI stands for Multiple Document Interface, and is a standard interface protocol familiar to users of many Windows applications. Because this looked like an MDI application I hopefully assumed that it meant I could open multiple J script windows, all as children of the main window. I was not disappointed.

The MDI frame hosts a number of menus and some familiar icons. Clicking the first icon opens a new window with the label “1.JS”. This is an empty window ready to receive J sentences that form part of a J script. Pressing the Enter key in a script window does not cause the sentence to be executed; we are building a script for deferred execution.

An active script window can be executed from a menu option or simply by pressing Ctrl-T. Each sentence in the script is transferred to the execution window and then executed, so that the script is “run”. Alternatively, Ctrl-R will run the script without transferring each line to the execution window, which saves cluttering it up. Screen Shot 2 shows an open J script window and two minimised script windows.

Clicking the second icon presents a file-open dialogue, and an invitation to select a saved script file. Pressing OK then opens a script window on the selected script file. These features make defining, storing, retrieving, editing and executing J scripts a real joy. It just feels right.

The File menu also contains an option for opening a new J execution window. This means you can have more than one window in which J sentences can be entered and executed. You could use different J execution windows to reflect different lines of reasoning on the same problem – each execution window is a view on the same J session. The next three icons in the MDI frame let you cut, copy and paste, as one might expect. There are also icons for printing the contents of a window, for getting an “about” box, and for activating the Windows Help File associated with this product.

The File menu contains a rather splendid Word for Windows style Print Preview option, which can be used to preview the print form of a J execution or script window. In print preview mode three levels of magnification are supported, and you can page forwards and backwards through the preview. The menu also contains an option for setting the font to be used for the currently active child window. ISI supply their own TrueType font called ISIJ with the product, and the selectable sizes range from 8 to 72 point.

The nice ISI scaleable font and the funny icons in the MDI frame give me an idea. I wonder if it might not be possible to replace those one and two-character J mnemonics by special symbols whose graphical appearance reflects their function? Oh well, just a thought.

In the beta-test version the label in the main window was “J Application Development Environment”, which ought to mean that the product should be called JADE, but I am told that it will appear as J Release 2.

### Distributed Scripts

After playing a bit with the new interface I started to poke around the J scripts which were distributed with the beta release. There is already a lot of good material here, and it is clear that there is yet more to come.

The supplied PROFILE.JS script conditions the environment in a new and interesting way. Script files are apparently classified as belonging to one of three libraries, Private, Public or Project, in a manner which is reminiscent of certain mainframe system environments.

The z locale is populated with a host of more general utilities. To give just two examples, the verb deb deletes extraneous blanks from a list of characters, and the noun LF contains a line-feed character.

I found a lot of utility and demonstration scripts in the beta release, much of which was obviously still in development. There are demonstrations of how to program Windows controls such as list boxes, dialog boxes, font dialogues. There are a number of graphical demo scripts; a traffic-light simulator, a bouncing ball, a polygon painter, and a rather attractive implementation of a game called enigma which made excellent use of colour.

The Windows programming is performed via the Window Driver tool, which has been the subject of further development. This powerful windows scripting language is also available in ISI’s APL product. Visual Basic controls are already supported as Window Driver child windows, and the final product will provide business graphics and a spreadsheet control.

The Windows Driver tool can also be used to dynamically add your own menu items to the main window. You can arrange for the selection of such menu items to result in the execution of an arbitrary J sentence. This is in fact how the active script window execution facility is supported; Ctrl-R just acts as the accelerator keystroke for its corresponding MDI menu option.

There are scripts for printing text via Windows Write, and for directly driving a Laserjet printer. I also found scripts for performing financial and actuarial computations.

Scripts providing ODBC access to a wide range of databases via the Q&E Database Library product are promised for the final product. This should provide access to local as well as network-based SQL databases.

A lot more is promised for the final release, and the distributed scripts will be documented in the J Users Guide.

### Language Changes

As usual, there are a number of incompatible language changes of which J users need to be aware. The given names feature (abd:) has been decommitted. J workspaces are also decommitted. A number of changes have been made to the arguments of certain derivations of the foreign conjunction. In particular, the way that the script reader works has changed, so that it is now possible to read a script either from file or directly from a noun.

Most seriously perhaps, the name classes have been redefined as follows:

```
     was     is
noun     2     0
adverb     4     1
conjunction     5     2
verb     3     3
```

The vaguely numerological rationale here is that cases 0, 1 and 2 represent the valence of the name, and 3 represents the sum of the possible valences (monad + dyad = 3).

The Explicit Definition verb has been extended. For example, you can now enter a definition by opening the keyboard for input using n : 0 as follows:

```
   noun =. 0 : 0
how now
brown cow
)
   noun
how now
brown cow
```

The 0 : 0 opens the keyboard for entry of a list of characters, and closes it when a singleton close parenthesis (followed by a return) is encountered. A line-feed character terminates each line. Varying the left argument controls the class of the object produced. A verb, for example can be directly defined as follows:

```
   foo =. 3 : 0
2*y.
:
x.+y.
)
   foo 3
6
   2 foo 3
5
```

The singleton colon in the example above separates the definition of the monad from that of the dyad. The colon and the dyadic definition can be elided. Similarly, 1 : 0 and 2 : 0 can be used to define adverbs and conjunctions respectively.

The right argument can be text instead of the number 0. For example:

```
   ]v =. '2*y.',LF,':',LF,'x.+y.'
2*y.
:
x.+y.
   (3 : v) 3
6
   2 (3 : v) 3
5
```

The text can be a line-feed delimited string, a character matrix or a list of boxed strings. Adding 10 to the left argument will yield a tacit definition where possible, for example:

```
   3 : '2*y.'
┌─┬─┬────┐
│3│:│2*y.│
└─┴─┴────┘

   13 : '2*y.'
┌──┬─┬─┐
│2:│*│]│
└──┴─┴─┘
```

### Locales

A good understanding of locales is now necessary in J, particularly as it looks like their use will pervade the system environment. If you think about what other APLs are doing with namespaces, then you will have a pretty good idea of what locales are about.

The name mean\_a\_ is called a locative, and is said to refer to the name mean in the locale a. By contrast, an unqualified name such as foo is resolved by referencing the base locale.

For example, if the file ’stats.js’ contains the following script:

```
mean =. sum % #
sum =. +/
```

then the following sentence defines a verb that will execute a script in locale a:

```
exec_a_ =. 0 !: 0    NB. execute script silently in locale a
```

and the following usage populates the locale a with the script contents:

```
   exec_a_ <'stats.js'     N.B. so then ...
   nl_a_ 3   NB. namelist of verbs in locale a
┌────┬────┬───┐
│exec│mean│sum│
└────┴────┴───┘

   mean =. 34
   sum =. 'hello'
   mean_a_ 3 4 5
4
```

The name mean\_a\_ is resolved by executing the verb mean in locale a. The value of sum in that locale is the verb rather than the homonymous noun defined in the base locale. There is also a special locale, the z locale, from which names unresolved in other locales can be inherited. For example:

```
   sum_z_ =. */
   mean_a_ 3 4 5
4
```

but then

```
   erase 'sum_a_'   NB. erase sum in locale a
1

   nl_a_ 3
┌────┬────┐
│exec│mean│
└────┴────┘

   mean_a_ 3 4 5
20
```

Erasing sum in the locale a causes the value of sum (referenced in mean) to be inherited from the special z locale. The z locale can be populated with common utilities, which can be referenced anywhere, but do not clutter up the base locale. For example:

```
   nl_z_ 3

┌─────┬──┬───┐
│erase│nl│sum│
└─────┴──┴───┘
```

In addition, the j locale is typically populated with useful environmental utilities such as verbs that display the names of scripts in the libraries, or others that open windows on and permit the editing of named script files.

By the way, the name class of a locale is 6, so that:

```
   nl 6
┌┬─┬─┐
││j│z│
└┴─┴─┘
```

The leading empty box represents the anonymous base locale.

Try as I might, I could not succeed in nesting locales. Executing a\_b\_c\_ =. 42 only serves to create the noun a\_b in the c locale. What I wanted of course, was to create a noun a in the b locale of the c locale. The most recent release of Dyalog APL will support nested namespaces.

### Control Structures

Yes, it’s true. ISI have put control structures into J. And yes, they are different from anyone else’s. I feel strongly ambivalent about this; on one hand they pervert the functional simplicity of the language; on the other hand, if there ever was a language that could be made easier to understand with control structures, J is it. The real justification to my mind, is that they are required to make J acceptable in a commercial arena beyond the APL market place. (The same argument does not in my view apply to APL\*PLUS III, but that’s another story...)

J’s control structures are provided via a set of dot-terminated control words, as follows:

```
  break.
  catch.
  continue.
  do.
  else.
  elseif.
  end.
  goto_<name>.
  if.
  label_<name>.
  return.
  try.
  while.
  whilst.
```

The control words if., try., while. and whilst. all mark the start of a block which is terminated by a corresponding end.. Usage of these can be summarised as follows:

```
 if.  T do. B end.

 if.  T do. B else. B1 end.
 if.  T do. B elseif. T1 do. B1 elseif. Tn do. Bn end.

 try. B catch. B1. end.

 while.  T do. B end.
 whilst. T do. B end.
```

Words with B or T denote blocks of J sentences. The last sentence in the block determines the block to be executed next, depending on whether its leading atom is non-zero. The final result is the last sentence executed that was not in a T block.

The control words while. and whilst. differ only in that the latter skips the test the first time through, so that a while. structure may never execute its block, whereas a whilst. block is executed at least once.

The control word break. goes to the end of a while. or whilst. structure, whereas continue. goes to the top.

The try. and catch. structures permit recovery from an execution error. For example in the example in the paragraph above if block B executes successfully, then B1 is not executed. If however the execution of block B fails, then block B1 is executed.

The goto\_ and label\_ forms provide a family of control words which allow branching to arbitrarily labelled paragraphs. I think it is a shame that these have been included. They are at best unnecessary and at worst severely detrimental to code structure. Why should a highly functional language like J add them now, just as other language designers are recognising their disadvantages and are busy removing them?

The following verb demonstrates the use of control structures. It finds the root of a function f from a two-element list that brackets the root:

```
   root =. 3 : 0
mean =. +/ % #
while. ~:/y.
 do. if. ~:/*f b=. (mean,{.) y.
         do. y. =. b
       else. y. =. (mean,{:) y.
 end.
end.
mean y.
)
   f =. *: - +:  NB. square minus double
   root 0 100
2
```

### The J Package

The product is priced at around US$50 for personal use, and somewhere around US$500 for a commercial licence. The documentation will apparently comprise three perfect-bound manuals, the Introduction to J, the Dictionary of J, and the new J Users Guide. The latter was not available at the time of this review. At the time of writing I did not have any information about commercial product support fees.

### The Future

The speed with which ISI have moved J from a research tool to a viable commercial Windows product is impressive, and should put many an APL vendor to shame.

Pursuing a commercial goal for J will present ISI with two new difficulties. On the one hand they will have to enhance their product to keep up with the changes to the Windows environment; for example, it is hard to see how Windows 4 could support MDI in its current incarnation. On the other hand, they will have to resist the strong temptation to continue to make backward-incompatible changes to the J language. Commercial users do not take kindly to new software releases that cannot run their existing applications.

I wish ISI every success with their excellent product.

(webpage generated: 3 October 2007, 00:02)
