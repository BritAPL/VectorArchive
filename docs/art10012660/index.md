---
vid: '10012660'
title: 'Hacker''s Corner: Minding Your .INIs'
authors:
- Adrian Smith
byline: by Adrian Smith, . (adrian@apl385.com)
volume: '10'
issue: '3'
page: '96'
source: trad/v103/smith103_96.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- system interfaces
- data and files
---

by Adrian Smith, <span class="affil">.</span> <span class="email">(<a href="mailto:adrian@apl385.com">adrian@apl385.com</a>)</span>
{ .byline }

### Introduction

Some Windows applications are better behaved than others. Winword is one of the worst – it writes about 100 lines of junk all over your WIN.INI, which is a wonderful way of slowing the Windows startup. A well-behaved application (such as Dyalog APL/W) just slots in a minimal entry:

```
[DyalogAPL]
dyalog=c:\wdyalog
inifile=c:\wdyalog\apl.ini
sound=quack
```

to tell it where to go and look for a private .INI file, which can be hidden away in its own directory. (As for how that spurious “sound=” entry got there ... well, you will have to read on to find out.)

Many applications compromise, and maintain a private .INI, but stick it in the Windows directory where they can be sure to find it. MS-mail is typical, as is CLOCK, which leaves behind a CLOCK.INI looking something like this:

```
[Clock]
Maximized=0
Options=1,1,0,0,0,0
Position=119,708,155,744
```

Which leaves you with a couple of choices to make, and a little gentle research to do to find out how to maintain these files safely from your APL application.

### Choices

Here are my views on the three methods outlined above:

1. Sticking everything into a \[section\] in WIN.INI. This is for lazy programmers only. It is definitely the soft option, and Windows gives you a simple utility which does just that. It has the unfortunate side-effect of adding to an already overloaded file, and also makes your application a pain to un-install. I suggest you don’t do it.
2. Making an APPL.INI file in the Windows directory. This is more reasonable, as this is where most simple folk start looking for .INI files. Again, there are simple utilities available to help you. The only disadvantage is that your application is now scattered over a couple of directories, and someone may re-install Windows and clean you out accidentally.
3. Making an APPL.INI file in your own home directory, and inserting a pointer to it in WIN.INI. You could use the existing \[DyalogAPL\] section, or you could start an \[APLApps\] section on the assumption that you won’t be the only APL program on the machine. I think I like this option best, and it is what the better-behaved Windows programs are now tending to do.

Assuming that you want to do something sensible to record your startup parameters (no, don’t put them in a component file!) where they can be fixed up with any old text editor – read on.

### Option-1: Infesting WIN.INI

Before you go updating things, make sure you can read them! This is the first function you need to know about:

```
     ∇ r←GPS arg;gps;section;entry
[1]   ⍝ Access to profile string from WIN.INI
[2]   ⍝ e.g. GPS 'dyalogAPL' 'inifile'
[3]    'gps'⎕NA'I kernel.P16|GetProfileString <0T <0T <0T >0T I'
[4]   ⍝ Usage <gps 'DyalogAPL' 'inifile' 'default value'  40 39>
[5]    section entry←arg
[6]    r←2⊃gps section entry'(undefined)' 256 255
     ∇
```

This takes a vector of two elements, the first of which is the section (but don’t put the \[ \] around it); the second is the parameter you want to know about. It returns a simple vector, with the parameter value, or “(undefined)” if it failed to find anything. For example:

```
     GPS 'Colors' 'ButtonFace'
192 192 192
```

Anything you can get, you can set:

```
     ∇ {r}←WPS arg;wps;section;entry;value
[1]   ⍝ Write info to profile string in WIN.INI
[2]   ⍝ ****** USE WITH CARE!! ******
[3]   ⍝ e.g. WPS 'DyalogAPL' 'sound' 'quack'
[4]   ⍝
[5]    'wps'⎕NA'I kernel.P16|WriteProfileString <0T <0T <0T'
[6]   ⍝ Usage <wps 'section' 'entry' 'new value'>
[7]    section entry value←arg
[8]    r←wps section entry value
     ∇
```

... which is how that “quack” got in there! If you use a new section name, Windows creates the section for you, if you use a new parameter name, it is added automatically to the section. It really is amazingly helpful, which probably explains why the average WIN.INI is so full of junk.

### Option-2: Infesting the Windows Directory

For simple applications, this is probably a good compromise. This time, to create your .INI file, you use a related Windows call:

```
     ∇ {r}←fi WPPS arg;wpps;section;entry;value
[1]   ⍝ Write info to private profile string such as appl.ini
[2]   ⍝ e.g. 'clock' GPPS 'clock' 'position' '200,200,100,100'
[3]   ⍝ Assumes '.ini' unless file extension given.
[4]   ⍝
[5]    'wpps'⎕NA'I kernel.P16|WritePrivateProfileString <0T <0T <0T <0T'
[6]   ⍝ Usage <gpps 'section' 'entry' 'new value' 'inifile'>
[7]    fi←fi,(~'.'∊fi)/'.ini' ⋄ section entry value←arg
[8]    r←wpps section entry value fi
     ∇
```

This continues the helpful approach by creating the entire file for you if it didn’t already exist. For example:

```
     'fluff' WPPS 'Options' 'colour' '255 255 255'
     'fluff' WPPS 'Options' 'size' 'small'
     'fluff' WPPS 'Toolbar' 'setting' 'off'
```

results in the creation of a ‘fluff.ini’ file which looks like:

```
[Options]
colour=255 255 255
size=small

[Toolbar]
setting=off
```

... which can later be recovered just as straightforwardly with:

```
     ∇ r←fi GPPS arg;gpps;section;entry
[1]   ⍝ Access to private profile string such as clock.ini
[2]   ⍝ e.g. 'clock' GPPS 'clock' 'options'
[3]   ⍝ Assumes '.ini. unless file extension given.
[4]   ⍝
[5]    'gpps'⎕NA'I kernel.P16|GetPrivateProfileString <0T <0T <0T >0T I <0T'
[6]   ⍝ Usage <gpps 'section' 'entry' 'default value'  40 39 'inifile'>
[7]    fi←fi,(~'.'∊fi)/'.ini' ⋄ section entry←arg
[8]    r←2⊃gpps section entry'(undefined)' 50 49 fi
     ∇
```

You might even find these handy for fixing the settings on some of the standard applications, for example to start a scientific calculator from an APL button.

### Option-3: Hiding the .INI in your own Directory

This is really just a combination of the first two. I think the best compromise is to add an entry to an \[APLApps\] section:

```
WPS 'APLApps' 'Fluff' 'c:\apps\fluff\fluff.ini'
```

... and to maintain FLUFF.INI as above with WPPS. This does mean one extra lookup when you start, but you can be sure the \[APLApps\] section is there, and you have generated the least possible clutter in the Windows directory.

### Summary

There really is no excuse for not following the way of Microsoft, and putting all your startup options in an editable .INI file. You don’t even need to write the editor!

### Postscript

`∇ {r}←{flg}PLAY snd;sps [1] ⍝ Play a .WAV file (asynchronous by default) [2] ⍝ flg←+/0(synch) 1(asynch) 2(no deflt) 8(repeat) 16(nostop) [3] snd←snd,(~'.'∊snd)/'.WAV' [4] 'sps'⎕NA'I mmsystem.P16|sndPlaySound <0T U' [5] ⍎(0=⎕NC'flg')/'flg←1' [6] r←sps snd flg ∇      PLAY 'c:\dash\dog' ⍝ for the friendly Dashboard dog or     PLAY 'c:\windows\sndbits\music\didjeri' ⍝ (Sic)</p>`

... for that true Australian flavour! Does anyone know of a good duck?

(webpage generated: 18 February 2006, 02:17)
