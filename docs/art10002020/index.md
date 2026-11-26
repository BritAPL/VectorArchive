---
vid: '10002020'
title: 'Hacker’s Corner: Ghost variables'
authors:
- Ray Cannon
byline: by Ray Cannon
volume: '18'
issue: '3'
page: '90'
source: trad/v183/hack183.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2002
tags:
- programming techniques
- language design
---

*by Ray Cannon*
{ .byline }

*As I was going up the stairs,  
I met a man, who was not there,  
He was not there again today,  
I do wish he would go away.*

## How to emulate “Persistent Local Variables” in Dyalog APL/W.

Stephen Taylor’s article on “Persistent Local Variables”, and recent exchanges in the “DFNS discussion group” have prompted me into writing these notes.

In APL, every time a function is called, any LOCAL variable defined within the function needs to be assigned before it can be used. That is on subsequent calls, the function retains no memory of its local variables’ previous values. Outside the function, these local variables have no presence.

However, if the function refers to a GLOBAL variable, once initially assigned, no further assignments are required, and values are maintained between calls. Outside the function, these global variables do have a presence, and can be “molested”.

It would be very useful if we could define a third type of variable, which although local to the function (*i.e.* have no presence outside the function) could retain their value between calls. This is what is meant by the term “Persistent Local Variable”.

As way of an example consider the dynamic function `Next`:

```
Next←{
     ⍵≢'':⍺.next←⍵      ⍝ Assign a new starting value
     ⍺{⍺.next}⍺.next+←1 ⍝ Return incremented value
 }
```

When called, its left argument is a reference to a namespace, in which it stores its GLOBAL variable “next”. Its right hand argument may be either a numeric which it uses to initialise itself, or the empty text vector `''`, in which case, it increments the value of the global variable, and returns that value as its result.

```
                    ⎕SE  Next 0
                    ⎕SE  Next ''
1
                    ⎕SE  Next ''
2
```

Note that the global variable “next” is located in the quadSE namespace:

```
                    ⎕SE.⎕nl 2
next
                    ⎕SE.next
2
```

Now Dyalog APL allows one to assign functions and derived functions. So I can “glue” the quadSE namespace onto the `Next` function, and call it `NextOne`:

```
                 NextOne←(⎕se)∘Next
                 NextOne 10
                 NextOne ''
11
                 NextOne ''
12
```

But this is still using the same global variable

```
                 ⎕SE.next
12
```

However, if instead of using an existing NAMED namespace, I create an UNNAMED namespace

```
                 NextTo←(⎕NS'')∘Next
                 NextTo 50
                 NextTo ''
51
                 NextTo ''
52
```

The global variable next exists in an UN-NAMED namespace to which I have NO access except via the derived function `NextTo`.

Note that if I create another Un-Named namespace “It” does not effect the first UN-NAMED namespace:

```
                 NextFree←(⎕NS'')∘Next
                 NextFree ¯50
                 NextFree ''
¯49
                 NextFree ''
¯48
                 NextTo ''
53
                 NextOne ''
3
```

So there we have it, outside the function, a variable that has no presence, but whose value persists from one call to the next.

`NextTo` and `NextFree` both have their own separate copy of a “GLOBAL” (but invisible) variable called `#.[Namespace].next`. One could describe them as functions “HAUNTED” by their “GHOST” variables.

Since these GHOST variables are really GLOBAL, one should be able to share them between two functions. (In FORTRAN I think this is described as COMMON data.)

Well, since “Real programmers can write FORTRAN in any language”….

Operator “attach” should do the trick:

```
a(aa attach ww)w
 ⍎(1⊃a),'←w∘aa'
 ⍎(2⊃a),'←w∘ww'
```

```
'PUT' 'GET'( {⍺.data←⍵}   attach   {⍺.data}) ⎕NS''
```

This creates functions `Put` and `a` which share the same COMMON but private variable `#.[Namespace].data`

```
       Put 1 2 3
       Get''
1 2 3
       Put 'Hello Sprite World'
       Get ''
'Hello Sprite World'
```

***
