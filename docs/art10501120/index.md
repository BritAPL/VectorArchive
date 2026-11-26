---
vid: '10501120'
title: APLUnit - An APL unit test library
authors:
- Gianfranco Alongi
byline: Gianfranco Alongi (gianfranco.alongi@gmail.com)
volume: '26'
issue: '1'
page: '67'
received: '2013-09-12'
online: '2013-09-23'
description: Alongi describes APLUnit, a tool for test driven development in APL
source: content/printed/261/aplunit/aplunit.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- development practice
---

Gianfranco Alongi (gianfranco.alongi@gmail.com)
{ .byline }

## What is test first programming?

Test first programming, or TDD (Test Driven Development)[\[1\]](#ref1) as it is also called, is a popular approach for development where you design your system in an incremental fashion by writing a test, and then adding the code for this test to pass.

One way to look at it, is that we figure out how we would like our code to work, set up a test so that we know when we are done, and then making it pass our test, thus making our code work as we intended it to.

Yet another way to look at it: we are constantly capturing our assumptions about our code, and documenting it with a language that allows the documentation to check it’s own correctness.

And finally, I like to think of it as rock climbing. I start out at the base of the mountain, with a pretty good idea of where I want to go, and which path I’ll take. So, I set out to reach the first point I want (I write my first test), and I go there (I make the test pass). I now have a nail in the rock and my safety line through the loop. I set out to the second goal, (I write my second test) and I make it there (I make it pass). Just like in rock climbing, it’s dangerous to make it too far between the safe points.

The idea is that you should spend at most about 15 minutes per step. If the test you wrote was too hard then remove it; this would be the equivalent of admitting that maybe you should take another path up to the next stop. Go back, ponder your strategy and set out again (remove the test, write another one - a smaller step - and make it pass).

The way to do TDD is to always write a test first and then write code to make that test pass. As long as that test fails; you have a problem to think about. It follows the principle of separation of concerns; only concern yourself with one problem at a time. Once your test passes, you know it works, and you can prove it! You run the test. You run the test often to feel secure and comfortable; and sometimes just because you like the ego-boost of seeing all the tests pass.

But only write the minimal amount of code needed to make the test pass!

## A testing library for APL

As I like the incremental development model which is a result of TDD. I always approach languages with TDD. To do this, I keep things simple and look for libraries with a minimal footprint, rich feature set and helpful error reports that facilitate the error localization. Unfortunately I could not find such a library for Dyalog APL, so, I set out and rolled my own TDD library.

This became what is now *APLUnit* a Test framework with a small syntactical footprint and rich feature set. This has evolved through technical, borderline theological and philosophical discussions with Morten Kromberg (CTO @ Dyalog), who represented the APL community while I worked on the initial development and style of the library.

### Installation

APLUnit is written as a Dyalog SALT script - installation is done by downloading the UT.dyalog file onto your system and loading it into the environment using

```
    ⎕SE.SALT.Load './UT.dyalog -target=#'
```

### A Test and it’s expectation

A test is any traditional (tradfn) or direct (dfn) function which ends with ‘\_TEST’ as part of the name. All tests shall set the expectation of the test into the UT namespace variable `expect` (match the following) or `nexpect` (do not match), as in the trivial examples below, written as a tradfn and a dfn.

```
    ∇ Z ← user_defined_function_TEST
        #.UT.expect ← 1 2 3
        Z ← ⍳ 3
    ∇

    dfn_TEST ← {
        #.UT.nexpect ← 1 2 3
        1 + ⍳3
    }
```

### Running tests

Once loaded - the #.UT.run command is available and capable of running any test which is defined. You may run a single test by naming it directly

```
 #.UT.run 'user_defined_function_TEST'
```

You can run an array of tests

```
 tests ← 'user_defined_function_TEST' 'dfn_TEST'
 #.UT.run tests
```

You can run all tests in a particular SALT file (dyalog script)

```
    #.UT.run './test/known_bugs.dyalog'
```

Finally you can also run all tests from test files in a directory. Test files are SALT dyalog scripts with a name ending in ‘\_tests.dyalog’- this enforces a standardized format making it easy to locate test files.

```
    #.UT.run './bugs_reported_by_customers/'
```

### Result reporting

If the expected value matches the produced result of the test the test is said to have passed, and APLUnit prints this for you on the screen:

```
    #.UT.run '#.bowling_tests.parse_strike_frame_TEST'
    Passed 0 m 0 s 0 ms
```

If the test fails, (`expect` and actual result does not match, or `nexpect` matches the output of the test), a graphical display is printed to help you understand the cause of the failure.

```
    #.UT.run '#.bowling_tests.parse_strike_frame_TEST'
    FAILED: #.bowling_tests.parse_strike_frame_TEST
    Expected
    ┌→────────────┐
    │    ┌→─────┐ │
    │ 10 │strike│ │
    │    └──────┘ │
    └∊────────────┘
    Got

    10
```

If you ran a collection of tests through the array, file or directory mechanic, you get a test report nicely written on the screen at the end of the test, and the same Pass/FAILED output for every test executed.

```
    #.UT.run  './test/'
    Passed 0 m 0 s 0 ms
    Passed 0 m 0 s 0 ms
    .....
    .....
    Passed 0 m 0 s 0 ms
    Passed 0 m 0 s 0 ms
    Passed 0 m 0 s 0 ms
    -----------------------------------------
    ./test/fixed_bugs_tests.dyalog tests
        ⍋  Passed:  141
        ⍟ Crashed:  0
        ⍒  Failed:  0
        ○ Runtime:  0 m 0 s 10 ms
```

#### Crashes

When APLUnit runs a test, the default behaviour is to trap all errors and continue execution until reporting is done. If you wish to change this behaviour - you can control the trapping by setting the (s)top (a)t (c)rash configuration variable in the UT namespace to either 1

```
    #.UT.sac ← 1   ⍝ stop at crash
```

or

```
    #.UT.sac ← 0   ⍝ trap and run on
```

0 is the default value. This is to ensure that bulk-running of several tests allows you to get a full report without manual intervention.

If you do set `#.UT.sac←1` you must remember to `→⎕LC` once you fix your test.

### Example

For this example I demonstrate how to use the library on a dynamic toy problem with a made up customer.

#### Setup

First I need to load the test framework into the APL environment.

```
    ⎕←⎕SE.SALT.Load 'UT.dyalog'
    #.UT
```

Now I need two SALT files (censor.dyalog and censor\_tests.dyalog) with empty namespaces

```
    #.censor
```

which will contain the functionality, and

```
    #.censor_tests
```

which contains the tests for my implementation.

#### Coding

The customer EvilCorp wants the system to censor all words which has a high enough match with blacklisted words, nasty words, such as ‘pony’, ‘flower’and ‘sun’.

Thus, we create an initial test for one of them - the ‘pony’ word. In order to achieve this I add the first test to the `#.censor_tests` namespace:

```
    :NameSpace censor_tests

    censor_pony_TEST←{
        #.UT.expect← 'little xxxx friends'
        'pony' #.censor.run 'little pony friends'
    }

    :EndNameSpace
```

Now we run the tests in the current directory. It will find the censor\_tests.dyalog file and execute the tests. We should expect a failure:

```
    #.UT.run './'

    CRASHED: censor_pony_TEST
    Expected
        ┌→──────────────────┐
        │little xxxx friends│
        └───────────────────┘
    Got
        ┌→──────────────────────────────────────────────────────────┐
        ↓VALUE ERROR                                                │
        │censor_pony_TEST[2] 'pony'#.censor.run'little pony friends'│
        │                   ∧                                       │
        └───────────────────────────────────────────────────────────┘
    -----------------------------------------
    ./censor_tests.dyalog tests
        ⍋  Passed:  0
        ⍟ Crashed:  1
        ⍒  Failed:  0
        ○ Runtime:  0 m 0 s 20 ms
```

We can now write the implementation function.

```
    run←{
        w←⍺
        t←⍵
        i←(w⍷t)/⍳⍴t
        t[¯1+(⍳⍴w)+i]←(⍴w)⍴'x'
        t
    }
```

And we can re-run the tests:

```
    #.UT.run './'
    Passed  0 m 0 s 0 ms
    -----------------------------------------
    ./censor_tests.dyalog tests
        ⍋  Passed:  1
        ⍟ Crashed:  0
        ⍒  Failed:  0
        ○ Runtime:  0 m 0 s 6 ms
```

Now - we can write the test for a list of several censored words.

```
    censor_many_TEST←{
    #.UT.expect←'xxxx in the xxxshine'
    'pony' 'sun' #.censor.process 'pony in the sunshine'
    }
```

Re-running the tests will cause our newest test to fail:

```
    #.UT.run './'
    CRASHED: censor_many_TEST
    Expected
    ┌→───────────────────┐
    │xxxx in the xxxshine│
    └────────────────────┘
    Got
 ┌→─────────────────────────────────────────────────────────────────────┐
 ↓VALUE ERROR                                                           │
 │censor_many_TEST[2] 'pony' 'sun'#.censor.process'pony in the sunshine'│
 │                    ∧                                                 │
 └──────────────────────────────────────────────────────────────────────┘
    Passed  0 m 0 s 1 ms
    -----------------------------------------
    ./censor_tests.dyalog tests
        ⍋  Passed:  1
        ⍟ Crashed:  1
        ⍒  Failed:  0
        ○ Runtime:  0 m 1 s ¯979 ms
```

We can now write the implementation for this process function:

```
    process←{⊃run/⍺,⊂⍵}
```

Finally we re-run the tests:

```
    #.UT.run './'
    Passed  0 m 0 s 0 ms
    Passed  0 m 0 s 0 ms
    -----------------------------------------
    ./censor_tests.dyalog tests
        ⍋  Passed:  2
        ⍟ Crashed:  0
        ⍒  Failed:  0
        ○ Runtime:  0 m 0 s 8 ms
```

They all passed. Now we know that we are done with the implementation.

Since we have tests for correctness, we are free to work the code over (refactoring), as long as we run all the tests often.

This will be done now, I will simplify the run function a little. I will inline the index (i) and remove the temporary variable (w).

```
    run←{
        t←⍵
        t[¯1+(⍳⍴⍺)+(⍺⍷t)/⍳⍴t]←(⍴⍺)⍴'x'
        t
    }
```

After this change, I rerun the tests.

```
    #.UT.run './'
    Passed  0 m 0 s 0 ms
    Passed  0 m 0 s 0 ms
    -----------------------------------------
    ./censor_tests.dyalog tests
        ⍋  Passed:  2
        ⍟ Crashed:  0
        ⍒  Failed:  0
        ○ Runtime:  0 m 0 s 8 ms
```

All is good!

This approach of starting from the bottom up, is called the ‘Chicago School of TDD’.

Now, I can focus on the next requirement.

#### Final code

The final version of the censoring program from censor.dyalog

```
    :NameSpace censor

        process←{ ⊃run/⍺,⊂⍵ }

        run←{
        t←⍵
        t[¯1+(⍳⍴⍺)+(⍺⍷t)/⍳⍴t]←(⍴⍺)⍴'x'
        t
        }

    :EndNameSpace
```

The tests from censor\_tests.dyalog:

```
    :NameSpace censor_tests

    censor_pony_TEST←{
        #.UT.expect←'little xxxx friends'
        'pony'#.censor.run'little pony friends'
    }

    censor_many_TEST←{
        #.UT.expect←'little xxxx in the xxxshine'
        'pony' 'sun'#.censor.process'little pony in the sunshine'
    }

    :EndNameSpace
```

### References

1. <a id="ref1"></a>  [http://en.wikipedia.org/wiki/Test-first\_programming/](http://en.wikipedia.org/wiki/Test-first_programming)
