---
vid: '10501210'
title: Dyalog’s parser - a new parser in town
authors:
- Dan Baronet
byline: Dan Baronet (danb@dyalog.com)
volume: '26'
issue: '1'
page: '79'
received: '2013-09-27'
online: '2013-10-01'
description: Dyalog’s parser - a new parser in town
abstract: In the following text I use terms specific to our trade. You won’t find them in the dictionary but I assume the reader is familiar with words such as ‘monad’, ‘global’ (as a noun) and ‘default’ (verb). I also use quotes and angle brackets to help determine the type of the object I am referring to. ‘Quotes’ denote a variable or workspace and <angle brackets> refer to a function/operator or file. Often, the context is sufficient to remove ambiguities. Emphasized words have a special meaning. Definitions are especially marked up, too.
source: content/printed/261/baronet/baronet.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- programming techniques
---

Dan Baronet (danb@dyalog.com)
{ .byline }

In the following text I use terms specific to our trade. You won’t find them in the dictionary but I assume the reader is familiar with words such as ‘monad’, ‘global’ (as a noun) and ‘default’ (verb). I also use quotes and angle brackets to help determine the type of the object I am referring to. ‘Quotes’ denote a variable or workspace and &lt;angle brackets> refer to a function/operator or file. Often, the context is sufficient to remove ambiguities. *Emphasized* words have a special meaning. *Definitions* are especially marked up, too.
{ .abstract }

## Introduction

A line (string) parser is a handy tool to carry around.

Such a tool should be able to accept a string as argument (its input) and be able to attribute meaning to its constituent parts by following a number of simple rules.

For example, in C, a function’s list of arguments is given by “a left parenthesis, 0 or more non-blank strings separated by commas and a right parenthesis”. The statement `ABC(2+3,x,y/z);` is a perfectly valid C statement, calling function `ABC` with 3 arguments.

Another example, in DOS (or Windows’ command mode), a command (a keyword) takes 0 or more words as arguments, followed by 0 or more switches, a word being a series of non-blank characters and a switch being a special character (here /) followed by a letter, possibly followed by a colon (:) and a string.

The TREE command in DOS, for example, accepts 0 or 1 argument, possibly followed by the switches /F and/or /A.

Unix is similar, using dash (-) instead of / as switch delimiter.

There are many times when a similar situation arises and we must supply a known number of arguments and switches to a program.

Closer to home, there are cases where such a parser could come in handy. Imagine a program, `REPORT`, which accepts `DATA` as right argument and some Options as left argument. The options could be e.g. "Title", “page width”, “use page numbers”. The program’s header and first line would typically look like this:

```
        ∇ Options REPORT Data;Title;PW;P0;...
[1]     (Title PW P0)←Options,(⍴,Options)↓''  70  0
[2]     ...
```

The onus is on the user to remember the order of `Options` but with only 3 options it isn’t so bad. Imagine now that there are a large number of options. It might be simpler to specify something like:

```
    'Sales Report +usepagenos +pagewidth=80 +margin=10 10' REPORT data
```

The syntax is cleaner and the user doesn’t have to remember all the options and their order. If a change occurs and new options are added they can be inserted easily. The problem of course is to make sense of that left argument.

This kind of problem arose in the 90s when STSC introduced user commands in the APL/PC product.

User commands are commands that the developer writes in APL and are called by the system using a right bracket (\]) syntax, similar to the )SYSTEM commands. A programmer writes command `XYZ` which is called in the session by writing `]XYZ`. If the command takes arguments and/or switches they are added after the command name. The programmer is responsible to parse the line and figure out the meaning. Simple commands with few arguments and switches take only a few lines to parse but many acceptable switches become quickly overwhelming and the lack of standard makes it confusing for the user. And the code to parse the line quickly shadows the important code.

When user commands came out with APL/PC there was no parser and I wrote one for them and used it for many years. I even wrote an article in <cite>Vector</cite>[\[1\]](#ref1) about it. The problem was that there were no enclosed arrays at the time and the parser had to do all kinds of tricks to do its job like setting globals, using delimited strings and so on.

Today with arrays of enclosures and, in Dyalog APL’s case, namespaces, it is easier to pack all the information into a tight object.

That’s the purpose of this text. I will use user commands as example. This is in fact the parser used at Dyalog for their user command processor.

## A few definitions

### Generalities

A *sentence* is made of characters and divided into 0 or more *sections*.

Sections are separated from each other by one or more of a special character, the *separator*.

Each section contains a single *field* and a *value*. A value is 1 or a string.

A field has an *ID*. Some fields may have a *name* in which case their ID is their name. An unnamed field’s ID is a unique number in the range 1 to N where N is the number of allowed unnamed fields.

Named fields are always optional. Unnamed fields can be compulsory. In a sentence, sections may contain named fields which may be repeated.

A named field is introduced with a special symbol followed by the name. If a section with a named field in it will have a value it will appear after the ‘=’ sign after the name.

Example: here is a sentence with named and unnamed *sections*, the comma is the *section* separator:

```
    una,unb,,$city=mtl,,,$cnt=can,$nice
```

This *sentence* has 5 *sections*, 2 with unnamed *fields* and 3 with named *fields*. The *ID* of each one is `1`, `2`, `city`, `cnt` and `nice`. All *sections* but `nice` have a *value* specified. The first two *sections* have unnamed *fields* with *values* `una` and `unb`. The last three *sections* have named *fields* and the last one has no *value* specified, its *value* is 1.

### Implementation

A parser should be able to determine if a *sentence* follows specific rules.

The class `Parser` produces a parser capable of recognizing if a sentence follows specific rules. Those rules are supplied at instantiation time.

The rules specify

- the number of unnamed sections.
- how many are compulsory.
- the list of named sections.
- whether they accept a value.

Example: the expression

```
    CP←⎕NEW   Parser   ('$city=  $cnt=  $nice'      'nargs=2')
```

produces a parser (CP) only capable of determining if a sentence, like the one above, follows the rules by applying its `Parse` method to a sentence, e.g.:

```
    Data←CP.Parse  ‘una  unb  $city=mtl $cnt=can $nice‘
```

If the sentence does **not** follow the rules the parser will signal an error. For example if a non-existent named field is specified or if a named field accepting a value is not given one (or vice versa) then it will signal an appropriate error. The validation is very strict.

If the *sentence* is valid, `Data` will be a regular namespace containing all the possible *sections* with their name and value. If a *section* is absent its value will be 0. If it is present without a value (e.g. `$nice`) it will be 1 (not ‘1’). If a section is repeated only the last value is retained. To see all of them you can do `Data.SwD`. In the example above you would get

```
 city  mtl
 cnt   can
 nice    1
 _1    una
 _2    unb
```

You can access the value of a *section* directly, e.g.

```
    Data.city
mtl
```

The unnamed *sections* are given the *ID* `_1` and `_2`. This way you can access their value directly:

```
    Data._1
una
```

The parser can be applied again to another *sentence*:

```
    Datb←CP.Parse    'I  love  $city=Paris  '
    Datb.city
Paris
    Datb.SwD
 city  Paris
 cnt       0
 nice      0
 _1        I
 _2     love
```

Since `cnt` was not specified in the *sentence* its value is 0. Same for `nice`.

### Unnamed sections

Optional unnamed sections can be done using `'S'` with the number of arguments. `'S'` stands for ‘Short’ to allow a shorter number of arguments. This makes them all optional as 0 is an acceptable number of arguments too. If `CP` above is defined as (note the `'S'` after the 2)

```
    CP←⎕NEW   Parser   ('$city=   $cnt=  $nice'      'nargs=2S')
```

Then

```
    Datc←CP.Parse    ‘great    $nice   $cnt=Canada  ‘
```

would produce (note the 2nd argument is 0 because it is not in the *sentence*)

```
    Datc.SwD
 city      0
 cnt  Canada
 nice      1
 _1    great
 _2        0
```

### Sections with spaces in them

If a *section* contains a *section delimiter* (a space here) in it there must be a way to tell the parser. The preferred way is to use yet another character to escape the space or to surround the section with a pair of enclosing special characters. An obvious character to use in this case is “. For example:

```
        Datd←CP.Parse    '  ”Добрый    день”      $cnt=Russia  '
```

produces

```
    Datd.SwD
 city           0
 cnt    Russia
 nice           0
 _1    Добрый    день
 _2             0
```

`Parse` will accept both ‘ and “ as string delimiter as long as they are paired properly, i.e. `'a … z'` and `"a … z"` are ok but `‘a … z"` is not.

If a quote is part of the string the other quote can be used or you can double the quote inside the quotes string, e.g. `"I'm OK"` or `'I''m OK'`.

Quotes should also be used if the text includes a character used to introduce a named field (e.g. `$` above). Example: `'amount is $20'`.

## The Dyalog parser

The Dyalog parser is located in `⎕SE`.

In this parser the space is used as section delimiter. It cannot be changed.

### Terms

Because of the context in which the parser is used an unnamed field is called an *argument* and a named field is called a *switch* or *modifier*.

In theory the *arguments* could appear anywhere in the sentence but Dyalog’s parser does not allow it; all *arguments* must appear at the beginning of the sentence. This means that since only sections containing *modifiers* can appear at the end there is no need to quote the values if they contain spaces, i.e. in

```
        Datf←CP.Parse    'huge     $cnt=US of A '
```

`US` of `A` does not need to be quoted to include the spaces. Note that the trailing spaces are ignored.

On the other hand, since arguments have an ID you can specify them elsewhere in the sentence by simply using their ID followed by = and the value. The example above then becomes

```
        Datf←CP.Parse    '   $cnt=US of A   $_1=huge  '
```

### Features

This parser has many features.

No need to specify the number of arguments if it is possibly unlimited. The class’ argument is then a single string:

```
    CP←⎕NEW   ⎕SE.Parser   ‘$city=   $cnt=  $nice’
```

If you do not specify the number of arguments no `_n` variable will be stored in the resulting namespace. However, the list of arguments is **always** stored in variable *Arguments*. Example:

```
    Datf←CP.Parse 'there are 7 arguments here, no modifier' ⍝no nargs=string
    Datf.SwD
city   0
cnt    0
dsa    0
        ]disp Datf.Arguments
┌→────┬───┬─┬─────────┬─────┬──┬────────┐
│there│are│7│arguments│here,│no│modifier│
└────→┴──→┴→┴────────→┴────→┴─→┴───────→┘
```

The character introducing the names must be specified.

It is the 1st char in the list (here #) and separates the names:

```
    CP←⎕NEW   ⎕SE.Parser   ‘#city=   #cnt=  #nice’
```

#### Minimum character needed to specify names

There is no need to enter the entire name, only the minimum suffices:

```
    CP←⎕NEW   ⎕SE.Parser   '+color = +country='
    Datf←CP.Parse    '   +col=blue   +cou=UK '
```

Here `+col` is sufficient to determine that it is color. Same with `+cou` for country.

If only `+c` or `+co` is used the parser won’t be able to tell which one is meant and an error will be signalled.

On the other hand you may want to force the entry of a name to a minimum. You use parentheses for that:

```
    CP←⎕NEW   ⎕SE.Parser   '-color=  -country(ofresidence)='
```

Here `-color` can be abbreviated to `-col` but `-countryofresidence` can only be entered with a minimum of `-country`. This is useful when forcing the user to enter the whole name because of a security problem, e.g.

```
    CP←⎕NEW   ⎕SE.Parser   '/file=  /delete()'
```

Here `/file` can be entered as a single `/f` but we don’t want the user to enter `/d` alone by mistake and a full `/delete` is required.

Only the ‘(‘ is important and the last ‘)’ is ignored but it is tolerated.

#### Case insensitive

Normally modifiers’ names are used “as is” but you may want to enter them in lower or uppercase. If you do

```
        CP←⎕NEW ⎕SE.Parser ('$City= $Cnt= $nice'  'nargs=2   upper')
        Datg←CP.Parse    'I  love  $cITy=Paris '
        Datg.SwD
CITY  Paris
CNT       0
NICE      0
_1        I
_2     love

    Datg.CITY
Paris
```

all names are uppercased. There is no way to get them in lowercase form.

#### Minimum-maximum number of arguments

It is possible to add an `'S'` to the number of arguments to specify that they are all optional, i.e. that 0 to n can be entered (here 5):

```
    CP←⎕NEW   ⎕SE.Parser   ('/file=  /delete'      'nargs=5S')
```

It is also possible to use n1-n2 to specify a minimum (here 2 to 5):

```
    CP←⎕NEW   ⎕SE.Parser   ('/file=  /delete'      'nargs=2-5')
```

If the number of arguments is not from 2 to 5 the parser will issue an error, either ‘too few arguments’ or ‘too many arguments’. Using `'S'` is the same as 0-n.

It is possible to merge extra arguments together.

For example if the last section contains spaces it must be used like this:

```
    CP←⎕NEW ⎕SE.Parser (''  'nargs=3')  ⍝ note no modifiers accepted
    Dath←CP.Parse    '   Joe Blough   "42  Penny Lane  E." '
```

If there is nothing following the 3rd section we can tell the parser that it is “Long” and quotes are not needed (but still accepted). Note the L after the 3:

```
    CP←⎕NEW  ⎕SE.Parser (''  'nargs=3L')
    Dath←CP.Parse ' Joe Blough 42 Penny Lane E. ' ⍝ no quotes needed at the end
    ]disp       Dath.SwD        ⍝ note the spaces are preserved
┌→─┬──────────────────┐
↓_1│Joe               │
├─→┼─────────────────→┤
│_2│Blough            │
├─→┼─────────────────→┤
│_3│42  Penny Lane  E.│
└─→┴─────────────────→┘
```

This feature is useful when expecting a single long argument:

```
    Log←⎕NEW   ⎕SE.Parser   ('-file='      'nargs=1L')0
    Dath←Log.Parse    '   Joe Blough   42 Penny Lane E.  –file=\tmp\log.txt'
    ]disp       Dath.SwD
┌→───┬─────────────────────────────┐
↓file│\tmp\log.txt                 │
├───→┼────────────────────────────→┤
│_1  │Joe Blough   42 Penny Lane E.│
└───→┴────────────────────────────→┘
```

The number of arguments can be both, "Long" and "Short". There is no restriction in that respect. The rules may specify less than, say, 3 (Short), but merge any argument above 3 with the 3rd one (Long). This would be specified as

```
    CP1←⎕NEW   ⎕SE.Parser   (‘’      ‘nargs=3SL’)
```

#### There is no limit on the number of arguments

As noted before it is possible to specify that there is no limit on the number of arguments simply by not specifying the `'nargs='` field in the 2nd string (or eliding the 2nd string completely).

```
    CP2←⎕NEW   ⎕SE.Parser   '/file=/del'
```

It is also possible to enter `'nargs=99999'` to signify ‘a large number of arguments’.

The difference is in the resulting namespace which will only contain the `_1`, `_2`, … variables if `nargs=n` has been specified.

Although there is no limit, in order to limit the number of variables defined in the resulting namespace (like `Dath`, above), the number of variables produced is limited to 15, i.e. `_1`, `_2`, …, `_15` will be there but `_16` and up won’t be. The list of all arguments is always kept in `Arguments` inside the namespace so they are always available. For example:

```
    CP←⎕NEW   ⎕SE.Parser   '+s1'         ⍝ no nargs=
    Dati←CP.Parse 'Joe Blo  42 Penny Lane E. tel 0 44 12345 890, and more '
    ⍴Dati.Arguments
16
    ]disp Dati.Arguments
┌→──┬───┬──┬─────┬────┬──┬───┬─┬──┬─────┬───┬───┬────┐
│Joe│Blo│42│Penny│Lane│E.│tel│0│44│12345│890│and│more│
└──→┴──→┴─→┴────→┴───→┴─→┴──→┴→┴─→┴────→┴──→┴──→┴───→┘
    ]disp Dati.SwD
┌→─┬─┐
↓s1│0│
└─→┴─┘
    CP←⎕NEW   ⎕SE.Parser   ('+s1'  ' nargs=999S')
    Dati←CP.Parse 'Joe Blo  42 Penny Lane E. tel 0 44 12345 890, and more  +s'
    ]disp Dati.Arguments
┌→──┬───┬──┬─────┬────┬──┬───┬─┬──┬─────┬───┬───┬────┐
│Joe│Blo│42│Penny│Lane│E.│tel│0│44│12345│890│and│more│
└──→┴──→┴─→┴────→┴───→┴─→┴──→┴→┴─→┴────→┴──→┴──→┴───→┘
    ]disp Dati.SwD
┌→──┬─────┐
↓s1 │1    │
├──→┼~────┤
│_1 │Joe  │
├──→┼────→┤
 …
├──→┼────→┤
│_15│is   │
└──→┴────→┘
```

#### Ambivalent modifiers

Sometimes modifiers accept a value, sometimes they don’t. If their nature is ambivalent you can specify it at parser creation time, using square brackets around `=` to mean “maybe”, like this:

```
    CP←⎕NEW   ⎕SE.Parser   '+s1[=]'
```

Here, `s1` is a modifier that may be specified with or without a value:

```
    Datj←CP.Parse '+s'
    Datj.SwD                    ⍝ s1 is on the line without a value
s1  1
    Datj←CP.Parse '+s=abc'
    Datj.SwD
s1  abc
```

### Validation

#### List member

The parser is able to perform minimalistic validation on the values entered with modifiers. For example, if modifier `s1` above accepts any of the values in `'ab' 'cde' 'fgjk'` then we can create a parser to validate it like this:

```
    CP←⎕NEW   ⎕SE.Parser   '+s1=ab  cde  fgjk'
```

and using it is as before:

```
    Datj←CP.Parse '+s=ab'
    Datj.SwD
s1   ab
```

except that if we enter a value not in the list we get:

```
    Datj←CP.Parse '+s=abc'
invalid value for switch <s1> (must be ONE of "ab  cde  fgjk")
    Datj←CP.Parse'+s=abc'
    ∧
```

#### Set member

The values can also be checked against a list of characters and ensure they all belong to the list. We use `∊` instead of `=` for this. For example, if modifier vowel below accepts any character in the set `'aeiou'` then we can create a parser to validate it like this:

```
    CP←⎕NEW   ⎕SE.Parser   '+vowel ∊aeiou'
```

And using it is as before:

```
    Datk←CP.Parse '+v=aooaee’
    Datk.SwD
 vowel   aooaee
```

except that if we enter a character not in the list we get:

```
    Datk←CP.Parse '+s=aey'
invalid value for switch <s1> (must be ALL in "aeiou")
    Datk←CP.Parse'+s=aey'
     ∧
```

#### Default values

By default all fields have the value 0 to mean “not specified on the line”. When a modifier (or even an argument) is not specified we may wish to give it a value by default. For example, you may wish to use the value `'abc'` for modifier `s1` if it not on the line. In APL the code to do this would look like

```
    :if 0≡v←Datj.s1   ⋄   v←'abc'   ⋄   :endif
```

There are 2 ways to get a default value with the parser. The first one involves telling the parser at creation time:

```
    CP←⎕NEW   ⎕SE.Parser   ‘+city:London’
    Datl←CP.Parse '+c=Kbh'
    Datl.city
Kbh
        Datl←CP.Parse 'blah’    ⍝ no +city specified
Datl.city
London
```

The second method involves using a function (called `Switch`) in the resulting namespace.

That function takes the name of a modifier and returns its value when called monadically.

When called dyadically it returns its left argument if the modifier’s value is 0 (e.g. not in the statement).

```
    CP←⎕NEW   ⎕SE.Parser   '+city='
    Datl←CP.Parse '+c=Toronto'
    Datl.Switch ‘city’          ⍝ city has the value "Toronto" as specified
Toronto
    ‘NY’  Datl.Switch  ‘city’   ⍝ city was specified, it is returned
Toronto
    Datl←CP.Parse  'blah'       ⍝ no +city specified
    Datl.Switch 'city'                  ⍝ no city means 0
0
    ‘NY’ Datl.Switch 'city'     ⍝ no city can mean NY when not specified
NY
```

`Switch` has the advantage over the :default syntax in that it can turn strings representing numbers into numbers.

```
    CP←⎕NEW   ⎕SE.Parser   '+age:18'
    Datl←CP.Parse        '+a=70'
    70=⎕←Datl.age       ⍝ ‘age’ is '70'
70
0  0
    Datl←CP.Parse 'blah'        ⍝ no +age specified, its value is '18'
    18=⎕←Datl.age                   ⍝ this is a string, not a number
18
0  0
```

The parser cannot tell whether ‘18’ is meant to be a string or a number. `Switch`, on the other hand, is smart about it:

```
    CP←⎕NEW   ⎕SE.Parser   '+age=' ⍝ we don’t specify a default value here
    Datl←CP.Parse '+a=70'
    70=⎕←Datl.Switch 'age'      ⍝ this is character
70
0  0
    70=⎕←18  Datl.Switch  'age'         ⍝  this is numeric, thanks to Switch
70
1
    Datl←CP.Parse  'blah'   ⍝ no +age specified
18=⎕←18  Datl.Switch  'age'     ⍝  this is numeric, thanks to Switch
18
1
```

Note that the result is a numeric vector, not a scalar.

If you try to turn a non-numeric modifier into a number `Switch` will also complain:

```
    Datl←CP.Parse '+a=seventy'
    666  Datl.Switch 'age'
value must be numeric for age
    666  Datl.Switch 'age'
    ∧
```

### Other features

There are a few more features left:

#### Prefixing names

Modifier names cannot start with a number but if you use a prefix for them it can be made to work:

```
    CP←⎕NEW   ⎕SE.Parser  ( '+007[=]')
switches must be valid identifiers
    CP←⎕NEW ⎕SE.Parser('+007[=]')
    ∧
    CP←⎕NEW ⎕SE.Parser  ('+007[=]'    'prefix=∆')
    Datm←CP.Parse  'whatever   +007=JB'
    Datm.SwD
007  JB
    Datm.⎕nl-2
Arguments  SwD  ∆007
    Datm.∆007
JB
    Datm.Switch '007'
JB
```

#### Not requiring space before modifiers

Since names start with a special character there is no real need to force a space to delimit them. An example is DOS commands which may be abutted as in DIR /T/A; here /A follows /T without any space in between.

If this can be allowed it can be specified as in

```
    CP←⎕NEW   ⎕SE.Parser  ('/sw1  /sw2'   'allownospace')
```

#### Changing the error number when things go wrong

When the parser refuses to accept a set of rules it signals an error in the 700-710 range. If this can interfere with the calling program it can be changed using `error=` to specify the lower range value:

```
    CP←⎕NEW   ⎕SE.Parser   ('/sw1'   'error=800')
```

#### Propagating the modifiers

Sometimes it is necessary to pass the modifiers received to another program which uses similar modifiers.

For example, in SALT, the program `Snap` uses many modifiers, some of which are passed along to the program `Save`. Both use some same modifiers. The modifier `–noprompt` is one of them. When `Snap` calls `Save` it has to pass along that modifier in the command string. Assuming all the modifiers and arguments are in namespace `A`, one thing it could do is

```
        Save  cmdstring, A.nopromt / ' –noprompt'
```

Because there are many modifiers to pass along this statement would be in fact much more complicated, especially when modifiers have values.

The arguments namespace contains a function, `Propagate`, which will generate a string defining the switches as they were submitted.

For example, if `–noprompt` was specified on the `Snap` command line, doing `A.Propagate 'noprompt'` would return `'-noprompt'`. If `–noprompt` was not specified then it would return `''`. If a modifier to be propagated has a value the function will reproduce it verbatim, e.g. if `–nop –file=\ab\c` is used then doing `A.Propagate 'noprompt file'` would return `'-noprompt -file=\a\b\c'`.

Example: going back to the REPORT example we can see that writing:

```
        ∇ Options   REPORT   Data;all;Parse;...
[1]     Parse←{ (⎕new ⎕se.Parser  ⍺).Parse  ⍵}
[2]     all← ‘+margin= +usepagenos +pagewidth=’   Parse   Options
[3]     :if  all.usepagenos    ...
```

is easier to read and modify. We can now call this program like this:

```
'Sales Report  +usepagenos  +pagewidth=80  +margin=10  10'  REPORT  data
```

Another example, coding the DIR command in DOS (we use a prefix because of /4):

```
    pDIR←⎕new  ⎕se.Parser  ('/a=/b/c/d/n/o=/p/q/r/s/t=/w/x/4' 'allow prefix=S')
```

## Epilogue

This tool is a bit elaborate but covers many aspects of line parsing. Many years of programming convinced me of its usefulness. I have programmed variants of this code in several languages but none as advanced as in Dyalog APL. If you write your own user commands this will prove to be very helpful.

If your version of Dyalog APL does not have all these features try to use the user command `]uupdate` to update your version of SALT and User Commands. This should work with all versions of Dyalog APL starting at V13.1.

## References

1. <a id="ref1"></a> See "Tools, Part 1. Basics" in *Vector* 19.4 (April 2003) for details
