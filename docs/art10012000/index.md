---
vid: '10012000'
title: Parallel Each
authors:
- David Liebtag
volume: '23'
issue: '4'
page: '51'
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
source: trad/v234/liebtag.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2008
tags:
- language design
- performance
- implementations
---

![David Liebtag](../../images/faces/liebtag.jpg)

## Parallel Each

## David Liebtag liebtag@us.ibm.com

The APL Working Group of GUIDE SHARE EUROPE in Germany and APL-Germany e.V. held meetings in Hanover, Germany on 12 and 13 June 2008. David Liebtag made several presentations at those meetings about IBM Workstation APL2 Service Level 12. These articles summarise his presentations about APL2’s new support for parallel processing and structured storage. *Ed.*
{ .ednote }

There has been a lot of discussion recently about how to make it easy for programmers to exploit multiple processors. This topic is of great interest to computer scientists, language developers, application developers and end-users. The problem is that most languages are designed to provide instructions to computers on how to perform a sequence of calculations in series. They do not provide a convenient way to specify how an algorithm should be partitioned and distributed onto separate processors.

For a few years people have been suggesting that because APL already works with whole arrays rather than individual data items, perhaps APL has an opportunity to lead the way in this field. Perhaps APL’s semantics already reveal the parallel nature of many algorithms. If that is so, perhaps APL language processors could detect the parallel nature of application components and automatically distribute them to multiple processors.

This article includes an overview of the hurdles to be overcome in the exploitation of multiple processors. It also demonstrates two new operators in Workstation APL2 which allow developers easily to distribute sections of applications onto multiple processors, to network connected machines and achieve significant performance improvements.

## What is performance?

When discussing performance, it is a good idea to understand what we mean. However, performance can be hard to define.

For example, does performance simply mean how many calculations are performed? If so, then what is a calculation? Do calculations always use input data and produce results? What about an application’s internal calculations, such as incrementing array indexes and determining where and how to access data?

Simply copying data to and from memory can significantly impact performance. In addition, some algorithms require more storage than others and can be constrained by the amount of available storage. Providing more storage using virtual memory and reorganizing algorithms to work within constraints can also significantly impact performance.

Furthermore, performance is affected by *how* data is accessed. Data is read and written from disk drives, networks, and real-time measurement devices. The speed of all these devices can impact performance.

You may be thinking, *Wait a minute. It’s obvious. Performance means how fast an application runs.* But what does it even mean for an application to run? Consider that sometimes you may require results with a high degree of accuracy and precision. Other times you may require results that provide only a rough estimate of the answer. These two ways to run an application may require very different amounts of time to run, or amounts of performance. So even beyond the general specification of the results, the specification of the required precision and accuracy of the results can affect performance.

Finally, all these contributions may be insignificant to overall application performance, when using sample data. However, when using scaled-up production data, they may have a profound effect on performance.

## How does hardware affect performance?

For years, computer manufacturers’ specifications have included clock speed and memory size. More sophisticated users may also understand specifications for chip architectures and instruction sets, and machine cache sizes and bus widths. Clearly, these hardware features affect performance and we have all used some or all of them to inform our decisions when buying computers. If we wanted our applications to run faster, we bought faster machines with more memory.

More recently, machines have become available with multiple cores. You can even buy multiple machines and connect them in a grid or a cloud. How will this affect performance?

It depends on the application. Although some specialised applications are beginning to appear that exploit multiple processors, most applications process a single set of sequential instructions. In general, no matter how many processors you have available, these applications will use only one processor and will not run any faster. Modifying these applications to use multiple processors or multiple machines, can be a very, very difficult job.

## What affects distributed computing performance?

In order to distribute an application onto multiple processors, you, or a language processor, must identify which parts of an application can be distributed, slice the data into sections, copy them to the processors, run the code on each of the processors, and gather the results back together in the main application’s storage.

Each of these decisions must be done efficiently. The time it takes to determine whether an operation can be distributed, and to copy the data and results between main and multiple processors, can easily overwhelm the performance gains achieved by using multiple processors. In addition, simply managing multiple processors takes time. It is important to minimize the number of times that processors are started and data is sliced, distributed, and gathered.

## What affects interpreter performance?

Several features of APL interpreters and applications affect performance.

First, consider scalar operations such as this:

```
      →BIN/LABEL
```

This type of code is very common. It is widely used to control flow of operations within applications. In fact, it is so common that most applications spend the vast majority of their time executing scalar operations. Indeed, since most applications spend most of their time running scalar operations, even significant improvements in array operations can have little affect in overall application performance.

Next, consider array operations such as this:

```
      Z←A×B
```

You may think that these expressions are terrific candidates for automatic distribution to multiple processors. The APL interpreter could slice up the arrays and distribute the primitive to multiple processors. However, consider this example, that uses multiple array primitives:

```
      Z←A×B×C
```

If the interpreter distributed each primitive array operation to multiple processors, it would have to slice up the array, distribute it, and gather the results together for each primitive. This could require a lot of overhead. These remarks seem to imply that the pundits were wrong: APL code does not appear to lend itself to distribution on multiple processors. How can we prove them right? Clearly, we need a way to avoid distributing operations over and over again. We need a way to identify when to distribute operations at times when it will give us the most bang for the buck.

## Parallel Each

The APL2 primitive operator *each* `¨` applies a function to multiple arrays. The application developer uses *each* to indicate when operations can be performed independently. IBM has extended this facility with two external operators:

PEACHP
:   parallel *each* using processors

PEACHT
:   parallel *each* using threads

Like the *each* operator, the parallel-each operators apply the function to each element of the argument arrays. Unlike *each*, the elements are processed asynchronously.

`PEACHP` processes each element in a separate process, which can be on the same or other machines. `PEACHT` processes each element in a separate thread running on the same machine.

Both operators use multiple processes or threads to process the data. The number of processors or threads used is controlled by the number of elements in the arrays and the number of machines and processors that are available.

### Syntax

```
result ← [larg] (function PEACHP (options [processors])) rarg
result ← [larg] (function PEACHT options) rarg
```

where

options
:   Vector of character vectors containing APL2 invocation options

processors
:   Integer vector containing identifiers of processors running AP 200, the *Calls to APL2* processor.

function
:   Character vector containing a function name

`larg` and `rarg`
:   *each* arguments

The parallel-each operators start multiple slave APL2 interpreters, each with a separate workspace. The invocation options are used when starting these interpreters. The list of processors is used to locate the machines to use.

The function named in the operand is copied from the caller’s namescope into the slave interpreters. For this reason, the named function is usually an association with a function in a namespace.

The elements of the arrays are copied into the separate slave interpreters’ workspaces. Because the function is applied on the array elements in separate workspaces, side-effects are not supported. The function can not access the caller’s workspace.

## Sample

The following example compares using the `¨` primitive operator and `PEACHT` to call the `FFT` function from the `MATHFNS` namespace to calculate Fast Fourier Transforms on a machine with two processors.

```
      DATA←⊂[2]?20 65536⍴1000     ⍝ Generate some data

      START←⎕AI                   ⍝ Record the time
      'MATHFNS' 11 ⎕NA 'FFT'      ⍝ Associate FFT
1
      ⍴FFT¨DATA                   ⍝ Apply FFT on each element
20
      ⎕AI-START                   ⍝ How much time did it take?
0 6209 6443 218

      START←⎕AI                   ⍝ Record the time
      4 11 ⎕NA 'PEACHT'           ⍝ Associate PEACHT
1
      ⍴('FFT' PEACHT '')DATA      ⍝ Apply FFT with PEACHT
20
      ⎕AI-START                   ⍝ How much time did it take?
0 47 3541 203
```

The second and third elements of `⎕AI` are the compute and connect times. Notice that by simply changing the code to use `PEACHT` rather than the `¨` primitive operator, the connect time was reduced by 45 percent. The compute time was also reduced to nearly zero, but this is only because CPU time used by the asynchronous interpreters is not accumulated in the calling interpreter’s account information.

## Usage guidelines

The parallel-each operators currently copy the argument data from the application’s workspace to the slave interpreters’ workspaces. The results are created in the slave interpreters’ workspaces and then copied to the application’s workspace. The time required to copy the arguments and results can overwhelm the benefits of using multiple processors.

The parallel-each operators are appropriate for a subset of possible uses of the primitive *each* operator. They can give significant improvements for applications which do not require extremely large arguments, perform computationally intensive calculations, and do not return extremely large results.

## Summary

People have been suggesting for a number of years that APL may be able to automatically exploit new machines with multiple processors. With Workstation APL2 service level 12, the IBM APL Products and Services group has proved they were correct. With trivial changes to their programs, APL2 developers can now run their applications on multiple processors and gain performance improvements that are directly proportional to the number of cores available to the applications.

Further information about APL2 and Service Level 12 is available at [http://www.ibm.com/software/awdtools/apl](http://www.ibm.com/software/awdtools/apl). Detailed information about APL2, the parallel each operators, and service level 12’s other new facilities can be found in the *APL2 User’s Guide* through the Library link.
