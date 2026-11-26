---
vid: '10007460'
title: Static Exception Handling for APL+Win
authors:
- Bill Rutiser
byline: by William Rutiser, APL2000 (wru@apl2000.com) Talk given at the 2005 APL2000 Users’ Meeting
volume: '22'
issue: '2'
page: '79'
source: trad/v222/try222.htm
converter: vecrescue 0.1.0
issue_tab: n2-march-2006
tags:
- language design
- implementations
---

by William Rutiser, APL2000 ([wru@apl2000.com](mailto:wru@apl2000.com))  
Talk given at the 2005 APL2000 Users’ Meeting
{ .byline }

## Introduction

This paper introduces try-catch exception handling, a feature recently added to APL+Win.

Early APL interpreters treated most exceptions by printing an error message and then suspending in immediate execution mode. A branch statement entered from the terminal caused execution to resume. Subsequent to the de facto standardization of the core APL language, each interpreter vendor introduced their own facility for dealing with errors. STSC, APL2000’s corporate ancestor, added the <span class="APLU">⎕ELX</span> and <span class="APLU">⎕DM</span> system variables. In the event of an error, the interpreter sets <span class="APLU">⎕DM</span> to the formerly printed diagnostic message, and then executes the current latent expression assigned to <span class="APLU">⎕ELX</span>. That expression may include a branch statement to resume execution. Two decades of experience have shown this simple facility to be sufficient for implementing both simple and complex exception handlers.

There are two widely used patterns for the design of <span class="APLU">⎕ELX</span> based handlers. One pattern relies on application wide adherence to protocols for manipulating <span class="APLU">⎕ELX</span>. Local error handlers are dynamically installed by changing <span class="APLU">⎕ELX</span> and removed by restoring or reconstructing the previous handler. In the other pattern, <span class="APLU">⎕ELX</span> is set once and for all to invoke a global exception handling function. This relatively complicated function parses the code near the near the failing statement to locate and execute local handlers. In either case, effective exception handling code is often difficult to write and maintain. And its very presence can conceal the intent of its surroundings.

Like control structures, this new static textually based exception handling facility is intended to enable clearer and more reliable code.

## Preliminary Notes

This paper makes no important distinction between exceptions and errors. Errors are exceptions, and the interpreter describes some exceptions as errors. Exceptions may be usefully classified as *anticipated* or *unanticipated*. Anticipated exceptions are fully understood, and are known to arise and are essential to handle during a program’s routine execution. Consider a <span class="APLU">FILE INDEX ERROR</span> while sequentially reading components. It is, or should be, an anticipated event that is usually handled by terminating the reading loop and closing the file.

Contrast that error with the <span class="APLU">XFHOST ERROR</span>. Its diagnostic message includes a 32 bit numeric code and a brief error description, both provided by the host operating system. After days of research you may only have learned that an unknown bad thing happened in a previously unheard of corner of the system. This is an unanticipated exception. Unanticipated exceptions are unknown or incompletely understood before they happen. They are often triggered by a defect or error in the application program or its software and hardware environment.

This paper uses several forms of the verb “to raise” to refer to the APL interpreter’s initiation of an error report or exception. When passive voice is used, the interpreter is often the intended actor. In APL code, descriptions in *italic sans-serif type* replace overly specific and irrelevant details. *Italic body text* indicates both emphasis and definition.

### Overview of the Try-Catch Control Structure

The try-catch control structure extends the APL+Win control structure family. It is designed to be compatible with and interact with existing <span class="APLU">⎕ELX</span> handlers. Adding a try-catch structure to an existing application should not interfere with anything outside the structure. The structure’s influence is confined to the statements it contains where it always takes precedence over <span class="APLU">⎕ELX</span>. The following brief examples illustrate its two basic forms.

### Catching All Exceptions

```
:Try
```

*A block of statements to try*

```
:CatchAll
```

*A block of statements to handle any exception*

```
:EndTry
```

The first block of statements (the *try-block*) is executed. If an exception should arise, it will be handled by the second block (a *catch-block*).

### Catching Specific Exceptions

```
:Try
```

*A block of statements to try*

```
:CatchIf 'FILE' ≡ 4 ↑⎕DM
```

*A block of statements to handle just FILE errors*

```
:End
```

The try-block is executed. If an exception arises, the expression in the <span class="APLU">:CatchIf </span>statement is evaluated. When its value is <span class="APLU">1</span>, the exception is handled by the second block of statements. Otherwise, the exception is passed to the next surrounding exception handler, which may be another try-catch structure or <span class="APLU">⎕ELX</span>.

## Try-Catch Details

### Syntax

The try-catch structure consists of a sequence of several clauses terminated by an end-statement. Each *clause* begins with a control statement which is optionally followed by a block of ordinary statements. These blocks may contain try-catch and other control structures which must be correctly nested but are otherwise unrestricted.

This skeleton structure shows all possible clauses:

```
:Try
```

*Statements to try*

```
:CatchIf first condition expression
```

*Statements to handle exceptions satisfying the first condition*

```
:CatchIf second condition expression
```

*Statements to handle exceptions satisfying the second condition*

```
:CatchAll
```

*Statements to handle all other exceptions*

```
:EndTry
```

The structure always begins with a <span class="APLU">:Try</span> clause and ends with a <span class="APLU">:EndTry</span> statement. As with other control structures, the keywords are case insensitive. The <span class="APLU">:End</span> keyword may be used instead of the <span class="APLU">:EndTry</span> keyword.

There must be at least one <span class="APLU">:CatchIf</span> or <span class="APLU">:CatchAll</span> clause. There may be no more than one <span class="APLU">:CatchAll</span> clause. All <span class="APLU">:CatchI</span>f clauses must precede the <span class="APLU">:CatchAll</span> clause.

The <span class="APLU">:CatchIf
</span>statement consists of its keyword followed by an expression that yields <span class="APLU">1</span> or <span class="APLU">0</span>. The other control statements consist of only a keyword.

Each control statement except <span class="APLU">:EndTry</span> is followed by a block of APL statements (which may be empty). These blocks may contain additional try-catch and other control structures, which must be correctly nested but are otherwise unrestricted.

As with other control structures, an incomplete or incorrect try-catch structure will cause an <span class="APLU">OUTER SYNTAX ERROR</span>.

### Execution

Execution begins at the first statement in the try-block and continues from statement to statement in the usual way. When control reaches the end of the try-block, control passes out of the try-catch structure. When an exception arises within a try-block statement, the interpreter assigns the usual diagnostic message to <span class="APLU">⎕DM</span>. Control then passes, in turn, to each catch-statement until one *accepts* the exception. The interpreter’s handling of an *accepted* exception is complete. If an exception is not accepted by any of the catch-statements, the interpreter passes the exception to a surrounding try-catch structure for handling. When no surrounding structure is present, <span class="APLU">⎕ELX</span> is activated.

The <span class="APLU">:CatchAll</span> statement accepts any exception that reaches it.

The <span class="APLU">:CatchIf</span> statement accepts an exception only if its condition expression evaluates to <span class="APLU">1</span>. In most instances, the expression should depend on <span class="APLU">⎕DM</span>.

When a <span class="APLU">:CatchIf</span> or <span class="APLU">:CatchAll</span> statement accepts an exception, the following block of statements is executed, after which, control passes out of the try-catch structure. When the block is empty, the exception is handled but otherwise ignored.

### Extent of the try-block

A try-catch structure handles *only* exceptions arising within:

- Statements *textually* contained in the try-block.
- Statements contained in arguments to the execute primitive (<span class="APLU">⍎</span>) in statements satisfying these criteria.

This recursive rule allows executes within executes.

A try-catch structure *does not* handle exceptions raised in functions *called* from its try-block. These may be handled by a try-catch structure in the function where the exception was raised or by its ambient <span class="APLU">⎕ELX</span> setting.

Exceptions arising directly in immediate execution statements, event handlers, or latent expressions are *never handled* by try-catch structures.

### Exceptions in the try-catch structure itself

An exception arising in a catch-block or <span class="APLU">:CatchIf</span> condition is immediately passed to an enclosing try-catch statement or the ambient <span class="APLU">⎕ELX.⎕DM</span> is changed to describe the new exception. The <span class="APLU">⎕DM</span> message for the previous exception is lost.

## Using the Try-Catch Control Structure

These examples are intended to be illustrative and suggestive rather than complete. Some conclude with a discussion of failure modes, programming style, and other pragmatic matters. The author’s suggestions are stated as imperative prescriptions.

### Handling a Single Specific Anticipated Error

```
∇ ProcessAllComponents ftn;i;eof
   i←1
   eof←0
   :while ~eof
     :try
        ProcessOneComponent ⎕fread  ftn i
        i← i + 1
     :catchif 'FILE INDEX' ≡ 10↑⎕dm
        eof←1
     :end
   :end
∇
```

Here, a <span class="APLU">FILE INDEX</span> error is anticipated. <span class="APLU">A FILE TIE ERROR</span>, probably a consequence of a coding error is not anticipated. Keep in mind that exceptions directly raised in <span class="APLU">ProcessOneComponent</span> will not be seen by this try-catch; they may be handled by a try-catch in the called function or by its ambient <span class="APLU">⎕ELX</span>.

### Handling Several Anticipated Errors

Applications involving trigonometry, coordinate geometry, or complex numbers may require the arc tangent of the quotient of a pair of numbers. This example deals with one tricky part of the computation – computing a result when the divisor is very small or zero. The theoretical quotient may be very large or infinite. The division operation can go wrong in two ways: division by zero raises a <span class="APLU">DOMAIN ERROR</span>; division of an extremely large number by an extremely small number raises a <span class="APLU">LIMIT
ERROR</span>.

```
∇ z ← x principal_arc_tan y
   :Try
      z← ¯3 ○ y ÷ x
   :CatchIf 'DOMAIN ERROR' ≡ 12 ↑ ⎕DM
      z← ○ ÷ 2
   :CatchIf 'LIMIT ERROR' ≡ 11 ↑ ⎕DM
      z← ○ ÷ 2
   :End
∇
```

This fairly simple example illustrates a pitfall that must be considered for all exception handling code, in every language, using any exception handling features. This example does correctly catch a <span class="APLU">DOMAIN</span> or <span class="APLU">LIMIT ERROR</span> raised in the divide primitive, however this try-catch structure would also catch the same errors if raised by the circular functions primitive. The author is only “pretty sure” that the code is safe as written. But this is an example and not an extract from a real application where the try-block could be much larger and more complicated and consequently harder to analyze with any certainty.

Therefore, when handling specific exception possibilities make the try-block as small and simple as practical to reduce the risk of errors in your analysis of possible exceptions.

The text of <span class="APLU">⎕DM</span> contains unused information about the exception’s source. It would be possible, with suitable support utilities, to parse the entire message to verify that the caret points to the divide operation.

Therefore, make the catch-if selections as specific as necessary to exclude unanticipated exceptions.

### Handling Unanticipated Errors in a Transaction

```
∇ AttemptOneTransaction details
   :Try
```

*Statements to do one transaction.*

```
   :CatchAll
     ⎕← "Something bad has happened!!"
     ⎕← "⎕DM is:"
     ⎕←⎕DM
     ⎕← "Abandoning this transaction and"
     ⎕← "continuing with the next."
     z← "Failed"
     :Return
   :EndTry
   z← "OK"
   :Return
∇
```

### Ignoring All Exceptions

This example is supposed to compute the value of a function Foo with given arguments. The code to do this raises one or more known exceptions for some argument values. A specified default result value is to be returned when such an exception arises.

<pre class="APLU">∇ Z← default_result FindFooWithDefault args
   Z← default_result
   :Try
     <i>Statements to compute and set Z to Foo(args)</i>
   :CatchAll
   :EndTry
∇</pre>

The catchall-statement accepts the exception but its empty handler does nothing, leaving Zset to the desired default result.

Using a catch-all clause, empty or not, to handle anticipated exceptions is somewhat risky unless you are certain that no unanticipated exceptions are possible. Therefore, reduce the risk by handling anticipated exceptions as specifically as practical.

Notice that Z is set to the default result before the try-block is executed with the expectations that the try-block will reset it to the correct computed value or leave it unchanged otherwise. A programmer could easily and mistakenly use Z for an intermediate result; after all, it is *always* assigned a value a few lines (or pages) down the function. This mistake becomes more likely as the try-block becomes larger and the program is passed on to other maintainers. Moving the default result assignment to the catchall-block prevents this particular mistake; it also clarifies the assignment’s purpose.

A root cause of this programming error is that there are *invisible* control paths leaving the try-block. Overlooking such paths may invalidate any conclusion based on examination downstream code.

Therefore, always remember that every primitive operation in the try-block potentially contains an invisible branch to the catch-block. Practice defensive programming where practical.

### Handling Exceptions in Called Functions

This example illustrates a simple global handler for unanticipated errors. It also illustrates <span class="APLU">⎕ELX</span> and the try-catch structure working together. Only two called functions are shown here. Imagine that there are many others.

<pre class="APLU">∇ FooOne
   <i>Other stuff</i>
   FooTwo
   <i>More other stuff</i>
∇</pre>

<pre class="APLU">∇ FooTwo
    <i>Even more other stuff</i>
    "two" + 2
    <i>And more other stuff</i>
∇</pre>

Calling <span class="APLU">FooTwo</span> will yield a <span class="APLU">DOMAIN ERROR</span>. This example just displays the error message and a simple trace-back.

```
∇ CatchDeepErrors;⎕ELX
   ⎕ELX← " ⎕error '@ ' , ⎕DM"
   :Try
      FooOne
   :CatchIf '@' ≡↑⎕DM
      ⎕← ⊃(~ ⎕DM = ⎕TCNL) ⊂ ⎕DM
   :CatchAll
      ⎕ERROR "PANIC"
   :End
∇
```

Execution produces:

```
      CatchDeepErrors
@ @ DOMAIN ERROR
FooTwo[??]  "two" + 2
                 ^
FooOne[??]  FooTwo
            ^
CatchDeepErrors[3]     FooOne
                       ^
```

At each SI level, <span class="APLU">⎕ELX</span> handles an error by raising a new exception in the preceding SI level. The “<span class="APLU">@</span>” characters indicate the number of stack levels traversed and distinguish exceptions raised in the try-block from those raised in the called functions. Simple parsing of the accumulated diagnostic messages provides the trace-back.

Some programming language designers have chosen “throw”as the keyword in a statement that programmatically raises an exception. This example *throws* an exception up (or down as the reader prefers) the call stack.

### Intercepting an Exception Being Thrown Up the Stack

Suppose, in the preceding example, that FooOne normally takes some action to release a resource before returning.

<pre class="APLU">∇ FooOne
   <i>Open a file</i>
   FooTwo
   <i>Close the file</i>
∇</pre>

When an exception is thrown up from <span class="APLU">FooTwo</span>, the file will be left open, perhaps causing trouble in an unrelated part of the application. We can fix this by involving <span class="APLU">FooOne</span> in the exception throwing.

<pre class="APLU">∇ FooOne
   :Try
      <i>Open a file</i>
      FooTwo
   :CatchIf '@' ≡↑⎕DM
      <i>Close the file</i>
      ⍎⎕ELX
   :End
   <i>Close the file</i>
∇</pre>

<span class="APLU">FooOne</span> has been changed to catch the exception that the latent exception throws from <span class="APLU">FooTwo</span>. After closing the file, <span class="APLU">FooOne</span> executes <span class="APLU">⎕ELX</span> to throw the exception to the next level.

In a real application, careful attention should be given to the possibility of something else going wrong while closing the file. This example does not handle exceptions raised while opening or closing the file.

Readers familiar with some other popular programming languages will see this example as one where a <span class="APLU">:Finally</span> clause would be helpful. Such an enhancement is under consideration.

### Another Way to Emulate a <span class="APLU">:Finally</span> Clause

In this example, we give the global <span class="APLU">⎕ELX</span> an additional responsibility. It must execute the value of the variable <span class="APLU">∆FINALLY</span> at each level.

Change <span class="APLU">CatchDeepErrors</span> to set <span class="APLU">⎕ELX</span> with:

```
⎕ELX← " ⍎∆FINALLY ⋄ ⎕ERROR '@ ' , ⎕DM "
```

With this in place we can use it in <span class="APLU">FooOne</span>.

<pre class="APLU">∇ FooOne;∆FINALLY
   ∆FINALLY←" Close the file "
   <i>Open a file</i>
   FooTwo
   ⍎∆FINALLY
∇</pre>

Again we should think about problems closing the file.

This idea can be extended and improved in a number of ways. There is a potential <span class="APLU">VALUE ERROR</span> in the reference to <span class="APLU">∆FINALLY</span>. Since the variable will be empty in functions that don’t need its effect, it would be only a small matter of programming to make it optional. The magic variable could be replaced with a magic label or private comment in the function itself. Most of the <span class="APLU">⎕ELX</span> code should be moved into a new function but the <span class="APLU">⎕ERROR</span> invocation must remain in the latent expression. There will be the usual issues with shadowed variables to resolve.

### Suspending Execution in a Handler

This trick doesn’t really involve exception handling except that a catch-block is a likely place to use it. While experimenting or debugging it may be useful for a program to programmatically suspend for immediate execution. This shows one way to do that. When active, Code Walker will show the stop suspension. The statement label gives, by convention, a convenient common branch target when resuming execution.

```
∇ Suspend
   :Try
      × "do" "something" "wrong"
   :CatchAll
      0 0 ⍴ (1 + ↑ ⎕lc) ⎕stop 'Suspend'
GOON: 0 0 ⍴ "place to stop"
   :End
∇
```

The stop could be set manually; however, it would need to be set manually whenever the function is edited or otherwise redefined.

### Retrying After Correcting an Exception

Some situations require that the try-block be retried after somehow improving the chances of success – perhaps by creating a missing file or waiting for a shared resource to become available.

<pre class="APLU">∇ TryAndTryAgain
   :Try
RETRY:
      <i>Do something</i>
   :CatchIfsome condition
      <i>Fix the problem</i>
      :GOTO RETRY 
   :End
∇</pre>

This use of the dreaded <span class="APLU">:GOTO</span> statement forms a loop. It will never end unless the problem is really fixed. Therefore, consider using a counter or timer to limit retries.

<pre class="APLU">∇ TrySeveralTimes retry_limit;retry_count
   retry_count← 0
   :Try
RETRY:
      <i>Do something</i>
   :CatchIf retry_count ≥ retry_limit
      ⎕ERROR "Retry limit exceeded"<br>   :CatchIf  <i>some condition</i>
      <i>Fix the problem</i>
      retry_count← retry_count + 1
      :GOTO RETRY 
   :End
∇</pre>

The first catch-if condition is unusual in that its condition doesn’t refer to ⎕DM.

## General Advice

This section, like the suggestions after the examples above, expresses the author’s advice, perhaps with more certainty than is really warranted.

Serious exception handling may be hard. This is particularly so for exceptions initiated outside the APL interpreter. Correct handlers depend on detailed knowledge of the source and causes of the exception to be handled. Sometimes, such knowledge is not available and may be obtained only through research and experimentation.

Therefore, carefully consider the costs before attempting to handle situations that can’t be readily reproduced. Where possible, fix root causes instead of trying to recover from their manifestations. Remember that a clean termination is usually better than continuing after a bungled recovery.

Handlers for unanticipated exceptions should preserve evidence.

Handle unanticipated exceptions globally; handle anticipated exceptions locally.

## Prospects

Prior to this conference and this paper, the try-catch structure has had neither documentation nor significant use in real programs. Reports of testing and experiments by conference participants will help to improve try-catch in its production quality release. Experience and bug reports may be sent to *[support@apl2000.com](mailto:support@apl2000.com)*. General comments, suggestions, and criticism may be sent to the author at *[wru@apl2000.com](mailto:wru@apl2000.com)*. (Cash bribes included in an email message envelope are not guaranteed to reach the author.)

Some extensions being considered:

• A <span class="APLU">:Finally</span> clause.

• A variant of catch-if with pattern matching capability.

• Enabling the use of <span class="APLU">:AndIF</span>and <span class="APLU">:OrIf</span> statements in catch-if clauses.

## Some Plagiarized Wisdom

Everyone knows that debugging is twice as hard as writing a program in the first place. So if you’re as clever as you can be when you write it, how will you ever debug it? – *Brian Kernighan*

There are two ways of constructing a software design. One way is to make it so simple that there are obviously no deficiencies. And the other way is to make it so complicated that there are no obvious deficiencies. – *C.A.R. Hoare*

Dead programs tell no lies.

Don’t publish papers when you have no experience with the subject.

Pay no attention to the grumpy old programmer behind the curtain.

***
