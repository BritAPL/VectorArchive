---
vid: '10004600'
title: OO for APLers (part 1)
authors:
- Morten Kromberg
byline: by Morten Kromberg, Dyalog Ltd, December 2005
volume: '22'
issue: '1'
page: '56'
source: trad/v221/oop1221.htm
converter: vecrescue 0.1.0
issue_tab: n1-november-2005
tags:
- language design
- implementations
---

by Morten Kromberg, Dyalog Ltd, December 2005
{ .byline }

## Introduction

This introduction has been written as a companion to version 11.0 of Dyalog APL. Our goal is not only to explain the details of new functionality in version 11.0 of Dyalog APL, but also be mildly provocative and entertaining, to convey a flavour of the thinking which is behind the object oriented extensions to APL, and something about how the development team imagines that you might make use of the new features – in the belief that this will make them easier to understand and use.

It is recommended that you have version 11.0 of Dyalog APL available for experiments as you work through the guide, and use it to verify your understanding of the new features as they are introduced.

If you have an electronic copy of this guide, and the “Paste Text as Unicode” option enabled (see Options|Configure|Trace/Edit), you should be able to copy and paste code from the guide into the Dyalog APL editor and session. Alternatively, the folder OO4APL, included with the version 11.0 installation, contains a workspace with the same name as each of the classes and namespaces used in the guide.

## <a id="_Toc117440111"></a>1. Introduction

Version 11.0 of Dyalog APL introduces *Classes* to the APL language. A class is a blueprint from which one or more *Instances* of the class can be created (instances are sometimes also referred to as *Objects).* For example, the following defines a class which implements a simple timeseries analysis module:

<pre class="aplu">:Class TimeSeries

    :Field Public Obs      ⍝ Observed values
    :Field Public Degree←3 ⍝ 3rd degree polynomials by default<br>    ∇ r←PolyFit x;coeffs
      :Access Public
      ⍝ Fit polynomial to observed points and compute values at x 
      coeffs←Obs⌹(⍳⍴Obs)∘.*0,⍳Degree ⍝ Find polynomial coeffs
      r←coeffs+.×⍉x∘.*0,⍳Degree      ⍝ Compute fitted f(x)
    ∇
    ∇ make args
      :Access Public
      :Implements Constructor 
      Obs←args
    ∇
       
:EndClass ⍝ Class TimeSeries&lt;/pre&gt;</pre>

The above description declares that instances of <span class="aplu">TimeSeries</span> will have four *Members*: two *Public Fields* called <span class="aplu">Obs</span> and <span class="aplu">Degree</span> (the latter having a default value of 3), a *Public Method* called <span class="aplu">PolyFit</span> (header plus 4 lines of code) and a *Constructor*, which is implemented by the function <span class="aplu">make</span> (header plus 3 lines of code). Note that methods (or functions) begin and end with a ∇. The term *Public* means that the methods are for “public consumption” by all users of the class.

The system function <span class="aplu">⎕NEW</span> is used to create new instances using the class definition. The first element of the right argument to <span class="aplu">⎕NEW</span> must be a class, the second element contains *instance parameters*, which are passed to the constructor:

ts1←⎕NEW TimeSeries (1 2 2.5 3 6)

During *instantiation*, the constructor function <span class="aplu">make</span> is called, and it initialises the instance by storing its argument in the public field <span class="aplu">Obs</span>. We can now use the instance <span class="aplu">ts1</span> in much the same way as if it were a namespace:

```
      ts1.Obs
1 2 2.5 3 6

      ts1.PolyFit ⍳5
0.9714285714 2.114285714 2.328571429 3.114285714 5.971428571

      1⍕ts1.(Obs-PolyFit ⍳⍴Obs)
 0.0 ¯0.1 0.2 ¯0.1 0.0

      ts1.⎕nl ¯2 ⍝ NameClass ¯2 is roughly equivalent to "PropList"
 Degree  Obs
      ts1.⎕nl ¯3 ⍝ NameClass ¯3 is "MethodList"
 make  PolyFit
```

For negative arguments, <span class="aplu">⎕NL </span>returns a vector of names rather than a matrix. The Degree field allows us to decide the degree of the polynomial function used when fitting the curve. The following example uses a straight line:

```
      ts1.Degree←1
      ts1.PolyFit ⍳5
0.7 1.8 2.9 4 5.1
```

Arrays of instances are handled in much the same way as arrays of namespaces:

```
      ⎕rl←16807 ⍝ Value in CLEAR WS - so we get the same random numbers
      tss←{⎕NEW TimeSeries ((10×⍳5)+?5⍴5)}¨⍳4

      ↑tss.Obs
11 24 33 43 52
11 24 34 45 52
13 25 31 41 53
14 21 32 41 53

      tss.Degree←2 3 2 1

      1⍕↑tss.PolyFit ⊂⍳10
 11.4 23.0 33.6 43.2 51.8 59.4 66.0 71.6 76.2  79.8
 11.1 23.5 34.8 44.5 52.1 57.2 59.2 57.6 52.0  41.8
 14.0 22.7 32.0 41.9 52.4 63.4 75.0 87.2 99.9 113.2
 12.6 22.4 32.2 42.0 51.8 61.6 71.4 81.2 91.0 100.8
```

APL developers have often used naming standards, and in recent versions of Dyalog APL namespaces, to collect related functionality into modules. Users of Dyalog APL will recognise that an instance is very similar to a *namespace*. One of the big advantages of classes is that they make it possible to “clone” a namespace and create multiple data “contexts”, without copying the code. This saves space, but more importantly it means that you are less likely to lose track of where the source code is.

Imagine that we have prototyped our way through to a “classical” solution to fitting multiple polynomials. Looking back, we are able to copy the following expressions from our session:

```
      data←(4⍴⊂10×⍳5)+?4⍴⊂5⍴5       ⍝ Generate some test data
      exp←0,¨⍳¨2 3 2 1              ⍝ Our polynomials vary by degree
      coeffs←data⌹¨(⊂⍳5)∘.*¨exp
      1⍕↑coeffs+.×¨⍉¨(⊂⍳10)∘.*¨exp
 14.2 23.0 32.8 43.6 55.4 ...etc...
```

After thinking a bit more about it, we might identify a couple of potential functions. If we use dynamic functions, we can refactor our solution as follows:

```
      polyfit←{⍵⌹(⍳⍴⍵)∘.*0,⍳⍺}
      polycalc←{⍺+.×⍉⍵∘.*¯1+⍳⍴⍺}
      1⍕↑(2 3 2 1 polyfit¨data) polycalc¨⊂⍳10
 14.2 23.0 32.8 43.6 55.4 …etc…
```

In a traditional APL system, we could now create a workspace called <span class="aplu">POLY</span> with these functions inside, write some documentation explaining how to call them, and then store that documentation in a variable in the workspace, or in a separate document. Anyone wanting to use the functions would have to find the relevant documentation and make sure that he did not already have any functions with these names in his application (or variables with the same name as the documentation). A namespace could be used to isolate the names from our application code.

Classes make it possible for the developer to encapsulate functionality in a way which keeps related code and data together, avoids name conflicts **and** provides some degree of documentation which suggests and can limit how the solution is used. This makes the module easier to learn to use, while the control over how the module can be used makes it easier to maintain. At the same time, splitting an application into objects with well defined behaviour and interfaces is a valuable tool of thought when dealing with complex design issues.

On the other hand, it is also clear that the simple functions <span class="aplu">polyfit</span> and <span class="aplu">polycalc</span> are more *generally* useful than the <span class="aplu">PolyFit</span> method of the class <span class="aplu">TimeSeries</span>, which exposes a specific use of polynomial fitting. The encapsulation of data within instances can make it harder, slower, and sometimes virtually impossible to go “across the grain” and use the properties and methods in a way which is different from that which was intended by the class designer. OO fans may argue that object orientation will help you think more carefully about how things will be used and this is to your advantage. However, APL is often used in problem areas where requirements change *very* unexpectedly. Providing a flexible solution with OO design is as much of an art, and requires the same insight into where the solution might be heading, as any other technique.

A key design goal for version 11.0 has been to make it as easy as possible to blend the array and functional paradigms which already make APL so productive, with the object oriented view of data, a Tool of Thought in its own right.

This is one of the reasons why, if you have a namespace <span class="APLU">POLY</span> which contains the two dynamic fns we developed above, you can add a line which says:

```
      :Include #.POLY...
```

at the beginning of <span class="aplu">:Class TimeSeries</span>, and subsequently write <span class="aplu">PolyFit </span>as:

```
    ∇ r←PolyFit x;coeffs
      :Access Public
      ⍝ Use cubic fit of observed points to compute values at x

      coeffs←Degree polyfit Obs ⍝ Find polynomial coeffs
      r←coeffs polycalc x       ⍝ Compute fitted f(x)
     ∇
```

This *Introduction to Object Oriented Programming for APL Programmers* will attempt to illuminate the issues and put the reader in a better position to decide when and how to combine array, functional and object thinking. In order to achieve this, we will:

First, briefly explore the thinking which lead to the emergence of OO, to get an idea about the type of problems which OO is likely to help us solve.

Introduce the fundamentals of OO programming using a number of examples written in Dyalog APL version 11.0.

Illustrate how the new OO functionality in Dyalog APL makes it easier than ever before to implement components which can be “consumed” by other development tools.

Where possible, try to remember to discuss alternative solutions, and present some guidelines on how to choose between the various techniques which are available. Given that the temperament and environment of the developer, the department and the company will weigh heavily on any choice of technique, it is clear from the outset that there will be no universal answers.

## <a id="_Toc117440112"></a>2. Origins

Although OO feels like a recent invention to many of us, the first OO language saw the light of day around the same time as the first APL interpreter. SIMULA (SIMUlation LAnguage) was designed and implemented by Ole-Johan Dahl and Kristen Nygaard at the Norwegian Computing Centre between 1962 and 1967, based on ideas which Nygaard had developed during the 1950s<a id="_ftnref1"></a>[<sup>\[1\]</sup>](#_ftn1). At the same time that Ken Iverson was working on new ways to conceptualise algebra and computation involving large groups of numbers, Nygaard was searching for ways to think about a different type of systems using symbolic notation<a id="_ftnref2"></a>[<sup>\[2\]</sup>](#_ftn2).

The focus of the NCC work was on the simulation of complex systems. Nygaard explained the rationale behind SIMULA as follows:

*SIMULA represents an effort to meet this need with regard to discrete-event networks, that is, where the flow may be thought of as being composed of discrete units demanding service at discrete service elements, and entering and leaving the elements at definite moments \[sic\] of time. Examples of such systems are ticket counter systems, production lines, production in development programs, neuron systems, and concurrent processing of programs on computers.*

The desire to describe and model so-called discrete-event networks led to object-oriented notation, in which the description of the ways in which the “service elements” interacted with each other is separated from the details of how each service element (or *object*) manages its internal state. As with APL, the language subsequently evolved into a notation which could be executed by computers.

SIMULA turned out to be a powerful notation for simulating complex systems, and other OO languages followed. Initially, OO languages were used for planning and simulation applications (much the same areas as APL has been most successfully applied to), but with the arrival of graphical user interfaces, which are a form of discrete-event network, and concurrent or networked computing systems, OO languages proved that they had more to offer. As systems and teams used to implement them have grown in size and complexity, OO has grown from a humble start as a specialist modelling technique to become the most popular paradigm for describing computer systems.

With Dyalog APL version 11.0, Arrays, Functions and Objects are now happily married. It is possible to have arrays of instances, and instances can contain arrays (of more instances, if necessary). The challenge is to pick the best architecture for a given problem!

### <a id="_Toc117440113"></a>Queue SIMULAtion

As an illustration, let us take a look at one of the classical examples which the inventors of SIMULA used the new language to model: The *queue*. Customers arrive at random intervals and enter the queue. An algorithm simulates the time required to process each customer. The goal is to run a number of simulations with different parameters and see how long the queue gets and how long customers have to wait. With luck, we will discover how the queue or system of queues can be optimized, and measure the effect of improving the system without having to perform expensive experiments. In the post-modern age, these systems are probably being used to see how much *longer* the queues will get if the Post Office spends *less* money. Assuming that the planning department has not already been “made redundant”.

The following is a simple Queue class, written in Dyalog APL version 11.0<a id="_ftnref3"></a>[<sup>\[3\]</sup>](#_ftn3).

```
:Class Queue

      :Field Public Instance History←⍬
      Customers←⍬

      ∇ r←Length
        :Access Public
        r←1⊃⍴Customers
      ∇

      ∇ Join Customer
        :Access Public
        Customers←Customers,⊂Customer,(3⊃⎕AI),1+⍴Customers
        :If 1=⍴Customers ⋄ Serve&0 ⋄ :EndIf
      ∇

      ∇ Serve dummy;t;elapsed
      ⍝ Start serving queue in new thread when queue length grows to 1
      ⍝ Stop when queue is empty.

        :Repeat
            ⎕DL 9+?11 ⍝ Processing takes between 10 and 20 seconds
            elapsed←(3⊃⎕AI)-1 2⊃Customers ⍝ Since customer entered queue
            History,←⊂(elapsed,1⊃Customers)[2 1 4]
            ⍝ History records (Cust#, Total Time, Initial Queue Length)
            Customers←1↓Customers ⍝ Customer has left the building
        :Until 0=⍴Customers
      ∇

 :EndClass ⍝ Class Queue
```

The class has two *public methods* called <span class="aplu">Join</span> and <span class="aplu">Length</span>, which are used to add customers to the end of the queue and report the current length of the queue, respectively. There is a *public field* <span class="aplu">History</span> which contains a record of customers who passed through the system. Note that *members* of a class are *private* unless they are declared to be public. Private members cannot be referenced from outside the class.

While the <span class="aplu">TimeSeries</span> class in the previous chapter only had public members, <span class="aplu">Queue</span> has a *private field* called <span class="APLU">Customers</span>, and a *private method* called <span class="APLU">Serve</span>, which is launched in a background thread when there are customers in the queue (thread-savvy readers are requested to ignore the potential race condition if <span class="APLU">Server</span> drops the last element of <span class="aplu">Customers</span> at the same time as <span class="APLU">Join</span> adds one).

If we have a workspace containing the above class, we can experiment with it from the session:

```
      aQueue←⎕NEW Queue
      aQueue.Join¨1 2 3   ⍝ 3 customers stormed in together
      aQueue.Length       ⍝ Result will depend on how quickly you type
2
      ⎕←⎕DL 60
60.022
      aQueue.Length
0
```

The variable <span class="aplu">History</span> contains a log of the customers who passed through the queue, the time they spent in line and the length of the queue when they entered it. Since <span class="aplu">History</span> was declared as a public field, we can refer to it from “outside”:

```
      ↑aQueue.History        ⍝ Complete history
1 11032  1
2 26800  2
3 41833  3

      ⌈/aQueue.(¯2↑¨History) ⍝ Longest wait and max queue length
 41833 3
```

### <a id="_Toc117440114"></a>Encapsulation

In our <span class="aplu">Queue</span> class, we have decided that the <span class="aplu">Customer</span> field, which contains the list of customers in the queue, is *private*. We do not want users of our class to reference it. We have provided a public method <span class="aplu">Length</span> which makes it possible to determine how long it is.

Why have we not simply made the variable public and allowed the user to inspect <span class="aplu">aQueue.Customers</span> using <span class="aplu">⍴</span> or other primitive functions, rather than doing extra work to implement a method ourselves? We would typically do this if we want to reserve the right to change this part of the implementation in the future, or if we do not wish to take responsibility for the potential bugs resulting from the use of these members (“Warranty void if seal broken”).

If we had exposed <span class="aplu">Customers</span> directly, users would have the right to expect that we would continue to have a one-dimensional vector called <span class="aplu">Customers</span> with similar characteristics in the future. It would be virtually impossible for us (as class designers) to estimate the impact of a change to this variable, without reading all the application code to see *exactly* how it was being used. And even then, we would probably get it wrong. The user might also feel tempted to modify the variable, which might cause bugs to be reported to us, even though there were no errors in our code.

If our class evolved into a more general tool where the contents of the queue were not necessarily customers, we could not rename it – so we would end up with a system where variable names were misleading<a id="_ftnref4"></a>[<sup>\[4\]</sup>](#_ftn4). We cannot turn it into a 2-dimensional matrix, store it on file, or make any other architectural changes which might be convenient as requirements evolve and we need to store more or different types of information about the queue.

We have thought ahead a little bit and decided that it will always be reasonable for users of the <span class="aplu">Queue</span> class to ask what the current length of the queue is, and therefore we have exposed a public method for this purpose.

*Information hiding*  is one of the cornerstones of OO. The ability to decide which members of a class are visible to the users on the one hand and the developer of the class on the other is seen as the key to reduced workload, improved reliability and maintainability of the code on *both* sides of the divide. Dyalog APL version 11.0 takes a strict view of encapsulation. It is *not* possible to reference private members (a <span class="aplu">VALUE
ERROR</span> will be signalled if you try).

```
      aQueue.Customers
VALUE ERROR
      aQueue.Customers
      ^
```

Clearly, if the designer of a class you are using has decided to hide information which you really need before 9am tomorrow, this can be frustrating. Before you get too concerned, the good news is that there are a variety of techniques for getting past the gatekeepers in an emergency, or in a “prototyping session”. We will discuss a couple of these in the following chapters. However, to enjoy the full benefits of OO, it is important to have the discipline to use such tricks only when required, and re-factor the class in question at the first available opportunity.

The variable <span class="aplu">History</span> is obviously susceptible to the same problems as <span class="aplu">Customers</span>, and seasoned OO designers would probably consider it to be bad form to expose it. This is perfectly true, exposing all the result data in this form in order to make it easy to analyze is a result of “traditional APL thinking”. We will investigate a number of alternatives which we could have used to expose data in subsequent chapters.

## 3. Working with Classes

In order to help you successfully experiment with APL as we explore more OO functionality, let us take a closer look at the practical details of functionality which has been introduced in the first two chapters, and get comfortable with actually working with classes and instances.

The easiest way to create a class is probably to use the editor. Start an <span class="aplu">)ED</span> session, prefixing the name of your new class by a circle (ctrl+O on most keyboards). We’re going to use a class for generating random numbers to illustrate some important issues:

```
      )ed ○Random
```

This will present you with an empty class, which only contains the <span class="aplu">:Class</span> and <span class="aplu">:EndClass</span> statements. Insert a few lines to create the following simple class:

```
  :Class Random

       :Field Private InitialSeed←42
       ⎕←'Default Initial Seed is: ',⍕InitialSeed

       ∇ make args
         :Implements Constructor
         :If 0≠args ⋄ InitialSeed←args ⋄ :EndIf
         Reset
       ∇

       ∇ Reset
         :Access Public
         ⎕RL←InitialSeed
         'Random Generator Initialised, Seed is ', ⍕⎕RL
       ∇

       ∇ r←Flat x
         :Access Public
         ⍝ Return x random numbers in range [0,1>, flat distribution
         r←(¯1+?x⍴100)÷100
       ∇

   :EndClass ⍝ Class Random
```

This class can be used to generate sequences of random numbers with a “flat” distribution between 0 and 1 (with only 2 digits, to allow us to easily recognize them in the examples). One advantage of encapsulating it in a class is that it can manage its own seed (<span class="aplu">⎕RL</span>) completely separately from the rest of the system. We can generate repeatable or suitably randomized sequences according to the requirements of our application.

As you exit from the editor, APL evaluates the class definition from top to bottom. Most of the script consists of function definitions, but in our class there are two APL expressions, which are executed as the script is fixed in the workspace:

```
    InitialSeed←42
    ⎕←'Default Initial Seed is: ',⍕InitialSeed
```

As a result, you should see one line written to the session as you leave the editor. If there are errors in any of the executable lines in your script, you will see one or more error messages in the status window, and the class will not be fixed in the workspace. If you are unable to correct (or comment out) all the errors, you can save your work and return to it later by changing the type of the name to a character vector.

Let’s perform some experiments with our new class:

```
      rand1←⎕NEW Random 0
Random Generator Initialised, Seed is 42
```

The constructor also has an output statement, which shows us which initial seed was selected for the instance.

```
      rand1.Flat 6
0 0.52 0.73 0.26 0.37 0.19

      rand2←⎕NEW Random 0
Random Generator Initialised, Seed is 42

      rand2.Flat 3
0 0.52 0.73

      ?6 6 6
1 5 3

      rand2.Flat 3
0.26 0.37 0.19
```

As can be seen above, each instance produces the same sequence of numbers, if the same initial seed is used. The sequence is unaffected by the use of <span class="aplu">?</span> in the root, or indeed anywhere else in the application.

```
      rand3←⎕NEW Random 7913
Random Generator Initialised, Seed is 7913

      rand3.Flat 6
0.06 0.85 0.1 0.29 0.78 0.62

      rand3.Reset
Random Generator Initialised, Seed is 7913

      rand3.Flat 6
0.06 0.85 0.1 0.29 0.78 0.62
```

We can create a generator with a non-default initial seed, and we can reset the sequence. If you are accustomed to using namespaces, the above behaviour will not come as a surprise, as you will be accustomed to each namespace having a separate set of system variables. However, the encapsulation provided by an instance is even stronger, as illustrated by the following example:

```
      rand4←⎕NEW Random 0
Random Generator Initialised, Seed is 42

      rand4.Flat 3
0 0.52 0.73

      rand4.(?6)
4

      rand4.Flat 3
0.26 0.37 0.19
```

If <span class="aplu">rand4</span> had been a namespace rather than an instance, the call to <span class="aplu">?</span> inside <span class="aplu">rand4</span> would have modified <span class="aplu">⎕RL</span> in the namespace, and the subsequent call to <span class="aplu">Flat</span> would have continued from a different point in the sequence. However, APL expressions executed on an instance from outside are executed in an “APL execution space”, which is *separate* from the space in which the class members run.

In effect, when an instance of a class is created, APL encapsulates it within a namespace. This has always been the case for instances of COM or DotNet classes, and as a result, Dyalog APL also allows the use of APL expressions in parenthesis following the dot after the name of one of these instances. Such APL expressions have access to all the public members of the instance, but are (obviously, since Excel cannot run APL expressions) executed outside the instance itself, as in the following example:

```
      'XL' ⎕WC 'OLECLIENT' 'Excel.Application'
      XL.(Version,'/',OperatingSystem)
11.0/Windows (32-bit) NT 5.01
```

In this example (which would work the same way in Dyalog APL versions 9 or 10), there is an APL expression which references the public properties <span class="aplu">Version</span> and <span class="aplu">OperatingSystem</span> and catenates them together. For consistency, the same approach is used for instances of APL-based classes, rather than simply running the expressions as if the instance was a namespace. Thus, the behaviour of an APL class will not change if it is exported to a DotNet Assembly or a COM DLL and subsequently used from APL.

When an instance method such as <span class="aplu">Flat</span> is referenced in one of these expressions, <b><i>it</i></b> runs in the instance environment. For example, the example on the previous page could have been written:

```
     rand4←⎕NEW Random 0
Random Generator Initialised, Seed is 42
     rand4.Flat 6
0 0.52 0.73 0.26 0.37 0.19

     Rand4.Reset
Random Generator Initialised, Seed is 42
     rand4.(⌽(Flat 3) (?6) (Flat 3))
 0 0.52 0.73  4  0.26 0.37 0.19
```

The reverse is required because the rightmost call to <span class="aplu">Flat</span> happens first J. The important point is that the call to (<span class="aplu">?6</span>) in the middle of the expression executes in and uses the <span class="aplu">⎕RL</span> in the *APL space* and does not modify the value of <span class="aplu">⎕RL</span> in the *instance space*.

**Note:** There are a couple of small potential surprises which are worth mentioning:

First, the APL space inherits the values of <span class="aplu">⎕IO</span>, <span class="aplu">⎕ML</span> and other system variables when the object is instantiated –<i>not</i> where it is being used. Secondly, if you mistype the name of a property or field in an assignment, this will create a variable in the APL space. For example:

```
      ts1←⎕NEW TimeSeries (1 3 2 4 1)
      ts1
#.[Instance of TimeSeries]
      ts1.Obs
1 3 2 4 1

      ts1.obs←1 3 2 4 2 ⍝ Lowercase "o", note no error message!
```

This is the same behaviour as you would get if you made a spelling error in APL, but might come as a bit of a surprise in an OO setting. However, we believe that it is desirable to allow a user to introduce own names for analytical purposes. For example, if <span class="aplu">iPlan</span> is an instance of some object which exposes properties named <span class="aplu">Actual</span> and <span class="aplu">Budget</span>, it may be very useful to introduce a new property:

```
      iPlan.(Variance←Actual-Budget)
```

It is possible that a future version of Dyalog APL will allow the class designer or the user to place restrictions on the introduction of new names into the APL space.

## <a id="_Toc117440116"></a>4. Some Useful Debugging Tricks

The *strict encapsulation* described in the previous chapter may be a bit disconcerting to APL developers, who are accustomed to having access to data on a “want to know” rather than a “need to know” basis J. What if we want to know what value <span class="aplu">⎕RL</span> or <span class="aplu">InitialSeed</span> currently have in the instance, because the instance seems to be misbehaving?

The first thing which is important to realize is that if you set a stop in an method, or trace into a call, the internal environment where the method is running is available to you while the method is on the stack. To experience this first hand, create a new instance of <span class="aplu">Random</span>, trace into a call to <span class="aplu">Reset</span> or <span class="aplu">Flat</span> and examine the value of <span class="aplu">⎕RL</span> while one of these functions is suspended.

It is also important to realize that classes and instances are dynamic in APL (as you would expect)! If you edit a class and fix it, all existing instances will be updated to include the new definition. You can inject temporary methods into a class for debugging purposes. Type <span class="aplu">)ED Random</span> and add a public method to the class:

```
  ∇ r←RL x
    :Access Public
    r←⎕RL
    :If x≠0 ⋄ ⎕RL←x ⋄ :EndIf
  ∇
```

Using our new method <span class="aplu">RL</span>, we can now query and set <span class="aplu">⎕RL</span> in the instance as follows:

```
      rand1.RL 77
 42
      rand1.RL 0
 77
```

The system function <span class="aplu">⎕CLASS</span> returns the class of an instance. This can be useful in a debugging situation where you are faced with a misbehaving instance of unknown pedigree and need to know which class to edit. You could just display the instance, the default display will often tell you the class name, but as we will learn a bit later, it is possible to change this – so it is not a reliable way to determine the class.

```
      ⎕←rand1
#.[Instance of Random]

      ⎕CLASS rand4
#.Random
```

In one of the following chapters, we will show how it is possible to define a *derived* class. A derived class *extends* an existing class by *inheriting* its definition and adding to it. For an instance of a derived class, the result of <span class="aplu">⎕CLASS</span> will have more than one element, and document the entire class *hierarchy*. The first element always starts with a reference to the class which was used to create the instance.

### <a id="_Toc117440117"></a>The Mother of All Workarounds

The ultimate workaround or back door to break encapsulation is of course the introduction of a public method with a name like <span class="aplu">Execute</span>, which allows you to execute any APL expression you like in the instance space. We can use the <span class="aplu">:Include</span> keyword to embed a namespace containing suitable development tools in your classes. An example namespace called <span class="aplu">OOTools</span> can be found in the workspace of the same name in the <span class="aplu">OO4APL</span> folder. It includes a number of functions which may be useful during development. The functions with names beginning with **i** will execute in the instance space, those beginning with **s** will run in the shared space (more about the shared space later):

```
      rand1←⎕NEW Random 0
Random Generator Initialised, Seed is 42

      rand1.xxx←'Bingo'          ⍝ New variable in the APL space
      rand1.⎕NL 2                ⍝ Vars in the APL space
xxx
      rand1.iNL 2                ⍝ Vars/Fields/Props in Instance Space
InitialSeed
      rand1.iNC 'make'           ⍝ make is an instance Function
3
```

And:

```
      rand1.⎕RL←77
      rand1.⎕RL                 ⍝ No surprise!
77

      rand1.iExec '⎕RL'         ⍝ ... the instance value is unchanged
42
      rand1.iExec '⎕RL←99'      ⍝ But now we are able to change it!
99
```

When you release the application for testing, you should empty the <span class="aplu">OOTools</span> namespace to ensure that your application code does not use any of these “forbidden” methods.

## <a id="_Toc117440118"></a>Summary of Chapters 1–4

In the first four chapters, we have discussed the extensions to Dyalog APL which are summarized in the following table.

<table border="1" cellspacing="0" cellpadding="4">
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">)ED   ○MyClass</pre>
  </td>
  <td width="324" valign="top">
  
        <p>Edits the class <span class="aplu">MyClass</span></p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">Instance←⎕NEW   MyClass Args</pre>
  </td>
  <td width="324" valign="top">
  
        <p>Create a new instance of <span class="aplu">MyClass</span>, passing Args to the constructor.</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Class MyClass</pre>
  
        <pre class="aplu">:EndClass</pre>
  </td>
  <td width="324" valign="top">
  
        <p>Statements which begin and end the class
  script for <span class="aplu">MyClass</span>.</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Field MyField
:Field Private MyField</pre>
        
  </td>
  <td width="324" valign="top">
  <p>A private field (can only be used by code
  defined in the class)</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Field Public MyField</pre>
  </td>
  <td width="324" valign="top">
  <p>A public field (visible from the outside)</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Field ... MyField   ←expression </pre>
        
  </td>
  <td width="324" valign="top">
  <p>A field with a default value</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">∇MyFn
∇</pre>
        
  </td>
      <td width="324" valign="top">
  
        <p>Beginning and end of a function or “method”</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Access Public</pre>
  </td>
  <td width="324" valign="top">
  <p>Declares the current function to be public</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Access Private</pre>
  </td>
  <td width="324" valign="top">
  <p>Function is private (the default)</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Implements Constructor</pre>
  </td>
  <td width="324" valign="top">
  <p>Identifies a method as the constructor, which
  is used to initialize the contents of a new instance</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">:Include NameSpace</pre>
  </td>
  <td width="324" valign="top">
  
        <p>Makes all functions and variables in <span class="aplu">NameSpace</span> private members of the current class, unless this is overridden
  by<span class="aplu"> :Access </span>statements in the code.</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">⎕CLASS Instance</pre>
  </td>
  <td width="324" valign="top">
  <p>Returns the class hierarchy for an instance.</p>
  </td>
 </tr>
 <tr>
      <td width="259" valign="top" nowrap>
  
        <pre class="aplu">⎕NL</pre>
  </td>
  <td width="324" valign="top">
  
        <p>Negative arguments return a vector of names.
  Further extensions to <span class="aplu">⎕NC</span> and
  <span class="aplu">⎕NL</span> will be introduced shortly.</p>
  </td>
 </tr>
</table>

## Coming Next

Properties, Constructors, Shared members, Inheritance and Interfaces.

***

<a id="ftn1"></a>

<a id="_ftn1"></a>[\[1\]](#_ftnref1) For more on the fascinating story about SIMULA, see “Compiling Simula” by Jan Rune Holmevik, [http://www.ifi.uio.no/~cim/sim\_history.html](http://www.ifi.uio.no/%7Ecim/sim_history.html)

<a id="ftn2"></a>

<a id="_ftn2"></a>[\[2\]](#_ftnref2) Ken Iverson was born in Canada in 1920, but Norway also figures prominently in his heritage. Therefore, we should have reason to hope that the paradigms are not incompatible J.

<a id="ftn3"></a>

<a id="_ftn3"></a>[\[3\]](#_ftnref3) For a typical SIMULA solution, see Example 2 on page [http://staff.um.edu.mt/jskl1/talk.html#Simulation](http://staff.um.edu.mt/jskl1/talk.html#Simulation)

<a id="ftn4"></a>

<a id="_ftn4"></a>[\[4\]](#_ftnref4) One-letter names tell no lies, of course. J
