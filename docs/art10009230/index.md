---
vid: '10009230'
title: 'R.Net: A Reverse Polish Interpreter for .Net'
authors:
- Richard Smith
byline: by Richard Smith (richard@redcorona.com)
volume: '20'
issue: '2'
page: '136'
source: trad/v202/rcs202.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2003
tags:
- language design
- implementations
---

*by Richard Smith ([richard@redcorona.com](mailto:richard@redcorona.com))*
{ .byline }

## Introduction

Some of you may already know about the original R, which was essentially my ‘learning exercise’ for Java. For those of you (probably most) who haven’t, I will run through the basic features of the language here. (If you want to try it out as you go through, you can download the latest copy from [http://www.apl385.com/rcs/programs/r/](http://www.apl385.com/rcs/programs/r/).)

Firstly, it is a reverse Polish (or stack-based) interpreter, like PostScript, meaning that everything is pushed onto a stack and functions run on the items at the top of the stack. For example, to perform 3+5 in R, you would type `3 5 +`. This pushes a 3, pushes a 5, and then runs the function `+` on the top two items, pushing the result (8) onto the stack as a result. (In effect, everything only has left arguments.) This is because (a) it is much easier to program, and (b) it is nice to be different. I don’t believe it would be too hard to adapt Roger Hui’s amazing technicolor parse table (*Vector* 9.4, p.85) to get APL syntax.

Secondly, it has native support for vectors (as befits a language written in a house exposed to APL for so many years). For example, try `[3 4][2 3] *` and you will see `(6 12)`. It will scalar extend, too; `5 #i 1 +` will produce `(2 3 4 5 6)` as you would expect.

Finally, because it is in genuine .Net assemblies, you have access to all the classes in the Framework; for example `$System.Console "Hello, world" :WriteLine` will work just as it should.

## Arrays

Arrays in R are bounded by *square brackets*, and can be nested as deeply as you like. You can create heterogeneous arrays – such as `["Richard Smith" 19 "Gilling"]` – but there is little you can do with these apart from use them as a data store. Homogenous arrays (which are vectors, vectors of vectors etc), particularly of numbers, are much more useful as any simple primitives will also work on arbitrarily nested arrays; for example:

```
   [2 3 4 [5 6 7]] [[3 4 5] 1 1 [2 1 [2 1]]] *
 [(6 8 10) 3 4 [10 6 (14 7) ] ]
```

R has the usual array-manipulation features - `#get`, `#take`, `#drop`, `#resize` and `#reverse` do what you would expect. You can also flatten an array with `#apop` (effectively removing one level of nesting), and wrap up a number of values with `#apush`, making it possible to build arrays in the code as well.

## Functions and Variables

Of course no language is practical without being able to store and process data. R’s workhorse equivalent to the assignment arrow is `#def`, which is used in exactly the same way to define variables (`"qq" 13.5 #def`) and functions (`"AddTwo" {2 +} #def`). The difference is that function blocks are delimited by *curly brackets*; any assignment of a function block results in a function. When a token is resolved, a variable will have its content placed on the stack, whereas a function will be run.

You may well want to create multi-line functions, and both the command-line and windowed versions of R let you do this at the session (as long as there is a brace open, anything you type is added to the block). You can also use `#ed` in the windowed version, which lets you edit pre-existing code as well as create new functions.

Function blocks have other uses besides simply being assigned to names, although that is the most common. Control structures also use them, as do operators (e.g. `10 #i {+} #reduce`) and exception handlers (see Using .Net).

## Control Structures

R supports two flow-of-control functions (`#if` and `#ifelse`) and four types of loop (`#for`, `#forall`, `#loop` and `#repeat`). If and if-else blocks should be familiar (for example `1000 > {$System.OutOfMemoryException "You can't ask for that many!" #create #error} #if`), but some of the loops will be less so (except to PostScript aficionados). `#for` takes 3 numbers – a start value, a step and an end value – rather like Basic’s `FOR I = 1 TO 10 STEP 2`. For example:

```
   1 2.5 8 {} #for
 1 3.5 6
```

`#forall` works like a C# `foreach` statement, executing the block once for each item in an array. For example:

```
   [1 2.5 8] {2 +} #forall
 3 4.5 10
```

`#loop` repeats the block indefinitely, and `#repeat` repeats it a given number of times. Now, at this point you may well be saying “Why would you want to run an infinite loop?”, and yes, you can work the CPU quite hard with `{} #loop`. Of course, the answer is that you may well want to keep going until something happens, and when it does you will want to escape. The escape is provided by `#exit`, which leaves the innermost enclosing loop (think `break` in C); you can also use `#continue` which jumps back to the start of the loop (once again, think of the C `continue`).

## Using .Net

Because R.net is in real .Net assemblies, you can get at all the classes Microsoft kindly provides as part of the Framework. The name of a class starts with a $ sign (e.g. `$System.Console`), and you can access static methods and fields directly using *colon syntax*; for example `$System.Console "Hello, world, from R!" :WriteLine` will call the WriteLine method with one parameter, the string *Hello, world, from R!*. If you want to supply more than one parameter, use an array. For example:

```
   "System" #using
   $Int32 ["7F" $Globalization.NumberStyles:HexNumber] :Parse
 127
```

This example also shows that you can reference fields without including a parameter list between the class and the colon (this works for methods with no argument list too), and the `#using` function, similar to C#’s `using` statement, to make use of the `System` namespace.

Of course you will want to do more than just use static members of classes – you also need to be able to instantiate a class (create an object). You do this with the `#create` function, which takes a class and the arguments to its constructor and returns an instance of that class. You can then use colon syntax to get at the non-static members of the object. For example:

```
   "RCS" $System.DateTime [1983 10 04] #create #def

   RCS
04/10/1983 00:00:00
   RCS:Year
1983
   RCS 20:AddYears
04/10/2003 00:00:00
```

## Exception Handling

All errors in R.net are thrown as .Net exceptions. For example:

```
   2 "a" +
System.InvalidCastException: Operands of type String and Integer cannot be passed to Plus
   at R.CoreFunctionality.FastPrimitives.Plus(IEngine engine, Symbol op1, Symbol op2, Boolean invert) in c:\DATA\SharpDevelop\R\CoreFunctionality\FastPrimitives.cs:line 523
   at R.CoreFunctionality.ExposedInterface.Plus(IEngine engine) in c:\DATA\SharpDevelop\R\CoreFunctionality\ExposedInterface.cs:line 23
   at R.Interpreter.Engine.ExecuteBlock(ArrayList tokens) in c:\DATA\SharpDevelop\R\Interpreter\R.cs:line 492
   at Session[1]
```

This is a complete .Net exception stack trace (including debugging info for the actual C# code), followed by the R function stack at the moment the exception was thrown. It may seem trivial here, but if we have an error somewhere deep in our code it can be very useful:

```
System.MissingMemberException: Undefined token .exit
   at R.Interpreter.Engine.ExecSymbol(String name) in c:\DATA\SharpDevelop\R\Interpreter\R.cs:line 405
   at R.Interpreter.Engine.ExecuteBlock(ArrayList tokens) in c:\DATA\SharpDevelop\R\Interpreter\R.cs:line 489
   at #if[1]
   at #loop[2]
   at Eratos[13]
   at Session[1]
```

This also means that exceptions from external calls fit into the R error model, and produce a similar stack trace:

```
   $System.Int32 "hello" :Parse
System.FormatException: Input string was not in a correct format.
   at R.Interpreter.Engine.ExecuteBlock(ArrayList tokens) in c:\DATA\SharpDevelop\R\Interpreter\R.cs:line 573
   at Session[1]
```

Now, as well as seeing what you did wrong, you might want to catch the exception and do some other processing. This is accomplished with the `#trap` function, which works in a similar way to the `try` ... `catch` blocks in other languages – you can trap for a number of different exceptions and provide handlers for each. A good example is reading properties that are supposed to be numbers, where you might do something like `{ "num" $Int32 str :Parse #def } [[$FormatException { "num" 0 #def}]] #trap` to use a default value where it was not numeric. Or you might want a file loading routine like this:

```
"LoadFile" {
 // Load a text file and return its content
 #pushdict
 "fnm" #exch #def
 {
  "sr" $IO.File fnm :OpenText #def
  "res" "" #def
  {
   "thisline" sr:ReadLine #def
   thisline #null = {#exit}{"res" res thisline ; '\n' ; #def} #ifelse
  } #loop
  res
 } [ [$IO.FileNotFoundException { "File " fnm ; " was not found" ; #output "" }]
   [$Exception {"Unexpected error occured!" #output ""} ] ] #trap
 #popdict } #def
```

## Dictionaries

The `LoadFile` example also shows how scope is managed in R. Instead of having localisation at the function level, it uses *dictionary scope* (like PostScript), where names are saved in the topmost dictionary. The `#pushdict` function pushes a new dictionary onto the top of the stack, effectively localising everything following it, and `#popdict` removes the dictionary *and its contents*, leaving the symbol table in the state it was before `#pushdict` was run.

This means that you can control (and see) exactly where localised areas of code are; however, it has a couple of bad points – firstly, if an error occurs inside a local region, you have to manually `#popdict` the dictionaries; and secondly, it is not possible to save data in the base dictionary from within a local region (you have to leave it on the stack until the region is left, and then save it). Neither of these drawbacks is terribly severe.

## Namespaces

R.net supports ‘shallow’ namespaces, i.e. there is only one symbol table and system state, which pervades all the namespaces. In fact a namespace is just a way of avoiding name clashes, and a namespace exists only because it contains data. For example, type `"one.qq" 23 #def` and you will automatically create a `one` namespace (it appears in the tree in the IDE); erase the variable and it will disappear again.

A function can call functions and variables in other namespaces by using a relative path between them (using dots to separate namespaces, `@` for root and `@@` for parent) – for example:

```
   "base" { one.fn } #def
   "one.fn" { two.fn } #def
   "one.two.fn" { @@.var } #def
   "one.var" 17 #def
   base
 17
```

You can move around the namespace tree using `#cs` (similar to Dyalog’s `)cs`), and find out where you are with `#curns`. For example, you could have coded the previous example as:

```
   "base" { one.fn } #def
   "one" #cs
   "fn" { two.fn } #def
   "two" #cs
   "fn" { @@.var } #def
   "@@.var" 17 #def
   "@" #cs
   base
 17
```

You can see what namespaces are defined by using `#spaces`, or by looking at the tree in the IDE.

## Loading and Saving

R uses scripts as its storage system, and a script is just a plain text file containing executable R code. For example, here is a script which gets prime numbers by a very well-known route:

```
// R Script created by the ScriptWriter 2.1
// Saved at 07/09/2003 20:48:39

"@" #cs
"Eratos" {#pushdict
"max" #exch #def

"primes" [1] #def
"candidates" max #i 1 #drop #def

{
 candidates #size [0] = {#exit} #if
 "check" candidates 1 #get #def
 check "" ;#setstatus
 "primes" primes check , #def
 "candidates" 0 candidates check % < candidates #exch #compress #def
} #loop

primes
#popdict} #def
```

As you can see, this one was actually saved by the ScriptWriter (the `#save` function in the interpreter), which saves all functions and variables present in a script file. You can also see that it is perfectly possible to edit the scripts in a standard text editor. To run a script, use the `#run` function.

Of course, there is no reason why a script cannot contain executable code as well as (or instead of) `#def` calls – in this case the code will be run when `#run` is executed, and then discarded.

## Conclusion

R.net has certainly fulfilled its primary objective, which was to teach me how to use C# and get to grips with the .Net Framework. I think it also does quite well on its secondary, namely to be a moderately useful programming tool. With its support for vectors and native calls to the Framework it fills a gap which is currently unfilled, since although Dyadic are moving towards .Net they do not yet have a fully managed, small (the whole of R is only 220Kb, 50Kb of which is the documentation) interpreter in which it is easy to call the rest of the Framework.

There are a few things still on the wish list – support for delegates, multidimensional arrays and deep arrays in the interface to the Framework, images in the treeview in the IDE (although this is actually SharpDevelop’s bug!) and better resumption of execution after an error – but overall I am pleased with it.

20th September 2003

***
