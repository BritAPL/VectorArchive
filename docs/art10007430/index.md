---
vid: '10007430'
title: Notes on Virtual Memory and the APL Workspace
authors:
- Bill Rutiser
byline: by Bill Rutiser (from a talk given at the 2003 APL2000 Conference)
volume: '21'
issue: '3'
page: '115'
source: trad/v213/rut213a.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- performance
- implementations
---

by Bill Rutiser (from a talk given at the 2003 APL2000 Conference)
{ .byline }

## A Little History

### 1950s

Machines were logically small but physically big. They were very expensive to buy and operate. Programs had full access to the entire machine. Programmers were responsible for all details of a program's operation. Machine time was scheduled and paid for in blocks.

### 1960s

Simple operating systems mediated I/O operations and transitions from job to job. Turn-around times under batch or job stream operation were measured in hours or days. Experimental time-sharing systems provided more or less immediate response to users.

### 1970s

Advances in hardware technology reduced the cost and size of computer hardware, which permitted logically larger mainframe systems and physically smaller mini-computers. Operating systems supported multi-programming. Several jobs could run more or less concurrently.

Emphasis was on increasing utilization of CPU by overlapping one program’s computation with another program’s I/O transfers. Access to and responsibility for I/O and storage management moved from programs to the operating system.

General and special purpose “time sharing” systems were in use but expensive.

### 1980s

Microprocessor technology enabled desktop and tower computers capable of supporting several users. Manufacturers of small Unix based systems temporarily proliferated. Hardware became cheap enough that personal computers were economically possible. Concerns about response time replaced concerns about hardware utilization.

Unix based systems directly supported concurrent operation of multiple programs with OS mediated I/O and resource management. Personal computer operating systems provided little more than support for loading programs and abstracting the lowest levels of hardware differences.

Local area networks came into use. Business applications distributed over multiple small networked machines became common.

### 1990s

PC processor speed and memory capacity increased while costs decreased. Graphical user interfaces, multiple concurrent applications, and larger disk storage capacity became increasingly common.

The Internet and the World Wide Web became widely accessible.

Business application architecture increasingly involved multiple programs running on multiple computers in multiple tiers tied together by multiple networks.

### 2000s

Personal computers have become large enough that users may have several large application programs open simultaneously. Workflow causes the user’s attention to shift from application to application.

## Evolution of Computer Memory Addressing

### Some concepts, definitions, and over-simplifications

Computer memory stores bits. Groups of bits can represent numbers, characters, or other things. Modern hardware generally provides direct support for groups of 8, 16, 32, and 64 bits. The hardware also supports certain data representations such as 8 bit characters, 16 or 32 bit integers, and 32 or 64 bit floating point numbers.

At the hardware level, machine instructions contain numbers that designate particular groups of stored bits. Such a number is a “memory address”. Almost all present day computers use “byte addresses” that designate a group of 8 bits. Depending on context, a byte address may refer to a single byte, or a group of adjacent bytes.

### Simple direct addressing

Memory is treated like a simple APL array. An address is simply an index into the array. There is only one such array and it is the entire memory.

Early programming required a programmer to explicitly associate data and memory addresses. One of the first uses of programming tools was to assign addresses to symbolic names.

### Memory protection

Practical use of multi-programming is much simplified by hardware support to ensure one program does not inadvertently modify another program’s memory. Numerous schemes have been used. All require that the hardware registers controlling the protection are accessible only to the operating system.

This, together with other considerations, leads to a distinction between system and user modes of processor operation.

### Based and segmented addressing schemes

Multi-programming requires a means to assign a distinct range of memory addresses to each program. The simplest scheme is to adjust all the addresses used by a program when it is loaded. This is time consuming and requires the program itself to observe appropriate conventions.

Simple hardware support adds a special “base register” to the CPU. The content of this register is automatically added to each memory address as it is used. The program can now be written to always use a range of addresses beginning at zero. The operating system sets the base register to the actual memory address of the segment of memory allocated to the currently running program.

Memory protection can be provided via a “limit register” setting the upper limit of allowed access. Base and limit registers are changed together.

### Memory swapping and time sharing

Observe that the introduction of the base and limit register permits an operating system to dynamically change the actual memory assigned to a running program:

- Suspend the program’s execution;
- Move all of its memory contents to a different base address;
- Change the base and limit registers; resume the program’s execution.

While a program is suspended, the contents of its memory can be written to auxiliary storage. Another program’s memory contents could be swapped into the vacated memory and run.

Some time-sharing systems worked this way. Response times were determined by the relative size of the memory and speed of access to the swap storage.

### Paging

Simple swapping schemes read in all of a program’s memory before continuing the program’s execution.

Most programs exhibit some locality of reference such that the recently used parts of its memory change slowly relative to the progress of execution. This locality arises because: instructions are executed in sequence with only occasional jumps to non-sequential addresses; a similar sequence of instructions is repeated in each traversal of a loop; related data is often stored at related addresses.

Paged memory systems divide memory into pieces of uniform size, commonly several thousand bytes. Such a piece of actual physical memory, either processor memory or auxiliary memory, is called a page-frame; the data itself is called a page. This distinction between pages and page-frames is often elided.

Hardware support for paged memory usually provides several special bits for each memory page-frame.

- One bit marks the frame’s contents as invalid. Any access to an invalid page raises an exception, called a page-fault, which is trapped by the OS. When a program is to be swapped into memory, the OS assigns a set of page-frames, marks them as invalid, and resumes the program’s execution. When the program accesses a marked page, the OS responds to the page-fault by suspending the program, reading the page into the page-frame, and then continuing the program’s execution.

- A second bit is set, automatically, by the processor, whenever a page-frame’s content changes. The OS resets this bit each time a page is read in. If the bit is still reset when the page is to be swapped out, the OS knows the page has not changed since last being paged in and, if the copy in the disk swap area has not been changed, the OS can skip writing out the page.

    A third bit is set by each reference to the page-frame. By periodically resetting these “reference” bits, an OS can determine whether a page has recently been used.

### Memory mapping and address translation

Paging as described above could be used to support segmented memory systems. However, modern processors contain yet one more piece of hardware support.

A special set of tables, maintained by the operating system, is used to map each address used by a program to a particular page-frame. The only relation between the address used by the program and the physical address of the page-frame is the number in the table.

Conceptually, at least, each address used by a program is broken into two parts. The high-order part is used as an index into an OS maintained table of page-frame addresses. The low-order part is used as an offset from the beginning of that page-frame. The processor hardware automatically performs this address translation for each access.

The hardware maintains a cache of recently translated page addresses so as to bypass most actual accesses of the page-mapping table. The potentially large tables, one for each process, are themselves stored in paged memory subject to swapping.

## Some Virtual Memory Terms

“Virtual memory” is the apparent simple directly addressed memory used by application programs. An address used to designate locations in virtual memory is a “virtual address”. Some writers confusingly and incorrectly use “Virtual Memory” to refer to swap space or paging file as defined below.

“Real memory” is physical hardware memory, most of which is used as page-frames for mapping virtual memory. The rest contains OS kernel programs and data.

“Swap space” is an area of disk storage used as page-frames to contain pages that are paged out. “Paging file” is synonymous. “Paging” is the I/O activity used in transferring pages between swap space and real memory.

“Virtual address space” has several closely related meanings:

- The potential range of addresses that could be used by a program running in a particular environment;
- The actual virtual addresses and data in use by a program.

A “process” is an OS defined abstraction, consisting of a “virtual address space” , a current point of program execution, and assorted other resources such as open files, synchronizing locks, permissions, identities, etc.

“Thrashing” describes a state of sustained high paging with little useful work done between page faults. Thrashing arises when a process or set of processes repeatedly and rapidly touches more pages than can simultaneously be allocated page-frames. Thrashing has been called an “unproductive flutter of pages”. Although an OS can sometimes detect thrashing and suppress it by adjusting the mix of active processes, an OS cannot prevent thrashing entirely caused by a single process actively using more pages than the machine has page-frames.

## Virtual Memory in Windows

The Intel IA32 architecture defines a 32-bit address space. This means that the processor family’s instruction and data formats have room for 32 bit addresses. Most actual computers have considerably less than 4 gigabytes of real memory but such a machine is no longer impossible.

Workstation versions of Windows NT divide the 32-bit address space into two equal parts. “System memory” is reserved for the use of NT itself and “user memory” is mostly available for the application process. Note that some small areas of user memory are also reserved, mostly for compatibility reasons, and that some process specific data is stored in user memory. In addition, data allocated by NT on behalf of the application is in user memory.

NT organizes virtual memory into 4k pages. 64K segments are also used for allocation purposes.

### NT virtual memory allocation

Addresses are allocated separately from memory. Initially all addresses in the user space are free and associated with no data.

The WIN32 API contains a pair of functions, “VirtualAlloc” and “VirtualFree”. In one mode, these functions reserve and release regions of address space. Such regions are specified by size in multiples of 64K. Reservation of a region does not consume significant physical resources; it is simply an entry in the system’s tables for the process. Releasing a region returns its addresses to free status.

In a second mode, “VirtualAlloc” and “VirtualFree” commit and de-commit specific pages within reserved address space. Committing a page allocates a page-frame in the paging file and adds an appropriate entry to the address translation tables. The first use of the newly committed page will cause a memory page-frame to be assigned. Subsequently the page will be paged out and in as necessary. De-committing a page discards its data and makes its memory and page-file frames available for other uses.

A process can have a large contiguous region of its address space reserved but only a few pages committed. A process can have multiple contiguous (but not overlapping) reserved regions of various sizes for various purposes.

Like other systems, NT can map one page frame into several address spaces, possibly at different page addresses. This permits several processes to share one physical copy of read-only data or executable code. Page-frames are also used to buffer and cache disk files.

## APL+Win and Virtual Memory

### APL and memory management

An APL interpreter is essentially an exercise in effective management of dynamically allocated memory. Several aspects of APL as a language contribute.

- Primitive APL functions produce new values. Few APL constructs directly modify existing data objects. Instead newly created arrays replace existing arrays.
- The dimensions and representation of APL data is not readily predictable until the actual execution of an expression.
- The arrays generated by an APL program may range in size over eight or more orders of magnitude.
- Temporary objects half the size of a computers memory easily and usefully arise.
- The lifetimes of arrays and other objects overlap and are unpredictable.

Memory management for large pools of small objects has been well studied by computer science and is fairly well understood. There is little literature on the management of rapidly changing pools of large objects.

Historically, APL programmers have been hampered by fixed size workspaces. APL\\360 based time-sharing systems swapped active workspaces between fixed size memory and disk slots. Maximum workspace size was determined by a compromise between memory costs and response time.

### Memory Management

The memory manager in an APL interpreter begins with a large, usually contiguous, area of unused memory.

The manager maintains an inventory of free pieces of memory.

Allocation requests are satisfied by selecting an entire piece or by subdividing a larger piece into free and allocated parts.

Eventually, an initial contiguous piece of free memory will have become subdivided into many smaller pieces, some free and some allocated.

### Fragmentation and compaction

Depending on the statistical distribution of allocation sizes and lifetimes, the distribution of sizes in the free piece inventory may reach a stable state or the inventory may fragment into unusable small pieces. A memory manager’s policies for choosing free blocks for allocation and splitting affect the rate of fragmentation but no policy can always avoid fragmentation.

Fragmentation is overcome by moving allocated objects so that the free pieces are adjacent and coalesced into larger pieces. Such compaction is potentially costly since it may require moving most of the objects in the workspace. In addition, when objects contain addresses of other objects, these pointers must be adjusted when the target object is relocated. A compacting memory manager may compact preemptively or only as needed.

### The APL+Win Interpreter

The general architecture of the APL+Win interpreter originated in 1982 as an interpreter for a Unix based workstation with 1Mbyte of RAM and a 20Mbyte hard drive. That machine also had serial ports connected to terminals for a team of five developers.

The UNIX virtual memory model was quite simple.

- Each process had an address space that grew upward from zero and down from a very high address. In NT terms, memory was committed for an upwardly growing bottom part and a stack that grew down from the top of the address space.
- A new process began with its program loaded at the bottom of the address space together with a small amount of per process data. The program extended the bottom part upwardly as needed.
- Most programs used the malloc() routines from the C library to manage smallish allocations.

Our design allocated a fixed size workspace “slot” just above the program. The workspace size could be increased or decreased by freeing and reallocating at a different size. Generally there was little need to change size. Users were advised to keep workspaces smaller than the machine size reduced by system overhead. The kernels paging machinery gracefully handled switching to another process.

Over the next two decades, the interpreter was readily ported to many other systems as diverse as DOS, VMS, and Windows.

## Changes

### Release 4.0 and before

The memory manager policies are intended for environments where hardware and operating system keep all of the active workspace in real memory while APL is active. The memory manager’s policies treat all parts of the active workspace uniformly. Its compaction strategy is intended to minimize CPU time by only compacting when necessary. There is also a periodic compaction to mitigate the effect on allocation of large numbers of scraps.

The implementation and its policies are largely unchanged from those in the early UNIX APL\*PLUS products.

### Release 5.0

While the memory manager has been entirely rewritten, to take advantage of modern system programming tools, and to prepare it for future changes, most of it is similar in effect to the old memory manager. When the interpreter is started, it asks the NT kernel to reserve a large contiguous region of the process’s address space. As explained above, this reservation does not use significant actual memory. This region is referred to as the slot.

As the needs of the workspace change, memory is committed and de-committed at each end of the slot. The SI stack is maintained at the bottom of the slot while data and functions are placed in the upper committed area. Workspace compaction moves allocated objects upwards leaving a contiguous area of unused but committed storage.

The manager is controlled by several policies:

- One decides when compactions are worthwhile.
- Another decides when to increase or reduce the size of the upper committed area and by how much.
- A third controls the expansion and contraction of the lower committed area.

These policies will be adjusted to balance the use of committed memory against the CPU cost of too frequent compactions. Indeed, they have changed a lot during the last year.

The intent is that in most situations, a default slot size, proportional to the amount of real memory in the machine, will be appropriate. The chosen default is 75% or the machines apparent real memory.

## Some Questions

### Can I make the slot size larger than my machine’s real memory and have a really big active WS?

Perhaps, but it’s very unlikely that you will be at all happy with the result. Paging transfer times are measured in milliseconds. Machine instruction times are measured in nanoseconds. That’s a difference of six orders of magnitude. Your disk drive is likely to wear out before your program completes execution.

### Why would I ever want to make the slot size smaller?

APL shares the address space with any in-process ActiveX objects it is using. If an object should require a very large part of the address space or needs most of your machines memory to avoid thrashing, then you may need to adjust the slot size to balance the conflicting needs. Note that in this context “very large” means 100’s of megabytes or more.

### Can I change the slot size with the )CLEAR command?

No. No need has been demonstrated for such a facility. The eventual release system will be arranged to default to a generally useful slot size without additional parameters.

### How do I specify the slot size?

With the initial builds, use a command line parameter or set “\[Config\]Wssize” in your “.ini” file. In the future, the interpreter will default to a generally useful slot size and will have means to specify the slot size as a percentage of real memory.
