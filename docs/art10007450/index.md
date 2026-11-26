---
vid: '10007450'
title: Object Oriented Programming in APL+Win
authors:
- Bill Rutiser
byline: by William Rutiser, APL2000 (wru@apl2000.com) Talk given at the 2005 APL2000 Users’ Meeting
volume: '22'
issue: '2'
page: '69'
source: trad/v222/rutoops.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- language design
- implementations
---

by William Rutiser, APL2000 ([wru@apl2000.com](mailto:wru@apl2000.com))  
Talk given at the 2005 APL2000 Users’ Meeting
{ .byline }

## Introduction to the MOM Object System

### Elevator description

The MOM object system extends APL with simple basic facilities for object oriented programming, a predominant design and structuring paradigm for modern programming languages. MOM objects are typeless, classless, and inherently polymorphic. They provide syntactic and semantic foundations for future elaboration guided by experience. Entirely internal to APL, MOM objects are not related to <span class="APLU">⎕WI</span> objects, COM objects, ActiveX objects, or any other family of objects.

A MOM object is a simple container enclosing a set of named APL functions and variables. Each MOM object is distinct from every other MOM object. While MOM objects themselves are unnamed, each is associated with a unique *object-reference* value. Object-reference values join numbers and characters as a third fundamental APL data type. Object-references, just like numbers and characters, can be formed into arrays, assigned to variables, passed as parameters, and saved in workspaces. Where meaningful, they are included in the domains of primitive APL functions.

Syntactically, object members are accessed as *objref.member* where *objref* is a variable holding an object reference and member is the name of the member within the object. This use of the dot, “<span class="APLU">.</span>”, is given very high parsing precedence so that this compound name can be used wherever a simple name is allowed. This character retains its traditional meaning and precedence as the inner product operator; the two uses are distinguished by the immediate left context. A function, including user defined functions, causes the dot to be interpreted as inner product. A variable or undefined name will cause the dot to be interpreted as part of a compound name.

Objects are created by the <span class="APLU">⎕MOM</span> system function. Arguments to <span class="APLU">⎕MOM</span> describe the object to be created; its result is a reference to the new object.

During the execution of a member function, the system variable <span class="APLU">⎕MSELF</span> is set to the reference to the containing object. This gives the function access to the object's other members.

Some system functions, such as <span class="APLU">⎕NL</span> and <span class="APLU">⎕CR</span>, are implicit members of every object; they refer only to an object’s members. So, just as <span class="APLU">⎕NL 3</span> gives a list of the functions in the workspace, <span class="APLU">MyThing.⎕NL 3</span> gives a list of the functions in the object referred to by the variable <span class="APLU">MyThing</span>.

## Questions Answered

### What is an object?

In the context of object oriented programming, an *object* is an encapsulation of data and code with certain additional properties that very by language. The important properties that distinguish objects from other programming constructs are:

• **Identity** – Each object can be distinguished from all other objects including otherwise identical objects.

• **State** – The data enclosed in the object. It may change with time and influence the object’s behaviour. A MOM object may contain named members that act just like ordinary APL variables; they can be assigned any APL value.

• **Behaviour** – The effects of invoking the object’s member functions. These effects may depend on the current object’s state and may include changes to the state. MOM objects may contain named members that act just like ordinary APL user defined functions. Also, some system functions are implicit members.

In general, an object is characterized by the combination of its state and behaviour. Some simple, but useful, objects may contain no variables or no functions; they serve as containers. Their behaviour is to preserve or their contents unchanged.

### What is an object reference?

Classical APL is a *value oriented* language. Arrays are values. Function arguments are values. Function results are values. These values are abstract and immutable. They have no identity of their own. APL variables are named things that are temporarily bound to values. The assignment statement replaces the value bound to a name with a different value. We often say that we are assigning something to a variable but Ken Iverson often referred to this as assigning a name to the value.

Most common programming languages, particularly those that are often taught as first languages, are *storage oriented*. The programmer thinks of bits, bytes and words in memory and setting those bits to representations of numbers, characters, and the like. Such languages often have an *indirection* mechanism that allows a program variable to refer to different pieces of memory at different times. In C, the mechanism is called a pointer. In these languages, an object is essentially just a block of storage accessed via a pointer.

Object references are a value oriented abstraction of the pointer. An object references connect variables and expressions with MOM objects. An object reference is a value that identifies a particular object. When assigned to a variable, an object reference value is used to access the members of its object. It is important to remember that the same object reference value can be assigned to several variables at the same time. The reader may find it helpful to visualize an object reference as a length of string tied to a value at one end and an object at the other.

### How can this be demonstrated?

Consider the following code. It can be executed in a clear workspace.

First we create two empty objects.

```
  x← ⎕MOM '' ''
  y← ⎕MOM '' ''
```

Their object references are different.

```
  x ≡ y  ===> 0
  x ≡ x  ===> 1
```

Put something into the first object.

```
  x.data← "This is data assigned thru x"
```

Look at it.

```
  ⎕← x.data ===>  This is data assigned thru x
```

Put something into the second object.

```
  y.data← "data assigned thru y"
```

Display it.

```
  ⎕← y.data ===>  data assigned thru y
```

The first object hasn’t changed.

```
  ⎕← x.data ===>  This is data assigned thru x
```

Change it.

```
  x.data← "New data assigned thru x"
```

Verify that it changed.

```
  ⎕← x.data ===> New data assigned thru x
```

Verify that the second object didn’t change.

```
  ⎕← y.data ===>  data assigned thru y
```

Copy the reference to the first object.

```
  z← x
```

Both x and z refer to the same object.

```
  x ≡ z ===> 1
  ⎕← x.data ===> New data assigned thru x
  ⎕← z.data ===> New data assigned thru x
```

And y still refers to the other object.

```
  x ≡ y ===> 0
  ⎕← y.data ===> data assigned thru y
```

Switch the values assigned to x and y.

```
  x← y
  y← z
```

See that the two variables have swapped objects.

```
  ⎕← x.data ===>  data assigned thru y
  ⎕← y.data ===> New data assigned thru x
```

### What good is all this?

Object oriented ideas permeate modern programming. Most current languages and systems are built on objects or at least the idea of objects. The traditional APL language has no deliberate support for object oriented ideas. Simulation and emulation of objects in APL, while possible, is tedious, awkward and at best dubiously efficient. The MOM object system is a means to experiment with OOP in APL. The author expects benefits in several areas including:

• Organization of large programs. Objects provide a means to compartmentalize functions and data in a workspace.

• Simplification of control flow. Object polymorphism and indirection of reference can be used to reduce or eliminate many conditional tests to reestablish something that another part of the program has already learned.

• Clarification of design. Objects can be used to directly express the relation between problem domain entities and their representation within an application.

• Separation of concerns. An object can provide a single point of definition for a design decision. Code to access and construct data can be encapsulated with the data itself – need not be duplicated at every point in the program that uses the data. 

• Even a rudimentary use of objects should help with increasingly complicated interactions with non-APL programs and host system facilities.

• The difficulty of importing commonly available skills, tools, and literature into the APL world may be reduced.

### Are the current design and implementation of MOM complete?

Emphatically not. The design has reached a useful plateau. We have basic objects, means to access their members, and an incompletely implemented means to create them. The implementation of this design is definitely incomplete:

• While many APL functions handle object references as intended, many still give nonce errors and some will crash the interpreter. 

• Some functions and other facilities appear to work, but misbehave around the edges – things like prototypes of empty arrays involving object references, creation and manipulation of heterogeneous results, and so forth. Display of the state indicator and error reports desperately need to include information about member functions.

• Since this part of the interpreter is still experimental, many implementation quality issues haven’t yet been addressed, including memory use and other performance issues.

### Does MOM’s presence affect the interpreter’s stability?

It’s not supposed to. Some of the slowness of progress can be attributed to the effort of ensuring the existence of adequate test coverage for existing code before changing it to accommodate object references. This testing has revealed an occasional bug that had gone unreported since the dawn of time; so it might be claimed that MOM has improved stability.

If your application doesn’t use objects, they should cause no trouble.

### Can I safely use MOM objects in production code?

That depends of *your definition* of safely<i>.</i> If thorough testing of your application shows no problems and the cost of a failure is only modest embarrassment, go ahead.

### What is polymorphism? Why is it useful?

For our purposes, a group of polymorphic objects have common member names but different behaviour. Consider an application that maintains records for a fleet of vehicles. Cars, buses, trucks, canoes, and airplanes are unlikely to require all the same data items. However, all have an asset number and geographic location. The application is to produce reports to describe the assets in particular locations, ordered by asset numbers. A program that loops thru each of the asset records, examines various fields to determine which items to print and how to format them has a good chance of degenerating into a large tangle of conditionally executed statements. To add a new vehicle type, we risk breaking the reports for existing types.

Now suppose we arrange for each asset to be represented by an object. The objects for one type of vehicle will have just the data members needed for that type. However, each object will have function members that perform needed actions on that data. For example there might be a *FormatAssetReportEntry* member in each object. In the truck objects, the member function is concerned only with truck reports. Similarly, the canoe objects are concerned only with canoes. Such functions can be simple straight line code. All of the decisions were made once and for all when the object was constructed. The program’s main loop could look something like this:

```
:while more_records
  this_vehicle← GetNextVehicleObject...
  its_report_entry← this_vehicle.FormatAsset
ReportEntry
  file Append its_report_entry
:end
```

### What about types, classes, and inheritance?

Formal language support for such things is more useful in statically typed compiled languages where everything must be carefully declared so that the compiler will know what to do. The properties declared for a variable (or function) constrain the ways that the variable can be used and the operations that can be performed with it. Some constraints can be verified by the compiler, turning a potential run-time error into a compile-time error.

Dynamically typed interpreted languages don’t benefit as much from declarative complexity. At this stage of its evolution, MOM relies entirely on *duck typing.* Duck typing considers the actual properties of an object to be its type. These properties are not limited to those that can be declared in mainstream languages; they can include aspects that are even difficult to express in natural language. If an object doesn’t have suitable properties for the way it’s used, the program is in error. Perhaps the wrong object is being used correctly or the correct object is being used incorrectly. Exercising the code shows the conflict. Appropriate testing should reveal a problem before it causes embarrassment or damage. APL programmers have worked this way for forty years.

In the polymorphism example, the important characteristics of a “vehicle”object are that it has a FomatAssetReportEntry member and that the member delivers an appropriate rendition of the object’s state. If it walks like a duck, talks like a duck, and squawks like a duck, it’s a duck. For the original definition, see *[www.rubygarden.org/ruby?DuckTyping](http://www.rubygarden.org/ruby?DuckTyping)*. MOM ducks the typing issue entirely. Also see *[en.wikipedia.org/wiki/Duck\_typing](http://en.wikipedia.org/wiki/Duck_typing)*.

## Reference

### The <span class="APLU">⎕MOM</span> System Function

This function is the sole means for object creation. Its result is a scalar object reference. Its right argument, describes the object to be created. The left argument and other forms of the right argument are reserved for future extensions.

Used nomadically with a scalar zero argument, <span class="APLU">⎕MOM</span> returns a reference to the sole instance of the special *NIL* object. This object has no state or behaviour. It can be used as a placeholder or sentinel value. All references to the null object match each other, and do not match any reference to other objects.

When the right argument is exactly a vector of character vectors, a new object is created. The character vectors in the argument must be the names of currently visible user defined functions and variables. Copies of the named functions and variables become the initial members of the new object. The object’s state is not changed by subsequent erasure of the named functions and variables.

Note well that the argument must be a vector of vectors. An un-nested character vector or a character scalar won’t be accepted. However, empty names are ignored. This statement creates a new empty object.

```
a_new_thing← ⎕MOM '' ''
```

This one creates an object with four members.

```
StrUtils← ⎕MOM 'DLB' 'DEB' 'DTB' 'CONTAINS'
```

### Temporary limitation:

The names in the argument may not contain leading, trailing, or embedded spaces.

### The <span class="APLU">⎕MSELF</span> System Variable

This read-only variable returns the object reference for the most recent incomplete invocation of a member function. When no call to a member function is in progress, <span class="APLU">⎕MSELF</span> yields the special reference to the NIL object.

### System Functions as Members

Several system functions are implicit members of every object. As member functions they refer only to the object’s member functions and variables. Their arguments are analogous to those of the similarly named ordinary system function.

### Temporary limitation:

Only ⎕CR and the monadic variant of <span class="APLU">⎕NL</span> are currently implemented. <span class="APLU">⎕FX</span> is in progress but not yet operational.

### The Match Function

The APL match function ( <span class="APLU">≡</span> ) shows whether two reference values refer to the same object.

## Some Simple Patterns

These pattern descriptions begin with a description of a situation where use of the pattern may be appropriate. The pattern itself is the prescription beginning with the word “Therefore”. The prescription may be followed by examples and other comments, sometimes including a critique of the resulting situation.

More interesting and complex patterns can be found in the literature. Try a google search for “design pattern”.

### Simple Collection Objects

You have an assortment of unstructured and perhaps only loosely related data. Items are needed one at a time in different parts of your program. The data items are stored in individual variables with carefully chosen names. However these names tend to clutter the global environment and the data is not readily handled as a whole.

Therefore, maintain the data items as members of a simple collection object. New items can be added as needed and the object represents the collection itself.

```
height←17⋄width←37⋄brdrcolor←"fuschia"...
win_props←⎕MOM'height' 'width' 'brdr_color'
...
handle← OpenNewWindow win_props
```

Simple collection objects behave a lot like the hash objects in some other languages.

At least with the current implementation, you may encounter performance and memory use when the collection is large. Member names share the symbol table, which has a limited number of slots, with ordinary functions and variables.

### Function Collection Objects

You would like to refactor a large and complicated function into a confederacy of smaller functions with intention revealing names. You are concerned that the relation between the new functions will disappear in a multi-screen <span class="APLU">)fns</span> display. You also need to avoid name collisions with existing and future global functions and want to avoid relying on distracting naming conventions.

Therefore, make the new functions members of a single object.

You could also group related utility or other functions in the same way.

### Parameter Objects

You want to pass some stuff to a function, perhaps even another function. The usual APL technique is to pass the stuff as items of a nested array. The name of the parametric function would be passed as a character vector with the called function using the <span class="APLU">⍎</span> (execute) primitive to actually call the function. For this to work, the calling function and called function must agree on the indices of particular items in the passed array. If several functions are involved, each with somewhat different requirements, it may take some effort to establish a consensus. Also note that a numeric index has very little mnemonic value leading to a need for documentation which tends to get out of date when changes are made.

Therefore, keep the parametric data and functions in an object’s named members and pass the object to the called function, which may return a complicated result in the same object. The updated object may be passed on to other functions.

### Monikers

You want to store an object reference in a context that does not allow object references such as a user defined property on a <span class="APLU">⎕WI</span> or <span class="APLU">⎕NI</span> object. You can store the name of a global variable, the value of which is the object reference. However, both the object references and the places for storage are created dynamically; and there may be many of them. The name of the global variable itself never needs to be used directly.

Therefore, use a utility function, such as <span class="APLU">MonikerFor</span> from the example workspace, that invents a unique name and creates the global variable by assigning the object reference to the invented name.

<pre class="APLU"><i>some.name</i> ⎕WI '∆<i>udp</i>' (MonikerFor <i>theObjectReference</i>)</pre>

This property can then be used in a handler:

<pre class="APLU">t← "⍎(⎕WSELF ⎕WI '∆<i>udp</i>') , '.<i>method</i>'"</pre>

<pre class="APLU"><i>some.name</i> ⎕WI 'onClick' t</pre>

A simple implementation might create a new global variable for each use. A more complicated implementation might seek to reuse names or store the object references in an array. In the later case <span class="APLU">MonikerFor</span>’s result might be something like <span class="APLU">"MONIKERS\[131\]"</span>.

### Factory Functions and Objects

You want to create multiple similar, but not identical, objects at several places in your program. The necessary code to populate the members is repetitive and distracting.

Therefore, isolate this code in a function or object, the sole purpose of which is to create and set up a new object. Large objects, or those that use a number of other objects, may be more conveniently dealt with by several functions encapsulated in an object. The OOP design pattern literature frequently calls these factory objects.

Note that the objects created by a particular factory object are analogous to the instances of a class. With duck typing the factory serves as the class definition.

When several factory functions are used together in various combinations, we have something like multiple-inheritance.

### Delegation

You find that you have multiple objects that contain copies of the same member functions. This duplication seems wasteful, but more importantly, if the common function needs to be changed, it will need to be changed in many places. Furthermore, these functions are related and share a common purpose.

Therefore, create a single new object that contains the sole copy of the functions. Place an object reference to the new object in each of the original objects. Revise the calls to the duplicated functions to call the member functions of the new object. It may be useful to pass the value of <span class="APLU">⎕MSELF</span> so that the now remote functions can access members in the calling object.

This delegation of responsibility to other objects is used as an alternative to inheritance in some object oriented languages.

***
