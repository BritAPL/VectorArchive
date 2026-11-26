---
vid: '10500870'
title: ⎕S and ⎕R
authors:
- Dan Baronet
byline: Dan Baronet (danb@dyalog.com)
volume: '25'
issue: '4'
page: '61'
received: '2012-01-16'
online: '2012-09-20'
editor: John Jacob
description: With version 13 Dyalog introduced two new system operators to deal with regular expressions using the open-source regular-expression search engine PCRE (Perl Compatible Regular Expressions).
abstract: With version 13 Dyalog introduced two new system operators to deal with regular expressions. The implementation uses the open-source regular-expression search engine PCRE (Perl Compatible Regular Expressions), a library, which is built into Dyalog APL. The regular expression syntax which the library supports is not unique to APL nor is it part of the language. Dyalog introduces new features and the way to use it is more like APL. ⎕S and ⎕R are the new system operators that use the PCRE engine to do their job. Like all other functions, ⎕S/R work with Unicode.
source: content/printed/254/sandr.htm
converter: vecrescue 0.1.0
issue_tab: n4-november-2012
tags:
- implementations
- programming techniques
---

Dan Baronet (danb@dyalog.com)
{ .byline }

With version 13 Dyalog introduced two new system operators to deal with regular expressions.  
The implementation uses the open-source regular-expression search engine PCRE (Perl Compatible Regular Expressions), a library, which is built into Dyalog APL. The regular expression syntax which the library supports is not unique to APL nor is it part of the language. Dyalog introduces new features and the way to use it is more like APL.  
`⎕S` and `⎕R` are the new system operators that use the PCRE engine to do their job.  
Like all other functions, `⎕S/R`  work with Unicode.
{ .abstract }

## Who should read this

This article assumes the reader knows a little bit about regular expressions. Although some basic syntax is given, few details will be provided. It will focus on the APL part of it instead and the examples will use simple regular expressions. Some advanced examples are shown towards the end for those interested in that sort of thing.

## Basics

`⎕S` is used to report information on string matches and `⎕R` is used to make replacements in situ. They use regular expressions for that.

A regular expression, often called a **regex**, is a string representing a pattern, a way to match text (e.g. words) in another piece of text. Here are 2 examples.

1\. Find the **offset** of all single characters followed by ‘at’

```
    (‘.at’ ⎕S 0) ‘The cat sat on the mattress’
4 8 17
```

2\. Change to uppercase all occurrences of single letters followed by ‘at’

```
    (‘.at’ ⎕R ‘\u&’) ‘The cat sat on the mattress’
The CAT SAT on the MATtress
```

`⎕S` (and `⎕R`) being a dual operator, it takes 2 operands and returns a function. The left operand is always the pattern(s) and the right operand is the transformation, if any, to apply. It can either be a code (example 1 above: 0 means return the offset of each match), a transformation pattern (example 2: ‘\\u&’[\[1\]](#ref1) means Uppercase the whole match), or a user function discussed below. The result, a function, is applied to an argument which holds text or a number tied to a native text file (including Unicode encoded files).

Regular expressions, or regexes, can specify more complex patterns and an expression to find the length AND offset of *cat* or *lion* would be:

```
    ('cat|lion' ⎕s 1 0) 'the cat sat on the medallion'
3 4   4 24
```

The result is 2 pairs of numbers: ‘a 3 long match at offset 4’ and ‘a 4 long match at offset 24’.

Note that **lion** was found in **medallion**. Regexes allow you to specify whether a letter should be on a word boundary by using **\\b** before and/or after it. For example to look for the offset of exact words **cat** and **lion** (using `⊢` this time to separate the right operand from the argument):

```
    ⍴ '\bcat\b|\blion\b' ⎕S 0 ⊢ 'the cats sat on the medallion'
0
```

Nothing found. And this is right, the words cat and lion nowhere appear by themselves.

There are many rules which are out of the scope of this article. They can be found either on the Net or in Dyalog’s manuals; too many to list them all here. It suffices to say that you can look for patterns in a very flexible and concise manner. Here is a short list of pattern characters and what they mean:

<table>
        <tr><td>.	</td><td>stands for any char but line delimiter </td></tr>
        <tr><td>{n}	</td><td>stands for N times</td></tr>
        <tr><td>?	</td><td>stands for 0 or 1 time</td></tr>
        <tr><td>+ 	</td><td>stands for 1 or more times</td></tr>
        <tr><td>* 	</td><td>stands for 0 or more times</td></tr>
        <tr><td>|	</td><td>means OR</td></tr>
        <tr><td>()	</td><td>group elements or alternatives</td></tr>
        <tr><td>][	</td><td>group possible characters</td></tr>
        <tr><td>\b	</td><td>means “at a word Boundary”</td></tr>
        <tr><td>^	</td><td>means the beginning of a line</td></tr>
        <tr><td>$	</td><td>means the end of a line</td></tr>
    </table>

For example to look for a string representing a number smaller or equal to 2012 (remember we are dealing with characters here) the pattern <strong>“\\b ( 1\\d\{3\} | 200\\d | 201\[012\] ) \\b”</strong>[\[2\]](#ref2) would do it. It means: look for either

a **‘1’** followed by 3 digits (**\\d** is any numeric digit – 0 to 9, **\{3\}** means repeat 3 times)  
OR (the vertical bar <strong>|</strong> )  
**‘200’**  followed by a digit <strong>(\\d)</strong>  
OR (|)  
**‘201’** followed by either **‘0’**, **‘1’** or **‘2’** (square brackets regroup the set of characters that the next character should be in)

The parentheses regroup the 3 alternatives and the **\\b** before and after ensures the string found is not part of a bigger number (like 1<strong>2000</strong>9)

You can look for patterns that repeat, recurse, and do all kinds of things.

But like anything else you need to spend a bit of time to do more fancy things.

Again, if you want to delve into this, type ‘regular expressions’ in your favourite search engine and let yourself loose. If you want APL examples see “Tools, Part 4: Regular Expressions” in Vector 21:2, p.126.

We will now look into what Dyalog did with this.

## Transformations

There are 3 types of results we may want to get from `⎕S:`

1. Frequent values like offset, length, line number for which codes (small ints) can be used
2. Transformation (text) pattern like “uppercase the hit”
3. More complex expression for which you must provide a function.

We have seen examples of 1 and 2 above. If you cannot do what you want with those you’ll need to write a monadic function which will accept a namespace as argument. This namespace will contain a series of variables related to the current match:

<table>
        <tr><td>Block		</td><td>the text being searched, in line mode<a href="#ref3">[3]</a>  this is the line we’re looking at</td></tr>
        <tr><td>BlockNum	</td><td>the line number (<code>⎕IO 0</code>), in line mode again</td></tr>
        <tr><td>Pattern		</td><td>the pattern used</td></tr>
        <tr><td>PatternNum	</td><td>the pattern number (<code>⎕IO 0</code>)</td></tr>
        <tr><td>Match		</td><td>the string that matches the pattern</td></tr>
        <tr><td>Offsets		</td><td>the offset of each sub pattern </td></tr>
        <tr><td>Lengths		</td><td>the length of each sub pattern</td></tr>
        <tr><td>Names		</td><td>the names of each sub pattern</td></tr>
        <tr><td>ReplaceMode	</td><td>whether we are using <code>⎕R</code> (1) or <code>⎕S</code> (0)</td></tr>
    </table>

## Examples

1\. Find all but the 1st digit of all unsigned integers found in a text

```
    '\d(\d+)' ⎕S '\1' ⊢ 'dsa  1233  30  3 xyz'
 233  0
```

**Explanation:** the pattern \\d(\\d+) means “find a digit (\\d) followed by “more-than-1 digits” (\\d+). The parentheses capture that sub pattern and since it is the 1st (and only) sub pattern it is #1 (#0 is given to the whole match). So ‘1233’ is the 1st match found (and sub pattern 0), in it is the sub pattern ‘233’ which is returned because this is what the right operand (\\1) says to do. The same thing happens with the 2nd match: 30. So `⎕S` returns 2 results: ‘233’ and ‘0’. Note that ‘3’, the 3rd integer in the string does NOT match the pattern as there is no “more-than-1 digit” (\\d+) after the 1st one.

2\. Reverse and return all the words in a sentence  
This one is impossible to do with a transformation string so we use a function:

```
'\w+' ⎕S {⌽⍵.Match} 'The cat sat on the mat'
 ehT  tac  tas  no  eht  tam
```

**Explanation:** each time the `⎕S` derived function finds a match, e.g. ‘The’, it gives the right operand function a namespace as argument which contains the variables mentioned above. In it is found ‘Match’ which can be referred to and, in this case, flipped and returned. This is done each time a match is found (6 times). So here `⎕S` returns a Vector of 6 Text Vectors (a VTV).

## `⎕R`

So far we’ve only seen searches. `⎕R` is the other new system operator introduced in V13.

It is very much like `⎕S`. except that it performs replacement in the argument string. If we were to run the last 2 examples with `⎕R` instead we would get this:

1\. Remove the 1st digit of all integers >9 :

```
 '\d(\d+)' ⎕R'\1' ⊢ 'dsa  1233  30  3 xyz'
dsa  233  0  3 xyz
```

**Explanation:** the pattern \\d(\\d+) means “find a digit (\\d) followed by “more-than-1 digits” (\\d+) just like before. The parentheses capture that sub pattern. So ‘1233’ is the 1st match found, and it is replaced by the sub pattern ‘233’ because this is what the right operand (\\1) says to do. The same thing happens with the 2nd match: 30. So `⎕R` returns the argument minus the 1st digit of each match.

2\. Reverse all the words in a sentence

```
    '\w+' ⎕R {⌽⍵.Match} 'The cat sat on the mat'
ehT tac tas no eht tam
```

**Explanation:** each time the `⎕R` derived function finds a match, e.g. ‘The’, it gives the right operand function a namespace as argument which contains, among other things, ‘Match’, which can be referred to and, in this case, flipped and used to replace the original match. This is done each time a match is found (6 times) resulting in a modified argument. Note that there is no need to separate the right operand function from the argument since it binds with `⎕R` first.

## Details

Normally a regex engine like PCRE works on a string in two possible modes: as a whole, called singleline, or as a series of lines delimited by a line delimiter (like NL), called multiline. This affects the way some searches are performed: in singleline mode the ^ and $ only match at the beginning and end of the searched string. In multiline mode they match at the beginning and end of every line in it. For example, an expression like **^a.\*b$** would find all lines starting with **a** and ending with **b**. The <strong>^</strong> means “the beginning of the line”, the dot (.) means “any character but NL”, the star (<strong>\*</strong>) means “as many times as possible” and the <strong>$</strong> means “the end of the line”.

`⎕S/R` have the same 2 modes called Document mode (=PCRE *singleline* mode) and Mixed mode (=PCRE multiline mode) plus another mode called Line mode. Line mode splits the searched text into logical lines so searches cannot span across them. It effectively treats every line as a different text to be searched. This is something not available with PCRE. Line mode is less prone to WSFULLs. **It is the default mode.**

APL being array oriented it also works on VTVs (Vectors of Text Vectors). With VTVs each vector represents a line in the text separated (by default) by CRLF and all results are the same in every mode, whether you use a VTV or `(⍴,LineDel)↓⊃,/LineDel∘,¨VTV`.

## The Variant operator `⍠`

A regex program like PCRE is a complex program that assumes some settings to be present. Just like dyadic iota uses implicit arguments like `⎕IO` and `⎕CT`, a regex engine uses implicit arguments that can be modified.

This new primitive operator is used to modify the behaviour of a function[\[4\]](#ref4). Here it is used to modify the way the resulting function from `⎕S` will be used. Note that the `⍠` character is not in `⎕AV`[\[5\]](#ref5).

Variant takes a function to the left and an array to the right. The array specifies the options that modify the function, resulting in a new function returned by Variant. For example, in

```
        F2← F1 ⍠ array
```

F2 is now a modified version of F1.

There are many parameters that can be set via `⍠` for `⎕S` and `⎕R`. Here are the main ones (the 1st value is the default):

<table>
        <tr>
			<td>IC	    </td>
			<td>Ignore Case. Possible values: 0 or 1 </td>
		</tr>
       <tr>
			<td>Mode	</td>
			<td>Operating mode. Possible values: L=line mode, D=document mode, M=mixed mode</td>
		</tr>
       <tr>
			<td>DotAll	</td>
			<td>makes the dot match line delimiters too. Possible values: 0 or 1</td>
		</tr>
       <tr>
			<td>EOL	    </td>
			<td>what the line delimiter is. Possible values: CR, LF, CRLF, VT, NEL, FF, LS, PS</td>
		</tr>
       <tr>
			<td>ML	    </td>
			<td>Match Limit. Possible values: an integer. 0=no limit, &lt;0=only match |ML</td>
		</tr>
       <tr>
			<td>Greedy	</td>
			<td>whether matches are the longest or shortest. Possible values: 1 or 0</td>
		</tr>
    </table>

## Examples

1\. Change each vowel into XX

```
    ('[AEIOU]' ⎕R 'XX' ⍠ 'IC' 1) 'ABCDE abcde'
XXBCDXX XXbcdXX
```

**Explanation:** the pattern specifies a set of letters to match, 5 vowels, each to be replaced by ‘XX’. Normally only the ‘A’ and the ‘E’ would be replaced but because the Variant operator has modified the `⎕R` resulting function to be Case Insensitive (with `⍠'IC' 1`) then ‘a’ and ‘e’ are also modified.

2\. Change ‘C’ followed by any character then ‘D’ by dash (-) in a multi line text

```
    ('C.D' ⎕R '-' ⍠ ('Mode' 'D')('DotAll' 1)) 'ABC',CR,'DEF',CR,'CAD'
AB-EF
-
```

**Explanation:** the dot in the pattern usually means ‘any character but EOL’ but because Variant specifies that the **dot** matches **all** characters and because it also specifies ‘Document mode’ (Mixed mode would do too) then the sequence ‘C’,CR,’D’ matches and it is replaced by ‘-‘. The sequence ‘CAD’ also matches and is replaced accordingly. Note how multiple options are written. Because Variant modifies an existing function it can be applied many times. The following is all equivalent:

```
  ('C.D' ⎕R '-' ⍠ ('Mode' 'M')('DotAll' 1))…
  ('C.D' ⎕R '-' ⍠ 'Mode' 'M' ⍠ 'DotAll' 1)…
  CXD←'C.D' ⎕R '-' ⋄ (CXD ⍠ ('Mode' 'M')('DotAll' 1))…
  (CXD ⍠ 'DotAll' 1 ⍠ 'Mode' 'M')…
  CXM← CXD ⍠ 'Mode' 'M' ⋄ (CXM ⍠ 'DotAll' 1)…
```

3\. Change the first 2 letters of each line by ‘x’

```
    ('[A-Z]' ⎕R 'x' ⍠ 'ML' 2) 'ABC' 'DEF'
xxC xxF
    ('[A-Z]' ⎕R 'x' ⍠ 'ML' ¯2) 'ABC' 'DEF'
AxC DxF
    ('[A-Z]' ⎕R 'x' ⍠ 'ML' ¯4 ⍠ 'Mode' 'D') 'ABC' 'DEF'
ABC xEF
```

**Explanation :** in the first case we ask to change any character in the set A-Z by x. Because we are working in line mode, each vector is processed independently and the first 2 letters of each line is modified. In the second case we ask that only the **second** (`¯2`) match be acted upon, again in each line. In the last case we ask that only the 4th match be processed. Because we now treat the argument as a whole document (`'Mode''D'`), only the 4th letter, ‘D’ is modified.

## Using files as input source

It is possible to use a native file as source instead of a array (character vector or VTV).

To do so you supply the tie number of the file to process. If the file is read from the start, and there is a valid Byte Order Mark (BOM) at the start of it, the data encoding is determined by this BOM.

The input document may be processed in any mode. In document mode and mixed mode, the entire input document, line ending characters included, is passed to the search engine; in line mode the document is split on line endings and passed to the search engine in sections without the line ending characters. The choice of mode affects both memory usage and behaviour.

## Solved patterns

Find a number from 0 to 255: \\b(25\[0-5\]|2\[0-4\]\\d|\[01\]?\\d\\d?)\\b

Find an IP address:

\\b((25\[0-5\]|2\[0-4\]\\d|\[01\]?\\d\\d?)\\.)\{3\}(25\[0-5\]|2\[0-4\]\\d|\[01\]?\\d\\d?)\\b

Finding a date:

\\b(19|20)\\d\\d\[- /.\](0\[1-9\]|1\[012\])\[- /.\](0\[1-9\]|\[12\]\[0-9\]|3\[01\])\\b

## Finding APL names or numbers

It may be interesting to know where some names are in some code.

Finding a name with a regex in a conventional language is fairly easy: <strong>\[A-Za-z\]\\w\*</strong> will usually do it: that’s 1 letter followed by 0 or more word characters (the \* means 0 or more times). Not in APL. APL names in Dyalog can have all sorts of characters like `∆` and `⍙`. A regular expression to find such a name would look like

```
(?x) # ignore spaces and comments
# define what the Leading character of a name and what an APL ID is
(?(DEFINE)(?<L1>[_A-Z∆a-
z⍙ÁÂÃÇÈÊËÌÍÎÏÐÒÓÔÕÙÚÛÝþãìðòõÀÄÅÆÉÑÖØÜßàáâäåæçèéêëíîïñóôöøùúûü]) )
(?:            # do not capture
(?<!\s:|::)    # not preceded by space: or ::
(?<!(?&L1))    # not preceded by any of the characters above
(?&L1)         # must start with one of them
(?:(?&L1)|\d)* # followed by 1 of them or a digit 0 or more times
|⍺⍺|⍺|⍵⍵|⍵)    # or any of those
```

which is a bit hard to remember and type – and to explain in details here, even with the comments.

Assuming we put the above pattern in variable ‘patid’ we can find APL names in code like this:

```
    patid ⎕S {⍵.Offsets,⍵.Lengths} '⎕←dsÀÄa←_-.=0 u∆io8 321'
2 5  8 1  14 5
```

Numbers are similar. A number in Dyalog would require a long expression to be used.

There is a project at Dyalog to provide a shortcut for such patterns. For now if you wish to find general APL names or numbers in your code you have to resort to user command `]WSLOCATE`. Type `]?WSLOCATE` for details.

## Technicalities

The engine needs to prepare the search before using it.

This involves parsing the pattern and compiling a structure subsequently used in the process. If a search will be performed repeatedly it would be inefficient to recompile it each time. The interpreter tries to account for this by keeping the last few structures around for a while in case they are to be reused. The interpreter keeps a cache of compiled patterns and flushes the one least recently used when the cache is full and a new pattern is created. This cache is saved in the workspace; and is never garbage collected.

## Real life example

This example is for those with a good understanding of regular expression.

In 2011 I needed to look into a log file and extract some data in a log file tied to **tieno**. I was looking for the patterns

P1: `'(type=\d+, shape=(?:(?:\d+[ ,])+))'` (here I wanted the whole thing)

Followed by anything and then by

P2: `'≡⍴2 0⍕B *\d+ (\d+)'`  (here I only wanted the number in the parentheses)

This is basically the pattern P1.\*?P2.

By doing `'P1.*?P2.' ⎕S info tieno'` I should have been able to extract all the data with &lt;info> a function catenating P1 with P2 results for each match.

Unfortunately the file sometimes contained P1…P1…P2 which means that the 1st P1 (and not the 2nd) was matched with P2. So I had to use a different pattern.

I used `p1((?!p1|p2).)*p2`

This is “look for p1, advance while you don’t have either p1 or p2, then look for p2”.

### Explanation:

Let’s use the following text as an example:

aap1bbp1ccp2dd

The 'p1' at the start of the pattern will cause the text pointer to skip over the "aa" characters and match the first "p1" characters.

The next part in parentheses breaks down to a negative lookahead (the ?! part) and the '.'. The negative lookahead will check the next character(s) (here "bb") and, since there is no match either with p1 or p2 (the 2 alternatives), will succeed and so the '.' will match the first "b". This is repeated with the "bp" characters.

As the next characters are "p1" the lookahead will fail (because of its negative aspect). Because the quantifier that controls this part of the pattern is '\*' (0 or more matches) the fact that the lookahead failed is not a problem, it’s only the parentheses group that stops matching. The next thing in the pattern is p2 which is compared with the "p1" in the text - which will fail. This global failure causes the regex engine to start backtracking. In this case, there are no alternative ways that they can match so the whole pattern match is rejected. ‘p1bbp1’ failed to match.

When this occurs, the regex engine will step forward 1 character over the 1st "p" and start the match at the "1bb" character sequence. In this case the regex engine will again start skipping forward until it gets to the 2nd "p1". The process described above will be repeated except that the lookahead will stop the '.' matching when it gets to the "p2" characters. Now, the last part of the pattern (p2) does match which lets the regex engine declare that a complete match has been found.

The sequence ‘p1ccp2’ matches, &lt;info> grabs it, extract the sections I want, merges and returns them. I’m one happy camper.

This last example is a real life example which would have been more difficult to program in APL (or any other language for that matter). The ability to use regexes combined with APL allowed me to perform the task in a fraction of the time it would have taken me normally.

## Conclusion

Regular expressions are extremely useful when dealing with complicated patterns. Without them you must spend time writing code, sometimes a lot of, to parse and extract, time that could be better spent, well, any other way. Up until now there was no choice, but now there is.

You have to be careful and not over do it. Using code like `‘x’ ⎕R ‘X’ +text` is very inefficient compared to `((text=’x’)/text) ← ‘X’`

Caveat emptor.

Books : Regular Expressions Cookbook (O’Reilly 2009)

### References

1. <a id="ref1"></a> Another one is \\ln to Lowercase match of sub pattern ‘n’
2. <a id="ref2"></a> Spaces are important in regular expressions but ignore them here
3. <a id="ref3"></a> Modes are explained below
4. <a id="ref4"></a> For example used with iota (`⍳`) it could generate a function to generate series from either 0 or 1. By using `⍳⍠ 0` we could create a function that generates numbers starting at 0 regardless of `⎕IO`.
5. <a id="ref5"></a> Astute observers will wonder what Classic users do. For them there is an equivalent `⎕OPT` system operator.
