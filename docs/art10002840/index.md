---
vid: '10002840'
title: 'SCREENIO: A full-screen manager'
authors:
- David Doherty
byline: by David Doherty.
volume: '1'
issue: '1'
page: '121'
source: trad/v011/doherty011_121.htm
converter: vecrescue 0.1.0
issue_tab: n1-may-1984
tags:
- user interfaces
- applications
---

by David Doherty.
{ .byline }

For some time now, BUPA has been a major user of APL. The majority of the APL applications which we write for our internal users are highly interactive in nature and to ensure that this interaction is as “painless” as possible we take full advantage of the IBM fullscreen facilities available to us under AP124.

Until SCREENIO, the implementation of these full-screen systems relied solely upon a set of screen design and low-level screen handling software which had been developed within BUPA. This software is similar in nature to the IBM supplied screen software.

SCREENIO treats screens as self-contained entities and addresses such problems as dynamic screen output, complex validity checks, PF key handling and scrolling.

The simple parameter definitions which describe the SCREENIO screen ensure that they are simple to build, maintain and modify.

Before describing SCREENIO, let us first consider some other problems related to screens on their management. We shall also review the screen software we already had and examine how far it went towards solving those problems.

## SCREEN MANAGEMENT

Designing a screen to run within any application system requires careful consideration and planning. It must be considered both from the aspect of the user and the application itself.

### The Screen-User Interface

- Physical layout:  
    The physical layout of fields on the screen, i.e. their position, a shape, and logical order (I have not seen many screens where the “first” prompt appears at the bottom left-hand corner of the screen!). Physical layout is a vital aspect of screen design and can mean the difference between a screen being easy or difficult to use.
- Field intensity:  
    Skilful use of intensity can improve readability of the screen and highlight important screen details.
- Static text:  
    Fixed prompts and headings which guide the user in both the use and purpose of the screen, are vital and must be clear and concise.
- Dynamic text:  
    Anything from time and date displays to dynamic headings or error messages constitute dynamic text; these guide the user in his treatment of the screen.
- PF keys:  
    They are useful for all manner of things e.g. summoning help displays which describe the screen, signalling QUIT or SAVE, controlling vertical and horizontal scrolling, invoking special facilities peculiar to that screen or application.

### The Screen-Application Interface

- Referencing screen fields:  
    Screen fields must be referenced in order to read or write to them.
- Validation:  
    Once the screen has been displayed, the user's entries are examined and validated.
- Screen handling:  
    After, for example, an invalid input, it is usual to reposition the cursor, display error flags or messages, sound the terminal alarm and so forth.
- Capturing screen input:  
    Finally, it is necessary to capture the users (validated) inputs to the screen.

## SCREEN MANAGEMENT -- WHAT HAD BEEN DEVELOPED

Screen design software and low-level screen utilities were already in use ...

### Screen-User Interface

- Physical layout:  
    Our screen design software allowed easy definition and modification of the positions, shape and type of all screen fields.
- Field intensity:  
    Field intensity could be set dynamically as well is that the design stage.
- Static text:  
    Defined during the design stage.
- Dynamic text:  
    Low-level software allowed us, under the control of application code, to write to the screen.
- PF keys:  
    The key depressed may be detected, but handling of the key has to be controlled by application code.

### The Screen-Application Interface

- Referencing screen fields:  
    Field labels, which simplify field referencing, are defined during the design stage.
- Validation:  
    All validation must be handled by application code.
- Screen handling:  
    Screen control is effected through low-level software driven by application code.
- Capturing screen imports:  
    The capturing of screen inputs must be controlled by application code.

Clearly, the management of the screen requires a combination of calls to the low-level screen handling functions for communication with and manipulation of the screen, coupled with application code to actually govern the handling of the screen.

## SCREEN MANAGEMENT -- A CHANGE IN APPROACH

From careful consideration of the above, the advantages in treating screens as self-contained entities which could be handled by standard software were recognised, thereby:

- Removing the repetitive aspects of the process, saving development time and eliminating redundant code.
- Combining both the definition of the physical attributes of the screen with dynamic output, verification, error messages and PF key handling, so that the screen could be regarded as a single entity.
- Providing a standard way of defining these “facets” of the screen so as to simplify its maintenance.

It would then be possible to drive screens via a standard screen manager which would only require the name of the screen to be used. The manager would then drive the screen, performing all validation, executing any dynamic outputs and handling the PF keys according to the parameter specifications made at the design stage.

One possible problem with this approach might be that the programmer would be constrained by such a package. Flexibility was therefore given a high priority.

## SCREENIO—A POSSIBLE SOLUTION

With these aims in mind, SCREENIO evolved. Logically, it is divided into two groups -- maintenance and application. The maintenance routines (which are themselves SCREENIO screens!) are held on a central library and loaded as necessary.

Within the application system only the application group is needed to drive the screens and should be stored as part of the application.

### Maintaining the Screens

SCREENIO screens are held on file and are referenced by name. Its screen maintenance routines utilise our existing screen design software: they do not supplant it.

The creation or amendment of a SCREENIO screen involves extra steps in the design stage these extra steps are integrated with existing screen design software.

SCREENIO's parameter definitions consider the screen both from the aspect of the user and the application.

### The Screen-User Interface.

- Physical layout: no change.

- Field intensity: no change.

- Static text: no change.

- Dynamic text:  
    Text may be written to any field on entry to the screen. An output parameter is simply defined for that field. Most dynamic headings, time, help and date displays are catered for.

- PF Keys:  
    They may be separately defined for each screen. Some PF keys are defined according to our own internal standards. These are:
    - PF1 summons a help display.
    - PF3 is QUIT.
    - PF12 is END.
    - PF9 displays the PFD descriptions.
    - PF keys 7, 8, 10 and 11 are reserved for vertical and horizontal scrolling of data.

    These keys, along with their standard descriptions, are automatically defined for each screen when it is created. Scrolling will only operate if the data exceeds the size of the field in either, or both of, the vertical and horizontal dimensions.

    The remaining keys, 2, 4, 5 and 6, may be defined by the programmer as special-purpose keys. Both the description and a definition may be entered. EXECUTABLE EXPRESSIONS are entered for the definitions and text for their descriptions.

### The Screen-Application Interface

- Reference screen fields: no change.

- Validation:

    SCREENIO provides for the verification of screen input in two ways:

    INDIVIDUAL FIELD VALIDATION

    Fields may be validated on an individual basis. For each input field, a number of parameters are defined during the design stage and they control the validation for that field.

    For each field, an (EXECUTABLE) VALIDATION EXPRESSION is entered, which is used to validate the contents of the field. SCREENIO contains some “inbuilt” validation routines for the more common validation requirements, like numeric validation, table lookups etc. They are invoked using special characters within the expression.

    If the input into the field is INVALID, SCREENIO will write an ERROR MESSAGE which the programmer has defined, to a nominated ERROR field. The ERROR MESSAGE may be static, or dynamically defined.

    The input fields can be flagged as MANDATORY or OPTIONAL which means that the programmer can force the user to input details where necessary or omit them where defaults have been predefined.

    Similarly SCREENIO can be directed to CLEAR INVALID entries from input fields or leave the entry for the user to edit. Finally, SCREENIO will assign the contents of the field to a (GLOBAL) variable which the programmer nominates.

    RELATED FIELDS

    It is frequently necessary to consider fields in relation to one another: for example, it may be necessary to multiply two fields together to form a new result, which may in turn have to be validated.

    SCREENIO provides for this through the entry of “rules” which apply to the input fields. Each “rule” may apply to as many, or as few fields as required. Like the individual field validation, the programmer defines several parameters.

    An EXECUTABLE VALIDATION EXPRESSION is used to examine input fields in relation to one another.

    If the result is VALID, an (optional) result may be written to a designated field. If the result is INVALID, an ERROR MESSAGE which again may be static or dynamic is written to an ERROR FIELD.

- Screen handling:  
    SCREENIO handles the screen in a consistent and logical manner following the use of PF keys, error messages etc.

- Capturing screen inputs:  
    SCREENIO assigns (as described above) all the users' inputs into (GLOBAL) variables.

### Driving the screen

To run a SCREENIO screen it is only necessary to call SCREENIO with the name of the required screen passed to it as an argument.

It utilises the low-level screen handling software, governing it according to the operation being performed. We may summarise the execution of SCREENIO as follows.

1. Reads the file-held variables for the required screen into the workspace.
2. Formats the screen and writes both STATIC and DYNAMIC text.
3. Displays the screen and awaits user input.
4. Action keys depressed.
5. Validates input and assigns it to (GLOBAL) variables.
6. The screen is finally left. The key depressed is returned as an explicit result. The application code can act upon the key returned and utilise the GLOBAL variables SCREENIO has defined.

## CONCLUSIONS

The effort put into SCREENIO has proved worthwhile. It has shown itself to be a valuable tool which meets its design criteria.

By treating screens as single entities it provides us with a coherent approach to the use of screens within applications and the problems encountered in their design use.

It is easy to use, requiring only a single call to SCREENIO within application code.

It is eminently flexible, controlling dynamic output to the screen, PF key handling, validation of fields both individually and in relation to each other, and finally, returning all input from the screen in a convenient collection of variables.

Above all, SCREENIO does not constrain the programmer because it allows EXECUTABLE EXPRESSIONS within the parameter definitions.

We are not, however, complacent. SCREENIO evolved into its present state and is continuing to evolve as the problems associated with screens and their management are reconsidered, new problems encountered or new approaches sought.

(webpage generated: 12 December 2009, 09:55)
