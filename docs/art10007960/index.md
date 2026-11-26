---
vid: '10007960'
title: The BAA Membership Database
authors:
- Nicholas Small
byline: by Nicholas Small
volume: '11'
issue: '1'
page: '119'
source: trad/v111/small111_119.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- data and files
- applications
- APL community
---

by Nicholas Small
{ .byline }

Our membership database system was re-written in APL\*PLUS/PC some little while ago. As the techniques used would be suitable for any simple database, it was thought that an outline of its operation might be of interest to members.

The primary purposes of the membership database are the maintenance of details relating to members and the printing of labels for meeting notices, Vector mailings, and membership cards. In addition, addresses are accessed for the sending of invoices and processing of credit card payments. There are a number of potential additional uses for the database, for example to record those members who are willing to answer queries from people learning APL.

The use of personal data in the database is governed by the Data Protection Act, our registration under which is described in Vector 9.3, p.9. Your Committee has resolved that members’ details shall not be revealed to third parties for mailing purposes.

### History

The original database was run in a proprietary database package that was not owned by the Association, nor was it a package that was familiar to the (then) new administrative assistant or her technical support team (myself). The only software on the administrative assistant’s PC (an 80286 machine) that could be applied to the task was APL\*PLUS/PC (version 7) and by chance I happened to have a database package in that very language, written to process time-sheets and monitor projects for a company long since defunct; I could see that by throwing away about 95% of the application code we would be left with something that could readily be converted to the task in hand.

My database package had used component files to store data, but for the membership system I opted for a native DOS file with fixed length records for the main data (component files are retained for control data). This format was chosen for two reasons: such a file can be used by a program as a direct access file, to allow fast input and output; also, it is possible to read and manipulate the data using one’s favourite text editor.

When deciding on data handling techniques for a database, the options available depend on the size of the database, both in terms of number of records and in the size of individual records. Our membership database is currently rather more than 250k bytes (there are nearly 900 records of length 284 bytes, including CR/LF). If the database were half this size there would be scope to manipulate it as a single object within the APL workspace. At the other extreme, if the file was much more than twice its current size then a simple text editor would not read it in its entirety either. (Even with a PC that had extended memory and an APL interpreter and text editor to make use of it, these limitations would apply for a database of more than a certain size.)

### Description of the Data

The main fields in the database are:

- a zone code, used to group labels for posting;
- a unique membership number (previously, this had only given a unique identifier when combined with the zone code, so some re-numbering was needed);
- a sort field (based on Name or organisation);
- name and address (this is three-quarters of the record);
- membership details and contact dates.

There are no separators between fields; this reduces the amount of processing required, at the cost of using global variables to index fields in a record.

Four component files are used, three of which are log files – for membership cards, mailing label runs, and invoice details. The fourth file contains semi-permanent data, for example correspondence and remittance addresses for invoices, subscription rates, the membership numbers of the current committee, and a template for the display of a membership record.

### Program Structure

The system has a simple hierarchical structure, with selection by position of the menu bar. The main menu has just three sub-menus – one to access membership records, one to print labels, and Utilities to do everything else. On the first of these, there is an option to add a new record and two options to select a record: either by membership number, for which there is a prompt, or via the sort field, with a prompt that leads to a display of the (sorted) sort fields as a menu, starting at an appropriate point.

The labels menu starts with two toggle items, one setting the number of lines to be printed per label (depending on what stationery is loaded in the printer) and the other determining whether or not expiry dates are printed on the labels. Most of the other options actually print labels; the list that is referred to is a vector of membership numbers which is built up by pressing a function key when editing the membership record.

The Utilities menu includes options to call functions such as MCOUNTS, which provides an analysis of membership numbers and Vector circulation.

There is a start-up routine which defines constants, reads the semi-permanent data from the component file, and gets all membership numbers and sort fields from the database (the sort field is 30 characters, so these take up about 32k bytes, which is not a problem in terms of workspace size). The defined constants include all application-specific variables used in the workspace, ranging from menu text to the location in a membership record of, for example, the membership class field; consequently, when a change has to be made, things are all in one place.

### Workspace Functions

The main functions in the workspace may be divided into three groups: screen handling functions; functions to read from or write to the database file; and functions for printing.

The functions for screen handling – menus and record editing – are based on the utilities supplied with APL\*PLUS/PC in the INPUT workspace. Only minor modifications were made to these functions, for example, to accommodate the chosen screen layout – a couple of lines at the top for titles and a few lines at the bottom for instructions and messages, with the works in between. The function sFIELDS takes a mask of the form shown in Figure 1, returns the parameters for the fields defined by the +s, and puts the text up on the screen; this function was modified to put the text into a variable, which, after dropping the first two rows, I could then write to the screen myself, as and when required.

Another modification was to the function sINPUTFIELDS, which allows data to be entered in fields on the screen. Here some user-defined input types were introduced, to inhibit the input of negative or exponential format numbers where these were inappropriate.

The only non-trivial modification made was to the function that writes data to the screen: the supplied routine (sPUTFIELDS) writes data to fields individually, which was painfully slow in the original applications which had four times as many fields, and was running on a PC with an 8086 chip. It is much faster to read the basic screen image into a variable, insert text as necessary, and re-display – see the listing of qPUTFIELDS in Figure 2. Of course, one could improve the speed even more by working out beforehand the indices of the characters in all the fields in the ravel of the screen image and putting the data directly there, so eliminating the loop.

To get data into and out of the database, there are functions to read, write, and append fixed length records, plus one to read specified columns for all records. These are all straightforward and are shown in Figure 3.

Of the print functions, the one most frequently used is that to print a label, which is called by a number of functions that select the records for which labels are required. As mentioned above, the membership numbers for each label run are stored on a component file for reference. Most of the other print functions are for the benefit of the Treasurer – for the production of invoices or statements, and to process credit card payments. The last of these is a great boon: previously, the Treasurer had to write out credit card slips by hand, which was tedious and error-prone; now, the computer checks the card number (which should satisfy a modulus 10 Luhn formula and generates a print-out that is submitted in place of the more familiar voucher. (The processor makes a surcharge for electronic submissions, else we would avoid the use of paper.)

The modulus 10 Luhn formula that credit card numbers satisfy is the following expression, where DIGITS is a vector of the 13 or 16 digits in the number:

```
0=10|+/Y+9<Y←(⌽DIGITS)×(⍴DIGITS)⍴1 2
```

### Conclusion

Using the functions in the INPUT workspace as supplied, plus functions to access the data, as in Figure 3, a workable database system can easily be created. (A better starting point would be a similar family of functions, written by Adrian Smith, that are available from APL-385 on receipt of a blank disk and return postage.)

With some relatively minor modifications to those functions, one would have a base from which a database application generator could be created.

```
                MEMBER'S DETAILS

ZONE +         IDENTIFIER ++++
SORT FIELD     ++++++++++++++++++++++++++++++

   NAME  +++++++++++++++++++++++++++++++++++++++
   ADDR1 +++++++++++++++++++++++++++++++++++++++
   ADDR2 +++++++++++++++++++++++++++++++++++++++
   ADDR3 +++++++++++++++++++++++++++++++++++++++
   ADDR4 +++++++++++++++++++++++++
POSTCODE ++++++++         COUNTRY +++++++++++++++++++++++++

            yy mm dd                 yy mm dd
DATE JOINED ++/++/++    LAST CONTACT ++/++/++

                                        yy
COPIES +++    MEMBERSHIP CLASS ++   EXP ++   AMOUNT O/S ++++
                                                   (or: Ayy)
FLAGS + + + + + + + + + +
      1 2 3 4 5 6 7 8 9 10
```

Figure 1 - Membership Details

```
    ∇ DATA qPUTFIELDS FIELDS;SCRN;I;J;K;X;W;WIN
[1]    ⍝ Put data into various (one-line) fields on the screen
[2]     WIN← 0 0 25 80
[3]     SCRN←WIN ⎕WGET 1
[4]     X←1↑⍴FIELDS
[5]     I←J←0
[6]  L1:→(X<I←I+1)/L2 ⋄ W←FIELDS[I;]
[7]     SCRN[W[1]+W[3];W[2]+K]←DATA[J+K←⍳W[4]] ⋄ J←J+W[4] ⋄ →L1
[8]  L2:WIN ⎕WPUT SCRN
    ∇
```

Figure 2 - faster screen output

Figure 3 - The APL Code

```
    ∇ Z←DAREAD PARMS

[1]  ⍝ Read a record from direct access native file.
[2]  ⍝ PARMS is: tieno, record length, record number

[3]   Z←⎕NREAD PARMS[1],82,PARMS[2],PARMS[2]×PARMS[3]-1
    ∇
    ∇ Z←DATA DAWRITE PARMS

[1]  ⍝ Write a record to direct access native file.
[2]  ⍝ PARMS is: tieno, record length, record number

[3]   DATA ⎕NREPLACE PARMS[1],PARMS[2]×PARMS[3]-1
[4]   Z←1
    ∇
    ∇ Z←DATA DAAPPEND PARMS

[1]  ⍝ Append data to (direct access) native file.
[2]  ⍝ PARMS is: tieno
[3]  ⍝ Overwrite eof with new record followed by CR, LF, eof.

[4]   (DATA,⎕TCNL,⎕TCLF,⎕AV[27])⎕NREPLACE PARMS,¯1+⎕NSIZE PARMS
[5]   Z←1
[6]
    ∇
    ∇ Z←COLS DAGETCOLS PARMS;BITE;S;X;TT;ROWS

[1]    ⍝ Get a matrix of columns from direct access native file.
[2]    ⍝ PARMS is: tieno, record length
[3]    ⍝ COLS are columns required

[4]     COLS←,COLS
[5]     Z←(0,⍴COLS)⍴' '
[6]     TT←0 ⍝ Bytes read so far
[7]     →(PARMS[2]>S←⎕NSIZE PARMS[1])/0
[8]     S←PARMS[2]×⌊S÷PARMS[2] ⍝ Round down in case of EOF marker
[9]     BITE←PARMS[2]×ROWS←⌊32000÷PARMS[2] ⍝ Chunk size
[10] LP:→(0=BITE←BITE⌊S-TT)/0
[11]    ROWS←BITE÷PARMS[2]
[12]    Z←Z,[1]((ROWS,PARMS[2])⍴⎕NREAD PARMS[1],82,BITE,TT)[;COLS]
[13]    TT←TT+BITE
[14]    →LP
    ∇
```

(webpage generated: 14 October 2007, 17:50)
