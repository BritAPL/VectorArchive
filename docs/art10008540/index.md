---
vid: '10008540'
title: 'Review: APL*PLUS III Control Structures'
authors:
- Adrian Smith
byline: by Adrian Smith
volume: '11'
issue: '1'
page: '84'
source: trad/v111/smith111_84.htm
converter: vecrescue 0.1.0
issue_tab: n1-july-1994
tags:
- reviews
- language design
- implementations
---

by Adrian Smith
{ .byline }

### Background

Remember that I started life as a Basic programmer, moved to PL/1, and then had my first encounter with APL back in November 1978. Since then I have learned C, SQL and PostScript, and may soon encounter ABAP/4 in earnest (yet another 4GL/query language). Of these, only SQL and APL have been structureless, as neither was intended for writing the sort of procedural code that PL/1 excels in. PostScript is unusual, in that it has no ‘goto’, so you have to work with the ‘if-else’ and ‘repeat’ constructs. In practice, the same is almost true of C, and I usually tried to write ‘goto-less’ PL/1 where I could.

As you can see from this brief life-history, I am a slightly biased observer, particularly as I have always tended towards the ‘keep-it-simple-stoopid’ style of APL, which has meant avoiding deeply nested structures and using loops in place of fancy 3-D reshapes and diagonal transposes! With this in mind, please read sceptically, and hope for a balancing article in the next Vector from a less structured author.

### Philosophical Stuff

If there is a wasp in the room, I like to know where it is! The same applies to loops in APL, which is why I have always used an old STSC ‘whizzbang’ technique to make them very obvious:

```
Loop:→LAB←(⍴data) iterate End,ct←1
 process data[ct]
End:→LAB[ct←ct+1]
```

... of the various OR scientists who have used APL over the years, about half detest this idea (and stoutly refuse to use it), and the other half think it is quite sensible, and take to it immediately. I think the split will almost certainly be reflected in the APL world as a whole, but at least the APL\*PLUS III approach has the merit of being totally ignorable (and takes a mere 14 pages of manual to explain), so the less-structured APLers amongst us cannot get too upset over it.

It is also interesting that the latest J release has introduced a very similar set of structures (see David Ziemann’s review and Gene McDonnell’s “At Play with J”), and no-one seems too bothered that these undermine the clean simplicity of the notation. I think that we need to see the languages evolving at three levels:

- the fundamental notation is simple mathematics plus some structural stuff to do with constructing and reshaping arrays.
- control structures are an outer level, which select the particular bit of APL notation to execute under particular circumstance.
- namespaces/locales are an application management tool which only become relevant for large APL sites and complex systems, typically those being worked on by more than one programmer.

I think I would like to start a drive towards the global elimination of each, on the grounds that:

<ol type="a">
<li>it looks incredibly ugly and makes code really hard to read</li>
<li>it kids you into thinking that you aren't looping</li>
<li>it confuses the nice classification shown above, by falling midway between levels 1 and 2.</li>
</ol>

This becomes a practical proposition only if the control structures are easy to work with, and execute efficiently.

I think I only have one serious worry – control structures may stop people bothering to think through the problem and result in simplistic iterated code rather than simple array code. However each is just as bad, if not worse, and I have seen enough awful APL even at the VS APL stage to realise that FORTRAN programmers will write bad APL anyway, so we may as well make them a little more comfortable whilst doing it.

### When Would I use this Stuff?

There are some places in APL where you cannot escape a ‘branch if’ construction – one of the most obvious is defaulting a missing left argument to an ambivalent function:

```
    ∇ R←CHAR AMBI1 TXT
[1]  ⍝ Remove CHAR from TXT
[2]   ⍎(0=⎕NC 'CHAR')/'CHAR←'' '''
[3]   R←TXT~CHAR
    ∇
```

... which might generate a STYLE ERROR due to its gratuitous use of Execute and the doubled quotes, so maybe we could also code:

```
    ∇ R←CHAR AMBI2 TXT
[1]  ⍝ Remove CHAR from TXT
[2]   →(2=⎕NC 'CHAR')↑⎕LC+1 ⋄ CHAR←' '
[3]   R←TXT~CHAR
    ∇
```

... or even infest our code with some kind of DOIF construct. The alternative offered by APL\*PLUS III is to write it like this:

```
    ∇ R←CHAR AMBI3 TXT
[1]  ⍝ Remove CHAR from TXT
[2]  :IF 0=⎕NC 'CHAR'
[3]     CHAR←' '
[4]  :END
[5]   R←TXT~CHAR
    ∇
```

... or to pack it all on one line if you prefer ...

```
    ∇ R←CHAR AMBI4 TXT
[1]  ⍝ Remove CHAR from TXT
[2]  :IF 0=⎕NC 'CHAR' ⋄ CHAR←' ' ⋄ :END
[3]   R←TXT~CHAR
    ∇
```

... but woe betide those who think they should get away with:

```
    ∇ R←CHAR AMBI5 TXT
[1]  ⍝ Remove CHAR from TXT
[2]  :IF 0=⎕NC 'CHAR' ⋄ CHAR←' '
[3]   R←TXT~CHAR
    ∇
```

They will get no further than...

```
     'f' AMBI5 'fat cat'
OUTER SYNTAX ERROR, LINE 4, STATEMENT 1
      'f' AMBI5 'fat cat'
          ^
```

in other words, their function will not even start executing unless the control structures are all balanced up nicely. In a trivial example like this, no problem, but I wonder how APLers will take to this very ‘compile and run’ approach to finding syntax errors.

Does it make the code easier to read? In as trivial an example as this, I don’t really think it does, but I was looking through some code yesterday which unscrambles EDI messages and does interesting things with the product codes and variant tags. The main workhorse function is a set of 6 almost identical blocks of code, one for each message type. It is begging for a ‘Select ... Case’ construction, and would benefit greatly from the indented code which this encourages.

```
:select msgtype
  :case 1
    lots of quite
    horrid code
  :case 2
    and so on

  :else
    'Unknown message type'
  :end
```

I really can’t see any objection to this – it adds a degree of structural readability to the function as a whole, without in any way interfering with the actual APL code. The APL\*PLUS III editor helps matters with its automatic indenting. It also seems possible to stop in the middle of such a structure and add lines of code without an SI DAMAGE, but I haven’t had time to explore this fully.

The other obvious application of control structures is in replacing my old iterate code with something that reads as an explicit each:

```
Loop:→LAB←(⍴data) iterate End,ct←1
 process data[ct]
End:→LAB[ct←ct+1]
```

... becomes

```
:for ct :in (⍳⍴data)
   process data[ct]
:end
```

... a very obvious simplification, and on almost any terms, a good thing.

### Is it Fast Enough to be Useful?

I can’t see that there are any timing issues around the Select-Case or While-Until constructions – the APL code inside the loop almost always heavily outweighs the trivial ‘check and branch’ outer syntax. What is more interesting is the speed of iteration, as there are times when you want to process record by record, so the choice here is between:

- the new iterative structure
- nesting the data and using the each operator
- doing some serious thinking and avoiding the loop altogether.

I though a good example would be a text search through a reasonably large address list, such as the delegate list from Toronto. I might want to find everyone with a CompuServe account, so that I can add them to my address book:

`:whinge`  
how to get the file into the PlusIII workspace? I loaded it into my favourite text editor and copied it to the clipboard. Would the editor accept it – too big at 39396 bytes. OK, let’s `⎕NREAD` it and partition-enclose on the `⎕TCNL`. NONCE ERROR it says – dive for the manual and use `⎕PENCLOSE` instead. Now I have a nested text vector – double click it to edit and all I get is ‘object not editable’. This is NOT ON. I have never had PLUSII on my machine, and this junk is going straight back to Anthony until it works with vectors of vectors. So there.  
`:endwhinge.`

Now for those timings I set out to do 20 minutes ago ...

```
     a←0
     vvv←5000⍴'fat' 'cat' 'sat'
     t←⎕ai[2] ⋄ process¨vvv ⋄ ⎕ai[2]-t
0.77
     t←⎕ai[2] ⋄ loopy vvv ⋄ ⎕ai[2]-t
1.49

loopy x
:for ct :in (⍳⍴x)
 process x[ct]
:end

process x
a←a+1
```

If you were starting off with a text matrix, you could add around .1 sec to the time for `process¨` as you would need to split the data into vectors, and maybe a little more time to junk trailing blanks. Assuming that you wanted to do some significant processing within the loop, I would suggest you make an explicit loop and tolerate the small loss of performance.

`:whinge`  
I just tried some more simple stuff with nested vectors and came upon:

```
      ⍴vvv
5000
      www←5000⍴'ere' 'we' 'go' 'again'
      t←⎕ai[2] ⋄  p←vvv⍳www ⋄ ⎕ai[2]-t
195.04
```

          ... that time you see is in seconds – that’s right: it took over 3 minutes on a fast DX2. What’s more it totally locked the machine while it did it, so that I was on the point of giving up and hitting &lt;reset> and hence losing several of the above (unsaved) paragraphs in the process. Dyalog/W executes the above in .22 seconds, which is more reasonable.

`:endwhinge.`

### An Idea Brought on by Reading Dave’s Review

As you can tell, I feel really positive about both the idea and most of the implementation of control structures in APL\*PLUS III. It seems to me that there is one thing Manugistics could usefully add – the J constructs of try. and catch. which are similar in concept to IBM’s `⎕EA`. For example:

```
    ∇ R←CHAR AMBI6 TXT
[1]  ⍝ Remove CHAR from TXT
[2]  :TRY  R←TXT~CHAR
[3]   :CATCH R←TXT~' '
[4]  :END
    ∇
```

... looks logical and clean. It would also make sense for the very commonly coded “create this file; if it’s already there tie it and truncate it” logic which is very messy and a little dangerous to code with `⎕ELX`.

### Conclusion

I stick by my view that Manugistics have done the right thing, and introduced control structures in a remarkably non-invasive way which allows the programmer to add structural information to functions without interfering with the basic notation at all. It executes fast enough to be a usable each replacement, and improves heavy Select-Case oriented functions enormously.

I worry about portability, and I would like to see some code to ‘de-structure’ a function into plain VS APL – there was a proposal at Toronto for a similar style of structuring which did offer this possibility, so perhaps some dedicated hack would like to have a go? This apart, I can see no problems with the basic idea, so the sooner everyone else copies it, the happier I will be.

(webpage generated: 10 October 2007, 05:08)
