---
vid: '10500950'
title: Function design
authors:
- Kai Jaeger
byline: by Kai Jaeger (kai@aplteam.com)
volume: '25'
issue: '4'
page: '90'
online: '2012-10-21'
editor: Kai Jaeger
description: Discusses function design and how to pass parameters
abstract: Designing functions is something many APLers don't pay much attention to. They just carry on. This article covers some of the topics associated with function design. In particular it discusses different ways of how to pass parameters, when (and why) to create a direct function (dfn) and when a traditional function (tfn) and why honouring the DRY principle (don't repeat yourself) might be a good idea.
source: content/printed/254/jaeger.htm
converter: vecrescue 0.1.0
issue_tab: n4-november-2012
tags:
- development practice
- programming techniques
---

by Kai Jaeger (kai@aplteam.com)
{ .byline }

Designing functions is something many APLers don't pay much attention to. They just carry on. This article covers some of the topics associated with function design. In particular it discusses different ways of how to pass parameters, when (and why) to create a direct function (dfn) and when a traditional function (tfn) and why honouring the DRY principle (don't repeat yourself) might be a good idea.
{ .abstract }

## Passing parameters to functions

Passing parameters is something people tend to spend little time on. It's so natural to pass some data as an argument, what's there to ponder about? Well, a lot actually. There are so many different ways to do this that it is worthwhile to figure out what's best for certain circumstances.

Note that the techniques discussed rely on Dyalog because they rely on namespaces. They also assume `⎕IO←0` and `⎕ML←3` .

### The early days

Passing parameters in the first versions of APL was a serious limitation to the language: there was just a right argument or a left and a right argument, and one could only pass simple arrays. This is sufficient for problems that are somehow “mathematically oriented”. For normal programming tasks however we more often than not need more than just 2 parameters.

### Nested Arrays

Only with the introduction of nested arrays did APL become a real programming language: nested arrays made it possible to pass as many parameters as needed.

### Mandatory and optional parameters

Often we can make a difference between parameters which are mandatory and those which are optional. In the past I often tended to provide mandatory parameters as the right argument and optional ones as the left argument. How to provide the mandatory parameters seemed to be obvious: define a sequence, often called fixed parameters.

### Name-value pairs

For optional parameters it is less obvious. Fixed parameters are naturally not an option but what about name-value pairs? This allows us to specify a vector of two-item arrays like this one:

```
      parms←''
      parms,←⊂ 'hide' 1
      parms,←⊂ 'workdir' 'C:\App'
      parms,←⊂ 'debug' 0
```

Let’s assume that we have a function `Foo` which takes just one mandatory parameter. The name-value pairs we’ve just defined allow us to specify them as optional parameters:

```
      parms Foo someArray
```

Alternatively, we can provide no optional parameters at all:

```
      Foo someArray
```

That’s all well and good but there are a couple of obstacles we have to deal with. First of all, if we specify just one optional parameter:

```
      (⊂'hide' 1) Foo someArray
```

we must enclose the single pair to ensure conformability. You might think that this can be avoided by investigating the left argument within `Foo` and deal with it appropriately depending on its depth but you might find this surprisingly difficult and error prone depending on the parameters you expect: if the second item of a name-value pair can be nested itself trouble is looming.

Two scalars also pose a problem:

```
      (⊂'a' 1) Foo someArray
```

It is also not particularly easy to check the parameters for being valid. What about case sensitivity? Shall “hide” and “Hide” both be treated as a valid name for a certain parameter? Last but not least you have to assign the values to variables inside `Foo`.

Nothing of this is particularly laborious but you need to care about these problems.

### Namespaces to the rescue

Now there is another approach which is ridding us of all these problems effortlessly. Look at this:

```
      parms←⎕NS' '
      parms.hide←1
      parms.workdir←'C:\App'
      parms.debug←0
```

In the first line we create a new unnamed namespace and assign it to parms. To rephrase it: parms is now a reference pointing to an empty namespace. We then create variables inside this namespace with the appropriate values.

Now we can pass this namespace as left argument:

```
parms Foo someArray
```

Looking at this from inside Foo there are some differences:

- We don’t need to bother about the format of the left argument.
- We don’t need to establish variables – we already have them.
- As a bonus the parameters are separated from other local variables in the function.

All this makes this solution significantly more attractive than name-value pairs, but there is even more. Look at this function:

```
      [0]  parms←CreateParmsForFoo
      [1]  :Access Public Shared
      [2] ⍝ Creates a namespace with default values for all
      [3] ⍝ optional parameters of method Foo
      [4]  parms←⎕NS''
      [5]  parms.hide←1
      [6]  parms.workdir←'C:\App'
      [7]  parms.debug←0
```

Note that the first line makes this a public shared method. Now let us assume that `CreateParmsForFoo` and `Foo` are methods of a class “Sample”. Let’s also assume that `Foo` has a line `:Access Public Shared` . When you consider calling the method `Foo` with a certain parameter different from its default you can now do this:

```
      myParms←#.Sample.CreateParmsForFoo
      myParms.hide←0
      myParms Foo …
```

From a user’s point of view this is not different except that a) she cannot forget to enclose a single pair, b) she can stop worrying when passing two scalars. Most importantly, if she is interested in the default values processed by `Foo` she doesn’t need to look into `Foo` anymore, or combing through documentation. Inspecting the contents of the namespace gives the answer. In short: life is easier now from a caller’s point of view.

Other examples are overloaded constructors of classes. Depending on the number of parameters as well as the data types of the parameters somehow automatically the correct constructor is executed. That sounds nice but it has a clear drawback: it makes reading and understanding a statement like the following one harder than necessary not only because of the positional parameters provided but also because there is no easy way to find out the defaults for the parameters not specified:

```
      ⎕NEW Sample (‘hello’ 1 (3 4) ‘universe’)
```

Obviously these statements:

```
      myParms←#.Sample.CreateParmsForFoo
      myParms.hide←0
      myParms Foo …
      ⎕NEW Sample (,⊂myParms)
```

are more readable but also provide an easy way to inspect the defaults.

### Adding a “List” method

We can make the caller’s life even more comfortable by adding one more line to `CreateParmsForFoo` :

```
      …
      [7]  parms.debug←0
      [8]  parms.⎕FX'r←List' 'r←{⍵,⍪⍎¨⍵}~∘'' ''¨↓⎕nl 2'
```

Now after assigning the result of the function to a variable my one can say:

```
      my.List
debug         0
hide          1
workdir  C:\App
```

That is certainly a nice way to investigate the contents of the parameter space.

### Checking optional parameters

For a programmer it is also more convenient to deal with namespaces rather than name-value pairs. Think of how to make sure that such a namespace contains just the variables it’s supposed to contain. Our method could achieve that quite simply:

```
      [0] {optional}Foo array;f;b;msg;l
      [1]  :Access Public Shared
      [2]  :If 0=⎕NC'optional'
      [3]    optional←CreateParmsForFoo
      [4]  :ElseIf 0<1⊃⍴optional.⎕NL 2
      [5]    l←~∘' '¨↓optional.⎕NL 2
      [6]  :AndIf 1∊b←~l∊CreateParmsForFoo.List[;0]
      [7]    msg←'Invalid optional parameter',((1<+/b)/'s'),': '
      [8]    11 ⎕SIGNAL⍨msg,↑{⍺,',',⍵}/b/~∘' '¨↓optional.⎕NL 2
      [9]  :EndIf
      [10] ⍝  ...
```

- Line 3 creates optional with default settings in case no left argument was provided.
- Line 4 checks whether optional is empty. If it is not…
- Line 6 checks whether we have a problem. If we have one than b (for Boolean) can be used as a mask.
- Line 7 compiles a proper message and…
- Line 8 adds the name(s) of the problem case(s), performs some formatting gymnastics and throws an error.

If we now do this:

```
      parms←CreateParmsForFoo
      parms.whatIsThat←'?'
      parms.Hide←0
      parms Foo 1
```

we get

```
Invalid optional parameters: Hide,whatIsThat
```

because the parameter “Hide” was misspelled.

Obviously this way of specifying parameters has advantages for both, the user of a function (or method) as well as the implementer of it.

### References as parameters

So far we have restricted the stuff a parameter space can contain to variables. There is a good reason to lift that restriction. Think of a parameter refToUtils. Obviously his parameter is expected to be a reference to a namespace that holds utilities. Presumably the default is just #. In order to deal with references we need to change the `List` function so that it also reports references:

```
      parms.⎕FX'r←List' 'r←{⍵,[0.5]⍎¨⍵}'' ''~¨⍨↓⎕nl 2 9'
```

### Constants

Sometimes you might want to add a “parameter” which can’t actually be changed because its value depends on the environment and can be worked out by the CreateParmsFor function itself. Why would you want to add this? To give the programmer an easy way to actually look at the information. That is not only convenient; it also makes clear that this value is taken into account by the program the parameter space is going to be fed to. But the user must not change it, so a variable is not appropriate. APL has no concept of what is called a Constant in most other programming languages. Niladic functions to the rescue:

```
      ∇r←IS_DEVELOPMENT
      [1]  r←'Development'≡3⊃'#'⎕WG'APLVersion'
      ∇
```

Strictly speaking this is not a constant, but a niladic function poses convincingly as a constant. In most other programming languages names for constants use uppercase letters; that makes them easy to recognize. This seemed to be a good idea so I adopted this here.

### Enhancing “List”

To include our pseudo-constants we need to make sure that List is taking functions into account but without List itself, therefore we localize List:

```
        parms.⎕FX'r←List;List' 'r←{⍵,[0.5]⍎¨⍵}'' ''~¨⍨↓⎕nl 2 3 9'
```

Finally the information what name class a certain parameter actually belongs to is sometimes valuable, so we add it to the result returned by List:

```
      parms.⎕FX 'r←List;List' 'r←{(⍵,[0.5]⎕NC ⍵),⍎¨⍵}'' ''~¨⍨↓⎕NL 2 3 9'
```

Our function would now look like this:

```
      r←CreateParmsForFoo
       :Access Public Shared
      ⍝ Creates a namespace with default values for all
      ⍝ optional parameters of method Foo
       r←⎕NS''
       r.hide←1
       r.workdir←'C:\App'
       r.debug←0
       r.⎕FX'r←IS_DEVELOPMENT' 'r←''D''≡3 1⊃''#''⎕WG''APLVersion'''
       r.refToUtils←#
       fns←'r←List;List' 'r←{⍵,⍪⍎¨⍵}~∘'' ''¨↓⎕nl 2 3 9'
       r.⎕FX fns
```

Let’s check:

```
      parms←CreateParmsForFoo
      parms.List
 IS_DEVELOPMENT  3.1       1
 debug           2.1       0
 hide            2.1       1
 refToUtils      9.2       #
 workdir         2.1  C:\App
```

That’s exactly what we are looking for; job done.

## Direct functions or traditional functions?

When direct functions were introduced by Dyalog my first thought was something along the lines of “well, nice, but there are more important things we need right now.” That was a long time ago; we called them dynamic functions back then. Boy has my opinion changed since then! Today about 90% of the functions I write are direct functions. Why is that?

### Name scope

First of all it’s about name scope: when a variable is created in the direct function (also called dfns or curlies because of the curly brackets `{}` ) all the variables created inside that function are local by default. In order to create a true global you have to say:

```
              ⎕THIS.MyGlobal←'something'
```

Even better: “local” really means local: in traditional functions every local variable created in a function can be seen be functions called within that function like a global which is the reason why they are sometimes called semi-globals. There is no such problem in dfns: local really means local.

You think that’s not that important? Allow me to tell you a story emphasizing the fact that it is important: In the early eighties I worked for a client who ran VSAPL on a mainframe. My task was to maintain and enhance a large program written by somebody who had moved on. One day I inserted a new comment into the main function, the one executed by `⎕LX` . An hour or so later I got feedback from users claiming that the results were rubbish. It took me a while to make the connection: was it me who was to blame for the problem because I added the comment line? A short investigation showed that this was unlikely: the program did not do any branching with `→`, so how could the new comment line make a difference?

I went for the pragmatic approach anyway and removed the comment line. Then I restarted the program and asked the users. They reported that now the program’s results were back on track.

But why was that? As it turned out the function had a couple of labels defined, despite not using branching in that function. That was not unusual in these days: the original author used labels not only for branching but also for documentation purposes. Now in a traditional APL function a label is simply an integer variable and its value is the line number, and they are also semi-globals.

As it turned out deep in the calling hierarchy there was a function which used branching, and it tried to jump to a label with a name also used in the main function. Unfortunately the programmer forgot to specify the label. Rather than causing a VALUE ERROR APL managed to find a variable with that very name on the stack. Finally the value of that variable (=the line number) was good enough to let the function with the missing label do its job.

When I introduced my comment line, the label got a new value; in the function with the missing label that had a consequence: one line that was executed in the past was now simply ignored. Unfortunately this did not make the program fail, it rather created wrong results. This is a perfect example why local should really mean local.

### Drawbacks of dfns

Unfortunately direct functions come with drawbacks, some of which could be removed easily by Dyalog:

- There are no stop vectors. Despite the editor pretending otherwise dfns do not honour stop vectors. There are rumours that this will be fixed in version 13.2.
- If in a direct function a value is assigned twice to a variable “foo” then the second assignment effectively creates a new variable “foo” shadowing the first one. “So what?!” you might ask, but this has a nasty side effect when you try to watch changes made to a variable by opening an edit window on that variable: that works brilliantly with tfns but not at all with dfns: the new value is not shown because a new variable is created, and the editor does not care about this new variable.
- Sometimes people complain that it is a disadvantage that you cannot have named arguments with dfns, therefore reducing readability; right, good point, but nothing stops you from saying `(parm1 parms anotherParm)←⍵` in the first line of your dfns which has the same effect. Like in tfns these three variables are local by definition, so everything is okay.
- Sometimes a `:For` loop or a `:Select` statement has its merits, in particular when it comes to debugging, but in those rare cases one can still write a tfn.

## DRY and functional programming

DRY stands for “don’t repeat yourself”. It means that any piece of information should only have just one representation in an application. Easy to understand examples are:

- The name of an application which is repeatedly shown in window captions.
- The main key of all Windows Registry entries used in an application.

Rather than repeating these pieces of information over and over again it should come from just one source, be it a variable or the result of a function call. In these cases the advantages are obvious: in case of a change one needs to change just one single line in the application and the job is done.

However, there are less obvious cases: when a certain piece of code gets used in two or more different places then this is already good enough a reason to put it into a separate function. Let’s discuss this by looking at a real life example.

### Calculating index positions

With the help of direct functions calculating index positions can be done by the expression `{⍵/⍳⍴⍵}`. Is assigning this expression to a function name a good idea or not? According to the DRY principle the answer must be yes. Reality proved that this is true.

There was a longstanding bug in Dyalog: prior to version 13.0 the expression `⍳⍬` returned `⎕IO` when it should have returned `⊂⍬` . You may think “so what?!” but this can have quite a dramatic impact: when the expression `{⍵/⍳⍴⍵}` gets a scalar as argument, the result in 12.1 is very different from that in 13.0. With `⎕IO←0` it is:

```
      12.1:     0  ←→ {⍵/⍳⍴⍵} 1
      13.0:     ⊂⍬ ←→ {⍵/⍳⍴⍵} 1
```

In most applications we have to make sure that the expression continues to return `⎕IO` rather than an enclosed empty vector. Now when there is a function defined like this one:

```
      Where←{⍵/⍳⍴⍵}
```

then obviously you change that function to

```
      Where←{⍵/⍳⍴,⍵}
```

and your application is ready for 13.0 in this respect. Instead you might perform a search in a big application and find plenty of places where the expression is used. All of them need to be changed.

By following the DRY principle you are going to write code that is better to maintain, but there is a second advantage: your functions tend to get smaller, and that’s a good thing.

### Where is the DRY principle coming from?

This is what the Wikipedia has to say:

> In software engineering, Don't Repeat Yourself (DRY) is a principle of software development aimed at reducing repetition of information of all kinds, especially useful in multi-tier architectures. The DRY principle is stated as "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

The principle has been formulated by Andy Hunt and Dave Thomas in their book *The Pragmatic Programmer*. \[…\] When the DRY principle is applied successfully, a modification of any single element of a system does not require a change in other logically-unrelated elements. Additionally, elements that are logically related all change predictably and uniformly, and are thus kept in sync.

### Is the DRY principle globally valid?

Well, some big shots claim that it is not: a given test case should create its environment, execute the test cases and then tidy up. Actually it should tidy up (because there might be leftovers from a former test case that failed), create its environment, perform the test and finally tidy up again.

This allows one to read the test case from top to bottom and understand what it’s supposed to do and how it will try to do it. At the same time it’s likely to violate the DRY principle.

Of course one has to make exceptions: if many or all test cases need a particular environment and creating that is costly than this should be created upfront and deleted after having executed the last test case. A typical example is creating a (potentially big) data base.

### Function size

Functions should be small, and most people would agree with this. But what exactly should go into a function? Following the DRY principle will quite often introduce plenty of small functions into an application by separating certain parts of the code, but that alone is not enough.

Often programmers populate a function with statements that are supposed to be executed together in terms of time. In all honesty, that is not good enough a reason to put them together! A good advice I once got from an old-hand is that when you have difficulties to find a proper name for a function then that function is a candidate for splitting it into several functions.

Statements should be combined into a function when they serve a certain task. That also makes it easier to find good names.

### Names

Many people have strong views on this, but it’s probably fair to say that there is some common ground between most people.

### Avoid abbreviations

Names should clearly advertise what a function is doing. With autocomplete at your fingertips there is no good reason to avoid clear self-explaining names. Actually, there are not even bad reasons. Abbreviations are fine when they are well-established: programmers would know what RC stands for, so that is fine. But `cntrs` rather than `countries` does not save much and certainly makes a program harder to read. It can also become an obstacle when somebody scans code for “country”.

The trouble is that within a program you are just about to write you might find such abbreviations handy, and reading as well as understanding them is not a problem at all. Right, but when another programmer will take over one day she might be in more trouble than necessary. Even you might run into this: I myself more than once stopped looking at some code for a while, and when I came back 2 years later I found the names hard to understand.

### Meaningful names

There are programmers who always name the first local variable needed in a program with `a` and the next one will be `b` and so on. Bad move. It saves you the time to find a good name which quite often is a difficult thing to do, but it makes it harder to read.

### Reasonable exceptions

If a function has only a few lines however then nothing is wrong with a statement like this:

```
      lc←GetAllCountryNames ⍬       ⍝ List of Countries
```

and then using the name `lc` later in that function. It’s easy to read and understand, and because the function is short the assignment line will always be visible, so there is nothing wrong with this technique.

### Side effects

One of the fundamental ideas of functional programming is to avoid side effects. That’s why functional programming languages are back in the mainstream: side effects are deadly in multi-threaded (or multi-cored) programs.

APL is not only a functional programming language: it was the very first functional programming language ever. Worth to remember because these days I keep reading claims that Lisp was the first one.

Although the general design of dfns makes it almost impossible to implement functions that have unwanted side effects it is of course still possible to implement dfns that actually have side effects. However, it is certainly a good idea to try hard to avoid this or at least to make the fact obvious.

For example, it is certainly a good idea to emphasize the fact that in a dfn a global variable is updated. Although the following statement would work in a dfn if there is a global variable `Buffer` (otherwise it's a VALUE ERROR):

```
      Buffer,←⊂
```

It might be better to say:

```
      ⎕THIS.Buffer,←⊂
```

which makes it obvious that a global is involved here.

## Conclusion

There is certainly more to say about the design of functions in general and how we pass parameters to them. Discussing the issue with my fellow colleagues turned out to be surprisingly difficult. It seems that APLers have particular difficulties to agree on something, or shall I say anything? That’s a bit strange and certainly not helpful. It might be one of the reasons why APL is not as successful as it deserves to be because it stops us from doing things that other communities have so easily achieved, for example common libraries of utilities, all developed following certain style guidelines accepted by that community. That is certainly true for the Ruby and the Python community, and it is likely to be true in other communities as well. I would love to see a discussion regarding this in the APL world, a discussion that would ideally result in a paper “APL Style Guidelines” we can hand over to newcomers one day.
