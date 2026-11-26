---
vid: '10007120'
title: Function Arrays in APL
authors:
- Gianluigi Quario
byline: by Gianluigi Quario (giangiquario@hotmail.com)
volume: '21'
issue: '4'
page: '39'
source: trad/v214/funcarray.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- language design
- programming techniques
---

by Gianluigi Quario (giangiquario@hotmail.com)
{ .byline }

The foundations of APL have been be extended to encompass arrays of functions. Nevertheless we have few APL’s instruments for handling those arrays: this note is an attempt to open up a new panorama to those willing to cultivate this meagre field.

## Building an Array of Functions

It is possible to build an array-of-functions by means of unnamed namespaces.

You can define a set of unnamed namespaces like this:

```
     (ns1 ns2 ns3)←(⎕NS ⍬)(⎕NS ⍬)(⎕NS ⍬)
```

and build a namespace-array

```
     nsA←ns1,ns2,ns3
```

`nsA` is a vector-of-namespaces (shape is 3) and its name class is 2.

Let FOO be a function, for example

```
     FOO←{3+⍵}
```

and make some assignments:

```
     ns1.f←FOO ⋄ ns2.f←FOO  ⋄ ns3.f←FOO
```

Then `nsA.f` is a set of functions. Try

```
     nsA.f
 #. ∇f    #. ∇f    #. ∇f
```

`nsA.f` is a strange object: its name class is 0 ... but

```
     nsA.f 5
8 8 8
```

and

```
     nsA.⎕NC'f'
3 3 3
```

Now let us write

```
     FA←nsA.f
```

and obtain

```
     ⎕NC'FA'
3
```

FA is an actual array-of-functions:

```
     FA 5
8 8 8

     FA 5 6 7
8 9 10

     FA 5 6
LENGTH ERROR
```

You can also expunge the involved namespaces and functions; the array-of-functions FA is still alive:

```
     ⎕EX 5 3⍴'ns1ns2ns3nsAFOO'
     FA 5
8 8 8
```

FA has its own actual identity amongst other APL objects.

## Array of Functions for the Parallel Headed APLer

The APL scalar functions have a pervasive behaviour over their arguments and are the most "politically correct" functions in a parallel environment. When a function is not scalar, we can force it to behave in a more diligent manner by means of some operator like primitive each `¨` or “saw” or “perv”.

I do not know how you envisage those operators; my basic instinct is to look at them like substitutes for loops.

At the opposite the definition of an array-of-functions carries my mind in a different mood and I actually feel myself thinking in a more holistic way when my attention is forwarded to every kind of arrays.

Consider for example:

```
     ⍴¨ (1 2 3)('abcde')
3  5
```

I usually read (i.e. my internal semantics interpreter reads) that statement in this way: “compute the shape of first vector and afterwards of the second one”

But if we afford this task ...

```
     nsA←(⎕ns ⍬) (⎕ns ⍬)
     nsA[1].f←⍴ ⋄ nsA[2].f←⍴
     RHO_parallel←nsA.f
```

... then my mood is much different when I look at:

```
     RHO_parallel (1 2 3)('abcde')
3  5
```

We can avoid the upper manual task of defining many namespaces and afterwards assigning a function by means of a new operator:

```
     Parallelized←{((⍴⍵)⍴(⎕NS ⍬).##).⍺⍺ ⍵}
```

and obtain in a more direct manner:

```
     ⍴Parallelized (1 2 3)('abcde') ⎕A ⎕A
3  5  26  26
```

That new operator can be rewritten in a more general way:

```
    Parallelized←{
      0∊⍴⍵:⍵
      ⍺←{⍵}           ⍝ambivalency
      ⍺((⍴⍵)⍴(⎕NS ⍬).##).⍺⍺ ⍵
     }
```

It may be the mate of both monadic and dyadic functions, both primitive and defined. Furthermore it modifies the behaviour of primitive each `¨` operator.

I think that the implementation of each operator by Dyalog APL has some drawbacks.

Let us consider the monadic primitive each:

```
    (0⍴⊂⍬)≡⍴¨⍬
1

    (0⍴⊂⍬)≡⍴¨''
1

    (0⍴⊂'')≡⍴¨''
0
```

That I cannot understand!

The Dyalog APL language reference says:

> “If the argument Y is empty, the derived function is applied once to the prototype of Y, and the shape of R is the shape of Y.”

The thinking behind Dyalog’s implementation of each on null arrays is that the prototypical item of the result is determined by the *function*, rather than the argument.

Owing to the fact that “each” is an operator, a more consistent behaviour ought to be:

> “If the argument Y is empty, the derived function is the prototype of function operand Lop.”.

The prototype of any function could be the “TRANSPARENT” function.

You can also consider another example:

```
     {⍵[⍋⍵]} ¨ ⍬
RANK ERROR
```

On the contrary it should happen like the following:

```
     {⍵[⍋⍵]} Parallelized ⍬
```

gives the zero length numeric vector.

I like to baptize the Parallelized operator with the name “peach”, that means parallel each.

```
     peach←Parallelized
```

## Building an Array of Different Functions

We are now going to build an array of possibly different functions.

The procedure is similar to what seen beforehands.

A namespace-array is built:

```
     nsA←⎕NS peach 3⍴⊂⍬   ⍝length 3 vector
```

and some assignments are made:

```
     nsA[1].f←+ ⋄ nsA[2].f←-  ⋄ nsA[3].f←÷

     FA←nsA.f
```

Now the array-of-funtions FA can be exploited:

```
     FA 5
5 ¯5 0.2

     FA 3 4 5
3 ¯4 0.2
```

You can see that there is a major duality tie between the array-of-data and the array-of-functions; APL always tries to behave the parallel way:

<ol type="a">

<li> if FA is a (scalar) function and DA is an array-of-data, then FA DA gives
   an array with the same shape as DA</li>
<li> if FA is an array-of-functions and DA is a scalar datum, then FA DA gives
   an array with the same shape as FA</li>
<li> if FA is an array-of-functions and DA is an array-of-data, then FA and DA must 
have the same shape and FA DA gives an array with the same shape as FA and DA

</li></ol>

In Vector Vol.20 No.1 Graeme D.Robertson illustrated a mathematical application of a vector-of-functions related to the velocity of a fluid at a point in 3D space;  
in traditional notation:  `V(x,y,z)=(xy, -xy2, yz2)`

We define:

```
     fx←{⎕IO←1 ⋄ ⍵[1]×⍵[3]} ⋄ fy←{⎕IO←1 ⋄ -⍵[1]×⍵[2]*2} ⋄  fz←{⎕IO←1 ⋄ ⍵[2]×⍵[3]*2}

     nsA←⎕NS peach 3⍴⊂⍬   ⍝length 3 namespace-vector

     nsA[1].f←fx ⋄ nsA[2].f←fy  ⋄ nsA[3].f←fz

     FA←nsA.f
```

and now can exploit the vector-of-functions FA:

```
     FA 1 2 3
3 ¯4 18
```

## Tools for Handling Arrays of Functions

The Dyalog 10.0 APL interpreter does not allow to handle arrays-of-functions in a direct way.

You cannot use indices:

```
     FA[1]
```

is a pitfall for the interpreter;

```
     1⊃FA
SYNTAX ERROR

     4⍴FA
SYNTAX ERROR
```

The structural functions need an effort to be promoted to operator level in order to handle the arrays-of-functions.

But now the arrays-of-functions are a kind of black boxes after they were built.

We may look for some workarounds.

## Transformation of a Vector of Functions into a Vector of Namespaces

First of all, let us find a way to obtain an array-of-namespaces from an array-of-functions.

We shall use the FA’s canonical representation, which is a class 2 object.

When FA is a vector of functions, its canonical representation is a (complicated) nested array but it is possible extract the built-in functions.

I defined a traditional operator for executing that job; it was not possible to define a function, because its arguments cannot be class 3 objects.

The syntax is

```
         nsArray←(dummyOperand FAtoNS FunctionVector)dummyArg
```

the result is a namespace-array (class 2) where every namespace contains only one function.

The operator `.FAtoNS` looks complicated because of the complicated structure of canonical representations inside operators. It could be transformed to a Defined function, because the result is a class 2 object; the version here represented allows to show and comment the logical structure.

```
∇
 nsArray←(fdummy FAtoNS funcArray)dummy;cr;peach;funcName;lasteach;last;true_cr;extract_cr;enlist
  ⍝ transform a single function or a vector of functions into a namespace array
  ⍝ funcArray is a function(0 rank vector of functions) or a vector of functions
  ⍝ nsArray is a namespace-Array(vector) where each ns contains 1 function whose name is f
 cr←⎕CR'funcArray'
 :If 2=⍴⍴cr         ⍝operand is a single function
 :OrIf 2≥|≡cr       ⍝  or a primitive function
     nsArray←{ ⍝returns a namespace with function ¨f¨ embedded
         ns←⎕NS ⍬ ⋄ ns.f←⍎⍵ ⋄ ns
     }'funcArray'
 :Else              ⍝operand  is an array of functions
     peach←{ ⍝parallel each operator
         2≠⎕NC'⍺':((⍴⍵)⍴(⎕NS ⍬).##).⍺⍺ ⍵
         ⍺((⍴⍵)⍴(⎕NS ⍬).##).⍺⍺ ⍵
     }
     enlist←{⎕ML←0           ⍝ List ⍺-leaves of nested array.
         ⍺←0                 ⍝ default: list 0-leaves.
         ⍺≥¯1+|≡⍵:,⍵         ⍝ all shallow leaves: finished.
         1↓↑,/(⊂⊂⊃⊃⍵),⍺ ∇¨,⍵ ⍝ otherwise: concatenate sublists.
     }
     :If 3∊⍴cr      ⍝ housekeeping:⎕CR contains a namespace reference
     :AndIf '.'≡⊃1↓cr
         cr←⊃¯1↑cr
     :EndIf
     extract_cr←{2>|≡⍵:⍵ ⋄ ∇⊃¯1↑⍵}
     true_cr←extract_cr peach cr   ⍝canon.rep of all functions
     nsArray←{ ⍝returns a namespace with function ¨f¨ embedded
         ns funcName←{  ⍝returns function_name and namespace where function was defined
             ~(,3)≡⍴⍵:⍬(⎕FX' ',⍵)
             (⊃⍵)(⎕FX' ',⊃¯1↑⍵)
         }⍵
         0=1↑0⍴funcName:{ns←⎕NS ⍬ ⋄ ns.f←⍎,enlist ⍵ ⋄ ns}⍵   ⍝without canonical representation (primitive function)
         ns{                                                 ⍝with    canonical representation
             0∊⍴⍺:{ns←⎕NS ⍬ ⋄ ns.f←⍎,enlist ⍵ ⋄ ns}⍵
             ⍺{ns←⎕NS ⍬ ⋄ ns.f←⍺⍎,enlist ⍵ ⋄ ns}⍵
         }funcName
     }peach true_cr
 :EndIf
∇
```

## Reshaping an Array of Functions

The following operator allows the return of the shape of an array-of-functions.

The syntax is

```
         shape←(dummyOperand FA_shape FunctionArray)dummyArray
```

the result is a vector (class 2) of the shape of the array-of-functions.

```
∇
 shape←(fdummy FA_shape funcArray)dummy
   ⍝return shape of an array-of-functions:primitive"monadic ⍴"is promoted to function level-GQR 20040210
   ⍝ func is a function(0 rank vector of functions) or a vector of functions
   ⍝ try:
   ⍝   ⎕←('' FA_shape {2×⍵})''
   ⍝   FA2←(2 3 FA_reshape +)''       ⍝FA2 is an array-of-functions
   ⍝   ⎕←('' FA_shape FA2})''
 shape←⍴(+FAtoNS func)⍬
 ⍝  ⎕NC'funcArray'←→3     ⎕NC'shape'←→2
∇
```

The following operator allows the reshaping of an array-of-functions.

The syntax is

```
        ArrayFunc←(shape FA_reshape funcArray)dummyArray
```

the result is an array-of-functions (class 3) obtained by reshaping another array-of-functions (class 3).

```
∇
 ArrayFunc←(shape FA_reshape funcArray)dummy;ns
 ⍝reshape an array-of-functions: primitive"dyadic ⍴"is promoted to function level-GQR 20040204
   ⍝ funcArray is a function(0 rank vector of functions) or a vector of functions
   ⍝ ArrayFunc is an Array-of-Functions
     ⍝ try:
     ⍝   FA2←(2 3 FA_reshape +)0       ⍝FA2 is an array-of-functions
     ⍝   FA2←(4 FA_reshape {2×⍵})0     ⍝FA2 is a vector-of-functions
     ⍝   FA3←(1 3 FA_reshape FA2)0     ⍝FA3 is an array-of-functions
 ns←(+FAtoNS funcArray)⍬  ⍝the function_array becomes a namespace_array;⎕NC'ns'←→2
 ArrayFunc←(shape⍴ns).f
     ⍝  ⎕NC'funcArray'←→3     ⎕NC'ArrayFunc'←→3
∇
```

## Indexing an Array of Functions

The following operator allows the return of a sub-Array from an array-of-functions.

The syntax is

```
        ArrayFunc←(indicesArray FA_from funcArray)dummyArray
```

the result is an array-of-functions (class 3) obtained by means of another array-of-functions (class 3).

For seek of simplicity let us impose that indicesArray is a vector with same length as the shape of funcArray operand.

```
∇
 ArrayFunc←(indicesArray FA_from funcArray)dummyArray;ns
    ⍝indexing of an array-of-functions: primitive "[ ]"is promoted to function level
   ⍝ funcArray is a function(0 rank vector of functions) or a vector of functions
   ⍝ ArrayFunc is an Array-of-Functions
     ⍝ try:
     ⍝   FA2←(2 4 FA_reshape {2×⍵})0     ⍝FA2 is a (2×4) array-of-functions
     ⍝   FA3←(1 1 FA_from FA2)0          ⍝FA3 is a (scalar) array-of-functions
     ⍝   FA4←((1 2) (2 3)FA_from FA2)0   ⍝FA4 is a (2×2) array-of-functions
 ns←(+FAtoNS funcArray)⍬  ⍝the function_array becomes a namespace_array;⎕NC'ns'←→2
 :Select ⍴⍴ns
 :Case ,0 ⋄ ∘ ⍝length error
 :Case ,1 ⋄ ArrayFunc←ns[indicesArray].f
 :Case ,2 ⋄ ArrayFunc←ns[⊃1↑indicesArray;⊃¯1↑indicesArray].f
 :Case ,3 ⋄ ∘ ⍝ et coetera
 ⍝ et coetera
 :EndSelect
 ⍝  ⎕NC'funcArray'←→3     ⎕NC'ArrayFunc'←→3
∇
```

## Catenating two vectors of functions

By means of the last structural operator we can have arrays-of-functions whith different function-items.

The syntax is

```
        ArrayFunc←(LfuncVector FA_catenate RfuncVector)dummyArray
```

the result is a new vector-of-functions (class 3) obtained by means of two vector-of-functions (class 3).

```
∇
 ArrayFunc←(Lfunc FA_catenate Rfunc)dummyArray;Lns;Rns
   ⍝ catenate 2 vectors of functions: primitive "," is promoted to function level-GQR 20040204
   ⍝ Lfunc is a function(0 rank vector of functions) or a vector of functions;same for Rfunc
   ⍝ ArrayFunc is an Array(vector) of Functions whose names are Lfunc and Rfunc
     ⍝ try:
     ⍝   FA←(+ FA_catenate -)0       ⍝FA is a vector of functions : FA 3 ←→ 3 ¯3
     ⍝   FA←({2×⍵} FA_catenate -)0   ⍝FA is a vector of functions : FA 3 ←→ 6 ¯3
     ⍝   FB←(× FA_catenate FA)0      ⍝FB is a vector of functions : FB 3 ←→ 1 6 3
 Lns←(''FAtoNS Lfunc)⍬  ⍝the left  function_array becomes a namespace_array;⎕NC'Lns'←→2
 Rns←(''FAtoNS Rfunc)⍬  ⍝the right function_array becomes a namespace_array;⎕NC'Rns'←→2
 ArrayFunc←(Lns,Rns).f
 ⍝  ⎕NC'Lfunc'←→3   ⎕NC'Rfunc'←→3   ⎕NC'ArrayFunc'←→3
∇
```

## Some Examples

```
     ⎕←(''FA_shape +)''            ⍝the shape of a primitive function is ⍬

     ⎕←(''FA_shape {,⍵})''         ⍝the shape of a function is ⍬

     FA←(+ FA_catenate -)0         ⍝FA is a vector of functions : FA 3 ←→ 3 ¯3

     FA←({2×⍵} FA_catenate -)0     ⍝FA is a vector of functions : FA 3 ←→ 6 ¯3
     ⎕←(''FA_shape FA)''
2
     FA2←(2 4 FA_reshape FA)0      ⍝FA2 is a (2×4) array-of-functions
     ⎕←(''FA_shape FA2)''
2 4

     FB←(× FA_catenate FA)0           ⍝FB is a vector of functions : FB 3 ←→ 1 6 3
     FB2←(2 6 FA_reshape FB)0         ⍝FA2 is a (2×4) array-of-functions
     FB3←((1 2) (2 3 4) FA_from FB2)0 ⍝FB3 is an array-of-functions
```

## References

\[1\] G.D.Robertson, *New Foundations*, Vector 20.1(2003) 132-142  
\[2\] Dyalog APL/W version 10.0 Language Reference(2003)
