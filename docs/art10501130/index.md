---
vid: '10501130'
title: Semantic arrays
authors:
- Stephen Taylor
byline: Stephen Taylor (sjt@5jt.com)
volume: '26'
issue: '1'
page: '46'
received: '2012-12-29'
online: '2013-09-24'
description: Taylor presents APL functions to support ‘semantic indexing’. These functions are modelled on features of the K language
abstract: APL functions are presented to support ‘semantic indexing’. These functions are modelled on features of the K language.
source: content/printed/261/semanticarrays/semanticarrays.htm
converter: vecrescue 0.1.0
issue_tab: n1-september-2013
tags:
- programming techniques
- language design
---

Stephen Taylor (sjt@5jt.com)
{ .byline }

APL functions are presented to support ‘semantic indexing’. These functions are modelled on features of the K language.
{ .abstract }

An array is a map between its indexes and its values. The indexes of APL arrays are integers and represent position in the array. Where the elements are homegenous, this is congenial. But where elements represent different kinds of things, it is rarely helpful to indicate them by position.

Indeed, *Cannon’s Canon* \[[1](#ref1)\] deprecates using numerical constants to indicate anything but numbers. For example, while the 3 in `+3` indicates nothing but a number by which something is to be increased, `+customer[3]` is deprecated if it requires one to remember that the third element of `customer` indicates age. If one means *age* then one wants a way to say so.

Call the mapping of numbers to values *ordinal* mapping and the mapping of character strings to values *semantic* mapping.

The workspace’s global symbol table provides semantic mapping but, by definition, there can be only one global symbol table – and we frequently need multiple maps. Dyalog’s *namespace* provides multiple semantic mappings very nicely through ‘local’ symbol tables, eg

```
      customer.age←38
```

JavaScript and PHP provide semantic mapping through *objects*, collections of name/value pairs.

```
customer = {name: Jane Doe, age:38}
```

JavaScript also supports character strings as array indexes, allowing eg

```
customer['age'] = 38
```

The [K language](http://kparc.com) provides semantic mapping through *symbols*, which behave much like character strings, eg

```
 customer: `name`age!("Jane Doe";38)
 customer[`age]
38
 customer@`age`name
(38;"Jane Doe")
```

Without the use of Dyalog namespaces, I sought convenient representations using more generic APL. I was inspired particularly by K’s index function `@`. Emulating this in APL neatly avoided any need for special syntax. This article presents what I devised, much as it is used in the application on which I work.

## Dictionaries and tables

Call a semantic array of rank 1 a *dictionary*. It is a pairing of names and values. Represent it in APL by a vector of the values, prefixed by the enclosed vector of their names. Thus, `↑dict` has the same length as `1↓dict`. \[[2](#ref2)\]

```
      ]display French ← (⊂'cow' 'dog' 'cat') , 'vache' 'chien' 'chat'
.→------------------------------------.
∣.→--------------..→----..→----..→---.∣
∣∣.→--..→--..→--.∣∣vache∣∣chien∣∣chat∣∣
∣∣∣cow∣∣dog∣∣cat∣∣'-----''-----''----'∣
∣∣'---''---''---'∣                    ∣
∣'∊--------------'                    ∣
'∊------------------------------------'
```

Call a semantic array of rank 2 a *table*. It is a pairing of names and vectors. All the vectors have the same length: the depth of (number of rows in) the table. Represent it in APL by a nested matrix. The first row of the matrix contains the column names.

```
      ∆←('cow' 'vache' 'Kuh')('dog' 'chien' 'Hund')('cat' 'chat' 'Katze')
      ⎕←tbl←'English' 'French' 'German' ⍪ ⊃∆
 English French German
 cow     vache  Kuh
 dog     chien  Hund
 cat     chat   Katze
```

Happily, the default display of a matrix is exactly what one would want.

Clearly, the matrix can be seen as a special case of the dictionary, in which all the values are vectors of the same length. It is trivial to switch between forms:

```
      ]display dict ← (⊂tbl[1;]), ⎕SPLIT[1] 1 0↓ tbl
.→--------------------------------------------------------------
∣.→------------------------..→--------------..→-----------------
∣∣.→------..→-----..→-----.∣∣.→--..→--..→--.∣∣.→----..→----..→--
∣∣∣English∣∣French∣∣German∣∣∣∣cow∣∣dog∣∣cat∣∣∣∣vache∣∣chien∣∣cha …
∣∣'-------''------''------'∣∣'---''---''---'∣∣'-----''-----''---
∣'∊------------------------''∊--------------''∊-----------------
'∊--------------------------------------------------------------
```

## The `at` function

Tables and dictionaries are in the left domain of the `at` function. The right argument is either a single key:

```
      French at 'dog'
chien
      German at 'dog'
Hund
      tbl at 'French'
 vache chien chat
```

or an array of keys:

```
      French at 'dog' 'cow'
 chien vache
      German at 'dog' 'cow'
 Hund Kuh
      tbl at 'English' 'German'
  cow dog cat   Kuh Hund Katze
      dict at 'English' 'German'
  cow dog cat   Kuh Hund Katze
```

In our application we have large parameter sets known as *bases*. Their definitions vary slightly from one version of the application to the next, so we need to manage that variation during the upgrade.

Suppose we have a table `basestable` that contains columns `V8` and `V9`. Both columns contain bases, represented as dictionaries. The vector `parameters` lists names defined in the dictionaries. We can tabulate parameter values from the bases:

```
      (basestable at 'V8') ∘.at parameters
      (basestable at 'V9') ∘.at parameters
```

`at` can also take an array of names as its right argument, permitting, for example:

```
      basis←(⊂'MAGEDIF' 'MAGEDIFT' 'FAGEDIF' 'FAGEDIFT') , '5' '' '' 'diffs.csv'
      ]display basis at 'MF' ∘., 'AGEDIF' 'AGEDIFT'
.→-------------.
↓   .⊖.        ∣
∣ 5 ∣ ∣        ∣
∣ - '-'        ∣
∣.⊖..→--------.∣
∣∣ ∣∣diffs.csv∣∣
∣'-''---------'∣
'∊-------------'
```

## The functions `pop` and `push`

The functions `pop` and `push` are syntactic sugar for constructing and parsing dictionaries. For example:

```
      ]display French ← 'cow' 'dog' 'cat' push 'vache' 'chien' 'chat'
.→------------------------------------.
∣.→--------------..→----..→----..→---.∣
∣∣.→--..→--..→--.∣∣vache∣∣chien∣∣chat∣∣
∣∣∣cow∣∣dog∣∣cat∣∣'-----''-----''----'∣
∣∣'---''---''---'∣                    ∣
∣'∊--------------'                    ∣
'∊------------------------------------'
      (keys vals)←pop French
      keys
 cow dog cat
      vals
 vache chien chat
```

From this we can easily display a dictionary:

```
      ↑,[1.5]/pop french
 cow    vache
 sheep  mouton
 cat    chat
```

The `push` function is overloaded: it can be used dyadically as above or monadically on an argument with 2 elements. This makes monadic `push` and `pop` – when applied to a dictionary – inverses of each other, so that `French ≡ push pop French`.

The dictionary can thus be flipped, making its values keys and its keys values:

```
      French at 'dog'
chien
      (push⌽pop French) at 'chien'
dog
```

We can also make a French-German dictionary from the polyglot word table:

```
      (push tbl at 'French' 'German') at 'chien'
Hund
```

`push` and `pop` also give us convenient ways to turn a table into a vector of dictionaries:

```
      (hdr cols) ← pop ⎕SPLIT tbl
      dics ← (⊂hdr) push¨ cols
      (↑dics) at 'English' 'French'
 cow vache
```

## The `map` function

We often want to look up values in one column of a table and read the corresponding values from another column. To find the German forms of some French words:

```
      (push tbl at 'French' 'German') at 'chat' 'vache'
 Katze Kuh
```

The function `map` provides a little syntactic sugar for this:

```
      (tbl at 'French' 'German') map 'chat' 'vache'
 Katze Kuh
```

## The `spin` function

Converting rows to or from columns is helped by the `spin` function, which is its own inverse:

```
      ]display tbl at 'French' 'German'
.→-----------------------------------------.
∣.→-------------------..→-----------------.∣
∣∣.→----..→----..→---.∣∣.→--..→---..→----.∣∣
∣∣∣vache∣∣chien∣∣chat∣∣∣∣Kuh∣∣Hund∣∣Katze∣∣∣
∣∣'-----''-----''----'∣∣'---''----''-----'∣∣
∣'∊-------------------''∊-----------------'∣
'∊-----------------------------------------'
      ]display spin tbl at 'French' 'German'
.→-------------------------------------------.
∣.→-----------..→------------..→------------.∣
∣∣.→----..→--.∣∣.→----..→---.∣∣.→---..→----.∣∣
∣∣∣vache∣∣Kuh∣∣∣∣chien∣∣Hund∣∣∣∣chat∣∣Katze∣∣∣
∣∣'-----''---'∣∣'-----''----'∣∣'----''-----'∣∣
∣'∊-----------''∊------------''∊------------'∣
'∊-------------------------------------------'
      ∆ ≡ spin spin ∆ ← tbl at 'French' 'German'
1
```

Thus to loop through the rows of a table:

```
      :for en fr de :in spin tbl at 'English' 'French' 'German'
         ⎕←'English: ',en,'; French: ',fr,'; German: ',de
      :endfor
```

The above\[[3](#ref3)\] becomes awkward where many columns require many local ‘loop variables’. Instead one could loop through table rows as dictionaries:

```
      :for word :in ∆[1] push¨ 1↓∆←⎕SPLIT tbl
         ⎕←'English: ',word at 'English'
      :endfor
```

## Selecting from tables

To return a table containing only selected columns, the function `select`.

To return a table with selected rows, the `for` function. Its right argument is either a boolean vector whose length is the table depth; or a vector of which the first element is a column name and subsequent elements are values to be matched.

```
      tbl for 'English' 'cow' 'cat'
 English French German
 cow     vache  Kuh
 cat     chat   Katze
      tbl for 'c' = ↑¨tbl at 'French'
 English French German
 dog     chien  Hund
 cat     chat   Katze
      (tbl for 'English' 'cow' 'cat') select 'French' 'German'
 French German
 vache  Kuh
 chat   Katze
```

## The `amend` function

To add a new element, or replace an element of a table or dictionary with a new value, the `amend` function.

```
      (French amend 'cow' 'la vache') at 'cow'
la vache
      tbl amend 'Danish' ('kuh' 'hund' 'katte')
 English French German Danish
 cow     vache  Kuh    kuh
 dog     chien  Hund   hund
 cat     chat   Katze  katte
      tbl amend 'French' ('la vache' 'le chien' 'le chat')
 English German French
 cow     Kuh    la vache
 dog     Hund   le chien
 cat     Katze  le chat
```

## Conclusion

These ‘syntactic sugar’ functions have been invaluable in simplifying the representation of logic in our application, and keeping our code compliant with Cannon’s Canon.

## Notes

1. <a id="ref1"></a> *Cannon’s Canon* is so dubbed by me because it was introduced to me by Ray Cannon.

2. <a id="ref2"></a> The monadic function represented by `↑` varies between APL dialects and between ‘migration levels’ in at least one interpreter. `↑` here represents the *first* function, that returns the first element of its argument. The monadic function *mix* is here represented by `⊃`. The *cut* function that splits an array along its first axis (eg a table into rows), in some interpreters represented by the `↓` glyph, is here represented as `⎕SPLIT`.

3. <a id="ref3"></a> For interpreters in which the `:for` loop as not been so extended:

    ```
    :for ∆ :in spin tbl at 'English' 'French' 'German' ◊ (en fr de)←∆
    ```

## Appendix – Function definitions

```
    ∇ Z←L amend R;newhds;newcols;seln;hds
[1]   ⍝ sets one or more elements of semantic array L
[2]   ⍝ defined for table and dictionary L
[3]
[4]   ⍝ parse R
[5]   :if 2=↑⍴R
[6]   :andif (≡↑R)∊0 1
[7]       (newhds newcols)←,¨⊂¨R   ⍝ tbl amend 'col4' foo
[8]   :elseif ^/(≡¨↑¨R)∊0 1
[9]   :andif (↑¨⍴¨R)^.=2
[10]      (newhds newcols)←spin R  ⍝ tbl amend('col4' foo)('col5' bar)
[11]  :endif
[12]
[13]  :select ↑⍴⍴L
[14]  :case 1 ⋄ hds←↑L                                    ⍝ dict
[15]  :case 2 ⋄ hds←L[⎕IO;]                               ⍝ matrix
[16]  :endselect
[17]
[18]  seln←~hds∊newhds
[19]
[20]  :select ↑⍴⍴L
[21]  :case 1 ⋄ Z←push(seln/¨pop L),¨newhds newcols       ⍝ dict
[22]  :case 2 ⋄ Z←(seln/L), newhds⍪ ⍉⊃newcols             ⍝ matrix
[23]  :endselect
[24]
    ∇
```

```
    ∇ Z←L at cols;hdr;vals;DEFAULT
[1]   ⍝ select cols from table, represented either as
[2]   ⍝ - matrix with header row
[3]   ⍝ - dictionary: keys val val val...
[4]   DEFAULT←'' ⍝ for undefined values (where allowed)
[5]   :if 2=⍴⍴L ⍝ table: error if cols not found
[6]       (hdr vals)←(L[⎕IO;])(⎕SPLIT[⎕IO]1 0↓L)
[7]   :else ⍝ dictionary
[8]       (hdr vals)←pop L
[9]       vals←vals,⊂DEFAULT ⍝ default value if cols not found
[10]  :endif
[11]  :if 1=≡cols
[12]      Z←(hdr⍳⊂cols)⊃vals
[13]  :else
[14]      Z←vals[hdr⍳cols]
[15]  :endif
    ∇
```

```
    ∇ Z←tbl for cv;col;vals;msk;hdr;bdy
[1]   ⍝ table/dictionary syntax: tbl for 'col1' val1 val2 ...
[2]   ⍝ returns a table or dictionary according to tbl
[3]   ⍝ or cv may be a boolean mask
[4]   :if 1=≡cv ⋄ :andif 1=↑⍴⍴cv ⋄ :andif ^/cv∊0 1
[5]       msk←cv
[6]   :else
[7]       (col vals)←pop cv
[8]       msk←(tbl at col)∊vals
[9]   :endif
[10]
[11]  :select ↑⍴⍴tbl
[12]  :case 1 ⍝ table
[13]      (hdr bdy)←pop tbl
[14]      Z←hdr push msk/¨bdy
[15]  :case 2 ⍝ dictionary
[16]      Z←(1,msk)⌿tbl
[17]  :endselect
    ∇
```

```
    ∇ Z←L map R;to;from
[1]   (from to)←L
[2]   Z←to[from⍳R]
    ∇
```

```
    ∇ Z←pop R
[1]   Z←(↑R)(1↓R)
    ∇
```

```
    ∇ Z←L push R;A;B
[1]   ⍝ syntax sugar:
[2]   ⍝ 'abc' 'def' 'ghi'
[3]   ⍝ ←→ 'abc' push 'def' 'ghi'
[4]   ⍝ ←→ push ('abc') ('def' 'ghi')
[5]   :if 2=⎕NC 'L'
[6]       Z←(⊂L),R
[7]   :else
[8]       (A B)←R
[9]       Z←(⊂A),B
[10]  :endif
    ∇
```

```
    ∇ tbl←tbl select cols
[1]   ⍝ select cols from tbl
[2]   tbl←(tbl[⎕IO;]∊cols)/tbl
    ∇
```

```
    ∇ Z←spin R
[1]   Z←⎕SPLIT⍉⊃R
    ∇
```
