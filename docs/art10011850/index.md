---
vid: '10011850'
title: 'SALT: A Simple APL Library Toolkit'
authors:
- Dan Baronet
byline: Dan Baronet dan@dyalog.com
volume: '23'
issue: '3'
page: '103'
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
source: trad/v233/baronet.htm
converter: vecrescue 0.1.0
issue_tab: n3-june-2008
tags:
- development practice
- data and files
---

Dan Baronet  
[dan@dyalog.com](mailto:dan@dyalog.com)
{ .byline }

The ability to write readable APL code out to text files and read it back has been pretty much out of reach for mere mortals until recently. Reasons for wanting to do such a thing abound:

- Keeping code out of the workspace to conserve memory
- Sharing or sending copies of code to people who don’t have an interpreter
- Storing readable code outside APL
- wanting a copy of the code independently of having an interpreter or being bound to a version of it

With the advent of Unicode it is now easier to achieve this. APL characters are represented in Unicode, and editors and viewers of Unicode text files are now commonplace.

## Using script files in Dyalog APL

Since Version 11 it has been possible to get the source of some namespaces and the (then) new classes/interfaces. If desired, this source could then be written to file in Unicode format using the `UnicodeFile` class provided in `⎕SE`. This allowed one to create text files that could then be read or edited with programs such as Notepad, and keep the source out of the workspace:

```
      F←⎕NEW UnicodeFile
      F.Write 'MyFile.txt' text
```

With Version 12 the `UnicodeFile` class is no longer needed as the new `⎕UCS` system function can now create UTF-8 strings directly from the Unicode points (APL text/code). This stream can then be written out to file just like any other APL array. Reading back is also easy: read the stream, use `⎕UCS` to reconstruct the string.

```
      (⎕UCS 'UTF-8' ⎕UCS text) ⎕NAPPEND tie    ⍝ write to file
      ⎕UCS ⎕NREAD tie 80,⎕NSIZE tie            ⍝ read from file
```

Anybody can write two functions to write and read, put them in `⎕SE` and have the ability to store and retrieve APL code from native Unicode text files anytime they want to.

Of course, once you start doing something like this you realise it might be a good idea to save multiple versions of a file or script, to be able to retrieve any of those versions, list them, compare them, remove unused versions, and so on.

And this is exactly what Dyalog did when we came up with SALT: we wrote two functions, `Load` and `Save`, and added a few more functions to look up directories and files. We added the ability to store multiple copies of the same file and a function to be able to compare them, even by using your favourite comparison program.

We went a step further, and added the ability to start APL without a workspace, simply by loading scripts and executing a statement to start the process.

We modified the editor to generate an event upon fixing an object thus opening the door to intercepting the fix and saving the modified file automatically.

We wrote a few scripts, besides SALT (itself a script) and made them available.

And that was pretty much all we did.

SALT comes bundled with Dyalog and is enabled by default. You can disable it through the configuration menu or through the accompanying SALT workspace. Simply `)LOAD SALT` and use `enableSALT` or `disableSALT`.

## Usage

SALT functions use a consistent syntax: they all take a string argument which describes the arguments to the action to perform. They make use of switches à la Unix (e.g. `-version`). All script files have the extension `.dyalog`. If a relative path is used they are stored under a default directory, which may be changed.

Here is a list of the main SALT functions: (brackets denote optional switches)

```
Save  'object \path\to\file  [-version=x]'
```

Saves object in the file specified, with extension `.dyalog`. If `-version` is specified the filename will include the version number. Once version numbers are in use there is no further need to specify the version; SALT will attribute a new version number each time an object is resaved.

If a relative path is supplied SALT will store files under the default SALT path. This default is `\dyalog\installation\path\SALT` but it may be changed using the `Settings` function:

```
Settings  'workdir    \new\path'

Load  '\path\to\file    [-version=x]'
```

Loads the latest version or the version specified.

```
List   '\folder\or\file  [-recursive]'
```

Lists the scripts in a folder, recursively if `–recursive` is given.

```
Compare  'script1  [script2]'
```

Compares two scripts or the last two versions of a script if only one is supplied as argument.

## There’s more

There are other features not listed above: you can:

- skip the confirmation prompts in `Save`
- specify where to define the objects
- disperse their contents instead keeping them in their original namespace
- get the source instead of defining the object
- list the entire tree of a path
- clean the versions
- use external programs to compare objects, etc.

And it works as well under Unix or Linux.

If this is interesting, SALT comes with complete documentation which can also be found on Dyalog’s website. Have a look!
