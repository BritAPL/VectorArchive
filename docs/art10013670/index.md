---
vid: '10013670'
title: Zark Newsletter Extracts
authors:
- Gary Bergquist
byline: edited by Jonathan Barman
volume: '21'
issue: '3'
page: '18'
source: trad/v213/zark213.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- programming techniques
---

edited by Jonathan Barman
{ .byline }

## Utility Corner:

(The purpose of this column is to make you more productive by introducing you to utility functions. Think of utility functions as APL functions that have names instead of symbols. By expanding your function vocabulary, you’ll be able to write APL code that’s more concise, more efficient, and more readable.)

In last issue’s *Limbering Up* column, you were asked to define a dyadic utility function `DAYSPLUS` that allows you to do “date addition” with a “Market Day” calendar.

When working with investment data, a special calendar must be used – the calendar that has one day per “Market Day,” i.e. per day that the securities market is open for trading. The market is not open weekends nor on certain holidays. For certain analyses, it is necessary to move forward or backward a certain number of market days. How is this done? Suppose the “Market Day,” calendar is expressed in two arrays, `ciy` and `cid`:

```
      ciy
1994 1995

      ⍴cid
2 12 31

      cid
WWDDDDDWWDDDDDWWDDDDDWWDDDDDWWD
DDDDWWDDDDDWWDDDDDWWHDDDDWWDXXX
DDDDWWDDDDDWWDDDDDWWDDDDDWWDDDD
HWWDDDDDWWDDDDDWWDDDDDWWDDHDDWX
WDDDDDWWDDDDDWWDDDDDWWDDDDDWWHD
DDDWWDDDDDWWDDDDDWWDDDDDWWDDDDX
DWWHDDDDWWDDDDDWWDDDDDWWDDDDDWW
DDDDDWWDDDDDWWDDDDDWWDDDDDWWDDD
DDWWHDDDDWWDDDDDWWDDDDDWWDDDDDX
WWDDDDDWWDDDDDWWDDDDDWWDDDDDWWD
DDDDWWDDDDDWWDDDDDWWDDDHDWWDDDX
DDWWDDDDDWWDDDDDWWDDDDDWWHDDDDW
WHDDDDWWDDDDDWWDDDDDWWDDDDDWWDD
DDDWWDDDDDWWDDDDDWWHDDDDWWDDXXX
DDDWWDDDDDWWDDDDDWWDDDDDWWDDDDD
WWDDDDDWWDDDDHWWDDDDDWWDDDDDWWX
DDDDDWWDDDDDWWDDDDDWWDDDDDWWHDD
DDWWDDDDDWWDDDDDWWDDDDDWWDDDDDX
WWDHDDDWWDDDDDWWDDDDDWWDDDDDWWD
DDDDWWDDDDDWWDDDDDWWDDDDDWWDDDD
DWWHDDDDWWDDDDDWWDDDDDWWDDDDDWX
WDDDDDWWDDDDDWWDDDDDWWDDDDDWWDD
DDDWWDDDDDWWDDDDDWWDDDHDWWDDDDX
DWWDDDDDWWDDDDDWWDDDDDWWHDDDDWW
```

Here, `ciy` is a vector of the years in the calendar. `cid` is a 3-dimensional character array with one plane per year in `ciy`, 12 rows (months), and 31 columns (days). Thus, the ninth row of the second plane corresponds to September 1995:

```
DWWHDDDDWWDDDDDWWDDDDDWWDDDDDWX
```

The `D`s correspond to market days, the `W`s to weekend days, the `H`s to holidays, and the `X`s to nonexistent days. For example, September 4 is a holiday (Labor Day); September 2 and 3 are weekend days; and September 31 does not exist.

The left argument of the proposed utility function `DAYSPLUS` is an array of market day dates. The right argument is a corresponding array of numbers of days to add. The result is an array of the market day dates that come from the date “addition”. For example,

```
      19950906 DAYSPLUS 2 0 ¯1 ¯3
19950908 19950906 19950905 19950831
```

The arguments should be scalar conformable, you know, vector and vector returns vector, matrix and scalar returns matrix, and so on. Assume globals `ciy` and `cid`.

The solution from Rex Swain is direct and efficient:

```
     ∇ R←YMD DAYSPLUS N;C;Y;⎕IO
[1]   ⍝ Returns date that is N market days
[2]   ⍝ after date YMD (yyyymmdd).  Logic
[3]   ⍝ assumes YMD is a market day.
[4]   ⍝ Arguments are scalar conformable.
[5]   ⍝ Globals:
[6]   ⍝   ciy - vec of years
[7]   ⍝   cid - 3D char array of years by
[8]   ⍝         12 months by 31 days
[9]   ⍝    (D=mkt day; W=weekend;
[10]  ⍝     H=holiday; X=non-day)
[11]  ⍝
[12]   ⎕IO←0 ⍝ Origin 0 is simpler
[13]   Y←ciy[0]-1 ⍝ Base year
[14]   R←YMD-10000×Y ⍝ YYYYMMDD → IMMDD
[15]   R←0 100 100⊤R ⍝ IMMDD → I MM DD
[16]   R←0 12 31⊥R-1 ⍝ I MM DD → cid inds
[17]   C←+\,'D'=cid ⍝ Cumulative mkt days
[18]   R←C⍳C[R]+N ⍝ Perform "addition"
[19]   R←1+0 12 31⊤R ⍝ cid inds → I MM DD
[20]   R←0 100 100⊥R ⍝ I MM DD → IMMDD
[21]   R←R+10000×Y ⍝ IMMDD → YYYYMMDD
     ∇
```

Let’s walk through the logic in this function with an example:

```
       19950906 DAYSPLUS 2 0 ¯1 ¯3
[13]   Y←ciy[0]-1 ⍝ Base year
       Y : 1993
[14]   R←YMD-10000×Y ⍝ YYYYMMDD → IMMDD
     YMD : 19950906
       R : 20906
[15]   R←0 100 100⊤R ⍝ IMMDD → I MM DD
       R : 2 9 6
[16]   R←0 12 31⊥R-1 ⍝ I MM DD → cid inds
       R : 625
[17]   C←+\,'D'=cid ⍝ Cumulative mkt days
       C : 0 0 1 2 3 4 5 5 5 6 7 ...
[18]   R←C⍳C[R]+N ⍝ Perform "addition"
                 C[R] : 424
               C[R]+N : 426 424 423 421
       R :   C⍳C[R]+N : 627 625 624 619
[19]   R←1+0 12 31⊤R ⍝ cid inds → I MM DD
       R : 2 2 2  2
           9 9 9  8
           8 6 5 31
[20]   R←0 100 100⊥R ⍝ I MM DD → IMMDD
       R : 20908 20906 20905 20831
[21]   R←R+10000×Y ⍝ IMMDD → YYYYMMDD
       R : 19950908 19950906 19950905 19950831
```

The APL “magic“ that allows this function to work without looping takes place on lines \[17\] and \[18\]. The vector C designates, for every day in the calendar, the number of market days that have elapsed since the beginning of time (or, rather, since the beginning of the calendar). The expression `C⍳C[R]+N` does the “addition” by extracting the number of cumulative days for each given date, adding N days, and searching for the market date that corresponds to that new number of cumulative days.

The beauty of the expression `C⍳C[R]+N` is that it works for any shapes of `R` and `N` (as long as they are scalar conformable). For example, `R` can be a matrix and `N` a scalar, or `R` a scalar and `N` a matrix, or they can both be matrices of the same shape.

The submission from Jim Weigang used the same algorithm as above but refined the technique a bit by converting the year, month, and day values to origin 0 indices all at once. In the function above the adjustments occur on lines \[13\], \[14\], \[16\], \[19\], and \[21\]. In the function below, they occur only on lines \[15\], \[17\], and \[23\].

```
          ∇ R←YMD DAYSPLUS N;A;B;C;Y;⎕IO
[1]   ⍝ Returns date that is N market days
[2]   ⍝ after date YMD (yyyymmdd).  Logic
[3]   ⍝ assumes YMD is a market day.
[4]   ⍝ Arguments are scalar conformable.
[5]   ⍝ Globals:
[6]   ⍝   ciy - vec of years
[7]   ⍝   cid - 3D char array of years by
[8]   ⍝         12 months by 31 days
[9]   ⍝    (D=mkt day; W=weekend;
[10]  ⍝     H=holiday; X=non-day)
[11]  ⍝
[12]   ⎕IO←0 ⍝ Origin 0 is simpler
[13]   A←0 100 100 ⍝ Date-packing radix
[14]   B←0 12 31 ⍝ cid-indexing radix
[15]   Y←A⊥ciy[0],1 1 ⍝ Origin adjuster
[16]   C←+\,'D'=cid ⍝ Cumulative mkt days
[17]   R←YMD-Y ⍝ YYYYMMDD → IMMDD (⎕IO=0)
[18]   R←A⊤R ⍝ IMMDD → I MM DD
[19]   R←B⊥R ⍝ I MM DD → cid inds
[20]   R←C⍳C[R]+N ⍝ Perform "addition"
[21]   R←B⊤R ⍝ cid inds → I MM DD
[22]   R←A⊥R ⍝ I MM DD → IMMDD
[23]   R←Y+R ⍝ IMMDD → YYYYMMDD
     ∇
```

While there is a certain aesthetic appeal to this refinement, the efficiency of the function improved by only a fraction of a percent.

A side note: Many APL beginners are lured by the appeal of writing code that is compact as well as powerful. On encountering a function like the one above, they are tempted to condense the function by combining lines \[17\] to \[23\] like so:

```
R←Y+A⊥B⊤C⍳C[B⊥A⊤YMD-Y]+N
```

They may even include a comment for this line like, “Do the necessary conversion.” Resist this temptation! The joyful feeling that comes from squeezing one more rabbit in the hat will be more than offset by the feeling of despair that comes from looking at this line of code in four months and having no idea what it does.

Happily, the function does not run faster by jamming all the code onto one line, so don’t offer efficiency as an excuse. And the concept of saving storage by omitting comments is analogous to swallowing your food whole to conserve your teeth.

Enough preaching.

Another submission, from Bob Santos, takes the same approach as the above functions, but does the “addition” a bit differently.

```
     ∇ R←YMD DAYSPLUS N;A;B;I;Y;⎕IO
[1]   ⍝ Returns date that is N market days
[2]   ⍝ after date YMD (yyyymmdd).  Logic
[3]   ⍝ assumes YMD is a market day.
[4]   ⍝ Arguments are scalar conformable.
[5]   ⍝ Globals:
[6]   ⍝   ciy - vec of years
[7]   ⍝   cid - 3D char array of years by
[8]   ⍝         12 months by 31 days
[9]   ⍝    (D=mkt day; W=weekend;
[10]  ⍝     H=holiday; X=non-day)
[11]  ⍝
[12]   ⎕IO←0 ⍝ Origin 0 is simpler
[13]   A←0 100 100 ⍝ Date-packing radix
[14]   B←0 12 31 ⍝ cid-indexing radix
[15]   Y←A⊥ciy[0],1 1 ⍝ Origin adjuster
[16]   I←,'D'=cid ⍝ Cumulative mkt days
[17]   I←I/⍳⍴I ⍝ Bits to indices
[18]   R←YMD-Y ⍝ YYYYMMDD → IMMDD (⎕IO=0)
[19]   R←A⊤R ⍝ IMMDD → I MM DD
[20]   R←B⊥R ⍝ I MM DD → cid inds
[21]   R←I[N+I⍳R] ⍝ Perform "addition"
[22]   R←B⊤R ⍝ cid inds → I MM DD
[23]   R←A⊥R ⍝ I MM DD → IMMDD
[24]   R←Y+R ⍝ IMMDD → YYYYMMDD
     ∇
```

Compare the “addition” methods

Cumulative market days –

```
     C←+\,'D'=cid
     R←C⍳C[R]+N
```

Index of market days –

```
     I←,'D'=cid
     I←I/⍳⍴I
     R←I[N+I⍳R]
```

Both methods do `,'D'=cid`. The former uses `+\` on the resulting bit vector. The latter uses `/⍳⍴` on it. The former method uses `[]` and then `⍳`; the latter uses `⍳` and then `[]`. The two methods are similar enough that the relative efficiency will depend on the implementation of APL you use.

When timing these algorithms, it becomes apparent that much of the time consumed by these functions is spent on the `+\` or `/⍳⍴` operations. Anything that can be done to reduce this time will improve the overall efficiency of the functions. For example, in APL\*PLUS II/386, the `/⍳⍴` line of the latter function can be replaced by

```
[17]   I←WHERE I ⍝ Bits to indices
```

where the `WHERE` function is a “compiled” function from the APL\*PLUS workspace `ASMFNS`. The resulting `DAYSPLUS` function takes about 60% as long to run as the function without `WHERE`.

An even faster solution is to precompute the `C` or `I` variable once, and pass it as a global variable to the function. Here’s how this approach looks (for the latter function):

```
     ∇ R←YMD DAYSPLUS N;A;B;Y;⎕IO
[1]   ⍝ Returns date that is N market days
[2]   ⍝ after date YMD (yyyymmdd).  Logic
[3]   ⍝ assumes YMD is a market day.
[4]   ⍝ Arguments are scalar conformable.
[5]   ⍝ Globals:
[6]   ⍝   ciy - vec of years
[7]   ⍝   cid - 3D char array of years by
[8]   ⍝         12 months by 31 days
[9]   ⍝    (D=mkt day; W=weekend;
[10]  ⍝     H=holiday; X=non-day)
[11]  ⍝   cii - I/I⍴I←,'D'=cid (⎕IO=0)
[12]   ⎕IO←0 ⍝ Origin 0 is simpler
[13]   A←0 100 100 ⍝ Date-packing radix
[14]   B←0 12 31 ⍝ cid-indexing radix
[15]   Y←A⊥ciy[0],1 1 ⍝ Origin adjuster
[16]  ⍝ cii←,'D'=cid ⍝ Cumulative mkt days
[17]  ⍝ cii←cii/⍳⍴cii ⍝ Bits to indices
[18]   R←YMD-Y ⍝ YYYYMMDD → IMMDD (⎕IO=0)
[19]   R←A⊤R ⍝ IMMDD → I MM DD
[20]   R←B⊥R ⍝ I MM DD → cid inds
[21]   R←cii[N+cii⍳R] ⍝ Perform "addition"
[22]   R←B⊤R ⍝ cid inds → I MM DD
[23]   R←A⊥R ⍝ I MM DD → IMMDD
[24]   R←Y+R ⍝ IMMDD → YYYYMMDD
     ∇
```

This function runs faster than any other function we could devise. It runs in about 75% the time of the original (Swain) function with its `C` variable precomputed. The reason for this difference is that the vector you get from the `/⍳⍴` is about 67% the length of the vector you get from `+\` (because weekends, holidays, and invalid days are eliminated).

As a mental exercise, how would you modify the above function to develop another dyadic function name `DAYSDIFF` whose arguments are both arrays of dates, and whose result is an array of the number of market days between those dates? Which method (`+\` or `/⍳⍴`) would be faster? How about a monadic function named `DAYSVALID` whose argument is an array of dates, and whose result is a Boolean array flagging the dates that are valid market days? Which method would be faster?

## Limbering Up: Market Days

(The purpose of this column is to work some flab off you APL midsection. Like muscles, your APL skills can atrophy if not exercised with adequate frequency and variety. This column presents a task for you to perform. Set aside a few minutes from your busy schedule and work the task. Mail in your solution and stay tuned for the results.)

One of the features of APL is that you can use exponential, or scientific, notation to express numbers. For example, you can type 1E6 instead of 1000000. This convenience is, however, a two-way street. The computer also has the right to speak to you in exponential notation. For example, you can try the following in immediate execution mode:

```
      .00000123
0.00000123
      .000000123
1.23E¯7
      1230000000
1230000000
      12300000000
1.23E10
```

It appears the computer, too, grows weary of typing too many zeros. From a fairness point of view, we are comforted. However, at times you will find exponential notation unacceptable. For example, suppose you develop an application that prompts the user for a “Daily Declared Rate” for a Mutual Fund:

```
Daily Declared Rate: .000000125
```

Your program digests the number and files it. Later that day, the user wants to review the rate for subsequent modification. Here’s what he sees:

```
Daily Declared Rate: 1.25E¯7
```

Is this the number he entered or not? The answer is not immediately obvious. What do you, as the programmer, do?

One simple approach is to tell the computer to stop using exponential notation:

```
      .000000125
1.25E¯7
      ⎕EN
1
      ⎕EN←0
      .000000125
.000000125
      ⍕.000000125
.000000125
```

Don’t you wish? Unfortunately, there is no such `⎕EN` system variable. That’s where you come in. Your task is to write a monadic function `ENOFF` (Exponential Notation OFF) that behaves exactly like monadic `⍕` except it never returns its formatted numbers in exponential notation.

Send your solution to:  
Vector Production  
Brook House  
Gilling East  
York YO62 4JJ  
UK  
email: [apl385@compuserve.com](mailto:apl385@compuserve.com)

The notable functions and their authors’ names will be printed in the next issue of *Vector*.

Good luck and happy limbering.
