---
vid: '10010810'
title: 'Meeting: Presentation of APL*PLUS III'
authors:
- James Wheeler
byline: by James Wheeler
volume: '11'
issue: '1'
page: '92'
source: trad/v111/wheeler111_92.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- APL community
- implementations
- user interfaces
---

by James Wheeler
{ .byline }

The presentation was well backed with a series of messages projected onto a screen and was followed by a short demonstration of the product. In the first part the presentation closely followed the bullet points on the screen, so I have done my best to reproduce these.

James Wheeler began with Manugistics’ goals for this new product:

1. The look and feel of a real Windows product
2. Very easy and powerful GUI features
3. Enhance programmer productivity
4. Open APL product which co-operates with other Windows products
5. Accelerate change in the APL language

He said it is more an attitude than a product; these goals will guide further work.

The product architecture is related to Windows:

- It uses Win32s, the Windows 32-bit architecture and won't run where this isn't available
- It runs under Windows 3.1, Windows for Workgroups 3.11, Windows NT
- In future it will run under Chicago, Cairo and Windows 4 (if that is the name for the Windows 32-bit upgrade).

The key features of the product are:

```
○ A new Windows session manager and editor     
○ A debugger     
○ A user command processor     
○ Object-oriented Graphical User Interface
○ A Visual Forms Editor so you can draw your interfaces
○ Interface to Windows DLLs     
○ Dynamic Data Exchange     
○ Built-in graphics and printing     
○ New control structures     
○ A royalty-free run time system included with the product
```

It is a high-performance product. It had surprised even its authors by running computationally heavy work faster under Windows than APL\*PLUS II ran the same work under DOS alone.

It has GUI features built-in and so is faster than the agent of APL\*PLUS II.

The current workspace size limit is two gigabytes.

The built-in control structures are faster than branching.

About the control structures James said that there are many algorithms which cannot be elegantly expressed where the data has to be in flat arrays; with nested arrays some loops may be eliminated or, using each, removed at least from repeated interpretation. Even so rea- world APL is still full of spaghetti code and the new control structures make it possible to match other languages, and may help overcome the prejudice of some DP professionals. APL has an implicit structure in that after line 3 it will obey line 4 unless told not to.

### The new structures:     

```
○ are conforming extensions to standard APL     
○ provide a sort of 'outer syntax'     
○ improve the readability and structure of APL code     
○ are familiar to C and Pascal programmers
```

They are:

```
:if       :else   :endif     
:while    :endwhile     
:repeat   :until     
:select   :case   :caselist   :endselect     
:for      :in     :endfor     
```

They all begin with the colon and must be on the beginning of a line (which therefore cannot be labelled). And as his final knock-down point in favour James said that Adrian Smith approves of this innovation so it must be right!

In a short diversion on the benefits of the Graphical User Interface (for the mainframers who hadn’t yet encountered one) James said that the GUI gives the user a better intuitive understanding of how to work with software. By allowing exploration and allowing mistakes to be recovered it makes complex software much less intimidating, and it allows users to discover how an application works without reading the manuals. By contrast the old-fashioned fascist approach characteristically required the user to make valid entries in the order the programmer decided until the programmer deemed the user had earned the chance to make a choice. The old approach hardly encouraged the user to feel in charge.

The GUI in APL\*PLUS III provides forms (which are windows) and means to control the elements on a form. The control is via system commands `⎕wi ⎕wself ⎕wevent ⎕warg ⎕wres ⎕wcall ⎕na`.

Every part of a window is an object: an object is a self-contained lump of code and data. Other objects or software can interact with the object but cannot see inside it; all they can do is pass it arguments and maybe get results back. Objects can only be accessed through a well defined interface, with the overall result that GUIs are much simpler to build and operate.

There are two kinds of object in APL Graphical User Interfaces. They are:     

Methods: think of these as functions inside the object which can be called with arguments.

Properties: think of these as variables in an object that can be set and referenced.

Objects take action when a property is set (for example by keying into a field) or a method is invoked (for example to close a form) or when an event is signalled (for example by pushing a button). An event is the system notifying the program that something has happened that requires or may require a response. The system uses events to tell the program what the user has done to the interface. James concluded by saying that he regards APL\*PLUS III as an event-driven interpreter for writing event-driven programs.

Then he fired up the interpreter and, after checking that `2+2` and `⍳9` produced the expected results, showed that `)clear 5m` gave a workspace of five megabytes by assigning a million random numbers from 1 to 10 to a variable and then summing it to give 5500000.

He then showed the options available on the menu bar, demonstrated that one could cut in any of APL, MSWord and Excel and paste into any of them. A workspace called bouncer showed a ball bouncing around the inside of a window. James showed how it was constructed and that he could change the colour of the ball or its diameter on the fly without ill effect.

Actually he was unlucky; he increased the diameter just as the ball was about to bounce and the change in diameter caused the horizontal movement to stop. I suspect that the change perhaps took the edge of the ball past the edge of the window without triggering the event that should have reversed its horizontal increment. The ball then slid up and down the left side of the window. To my surprise, James released it by dragging the left edge of the window smartly left well beyond the ball position and it cheerfully resumed its bouncing as before.

## ![figure\_1](wheeler111_92-fig1.gif)

In response to questions James said that `)clear` would not be enough to reset the windows objects. Once created they have an existence independent of the APL workspace. They must be cleared with a reset command.

He hoped to add user-defined GUI objects next and then to provide Object Linking and Embedding but was unable to give much idea of a timescale for these additions.

Questions only died down after the display equipment had been packed away in its box and numerous members of the audience had had to leave to catch trains. This new APL product certainly is of great interest to BAA members and James was heartily thanked for coming to show it to us.

(webpage generated: 10 October 2007, 05:15)
