---
vid: '10006950'
title: Working with Existing Ascii Files Using J Memory Mapping
authors:
- Donald Pittenger
byline: by Donald B. Pittenger (dbpitt@demlab.com)
volume: '17'
issue: '3'
page: '103'
source: trad/v173/don173.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2001
tags:
- data and files
- implementations
---

*by Donald B. Pittenger ([dbpitt@demlab.com](mailto:dbpitt@demlab.com))*
{ .byline }

This article deals with memory-mapped files using J from the perspective of a pretty average J programmer who is a bit baffled by the terse J documentation and more that a little frustrated by the lack of information about ‘what this memory mapping is really like’. This is my attempt to close the information gap, albeit from the limited perspective of my own attempts to modify an existing system to incorporate memory mapping technology.

Documentation on J memory mapping is mostly in the form of two tutorial labs included with J (the Help index in J4.04c brings up nothing on memory mapping if you search on ‘memory’ or ‘mapping’ or ‘files’). Otherwise, Chris Burke has an introductory article in *Vector* 15.4 that offers useful background information. I urge you to read Burke’s article and to print and read the lab *Mapped Files*, as most of the important information there is not covered here.

The sources just cited mostly deal with files that are created within J; little is said about pre-existing files other than that they too can be mapped. Since I tend to work with files created by spreadsheets or that were packaged on CDs by government agencies such as the U.S. Census Bureau, I was keenly interested in how memory mapping might make life easier for me. I got some tips via the J forum at jsoftware.com, but ultimately had to experiment for myself. This article reports what I have discovered so far.

## J and Files: Problems Encountered

Even though modern APLs are quite capable of dealing with ‘native’ files, J somehow feels more comfortable doing so. For that subjective reason and because scripts can be easily modified using Notepad or a similar simple editor, I’ve followed the common path of writing more and more of my systems in J.

Aside from large-scale data extraction from CDs, I tend to work with data created by spreadsheets and saved as `*.txt` files (tab delimited, with end-of-line characters). Data from such files can be converted to boxed matrices using the J utility `clipunfmt`. When my systems create new data, these are usually text-formatted using the utility `clipfmt` and written to disc as text files. The advantage of text files is that they can include row and column labels (or even more elaborate self-documentation) and therefore are understood by humans. Further, if imported by a spreadsheet program, data are easily modified and the results re-saved as a text file. Also, spreadsheets can be used for data entry and `*.txt` versions can be saved for later use by J. If there are more convenient means of data keeping for small or moderate sized data sets, I don’t know what they are.

However, spreadsheet-based files become impractical once they grow beyond a megabyte or so in size. (I’ve dealt with one such beast of about 20MB, but the resulting text file nearly brought J to its knees, a subject that will be discussed below.) Here is an example of the sort of file I often deal with. The United States contains around 3,150 counties or county-equivalents plus 50 states and the District of Columbia. The sum of those areas, plus the nation itself, comes to about 3,200. I am a demographer and need data for populations by sex and age group. Typically, ages can be in five-year ranges starting with ages 0-4 and continuing 5-9, 10-14, etc. up usually to range 85+. This means three data dimensions—area, sex, and age—that need to be flattened into a two-dimensional table. I happen to work with age as the columns and stack sex into two rows for each area. Adding a column of sex totals plus a few columns needed to contain area names and geographical codes, I wind up with a table of 22 columns and around 6,400 rows. An Excel file with summation coding, to allow data entry validation, runs about 1,500KB and the text file version 655KB.

Let’s say I have a system written in J that fetches and uses the text file just described. The character string is read into J and `clipunfmt` transforms the string into an array of boxed strings. Let’s further say that I want to extract all the counties and place them into a rank-three area-sex-age array before getting on with whatever the system really does with the data. Normally, I’ll have the system strip off the area information columns and remove the duplications (remember, there are two rows per area—one per sex) so that I can set aside a boxed index table for use in extracting particular counties from the data array that is being built. I might also strip off some leading rows containing header information for further use. What’s left is a matrix containing only boxed character-based numbers.

Here trouble begins. The characters in those boxes need to become unboxed numbers. If the matrix was small, you might write `vec=. ;".each mat` to get a numerical vector which then could be shaped as desired. Unfortunately, if the matrix is large, J will churn, your hard drive light will assume a steady green state, and it will take a bit of frantic work to forestall a system lockup. The problem is, boxed arrays seem to chew up RAM. One computer I work with is a 300Mhz Compaq with 64MB RAM running under Windows 98 with a LAN in the background. Converting a boxed array of 60,000 cells will crash J in that environment. That’s maybe 500K of data with more than 100 times that amount of (potential) memory, and it crashes! (One problem with J is that its quirks are not as well documented as APL’s. Perhaps Roger Hui or some other knowledgeable Jster would write something for *Vector* paralleling the APL optimization pieces that have appeared over the years.)

My solution is a utility that converts the matrix into a vector and then executes and un-boxes in 10K or 20K box chunks. There are other ways of skinning this cat, no doubt, but all this is merely an annoyance. The *real problem* is when you have finished the conversion to numerical arrays. It happens that the system I’m discussing reads in data from seven censuses (1930 through 1990). The pre-1970 data are not complete; many counties have empty boxes and the conversion creates zeroes where no data exist. The conversion process (in which matrix chunks are erased the instant they are no longer needed) yields arrays which do not seem to take up more than three or four megabytes of RAM. Yet the result is that the computer paints Windows objects slowly and the hard drive is usually madly paging memory. Once J has been terminated, these symptoms persist, though in weakened form. On the computer I describe, the clock will begin to lose three or four minutes a day. The only cure is a reboot.

The other computer I use has 128MB RAM and runs under Windows NT with no network, and it doesn’t get as woozy as the 64MB machine. Still, no matter how much memory is on-board, a comparatively modest amount of boxing and un-boxing seems likely to create problems. Is there a solution? Why not try memory mapping?

## Playing with Memory Mapping

As was mentioned above, I didn’t find enough documentation to confidently plunge into experimentation: I had to make the plunge with trepidation. Happily, it seems that it is a simple matter and, so far, I’ve broken nothing.

The main thing with pre-existing files (as opposed to the sort of J-created files demonstrated in the labs) is that you apparently have to do the header file work on your own; the lab is sketchy on this point. Basically, you need to know the file structure and then create a descriptor table, program to the structure, or both. I hope to clarify this below.

First, here are some simple experiments you can try which let you see what happens when an existing file is mapped.

Start by fetching the mapped files script:

```
load 'jmf'
```

You also should have created a small Ascii file to use as the experimentation target. It is a good idea to create a table of four or five rows and half a dozen columns in a spreadsheet and save it as a tab-delimited or space-delimited or perhaps comma-delimited file. Say it is called `test.txt` and its path is `c:\directory.`

To map the file, simply type

```
JCHAR map_jmf_ 'dat';'c:\directory\test.txt'
```

where `dat` is a J variable (or ‘noun’ for those of you who insist on using J-speak) that represents the content of the mapped file. `JCHAR` is a byte-count cover supplied by J; others are `JB01`, `JINT`, `JFL` and `JCMPX`. Their meanings should be apparent, but you might find it interesting to replace `JCHAR` with one or another of them when mapping and then inspect the first few items using, say `3{.dat`. `JCHAR` should produce recognizable parts of the table you created; the others should yield various numbers whose form is related to the number of bytes specified by the cover variable used.

To clear the mapping, type

```
unmapall_jmf_ ''
```

and you are ready to map or re-map a file—J will protest if you try to map a currently-mapped file.

Lab *Mapped Files* indicates that you can give the rank and shape by changing the left argument of the mapping function. For instance,

```
(JCHAR;5) map_jmf_ 'dat';'c:\directory\test.txt'
```

will put `dat` into five columns, according to the lab. If you try this with your test data, you will probably find that each column is one character wide. I haven’t experimented with this much, so probably this feature is more useful than it seems.

The important thing is that, so long as you know the underlying structure of the file, you can almost instantaneously access any part of it using a simple command such as

```
a=. ((linecnt*80)+i.80){dat
```

where `linecnt` is the sequence number of a line you want to access, lines happen to be 80 characters each, and you want to assign the characters in that line to variable `a`. Selecting characters within the line will take a little more elaborate offset-specification, of course, but the principle is the same.

You can bog the system down or perhaps even crash it if you try to pass through the entire file at once. For example, I did `+/dat=TAB` on a 600K file, causing all of it to be handled in RAM or virtual-RAM. As a result, interactive work suffered from slow painting of characters to the session screen (remember the joys of Sharp APL/PC on the original 8088 IBM PC?: that’s how slow J looked). Moral: extract fairly small chunks of a file at one time. (‘Fairly small’ can be a hundred kilobytes or more. It depends on how much RAM you have that J can use without paging. Still, smaller is generally better.)

This might look about the same as simply opening a file and reading byte segments. One difference is that `jmf` uses calls to the operating system which is able to do file access in some sort of optimal fashion by bypassing normal data loading operations. But this goes far beyond my competence: read the references mentioned in the lab to understand what’s happening behind the memory mapping curtain. From a program-length standpoint, little is gained because byte segments have to be specified in both instances. (Remember we are dealing with character string files here; mapped files created from J can be integer, floating point, etc., so access can be in terms of, say, the first ten numbers which would be simpler than extracting characters and then converting.)

## How Memory-Mapped Files are Structured

It is interesting to write a small test memory-mapped file in J, following the instructions in the lab, and then examine the file once it has been unmapped. My early experimentation was with APL+Win, which lets me read files and file segments as integer, character, floating point, and so forth. (In J, you have to read the file as character data, then use an appropriate conversion routine for integer, float, or whatever. Score for APL.)

A J memory-mapped file has two parts: a header and the balance of the file. This was mentioned above. What the documentation does not tell you is that the header—presently 284 bytes long—is *integer* data. The balance of the file is whatever type it is. Mixed up in this is whatever junk the file has overwritten; when inspecting either the header area or the balance area you are likely to see scraps of HTML code or other stuff that might confuse you at first. The header tells you how large it is itself, how large the file balance is (this can be greater than the number of bytes populated with data), the number of bytes per item, the total number of items, the rank of the data and the shape of the data. There are a couple of other numbers whose meaning I’m not sure of, but they don’t matter so long as you already know the data type for the file body.

This means it is theoretically possible to (1) add a J-style header to an existing file or (2) construct a J-style memory-mapped file in APL or some other language. If correctly done (and I’ve tried the second option, but using J instead of APL), you can map the constructed file and manipulate it just as if it were a memory-mapped file created using the J tools.

A word of warning: The folks at Jsoftware, Inc. do not guarantee that the present (September, 2000) structure of J memory-mapped files will remain unchanged. This means ‘rolling your own’ file headers or other tricks might not work in the future. Of course, little in computing is really permanent. Not many years ago, the J Windows interface was drastically changed, and most programs dealing with event handlers had to be revised.

## Memory Mapping Existing Data Files

Text files pose a memory-mapping problem because delimited strings are not the same length. That is because the whole point of delimited files is to cut file size by eliminating unneeded blank characters. The only solution is to create some sort of mapping index table that specifies the sequential file position of each end-of-line character and each tab (or comma, for CSV files) within each line. I wrote a program that did this for the census data tables mentioned above, and found that the index was roughly a tenth the size of the target file. This explicit indexing with its requisite custom examination of each file—a task that would have to be repeated each time the target file was altered in any substantive way—persuaded me that text files and memory-mapping go together only if alternatives are even more costly or impractical. But it would be practical if I had to deal with a text file on a CD-ROM, which by definition is unalterable. Then I could pass the file through an indexing program and save the resulting index table with the knowledge that no further upkeep would be needed.

Fixed-field character-based files seem to be a better fit with memory mapping. Each line—or record—is the same length, so all you need to read the desired bytes is an index or data dictionary of the field width and perhaps the column subject plus maybe a rows index. This would be far smaller than the text table index mentioned above. Furthermore, so long as field widths are unchanged, there is no need for re-indexing the file after making changes.

So what I did was write a program converting the text files into fixed-column files. I had one row per area, but strung the sex data into a vector of males by age group followed by females by age group. Each row also started with a short column containing the area’s geographical identification code. The resulting files were around 1040KB, up from about 655KB.

A defect of fixed-format files is that I can’t simply read the data into a spreadsheet for easy modification. I might be able to modify data from a spreadsheet or word-processing program, but I would have to be very careful not to end up with too few or too many characters per line. Operationally, the best strategy is to maintain the text file and rebuild the fixed-column file each time the text file is changed. But this is nearly the same as the re-indexing of text files noted above except that the index file would be tiny and extraction index specification might run a little faster. (Of course you can use memory mapping to change characters in-place. This should work fine in a data processing environment. But small numbers of *ad hoc* changes are probably more suited to the spreadsheet option described above.) The reader should now see that selecting an optimal solution for his needs might require some head scratching or maybe even coin flipping.

## Memory Mapping in Action

Considerations of optimization of disc storage and operational hassle aside, how well does memory-mapping work in practice?

To find out, I rewrote a J script that reads in the census data and calculates estimates of intercensal net migration, displaying age-sex-specific net migration rates on a Windows form, one graph per census interval. The original programs created arrays containing data for all states and counties and caused noticeable slowing of screen painting and other signs of memory paging. There was really no alternative if I used the text files in my database.

The revised system uses fixed-field files, reading in county data for only one state at a time (a convenient way to organize the data in any case). This drastically reduced the size of data arrays while eliminating the need to create boxed tables and then unpack them in the process of creating arrays. The result was a system that did not have an extended ‘hourglass’ period for initial data assembly and there were no memory paging after-effects. There were pauses after a different state was selected, while data were fetched and migration rates calculated, but the wait was only a few seconds on the relatively slow 300MHz machine. I expect this process would be nearly imperceptible on a 1GHz computer, except perhaps when dealing with states with more than 100 counties.

The verdict? In this test case, the result was much better performance paid for by the relatively small cost of creating alternative files 60 percent larger than the original text files.

## Conclusion

Memory mapping existing character-based files takes some support work, but with a little planning it can eliminate some nasty J side effects related to working with large files.

***
