---
vid: '10001980'
title: 'Utilities for Dyalog APL/W developers: UCMD Take 2'
authors:
- Ray Cannon
volume: '15'
issue: '4'
page: '109'
source: trad/v154/ray154.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1999
tags:
- development practice
- implementations
---

## Utilities for Dyalog APL/W Developers: UCMD Take 2

*by Ray Cannon*

I have recently reviewed how my original “pseudo UCMD” code (Ref.: *Vector* 15.1 and 15.3) worked, and have now simplified it resulting in version 2. The utilities are available as a zip file (See [Description](resource/ucmd.htm)).

### Recap of Version 1

The utility suite consisted of a directory (called UCMD) containing many workspaces, one for each utility and one extra workspace (called “SE.DWS”) which is run each time Dyalog is loaded, to set up the environment for the current session.

Each utility workspace contained at least one and sometimes two (or more) top-level functions with the same name as the workspace name. (*e.g.* workspace “FNS.DWS” might contain two top-level functions “Fns” and “FnS”.) Niladic utilities appeared on the menu bar, while monadic and dyadic utilities appeared in a drop down list from the tool bar.

### Version 2

<b>Modifications made to the environment</b>  
Enhanced function key support, including multiple alternative key settings, and PFKey editing

Improved menu bar layout and structure

Improved access to HELP.

<b>Modifications required for “Dyalog 8.2”</b>  
Under Dyalog 8.2, the simple Dyalog menu-bar and tool-bar have been replaced by cool-bars. “UCMD” Version 2 has been modified to include support for this new environment. (Thanks to Bob Hoekstra for all code changes relating to Dyalog 8.2.)

<b>Modifications made to the “UCMD” process.</b>  
Under version 1, all the control of which “UCMD” utilities were available, was hard coded within the session set-up workspace “SE.DWS”. So every time a utility was added, removed or renamed, “SE.DWS” also had to be modified.

Under version 2, “SE.DWS” has no in-built knowledge of what utilities are available. Instead, it checks each workspace in the “UCMD” directory for the existence of a standard function (“<u>UCMDSETUP</u>” see below) which, if found, it uses to build a data-base of the utility workspaces currently available.

For convenience, utilities are now (loosely) grouped together by “CLASS”.

Monadic and Dyadic utilities are still available from the tool-bar drop down combo, but niladic utilities are now accessed (by default) via a single drop down menu called “UCMD”. In addition, each “UCMD” workspace has its own help.

**Some of the new utilities that have been added**

- “Explore+” – a “pan workspace” namespace explorer. Edit, copy and compare functions, across workspaces and namespaces
- “Store” – stores the current object back into a workspace. Like the “)STORE wsid fn” on old STSC mainframe APL.
- “Memorise” – copies the contents of temporary storage (see old utilities “Put”, “ListGot” and “GetAll”) onto file.
- “Recall” – copies the Memorised data back into the active workspace.
- “Forget” – erases the Memorised file. (“Put” and “GetAll” only worked within a single Dyalog session. “Memorise” and “Recall” can be used between sessions.)
- “PutTree” – puts a copy of the current object and all functions in its calling tree into temporary storage (see old utilities “Put”, “ListGot” and “GetAll”).
- “EditPfk” – edits the current PFKeys settings
- “ChangePf” – switch between different sets of PFKeys settings.
- “Setpf” – sets up the PFKeys.
- “CompareDws” – compares two Dyalog workspaces held in DWS files. (Similar to “WsCompare”, which compared the currently loaded WS with a single DWS file.)
- “EditSe” – A “UCMD” set-up utility, allowing you to re-configure several aspects of the “SE”/”UCMD” environment.

## How to Install Version 2 of “UCMD”

If you already have version 1 installed, please remove or rename the “UCMD” directory before installing this new version. This new code assumes that you use a standard Dyalog session (such as “def\_uk.se”) which creates both a menu bar and a toolbar. It has not been tested on non-standard or altered sessions, and may require modifications before it will work with them.

You can download the file “UCMD.ZIP” from http://www.vector.org.uk

In your Dyalog WS directory create a new directory “UCMD” (for example “C:\\Dyalog73\\ws\\UCMD”) and unzip the contents of “UCMD.ZIP”.

Start up Dyalog and (optionally) add the new “UCMD” directory in the Dyalog workspace search path.

To make these utilities available your session just load the “SE.DWS” workspace. I always auto-load the SE workspace, and never save the modified session.

The “SE.DWS” workspace looks for the command line parameter “loadws=&lt;wsid>”, and if found will attempt to auto-load the named workspace after it has finished initialising itself.

### The “<code><u>UCMDSETUP</u></code>” function.

If you are considering adding your own favourite utility into the “UCMD” environment, you will need to save it as a workspace in the UCMD directory. In addition, the workspace must contain a “<code><u>UCMDSETUP</u></code>” function used by “SE.DWS” to set up the menu/tool bars. Look at a few examples from directory UCMD. (The following is taken from “FNS.DWS”):

<pre>
     ∇ {r}←<u>UCMDSETUP</u> menu;helpfid;helpno;name;text;fn1;hint;fn2;left;right;
        class;readme
[1]   ⍝Add command to menu and combo
[2]    name←'fns' ⍝Unique menu item name. (Menu order sorted by this value)
[3]    class←'LIST'
[4]
[5]
[6]    fn1←'FnS'     ⍝ Must be a function in the workspace or '' if N/A
[7]    text←'&amp;Fns'   ⍝ Text that appears in the menu       or '' if N/A
[8]    hint←'List fns in current namespace.' ⍝ Hint text   or '' if N/A
[9]
[10]   fn2←'Fns'                 ⍝ Must be a function in the workspace or ''
[11]   right←'Criteria eg foo*'  ⍝ Description of Right hand argument
[12]   left←'Optional Namespace' ⍝ Descr of Left hand argument or '' if MONADIC
[13]
[14]   helpfid←'ucmd.hlp' ⍝ Help file name or '' if None Available
[15]   helpno←'700'       ⍝ Help context number or '' if None Available
[16]   readme←⊂'FNS.DWS --  Lists some/all of the Fs in the namespace (plus)'
[17]   readme,←⊂''
[18]   readme,←⊂'The menu command FnS lists all the functions (and operators)'
[19]   readme,←⊂'in the current namespace.'
[20]   readme,←⊂''
[21]   readme,←⊂'The combo command Fns lists only thoses function that match '
[22]   readme,←⊂'the entered text (with "*" acting as a "wild card").'
[23]   readme,←⊂''
[24]   readme,←⊂'If an optional namespace is supplied, the resulting listing'
[25]   readme,←⊂'include the full namespace path (for ease of editing).'
[26]   readme←↑readme
[27]   :If 0≠⍴menu
[28]       r←menu RunSetup name class fn1 text hint fn2 right left helpfid helpno
[29]   :Else
[30]       r←hint readme class
[31]   :EndIf
     ∇
</pre>

## Notes

- “class” groups related utilities. e.g. “Put” and “GetAll”, “Fns” and “Vars”;
- “name” must be unique.
- “fn1” if supplied, MUST be the name of a niladic function in the workspace with the same name (ignoring case). It should be empty if you do not want the item to appear on the “UCMD” menu;
- “text” is the text that will appear in the menu. It may contain spaces. Indicate the accelerator key (if required) via an ampersand. May only be empty if “fn1” is also empty;
- “fn2”, “right” and “left” relate to monadic and dyadic utilities;
- “fn2” may be empty if you do not want the item to appear in the drop down combo. If supplied it MUST be a monadic (or greater) function in the workspace with the same name (ignoring case);
- “right” contains the text that will be displayed when assigning the right hand argument. May only be empty if “fn2” is also empty;
- “left” must either contain the text that will be displayed when assigning the left-hand argument or must be empty if “fn2” is monadic or empty.
- “readme” defines the text used to create simple “HELP” messages.

The utilities are available as a [zip](resource/ucmd.zip) file (600K).

***
