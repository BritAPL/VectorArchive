---
vid: '10013970'
title: News from Sustaining Members
volume: '16'
issue: '2'
page: '14'
source: trad/v162/susnews.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1999
tags:
- implementations
- APL community
---

## Soliton Associates Ltd.

Soliton Associates Ltd. is pleased to announce the future availability of SHARP APL for Linux. SHARP APL for AIX and Solaris have been used by Soliton’s corporate customers since 1994. SHARP APL for Linux is the result of a POSIX port of the AIX/Solaris products. SHARP APL for Linux will be available in October 1999 as a beta release and a production version is planned for the first quarter of 2000.

Personal licences for SHARP APL for Linux will be available free of charge.

### A new platform

Linux extends the SHARP APL product line that includes OS/390, AIX and Solaris. Linux can run on personal computers and workstations. The initial investment for setting up Linux is rather small and SHARP APL for Linux is an excellent product for prototyping and market development. With the Linux platform, SHARP APL offers scalability and a growth path for customer applications.

Linux is extremely reliable and efficient. Soliton considers Linux as ‘industrial strength’ and well suited for SHARP APL. Linux growth will benefit our customers.

Commercial licences for SHARP APL for Linux offer different levels of support and regular upgrades. Personal licences do not have support, but problem reports will be accepted.

### Promoting APL

In our effort to promote APL, SHARP APL for Linux can be used, at no cost, for personal use. The free version of the product is complete. The product will be FTP-able from any Linux distribution site and from Soliton’s home page [(www.soliton.com/linux)](http://www.soliton.com/linux). There is no required registration process for the personal version. The documentation in PDF format is also free. We may consider selling a CD or a printed manual in the future.

We wish to promote APL among college and university students. By having a Linux version, we wish to encourage resellers and consultants to include SHARP APL in their solutions.

### Architecture

SHARP APL for Linux is a true multi-user system. Linux, like Unix, offers its users private disk areas and security. Consequently, SHARP APL for Linux offers private workspaces and files.

Because it runs in a server environment (Linux), you can have several users logged in at the same time running different applications unaware of each other. These users could be on different Linux machines or running W/98 with an X-Server, a JVM (Java VM), Telnet or a browser. As long as you are TCP connected to Linux and have a user-id, you can run APL.

The same multi-user architecture enables an application writer or user to have several APL sessions running concurrently with excellent performance. These sessions are called tasks. A shared variable processor enables tasks to share data with each other, either within the same user-id or system-wide. The shared variable processor is also the mechanism used to contact APs (auxiliary processors).

The workspace size is limited to 2GB but Linux will restrict this limit further depending on its swapping area and memory. Intel Linux kernel 2.0.36 and above is the only supported platform.

### Components

SHARP APL for Linux comes with:

- A interpreter that runs as a process;
- A file system where files can be shared among users;
- A private or global shared variable processor;
- An interface to C-code through the Intrinsic Function (IF) facility;
- A set of IFs for Linux file operations, Linux commands;
- A set of IFs for Socket calls;
- A host AP;
- A full screen function editor;
- A set of public workspaces for date calculation, printing, import and export of software;
- A utility to de-fragment files;
- All documentation in PDF format.

Additionally, a set of utilities will be available (not included in the beta package):

- SCL or Socket Client Library to send, and read E-mails from APL; and to FTP files to and from APL;
- SSM or Socket Server Manager to run socket servers including an FTP daemon;
- SBI or Browser Interface to enable applications to have a browser front-end;
- SJI or Java Interface to enable applications to have a Java front-end or use Java services like JDBC.

### GUI

The Graphical User Interface for application development is based on Java and the “SJI”. This approach is consistent with the server architecture of SHARP APL for Linux where the interface to an application is separated from the application server. This enables the ‘client’ to an application to be separated by a network, possibly the Internet and the choice of Java offers portability across many platforms including Windows (95, 98, NT, 2000), Mac-OS and most Unix implementations.

In particular, an Intranet connected machine with a Java VM (Netscape or IE4 for example) will be able to run GUI applications on Linux removing the need to have a second desktop. They can share a Linux machine with other users and have the same functionality as if they physically had it on their desks.

The SJI simplifies and provides the Java front-end capability for the application developers. Many will choose to call Java from APL via the SJI. However, it is also possible for a Java program to ‘call’ APL by using the SSM API for Java.

### Language

The SHARP APL language is based upon Ken Iverson’s APL dictionary. It has enclosed and heterogeneous arrays. Functionally, it is equal to APL2 but the syntax differs slightly. It is not difficult to program SHARP APL when you know APL2. Events and error trapping are similar to Dyalog APL. APL objects can be packed together into packages that can be stored on file and manipulated. Most large applications page-in code from files that have packages of functions and variables. A rank operator and the notion of rank in all the APL primitives enable elegant programming. A file system is incorporated in the language.

### IDE – Integrated Development Environment

A session manager coupled with a full screen function editor simplifies the interactions with APL during application development. This offers portability and capability to develop APL remotely, separated from the server. An X-server on a Windows platform can be used with the same benefit as using a Linux desktop.

We are currently developing an IDE within the SJI framework. An experimental version should be available by year-end.

### Support

The personal version of SHARP APL for Linux does not have any support. However, problem reports can be submitted to sax@soliton.com. We plan to offer comprehensive support plans to our corporate customers. Please contact Soliton for further details.

### Contact Information

Please refer to [http://www.soliton.com/linux](http://www.soliton.com/linux) for up to date information. For commercial licenses, please contact Laurie Howard ([ljh@soliton.com](mailto:ljh@soliton.com)) at +31 20 646 4475 (Europe) or Nancy Lamb ([nancy@soliton.com](mailto:nancy@soliton.com)) at (716) 256-6466 (North America).

***

## Dyadic Systems Ltd.

At APL99, Dyadic announced that it was developing a high-level interface to ActiveX Data Objects (ADO). This new Microsoft technology promises rapid access to data of all sorts including data in relational databases. ADO is a successor to Microsoft’s ODBC technology and works with existing ODBC drivers. This means that it can be used in place of SQAPL. During APL99, Dyadic ran two ADO workshops demonstrating the use of the interface.

Dyadic has recently announced the availability of the LAPACK and FFTW dynamic link libraries which have been customised for the Dyalog APL `⎕NA` interface.

LAPACK is a subroutine library for solving the most commonly occurring problems in numerical linear algebra. It is public-domain software, and can be used freely.

FFTW (Fast Fourier Transform for Windows) is free software, released under the GNU General Public License (GPL). To complement these DLLs, Dyadic supplies a MATH workspace that provides a high-level interface to the underlying C functions which are generally acknowledged to be among the fastest available for Windows.

Other recent enhancements, provided to DSS (Dyadic Support Service) subscribers, include a number of improvements to the Grid object and additional features for the Tracer (debugger).

Dyadic is nearing completion of Dyalog APL Version 8.2 for UNIX (APL/M) on Sun Solaris and IBM AIX and expects to announce a Linux implementation in early 2000. Version 8.2 for UNIX and Linux is almost completely compatible with Version 8.2 for the PC and workspaces developed on a PC may simply be loaded and run under UNIX.

***

## Causeway Graphical Systems Ltd.

New product releases now available ...

- *CausewayPro* for Dyalog 8.2 adds support for the Calendar, Splitter and ToolControl objects. Causeway treats the new toolbars as one-sided splitter bars, and will automatically move or resize any other objects which are affected when the bar changes size. There are many small improvements in the Designer (you can now set common properties on multiple objects, copy and paste properties and so on), and there is a new namespace to help edit bitmap artwork for toolbars and imagelists.
- *RainPro* adds support for mapped images (GIF or PNG format) to allow fully interactive graphics on your web site. All charts now have the option of high-colour, using the ‘#FF’ notation common to the web. Markers and fill patterns are now easy to define, and there is automatic support for trellis layouts to show slices through multi-dimensional data.
- *Newleaf* also supports high-colour, as well as background colour in table cells. Tables generated using Spread now have a ‘fit to page’ option which scales the output to fit to n pages wide, or to fit entirely on 1 page.

Users with licence numbers after 98041 may upgrade free; please download the software from the web site and contact us for the passwords and updated manuals.

We are delighted to welcome Jonathan Manktelow to Causeway; he has worked with the Causeway software for nearly 2 years, as well as being a highly professional Delphi and C++ developer. One of his first jobs will be to implement a good working subset of RainPro on Sharp APL for Linux; this will be available on the same terms as the interpreter.

***
