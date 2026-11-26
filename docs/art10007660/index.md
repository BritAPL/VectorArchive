---
vid: '10007660'
title: VideoSoft FlexGrid Pro with Dyalog APL
authors:
- Jon Sandles
volume: '15'
issue: '3'
page: '93'
source: trad/v153/sandles.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1999
tags:
- user interfaces
- system interfaces
- implementations
---

\[The APL text in this article uses [APL2741](resource/apl2741.zip) (45K download).\]

**VideoSoft FlexGrid Pro with Dyalog APL**  
*by Jon Sandles*

**Introduction**

Following on from Jonathan Barman’s article on the [FlexGrid](barman.htm) I decided to write an article which should highlight the way using this ActiveX control in Dyalog APL differs from using it in APL+Win. I used Dyalog APL 8.2 pre-release, and over time I have experienced quite a lot of resource leaks and crashes, but as each pre-release came out the situation improved; I finally managed to reproduce the worst of the errors and sent the Dyadic helpdesk a diagnostic workspace. The last I heard, they had located the bug and it will be fixed in the final version of 8.2. (By the way, one of the best new features of 8.2 is that it does not GPF anymore! Instead, you get a syserror:999! Plus, the added bonus is that you (normally) get an aplcore, which means you haven’t lost all your changes and there is something to send the helpdesk. Fantastic!)

The basic structure of my article follows Jonathan’s APL+Win one to ease comparison.

<b>Creating a Basic Grid</b>  
Dyadic’s ActiveX control support is quite similar to APL+Win’s in that it attempts to integrate with the existing Windows GUI facilities `(⎕WC/S)`.

```
     ∇ vsFlexGridDemo;headings;data;form;fg
[1]  ⍝ Create a class wrapper called FlexGrid
[2]   'FlexGrid'⎕WC'OCXClass' ':-) VideoSoft FlexGrid Control'
[3]   headings←'Region' 'Product' 'Type' 'Sales'
[4]   data←(4/'France' 'Germany' 'UK'),(12⍴2/'Gold' 'Silver'),(12⍴'I' 'E'),[1.5]⍕¨?12⍴200
[5]   form←'fm'
[6]  ⍝ Create a form
[7]   form ⎕WC'form'('coord' 'pixel')('caption' 'Videosoft FlexGrid Demo')
[8]  ⍝ Put the grid in it
[9]   fg←form,'.FG'
[10]  fg ⎕WC'FlexGrid'('posn' 0 0)('size'(form ⎕WG'size'))
```

Dyalog APL requires you to setup the reference to the ActiveX control before you create any instances of it (see line \[2\]). A resize handler is not required as Dyalog APL’s default behaviour is to resize the control automatically to fit the form. At this point the grid appears in exactly the same way as it would in APL+Win at the same stage.

Continuing in roughly the same way as Jonathan did with APL+Win:

```
[11]  fg ⎕WS'FixedRows' 1
[12]  fg ⎕WS'FixedCols' 1
[13] ⍝ Prepare for the headings
[14]  fg ⎕WS'rows' 1
[15]  fg ⎕WS'cols'(1+⍴headings)
[16] ⍝ Assign the headings .. could not resist a dynamic fn
[17]  (⊂fg)⎕WS¨(⍳⍴headings){('TextMatrix[0;',(⍕⍺),']')⍵}¨headings
[18] ⍝ Align the headings
[19]  fg ⎕WS('Cell[',(⍕flexcpAlignment),';0;0;0;',(⍕⍴headings),']')flexAlignCenterCenter
[20] ⍝ Assign the data (⎕av[10] is tab, one at the front to skip the row labels)
[21]  3 ⎕NQ¨(⊂⊂fg),¨(⊂⊂'AddItem'),¨⊂¨,/⎕AV[10],¨data
```

Dyalog APL exposes an ActiveX control’s properties, methods and events in the same way as a normal class, and in addition they are exposed as documented, unlike APL+Win. (So, no annoying x, X and onX prefixes.)

Like APL+Win, Dyalog APL also insists you must supply all the arguments to a property even if some are optional, but this is the least irritating thing about their implementation of properties that have arguments. These properties, which are really an array extension to the idea of a Visual Basic property, have been implemented using Dyalog APL’s array syntax which must be passed with the property name as a string! (see line \[17\]). To explain what I mean I need an example. Consider the Visual Basic syntax for setting the text heading (row 0) of column 1:

`TextMatrix(0,1) = headings(1)`

Now consider the APL+Win syntax:

```
'Set' 'xTextMatrix' 0 1 (headings[1])
```

In Dyalog APL you need to do the following:

```
fg ⎕WS 'TextMatrix[0;1]' (headings[1])
```

Which, to a certain extent, makes perfect sense. They have used the APL array notation to make it clear that this is an array property. The unfortunate effect is that the property and its array indices must be passed as a single string, which is a huge overhead when looping and assigning to thousands of cells. It also leads to some ugly looking lines of code.

```
[22] ⍝ Autofit the columns (set the font first!)
[23]  fg ⎕WS'Font' 'MS Sans Serif' 8
[24]  3 ⎕NQ fg'AutoSize' 0,⍴headings
[25]  fg ⎕WS'AllowUserResizing'flexResizeBoth
[26]  fg ⎕WS'ExplorerBar'flexExSortAndMove
```

Here I have deviated a little bit from the script. On line \[24\] I take advantage of FlexGrid’s autosize functionality (mentioned in the APL+Win article in the *Other Things* section). You should be sure to set the font of the data (potentially by cell, if you like!) before you do this. Notice that Dyadic make a clear distinction between properties and methods. Methods are `3 ⎕NQ`ed to be invoked.

The rest of the main function is pretty much the same as in APL+Win:

```
[27]  fg ⎕WS'Event' 'KeyDown' 'onFGKeyDown'
[28] ⍝ This cannot be rejected but is shown as an example
[29]  fg ⎕WS'Event' 'BeforeEdit' 'onFGBeforeEdit'
[30] ⍝ Allow editing
[31]  fg ⎕WS'editable' 1
[32] ⍝ Restrict the product and type to lists
[33]  fg ⎕WS'ColComboList[2]' 'Gold|Silver|Platinum|Iridium'
[34]  fg ⎕WS'ColComboList[3]' 'I',⎕AV[10],'Import|E',⎕AV[10],'Export'
[35] ⍝ Popup an edit box when user clicks country
[36]  fg ⎕WS'ColComboList[1]' '...'
[37]  fg ⎕WS'Event' 'CellButtonClick' 'onCellButtonClick'
     ∇
```

<b>In-Cell Editing</b>  
The implementation of onFGKeyDown is exactly as in APL+Win and is left as an exercise for the reader. I have also enabled editing (line \[31\]) and setup the combo boxes just as Jonathan did. On line \[29\] I have trapped the BeforeEdit event. This is the only reasonable way on the FlexGrid that you can make a cell read only. Jonathan gave an example of doing this in APL+Win where he rejected all editing of the Sales column. The equivalent code in Dyalog APL is:

```
     ∇ msg←onFGBeforeEdit msg
[1]   :If 'Sales'≡(1⊃msg)⎕WG'TextMatrix[0;',(⍕msg[4]),']'
[2]     msg[5]←1
[3]   :EndIf
     ∇
```

But here I got a bit of a surprise. It doesn’t work! The event is not rejected and the editing is allowed. After speaking to Dyadic, it turns out that the event handling from ActiveX controls is not yet fully featured and will be improved in the future. In particular, you cannot **reject** the events. This is a major failing of the current implementation and would probably force you into having to think up quite esoteric workarounds in order to use an ActiveX control like this in anger.

<b>Merge Cells and Outlining</b>  
Once again the code for merging cells and outlining is pretty much the same as in APL+Win. The only differences to look out for are in the formatting of the array properties, and the use of 1 (rather than ¯1) to stand for boolean ‘true’:

```
fg ⎕WS'MergeCol[1]' 1
```

Another exercise for the reader!

<b>Speed</b>  
I replicated Jonathan’s speed experiments and got nearly identical results. Clip was by far the fastest method, and I also found I had to use *enlist* to get optimum performance. For some reason I found I had to reduce the size of the batches in the Clip routine to be a bit smaller than in Jonathan’s, but nothing to worry about too much!

<b>Other Things</b>  
I had no problems with any of the “Other Things” in Jonathan’s review, until I tried the *Scroll Tips* example. The following code should enable a scroll tip to be shown as you scroll the grid up and down.

```
fg ⎕WS'ScrollTips' 1
fg ⎕WS'Event' 'BeforeScrollTip' 'onFGBeforeScrollTip'

     ∇msg←onFGBeforeScrollTip msg
[1]  (1⊃msg)⎕WS'ScrollTipText'('Row : ',⍕msg[3])
     ∇
```

I found that this code didn’t work at all. Sometimes the tip would appear and sometimes it wouldn’t. It certainly never appeared as I was scrolling, which is where it is most useful.

If you trace the code you will find you get **all** the BeforeScrollTip events at once when the user lets go of the scroll bar. This is way too late! Once again, this is a result of Dyadic’s incomplete handling of events from ActiveX controls. The events coming from the ActiveX control are just stuck on the message queue, and hence won’t be processed until Dyalog APL gets a bit of idle time.

<b>Failures</b>  
I have already mentioned a few major failures in my comparison with APL+Win:

- Incomplete event handling makes it difficult to do several essential tasks (e.g. trapping *BeforeEdit* to make cells read only, and handling *BeforeScrollTip* events to display Scroll Tips).
- Strange syntax for setting “array” properties, is difficult to code with and could lead to inefficient code.

Just like Jonathan I had trouble with the following....

- I could not work out how to set images on the grid. In Visual Basic the Image must support *IPictureDisp*. I wasn’t sure how to achieve this from Dyalog APL.
- I too found that the ‘ToolTipText’ property was not exposed by the FlexGrid. Is this a standard property that Visual Basic adds on to the interface? (If so APL should add it on as well.)
- I also didn’t even try to get *BindToArray* to work. And I agree: it would be great if it worked!

**Some new ones …**

- I found that some of the properties that are standard to Visual Basic and to Dyalog APL were not exposed, for example, the *Visible* property. (This one is easy enough to code around as you can get hold of the window handle of the control.)

- There was some confusion with colour parameters. For example, the *BackColor* property is defined in the helpfile as being of type OLE\_COLOR. Dyalog APL allows you to pass parameters defined as OLE\_COLOR as RGB triplets so in this case:

    ```
    fg ⎕WS'BackColor'(255 0 0)
    ```

    results in a lovely red background.

    Some of the other colour properties were not so clear:

    ```
    fg ⎕WS'Cell[6;1;1;2;2]'(0 0 255)
    ```

    Should result in a 2×2 rectangular block of blue cells in the top left hand corner of the grid. When I found it didn’t work I checked the parameter and found that it was Variant (because the Cell property can be used to set a whole range of stuff). Incidentally, the 6 is another magic number to tell the grid which property you are setting; the list of properties appears in the help file, but to get the numbers you either guess, or install VB! Some of the useful ones are:

    - Alignment = 2
    - Background colour = 6
    - Foreground colour = 7
    - Font name = 11
    - Font size = 12
    - Fond bold = 13
    - Font italic = 14

    Anyway, I eventually found the following worked:

    ```
    fg ⎕WS'Cell[6;1;1;2;2]'(256⊥⌽0 0 255)
    ```

    **The moral is: tread carefully with colour parameters!**

- The final failure is on the FlexGrid side. With so much flexibility it was disappointing that the only way to get non-scrolling regions was by using the *FixedRows* and *FixedCols* properties. These are strictly non-editable. There is quite a common requirement for non-scrolling regions that are still editable. I solved the problem by providing a keyboard shortcut to lock a number of columns and another keyboard shortcut to unlock the columns. This means that the users can lock the columns, scroll to the cell they are looking for, and then unlock the columns. Not ideal.

As Jonathan pointed out it would make a great Hackers’ Corner to try and present fixes to some of these problems. Please try! One possible solution is to write a thin ActiveX wrapper in Visual Basic (there is a wizard in VB 6.0 for doing precisely this) which contains workarounds for some of the problems. The one thing I tried this with was to try and pass the data to the ActiveX control in one array and use the Flex Grid’s BindToArray method to populate the grid. I found that I couldn’t pass a VariantArray from Dyalog APL (LIMIT ERROR!), so I gave up! I will continue to work on this technique and if I get good results, I promise to write up my experiences in the future.

<b>Conclusion</b>  
The FlexGrid has a very simple interface that allows you to get very impressive results very quickly. If Dyadic get round to ironing out some issues with their event handling from ActiveX controls, then this sort of control will soon become as popular as, if not more popular than, their own native grid control.

***
