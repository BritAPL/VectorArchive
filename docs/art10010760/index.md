---
vid: '10010760'
title: 'Review: APL*PLUS/PC Version 11'
authors:
- Adam Weber
byline: by Adam Weber
volume: '11'
issue: '1'
page: '61'
source: trad/v111/weber111_61.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- reviews
- implementations
---

by Adam Weber
{ .byline }

Brilliant! At last an APL\*PLUS/PC interpreter with a utility (and licence) to produce (and sell) executable files from workspace files. But why the need to produce EXE files? Basically it allows anyone to produce an application which they can then sell at a reasonable price without the need to supply the interpreter to go with it.

### Installation

Installation was NOT easy on a 386SX. I was upgrading from V 5 and V 11 simply wouldn’t install. Thankfully the installation manual came with manual installation instructions which told me (though vaguely) what I had to do!!! On a 486DX, installation was perfectly straightforward.

V 11 can be used windowed in Windows 386 enhanced mode or full screen in any other mode. It comes supplied with icons and PIF files.

### The Interpreter

The interpreter actually comes in 3 versions:

APLNOG – Standard APL with no graphics APL – Standard APL with ŒG style graphics. APLX – Extended APL with ŒG style graphics.

The extended system provides a more user friendly interface using a session manager. This allows easy interface with hardware such as keyboard, printer and display. It also allows you to edit functions and data in separate, simultaneous sessions. However, for the user friendliness, you lose memory for workspace since the extended system is bigger than the others.

All three interpreters can use VDI graphics (Virtual Device Interface). This is a graphics programming standard. V 11 comes with a wide variety of drivers to use VDI graphics. There is only one problem – using VDI means loading huge TSR drivers that gobble up precious workspace memory. If you have enough UMBs you can put these drivers there, else use the APLNOG (the smallest interpreter) and don’t use large arrays!

### Run-Time Applications

V 11 comes with a utility which converts a workspace into an EXE file. Before this can be done, you must assign ŒLX to a function in your workspace. You must make sure that your workspace does not go into immediate execution mode. This will result in your EXE file terminating to DOS. You can also assign ŒSA to ’OFF’ to return to DOS, but simply returning to immediate execution mode will do the same.

The next step is to ‘bind’ your workspace to a run-time engine. Four run-time engines are provided, one for the extended system and three different sized ones for the standard system. The difference in size is because each run-time engine supports different modules. The largest standard engine supports all standard APL functions. Below is a table showing which engine supports which modules:

```
   MODULE                             ? LARGE  ? MEDIUM ? SMALL
⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲⍲Å⍲⍲⍲⍲⍲⍲⍲⍲Å⍲⍲⍲⍲⍲⍲⍲⍲Å⍲⍲⍲⍲⍲⍲
BIG SCREEN SUPPORT                    ?  Y    ?        ?
CMD                                   ?   Y    ?   Y    ?   Y
FULL COMMUNICATIONS                   ?   Y    ?        ?
PART COMMUNICATIONS                   ?        ?   Y    ?   Y
⎕COPY, ⎕PCOPY                         ?   Y    ?        ?
DETACHED INPUT/OUTPUT                 ?   Y    ?        ?
DETOKENIZATION                        ?   Y    ?   Y    ?   Y
DOMINO FUNCTION                       ?   Y    ?   Y    ?
FIND FUNCTION                         ?   Y    ?   Y    ?
⎕DM                                   ?   Y    ?   Y    ?   Y
⎕FDUP                                 ?   Y    ?   Y    ?
GRAPHICS FONT                         ?   Y    ?        ?
⎕FMT                                  ?   Y    ?   Y    ?   Y
FULL SCREEN EDITOR                    ?   Y    ?   Y    ?
⎕G
STYLE GRAPHICS                     ?   Y    ?        ?
HELP FACILITY                         ?   Y    ?        ?
APL KEYBOARD                          ?   Y    ?   Y    ?   Y
MOUSE SUPPORT                         ?   Y    ?   Y    ?
USE OF OVERSTRIKE                     ?   Y    ?   Y    ?
⎕NA                                   ?   Y    ?        ?
⎕UCMD                                 ?   Y    ?   Y    ?
⎕WIN                                  ?   Y    ?   Y    ?   Y
⎕SOUND                                ?   Y    ?   Y    ?   Y
PRIMITIVE TRIG FUNCTIONS              ?   Y    ?   Y    ?   Y
VIRTUAL WORKSPACE FACILITY            ?   Y    ?   Y    ?   Y
```

To make your workspace into an EXE file you use the following command from DOS in whichever directory the BINDAWS.COM file is found:

BINDAWS startup(.aws) engine(.exr) applicationname(.exe)

Extensions are optional, as are paths. Your EXE file can have the same name as your workspace. The binding takes a couple of seconds. Note that this is not a compiling utility. It simply attaches a special version of the interpreter permanently to your workspace and then calls it an EXE file.

Before you can run the EXE file, you must serialise it just as you serialised the interpreters.

The EXE file will now run just like your workspace did, and at the same speed. You can also specify APL parameters after the filename on the command line just like when starting APL. Certain operations are disabled such as system commands, ŒSAVE, ŒPSAVE. Function definition is disabled or very restricted.

Unfortunately, the EXE file will use almost the same amount of memory as the workspace. This is due to the inefficient use of modules in a run-time engine. For example, if your workspace functions only use ŒG graphics and nothing else, you have to use the largest engine, the only one which supports ŒG graphics. This means you waste about 70K of memory because most of the engine sits there doing nothing! The only way around this is to ask Manugistics for a custom run-time engine! The other alternative is to choose a run-time engine, then write your code to fit that particular engine, rather than first writing your code any old way, then fitting an engine which provides all the required modules. For example, if you chose the smallest engine, you would have to define your own DOMINO function, rather than use the symbol in your code.

Once you make your EXE file, you can sell it, providing that you include the Manugistics copyright. Even applications using GSS device drivers are covered by the APL\*PLUS PC System licence.

### Other Features in V 11

V 11, compared with V 5, contains many new features, mainly those described above. It also has a few more Œ functions, as well as enhanced versions of existing ones. V 11 supports virtual workspaces if you wish. It still has its limits though. Functions such as GRADE UP are still limited to 64K arrays.

### Conclusion

The ability to make EXE files is, itself, worth the upgrade if you are still using APL\*PLUS PC. As an interpreter, it is probably years behind the 386 and windows versions, but it is quite good for DOS. This will probably be the last APL\*PLUS PC version out.

(webpage generated: 2 October 2007, 23:56)
