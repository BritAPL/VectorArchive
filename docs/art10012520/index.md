---
vid: '10012520'
title: 'Workshop: APL*PLUS II Sessions'
byline: by BAA-GUI
volume: '10'
issue: '3'
page: '57'
source: trad/v103/baa103_57.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- conference reports
- implementations
- user interfaces
---

by BAA-GUI <span class="affil"></span> <span class="email"></span>
{ .byline }

There were three speakers in the APL\*PLUS II stream of the BAAGUI Workshop. Peter Day with interjections from Dave Phillips both of Cocking and Drury, Duncan Pearson of Nestlé-Rowntree, and Dave Piper of Eagle Star.

### Peter Day and Duncan Pearson

*notes by Anthony Camacho*

Peter Day of Cocking and Drury introduced us to the interpreter and the differences between version 4.0 and 5.0. APL\*PLUS III will be in a different league and will not be fully upwards compatible. It will only work on 32-bit machines and will not try to run the `⎕WIN` or `⎕WGET` or `⎕WPUT` functions we all learned in APL\*PLUS/PC. To make conversion to PLUS III easy one should be prepared to replace these and all the DOS-dependent features such as `⎕CMD`, `⎕INKEY`, `⎕ARBIN`, `⎕G*`(anything), all the peeks and pokes, the printing methods and so on.

In APL\*PLUS II version 5.1, which we used for the day, there is a function Win which sends its right arguments to an APL\*PLUS Agent which runs minimised under Windows. This agent is what actually communicates with Windows.

Duncan Pearson explained that if you adopt the procedures recommended by Manugistics, each object (form, key, radio button, edit box, label, combobox and so on) requires a one or two line function to be created. A workspace with say 20 forms each with twelve objects on it would require a workspace of over a thousand functions and as these are created automatically and have long names (made up of the names of their elements) maintenance gets to be very tedious.

Instead he and Adrian Smith have devised a tabular way of recording and implementing the object. The details are entered into a ten column matrix (a nested array) per form where the first line defines the form and the subsequent lines defines the objects in it (which are sometimes, confusingly, called controls).

An advantage of this approach is that there is a single nested matrix for each form in the workspace. The workspace is not cluttered with hundreds of functions. It is then possible to write utility functions to display and amend the objects of a definition matrix. This makes the workspace much easier to understand and to maintain, as a print of each definition matrix effectively summarises the windows interface of the workspace.

We spent a large part of the afternoon trying to construct a definition matrix for a form to collect or amend name and address records. It had to have appropriate fields for name, initials, the lines of the address including country and zip or postcode, some other details such as a classification, telephone number or email address and the necessary keys and buttons to look people up, add, amend or delete them.

It is a tribute to Duncan’s exposition that some people actually got something working before the exercise was brought to a halt to allow Dave Piper to show us a different method of overcoming the difficulties in using APL\*PLUS II under Windows.

### David Piper of Eagle Star

*Notes by Duncan Pearson*

David Piper of Eagle Star showed us a completely different approach to the interface with windows. Instead of using all the support functions in the APLGUI workspace to define and create his windows, he uses a standard dialog box editor to define the boxes and a few quite simple windows routines to read them off a resource file and define them. He showed us a small application built using these tools, a PC/Mainframe file transfer utility that unsurprisingly did not have its full functionality, but which showed the user interface well. Then he gave a presentation of his design objectives and implementation details, which was a set of slides each of which was a dialog box run by his utilities. The result was very fast and looked easy for the application developer to use. However it obviously could not have been done without the system programmer (David) knowing a fair amount about Windows dialog box classes.

The process of defining windows interactively at run time, which is how the APL\*PLUS II APLGUI does it, is horrendously slow. Windows has to allocate resource individually for the form and for each of its controls (buttons, edit fields, scroll bars etc.) at the time of creation. However there is a class of window available whose definition contains the definition of all its controls and the creation of which is very fast. This is the dialog box. It is a little restricted in that it can contain only 255 controls, but obviously this design constraint is far outweighed for David by the speed of creation and the relative modularity of the code needed to manage it. It is also not possible for the APL program to change aspects of the definition (e.g. the height, width and number of edit fields) at run time.

The standard procedure with compiled languages is to define the dialog box with a resource editor and then store the compiled definition, firstly into a resource file and then finally into the application’s executable file. Instead of using a windows call to create each control in the box the application uses one call giving the name of the dialog box within the executable file; at which point windows creates the whole dialog box and all its controls in one fell swoop. This process is effectively what he has enabled his application developers to do from APL. He has produced a small set of APL utilities that read a dialog box definition from a resource file, call the windows function to create the dialog box from the definition and associate APL expressions with events on the dialog box.

The window definition is done in a standard dialog box editor from the Software Developer’s kit or from Borland Resource Workshop. The result is saved to a resource file (with extension .res) and when needed the definition is read from the file into the workspace using `⎕NREAD` and then used in a windows call to create the dialog box. The call is fast because all the hard work is done by the dialog box editor. Furthermore, as the definitions are held on file and only retrieved when they are needed for creation, it is possible to have the resource editor running at the same time that the application is running and to make small changes to the appearance of the boxes without halting the APL execution.

For a production system in which no change to the dialog boxes is envisaged the definitions can be compiled into the APL Agent executable file (the windows task that communicates with the APL interpreter) by the SDK resource compiler. This reduces the number of application files and means that the definition need no longer be read from file and passed to the ‘create dialog box’ API routine. Instead a slightly different routine is used that is passed the name of the box, which it then reads direct from within the Agent. This saves even more time as the definition does not have to be read from file.

The behaviour on a user interacting with a control is defined using the APLWIN function W\_CreateFilter, which associates a line of executable APL with a windows event, on a control. Whenever the event happens to the control the line gets executed. This is very flexible in contrast to the APLGUI where one needs to have one function per event per control per form, a restriction which can cause a vast number of one line functions in the workspace, but which will however be lifted in APL\*PLUS III. It also gives access to all the messages that Windows passes instead of having them filtered by a well-meaning middle layer.

To conclude, this is a good fast solution to the problem of the Windows interface from APL\*PLUS II. It will be interesting to see if APL\*PLUS III addresses the problem of speed and modularity to the extent that David thinks that it is viable. There is no doubt though that he feels happier working close to the metal and relying on his own code. The results were certainly impressive.

(webpage generated: 18 February 2006, 02:16)
