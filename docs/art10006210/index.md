---
vid: '10006210'
title: J Inscription
authors:
- Richard Oates
byline: by Richard Oates
volume: '11'
issue: '3'
page: '130'
source: trad/v113/oates113_130.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- development practice
- programming techniques
---

by Richard Oates
{ .byline }

Release 1 version 7 of J introduced an unobtrusive keyboard process for explicit definition. It is started with a zero left argument. I call it inscription and do it with the adverb train IN:=. 0 : . Inscription can be scripted. It is much more convenient than the prior definition technique. It has validated script and made the saved workspace obsolete. A script is a file where every line is a J sentence. To script a file is to read it as a keyboard, like Unix standard input.

I describe a J utility which edits DOS script. I like DOS more than Windows but less than Nextstep. Forty tacit verbs are defined in four explicit verbs. One tacit equivalent is fixed for each explicit verb. The utility runs from the explicit verbs or the tacit equivalents. It uses the adverse and agenda control conjunctions.

### Introduction

My profile scripts eight adverb trains and three verbs. A conjunction and either of its arguments is a train which makes an adverb waiting for the other argument. In J Release 2 some arguments of the foreign !: conjunction are changed.

```
    h:=. 0!: NB. Host       IN:=. 0 :      NB. Inscribe
    f:=. 1!: NB. File       v:=. "_        NB. Verb from noun
    n:=. 4!: NB. Name
    m:=. 5!: NB. Map        C=. (13{a.)v:  NB. Carriage return
    s:=. 8!: NB. Screen     L=. (10{a.)v:  NB. Line feed
    d:=. 9!: NB. Dial       e:=. (i.0 0)v: NB. Empty
```

The J editor 9 s: acts on line text but not on verbs. A line text is a vector where each line ends with a line feed. Before version 7 an argument of explicit definition could be a sentence in quotes, a list of boxed sentences, or the open thereof. Boxed sentences are clumsy. In an early version of the language I wrote a utility that used 9 s: to edit a verb in the workspace. I then saved workspaces and ignored script. It worked like APL del so I called it Nd for “Not del”.

Inscription suspends execution while the user enters J sentences one after another. Execution resumes on entry of a bare parenthesis. The new verb is in the workspace. The process is serial but the sequence can appear in script that can be edited freely before it is scripted into J. If a complete locale is defined in script, the whole can be scripted at the start of the session. A locale is a workspace or part of one. See “Version 7” below. Saved workspaces are no longer needed and support will be withdrawn. When version 7 appeared I changed Nd. It now applies 9 s: to text from a DOS script.

### Boxed Sentences vs Inscription

Brute force at the keyboard or in script is required to manage a list of boxed sentences.

```
   a=. 'Cut=. <;._2'
   b=. 'Go=. >@Cut'
   c=. ' Go y.'
   (Table=. (a;b;c) : '')   NB. Outer paren's display the verb
┌───────────┬─┬┐
│Cut=. <;._2│:││
│Go=. >@Cut │ ││
│ Go y.     │ ││
└───────────┴─┴┘
   Table'cup box '
cup
box
```

Inscription suspends the indented prompt and sentence execution. It has a double begin/end structure. A bare colon separates the monad from the dyad. A bare right parenthesis terminates the process. Explicit and tacit verbs and adverbs and conjunctions can be inscribed. Explicit verbs are selected by a zero right argument to definition: 0 : 0 . This inscription defines the same verb.

```
   Table=. 0 IN:
Cut=. <;._2
Go=. >@Cut
 Go y.
:
)
```

Table can be “edited” by bringing each sentence from the output to the input area with Ctrl/Enter. If the verb is longer than this or will be needed on another day it is defined in script.

In addition, line text can be inscribed with 4 IN: . If each line is a J sentence the line text can be used in a subsequent definition, Jtext : Y . I do not discuss this. Most tables are easier to inscribe than to write in script with primitives like append or shape. I inscribe them in line text and cut and open each text on the end-of-line indicator to produce a J noun, as described below in the verb Ndn.

### End-of-Line Indicators

Carriage return and line feed appear at the end of every line in DOS script. Line feed appears at the end of every line in line text. These indicators shatter boxed maps.

```
   (65{a.)v:         (13{a.)v:         (L=. (10{a.)v:)
 ┌─┬─┬─┐           ┌─┬─┬─┐           ┌─┬─┬─┐
 │A│"│_│           │"│_│             │
 └─┴─┴─┘           └─┴─┴─┘           │"│_│
                                     └─┴─┴─┘
```

I convert them to verbs with the constant conjunction in v: . Constant noun"noun is not as well known as rank verb"noun . Constant makes a verb that ignores its arguments and returns the noun on its left as its result. The verb 2: works the same way.

```
   's'2: 3      65{a.     's'((65{a.)v:)3      a.i. 's'(L=.(10{a.)v:)3
 2            A         A                    10
```

In Nd the hook CL sticks carriage return and line feed on the end of every line in a table.

```
   y.      a.i.y.     a.i.CL y.     (CL=. ,"1 (C , L))    CL y
 e       101        101 13 10     ┌───────┬───────┐       ,"1
 v       118        118 13 10     │┌─┬─┬─┐│┌─┬─┬─┐│      y   \
 e       101        101 13 10     ││,│"│1│││C│,│L││           ,
 r       114        114 13 10     │└─┴─┴─┘│└─┴─┴─┘│          / \
 y       121        121 13 10     └───────┴───────┘         C   L
                                                            y   y
```

### Cup Utility

The Nd utility is mapped with Cup. I introduce Cup with Table and a tacit verb. In Case the tie ‘ conjunction forms a gerund from verb arguments. A gerund is a list of boxed noun atomic representations. The right argument of the agenda @. conjunction selects one of the three cases for execution.

```
   ( Case=. ]`}:`(>@(<;._2))@.((2: <. ])@(L +/ .= ,)) )
┌──────────────────────┬──┬───────────────────────────────┐
│┌─┬──┬───────────────┐│@.│┌─────────┬─┬─────────────────┐│
││]│}:│┌─┬─┬─────────┐││  ││┌──┬──┬─┐│@│┌─┬───────────┬─┐││
││ │  ││>│@│┌─┬──┬──┐│││  │││2:│<.│]││ ││L│┌─────┬─┬─┐│,│││
││ │  ││ │ ││<│;.│_2││││  ││└──┴──┴─┘│ ││ ││┌─┬─┐│.│=││ │││
││ │  ││ │ │└─┴──┴──┘│││  ││         │ ││ │││+│/││ │ ││ │││
││ │  │└─┴─┴─────────┘││  ││         │ ││ ││└─┴─┘│ │ ││ │││
│└─┴──┴───────────────┘│  ││         │ ││ │└─────┴─┴─┘│ │││
│                      │  ││         │ │└─┴───────────┴─┘││
│                      │  │└─────────┴─┴─────────────────┘│
└──────────────────────┴──┴───────────────────────────────┘
   $Case 99 98 97 10 99 10{a.
2 3
```

The Cup utility maps verbs with lines instead of boxes. A gerund is not a verb so the boxes are retained. Cupped maps are less precise than boxed maps but snug ambiguous display is not foreign to J or apl.

```
   Cup'Table    Case'
?───────────:─??──────────────@.────────────?
?Cut=. <;._2  ??┌─┬──┬───────┐  ─────@──────?
?Go=. >@Cut   ??│]│}:│>@─────│  2:<.] L────,?
? Go y.       ??│ │  │  <;._2│         ──.= ?
└─────────────┘?└─┴──┴───────┘         +/   ?
               └────────────────────────────┘
```

### The Nd Program

There are four explicit verbs but no branches or labels. Explicit reference is confined to the last sentence in each definition. The tacit verbs appear in bottom-up order but can appear in any order. I expect to find local tacit verbs useful even within a named locale. The tacit equivalents for monadic Nd, Ndp, Nds and Ndn are fixed in Exhibit B.

```
Monadic     Nd     Edits any script
     Ndp     Defaults full DOS name for Nd
     Nds     Scripts the verb or the line text noun
     Ndn     Makes a J noun from line text
Dyadic     Nd     Copies a script that inscribes one object
```

### Nd Verb

Four steps make Nd work like APL del:

```
     1     f:  File Read inputs a C/L vector
     9     s:  J Edit takes and makes a line text
     2     f:  File Write outputs a C/L vector
     3     h:  Silent Script inputs a verb or a line text noun
```

Monadic Nd reads the file named in its argument, purges carriage returns, hands line text to the J editor, restores carriage returns, rewrites the file, and scripts it. “Silent” kills the echo of the “keyboard” on the screen. If the file, say Voice , does not exist the adverse :: conjunction in Read places Voice=: 0 IN: at the top of a fresh screen. If Voice is to be a noun, not a verb, change 0 to 4. If you misspell the name and get a fresh screen when you were expecting a definition, erase the top line to kill an unwanted script. Nd does not fail easily:

```
    Nd';Voice'
 Name? ;Voice
```

Dyadic Nd copies a file to a new DOS name. It changes the name on the top line to match the DOS name. For example, ’Fax’Nd’Voice’ changes Voice=: 0 IN: to Fax=: 0 IN: . Monadic Nd assumes the name at the top of a script matches the DOS name.

```
   Cup'Nd'
?──────────────────────────:──────────────────────?
?NB. Nd'name'      Not Del  NB.'new'Nd'old'   Copy?
?                                                 ?
?CL=. ,"1 (C , L)           Write=. ] 2 f: <@;@[  ?
?Tag=. ;@(CL&.>@(<;._2))                          ?
?EOL=. _2&}.@Tag@(] , L)    Jname=. }.@;@{:@}:@{. ?
?Out=. EOL 2 f: <@;@[       From=. ] +./\ .= '='v:?
?Write=. Out`]@.(0: = #@])  With=. [ , From # ]   ?
?                           Old=. 1 f:@<@;@{:     ?
?View=. 9 s:                New=. Jname With Old  ?
?                                                 ?
?Old=. 1 f:@<@; -. C        Copy=. {. Write New   ?
?Jname=. }.@;@{:@}:         Run=. Nds@{. [ Copy   ?
?New=. Jname , '=: 0 IN:'v: Go=. Run@(Ndp"0@])    ?
?Read=. Old ::New           No=. ('Name?'v: ; ])v:?
?                            Go ::(>@No) x.;y.    ?
?Edit=. ] Write View@Read                         ?
?Go=. (Nds [ Edit)@Ndp                            ?
?No=. 'Name? 'v: , ":
?
? Go ::No y.                                      ?
└─────────────────────────────────────────────────┘
```

Ndp Verb Ndp extends the argument of Nd with three defaults which complete the DOS file name. Each part of the name is boxed. Ndp is immune to the length of the first default. Its argument can override the second and third. Ndp"0 appears in dyadic Nd. Ndp is the only verb that needs to be adapted to a different operating system. Ndp ’Table’  
Ú–––Â––Â––––––Â–––?  
?\\J7?\\I?\\Table?.Js?  
À–––Á––Á––––––Á–––”  
N=. ’W\\Table.Bak’;’W\\Much\\Deeper\\Table.’  
Ndp"0 N  
Ú–––Â––Â––––––Â–––––––Â––––––Â–?  
?\\J7?\\W?\\Table?.Bak ? ? ?  
Ã–––Å––Å––––––Å–––––––Å––––––Å–?  
?\\J7?\\W?\\Much ?\\Deeper?\\Table?.?  
À–––Á––Á––––––Á–––––––Á––––––Á–”  
;@Ndp"0 N NB. DOS names  
\\J7\\W\\Table.Bak  
\\J7\\W\\Much\\Deeper\\Table.  
\}.@;@\{:@\}:@Ndp"0 N NB. J names  
Table  
Table  
$Ndp’;Voice’  
0  
Cup’Ndp’  
?––––––––––––––––––––––––––––––––:–?  
?NB. Ndp’n’ Ndp’d\\n.e’ DOS path ?  
? ?  
?Class=. -@1: &lt; 0 n:@&lt;@\}.@;@\{:@\}: ?  
?Cut=. e.&’\\.’ &lt;;.1 \] ?  
?Hit=. +./@(\[ =/ ’ \\ .’v:) ?  
?Cull=. ;@(-.@Hit # \]) ?  
?Dos=. (#~ Class)@Cut@Cull ?  
? ?  
?Root=. ’\\J7\\’v: ?  
?Dir=. ’I\\’v: ?  
?Ext=. ’.Js’v: ?  
?Default=. Root ; Dir ; \] ; Ext ?  
? ?  
? (Dos Default)@(>@\])y. ?  
À––––––––––––––––––––––––––––––––––”

### Nds, Line Text and Ndn

Nds scripts the verb or the line text noun. It executes Ndn when the inscription defines a noun. Noun cannot be a tacit verb because local names in Nds would mask global names for the name class 0 n: verb.

Ndn cuts and opens a line text to make a J table. Ndn is also applied in my profile to each noun after all have been scripted, and it appears in the script of any noun that is not an open table, as seen in Df . Reform cannot be a tacit verb because a tacit copula =: does not act on nouns.

```
   Cup'Nds                  Ndn'
?───────────────────────:─??───────────────────────────────:─?
?NB. Nds Ndp'n'   Script  ??NB. Ndn'n'  Noun from line text  ?
?Script=. 3 h:@<@;@[      ??Reform=. '' : '(<x.)=: <y.'      ?
?Noun=. '' : '2=0 n:<y.'  ??Shape=. (2: <. ])@(L +/ .= ,)    ?
?Form=. e:`(Ndn@])@.Noun  ??Up=. ]`}:`(>@(<;._2))@.Shape@".  ?
?Jname=. }.@;@{:@}:       ?? e:@(] Reform Up)y.              ?
? (Script Form Jname)y.   ?└─────────────────────────────────┘
└─────────────────────────┘

   2 h:<;Ndp'Df' NB.Script          1 f:<;Ndp'Df' NB.Read file
   Df=: 4 IN:                    Df=: 4 IN:
19940128                         19940128
\J7\W\Manu.Js                    \J7\W\Manu.Js
)                                )
   Ndn'Df'                       Ndn'Df'
   Df=: (".@{. ; {:)Df           Df=: (".@{. ; {:)Df
   Df
┌────────┬─────────────┐
│19940128│\J7\W\Manu.Js│
└────────┴─────────────┘
   Go=.>@Cut=. <;._2
NB. Tacit copula
   Cup'Go Cut'
?>@─────??<;._2?
?  <;._2?└─────┘
└───────┘
```

### Version 7

In addition to validating script, Version 7 introduces an error stack, suspended execution, and named locales. Suspension permits sentence execution in the local environment, and resumption. Named locales are alternate symbol tables. I have not used them yet, but I did put all utility scripts in a utility directory and the scripts for each application in a directory for that application. I expect each directory will become a named locale. My profile scripts a directory verb, runs it to get the names in the directory, scripts Ndn, scripts the other objects, and moves to the next directory. Taken together, these changes make 7 the first version of J that can be used outside the classroom.

I would like some additions. A foreign conjunction that edits script with the J editor and scripts the verb. An inscription which makes an open character noun of rank 2 or less; I do not inscribe numeric tables. Deletion of trailing blanks from each line of an inscription. A foreign conjunction that scripts a whole directory into a named locale; I had no conflicts with caseblind DOS names when I converted the objects in each version 6 workspace to script but this quirk of DOS needs to be outwitted. Fix f. as a conjunction instead of an adverb; sentences like the ones in Exhibit B would be simplified by an additional verb that could suspend name replacement.

### Conclusion

Tacit definition simplifies documentation. After the arguments of a tacit verb are described it’s just J all the way. Further is better. Further enlarges the space where unexplained data cannot lurk. The name of a tacit verb is more potent than a comment – it appears more than once. A comment to the right of the definition can provide an additional hint, but verbs like CL cannot be fully described without turning the program into a haystack. Tacit programs, like J explicit and APL programs, are best read actively at an open keyboard.

A workspace must be cleaned before you save 2!:2 or )SAVE it. A locale made from script is never saved. A directory does not get as dirty as a workspace. To clean it sort on the time”stamp and check the scripts at the bottom. Other system support is available like selective backup and string search and replacement through all script in a directory. In APL2 I wrote programs for jobs like these.

When del appears in an APL session the log goes to lunch and all hell breaks out. After lunch you can display the function but you don’t. It’s just “paper” you can’t use. Inscription fills the black hole of del. Session and definition – any definition – are one.

Unlike APL del, explicit definition permits independent specification of the monad and the dyad. Is 0 : 0 the simplest possible way to define a verb?

### ?Exhibit A: Booting

(webpage generated: 29 October 2007, 15:59)
