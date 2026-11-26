---
vid: '10500460'
title: Simulating the Enigma
authors:
- Keith Smillie
byline: Keith Smillie(smillie@cs.ualberta.ca)
volume: '24'
issue: '4'
page: '117'
online: '2010-03-16'
description: The author pits his wits and J against the famous WWII cipher machine
abstract: The Enigma was the electromechanical cipher machine used by all branches of the German armed forces during the Second World War. Enigma messages which were intercepted by the British and then deciphered at the Government Code and Cypher School at Bletchley Park provided intelligence that has been estimated to have shortened the war by one or two years. The present paper, which begins with some introductory remarks on cryptography, describes the Enigma in general terms and then gives a more detailed discussion of a somewhat simplified version and its implementation in J.
source: content/printed/244/smillie.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- applications
- programming techniques
---

Keith Smillie  
(smillie@cs.ualberta.ca)
{ .byline }

The Enigma was the electromechanical cipher machine used by all branches of the German armed forces during the Second World War. Enigma messages which were intercepted by the British and then deciphered at the Government Code and Cypher School at Bletchley Park provided intelligence that has been estimated to have shortened the war by one or two years. The present paper, which begins with some introductory remarks on cryptography, describes the Enigma in general terms and then gives a more detailed discussion of a somewhat simplified version and its implementation in J.
{ .abstract }

## Cryptography

Cryptography is the science and art of both concealing the meaning of a message by enciphering it according to some given algorithm and also deciphering an enciphered message to recover its original meaning. The two main ciphering methods are the *substitution cipher* in which each letter of a message is replaced by another character but retains its position in the message and the *transposition cipher* in which each letter remains unchanged but changes its position. For example the phrase `The University of Alberta` may be enciphered either by substitution to `UIFVOJWFSTJUZPGBMCFSUB` in which each letter is replaced by the letter one place ahead of it in the alphabet or by transposition to `NHTEUEISVRTOFYILRABEEAHTT` in which each group of five letters is randomly permuted. The original message is referred to as the *plaintext* and the enciphered message as the *ciphertext*. In the following we shall consider only substitution ciphers.

One of the earliest examples of a substitution cipher is attributed to Julius Caesar and is therefore known as the Caesar cipher. In this cipher each letter in a message is replaced by the letter which occurs three places ahead of it in the alphabet. Therefore the cipher may be summarized as:

Plain alphabet
:   a b c d e f g h i j k l m n o p q r s t u v w x y z

Cipher alphabet
:   A B C D E F G H I J K L M N O P Q R S T U V W X Y Z

The plaintext phrase `The University of Alberta` of the previous paragraph when enciphered in this manner becomes `WKHXQLYHUVLWBRIDOEHUWD`. The shift need not be limited to 3 places, and may be any value between 1 and 26 where a shift of 26 leaves the message unchanged. The example of the substitution cipher in the previous paragraph is a Caesar cipher with a shift of 1.

Caesar ciphers are relatively easy to break since there are only 26 of them and each may be tried in turn on a portion of the enciphered message until the correct one is found and then applied to the entire message. If there is a sufficiently long message a frequency analysis of the text may assist in the decipherment making use of the relative frequencies of the letters in English text with `e` being the most common letter, followed by `t`, then `a`, and so on. (Also an analysis of the frequency of the 26 × 26 pairs of letters can be very helpful since, for example, certain pairs such as `qq` and `zz` never occur and other pairs occur usually in one order but not the other.) A considerably more complicated substitution cipher is obtained by using a random permutation of the cipher alphabet so that, for example, each letter of the plaintext in the first row would be enciphered by the letter below it in the second row in the following table:

```
a b c d e f g h i j k l m n o p q r s t u v w x y z
I O H C F P T V M G D A Z J Y U W K X R L N Q S B E
```

Even though the total number of such ciphers is 26!, which is equal to

403,291,461,126,605,635,584,000,000

such ciphers can still be broken by frequency analysis.

The Caesar ciphers of the previous paragraph are known as *monoalphabetic* substitution ciphers since only one cipher alphabet is used in a given message. However it is possible to define *polyalphabetic* substitution ciphers in which more than one cipher alphabet is used in a message according to a rule called the *key* which indicates which cipher alphabet is applied to each letter. The best known of these polyalphabetic ciphers is the Vigenère cipher named after the 16th-century French diplomat Blaise de Vigenère who further developed and promoted a method of encipherment which had been suggested during the previous century by the Italian architect Leon Alberti. In the Vigenère cipher there are 26 cipher alphabets corresponding to Caesar shift alphabets with shifts of 1, 2, …, 26 letters. These may be conveniently arranged as shown in part in the table labelled “Vigenère Square” below.

As an example of the use of the Vigenère cipher let us again encipher the phrase `The University of Alberta` using as a key the word `VECTOR`. Write the plaintext with the keyword above it and repeated sufficiently often so that each letter of the plaintext has an associated keyword letter:

```
V E C T O R V E C T O R V E C T O R V E C T
t h e u n i v e r s i t y o f a l b e r t a
```

To encipher the first letter `t` of the plaintext find the letter `V` of the keyword appearing above it and then select from the column headed `t` the letter `O` which appears below it in the row labelled with a `V` on the left. The second letter `h` of the plaintext is enciphered with `L` which appears below it in the row which begins with `E`. Similarly the third letter `e` of the plaintext is enciphered with `G` which appears below it in the row which begins with `C`. Continuing in this manner – if we had the complete Vigenère square - we may determine that the ciphertext is  
`OLGNBZQITLWKTSHTZSZVVT`.

<table class="left">
		<caption>Vigenère square</caption>
		<tbody valign="bottom">
			<tr><td> </td><td class="code">abcdefghijklmnopqrstuvwxyz</td></tr>
			<tr><td>A</td><td class="code">ABCDEFGHIJKLMNOPQRSTUVWXYZ</td></tr>
			<tr><td>B</td><td class="code">BCDEFGHIJKLMNOPQRSTUVWXYZA</td></tr>
			<tr><td>C</td><td class="code">CDEFGHIJKLMNOPQRSTUVWXYZAB</td></tr>
			<tr><td>D</td><td class="code">DEFGHIJKLMNOPQRSTUVWXYZABC</td></tr>
			<tr><td>E</td><td class="code">EFGHIJKLMNOPQRSTUVWXYZABCD</td></tr>
			<tr><td>F</td><td class="code">FGHIJKLMNOPQRSTUVWXYZABCDE</td></tr>
			<tr><td>G</td><td class="code">GHIJKLMNOPQRSTUVWXYZABCDEF</td></tr>
			<tr><td>H</td><td class="code">HIJKLMNOPQRSTUVWXYZABCDEFG</td></tr>
			<tr><td>…</td></tr>
			<tr><td>T</td><td class="code">TUVWXYZABCDEFGHIJKLMNOPQRS</td></tr>
			<tr><td>U</td><td class="code">UVWXYZABCDEFGHIJKLMNOPQRST</td></tr>
			<tr><td>V</td><td class="code">VWXYZABCDEFGHIJKLMNOPQRSTU</td></tr>
			<tr><td>W</td><td class="code">WXYZABCDEFGHIJKLMNOPQRSTUV</td></tr>
			<tr><td>X</td><td class="code">XYZABCDEFGHIJKLMNOPQRSTUVW</td></tr>
			<tr><td>Y</td><td class="code">YZABCDEFGHIJKLMNOPQRSTUVWX</td></tr>
			<tr><td>Z</td><td class="code">ZABCDEFGHIJKLMNOPQRSTUVWXY</td></tr>
		</tbody>
	</table>

The Vigenère cipher is equivalent to enciphering text with a several Caesar ciphers with different shifts applied in an orderly manner by use of a given keyword which may be selected from an almost infinite number of keywords. For many years the Vigenère cipher was considered unbreakable, and indeed was known as *le chiffre indéchiffrable*. However, a method of breaking it was found in the middle of the 19th century independently by Charles Babbage, honoured in the history of computing as the “father of computing” for his Difference and Analytical Engines, and by Friedrich Wilhelm Kasiski, a retired Prussian army officer. At the end of the First World War an American Army officer proposed a method of making the Vigenère cipher unbreakable by having for each message a random key as long as the message. However these “onetime pad” ciphers, as they were called, never became popular due to the problems of generating and distributing in a secure manner the random keys.

The first attempts to mechanize enciphering and deciphering were made in the fifteenth century by Leon Alberti who was mentioned above as the originator of the Vigenère cipher. He had two copper disks, one slightly larger than the other and mounted concentrically, each with the alphabet inscribed about its circumference. By rotating the disks so that, for example, `A` on the outer disc was opposite `D` on the inner disk, the alignment of the letters would facilitate the enciphering or deciphering of a message with a Caesar shift of 3. These cipher disks remained in use as late as the American Civil War, and indeed well into the 20th century as a children’s toy. The next development in the mechanization of cryptography is the Enigma which we will discuss in the next section.

## The Enigma

The Enigma was designed and built by the German engineer Arthur Scherbius and was intended as a simple method of mechanically enciphering and deciphering messages with a very large number of polyalphabetic substitution ciphers. He took out his first patent in 1918 for a machine about the size and weight of a typewriter which cost approximatelyt $30,000 at today’s prices. Although it was promoted widely – for example, it was shown at the 1923 Congress of the International Postal Union in Bern, Switzerland – it initially attracted little interest because of the cost and the belief that the degree of security it offered was unnecessary. However in the mid-1920s the German military became interested and Scherbius started mass producing Enigmas and eventually supplied over 30,000 machines to the German armed services.

The Enigma consisted of three parts: a typewriter keyboard for input, a ‘scrambler’ mechanism for applying a continually changing polyalphabetic cipher to each letter of the message, and for output a set of 26 lamps marked with the letters of the alphabet indicating the encipherment of each letter as it was input. The enciphered message was written down on paper and then transmitted either by telegraph or wireless to the intended recipient who would then use his Enigma which would be configured in the same way as was the sender’s Enigma to decipher the message.

The main part of the scrambler was a set of three rotating discs or *rotors* about 4 inches in diameter and mounted in the machine on a removable axle, Each rotor had a circle of 26 electrical contacts near the edge of each face with the contacts on one face being connected to those on the other face in a random order. Different random connections were used for each of the rotors. Depressing any key on the input typewriter would send an electrical current first to one of 26 contacts on a fixed input disc to the right of the three rotors. (The keyboard was connected to the fixed disc in simple alphabetical order with A on the keyboard connected to the first position on the disc, B to the second position, and so on.) The current would then pass to a contact on the right face of the right-hand rotor and then to the randomly connected contact on the left face of the rotor, through contacts on the right and left faces of the middle rotor, and then through the right and left faces of the left-hand rotor. From the left-hand face of the left rotor the current would pass through a fixed ‘reflector’ disc with contacts on its right face only which would effect a fixed interchange of pairs of letters. The current would then flow back through the three rotors in a left-to-right order, again through the fixed disc on the right, and finally to one of the 26 lamps which would display the encipherment of the letter which had been input.

The encipherment of each letter would involve a total of seven distinct polyalphabetic substitutions. However the Enigma was designed to change this encipherment from letter to letter by having the right-hand rotor advance to the next of its 26 possible positions at the input of each letter and before the electrical contact was made to the fixed input disc. When the input of 26 letters of a message had caused the right-hand rotor to make a complete revolution, a carry mechanism would cause the middle rotor to advance one position. After another complete revolution of the right-hand rotor the middle rotor would be advanced another position, and so on until it would have made a complete revolution at which time the left rotor would be advanced by one position. Thus the three rotors may be considered to function as a base-26 odometer. The introduction of the reflector had two important consequences: A letter could never be enciphered into itself, and an enciphered message could be deciphered by entering it into an Enigma with the same initial settings as the machine which enciphered the message.

Each of the rotors had a ring attached to the outside and labelled around the circumference, usually with the letters A to Z, which could be turned to any one of 26 positions. Furthermore each ring had a small carry notch which would engage the rotor to the left and make it advance one position. Although the ring setting of a rotor did not effect the total number of scramblings, it did effect the letter-by-letter substitutions and the point in the revolution of a rotor when the rotor to its left was advanced.

In addition to the rotors a further scrambling was introduced by a stationary plugboard which through a set of cables allowed pairs of letters to be imterchanged. Usually there were 10 sets of cables which thus allowed the swapping of 10 pairs of letters. The plugboard was connected both to the keyboard and to the output lamps so that the pairwise swapping of the letters took place both before it entered the fixed input disc and after it had been scrambled by passing through the rotors, the reflector disc and through the rotors a second time.

The Enigma initially had only three rotors which of course could be inserted in any one of six possible orders. In the late 1930s the Enigma versions used by the German Army and Air Force were issued with five rotors, labelled I, II, III, IV and V, three of which were selected for a given Enigma setting. The Naval version had three additional rotors labelled VI, VII and VIII, and some versions could accommodate four of these eight rotors.

It is of interest to calculate the total number of possible substitution ciphers made possible by the scrambling mechanism. If we assume that there are five rotors, then the three rotors may be selected and installed in 6 × 5 × 4 or 60 possible ways. For each of these rotor orders the total number of substitutions is 26 × 26 × 26 or 17,576. To calculate the number of substitutions provided by the plugboard we note that the number of ways of choosing m pairs out of n objects is n!/((n-2m)!m!2<sup>m</sup>). If we assume there were 10 cables that could be connected, then m=10 and since n=26, this expression becomes 26!/(6!10!2<sup>10</sup>), which may be calculated in J in extended precision as

```
   x: (!26) % (!6) * (!10) * 2^10
```

and has the value 150,738,274,937,250. Therefore the total number of substitutions available with the Enigma is given by

```
   60 * 17576 * x: (!26) % (!6) * (!10) * 2^10
```

which is equal to 158,962,555,217,826,360,000.

or approximately 159 million million million. Obviously some form of frequency analysis together with any knowledge of the possible content of the message or the writing style of the sender is the only possible method of breaking such a cipher.

## Using the Enigma

When using the Enigma the Germans drew up monthly setting sheets which specified for each 24-hour period all of the Enigma settings except the initial rotor positions, i.e., the selection of three rotors out of the five, their ring settings and order in the machine, and the 10 pairs of plugboard connections. A person sending a message would first set up his machine according to the settings for that day and then proceed as follows:

1. Select a three-letter *indicator* giving the rotor positions to be used to encipher the initial rotor positions or *message key* to be used when enciphering the message.
2. Turn the rotors to the indicator position and type the message key twice writing down the enciphered letters shown on the lamps.
3. Turn the rotors to the message key letters and type the message writing down the encipherment of each letter.
4. Give the enciphered message together with the indicator and the twice-enciphered message key to the radio operator for transmission.

The recipient of the message would proceed as follows:

1. Set up his Enigma according to the daily settings.
2. Turn the rotors to the indicator position which he would have received unenciphered.
3. Type in the next six letters of the message which would give the repeated message key.
4. Turn the rotors to the message key and type and decipher the message.

## An Enigma simulator

In this section we shall give a simulation in J of the simplified Enigma machine given in Simon Singh’s *The Code Book* with the following properties: The alphabet is limited to the letters A, B, C, D, E and F; the plugboard allows the interchange of only one pair of letters; the rotatable ring with the carry notch determining when the rotor adjacent to it would rotate one position is omitted; and the rotor or rotors advance one position after (not before) a letter has been entered at the keyboard and has been enciphered. However, whereas Singh’s example has the current passing through the rotors first from left to right and then through the reflector and back through the rotors from right to left, our simulation will adhere to the conventional order given in most accounts of the Enigma. The substitutions may be defined as follows:

<table class="left">
		<tbody valign="bottom">
			<tr><td>Rotor   1</td><td class="code">a → B; b → A; c → D; d → F; e → E; f → C</td></tr>
			<tr><td>Rotor   2</td><td class="code">a → C; b → A; c → D; d → B; e → F; f → E</td></tr>
			<tr><td>Rotor   3</td><td class="code">a → F; b → C; c → E; d → D; e → B; f → A</td></tr>
			<tr><td>Reflector</td><td class="code">a → F; b → C; c → B; d → E; e → D; f → A</td></tr>
			<tr><td>Plugboard</td><td class="code">a → B; b → A; c → C; d → D; e → E; f → F</td></tr>
		</tbody>
	</table>

We may note in passing that the number of substitution ciphers for this simplified Enigma is the product of the 3! or 6 permutations of the rotors, the 6 × 6 × 6 or 216 relative positions of the rotors for each of these permutations, and the 15 substitutions for the plugboard, or in total 6 × 216 × 15 or 19,440 substitutions. If we follow and extend Singh’s discussion of the substitutions given by the first rotor as it rotates for each keystroke, we may draw up the following table giving the cipher alphabet for each position of the rotor:

```
  0 1 2 3 4 5 6 7 …
a B D A C B F B D …
b A C E B D C A C …
c D B D F C E D B …
d F E C E A D F E …
e E A F D F B E A …
f C F B A E A C F …
```

This simplified Enigma may be defined in J by the following variables:

```
ALPHA=: 'ABCDEF'                              NB. alphabet
I=: 'BADFEC'; II=: 'CADBFE';  III=: 'FCEDBA'  NB. rotors
Reflector=: 'FCBEDA'                          NB. reflector
Plugboard=: 'BACDEF'                          NB. plugboard
```

If we keep track of the typewriter keystrokes by the variable `Counter`, then the amount of rotation of each of the rotors is given by

```
  'S2 S1 S0'=: 6 6 6#:Counter
```

and, for example, if `Counter=: 20` then we have that `S0` is 2, `S1` is 3 and `S2` is 0, indicating that the right rotor has moved 2 positions, the middle rotor 3 positions, and the left rotor has remained stationary. The values of `S0`, `S1` and `S2`, which are the digits in the base-6 representation of the decimal value of `Counter`, may be used to determine the substitutions effected by the various positions of the rotors. For example, the third column in the above table giving the substitutions for rotor I is given by

```
   ((_2|.ALPHA) i. _2|.I) { ALPHA
```

which is equal to `AEDCFB` representing the substitutions

a → A; b → E; c → D; d → C; e → F; f → B

All of the above substitutions are those given by the current passing through the rotors in a right-to-left direction before passing through the reflector. The second set of substitutions with the current passing through the rotors from left to right may be found in a similar manner, and using the example for rotor I we have

```
   ((_2|.I) i. _2|.ALPHA) { ALPHA
```

which is equal to `AFDCBE` which represents the substitutions

a→ A; b → F; c → D; d → C; e → B; f → E

The Appendix gives a J program in Version 4.06b for a simulation of the simplified Enigma machine based on these considerations. The following is a simple example of its use:

```
   InstallRotors 3 2 1
   SetRotors 'CFD'
   m=:enigma 'ABCDEFABVC'
   m
 BEAAAABC_F
   SetRotors 'CFD'
   enigma m
 ABCDEFAB_C
```

## References

1. Budinsky, Stephen *Battle of Wits: The Complete Story of Codebreaking in World War II.* The Free Press, New York, 2000
2. Hodges, Andrew *Alan Turing: The Enigma of Intelligence.* Unwin Paperbacks, London, 1985.
3. Kahn, David *Seizing the Enigma: The Race to Break the German U-Boat Codes 1939–1943.* Barnes & Noble, Inc., New York, 2009.
4. Sale, Tony *The Enigma Cipher Machine.* 2009  
    [http://www.codesandciphers.org.uk/enigma](http://www.codesandciphers.org.uk/enigma)
5. Singh, Simon *The Code Book: The Science of Secrecy from Ancient Egypt to Quantum Crytptography* Anchor Books, New York, 2000.

## Appendix. Enigma Simulator

```
NB.   Enigma Simulator
NB.   Keith Smillie
NB.   Department of Computing Science
NB.   University of Alberta
NB.   Edmonton, Alberta T6G 2E8
NB.   January 2010
NB.
NB. This program gives a simulation of a simplified model of the
NB. Enigma cipher machine used by the German armed forces during
NB. World War II. The version is very similar to that given in "The
NB. Code Book" by Simon Singh (Anchor Books, New York, 2000).
NB. The allowable characters are given in the list ALPHA. Blanks
NB. may be used and are not enciphered, and all other characters
NB. are enciphered as underscores "_".
NB.
NB. The following dialogue gives a simple example of its use:
NB.   NB. Install rotors in specified order
NB.      InstallRotors 3 2 1
NB.   NB. Set rotors to given key
NB.      SetRotors 'BCA'
NB.   NB. Encipher message
NB.      x=: enigma 'ABCDEFAC'
NB.   NB. Display message
NB.      x
NB.   BCABBAE_E
NB.   NB. Reset rotors
NB.      SetRotors 'BCA'
NB.   NB. Decipher message
NB.      enigma x
NB.   ABCDEFA_C
```

```
ALPHA=: 'ABCDEF'
I=: 'BADFEC'
II=: 'CADBFE'
III=: 'FCEDBA'
Discs=: I;II;III
Reflector=: 'FCBEDA'
Plugboard=: 'BACDEF'
```

```
InstallRotors=: 3 : 0
Rotors=: (<:y.) { Discs
Counter=: 0
empty ''
)
```

```
SetRotors=: 3 : 0
empty
Counter=: (3$#ALPHA)#.|.ALPHA&i. y.
)
```

```
rotate1=: 3 : 0
:
((x.|.ALPHA) i. x.|.y.) { ALPHA
)
```

```
rotate2=: 3 : 0
:
((x.|.y.) i. x.|.ALPHA) { ALPHA
)
```

```
Encipher=: 3 : 0
:
(ALPHA i. y.) { x.
)
```

```
enigma=: 3 : 0
'Left Middle Right'=: Rotors
PlainText=: y.
CipherText=: i. 0
while. 0 < $PlainText do.
'S2 S1 S0'=: -(3$#ALPHA)#:Counter
c=:
{. PlainText
if.
-. c e. ALPHA,' ' do.
c=:
'_'
elseif.
c e. ALPHA do.
c=: Plugboard Encipher c
c=: (S0 rotate1 Right) Encipher c
c=: (S1 rotate1 Middle) Encipher c
c=: (S2 rotate1 Left) Encipher c
c=: Reflector Encipher c
c=: (S2 rotate2 Left) Encipher c
c=: (S1 rotate2 Middle) Encipher c
c=: (S0 rotate2 Right) Encipher c
c=: Plugboard Encipher c
end.
CipherText=: CipherText, c
PlainText=: }. PlainText
Counter=: >:Counter
end.
CipherText <
)
```
