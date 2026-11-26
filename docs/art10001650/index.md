---
vid: '10001650'
title: What’s in a Name?
authors:
- Sylvia Camacho
byline: by Sylvia Camacho, . (sylviac@blueyonder.co.uk)
volume: '10'
issue: '4'
page: '78'
source: trad/v104/camacho104_78.htm
converter: vecrescue 0.1.0
issue_tab: n4-april-1994
tags:
- programming techniques
- applications
---

by Sylvia Camacho, <span class="affil">.</span> <span class="email">(<a href="mailto:sylviac@blueyonder.co.uk">sylviac@blueyonder.co.uk</a>)</span>
{ .byline }

The problem of finding a record by name is familiar to anyone who has set up a system to manage a membership or a customer list. Names are not unique and are often misheard and mis-spelled. Finding the first match and then presenting successive records to allow a choice to be made on the basis of the other details is an easy way to handle common names but the onus is on the keyboard operator to get the name exactly right and to cross-check with other details until completely satisfied that a match has been found. I am sure mine is not the only family with several members having accounts at the same branch of a bank to have found we are paying each others’ standing orders.

A more challenging case is to find a record which is believed to be in the database but an exact match for the supposed name cannot be found. The member or customer may not be on hand, may not speak good English or may not have legible handwriting. In such circumstances the operator could make successive guesses or try to set up a search on another field for which some details are known like address or post code. Even better would be to be given a list of near matches.

I found myself in need of some such retrieval functions recently when setting up a customer database for a shop selling TV and white goods on hire purchase or rental. Many customers do not know their account number and a facility to enter the first few characters of their surname and retrieve a list of near matches allows a quick response. The database holds the list of names as a matrix and uses it for sorting before printing. I decided, therefore, to use the sort matrix to control the sequence in which records would be put to the screen according to the ‘goodness of fit’ of the matrix name to the name input, which might be only the first few characters.

My first attempt was the function match below which compares the key input with the same number of columns down the matrix of names and returns the 2 decode of the resulting booleans. This gives more weight to a match closer to the beginning of the names but, of course, the sequence of the key must be an exact match over those columns selected by the length of the key. It is probably faster to confine the decode to those names having the correct initial letter as shown. One would normally hold the names matrix as upper case to save conversion on retrieval. The result is a numeric vector which is used to re-order the database index before the records are displayed. By this function Smith is more closely matched by Smythe than it is by Smillie but the column positions of characters must be the same for a match to contribute to the weighting, and most spelling variations do not have this property.

```
∇match[⎕]∇
    ∇ r←names match key;boo;s
[1]  ⍝ returns weighted match
[2]   names←ucase names
[3]   s←⍴key←clip ucase key
[4]   boo←names[;⎕io]=1↑key
[5]   r←names[boo/⍳⍴boo;⍳s]=((+/boo),s)⍴key
[6]   r←boo\2⊥⍉r
    ∇
      names[¥names match 'camacho';]
Camacho
Camaccio
Comacho
Comanche
the remaining names, having a zero weight, are not shown

      names[¥names match 'mahomet';]
Mahon
Mahmoud
Macleod
MacIntyre
Mohamet
Mohammed
McCloud
McIntire
Moon
and after this zero weighted
```

Then I remembered reading in a Zark Newsletter about a coding system based on the sound rather than the spelling of names. It was devised to resolve problems with early US Census returns where the large immigrant population Anglicised their names with very variable results. It has also proved useful for doing genealogical research where spelling is less standardised and varies over the generations. The principle is to group the letters of the alphabet according to similarity of sound. Those familiar with Pitmans shorthand will recognize this idea. The set of vowels and near-vowels is assigned to Group 0 and are only taken into account as spacers during the assessment of consonants, being omitted from the final code. The grouping is as follows:

```
     0     A E H I O U W Y
     1     B F P V
     2     C G J K Q S X Z
     3     D T
     4     L
     5     M N
     6     R
```

To obtain the code for a name, case is ignored and spaces and non-alphabetic characters are compressed out. Then the group number is substituted for each character. Contiguous duplicate numbers are compressed out and the first number is dropped and the first character of the name is restored in its place. Finally all zeros are compressed out and the code truncated to the alpha plus three digits with trailing zero fill if necessary. The result is called the Soundex Code for the name. Applying this algorithm to my list of names and using the upgrade of the result to order them should bring names close together which are similar in sound although not in spelling. Thus Macleod will have the same Soundex code as McCloud. However the initial alpha has an overriding effect as shown by the second of the namelists below. If as is seen in the third column the initial digit is retained instead of substituting the first character then Feezy can be matched with Pheasey.

Of course one has to accept that many more possible matches will be thrown up by the use of Soundex and this may be difficult to manage for a large database. Zark suggests that the return from the soundex function should be made numeric by using the index of the initial character in the alphabet as the start of the Soundex code. The Zark article was presented as a competition to find the fastest version which my attempt below will certainly not be. Has anyone seen the winning one?

```
∇soundex[⎕]∇
    ∇ codes←soundex names;vector;alph;nums;s;ab
[1]  ⍝ see Zark News 1993 Quarter 2
[2]   names←ucase leftjustify(1 0 ⌈¯2↑⍴names)⍴names
[3]   vector←,'*',names
[4]   alph←'AEHIOUWYBFPVCGJKQSXZDTLMNR*'
[5]   nums←'00000000111122222222334556*'
[6]   vector←(vector∊alph)/vector
[7]   codes←nums[alph⍳vector]
[8]   codes←(codes≠¯1⌽codes)/codes
[9]   codes←,'*', 0 1 ↓'0' vim codes
[10]  s←1↑⍴codes←'0' vim(codes≠'0')/codes
[11]  codes←names[;⎕io],(s,3)↑codes
    ∇
```

vim is a slightly amended version of the Sharp vtom

```
 alphabetic   Soundex            Soundex, no alpha

 Barclay      Barclay     B624   Feezy       1200
 Berkeley     Berkeley    B624   Pheasey     1200
 Braun        Braun       B650   Barclay     1624
 Brown        Brown       B650   Berkeley    1624
 Camaccio     Camaccio    C520   Braun       1650
 Camacho      Camacho     C520   Brown       1650
 Comacho      Comacho     C520   Shaksper    2216
 Comanche     Comanche    C552   Shakespeare 2221
 Feezy        Feezy       F200   Camaccio    2520
 Graeme       Graeme      G650   Camacho     2520
 Graham       Graham      G650   Comacho     2520
 Macleod      Macleod     M243   Smith       2530
 MacIntyre    McCloud     M243   Smythe      2530
 Mahmoud      MacIntyre   M253   Smiley      2540
 Mahon        McIntire    M253   Smillie     2540
 McCloud      Mahon       M500   Comanche    2552
 McIntire     Moon        M500   Graeme      2650
 Mohamet      Mahmoud     M530   Graham      2650
 Mohammed     Mohamet     M530   Yates       3200
 Moon         Mohammed    M530   Yeats       3200
 Newell       Newell      N400   Macleod     5243
 Newhall      Newhall     N400   McCloud     5243
 Pheasey      Pheasey     P200   MacIntyre   5253
 Reiter       Reiter      R360   McIntire    5253
 Rider        Rider       R360   Newell      5400
 Ryder        Ryder       R360   Newhall     5400
 Shakespeare  Shaksper    S216   Mahon       5500
 Shaksper     Shakespeare S221   Moon        5500
 Smiley       Smith       S530   Mahmoud     5530
 Smillie      Smythe      S530   Mohamet     5530
 Smith        Smiley      S540   Mohammed    5530
 Smythe       Smillie     S540   Reiter      6360
 Yates        Yates       Y320   Rider       6360
 Yeats        Yeats       Y320   Ryder       6360
```

### Wildcard Matching

The method of searching against an incomplete key which is included in DOS uses `*` and `?` as wild cards. This technique is available as an ISIAPL toolkit function called wildcard which returns a boolean vector showing the rows of the matrix for which a match is found. The search argument is summarised below.

```
  x     Select exact match on x.
  x*    Select names starting with the sequence x.
  *x    Select names ending with the sequence x.
  x*y   Select names starting with x and ending with y.
  *x*   Select names containing the sequence x.
  *     Select all names.
```

In summary, `'*'` matches an arbitrary sequence of any length, including 0. Also, the special character `'?'` is permitted within `<x>` or `<y>`, and has the significance of matching any single non-blank character.

```
      (names wildcard 'C*cho')⌿names
Camacho
Comacho
      (names wildcard 'Cam*')⌿names
Camaccio
Camacho
      (names wildcard '*Int*')⌿names
MacIntyre
McIntire
```

There is a very similar and somewhat more flexible facility in Microsoft Access: see the specification below. Of course this only allows one record at a time to be found so one has to step through the file using the Find Next button. In order to pull out all matches one would have to write some SQL and that involves reading the manual!

The asterisk (\*), question mark (?), number sign (#), exclamation point (!), hyphen (-), and brackets (\[ \]) are wildcard characters. You can use these characters in queries, commands, and expressions to include all records, file names, or other items that begin with specific characters or match a certain pattern. You can also use wildcard characters and matching characters to further refine a search.

```
*      wh* finds what, white, and why.
*at finds cat, bat, and what.
Like the MS-DOS asterisk (*) wildcard character, this asterisk matches any number of characters.  But unlike MS-DOS, it can be used as the first or last character in the character string.
?      b?ll finds ball, bell, and bill.
Like the MS-DOS ? wildcard character, this symbol matches any single character.
#      1#3 finds 103, 113, 123.
Matches any single digit.
[ ]      b[ae]ll finds ball and bell but not bill.
Matches any single character within the brackets.
!      b[!ae]ll finds bill and bull but not bell.
Matches any character not in the list.
-      b[a-c]d finds bad, bbd, and bcd.
Matches any one of a range of characters.
```

Is anyone prepared to amend the Sharp wildcard function to meet the MS Access specification? Or maybe, of course, to better it? Happy hunting!

(webpage generated: 28 March 2006, 06:49)
