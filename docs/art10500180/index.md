---
vid: '10500180'
title: Array languages for Lisp programmers
authors:
- Slobodan Blazeski
byline: by Slobodan Blazeski (slobodan.blazeski@gmail.com)
volume: '24'
issue: '2'
page: '26'
description: How studying the APLs will help you write better Lisp programs
source: content/printed/242/blazeski.htm
converter: vecrescue 0.1.0
issue_tab: n23-august-2010
tags:
- APL in perspective
- language design
---

by Slobodan Blazeski (slobodan.blazeski@gmail.com)
{ .byline }

A language that doesn’t affect the way you think about programming is not worth knowing. [\[1\]](#ref1)

You can post comments on this article at the [*Vector* blog](http://vectoreditor.blogspot.com/2009/11/j-and-q-for-lispers.html). *Ed.*
{ .ednote }

During my pilgrimage into the land of programming languages I’ve stumbled upon quite a lot of them. Most languages are just poorly-integrated duct-tape-and-wire rehashes of principles discovered by others. Few of them are true paradigm-shifting jewels. APL and its offspring are definitely among those. But what’s their value for spoiled Lisper brats? What’s so great about them that Lisp doesn’t already have? During my whirlwind presentation of examples I will try to answer this question.

The first thing that comes to mind is that array-languages such as APL, J and q operate on whole arrays instead of munching elements one by one.

By using array functions you can work on whole containers the same way as you work on scalars. It takes only a little tinkering with <acronym title="Common Lisp Object System">CLOS</acronym>[\[2\]](#ref2) dispatch to make a set of operators that work roughly the same. (Adding ranks will take more time than I currently have.) [\[3\]](#ref3)

<table class="left" summary="Comparison of J and Lisp expressions">
      <col span="2" style="width: 20em">
      <thead>
        <tr><th class="center">J</th><th class="center">Common Lisp</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>
<pre>
   2 + 2 3 4
4 5 6
</pre>
          </td>
          <td>
<pre>
   (j+ 2 #(2 3 4))
4 5 6
</pre>
          </td>
        </tr>
        <tr>
          <td>
<pre>
   2 3 4 + 1 2 3
3 5 7
</pre>
          </td>
          <td>
<pre>
   (j+ #(2 3 4) #(1 2 3))
3 5 7
</pre>
          </td>
        </tr>
      </tbody>
    </table>

Then having a set of operators for making arrays is very handy, so I wrote those quickly in Lisp.

<table class="left">
      <col span="2" style="width: 20em">
      <thead>
        <tr><th class="center">J</th><th class="center">Common Lisp</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>
<pre>
   2 3 $ 'abcde'
abc
dea
</pre>
          </td>
          <td>
<pre>
   (shape '(2 3) "abcde")
abc
dea
</pre>
          </td>
        </tr>
        <tr>
          <td>
<pre>
   i. 2 3
0 1 2
3 4 5
</pre>
          </td>
          <td>
<pre>
   (enum 2 3)
0 1 2
3 4 5
</pre>
          </td>
        </tr>
      </tbody>
    </table>

Adverbs give great power to J, but Lisp has had higher-order functions since forever. Things like built-in `reduce` already work on vectors and it doesn’t take much work to create a version of `reduce` such as `fold` that handles multidimensional arrays.

<table class="left">
      <col span="2" style="width: 20em">
      <thead>
        <tr><th class="center">J</th><th class="center">Common Lisp</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>
<pre>
   +/ 1 2 3 4
10
</pre>
          </td>
          <td>
<pre>
   (reduce #'+ #(1 2 3 4))
10
</pre>
          </td>
        </tr>
        <tr>
          <td>
<pre>
   +/ i. 2 3
3 5 7
</pre>
          </td>
          <td>
<pre>
   (fold #'+ #(1 2 3 4))
3 5 7
</pre>
          </td>
        </tr>
      </tbody>
    </table>

And once you have them it’s too easy to write factorial of 6 in array style:

<table class="left">
      <col span="2" style="width: 20em">
      <thead>
        <tr><th class="center">J</th><th class="center">Common Lisp</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>
<pre>
   */ 1+ i. 5
120
</pre>
          </td>
          <td>
<pre>
(reduce #’+ (j+ 1(enum 5)) <em class="plain">or</em>
(fold #’j+ (enum :start 1))
</pre>
          </td>
        </tr>
      </tbody>
    </table>

One of q’s very cool features allows omitting the first three parameters of a function. That’s a feature that I immediately ported to Lisp with a little macro.

<table class="left">
      <col span="2" style="width: 20em">
      <thead>
        <tr><th class="center">q</th><th class="center">Common Lisp</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>
<pre>
{x+y+z}⇔<!--&hArr;-->{[x;y;z] x+y+z}
</pre>
          </td>
          <td>
<pre>
(f + x y z) ⇔<!--&hArr;-->
(lambda (x y z)(+ x y z))
</pre>
          </td>
        </tr>
      </tbody>
    </table>

But maybe it’s not about single features but the ways to combine them into non-trivial programs and problem solutions.

Just take a look at the Lisp code answering the task below: [\[4\]](#ref4)

> We have a list of elements, some are duplicates. We’re trying to figure out how to find the duplicate elements and increase a counter value by 1 for each instance of the element found. The list consists of lists with two elements, the first being the incremental counter, the second being the string sought. Example:
>
> ```
> ((1 "one") (1 "two") (1 "three") (1 "one") (1 "four") (1 "two"))
> ```
>
> The result should be:
>
> ```
> ((2 "one") (2 "two") (1 "three") (1 "four"))
> ```

q
:   
    ```
    count:flip(count each group v[;1];unique v[;1])
    ```

Lisp
:   
    ```
    (defun count (list)
      (let ((hash (make-hash-table)))
        (dolist (el list)
          (incf (gethash (cadr el) hash 0) (car el)))
        (let (result)
          (maphash (lambda (key val)
            (push (list val key) result))
          hash)
        result)))
    ```

The above seems an exemplary case where Lisp loses its magic. But that’s only because q has `group` built in. If I write it, or being a lazy person like myself ask c.l.l [\[5\]](#ref5) denizens to write it for me, the edge is lost.

q
:   
    ```
    brunberg:flip(count each group v[;1];unique v[;1])
    ```

Lisp
:   
    ```
    (defun brunberg (l)
      (mapcar (f list (length x) (cadar x))  (group l #'cadr #'string=)))
    ```

The q solution is still shorter but only due to a Lisp culture of very long and descriptive names. Using a golfing [\[6\]](#ref6) library with operators’ bindings pointing to shorter names will make it look even shorter.

Another fine example is the problem of parsing identifiers:

> This piece of \[Haskell\] code goes through a parse tree of Haskell source code, locates every reference to an identifier that ends with `Widget`, puts it on a list, and removes duplicates so every identifier is represented in the list only once. [\[7\]](#ref7)

Haskell
:   
    ```
    extractWidgets :: (Data a) => a -> [String]
    extractWidgets = nub . map (\(HsIdent a)->a) . listify isWidget
      where isWidget (HsIdent actionName)
          | "Widget" `isSuffixOf` actionName = True
        isWidget _ = False
    ```

q
:   
    ```
    a:distinct raze over x
    a where a like "*Widget"
    ```

Lisp
:   
    ```
    (defun widgets (l)
      (unique (keep (f like x "*Widget") (flatten l))))
    ```

This time the difference is even smaller. I have the same number of tokens with a surplus of parentheses.

It seems it’s always the same old story. As soon as Lisp integrates the needed utility or invests into a domain-specific language, the advantage of the other language is lost. The big ball of mud is like a warlock, able to steal opponents’ powers while still staying the same.  

However the features above were easy compared to the work needed to implement my beloved tacit [\[8\]](#ref8) style. While point-free style isn’t a complete stranger to the Lisp community, it remains a rarity [\[9\]](#ref9):

Lisp
:   
    ```
    (reduce  (flip #'cons) #(1 2 3 4) :initial-value '())
    (4 3 2 1)
    ```

Writing a macro that understands forks and hooks is an exercise for a beginner – if the operators covered are strictly monadic and/or dyadic as are those in J. However in Lisp all those optional auxiliary and keyword arguments immensely complicate matters. In order to make it operational within a month or so of effort a tacit macro could be implemented to work just on a subset of Lisp operators.

A library consisting of operators with integrated looping, adverbs that understand verb rank, and a macro able to operate on hooks and forks would bridge the gap in Lisp array-processing facilities. But it’s not enough to bridge the gap between technology and need. Somebody needs to make humans want to cross that bridge or else it will follow the fate of Richard C. Waters’ series [\[10\].](#ref10)   

So why should Lispers study array programming languages?

Learning anything of APL, J or q makes programmers aware of opportunities opened by thinking at the higher level of container abstraction. This knowledge pays off well in Lisp, with or without the array sublanguage. Keep in mind too that the difference between an array sublanguage and an array language is like the difference between learning a foreign language at home or among its native speakers. You *can* learn just the language while remaining clueless about its culture!

loop
:   
    ```
    (loop for i from 0 to 10
      collect ( loop for j from 0 to 10
        collect (+ i j)))
    ```

functional
:   
    ```
    (flatten (outer #'+ (range 10) (range 10)))
    ```

loop
:   
    ```
    (defun separate (list)
      (loop for element in list
        if (plusp element)
        collect element into positives
        else if (minusp element)
        collect (abs element) into minuses
        else collect element into zeroes
        finally (return (list minuses zeroes positives))))
    ```

functional
:   
    ```
    (defun separate (lst)
      (mapcar  (f mapcar #'abs (remove-if-not x lst)))
        (list #'plusp #'zerop #'minusp)))
    ```

## Notes and references

1. <a id="ref1"></a> Alan J. Perlis, *Epigrams in Programming*
2. <a id="ref2"></a> [http://en.wikipedia.org/wiki/Common\_Lisp\_Object\_System](http://en.wikipedia.org/wiki/Common_Lisp_Object_System)
3. <a id="ref3"></a> The Lisp return values were presented in J style for increased readability.
4. <a id="ref4"></a> http://www.nsl.com/papers/kisntlisp.htm
5. <a id="ref5"></a> comp.lang.lisp  
    [http://groups.google.com/group/comp.lang.lisp/](http://groups.google.com/group/comp.lang.lisp/)
6. <a id="ref6"></a> Golfing:  
    [http://en.wikipedia.org/wiki/Perl#Perl\_golf](http://en.wikipedia.org/wiki/Perl#Perl_golf)  
    Some of the functional solutions were developed in the c.l.l. golfing sessions with feedback from many c.l.l. denizens.
7. <a id="ref7"></a> [http://www.defmacro.org/ramblings/haskell-productivity.html](http://www.defmacro.org/ramblings/haskell-productivity.html)
8. <a id="ref8"></a> [http://en.wikipedia.org/wiki/Tacit\_programming](http://en.wikipedia.org/wiki/Tacit_programming)
9. <a id="ref9"></a> Vassil Nikolov – “Could loop loop backward across vector?”
10. <a id="ref10"></a> [http://series.sourceforge.net/](http://series.sourceforge.net/)
11. <a id="ref11"></a> The first three solutions in the last block were written by Kenny Tilton, Matt Pillsbury and Rob St Amant respectively. Though Tilton’s and Amant’s solutions are far longer they’re also more efficient.
