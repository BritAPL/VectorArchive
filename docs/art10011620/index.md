---
vid: '10011620'
title: 'New Tricks for Old Dogs: making sense of classes and namespaces'
subtitle: Making sense of Classes and Namespaces
authors:
- Adrian Smith
byline: Adrian Smithadrian@causeway.co.uk
volume: '23'
issue: '1'
page: '113'
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
source: trad/v231/smith.htm
converter: vecrescue 0.1.0
issue_tab: n12-january-2008
tags:
- language design
- development practice
---

Making sense of Classes and Namespaces
{ .subtitle }

Adrian Smith  
[adrian@causeway.co.uk](mailto:adrian@causeway.co.uk)
{ .byline }

There is a well-worn proverb in English that “You can’t teach an old dog new tricks”, which is not entirely true when it comes to APLers. It just takes a little more time and patience as we get older. In my case, I have been avoiding *Classes* in favour of *Namespaces* as long as I can, on the grounds that I understand what a namespace is, and I know how to make it work for me.

*Classes* looked quite attractive when Morten first showed them in 2005, and they make for great demos. However I have always had a feeling that trying to maintain any reasonable volume of code in a **script** would be anathema to my APL soul. SharpPlot (aka RainPro) has 585 functions and totals around 20,000 lines of APL. I know what most of the functions are called, and I have Shift+F1 set up to `')ed #.SharpPlot.'` at which point autocomplete can help me type stuff. I can start anywhere in the calling tree, and ShiftEnter my way to where I want to be. The very idea of giving all this up in favour of a dumb script makes me want to hide in a corner and whimper.

But I do like the idea of *instances* which show up only the *public functions*, and have assignable *properties* and maybe global variables. I definitely like the idea that I can write some testing code for the SharpPlot namespace that will look just like the code that runs against the true .Net DLL version. This way I can run the same test against both versions, check the speed, and check that I get exactly the same chart back in either case. So what to do. In a weak moment, I decided to read the instructions. This had no effect whatsoever, so I went out and hit a bucket of golf balls. This somehow triggered a mad idea, so I came home and read the instructions again. This time, a couple of things made sense.

## Enlightenment – on Reading the Instructions

If you take time to browse through the “Latest Enhancements” help file that came with Dyalog 11 at this year’s conference, you will see that you can use `⎕FIX` to turn a *script* (vector of text vectors) into a *class*. What had escaped me on first reading was the fact that if the script omitted a class name, it would create an *unnamed* class that would persist in the workspace only while something was using it. Of course that *something* could be an instance, created by `⎕NEW`. What is more, the script could `:Include` any number of namespaces, and flags such as `:Access Public` in functions in included namespaces would be preserved and taken note of. Wow, thought Adrian, let’s give this a try…

```
      qq←⎕NEW ⎕FIX':Class' ':Include Tester' ':EndClass'
      qq.Hello
Hello, from Tester
```

We can even use `⎕DF` to make it look pretty:

```
      qq
#.[[Unnamed]]
      qq.⎕DF 'myTester'
      qq
myTester
```

This looks very promising – I can use a trivial script to bridge completely over the *class* and effectively just make an instance of my source *namespace*. Oddly, if I copy the namespace in from an earlier version of APL, everything ends up **Public** by default. Refreshing all the functions by refixing them solves the problem, and only functions marked with `:Access Public` show up – this may be a bug in the workspace upgrader, and it is easy to work around if it turns out to be intentional.

Another big benefit of this approach is that there is only one copy of the source code, and that if I change it, all existing instances see the change immediately:

```
      )ed Tester.Hello
      qq.Hello
Hello, from Tester!
```

… note the extra character on the end of the message! All my old habits (like leaving jots in functions to stop them) just work. Functions called via `qq` duly stop. I can edit them, fix up the code, save them and continue to the next jot. When I am done, I really did change the working copy of the code back in the original namespace – which makes me very happy indeed. If there are any horrible snags, I have yet to find them!

So what else do I need to do to make this (very useful) idea into a workable utility? Well, it must scan the source namespace for *Properties* and create the correct stuff in the script so that the temporary class knows which Get and Set functions to call. It should also call the *Constructor* function to make sure any data has been set up correctly to default values, and that any arguments to `⎕NEW` are passed down when the class is instanced. It is also an excellent idea to have a *default constructor* which Dyalog runs for you when you overtake an array of instances. Time to make a start!

## Making it Work for Real

### Properties

These are really just a comfortable syntax for a pair of Set and Get functions that maintain the value of some state variable. They are often trivial (like `SetHeading` and `GetHeading` in SharpPlot) but may do a little validation (in the Setter case) or formatting (in the Getter case) of the internal value. In SharpPlot, I chose to mark them out with comments like `⍝:PSet Heading The main chart heading` which my C# translator uses to make the appropriate markup in the translated class (it also extracts the extra text for the XML that leads to the Visual Studio tips). Alternatively you could simply define a global variable in the namespace that gave the property name, type, description, Getter and Setter for all properties. I quite like the former approach, as maintaining extra data can be a pain, but for SharpPlot I can easily create my property list automatically, so with around two hundred properties to scan for, it will save the script-writing function a lot of work! Use whichever method you prefer, or invent your own.

### Constructors

I had half hoped that if I added `:Implements Constructor` into my namespace initialiser, Dyalog would just run it for me – maybe they should and this is a bug, but I can see that you might get some collisions if you included several namespaces, so maybe it is only fair to have to do this myself!

My convention (following the C# rules) is that the constructor always has the same name as the source namespace, so `Tester.Tester` in the above example. I can easily scan for this name, use `⎕AT` to see if it requires an argument, and add the requisite entries into my script. Something like:

```
      ss←':Class' ':Include Tester' '∇ Create arg'
      …  ':Access Public' ':Implements Constructor'
      …  'Tester arg' '∇ ' ':EndClass'

      qq←⎕NEW (⎕FIX ss) 'Hello'
```

This now correctly calls my own initialiser with the text `Hello` which is fine if I remember to pass an argument to `⎕NEW`, but if I get sloppy:

```
      qq←⎕NEW ⎕FIX ss
LENGTH ERROR
      qq←⎕NEW ⎕FIX ss
     ^
      3↑qq
LENGTH ERROR
      3↑qq
     ^
```

The answer is to provide an extra *Default contructor* which Dyalog can call niladically, and which will call my own initialiser with a sensible default argument:

```
    ∇ CreateDefault
[1]   :Access Public
[2]   :Implements Constructor
[3]   Tester ''
    ∇
```

By adding these lines, I can now have Dyalog make the instance with no data, so it can also overtake the scalar instance to give me a vector of them:

```
       qq←⎕NEW ⎕FIX ss
       (3↑qq).Hello
 Hello, from Tester!  Hello, from Tester!  Hello, from Tester!
```

At last – I have complete control over how overtake does its prototyping! Just using this for vectors of data items (like name/age/salary triples) could make a bit of sense, and it is not madly slow, even with many thousands of ‘records’ in the array. If I am prototyping in APL with a view to compiling the finished application, then speed is the last thing I care about.

Of course, all of this is now well buried in a function called `new` which does the messy stuff where I never need to see it again. Copies are likely to be given away to anyone with a keydisk at APL meetings, and will be posted somewhere on the Vector site by the time this goes public!

## Wrap Up

My feeling is that this is quite a breakthrough. I am most of the way through rebuilding NewLeaf for .Net and I am doing it properly using a very object-based approach. I have an object called a `TextBlock` which knows how to handle formatted text (with font changes, superscripts and so on) very nicely. If I make an object called a `Cell` which is just a `TextBlock` with some additional properties like background colour, then a `Table` simply becomes a matrix of `Calls` and much of the complexity of handling multipage tables just falls away.

The big snag is that debugging this in Dyalog 10 is a pain, as you can only pretend to make instances by copying the entire source namespace. I can’t face moving the code into true V11 Classes, for all the reasons outlined at the top of this note. But I can believe in the idea of typing things like:

```
      lf←new LeafEngine mylayout
```

… and getting a true instance of my `LeafEngine` namespace. I don’t even have to enclose `mylayout`, but that’s another story…

The end.
