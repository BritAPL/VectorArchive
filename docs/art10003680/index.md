---
vid: '10003680'
title: 'Hacker’s Corner: A Special Locator in Dyalog APL/W'
authors:
- Joachim Hoffmann
byline: by Joachim Hoffmann (joho@ping.at)
volume: '16'
issue: '1'
page: '108'
source: trad/v161/hack161.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1999
tags:
- user interfaces
- system interfaces
---

*by Joachim Hoffmann (joho@ping.at)*
{ .byline }

Imagine you have to implement a special locator functionality inside a subform, where the locator should change its cursor to indicate if it is over a valid or invalid target object. So the user should see where it makes sense to drop his locator. Also you want to draw a special locator line, *e.g.* a dogleg instead of a straight line.

I solved this problem with some tricky `⎕NA` calls to the Windows API. On Objects where the locator is started a MouseDown handler returns 0 in order to interrupt the default mouse processing on the start-object. The same callback also sets the mouse capture via User32|SetCapture on the parent form. This has the effect that from now on all mouse events are processed by the parent form. This is where the MouseMove handler of the parent form starts to do its work. It checks on each call if the mouse is waved over a valid target object and sets the appropriate cursor on the parent form. On the MouseUp of the parent form the MouseMove handler is deactivated and the appropriate action is taken depending on where the locator was dropped, or in an even more object-oriented design an EndLocator event could be fired.

And as its MOST IMPORTANT action *it releases the capture* back to default processing (user32|ReleaseCapture) so that the other objects can receive mouse events again. I also set a global `⎕TRAP`, so that in case of error the capture would be released.

```
     ∇ InitQuadNACalls
[1]    ⎕NA'I user32.C32|WindowFromPoint {I I}'
[2]    ⎕NA'I user32.C32|ChildWindowFromPoint I {I I}'
[3]    ⎕NA'I user32.C32|GetCursorPos >{I I}'
[4]    ⎕NA'I user32.C32|ScreenToClient I ={I I}'
[5]
[6]    ⎕NA'I user32.C32|SetCapture I'
[7]    ⎕NA'I user32.C32|ReleaseCapture'
     ∇
```

In the MouseMove handler the object under the cursor needs to be detected. As a prerequisite I have registered the handles of all valid target objects in a global variable at creation time (obj `⎕WG` ‘handle’ after `⎕WC`). The MouseMove handler first queries the cursor position (user32|GetCursorPos), which is then translated from screen coords to client coords of the parent form (user32|ScreenToClient).

```
     ∇ xy←MousePosQna hdl;r
[1]    (r xy)←GetCursorPos 2 ⍝just a dummy argument
[2]    (r xy)←ScreenToClient hdl xy
     ∇
```

The position is then fed in turn into user32|ChildWindowFromPoint (CWFP), in order to query if there is a child window under the cursor. Please beware that CWFP also returns deactivated and/or invisible child windows, which have to be taken care of. (While writing this article I just found a new service called ChildWindowFromPointEx, for which you can now specify whether it should omit hidden or disabled child windows – at least it says so in MSDN Lib April 99). So if CWFP returns a handle, it’s a quick job to check it against the list of valid targets. I had to use CWFP instead of the simpler version WindowFromPoint (WFP), because WFP only returned the handle of the APL Session object.

```
     ∇ formhdl OnMouseMove msg;ps2;chdl;target_type
[1]   ⍝∇ MouseMove-handler on parent form
[2]    ps2←MousePosQna formhdl  ⍝ x/y(!) in parent form
[3]    chdl←ChildWindowFromPoint formhdl ps2
[4]    :If ~0∊⍴chdl  ⍝ if there is a child window
[5]        DrawSpecialLocator ∆ps1,[0.5]⌽ps2
[6]        target_type←formhdl CheckTarget chdl
[7]        SetCursor target_type ⍝ set Cursor prop on parent form
[8]    :Else ⍝ no handle → outside of parent
[9]        SetForbiddenCursor 1
[10]   :End
     ∇
```

Using this technique I could implement quite an efficient locator with hit-testing. This could also be used for a variety of special drag&drop operations.

***
