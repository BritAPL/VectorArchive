---
vid: '10003300'
title: A VSAM Simulation for APL*PLUS II
authors:
- Allan Gay
byline: by Allan Gay
volume: '10'
issue: '3'
page: '133'
source: trad/v103/gay103_133.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- data and files
- development practice
---

by Allan Gay <span class="affil"></span> <span class="email"></span>
{ .byline }

## Introduction

This article describes a VSAM simulation programmed in APL\*PlusII/386 and used by some recently-migrated mainframe APL2/TSO applications.

For convenience, the discussion is divided into the following sections:

I Overview  
II KSDSs, real and simulated  
III Using the simulation  
IV Migrating a mainframe KSDS  
V Conclusion

## I. Overview

The simulation comprises a workspace which may be copied into an application workspace to provide means of operating simulated VSAM key sequenced datasets (KSDSs) which reside on PC.

The workspace includes a set of public functions corresponding to the documented AP123 opcodes \[VSAPLTUG\].

A simulated KSDS is an APL\*PlusII/386 component file containing a mandatory arrangement of control components. Each VSAM data record is stored as a separate component.

### Features

A separate APL\*PlusII/386 component file is used for each simulated KSDS.

Alternate indexes are supported. If declared, they are imbedded in the component file concerned. A simulated KSDS’ base cluster and its alternate indexes may be open and accessed concurrently if desired. Multiple simulated KSDSs may be open concurrently, too.

Only key-sequenced datasets (KSDS) are simulated. VSAM’s entry-sequenced and relative-record dataset types are not supported.

The full repertoire of access opcodes offered by mainframe AP123 is implemented. Operations to open, position and close a KSDS are provided. Records may be read, written and erased. Access may be sequential as well as by key. Also programmed is a bogus read-backwards opcode "B".

Each AP123 opcode is simulated by a corresponding APL\*PlusII/386 function. The function arguments and results serve the role of the auxiliary processor’s CTL and DAT shared variables. This makes converting migrated mainframe application code a straightforward clerical task. During our own migration project, we typically spent about two hours per workspace reviewing and recasting AP123 calls.

## II. KSDSs, real and simulated

### What is a KSDS?

From the application standpoint, a KSDS is a matrix (perhaps with a ragged right-hand edge) with a keyfield stripe running from top to bottom of it. If the KSDS has any alternate indexes, they are just extra keyfield stripes down this imaginary matrix. Each row of the matrix is a separate record, and access is on an individual record basis.

Records can be read both sequentially and directly. When you read a record directly, you present a keyfield value and receive the record which bears that value in its keyfield. The applicable keyfield is established when the KSDS is allocated: if you allocate the base cluster, the primary keyfield is active, and if you allocate an alternate index, the relevant alternative keyfield applies.

When you read sequentially, the records are returned to you one by one in ascending sequence per the applicable keyfield.

### A simulated KSDS

A simulated KSDS is an APL\*PlusII/386 component file. The first ten components are reserved for control information and documentation, and include some spare components in case of future design extensions. Each subsequent component hosts one KSDS data record.

When adding a new record, any component released by a prior record deletion will be re-used; if there are no such components, a new component is appended to the file. Record ordering is therefore roughly chronological, with some inconsistencies.

The first ten components are: (1) freeform documentation, (2) the master index relating keys to component numbers, (3) a standard character vector identifying the file as a simulated KSDS and checked by the simulation at open time, and (4-10) empty vector spares.

### The Master Index Component

The master index component keeps the component number for each record key. Access is always via the master index, even when an application is accessing the simulated KSDS sequentially and is not supplying keys.

The component is a five-item nested vector. The first two items map the record keys to the corresponding components, being respectively a text matrix of keys and a vector of component numbers. The third and fourth items identify the positions of keyfields within the matrix of keys and within the records themselves. The fifth item contains padding and is used to buffer the effects of recordcount fluctuations upon the size of the master index component.

### Buffering the effects of a fluctuating record count

As users of component filing systems will know, the "expanding replace" is a phenomonon which can waste a lot of space in a component file. An expanding replace occurs when an enlarged component is refiled. Because the component is too large to fit into the space it previously occupied, it is written elsewhere, within the file’s free space, and the space which it previously occupied becomes orphaned until a file reorganisation is instigated by the user.

When a simulated KSDS is growing, new keys are being added to the master index component. This component is refiled after each addition. If the component were larger, an expanding replace would occur. Eventually, the component file would reach an enormous size, although much of the space it contained would be orphaned.

It is to prevent this that the master index component is overallocated by including a padding item. By adjusting the size of the padding item, the overall size of the master index component is mostly maintained at a constant figure. The effect is to buffer the effects of recordcount fluctuations, minimising the number of expanding replaces occurring when the master index component is refiled.

A manifest constant held in a global variable specifies the overallocation percentage required. For example, imagine we create a simulated KSDS with 100 records and specify a 10% overallocation. We get a padding item which is 10% the size of the rest of the master index. We can add ten more records before all the padding is used up. When we add the eleventh record, padding sufficient for eleven more records (10% of 111 records) is added and the first expanding replace occurs.

Similarly, if the simulated KSDS were to shrink through record deletion, the padding item would expand until the number of records fell below 90, at which time it would be reduced commensurately. Space thus released would be reclaimable via ŒFDUP.

In a file with a steadily growing recordcount, a large overallocation percentage gives advantages. In a file which is oscillating about some mean recordcount, a well-chosen percentage can completely eliminate expanding replaces of the master index component.

Internal consistency of the various filed components is not jeopardised by ŒFDUP.

### Record components

Record components contain character vectors. Applications may deem the records to be composed of fields, and may deem those fields to contain many forms of data. However, the VSAM simulation merely stores the bytes and leaves their interpretation to the applications.

From the application viewpoint, record fields can hold data in any of the mainframe formats - EBCDIC text, packed decimal, binary fullword, etc. This is unlikely to present any additional problem to a migrated mainframe application, provided that the records have been migrated correctly. Routines which encoded and decoded non-EBCDIC data on the mainframe should still work, in converted form, on the PC. Of course, this is not a VSAM-related issue: the same points apply to any dataset migrated from the mainframe regardless of access method.

## III. Using the simulation

### Functions

The functions supplied in the simulation workspace are classed as public or private. The public functions are identifiable by their naming convention. The private functions operate directly upon temporary global variables and should not be used explicitly by applications.

The simulation provides a corresponding public function for each AP123 opcode. These functions are named in the form "VSKS..", where ".." is the AP123 opcode. For example, mainframe opcode RU (Read for Update) becomes function VSKSRU in the APL\*PlusII/386 context.

In addition to the opcode functions, public functions VSAMchk, VSKSB, and VSKSSF are provided. The first may be used to filter returncodes, the second is a pseudo-opcode "B" (for Backwards read), and the last enables the application to switch between open simulated KSDS indexes.

The public functions are:

<table><tb>
<tr>
<th>   FUNCTION   PURPOSE              ⍺-ARG              ⍵-ARG</th>

</tr>
<tr>
<td>   --------   ------------------   ----------------   ---------------</td>

</tr>
<tr>
<td>   VSAMchk -  Check rc             acceptable rcs     rc to check</td>

</tr>
<tr>
<td>   VSKSB   -  Backwards read                          generic key *</td>

</tr>
<tr>
<td>   VSKSC   -  Close current view</td>

</tr>
<tr>
<td>   VSKSE   -  Erase record                            recordkey</td>

</tr>
<tr>
<td>   VSKSKF  -  Key Feedback</td>

</tr>
<tr>
<td>   VSKSOC  -  Open &amp; Clear         indexno (1=base)   libno filename</td>

</tr>
<tr>
<td>   VSKSOR  -  Open for Read        indexno (1=base)   libno filename</td>

</tr>
<tr>
<td>   VSKSOU  -  Open for Update      indexno (1=base)   libno filename</td>

</tr>
<tr>
<td>   VSKSOW  -  Open for Write       indexno (1=base)   libno filename</td>

</tr>
<tr>
<td>   VSKSPO  -  Position                                generic key **</td>

</tr>
<tr>
<td>   VSKSR   -  Read                                    recordkey *</td>

</tr>
<tr>
<td>   VSKSRU  -  Read for Update                         recordkey *</td>

</tr>
<tr>
<td>   VSKSSF  -  Switch File                             fileno</td>

</tr>
<tr>
<td>   VSKSW   -  Write                                   record</td>

</tr>
</tb></table>

\* when arg is empty, read next record in indicated direction. \*\* when arg is empty, position to first record in view.

### Programming considerations

Because the concept of preallocating files does not exist in the PC context, the openfile operations (AP123’s OC OR OU OW opcodes) take a component filename specification instead of a DDname. The spec must be in the form "libno filename", which means library numbers must be predefined in the APL\*PlusII/386 configuration file used at APL\*PlusII/386 startup.

Shared variables are not available in the APL\*PlusII/386 environment. Since there is no AP123 to record the state of play, temporary global variables are used instead. There is no ŒSVO analogue: instead the open is simply issued (by running the function corresponding to the desired opcode), specifying APL\*PlusII/386 library number and component filename as function arguments. A file-number is returned from the OC OR OU OW services and must be retained for the simulation to use in indexing the temporary global variables.

The simulation uses the temporary global variables to host control-data for the simulated KSDSs which are currently open. It is not necessary explicitly to pre-create these variables. They are automatically created when the first simulated KSDS is opened. They are automatically expunged when the last simulated KSDS is closed.

In mainframe AP123 programming, services are accessed by specifying one- or two-letter opcodes via the shared control variable, parameter information being preassigned to the shared data variable. The choice of shared variable pair determines which KSDS (whether base-cluster or alternate-index) of those currently open is accessed.

In the simulation, there are no shared variables. Instead, the opcode is implied by the application’s choice of public function, and the parameter information is specified via that function’s arguments. The current "view" is always accessed.

Every open base-cluster or alternate-index is termed a view, and every view has a unique file-number which was assigned when it was opened. The application should capture this number (returned by the open routines VSKSOC, VSKSOR, VSKSOU and VSKSOW).

One view (by default, the latest opened) is deemed current, and it is upon the current view that all I/O operations are transacted. To make another view current, the application specifies its number to the VSKSSF "Switch File" function.

When a simulated KSDS is first opened, its master index component is read into the workspace, and some additional view-related information is built. If another of that simulated KSDS’ indexes is subsequently opened, the same copy of the master index is re-used, and another set of view-related information is built. This approach enables an application to modify a simulated KSDS via any of its open indexes without the risk of conflicting updates. Logic to synchronise view-related information is included.

### Converting a migrated mainframe workspace

### Conversion is a manual process.

First, all TSO ALLOC commands and ŒSVOs are removed completely.

Next, each use of the AP123 opcodes is replaced by the equivalent simulation function. Assignments to DAT become function args, and assignments from DAT are now taken from function results. CTL returncodes are also taken from function results. If multiple pairs of shared variables were used, the VSKSSF function is added to select the appropriate file-number.

Next, each open function call is recoded to supply a libno and a filename (instead of supplying a DDname). An additional integer argument designates the index to be used. The indexes are numbered with the origin-1 indices of their respective keyfields in the master index component. The base cluster is always index number 1.

Finally, paired calls to functions VSKSSF and VSKSC are coded in place of every ŒSVR for which no prior close was issued, and all ŒSVRs are removed.

### Returncodes

Where appropriate, the returncodes documented for AP123 are used \[VSAPLTUG\]. Additional codes are provided for attempts to access nonexistent indexes or views. The extra codes form a new series associated with a new major returncode by which they are differentiated from the standard repertoire.

### Size

The simulation requires 127K of ŒWA when no simulated KSDSs have yet been opened.

Because the master index component of each open KSDS is held in global variables, the operating space requirement depends on the number of KSDSs open, their keylengths and their recordcounts, plus some additional penalty for each open alternate index. In short, the simulation trades space for speed.

For one application which we migrated, opening the base cluster on a 7,459-record simulated KSDS with 32-byte keys, took 593K of ŒWA. Going on to open that KSDS’ sole alternate index, which featured 8-byte keys, took an extra 119K of ŒWA.

Another similarly configured simulated KSDS contained 7,487 records and yielded proportionately similar results. Because the PC had 16MB of RAM (the limit for that particular machine but not, of course, for APL\*PlusII/386) and the application workspace amounted to only 815K including all simulations, there was no hint of a ŒWA problem.

## IV. Migrating a mainframe KSDS

### Migrating a KSDS to the PC

The records should first be dumped to a flat sequential file via the Access Method Services REPRO utility. The sequential file should then be transferred to PC, using a product such as IRMA or PC3270. Finally, APL\*PlusII/386 must be used to build a component file containing the correct arrangement of components.

If the KSDS records contain only EBCDIC text, translation to ASCII should be selected during transfer of the sequential file to the PC. If, however, the KSDS records contain numeric fields or APL expressions, translation must not be selected there. Instead, translation must be performed on a field by field basis when the records are loaded into the simulated KSDS.

Treating numerical quantities (packed decimal, binary fullword, and so on) as if they were EBCDIC text and translating their bytes to ASCII is definitely a mistake.

Matters can also get interesting if the records actually contain executable APL statements. Translation from EBCDIC to ASCII is then inappropriate. Translation from APL2’s ŒAV to APL\*PlusII/386’s ŒAV is required instead.

### Writing a KSDS builder

Writing a function to build a simulated ksds from a sequential file is conceptually quite straightforward.

First, the function should write the standard ten preliminary components, the second being a dummy as placeholder for the master index.

Second, the records should be read, processed, and appended as individual components. Processing a record entails translating any EBCDIC or APL fields within it, and then noting its key (or keys, if the KSDS includes alternate indexes).

Third, the master index component should be constructed from the accumulated key data, filed in the position occupied by its placeholder, and the component file reorganised via ŒFDUP.

As ever, the devil is in the detail. When a KSDS contains multiple record types, each incorporating many fields and using many data formats, the record-processing logic will be extensive.

## V. Conclusions

The migration of a given mainframe KSDS is a one-time affair. Given good record layout documentation, writing the record conversion logic is merely a tedious matter of coding the right field indices against the record vector. This is true for all types of file, not just VSAM.

This is a comprehensive simulation and its internals are necessarily complex. However, its external interface is systematic and largely preserves the look of its mainframe precursor. This keeps the work of converting migrated mainframe logic to a minimum.

Performance was not quantified, but it was certainly adequate on the top-end ’486 machines employed.

Next: simulating Defined Operators.

### References

1. \[VSAPLTUG\] VSAPL for TSO Terminal User’s Guide,
2. IBM, SH20-9180.

(webpage generated: 5 March 2006, 18:07)
