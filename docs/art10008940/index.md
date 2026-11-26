---
vid: '10008940'
title: Formatting Dates in Dyalog APL and APL+Win
authors:
- Adrian Smith
byline: experiments by Ray and Stefano, notes by Adrian Smith
volume: '18'
issue: '2'
page: '97'
source: trad/v182/hack182.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-2001
tags:
- programming techniques
- system interfaces
---

*experiments by Ray and Stefano, notes by Adrian Smith*
{ .byline }

*\[Ed: This paper uses the APL2741 font, which you can [download](resource/apl2741.zip)\]*.

## Background

I began to think about this the first time I saw John Daintree’s presentation on Dyalog.NET. He showed us how much easier it is to get at the Windows API calls, and one of his examples played some games with the date class which .NET provides. As well as letting you add and subtract dates, this has a basic ‘picture formatting’ capability so that you can throw it (more or less) `⎕TS` and get back ‘Sunday, October 21, 2001’ or any other reasonable representation. There it rested, until one of my GraPL users started kicking me for a ‘date,time’ capability on the X-axis. *RainPro* has always had ‘date’ and ‘time’ X-styles and it was easy enough to accept the combination, but I began to think that my habit of simply grabbing the ‘short date’ format from Windows would not be acceptable for much longer.

A quick dig around in Excel confirmed that you could set all sorts of formats with simple picture specifications such as ‘ddd, mmmm dd, yyyy’ and that (very important, this) the date would be formatted in your local language. This smelled like an API call (remember that .NET is mostly just a nice wrapper on the current API), so off to the Windows help file and hit the ‘full text search’ on formatting dates. Bingo … what should we find but **GetDateFormat** (now there is a misleading name for you) which is documented as follows:

```
The GetDateFormat function formats a date as a date string for
a specified locale. The function formats either a specified
date or the local system date.

int GetDateFormat(

    LCID Locale,        // locale for which date is to be formatted
    DWORD dwFlags,      // flags specifying function options
    CONST SYSTEMTIME *lpDate,   // date to be formatted
    LPCTSTR lpFormat,   // date format string
    LPTSTR lpDateStr,   // buffer for storing formatted string
    int cchDate         // size of buffer
   );
```

There is a matching **GetTimeFormat**, which does something very similar to the time part of the date/time vector. Time to ….

## Call in the Professionals

I knew that Ray Cannon had an extensive library of date functions, and guessed that Stef might well have done something similar, so out went a call on the internet, and sure enough, back came the code. Ray used ‘conventional’ Dyalog APL, but Stef had much more fun and wrote a splendid collection of ‘D’ to cover both calls in one go. The rest of this note explains how the calls work, and also has the equivalent implementation in APL+Win.

### Dyalog covers for simple Date and Time formatting (current time)

First the simple case – format the current date with a given picture specification:

```
r←GetDateFormat;get;fmt
⍝ Return the current system date

 fmt←'dddd'','' dd MMMM yyyy'
 'get'⎕NA'I2 kernel32.C32|GetDateFormatA I4 I4 I4 <0T >0T I2'

 r←⊃↑/get 0 0 0 fmt 255 255
```

The function returns the length of the result and the text as a two-element vector, hence the rather interesting reduction, followed by a ‘pick’ (we are in `⎕ml=0` here) to get back a simple string.

The ‘picture’ format is quite complex, and is well explained in the Windows help:

> Use the following elements to construct a format picture string. If you use spaces to separate the elements in the format string, these spaces will appear in the same location in the output string. The letters must be in uppercase or lowercase as shown in the table (for example, "MM" not "mm"). Characters in the format string that are enclosed in single quotation marks will appear in the same location and unchanged in the output string.

| Picture | Meaning |
| --- | --- |
| d | Day of month as digits (no leading zero for single-digit days). |
| dd | Day of month as digits with leading zero for single-digit days. |
| ddd | Day of week as a three-letter abbreviation. The function uses the LOCALE\_SABBREVDAYNAME value associated with the specified locale. |
| dddd | Day of week as its full name. The function uses the LOCALE\_SDAYNAME value associated with the specified locale. |
| M | Month as digits with no leading zero for single-digit months. |
| MM | Month as digits with leading zero for single-digit months. |
| MMM | Month as a three-letter abbreviation. The function uses the LOCALE\_SABBREVMONTHNAME value associated with the specified locale. |
| MMMM | Month as its full name. The function uses the LOCALE\_SMONTHNAME value associated with the specified locale. |
| y | Year as last two digits, but with no leading zero for years less than 10. |
| yy | Year as last two digits, but with leading zero for years less than 10. |
| yyyy | Year represented by full four digits. |
| gg | Period/era string. The function uses the CAL\_SERASTRING value associated with the specified locale. This element is ignored if the date to be formatted does not have an associated era or period string. |

This allows you plenty of scope, but a little surprisingly, formats like 12th October are not supported here. The similar call for the formatted ‘time’ is:

```
r←GetTimeFormat;get;fmt;str
⍝Return the current system time

 fmt←'HH'':''mm'':''ss tt'      ⍝ See Picture meaning

 'get'⎕NA'I kernel32.C32|GetTimeFormatA I4 I4 I4  <0T >0T I'
 r←⊃↑/get 0 0 0 fmt 256 255
```

and the ‘pictures’ are rather easier to remember here:

| Picture | Meaning |
| --- | --- |
| h | Hours with no leading zero for single-digit hours; 12-hour clock |
| hh | Hours with leading zero for single-digit hours; 12-hour clock |
| H | Hours with no leading zero for single-digit hours; 24-hour clock |
| HH | Hours with leading zero for single-digit hours; 24-hour clock |
| m | Minutes with no leading zero for single-digit minutes |
| mm | Minutes with leading zero for single-digit minutes |
| s | Seconds with no leading zero for single-digit seconds |
| ss | Seconds with leading zero for single-digit seconds |
| t | One character time marker string, such as a or p |
| tt | Multicharacter time marker string, such as am or pm |

Just to prove they work ….

```
      GetDateFormat
Sunday, 21 October 2001
      GetTimeFormat
20:49:32 pm
```

That deals with the simple case – now to pass it the time of our choice, which requires a little more research as we need to adapt `⎕ts` timestamps to fit the Windows model. Over to Stef, and a distinct change of coding style!

### Dynamic functions for general-purpose date/time formatting

```
GetDateFormat←{
⍝ ⍵ is a ⎕ts time
⍝ ⍺ is a picture description of the format required
   ⍺←'ddd'','' MMM dd yy'
   l←⊃gdf 0 0(TS2SYSTEMTIME ⍵)⍺ 0 0
   2⊃gdf 0 0(TS2SYSTEMTIME ⍵)⍺ l l
 }
```

```
GetTimeFormat←{
⍝ ⍵ is a ⎕ts time
⍝ ⍺ is a picture description of the format required
   ⍺←'hh'':''mm'':''ss tt'
   l←⊃gtf 0 0(TS2SYSTEMTIME ⍵)⍺ 0 0
   2⊃gtf 0 0(TS2SYSTEMTIME ⍵)⍺ l l
 }
```

```
TS2SYSTEMTIME←{
⍝ ⍵ is ⎕ts format
⍝ returns a Win32 SYSTEMTIME structure
⍝ ⍺ is the day of week: not required here
   ⍺←0
   ⍺{⍵[1 2],⍺,2↓⍵}7↑⍵
 }
```

```
Init
 'gdf'⎕NA'I kernel32_GetDateFormatA I U <{I2[8]} <0T >0T I'
 'gtf'⎕NA'I kernel32_GetTimeFormatA I U <{I2[8]} <0T >0T I'
```

Stef has used the slightly more rigorous approach of calling the routines once, with a zero-length buffer, to get the result size, then allocating a suitable buffer at this length. To show them in action:

```
      TS2SYSTEMTIME ⎕ts
2001 10 0 21 21 21 20 0
      'HH:mm' GetTimeFormat ⎕ts
21:21
      'hh:mm tt' GetTimeFormat ⎕ts
09:21 pm
      '''Today is ''dddd, dd MMMM - yyyy' GetDateFormat ⎕ts
Today is Sunday, 21 October - 2001
```

You can actually be fairly cavalier about missing out the quotes around inclusions – as long as the characters cannot be taken for formatting cues they will just appear verbatim in the result. Putting it all together, we have:

```
 FormatTS←{
⍝ ⍺ is the picture format
⍝ ⍵ a ⎕TS-like time specification
⍝ Assumes the time placeholders come all AFTER or BEFORE the dates
⍝          in the provided picture
   ⎕ML ⎕IO←1

⍝ default ⍺ is not a nice format, just a test
   ⍺←'ddd'','' MMM dd yy hh'':''mm'':''ss tt'
   pic←⍺ ⍝ we'll need this name later

   gdf←{}
   gtf←{} ⍝ trick to localise the following defns
   _←'gdf'⎕NA'I kernel32_GetDateFormatA I U <{I2[8]} <0T >0T I'
   _←'gtf'⎕NA'I kernel32_GetTimeFormatA I U <{I2[8]} <0T >0T I'

   TS2SYSTEMTIME←{
     ⍺←0
     ⍺{⍵[1 2],⍺,2↓⍵}7↑⍵
   }

   aux←{
     w←TS2SYSTEMTIME ⍵
     l←⊃⍺⍺ 0 0 w ⍺ 0 0 ⍝ buffer needed
     2⊃⍺⍺ 0 0 w ⍺ l l  ⍝ apply
   }

   bq←~{⍵∨¯1⌽⍵}≠\⍺=''''
   first←{
     ⊃(∨⌿bq\↑⍺⍷¨⊂bq/⍵)⍳1 ⍝ let's use lexical scoping... woah!
   }
   format←{
     ((⊃⍺)gdf aux ⍵)((2⊃⍺)gtf aux ⍵)
   }

   ix←¯1+TIMEPICTURES DATEPICTURES first¨⊂⍺
   </ix:∊⌽(ix[2]↓pic)(ix[2]↑pic)format ⍵ ⍝ date after time
   ∊(ix[1]↑pic)(ix[1]↓pic)format ⍵       ⍝ time after date
}
```

The variables `TIMEPICTURES` and `DATEPICTURES` simply list the allowed formats (column-1 of the tables a few pages back). Again, to test it:

```
'''It is now ''hh:mm:sstt ''on'' ddd, MMM-dd-yy' FormatTS ⎕ts
It is now 10:33:57pm on Sun, Oct-21-01
```

That just about wraps it up for Dyalog – now to see what APL+Win can do.

## Date and Time formatting in APL+Win

First things first – a quick trawl through aplwadf.ini revealed that neither of the two functions is known to `⎕WCALL`, so a quick email to APL2000 support, and the following was swiftly provided by John Walker:

```
GetDateFormat=I(D Locale, D dwFlags, *SYSTEMTIME lpDate, *C lpFormat, >C
    lpDateStr, I cchDate) ALIAS GetDateFormatA LIB Kernel32

GetTimeFormat=I(D Locale, D dwFlags, *SYSTEMTIME lpTime, *C lpFormat, >C
    lpTimeStr, I cchTime) ALIAS GetTimeFormatA LIB Kernel32
```

Having patched these in (and followed the instructions in the file to recompile it), the functions were plain sailing:

```
    ∇  st←dw TS2SYSTEMTIME ts
[1]   ⍝ <ts> is ⎕ts format
[2]   ⍝ Returns a Win32 SYSTEMTIME structure
[3]   ⍝ <dw> is the day of week: not necessary here
[4]    dw←0 ⋄ ts←7↑ts ⋄ st←ts[1 2],dw,2↓ts
    ∇
```

```
      ⎕VR'GetTimeFormat'
    ∇  fmt←pic GetTimeFormat ts;len
[1]   ⍝ <ts> is a ⎕ts time
[2]   ⍝ <pic> is a picture description of the format required
[3]    :if 0=⎕NC 'pic'
[4]      pic←'h'':''mmtt'   ⍝ 02:34pm is default
[5]    :end
[6]   ⍝ Checks the buffer size, then sends a buffer to hold the answer
[7]    len←1⊃⎕WCALL 'GetTimeFormat' 0 0(TS2SYSTEMTIME ts)pic 0 0
[8]    fmt←2⊃⎕WCALL 'GetTimeFormat' 0 0(TS2SYSTEMTIME ts)pic (len⍴' ') len
    ∇
```

```
      ⎕VR'GetDateFormat'
    ∇  fmd←pic GetDateFormat ts;len
[1]   ⍝ <ts> is a ⎕ts time
[2]   ⍝ <pic> is a picture description of the format required
[3]    :if 0=⎕NC 'pic'
[4]      pic←'ddd'','' MMM dd yy'
[5]    :end
[6]    len←1⊃⎕WCALL 'GetDateFormat' 0 0(TS2SYSTEMTIME ts)pic 0 0
[7]    fmd←2⊃⎕WCALL 'GetDateFormat' 0 0(TS2SYSTEMTIME ts)pic (len⍴' ') len
    ∇
```

A couple more examples, to show these in action:

```
     'MMMM/yy,dddd' GetDateFormat ⎕ts   ⍝ Custom date format
October/01,Sunday
     'HH:mm-ss' GetTimeFormat ⎕ts       ⍝ Custom time
22:46-49
```

As you can see, *Vector* is already exactly a month late, and the evening is moving on. Time to slump in front of the television and watch the football!

## Summary

I think these are a really useful discovery. The great benefit from my perspective is that I can offer my GraPL users ‘TimePicture’ and ‘DatePicture’ properties and any prior knowledge they have (of Excel, for example) can be immediately applied. Probably I should document these in my own help file, but all I really have to do is crib those two tables again, courtesy of Microsoft. Let’s hope .NET fulfils its promise of making many more of these useful utilities easy to find and apply in our own APL and J applications.

The workspaces containing these examples can be [downloaded](resource/hack182.zip) as a 6K zip file from the Resources page.

***
