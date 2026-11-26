---
vid: '10006000'
title: Bodyguard of Lies
authors:
- Peter Merritt
byline: by Peter Merritt
volume: '11'
issue: '3'
page: '119'
source: trad/v113/merritt113_119.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1995
tags:
- programming techniques
- competitions and puzzles
---

by Peter Merritt
{ .byline }

Well, several months and one new motherboard later, welcome to part two of my encryption piece for simple (minded) APL-ers. I must start by expressing my thanks to Ray Cannon for constructing and publishing my apology for lateness and my equally sorry solution to the original problem! Slipping out of my hair shirt for a while, I was a little disappointed at the lack of reaction in the letters page, but, as a regular contributor to historical magazines, I’ve come to expect it. Anyway, if anyone else has read this far, it’s time for the solution and some background notes on the ideas behind it.

As was mentioned last time, the message started life as a rank-2, 4-row by 30?column simple character matrix. The encryption process itself was in two stages, each involving a form of substitution (normally the easiest to crack, but I’ve added a twist). Firstly, I randomly generated a table of 256 2-character codes (using only upper- and lower-case letters, and the numbers 0–9). Then, using the order of occurrence in `⎕AV` to provide indices, I generated a numeric matrix of these indices with the same dimensions as the original text object. So far, so bland.

Now the problem was how to both disguise the original data AND transmit enough of the decryption key for the receiver – which is where my original APL doodling came in. Using a randomly generated number for each element of two vectors with the shape of the first and last dimensions, the ROTATE symbol was then applied, thus ‘jumbling’ the elements – in effect substituting one element for another. So now I had a collection of character tables and numeric vectors which needed to be ‘packaged’ in some regular form, ready for transmission, storage, or publication in the national press (depending on content, of course). As this was to be the ‘simple’ form for Vector competition purposes, the package was assembled as follows:

PART (1) – the table of 2-character codes (or as much as necessary; in the example, only characters, numbers and ONE punctuation symbol were used, or 63 chrs in all);

PART (2) – the three numbers which describe the object’s original dimensions, but expressed as codes from the above table (using the numbers as positional information – 6 chrs);

PART (3) – the 34 numbers which were the rotation figures (using their positions in the code table again; a further 68 chrs);

PART (4) – the 240 characters derived from the 120 code-position numbers which were produced by the original, simple look-up (or in other words, the data – you knew we’d get to it eventually, if only you hung on long enough.......).

The eventual character vector was then split into 5-character sets, so beloved of espionage systems in the 30’s to early 50’s (the gaps easing transmission/ recognition), the remaining odd set being ‘padded’ with randomly generated garbage (as opposed to the sophisticated garbage which preceded it). Now, to transmit the key-table does form a heavy overhead for small messages, but this becomes insignificant as the amount of text increases (as the table is the same size whatever the circumstances). The most obvious feature of the final vector is the unique pairs at the start, as opposed to the later repeating patterns – it is this break which is the best clue to solving this puzzle, the rest being extended game-playing. Interestingly enough, several of the testers who have tried the competition at first rejected some of their results because they were not expecting a mix of numbers and text – they ASSUMED that all results should conform or be significant EITHER as characters OR as text.

This was just the beginning, however – we can get much more devious than this, which is where the title of this piece comes in \[Winston Churchill’s instruction for the protection of the Overlord invasion plans was to “......shield the truth with a bodyguard of lies......”\]. Amongst the other techniques which could be tried, again using the simple application of the rotation operator, are:

- to rotate each of the 5-character sets (thus destroying the obvious unique key at the start);
- to rotate the order of the sets by a given number (either positive or negative);
- to add, somewhere near the start, a large-ish number (perhaps date-based?), together with a reasonably large prime number – this has NO relation to our encryption technique but is used by so many others that automated decryption can go off merrily down the garden path for hours. Still, it keeps the computers busy.........

Of course, once you start to use multiple (optional?) methods, then a further signal needs to sent to the other end indicating which methods apply and, equally important, the ORDER in which they are applied. One suggestion I’d like to make concerns the use of binary-equivalents as disguised selection vectors (that is to say, if methods 1, 2, 4 & 5 apply, this is 1 1 0 1 1 as a selection vector, but can be sent as a single number – or the code table substitute – of 27). This is also a good method for getting rid of the large-number dross mentioned earlier.

Oh, on a final note those with access to speech-to-text phonetics software might like to consider the advantages of using these files as a basis – again very useful in an age of automated decryption where the machine has a copy of the complete Oxford English (or perhaps Oxford Serbian?) Dictionary built-in, but which doesn’t have “Heh-LLohw” \{= Hello\} in its look-up table, and so would reject any method which obtained this text as ‘wrong’ – perhaps semi-logical, lateral humans aren’t redundant quite yet after all....

(webpage generated: 29 October 2007, 17:43)
