---
vid: '10014050'
title: News from Sustaining Members
volume: '17'
issue: '3'
page: '8'
source: trad/v173/sus173.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2001
tags:
- implementations
- APL community
---

## Dyadic Systems Ltd

One of the major new features introduced in Dyalog APL Version 9 is the concept of a *Namespace Reference*. This allows you to assign namespaces, to pass namespaces as arguments to functions, and to have arrays of namespaces.

Just before Christmas, Dyadic released further extensions namely *Namespace Reference Array Expansion* and *Distributed Assignment*. These enhancements provide additional syntax to reference and assign variables in arrays of namespaces.

For example, if you represent a personnel database by having a separate namespace for each person; where each namespace contains variables that specify details for that person, you can now pick out (and assign) those variables for some or all of the namespaces *in a single expression*. For example:

```
      JOHN←⎕NS''
      JOHN.(FirstName LastName Age)←'John' 'Smith' 50
      PAUL←⎕NS''
      PAUL.(FirstName LastName Age)←'Paul' 'Brown' 44
      MEN←JOHN PAUL
```

`MEN` is an array of Namespace References.

The *Namespace Reference Array Expansion* enhancement allows you to reference variables in each of the namespaces as follows:

```
      MEN.FirstName
 John  Paul
      MEN.(LastName Age)
 Smith 50  Brown 44
```

You can also assign values to these variables using *Distributed Assignment*.

```
      MEN.Age+←1
      MEN.Age
51 44
```

Dyadic continues to work with Microsoft to integrate Dyalog APL into the .NET Framework and has begun to demonstrate its capabilities to APL audiences around the world. Among these new features is the abilility to use Dyalog APL as a scripting language for ASP.NET web pages and to write and call Web Services from Dyalog APL.

Dyadic is also pleased to announce the availability of Dyalog APL/M Version 9 BETA for LINUX. It is expected that the production version will be available during first quarter 2001.

***

## Soliton Associates Ltd.

### SAX 6.0 Announcement

Soliton is pleased to announce the availability of SAX 6.0. An Enterprise Edition is available for IBM AIX, Linux and Sun OS, and a Personal Edition for Linux.

SAX 6.0 is supported for:

- IBM AIX 4.3
- RedHat Linux 6.2 and 7.0 for Intel
- Sun OS 6 and 7

The SAX Personal Edition is royalty-free for personal use, and can be downloaded from the Soliton FTP site.

The SAX Personal Edition is functionally identical to the SAX Enterprise Edition, except in areas such as:

- IBM AIX and Sun OS support;
- IBM OS/390 connectivity;
- Oracle, Sybase and DB2 connectivity;
- CORBA, SOAP and XML utilities.

In addition to more extensive functionality, the SAX Enterprise Edition provides the services of an Account Manager, and responsive telephone and e-mail support.

SAX 6.0 is the first fully POSIX-compliant version of SAX. The same compiler and source code are now used for all supported platforms, with resulting benefits for maintainability and reliability. Moreover, Soliton has successfully performed experimental ports of SAX 6.0 to other platforms, for example HP/UX, demonstrating the advantages of POSIX. Soliton is following the MAC OS/X and FreeBSD platforms closely.

Enhancements in SAX 6.0 include:

- A new SVP (Shared Variable Processor) with no practical limit on the number of shares or the amount of memory used;
- A factor of up to 5 times increase in the speed of floating-point arithmetic;
- A factor of up to 10 times increase in the speed of sort (grade up and down);
- A new algorithm that speeds up “less”, “epsilon” and “iota” by 25 percent in most cases;
- A set of interpreter shortcuts that speed up the most common uses of “take” and “drop”;
- A new operator, “ply”, that applies a function n times;
- A new operator, “tessallation”, that divides an array into sub-arrays;
- An extension to “assignment” that allows “indirect” or “multiple” assignments.

SAX 6.0 introduces new glyphs: “@” and “&”, for the “from” and “merge” primitives currently denoted by “\{” and “\}”. The new glyphs are a first step towards reclaiming “\{” and “\}” for reuse in control structure and direct definition (dynamic function) facilities that are planned for future releases of SAX.

In SAX 6.0, the “old” glyphs for “from” and “merge” continue to be supported. To assist in their location and removal, a SAX start-up parameter has been added that enables tracing of their use.

### Documentation

The SAX 6.0 Documentation Set is approximately 700 pages in size, and consists of a Language Guide, a System Guide, and a Handbook.

The Documentation Set can be downloaded from the Soliton FTP site, or ordered in printed form. The printed Documentation Set includes a CD containing SAX 6.0 for Linux and a PDF version of the documentation. The price of a printed Documentation Set is US $120, including shipment.

### SAX Java Interface (SJI)

The SAX Java Interface (SJI) can be downloaded from the Soliton FTP site for use with the SAX Personal Edition.

Java provides a large and growing range of platform-independent interfaces and utilities. One function of the SJI is to provide SAX with access to Java interfaces, such those for ODBC/JDBC, CORBA and SOAP; and to Java utilities, such as those for sound, compression, encryption and drawing.

Another function of the SJI is the support of Java-based client applications for SAX, for example graphical user interfaces.

### SAX Integrated Development Environment (IDE)

Soliton continues to develop the SAX IDE (Integrated Development Environment) that was presented at APL Berlin 2000. The IDE is implemented in Java, because it is intended for multi-platform use, but the APL developer does not require Java knowledge to use it. The IDE will operate on any Java platform, for example Linux or Windows, simply requiring a network connection to a SAX server.

The first release of the SAX IDE will ease printing and editing, offer an APL explorer, a view over all objects in workspaces and files, and have contextual highlighting.

Because the IDE, as an application, is separate from SAX, the SAX S-task interface has been enhanced with transactions for transfer of objects to and from active APL workspaces. For use with the IDE, these objects are simply transferred to and from a set of Java objects, as required. The benefit of this simple architecture is that the IDE can fully utilize the facilities of Java, for example drag and drop, and nested trees, without extensive coding effort.

### Contacts

Please visit the Soliton web site at [www.soliton.com](http://www.soliton.com/) for information about the SAX Personal Edition.

For information about the SAX Enterprise Edition, please contact:

> Andrew MacLeod (+1 416 364 9355) for North America Laurie Howard (+31 20 646 4475) for Europe

***
