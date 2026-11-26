---
vid: '10501212'
title: Dyalog Ltd industry news
authors:
- Morten Kromberg
volume: '26'
issue: '1'
page: '4'
received: '2013-09-28'
online: '2013-12-24'
description: Dyalog industry news
source: content/printed/261/dyalog/dyalog.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- implementations
- APL community
---

## Dyalog Ltd

In accordance with tradition, the preparations for the annual Dyalog User Conference (Dyalog ’13, to be held at Deerfield Beach on October 20th-24th) coincide with the final stages of planning of the next release of Dyalog APL (version 14.0, due out in Q2 of 2014). With about 15 developers beavering away on the APL interpreter and surrounding interfaces (including a full-time Technical Writer and Documentation Manager, Fiona Smith, who joined us in April), 2014 is going to be a very busy year for any users of Dyalog APL wishing to take advantage of all the new functionality and read all the new documentation that we are planning to roll out!

### Going Cross-Platform

Most computer users will have noticed that a major shake-up of computing hardware and software platforms is in progress. At the server end, Linux is growing in importance as a platform for virtualised server and cloud computing due to its relatively small footprint. At the client end, Android and other tablet and phone operating systems are also growing rapidly. Multi-core hardware is appearing on clients and servers. While Windows Desktop will probably remain the *bread and butter* platform for most of our customers for many years yet, we want to allow people to take advantage of new platforms when they are ready to do so.

To simultaneously make APL more portable and improve the user experience for those wanting to develop on multiple platforms, we are separating the APL Engine from the development environment. 2014 will see the introduction of a new Remote IDE, which will be able to run as a native application on most popular client platforms, providing rich user interfaces with the same functionality across on all platforms. The *RIDE* will be able to connect from any client platform to APL engines running on any platform, locally or across a network. We expect to announce official support for at least two additional engine platforms at the conference, in addition to the four that are currently supported: Microsoft Windows (Intel), IBM AIX (Power) and Linux (Intel and ARM).

Having APL on many platforms is only truly valuable if users can also develop applications that are easy to relocate. To facilitate this, there will be new releases of tools for cross-platform application development, such as our StandAlone Web Service framework (SAWS), the MiServer web server toolkit and the Dyalog File Server.

### Going Parallel

Making it possible for APL users to harness parallel hardware is another extremely important goal. We are working on three different approaches to this problem:

Futures and Isolates:
:   Version 14.0 will provide new language constructs to make it easy to use multiple processors without the use of locks or semaphores for synchronisation:

    - *Isolates* are namespaces that are managed by separate processes.
    - *Futures* are array items that can pass through structural functions and calls to defined functions without blocking but block automatically when passed to a primitive function that requires an actual value until the value is produced.

    Expressions that are executed within isolates immediately return futures.

A new parser to reduce interpreter overhead:
:   Also scheduled for delivery as part of version 14.0 is a new parser which performs static analysis and optimisation, significantly reducing interpreter overhead. Initially, the parser will only target functions that have no side-effects, but work on the parser will continue for the next several releases. For functions with small arguments, speed increases of a factor of 2 have already been observed.

An experimental APL compiler:
:   Dyalog is funding research at the University of Indiana into a highly-parallel compiler for “functional” APL code (initially only targeting dfns). Futures and isolates provide the ability to parallelise algorithms with coarse-grained parallel components - if the user indicates where the parallel sections are by explicitly using isolates. The compiler aims to automatically parallelise APL expressions. The goal is to seamlessly integrate the compiler into the interpreter, offering users the option of compiling code that is identified as suitable, but this is still very experimental, it is unknown when this technology might make it into our product line-up.

### Going Functional

Functional programming focuses on writing expressions without loops and without mutation of global state (or other side-effects). APL has always supported programming in a functional style, and in Dyalog APL the addition of the lexically-scoped dfn style of programming has made it easier to write more strictly functional application components. Apart from having advantages in terms of maintainability, the lack of side-effects means that it is easy to compile and automatically parallelise programs that are written in a functional style. The work that we are doing in the direction of compiling and parallelising the execution of user-defined functions is all significantly easier when the code is already functional: if you have not yet looked at using dfns for the computational components of your application, then this would be a good time to do so.

In addition to continuing to enhance the tooling for dfns, we are also adding a number of language features to version 14.0 that promote a more functional and parallel style of programming:

Function Trains:
:   Version 14.0 recognises trains of three functions as *forks* (as in the J programming language) and trains of two functions as *atop*. For example:

    ```
            (+/ ÷ ⍴) 1 2 3 4 ⍝ Average is sum divided by count
        2.5
            'abcdef' (~∊) 'aeiou' ⍝ ⍺ (~∊) ⍵ ←→ ~⍺∊⍵
        0 1 1 1 0 1
    ```

    Function trains allow the construction of functional units of code that are obvious optimisation targets. For example, version 14.0 will optimise uses of *atop* such as (`⍳∘0 >`), making an early exit from the execution of the relational function on the right when it knows it will be searching for a 0 or 1 in the result.

Rank operator (`⍤`):
:   provides a more general mechanism than the existing axis modifier, for the repeated application of primitive or user-defined functions to sub-arrays of specific ranks.

Key operator (`⌸`):
:   similar to an SQL “group by” statement, the key operator repeatedly applies functions to subsets of one array, corresponding to the distinct values in an array of keys. For example:

    ```
            ('red' 'green' 'blue' 'green') {⍺,+/⍵}⌸ 10 20 30 40
        red    10
        green  60
        blue   30
    ```

Many common cases of key and rank, such as (`{+/⍵}⌸`) which computes the sum of values corresponding to each key, are highly optimised and significantly faster than the best APL expression than can be written to compute the same values before v14.0. Also note that, in order to pave the way for parallel implementations, the order of the invocations of the operand function is undefined for rank and key, unlike existing operators such as each or outer product, which always process elements in “ravel order”.

### Other Version 14.0 Features

Version 14.0 will also include a number of speed-ups and an extension to dyadic iota to directly support *rowfind* operations in the primitive. We are looking at embedding compression algorithms into the interpreter to support compression of component files and the source of namespaces and classes, and many other improvements.

We are making a number of Technical Preview releases available – TP3 of v14.0 was released at the end of September. Contact sales@dyalog.com if you would like to help us evaluate the new language features.

If you can’t make it to Dyalog’13, check our web page around Christmas for recordings of conference sessions.
