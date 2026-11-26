---
vid: '10500100'
title: Structured Storage and Monitor Expressions
authors:
- David Liebtag
byline: by David Liebtag (liebtag@us.ibm.com)
volume: '24'
issue: '1'
page: '35'
description: Article submitted to vector.org.uk
source: content/printed/241/liebtag.htm
converter: vecrescue 0.1.0
issue_tab: n1-december-2008
tags:
- system interfaces
- implementations
---

by David Liebtag (liebtag@us.ibm.com)
{ .byline }

This is a version of a presentation made to the APL Germany 2008 Spring Meeting, since updated to reflect the release of APL2 Service Level 13. *Ed.*
{ .ednote }

## Introduction

APL2’s name association facility `⎕NA` allows APL programs to cause references to names to be processed by associated processors rather than the APL2 interpreter. Associated processors are supplied which provide access to files and programs written in other languages such as C, Java, and Rexx. APL2 Service Level 12 included a new Associated Processor 15, which provides access to structured storage. Service Level 13 added support for Processor 15 monitor expressions which can be used to perform data change tracing and validation.

APL is traditionally a loosely-typed language. Arrays simply contain numbers and characters and the structure and type of their data can be changed at any time. APLers usually view this as one of the language’s benefits: they don’t have to worry about how data is represented internally; they can focus on their problems. However, many users of other languages view strong typing as a benefit: type checking helps programmers avoid domain, rank, and length errors. Associated Processor 15 supports strong data typing and enables APL2 programmers to gain the benefits of type checking that are usually only enjoyed by programmers of compiled languages. In addition, Associated Processor 15 lets APL2 programs access data that is outside and larger than the workspace and share data with programs written in other languages. This article introduces Associated Processor 15 and demonstrates some of the ways that it can be used.

## Strong data typing

Have you ever encountered an error in an application and discovered that a variable’s array had an unexpected type, rank, length, or depth? How would you go about finding the cause of such an error? You might have to look at every line that the application might have executed that assigned a value to the variable. Needless to say, in a large application, this might be tedious. Wouldn’t it be nice if you could force your application to suspend at the point at which the incorrect value was assigned? That is exactly what you can do with Associated Processor 15.

Consider this expression:

```
      'I4 1 3' 15 ⎕NA 'Integers'
1
```

The expression associates the name `Integers` with a rank one array that contains three 4 byte integers. (`⎕NA` returns 1 to indicate the association succeeded.)

You can use the name just like normal:

```
      Integers←4 6 8
      Integers
4 6 8
```

But look what happens when you try to assign a value which does not match the pattern:

```
      Integers←12 4.56 17
DOMAIN ERROR
      Integers←12 4.56 17
      ^^
```

The new second element is a floating point number rather than an integer. Because the new array does not match the pattern, Associated Processor 15 immediately signals an error.

If you reference the variable, it has the last valid value:

```
      Integers
4 6 8
```

## Patterns

Associated Processor 15 supports simple, nonhomogeneous, and nested arrays. The arrays may contain a wide variety of types of data including 1-, 2-, and 4-byte characters, 1-bit and 1-, 2-, and 4-byte integers, and 4- and 8-byte floating-point numbers. Character vectors may be fixed-length or variable-length null-terminated strings. Here are some sample patterns:

<table>
			<tbody>
				<tr>
					<td class="left">Integer scalar:</td>
					<td class="code left">'I4 0'</td>
				</tr>
				<tr>
					<td class="left">Character matrix:</td>
					<td class="code left">'C1 2 3 4'</td>
				</tr>
				<tr>
					<td class="left">String:</td>
					<td class="code left">'S1 1 64'</td>
				</tr>
				<tr>
					<td class="left">Nested array:</td>
					<td class="code left">'G0 1 3 I4 1 3 E8 2 3 4 C1 1 32'</td>
				</tr>
			</tbody>
		</table>

In short, Associated Processor 15 supports the all same data types as APL2’s Associated Processor 11 and the `ATR`, `PFA`, and `RTA` external functions.

## Accessing data by address

Sometimes programs written in other languages refer to data by address rather than by value. Associated Processor 15 can be used to share data with these programs.

Suppose you used a variable named `ADDRESS` to hold the result of a program which returned the address of a three-integer vector. Consider this expression:

```
      ('I4 1 3' ADDRESS) 15 ⎕NA 'NotMine'
1
```

In this case, the name `NotMine` refers to the storage owned by the other program. You can use it just like before:

```
      NotMine←14 16 18
      NotMine
14 16 18
```

You can also retrieve the address of an APL2 array and pass it to programs written in other languages. Here’s another association expression:

```
      ('ADDRESS' 'Integers') 15 ⎕NA 'ArrayAddress'
1
```

This expression associates the name `ArrayAddress` with the address of the `Integers` array. You could use `ArrayAddress` as an argument to a non-APL program and that program could access the array directly.

For simple arrays with types corresponding exactly to the APL2 internal data types, Associated Processor 15 can access arrays that are larger than the workspace. This enables APL2 programs to access very large arrays that are allocated by other programs.

## Naming array elements

Because you can acquire the address of arrays, and you can associate names with arbitrary addresses, you can use Associated Processor 15 to name elements of arrays. For example:

```
      ('I4 0' (ArrayAddress+4)) 15 ⎕NA 'SecondInteger'
1
      Integers←3 4 5
      SecondInteger
4
      SecondInteger←67
      Integers
3 67 5
```

The expression adds 4 to `ArrayAddress` to adjust the address to the second four-byte integer.

## Accessing exported variables

Sometimes programs written in other languages share data by placing it in shared libraries (or DLLs on Windows) and exporting it by name. This enables multiple programs on the same machine to all use the same storage. Associated Processor 15 supports accessing exported variables.

For example, APL2 on Windows includes a library named `aplwin.dll` that exports a 4-byte Boolean variable named `DisplayLogo`. The following expressions demonstrate how to access this variable:

```
      ⍝ Associate name
      ('I4 0' 'aplwin') 15 ⎕NA 'DisplayLogo'
1
      ⍝ Reference value
      DisplayLogo
0
```

DisplayLogo controls whether the Session Manager displays the product information dialog during invocation.

## Monitor expressions

Each variable associated with Processor 15 has a monitor expression. The monitor expression is executed each time the variable is changed. Here’s another association expression:

```
      ('MONITOR' 'Integers') 15 ⎕NA 'Monitor'
1
```

This expression associates the name `Monitor` with the `Integers` variable's monitor expression. You can use monitor expressions to trace variable specifications. For example:

```
      3 11 ⎕NA 'DISPLAY'
1
      Monitor←'DISPLAY Integers'
      Integers←3 4 5
┌→────┐
│3 4 5│
└~────┘
```

You can also use monitor expressions to validate values. For example:

```
      ⍝ Ensure the first element is 1, 2, or 3.
      Monitor←'⎕ES(~(↑Integers)∊1 2 3)/5 4'
      Integers←17 2 3
DOMAIN ERROR
      Integers←17 2 3
      ^^
```

A variable’s monitor expression is executed after the variable's value has been changed.

## Arbitrary arrays

Sometimes you want to use Associated Processor 15’s support for monitor expressions, but you do not want to impose strong data typing. For these cases, you can supply an empty pattern:

```
      '' 15 ⎕NA 'Array'
1
```

Any arbitrary array can be assigned to `Array`.

## Summary

Associated Processor 15 provides efficient ways to detect data errors and share data with programs written in languages other than APL2.

Further information about APL2 is available at http://www.ibm.com/software/awdtools/apl. Detailed information about APL2, Associated Processor 15, and Service Level 13’s other new facilities can be found in the *APL2 User’s Guide* through the Library link.
