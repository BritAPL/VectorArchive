---
vid: '10003290'
title: A GDDM Simulation for APL*PLUS II
authors:
- Allan Gay
byline: by Allan Gay (Cocking & Drury)
volume: '10'
issue: '2'
page: '133'
source: trad/v102/gay102_133.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1993
tags:
- user interfaces
- system interfaces
- development practice
---

by Allan Gay <span class="affil">(Cocking &amp; Drury)</span> <span class="email"></span>
{ .byline }

### Introduction

This article describes the GDDM simulation developed during the migration of some APL2/TSO mainframe applications to APL\*PLUS II/386 on 486 machines.

As currently configured, the simulation can run complex character-based dialogue screens for data capture and display, and make them look very nearly the same as they would on a mainframe colour terminal.

Converting migrated mainframe code to use the simulation is straightforward.

Of the mainframe repertoire, the simulation provides 42 GDDM operations (see \[1\]) and 2 AP126 operations (see \[2\]). This may not seem much but it gives us an extensive range of character-based operations, including support for multiple GDDM pages and page-switching. Among the omissions are support for multiple pairs of shared variables and all the graphics.

The simulation’s simple architecture makes it easy to bolt on more simulated GDDM operations, although the difficulty of actually simulating a given operation necessarily depends upon the nature of the operation concerned.

### Interfacing the Code

The simulation has been geared to minimise the disruption entailed in converting mainframe code. An executor function named GDEXEC replaces AP126. Its argument and result variables assume the roles of AP126’s shared variables. Input values are the same as for AP126 and, with some minor exceptions, output values are too.

GDEXEC’s optional left argument receives any character data which would be assigned to AP126’s DAT shared variable. GDEXEC’s right argument receives the integer data which would be assigned to AP126’s CTL shared variable.

The result from GDEXEC is a 2-item vector. The first item contains the simulated AP126 CTL response, and the second item contains the simulated DAT response. Results are mostly identical to their mainframe equivalents. In the case of some of the more recondite GDDM calls (e.g. FSQSYS), contents may diverge owing to platform differences, but result formats are still preserved.

### Coding a Simulated GDDM Call

Here’s a sample mainframe ASCGET to obtain the content of a specified field:

```
  CTLs←422,fldno,fldlen            ⍝ Issue ASCGET
  discard←CHECK CTLs               ⍝ Check retcodes
  chars←DATs                       ⍝ Get field's content
```

And here’s the simulated equivalent:

```
 (nums chars)←GDEXEC 422,fldno,fldlen    ⍝ Do ASCGET & get rslt
 discard←CHECK nums                      ⍝ Check retcodes.
```

Because the ASCGET operation requires no character input, AP126’s DAT variable would not be assigned, so no left argument was supplied to GDEXEC.

Now here’s a sample mainframe ASCPUT to write some text into a field. Because we are sending data to GDDM rather than receiving it, our subsequent processing is rather more perfunctory that in the previous example:

```
 DATs←text                       ⍝ Post the field text
 CTLs←424,fldno,⍴text            ⍝ Issue ASCPUT for the field
 discard←CHECK CTLs              ⍝ Check the retcodes.
```

And here’s the same thing, using the simulation:

```
 discard←CHECK 1⊃ text GDEXEC 424,fldno,⍴text
```

### Batching Simulated GDDM calls

Both mainframe AP126 and the simulation allow you to batch multiple calls into a single exchange. This improves performance because the shared variable overhead is minimised. The calls’ inputs are catenated together and assigned to the shared variables in one shot. The results are received en masse and separated afterwards. Each result has its own GDDM returncodes.

Batching is not difficult to simulate. In the inputs to the individual GDDM operations, data lengths always either are supplied as parameters or are implied by the nature of the operations themselves. Consequently, like AP126, the GDEXEC function is able to unbatch each operation’s inputs, invoke the relevant simulated operation, and batch up the results.

### Easy Conversion

In our migration project, we found that we could convert a migrated mainframe workspace with just a couple of hours’ prosaic hoovering of the shared-variable uses. Typical applications operated between twenty and thirty display panels, most of which would be stashed in separate GDDM pages for speedy recall via FSPSEL.

The project ran for roughly nine months, during which time the simulation repertoire was extended and its behaviour enhanced several times, but the actual conversion of each application’s GDDM code took no more time than this.

### Architecture

The simulation comprises three layers of APL functions. The topmost layer consists of the three public functions: GDEXEC, GDinit and GDterm. The second layer consists of a validator function and an enactor function for each simulated GDDM operation. The third layer consists of utility functions. All the functions in the second and third layers are private.

Second-layer functions all bear a name commencing “GDv” (the validators) or “GDo” (the operations themselves). The trailing characters of the name are the numerical GDDM opcode. Thus, GDo101 is the ASREAD operation (GDDM opcode 101) and GDv424 is the operands validator for ASCPUT (opcode 424). This naming scheme enables GDEXEC to build the names of pre-existing functions dynamically, using opcodes from incoming parameter data. The functions are invoked via the Execute primitive.

AP126 operations, distinguished from GDDM operations by their negative opcodes, are dynamically renumbered into a 9xxx series. Thus incoming opcode ¯8 (“Query Modified Subset of Fields”) is converted by GDEXEC to pseudo-opcode 9008, from which a function name may safely be formed.

Third layer functions bear 6-character names commencing “GD” and completed with a 4-character acronym intended to denote the purpose. For example, GDhues computes equivalent PC video attributes for the rows of a 17-column mainframe format matrix.

These very restrictive naming conventions facilitate code management and reduce the likelihood of name clashes against migrating applications. It would have been nice to have used the GDDM operation names (ASREAD, FSQURY, etc.) to name the functions, but the authors of the code we were migrating had already pinched that idea some decades earlier.

### Repertoire

In addition to AP126’s “Query GDDM Calls” and “Query Modified Subset of Fields” operations, the following GDDM calls are currently offered by the simulation:

```
ASCCOL     Specify Character Colours Within a Field
ASCGET     Get Field Contents
ASCHLT     Specify Character Highlights Within a Field
ASCPUT     Specify Field Contents
ASDFLD     Define a Single Field
ASDFLT     Set Default Field Attributes
ASDFMT     Define Alphanumeric Fields, Deleting All Existing Fields
ASFCLR     Clear Fields
ASFCOL     Define Field Colour
ASFCUR     Position the Cursor
ASFEND     Define Field End Attribute
ASFHLT     Define Field Highlighting
ASFIND     Define Input Null-To-Blank Conversion
ASFINT     Define Field Intensity
ASFMOD     Change Field Status
ASFTYP     Define Field Type
ASQCOL     Query Character Colours for a Field
ASQCUR     Query Cursor Position
ASQFLD     Query Field Attributes
ASQHLT     Query Character Highlights for a Field
ASQMAX     Query the Number of Fields
ASQMOD     Query Modified Fields
ASRATT     Define Field Attributes
ASREAD     Device Output/Input
ASRFMT     Define Multiple Fields without Deleting Existing Fields
FSALRM     Sound the Alarm
FSCOPY     Send Page to Alternate Device
FSFRCE     Update the Display
FSPCLR     Clear the Current Page
FSPCRT     Create a Page
FSPDEL     Delete a Page
FSPQRY     Query Specified Page
FSPSEL     Select a Page
FSQCPG     Query Current Page Identifier
FSQDEV     Query Device Characteristics
FSQERR     Query Last Error
FSQUPG     Query Unique Page Identifier
FSQURY     Query Device Characteristics
FSQSYS     Query Systems Environment
FSQWIN     Query Page Window
PTSQRY     Query Partition Set Attributes
PTSSEL     Select a Partition Set
```

The two partition set operations are dummies – created only because they were used by the applications we were migrating. They assume that there is a single extant partition-set and that its id is zero.

### Data Structures

When the simulation is active, working global variables hold the simulated GDDM pages. Before GDEXEC can be used, the GDinit function must be run to start these global variables. At application shutdown when all GDDM simulation activity has concluded, the GDterm function may be used to revert the display to its pre-application state and to expunge the working global variables. GDinit and GDterm are thus in some degree analogous to `⎕SVO` and `⎕SVR`. They are niladic functions.

The working global variables are vectors, most of which are nested. Each element of a given vector contains data for one aspect of one extant GDDM page. Elements are added when new pages are FSPCRT’d. To save space, elements are nullified when their corresponding pages are FSPDEL’d. This is achieved by assigning an empty vector to such elements.

An extant page comprises: an alarm boolean, a rank-3 `⎕WPUT` screen-image, a cursor position vector, a GDDM format matrix, an equivalent `⎕WIN` matrix, a field ids vector, a boolean vector which flags modified fields, a default field-attributes vector, and a unique page id-number.

The simulated GDDM operations act upon these elements. The changes become visible when the ASREAD operation (opcode 101) is invoked to update the display and to capture keyboard input. For entertainment, a listing of the GDo101 function is appended to this article.

### Tuning

Simulation behaviour may be tuned by altering the values of certain manifest constants which are held permanently as global variables.

Tunable features include numeric checking (which may be set on or off to match your mainframe terminal), high-minus conversion (to hyphen if desired), and default video attribute (all unmapped screen areas are defaulted to low green on low black, but this can be changed).

### The Keyboard

Mainframe layout has been retained as far as possible. It is supplemented by the APL\*PLUS II/386 repertoire documented in the discussion of `⎕WKEY` in \[3\].

As on the mainframe, the cursor movement keys do not constrain the cursor to remain only in the defined fields. This apparent infringement of the APL\*PLUS II/386 ŒW`IN e`nvelope is managed by treating these keys as exit keys, handing control to a utility function which operates the entire screen as one field. When the user steers the cursor back into a mapped field, the utility function cedes control and normal service resumes. On a 486 machine, we can get away with this sort of overhead, and my 25MHz 80386DX machine copes quite well, too. When teaching the camel to play the violin, some expenditure is necessarily incurred.

Tabkey behaviour is not identical to mainframe tabkey behaviour. When a tabkey is struck, the cursor moves to the start of the next geographical field. (A mainframe tabkey moves the cursor to the next geographical field row.)

Output-only fields, used for selection by cursor position when a PFkey is struck, are declared as “protected alphanumeric; immediate pen-selectable” by coding fieldtype 3 in the GDDM format matrix. When the corresponding `⎕WIN` format matrix is generated, bogus fieldtype 4 “PFkey selectable”) is assigned. The GDo101 function includes special logic to handle fields which have this fieldtype.

### Video Attributes

Some compromise was entailed in reconciling the dissimilar video attributes of the mainframe terminal and the PC. The colours differ in hue of course, but APL\*PLUS II/386’s SETPALETTE function offers some scope for tuning PC colours. Except in the case of reverse-video fields, the simulation uses low-intensity black as the background colour; this is a tunable option.

The GDDM format matrix permits the specification of four attributes which influence the appearance of a field. They are Type, Intensity, Colour, and Highlight. Of these, Type influences only the default colour which is assigned when the Colour attribute is omitted.

The Intensity attribute may specify invisible, low, or high. Invisible is implemented as low-intensity black foreground symbols on a low-intensity black background. The other two intensities are implemented as foreground intensities for the colour concerned.

The two platforms have different Colour repertoires, but no insuperable difficulty presented itself. Mainframe pink became magenta, mainframe turquoise became cyan, and mainframe “default” became black.

The Highlight attribute gave the most trouble. The mainframe provides low, blink, reverse, and underscore. APL\*PLUS II/386 does not support underscore on the PC colour monitor, so this attribute is simply ignored where coded. Blink is an alternative to background intensity, and was selected via `1 ⎕POKE 902`. Reverse was implemented by exchanging the foreground and background colour numbers.

### Size

Copying the simulation workspace into an application workspace reduces `⎕WA` by 213KB. Across three applications operating a total of 80 GDDM pages, the additional space requirement per extant GDDM page averaged a little over 9KB.

### Conclusion

There was a time, a few years ago, when you couldn’t write a line of code for a full screen interface at Cocking & Drury Ltd without fifteen APLers interrupting to argue the toss over every line. Finishing anything was hell.

These days, all the action seems to be in GUIs. Freed of interruptions, I was able to do it up brown. But is there anybody still out there?

Next: the VSAM simulation.

### References

1. Graphical Data Display Manager Base: Programming Reference, IBM, SC33-0101
2. VSAPL for TSO: Terminal User’s Guide, IBM, SH20-9180
3. APL\*PLUS II/386 Reference Manual, Manugistics Inc.

### Appendix: the ASREAD function

The following example requires APL\*PLUS II/386 Release 4.0 or higher, operating in Evolution Level 2 mode.

```
∆Z←∆A GDo101 ∆W;∆C;∆F;∆G;∆M;∆N;∆p;∆P;∆Q;∆R;∆X;⎕IO
⍝ GDDM simulation - opcode 101 - ASREAD service - PRIVATE
⍝  ⍺ ⍵  are ignored.
⍝  ←    is 2-item vector:
⍝        1⊃  is 7-integer vector:
⍝             [1-2] are retcode
⍝             [3-4] are zero, denoting no results from this operation
⍝             [5 6] are exitkey-class and -type
⍝             [7]   is count of modified fields.
⍝        2⊃  is empty text vector.
 ⎕IO←1
                                        ⍝  PRELIMINARIES:
 ∆F←gdpage⊂gdpgff                       ⍝   Get format-matrix.
 ∆F[(∆F[;5]=2)/⍳1↑⍴∆F;5]←1+KKgdnf       ⍝   Numfld checking enable/disable
 ∆F←∆F[∆G←⍋∆F[;1 2];]                   ⍝   Sort it geographically.
 ∆N←gdpage⊂gdpgfn                       ⍝   Get field-ids.
 ∆N←∆N[∆G]                              ⍝   Sort them geographically.
 ∆X←∆F[;5]≠0                            ⍝   Note keyable fields.
 ∆M←(1↑⍴∆F)⍴0                           ⍝   Start off fld-modified flags.
 ∆C←GDcurw                              ⍝   Get useable curpos for ⎕WIN.
 ∆P←∊∆p←(⊂KKgdsz) GDmapf¨⊂[2]∆X⌿∆F      ⍝   Get maps of field positions.
                                        ⍝  DIALOGUE PREAMBLE:
 ⎕WPUT gdpage⊂gdpgsi                    ⍝   Display updated screen.
 8 GDstat 8⍴' '                         ⍝   Turn off 'X SYSTEM'.
 →(~gdpgal[gdpage])/DDD                 ⍝   If alarm request posted,
 ⎕ARBOUT 5⍴¯1+⎕AV⍳⎕TCBEL                ⍝    honk the hooter and
 gdpgal[gdpage]←0                       ⍝    unpost the request.
DDD:                                    ⍝  CONDUCT FULL DIALOGUE:
 →(~1∊∆X)/FFF                           ⍝   If keyable fields are extant
 →(0=∆C[1])/FFF                         ⍝    and cursor is in a field,
 ∆R←∆C ⎕WIN ∆X⌿∆F                       ⍝    conduct full dialogue,
 ∆M←∆M∨∆X\5↓∆R                          ⍝    note any modified fields,
 ∆C←∆R[3 4 5]                           ⍝    & note the cursor position.
 →HHH
FFF:                                    ⍝  AWAIT EXITKEY WITHOUT INPUT:
 ∆R←(1,1↓∆C) ⎕WIN 0 0 24 80 32          ⍝   Just wait for an exit key.
 ∆C←0,∆R[4 5]                           ⍝   Note curpos within page.
HHH:                                    ⍝  HANDLE ANY KEYBOARD-TOGGLE:
 →(∆R[2]≠KKgdkb)/JJJ                    ⍝   If keyboard-toggle hit -
 ⎕SEG←⍬ ⋄ ∆Q←(~0⌈1⌊⎕PEEK 118)⎕POKE 118  ⍝   .  toggle keyboard state
 GDstat ''                              ⍝   .  update status line.
 →DDD
JJJ:                                    ⍝  HANDLE CURSOR ↑↓←→ KEYS:
 →(~∆R[2]∊393 420 391 389)/SSS          ⍝   If cursor ↑↓←→ struck -
 ∆C←(∆R ∆P ∆p) GDmvcr ∆X⌿∆F             ⍝   .  run special handler
 GDstat ''                              ⍝   .  update status line.
 →DDD
SSS:                                    ⍝  HANDLE ANY INSERT-TOGGLE:
 →(∆R[2]≠KKgdik)/VVV                    ⍝   If Insert-key hit -
 ⎕SEG←⍬ ⋄∆Q←4 2 4 2[0 1 2 4⍳⎕PEEK 312]  ⍝    make new poke value to
 ⎕SEG←⍬ ⋄ ∆Q←∆Q ⎕POKE 312               ⍝    toggle insert/replace mode
 GDstat ''                              ⍝    & update the status line.
 →DDD
VVV:                                    ⍝  DIALOGUE EPILOGUE:
 ⎕SEG←⍬ ⋄ ∆Q←2 ⎕POKE 312                ⍝   Turn off insert/replace mode.
 8 GDstat 'X SYSTEM'                    ⍝   Turn 'X SYSTEM' back on.
 gdpgsi[gdpage]←⊂⎕WGET 3                ⍝   Store screen image.
 →(0=∆C[1])/XXX                         ⍝   Convert cursorfld index to
 ∆C[1]←∆C[1]⊂∆X/∆N                      ⍝    fldid (if not page-coords).
XXX: gdpgcu[gdpage]←⊂∆C                 ⍝   Store GDDM curpos, in
 gdpgcu[gdpage]←⊂GDcurs 1               ⍝    fld-coords if possible.
 gdpgmf[gdpage]←⊂∆M[∆G⍳⍳⍴∆G]            ⍝   Deorder & store modflds bits.
 ∆Z←0 0 3 0                             ⍝   Set retcodes and count.
 ∆Z←∆Z,KKgdkn[⍬⍴KKgdex⍳∆R[2];]          ⍝   Set GDDM exitkey-class & -no.
 ∆Z←∆Z,+/∆M                             ⍝   Set count of modified fields.
 ∆Z←(⊂∆Z),⊂''                           ⍝   Wrap up result.
```

(webpage generated: 7 December 2005, 01:33)
