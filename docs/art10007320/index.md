---
vid: '10007320'
title: Source Code Management of APL Code
authors:
- Richard Renich
byline: by Richard Renich, SoftMed Systems, Inc.
volume: '22'
issue: '2'
page: '138'
source: trad/v222/SCM222.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- development practice
---

by Richard Renich, SoftMed Systems, Inc.
{ .byline }

## Abstract

This document gives a quick introduction to source code management (SCM). It identifies problems specific to using text based SCM tools to manage APL code. It suggests some solutions for those problems.

## SCM Overview

Source code management means many things to many people. The common aspect is a system for recording and recalling revisions of code. The basic notion is that you can write some code, commit the code, make some changes, and submit again. You can then return to the code as it was at the previous commit. Each commit (or submission) of code creates new revisions of any files that have changed. The code can be recalled by date or by an identifier assigned during a commit. Most SCM systems have the ability to assign a name to specific revisions of files so that the code can be recalled by name as well.

A good introduction to source code management can be found at Perforce’s website *[www.perforce.com/perforce/technical.html](http://www.perforce.com/perforce/technical.html#whitepapers)*, specifically *High-Level SCM Best Practices* and *Perforce Life-Cycle Modelling*.

SoftMed currently uses component files to manage APL code. All other code is managed by *Perforce*, a commercial SCM tool. We are in the process of converting our APL code management to Perforce.

## Typical Use

Once you have loaded your workspace, you would normally sync up with the SCM repository. That brings in changes that others have made since your last sync. Then you would make code changes and commit those changes. How frequently you sync and commit depends on a number of things, like how many people are working on the code and what quality of code you want committed.

In a broader time-frame, you will make branches of the trunk, which holds all your code, and merge the branches back to the trunk. The trunk is just a branch from which most other branches are taken. The manner in which you work with a branch and the trunk is analogous to the manner in which you work with a workspace and the SCM repository. You periodically bring changes from the trunk into the branch and merge the changes of the branch back to the trunk. Again, the frequency of these activities depends on your environment.

It might seem that having all these copies (branches and revisions) of the code would use a lot of disk space. Virtually all SCM systems store only the latest revision of each file in a branch as a whole file, and a list of lines to be removed and inserted to produce an old revision from the next newer one. Some systems use a similar technique even on binary files. Some systems keep only a pointer to the original for the first revision of each file in a branch. So using a third party SCM tool can provide significant disk savings over a simple home grown system.

## SCM Benefits

SCM systems are designed to allow multiple programmers to collaborate on the same set of source code. While they don't always prevent two people from editing the same code at the same time, they identify when this happens before any work is lost. Some SCM tools include utilities to help resolve such conflicts, and most facilitate the use of third party tools for this. Some SCM systems allow each developer to indicate the files he is working on, which can help avoid these problems in the first place.

SCM systems maintain a revision history that permits retrieving the source code as it existed at various points in the past.

SCM systems allow code to branch. That is, there can be multiple sets of source code that were derived from the same source. The branches can then evolve independently. Some changes can be propogated among the branches. A common example of this is a product that has been customized for a specific client. The changes specific to that client stay in that branch. Changes to the trunk can be brought into the client branch  Branches can also be used to experiment with a new design or a new feature.

By learning and using a professional SCM tool you take advantage of the work and expertise of programmers familiar with SCM. You also don't need to maintain as much code to be able to use SCM.

## Character Set

APL code uses characters not in the ASCII character set. If this is ignored then some graphic characters will be treated as control characters by the SCM tool. The text that represents functions (`⎕CR` or `⎕VR`) could be converted to ANSI characters. A better solution is to use Unicode characters. Many modern programs, including most SCM systems, work well with Unicode. There is a function, UCS, available from [APL Now’s web site](http://www.apl2000.com/) that can perform the conversion.

## Binary Source Data

Variables in the workspace can be a part of the finished product. Even when they are not, if they aid in the development of the product they should be managed. If they do not aid in development and are not part of the finished product, they should be erased. So all variables should be managed. This reasoning holds just as well for workspace dependant systems variables (`⎕IO`, `⎕CT`, `⎕PP`, `⎕LX`, `⎕ELX`, `⎕ALX`).

Prototypes of empty arrays need to be preserved. This must be done correctly even for nested data.

There is a difference between `A←0 ⋄ A←1↓A,1 0 1 0 0 1 ⋄ A ⎕NAPPEND ¯1` and `A←2 ⋄ A←1↓A,1 0 1 0 0 1 ⋄ A ⎕NAPPEND ¯1`. This makes it important to pay attention to data-types as well as prototypes when working with the text representation of variables. Otherwise there would be a problem if the variable `A` were stored and recalled between the 2<sup>nd</sup> and 3<sup>rd</sup> statements.

The solution is to use `⍙ATR`.  `⍙ATR` is a suite of functions to convert between APL and Text Representations.

The representations of many APL arrays whose atoms are all character have been designed to allow easy editing in a standard text editor. The representation of a function takes advantage of this. Care must be taken to allow a character matrix to retain its width even when it has no lines.

## Two Masters

The APL workspace is one authority for the APL code. It is where changes are made and saved. A local file-system is another authority. It is where changes are received from collaborators. If changes are made in both places, some of those changes could get lost.

The solution is to keep a signature of each object as it was last synchronized with the file-system. If the signature in the WS has changed, it needs to be written to the file-system. If the signature in the file-system has changed, it needs to be read into the workspace. If both have changed, you have misused the system or the system is broken. To avoid having a change in both places, the SCM system is called only when the workspace and the file-system are synchronized.

## File System Names

It would not be wise to try to use any of `"⎕∆⍙"` as a part of a filename. These map to<code> <span class="aplu">"+^~"</span></code><span class="aplu">. </span>These were chosen to work reasonably well in both DOS and Linux.

APL object names are case sensitive. DOS file names are not. This could lead to problems if you have two names that differ only in case, say `∇WHERE∇` and `where←`. The solution is to append a suffix to the name that is unique to the case of the name. This is always done so that there is no way to have two file-system names that correspond to the same APL name.

For the same reason, variables and functions have the same extension (`.atr`). Grouping objects and storing the group by its own name would cause similar problems.

## Many Workspaces to Many Objects

Code reuse is generally considered a good thing. Because of this, the source code for one object may be needed by multiple workspaces. Obviously a workspace includes many objects. This results in a many to many relationship.

To avoid problems with two masters, we already have a list of objects and their signatures. To add the directory name where the object resides in the file-system is not much extra burden. This allows objects to be grouped into libraries.

The signatures must not be managed. They are specific to an instance of the workspace. The list of directory names for the objects does need to be managed. So signatures and directory associations must be kept in separate variables.

## Utility Clutter

The utilities required to use SCM should not clutter the workspace under development. The solution is to use a`UCMDS`file.

## Ability to Reconstitute a Workspace

It is a prerequisite to be able to use the SCM tool to check out a directory hierarchy that holds all objects of a workspace. From that, it should be easy to reconstitute any workspace stored there.

## Subversion

At *[http://subversion.tigris.org](http://subversion.tigris.org/).* Subversion is “a compelling replacement for CVS” written by the authors of CVS. It, like CVS, is free software.

## Perforce

At *[www.perforce.com](http://www.perforce.com/).* Perforce is a commercial SCM tool. It is very similar to Subversion. The main differences between these can be traced to the licensing. Subversion is free software, so there is no license per user. Subversion has no need to track users, so it scales well in this respect. Because Perforce uses a per seat license, the number of users is limited. It has a centralized list of users. It takes advantage of this to offer features not available in Subversion.

Perforce has a public interface in C++. We have used that to create an ActiveX object to invoke Perforce. It has the ability to override the class used to represent files. This allows us to solve the problem of two masters by intercepting reads and writes to the file-system. We have not yet completed this code.

## Credits

Jeff Pedneau wrote our current APL SCM system in APL. Jeff Pedneau, Joe Hatfield, and I have been working toward using Perforce to manage our APL code.

***
