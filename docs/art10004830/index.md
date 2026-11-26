---
vid: '10004830'
title: 'The Puzzle Corner: the vier-neun puzzle'
authors:
- Stefano Lanzavecchia
byline: by Stefano Lanzavecchia (stf@apl.it) and Jonathan Manktelow (jonathan@causeway.co.uk)
volume: '17'
issue: '4'
page: '38'
source: trad/v174/puzzl174.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-2001
tags:
- competitions and puzzles
- programming techniques
---

*by Stefano Lanzavecchia ([stf@apl.it](mailto:stf@apl.it)) and Jonathan Manktelow ([jonathan@causeway.co.uk](mailto:jonathan@causeway.co.uk))*
{ .byline }

## Introduction

Learning by playing, something kids do all the time and adults often dismiss as an inefficient way of tackling new challenges. I would love to have material for a puzzle corner to be run regularly, because I truly believe puzzles are a great way to enjoy oneself while exploring and learning. Instead of rewriting the Centigrade to Fahrenheit converter for the hundredth time, when trying a new feature of a known language or a new language altogether, why not take a simple puzzle and solve it in the new environment? The blinking cursor beside the correct solution is quite a rewarding sight and the techniques so learned are then ready in our brains to be applied to more serious matters.

For this issue I’ll present a new puzzle that recently swept onto the UUNet newsgroups and an article with a new solution to the Funny Cube of Mr Legrand. While the language used for the latter isn’t exactly an Array Programming Language, the author, who is also an APLer, raises a few interesting points.

## The Vier-Neun Puzzle

On March the first, Steve Graham posted the following message in a few discussion groups related to programming languages:

> This puzzle was originally posted on a mailing list for the Icon programming language. Thought members of this group might also want to give it a shot.

> VIER and NEUN represent 4-digit squares, each letter denoting a distinct digit. You are asked to find the value of each, given the further requirement that each uniquely determines the other.

> The “further requirement” means that of the numerous pairs of answers, choose the one in which each number only appears once in all of the pairs.

Before you read more I suggest you spend a minute figuring out a possible solution strategy, even if you don’t have the time to write the code: it will make what follows clearer.

First of all it’s easy to see that a brute force approach is viable since the pairs of perfect squares to be examined are less than 5000.

At the time of my writing this summary, on the web page where Mr Graham is collecting the solutions to the problem \[[members.home.net/js.graham/vierneun.html](http://members.home.net/js.graham/vierneun.html)\], almost 40 entries are listed, in a wide range of languages (APL, Dylan, Forth, Haskell, Icon, J, K, LISP, MUMPS, OCaml, Perl, Prolog, Python, Smalltalk, SNOBOL4, SPITBOL, Tcl) and I am aware of other unpublished solutions, like at least one in A+.

It would not be easy for me to review all the solutions, because I don’t understand all the languages that were used, nor would it be appropriate since Vector is devoted in particular to APL and friends. But I still think it’s interesting to spend some time looking at different worlds because there’s always a new trick to be learned.

Let’s start with APL. The first APL solution to appear was provided by Morten Kromberg, who has since revised his solution multiple times, thanks to an inspiring discussion with Roger Hui, until it reached this final version:

```
    r←NS2;m
[1] ⍝ New Scientist problem, Dyalog APL with Rogers improvements
[2]
[3] ⍝ 4624x8-column char matrix NEUNVIER
[4] r←↑,r∘.,r←↓4 0⍕(31+⍳68)∘.*,2
[5] ⍝ Must have same pattern as NEUNVIER or 12315628
[6] r←(({⍵⍳⍵}¨↓r)∊⊂{⍵⍳⍵}'neunvier')⌿r
[7] ⍝ Unique entries in each group of 4 columns
[8] r←(⊃^/{(w⍳w)=(1+⍴w)-(⌽w)⍳w←↓⍵}¨(8⍴4↑1)⊂r)⌿r
```

Next comes Roger Hui’s submission, at the same time elegant and efficient. Let’s read his post, where the strategy is clearly explained step by step:

> The 4-digit squares are (`>.%:1000`) squared to (`<.%:9999`) squared, or, as a character matrix,

```
   s=: 4":,.*:32+i.68
   4{.s
1024
1089
1156
1225
   _4{.s
9216
9409
9604
9801
```

> To form all pairs of such squares, catenate each row to all other rows, and make into a matrix, thus:

```
   p=: ,/ ,"1"1 _ ~ s
   $p
4624 8
   4{.p
10241024
10241089
10241156
10241225
   _4{.p
98019216
98019409
98019604
98019801
```

> The pattern of digit assignments must be the same as the pattern of letters in 'vierneun', and so:

```
   a=: ((i.~'vierneun')-:"1 i.~"1 p)#p
   $a
9 8
   a
13694624
13695625
17645625
43561521
47615625
62419409
70561521
75691681
75694624
```

> Finally, choose numbers that are unique in all the pairs. An item is unique if its index of first occurrence is the same as its index of last occurrence. Thus:

```
   ((4{."1 a} *.&(i.~ = i:~) (4)."1 a))#a
62419409
```

> Collecting together the essential lines:

```
   s=: 4":,.*:32+i.68
   p=: ,/ ,"1"1 _ ~ s
   a=: ((i.~'vierneun')-:"1 i.~"1 p)#p
   ((4{."1 a} *.&(i.~ = i:~) (4)."1 a))#a
62419409
```

> Forming all pairs is linear in the size of the result; all other operations are linear in the size of the argument(s).

It was easy for me to translate Roger’s idea in rough K:

```
   s:4$(32+!68)^2
   p:,/s,\:/:s
   a: p @ & ({x?/:x}"vierneun")~/:{x?/:x}'p
   k:{*:'x@&1=#:'x:=x}'+{(4#x;4_ x)}'a
   a k[0] @ &k[0] _in\: k[1]
,"62419409"
```

David Ness quickly provided a generalization to try other pairs of German numerals that have four letters, thanks to the fact that the condition which asks for each letter in the words “vier” and “neun” to be assigned to one digit is so cleanly expressed. In K:

<pre>   skp: z,'z:("eins";"zwei";"drei";"vier";"funf";"acht";"neun")
   num: num @ &amp;(#z)=skp ?/: num:,/z ,\:/:z


   <b>dj</b>:{[arg]  `0:"\n",arg,"\n"
       a: p @ &amp; ({x?/:x}arg)~/:{x?/:x}'p:,/s,\:/:s:4$(32+!68)^2
       if[0=#k:{*:'x@&amp;1=#:'x:=x}'+{(4#x;4_ x)}'a;:(arg;-1)]
       `0:t:"\t",'a k[0] @ &amp;k[0] _in\: k[1]
       (arg;#t)}</pre>

Typing

```
   dj'num
```

will result, among other things, in a list:

```
 (("zweieins";2)
 ("dreieins";2)
 ("viereins";6)
 ("funfeins";2)
 ("achteins";0)
 ("neuneins";-1)
...
 ("zweineun";1)
 ("dreineun";1)
 ("vierneun";1)
 ("funfneun";-1)
 ("achtneun";1))
```

which indicates each “problem” and the number of solutions found for that particular problem. The difference between ‘0’ and ‘-1’ is that 0 represents places where there are solutions but none of them satisfy the ‘single/single’ restriction. ‘¦1’ occurs when there are no solutions even relaxing this assumption.

John R. Clark provided a more traditional APL solution:

> Since we know they are 4 digit squares we can generate all possible values and start to do an elimination.

> Let V be the universe and A be the possible values for NEUN

```
   A←((V[;2]≠V[;3])^(V[;1]=V[;4]))⌿V←⍕((⍴V),1)⍴V←(33↓⍳99)*2
```

By removing duplicates from V we can obtain possible VIER values

```
   V←(NODUPS¨⊂[2]V)⌿V
```

If we cut out the values of V that fail we have a solution

```
   A,CUT¨⊂[2]A
1521 4356
         7056
1681 7569
4624 1369
         7569
5625 1369
         1764
         4761
9409 6241
```

<pre>      <b>Z←NODUPS X
</b>[1]   Z←1=⌈/+/X∘.=X</pre>

<pre>      <b>Z←CUT X
</b>[1]   Z←((V[;1]≠X[1])^(V[;2]≠X[2])^(V[;3]=X[2])^(V[;4]≠X[4]))⌿V
[2]   Z←(1+/+/Z∘.X)⌿Z</pre>

Koji Kawakami has submitted this function, to be run on Sharp APL. Please notice line \[6\] which is the direct equivalent of Roger’s assignment to `p`.

<pre>      <b>r←spuzzle</b>;t
[1] ⍝ all possible 4 digit combinations
[2]   r←'2i4' ⎕fmt (4624 2)⍴r(∘.(,{rank}0)r←(31+⍳68)*2
[3] ⍝ a rough sieve
[4]   r←((r[;3]=r[;6])^r[;5]=r[;8])⌿r
[5] ⍝ pattern matching
[6]   r←('vierneun'∊{on}≠{rank}1 r)[;1]⌿r
[7] ⍝ unique pair only
[8]   r←((∨⌿(1=+/t)⌿t←=r[;1 2 3 4])^∨⌿(1=+/t)⌿t←=r[;5 6 7 8])⌿r
[9]   r←1 1 1 1 0 1 1 1 1\r</pre>

Just for fun, I tried to craft a one-liner in the functional style John Scholes from Dyadic uses to build his exemplar dynamic functions. More an exercise in style than in code elegance or efficiency, here it is:

<pre><b>vierneun←{</b> ⍝ Solve the generalised New Scientist's Vier-Neun problem
     { (4↑[2]⍵){ ⍝ enforce uniqueness
         (⊃^/{(⍵⍳⍵)=1+(⍴⍵)-(⌽⍵)⍳⍵}∘↓¨⍺ ⍵)/[1]⍺,⍵
       }¯4↑[2]⍵
     }{ ⍝ remove the debris
         ↑(ל∘⍴¨⍵)/⍵
     }(⍺,⍵){ ⍝ test the combination for the proper pattern
         ,⍵∘.(({⍵⍳⍵}⍺){(⍺⍺≡{⍵⍳⍵}⍺,⍵)/⍺,⍵})⍵
     }{ ⍝ generate all the squares and convert them to text
         ↓4 0⍕,[⍬](32+⍳68)*2+⎕IO←0
     }⎕ML ⎕IO←1
 <b>}</b></pre>

The left and right arguments are the two patterns, in our case ‘vier’ and ‘neun’. The solution is again pretty much a rephrasing of Roger’s J solution. It is more easily understood if read from bottom to top.

We can readily calculate the solutions to the generalised problem (forgive the missing umlaut on “funf” which allows for more solutions to be generated):

```
   gn←'eins' 'zwei' 'drei' 'vier' 'funf' 'acht' 'neun'
   all←gn ∘.vierneun gn
```

Then we can count them:

```
   ⊃¨⍴¨all
36  2  2  6 2  0 0
 2 36 10  3 1  0 1
 2 10 36  2 1  0 1
 6 10  2 36 1  0 1
 2  1  1  1 5  1 0
 0  0  0  0 1 36 1
 0  1  1  1 0  0 5
```

... verify that the solution to the “vier-neun” case is the one expected:

```
   all[⊂gn⍳'vier' 'neun']
 62419409
```

... and finally extract all the pairs for which the problem has only one matching pair of squares:

```
   ↑[0.5](,1=⊃∘⍴¨all)∘/¨(,all)(,gn ∘., gn)
 37219409  zweifunf
 62419409  zweineun
 37219409  dreifunf
 62419409  dreineun
 [...]
 62419409  neunvier
 94093721  neunacht
```

For those of you who can never get enough, here’s the table built from the French numerals:

```
   fn←'zero' 'deux' 'cinq' 'sept' 'huit' 'neuf'
   ⊃∘⍴¨allf←fn ∘.vierneun fn
36  2  0  2  0  2
 2 36  0  2  5  8
 0  0 36  0  5  6
 2  2  0 36  0  2
 0  5  5  0 36  5
 2  8  6  2  5 36
```

... and the Italian ones:

```
   ⊃∘⍴¨alli←in ∘.vierneun in←'zero' 'otto' 'nove'
36 0  4
 0 0  0
 4 0 36
```

I will spare you the Danish, the Swedish and the Japanese, but before we give up, let’s have a look at solutions in other (programming!) languages.

First, the cryptic Perl solution due to Bruce Hoult for which I have no comments:

```
for $a(32..99){b:for $b(32..99){
    @cnt=();
    for(1..8){$cnt[substr($a*$a.$b*$b,$_-1,1)].=$_}
    for(0..9){next b if(sort{$b<=>$a}@cnt)[$_]!=(58,36,7,4,2,1)[$_]}
    $a{$a}++;$b{$b}++;$p{$a}=$b
}}
while(($a,$b)=each%p){print$a*$a," ",$b*$b,"\n"if$a{$a}*$b{$b}==1}
```

Then the revealing Smalltalk solution, courtesy of Reinout Heeck:

```
| squares neuns pairs tallies results |

squares := (1000 sqrt ceiling to: 9999 sqrt truncated)
               collect: [ :n | n squared printString ].
neuns := squares select: [ :string |
               string first == string last
                   and: [string asSet size==3]].
pairs := OrderedCollection new.
tallies := Bag new.
squares do: [ :square |
    neuns do: [ :neun |
        ((square at: 3 )==(neun at: 2)
            and: [(square,neun) asSet size = 6])
                ifTrue: [
                    pairs add: square -> neun.
                    tallies add: square; add: neun ]]].
results := pairs select: [ :pair |
                (tallies occurrencesOf: pair key) == 1
                    and: [(tallies occurrencesOf: pair value) == 1]]
```

The code yields:

```
results =
 OrderedCollection ('6241'->'9409')
```

This solution has many features in common with the Dylan and the Python solutions: they make heavy use of standard library collection objects. While in APL and J (and to a certain extent K) there is only the *array* (with the powerful *dyadic iota*, *shape*, *reduction primitives*), Smalltalk’s (as well as Dylan’s or Python’s) library provides the developer with objects such as the *Set* (an unordered collection of unique objects), or the key-value *pair*. The authors decided to make use of these features and, at the same time, hard-code the pattern “vier-neun” (the first digit of “neun” must match the last, “vierneun” has only 6 unique letters and so on) in the search code and not to extract the signature of the pattern and apply it in a more general way. Anyway, with the help of the library objects, many submissions while not as compact as the APL, J or K entries, are still very compact even for languages traditionally considered verbose.

Solutions where matching against a general pattern is tried can be found on the reference web page in the Icon ,LISP and, if I interpret the source code correctly, in the SNOBOL section. I wish I could read the solutions in Forth.

One thing that many authors did not try was to provide a completely coded solution, that is a solution that took into account the further requirement of uniqueness of both elements of the pair. After having generated the possible pairs matching the pattern, they left the last task of extracting the items uniquely identifying the pairs to the human.

Another interesting point is that the large majority of the languages listed earlier are interpreters with an interactive environment in which the solutions can be built step by step by trial-and-error or scripting languages that don’t need compilation.

Finally, from Oleg Kobchenko, a little pearl “to restore the decency to *conventional languages*” (his words). A direct translation in C of Roger’s algorithm, which shows “how a language like J can serve as a great prototyping tool”.

```
#include <stdio.h>
#include <string.h>

void main(void)
{  int i, j, k, n=0; wchar_t p[1000], v[]=L"vierneun", s[9];

  for (i=32;i<=99;i++) for (j=32;j<=99;j++) {
    swprintf(s,L"%04d%04d",i*i,j*j);
    for (k=0;k<8;k++) if (wcschr(s,s[k])-s != wcschr(v,v[k])-v) break;
    if (k==8) { p[n++] = i*i; p[n++] = j*j; } }
  for (i=0;i<n;i+=2) if (wcschr(p,p[i]) == wcsrchr(p,p[i])
      && wcschr(p,p[i+1]) == wcsrchr(p,p[i+1])) break;
  printf("%04d%04d\n",p[i],p[i+1]);
}
```

***

## The Funny Cube in Delphi

*\[by Jonathan Manktelow\]*

## But Delphi isn’t an Array Language!

Whilst Delphi (or to be more accurate Object Pascal) is an Object Orientated/ Procedural language it can handle arrays of arbitrary dimensions and sizes. It does lack a lot of the array manipulation primitives in APL etc., however it is a very expressive and powerful general-purpose language. It is also fast. Very fast. Both in terms of compilation and execution speed.

### So why try the funny cube in Delphi – surely APL would be better?

After seeing the funny cube puzzle in Vector Vol.16 No.3 I started to wonder if the problem was really an array manipulation problem or not. Obviously the pieces of the puzzle can be represented as arrays, but it turns out that the most intuitive approach to solving the puzzle (to me at least) is to recursively pick up a piece, put it into the next position \[A,B,C etc\] and see if it fits. If it does try the next piece, otherwise try a different piece. In other-words this is a recursive problem. Now Object Pascal (the language of Delphi) is an Object Orientated procedural language, and well suited to recursive problems. So I thought I would put together a Delphi solution to be compared to the APL ones already published in Vector. \[Please note – I did not read any of the solutions until after writing my code.\]

### Solving the puzzle

Since this is the Journal of the British APL Association, and not a Delphi magazine, I will just compare my solution to those in APL, rather than trying to explain how the code works. The source code and executable are both available on the Vector website.

**Development speed**. This took one very relaxed evening in Delphi, and would have taken me about the same in APL. This reaffirms my belief that developing programs in a specialist language such as APL is no faster than developing in a good general-purpose language.

**Execution speed**. For the simple case (no flipping allowed) the execution is 3ms. If you allow the pieces to be flipped then it takes 37ms on my 266Mhz Pentium laptop. These times are averaged over 1000 consecutive runs, as the basic Windows timer only has a resolution of 10ms. These results are nothing short of spectacular when compared with the fastest APL solutions from Vector Vol.17 No.2 which were 100ms for the non-flipping and 734ms for the flipping case on a PII 233Mhz machine.

## Conclusions

Having solved the problem in Delphi and read the various APL solutions, I am certain that the fastest way to solve the problem would be using the 6 bits of foam that made up the original puzzle (suitably labelled so that you can track which piece is which, and their rotations).

*\[Ed: Jonathan’s code is available as a 7K [text file](resource/jm174.txt).\]*

***
