---
vid: '10003010'
title: CRCOMPARE in J
authors:
- Adam Dunne
byline: from Adam Dunne (Tamil Nadu, India)
volume: '21'
issue: '4'
page: '7'
source: trad/v214/dunne214.htm
converter: vecrescue 0.1.0
issue_tab: n4-september-2005
tags:
- programming techniques
- development practice
---

*from Adam Dunne (Tamil Nadu, India)*
{ .byline }

The attached code (written in J but without any tacit coding for simplicity) is shorter than the APL version in *Vector*, but perhaps more importantly, I think the code is easier to grasp, resting as it does on one central idea, ‘a matching lines’ table, shown below (each line of `cr1` is a row, each line of `cr2` is a column; matches are flagged). One can intuitively see how the code extracts the line numbers from the table indices. A blank line is added to the top of `cr1` and `cr2` in `crcreate` as a 1 in position (0,0) of the table is always needed to make it work.

J does not use line numbers, but I thought they might be useful in an application like `crcompare`, so I added them. Sample output follows ...

```
 'cr1 cr2'=.'calc'crcreate'calc2'
    addlinenos cr1
   0|
   1|3 : 0
   2|display a+b+c
   3|d=.3
   4|e=.d=.a+b+c
   5|f=.d+e
   6|a=.0 0 $0
   7|b=.0 0$0
   8|g=.f^0.5
   9|display'done'
  10|z=.0.01*d
  11|)

    addlinenos cr2
   0|
   1|3 : 0
   2|display a+b+c]d=.3
   3|e=.d=.a+b+c
   4|f=.d+e
   5|g=.f^0.5
   6|a=.b=.0 0$0
   7|display'done'
   8|z*d%100
   9|)
```

```
    NB. This is the table of matching lines
    cr1-:/"1 1"1 2 cr2
 1 0 0 0 0 0 0 0 0 0
 0 1 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0
 0 0 0 1 0 0 0 0 0 0
 0 0 0 0 1 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 1 0 0 0 0
 0 0 0 0 0 0 0 1 0 0
 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 1

    'calc'crcompare'calc2'
   2|display a+b+c
   3|d=.3
 -replaced by----
   2|display a+b+c]d=.3

   6|a=.0 0 $0
   7|b=.0 0$0
 ----deleted----

   6|a=.b=.0 0$0
 ----added----

  10|z=.0.01*d
 -replaced by----
   8|z*d%100
```

```
 display=:(1!:2)&2

 crcompare=: 4 : 0
 'cr1 cr2'=.x.crcreate y.
 cr1nos=.addlinenos cr1
 cr2nos=.addlinenos cr2
 linesmatchtab=.cr1-:/"1 1"1 2 cr2
 inds=.I.1=,linesmatchtab
 ind2=.($linesmatchtab)#:inds
 z=.0 0,<:(}.ind2)-}:ind2 NB. indices increment
 z2=.ind2-z               NB. starting indices
 z3=.i.0
 for_ct.i.{.$z do.
   z3=.z3,(((<ct,0){z2)+i.(<ct,0){z);((<ct,1){z2)+i.(<ct,1){z
 end.
 z4=.((-:$z3),2)$z3
 lineflags=.dims"0 z4
 zout=.0 0$0
 width=.1{$cr1
 for_ct.i.{.$z4 do.
   flagsrow=.ct{lineflags
   'extralines1 extralines2'=.ct{z4
   if.flagsrow-:1;1 do.     NB. replace
     zout=.zout,(extralines1{cr1nos),(width{.'-replaced by----')
     zout=.zout,(extralines2{cr2nos),(1,width)$' '
   elseif.flagsrow-:1;0 do. NB. deleted
     zout=.zout,(extralines1{cr1nos),(width{.'----deleted----'),(1,width)$' '
   elseif.flagsrow-:0;1 do. NB. added
     zout=.zout,(extralines2{cr2nos),(width{.'----added----'),(1,width)$' '
   end.
 end.
 zout=.charmattovec zout
 )
```

```
 samewidth=: 4 :'(maxw{."1 x.);(maxw=.>./(1{$x.),1{$y.){."1 y.'
 charvectomat=: 3 :',;._2 y.,LF'NB.converts char vec with LFs to char mat
 crcreate=:4 :'('' '',charvectomat 5!:5<x.)samewidth '' '',charvectomat 5!:5<y.'
 dims=:3 :'<(#>y.)>0' NB. flags boxes where dimension>0
 elim_trail_bl=:3 :'(($y.)-(|.'' ''=y.)i.0){.y.'
 NB.eliminates trailing blanks
 charmattovec=:3 :0 NB. converts character matrix to char vector with LFs
 z=.i.0
 for_ct.i.{.$y. do.
   z=.(z,elim_trail_bl ct{y.),LF
 end.
 )
```

```
 addlinenos=:3 :'(3":,.i.{.$y.),.''|'',.y.' NB. adds linenos to crfn
```
