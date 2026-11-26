---
vid: '10002460'
title: How to Rig an Election
authors:
- Ian Clark
byline: 'by Ian A. Clark (email: earthspot@yahoo.com)'
volume: '21'
issue: '3'
page: '63'
source: trad/v213/elect213.htm
converter: vecrescue 0.1.0
issue_tab: n3-may-2005
tags:
- applications
- mathematics and statistics
---

by Ian A. Clark (email: [earthspot@yahoo.com)](mailto:earthspot@yahoo.com)
{ .byline }

My nominations for the two most beneficial inventions of all time are:

- the telephone, because it enables people to talk to each other who hitherto could not,
- the computer, because it enables the people to express their political will where hitherto they could not.

But both inventions have their dark side. The telephone gave rise to the loudspeaker and talking films, exploitation of which in the first half of the 20th century enabled the fascists to win power and consolidate their support in many European countries. In the first half of the 21st century, when computer technology (IT) can be expected to reach maturity, some feel that the world is yet again at risk of absolute power falling into the hands of the first group of people who exploit the new technology to its fullest extent.

The maintenance of democracy in the rapidly-growing population of the USA (for which the VAP or “voting age population” currently stands at around 200 million [\[4\]](#ref4)) has historically depended on IT. An early application of the Hollerith punched card system processed census data and ran elections. But IT, formerly so beneficial, may itself pose a threat to the democratic process if it is allowed either to stagnate in public use or develop out of control.

## The model used in the workspace: VOTE

The workspace VOTE offers a model of the voting process for the newly-enfranchised citizen to play with, in order to gain an understanding of its possibilities and abuses.

Our “toy” democracy has just two parties: the Apples and the Pears. We are to imagine that the Apples are in power and are seeking re-election. To achieve this they must win a majority of seats (one seat per ward, in this simple model). But this time their prospects are not rosy: the Pears have mobilised enough support to outvote them. What can the Apples do to stop the country going pear-shaped?

Well, quite a lot – and not all of it honest campaigning. Though if one party doesn’t put much effort into campaigning, it will warn the other side that it is confident it has things nicely stitched-up. But in the light of what we have to say, the reader may well wonder why any party in power need ever relinquish it. George Orwell [\[1\]](#ref1) thought it could only come about because the party lost the will to govern.

Ignoring countless variations and hybrids, we recognise four basic election rigging techniques:

- R1: Gerrymandering
- R2: Selective disqualification
- R3: Stuffing the ballot box
- R4: Selective discouragement.

We must remember that the lawyers for the Pears know these tricks too and will be watching the Apples closely. So success will largely depend on whether the Apples manage to disguise what they are doing.

VOTE isn’t claimed to be a faithful model of any given democracy. But it does allow us to conduct *in-silico* experiments (as bioengineers call them) on how to rig an election. We can use it to demonstrate a variety of rigging techniques and take a guess at their relative cost-effectiveness.

We will then discuss the morality and the politics of what we have done (which are actually two different things). Demonstrating how to achieve a reprehensible goal is a Machiavellian thing to do. But we should recall that the chief concern of Machiavelli (in [\[2\]](#ref2), if not so evidently in *The Prince*) was to draw the fangs of political abuses he identified in his native land by bringing them out into the open.

## The structure of the workspace: VOTE

Uppercase names like `VOTERS` and `WARDS` are used for global variables, lowercase names like **support** and **vote** for APL functions. We shall refer to these global variables as the *parameters* of the model.

The model manages a collection of “wards”, the number being determined by the value of parameter: WARDS (typically 100). Eschewing nested variables, we set up separate “parallel” arrays: `POPN[i] PRO[i] CON[i]...` all referring to ward `i`.

The workspace VOTE is designed for experimentation by a non-APL programmer, simply by tweaking the parameters. These parameters are initialised by an ordinary textfile (VOTE.INI), which presses into service a simple text editor (MS NOTEPAD) as a rough-and-ready interface. The same parameters have also been brought out to a crude Windows GUI interface, so the model can be adjusted by clicking combos and option buttons, once you understand what they do.

The model breaks down into these main functions, which may be rerun in turn from any stage onwards:

<table border="0" cellpadding="6"><tr>
   <td width="15%"><b>data</b></td>	<td>gets initial values for all model parameters.</td></tr><tr valign="top">
   <td><b>populate</b></td> <td>fills array: <tt>POPN</tt> which counts the voters in each ward.</td></tr><tr valign="top">
   <td><b>support</b></td><td>allocates voters in <tt>POPN</tt> to either <tt>PRO</tt> or <tt>CON</tt>.</td></tr><tr valign="top">
   <td><b>poll</b></td> <td>fills arrays: <tt>VPRO</tt> &amp; <tt>VCON</tt> with votes actually polled, handling turnout, defections and lost ballot-boxes.</td></tr><tr valign="top">
   <td><b>vote</b></td><td>counts, analyses and displays the votes in <tt>VPRO</tt> &amp; <tt>VCON</tt>.</td></tr>
   </table>

Some of these functions are governed by a “mode” parameter, viz `POPULATE_MODE SUPPORT_MODE POLL_MODE` respectively, which select a :Case-block inside the said fn to define different implementations of that function, so they can be viewed as pluggable “building blocks” of the model. Thus when `POPULATE_MODE` equals 1, we say that fn: **populate** is running in mode 1.

## Fn: data

Fn: **data** gets the values of the model parameters from VOTE.INI, echoing them in the APL session log to provide a visual confirmation, This also permits them to be re-entered by someone competent to use the APL session log. Use of the Windows GUI makes the APL session log redundant.

## Fn: populate

In mode 1, fn: *populate* simply assigns a fixed number: `VOTERS` (=1000) to each of the (100) wards by the statement:

<pre><tt>        POPN[]←VOTERS</tt></pre>

In mode 2, *populate* assigns a random number of voters to each ward by Zipf’s Law, which governs the distribution of individuals among such classificatory groupings as that of sharing the same surname. We achieve a Zipfian distribution by:

<pre><tt>     POPN←rounded VOTERS÷⍳WARDS</tt></pre>

The result of this is that if `POPN[1]=VOTERS`, then `POPN[2]=VOTERS÷2, POPN[3]=VOTERS÷3 ... POPN[i]=VOTERS÷i ...` which imposes Zipf’s Law. The fn then goes on to scale-up `POPN` in proportion to give us a total population `(+/POPN)` which is the same as that in mode 1, viz. 100,000 – a value which changes of course if you alter either `VOTERS` or `WARDS`.

## Fn: support

Fn: *support* now allocates voters to `PRO` and `CON`. The workspace currently only models a two-party system, and we are to imagine the Apples are in power. Therefore `PRO` is the number of Apple supporters in each ward and we must suppose everybody else (`CON`) supports the Pears (we shall handle non-voters and defectors later).

This is implemented by the statement:

<pre><tt>        CON←POPN-PRO</tt></pre>

But first we create `PRO`. In mode 1 this is done by:

<pre><tt>        PRO←PERCENT[1] percent_of POPN</tt></pre>

where `PRO[i]` gets a (rounded) fixed percentage of `POPN[i]`.

In mode 2, fn: **support** allocates individuals according to a binomial distribution. This is achieved by fn: **coins**, which postulates that each voter in a ward tosses an unbiased coin to vote.

<pre><tt>        PRO←POPN coins¨ 50</tt></pre>

Notice that for ease of handling the model interface, all probabilities, proportions and tendencies are expressed in percentages, and usually integer percentages at that.

In mode 2 we give ourselves the option to split the total population evenly between PROs and CONs, although some wards may have more of one than the other. Then, as we investigate spooky effects which win the PROs more wards than we think they deserve, we can be sure it’s not due to the PROs having an overall majority as things are. Fn: **balance** sprinkles the necessary additional (or “subtractional”) PRO members among the wards at random. It is switched off by `BALANCE_MODE←0`, and switched on by `BALANCE_MODE←1`.

Further modes offer increasingly complex cases, including ones where the percentage support for the CONs depends on the size of population in the ward. See the partial code listing at the end of the article.

## Fn: poll

Fn: **poll** now determines how the voting goes on polling day. It derives `VPRO` and `VCON` from `PRO` and `CON`, using percentage turnout rates and defections to adjust a random element in the decision of a PRO (or CON) supporter to cast his or her vote.

Mode 1 completes the polling in one shot. It then runs fn: **vote** (see next).

Mode 2 has a more complex behaviour. Here polling is reported and analysed one ward at a time. A combo-controlled timer releases successive results either slowly or fast. The user now has the option of intervening to rig the result according to how badly things are going. See its use below when we discuss voting machines and postal votes.

## Fn: vote

Fn: vote now analyses `VPRO` and `VCON` to report the results. It does not change any arrays, so each time it is run it will give the same result.

## Redrawing boundaries: is it just to ensure fair representation?

Zipf’s Law tends to be obeyed by those population groupings in which the rate of growth of a given group (we are interested in “wards” here) is proportional to its size. This is something that might be expected of wards if they were allowed to grow in an uncontrolled fashion – which in general they are not. Instead most democracies have provisions to redraw ward boundaries every ten years to equalise the populations within them, the idea being to achieve fairer representation of voters in the legislature.

In the USA, by a ruling of the Supreme Court, “districts” (UK: “wards”) must be “re-zoned” (UK: “boundaries redrawn”) every ten years at least. In the UK the Boundaries Commission undertakes this task every ten years. However in the USA it is invariably performed by locally-elected officials or political appointees, who are far from being disinterested agents. In the past, both Democrats and Republicans have been accused of taking advantage of this legal requirement to move enough of their opponents out of a given district to capture it for their own side. Recently, in the State of Texas, Republican-appointed officials were alleged to have done this using sophisticated software, one district (#25) getting itself known as the “Bacon Strip” for its egregious shape. Since 2000, re-zonings in Texas and Pennsylvania have been hotly contested in a series of bizarre actions, including one by Texas Democrats (the “Killer D’s”) who delayed the 2003 Restructuring Bill (which they claim was a Republican gerrymander) by fleeing to neighbouring Oklahoma in order to make the legislature inquorate.

To be fair, Republicans claim the said Bill was necessary to undo a Democrat gerrymander in 2001. But... when does a venial sin become a mortal sin? When it stops being hard work and starts being effective?

Even before the November 2004 elections, the lawyer for the Democratic challenge in the High Court went on record as saying: “it is now possible to take a mildly Democratic state and gerrymander it to return a strong Republican majority”. The Democrats insist that the Republicans have engaged in wholesale gerrymandering across the USA, a project they say has been coordinated by House Leader Tom DeLay. But this will be hard to establish without producing the modern software that did the job.

But there is no doubt such software exists. Cognos Planning, formerly Adaytum Planning, formerly Kunzle Planning System (KPS) [\[7\]](#ref7), is quite up to the job, though it lacks built-in geographic functionality. Of interest to *Vector* readers: this product happens to be written in Dyalog APL. The author would not be at all surprised to learn that this, or a competing product, had been used to gerrymander a re-zoning exercise. What would surprise the author would be to learn that the tool employed had been written in Visual Basic or Java.

Gerrymandering software exploits the unprecedented detail of present-day census data to permit a far more effective gerrymander than hitherto without gross distortion of a district’s boundaries.

## R1: Gerrymandering

The Gerrymander is named after Elbridge Gerry, whose re-zoning activity in the Commonwealth of Massachusetts was so blatant as to create a salamander-shaped district. This was in 1812 – which shows how long the practice has been going on.

Here’s how it works.

Consider two adjacent wards, W1 and W2, which are scheduled for re-zoning. Let’s suppose they have absurdly small populations to highlight the effect, viz. 21 and 33 inhabitants respectively.

This expression models the allegiance of voters to Apples and Pears respectively:

<pre><tt>        +/¨Z←(W1 W2)←(10 11)(16 17)
  21 33
        ×-/¨Z ⍝ 1 supports Apples, ¯1 supports Pears
  ¯1 ¯1</tt></pre>

Notice that both W1 and W2 are currently Pear wards, the Pear majority in each being precisely 1 vote. Now, W2 being the larger ward, (W2-W1)=12, we are legally required to redraw the boundary to even-up the populations by identifying an area containing half that number (6 voters) for transfer from W2 to W1. But suppose we are at liberty to choose these 6 voters with as skewed an allegiance as we can manage, in this case 2 voting Apple and 4 voting Pear. It’s not hard to achieve if (say) Pear voters predominate in poorer neighbourhoods.

This expression performs the required population transfer of 6 voters:

<pre><tt>        +/TRANSFER←2 4
  6
        ⎕←Z←(W1 W2) + (1 ¯1)ר⊂TRANSFER
   12 15  14 13
        ×-/¨Z ⍝ 1 supports Apples, ¯1 supports Pears
  ¯1 1</tt></pre>

but whilst performing our legal duty we have deliberately contrived to capture one of the two Pear wards for the Apples. This is what is meant by gerrymandering.

## R2: Selective disqualification

Selective disqualification is when the Apples try to stop given groups of people from casting votes that count, in a manner which stops more Pears than Apples. This can be by:

- direct targeting, by disqualifying a given group of people from the official lists of those eligible to vote (like those with criminal records)
- indirect targeting, by losing or rejecting all the votes from a given set in which there are more votes cast for the Pears than the Apples (e.g. those cast at a given polling station or voting machine).

Direct targeting is something the Pears will be watching for. The Apples know they won’t get away with disenfranchising someone (striking them from the voters’ list) just because they’ve said they’ll vote Pear. It’s got to be for some reason which most sane people will accept, e.g. the voter is insane.

The 2000 election in Florida was notorious for the poor usability of the voting machinery (e.g. the “butterfly ballot”) and the selectivity of the people affected by this – e.g. older people, the less well-educated, or recent immigrants. When new digital voting machines were installed prior to the 2004 elections, those suspecting a conspiracy had cause to wonder. Was their deployment conspicuously free from political interference? Well – no. What might be going on inside those machines? Hard to say.

The Apples, too, have installed new voting machines and have control over their operation on polling day. The Pears know this, so the Apples deliberately draw their fire by spreading a rumour that the machines contain illicit code to disqualify a selection of Pear votes. They don’t, of course: the political fallout from the Pears finding any such code would be far too great to bear. There’s a simpler trick the Apples can play.

The Apple election boss (you) can trigger a software glitch which disqualifies all the votes from any selected ward that has yet to declare its results. It doesn’t need hi-tech to apply this principle. You can also lose selected ballot boxes, or delay bags of postal votes from overseas. You won’t get away with it on a large scale, but – hey! – the voting machines are new and the occasional glitch may pass scrutiny. Demographic software allows the Apples to predict which wards are likely to fall to the Pears. Those wards which are also likely to deliver their results late in the day are the ones to target.

Fn: **poll** in mode 2 starts a timer which delivers results ward-by-ward, once every two seconds. The GUI’s “delay” combo can speed this up or slow it down. Set the “poll” combo to 2 (signifying mode 2) and click button “poll”. As voting proceeds, the figure labelled “PRO AHEAD/BEHIND” (i.e. &lt;PRO seats> – &lt;CON seats>) executes a drunkard’s walk towards what you’re hoping will end up as a positive number, signifying an Apple victory. If you don’t like the way things are going, then click:“knockout”. This runs fn: **ko**, which disqualifies the ward with the greatest predicted Apple defeat, so it’s worth one ward to you each time you click it.

With judicious use of the “knockout” button you can usually steer the Apples into a marginal victory. You may not have to use it at all, if luck (in the form of “?” or \{roll\}, the APL random number generator) is acting in your favour. If you over-use it and you win by a much greater margin than the exit polls predict, the Pears will ask why so many voting machines went wrong – and they might see a pattern and rumble you.

This is a stealth tactic. It is only feasible because partial results are analysed and broadcast by the networks as they come in, leaving those wards which have yet to declare their results vulnerable to what casinos call “black action”.

## R3: Stuffing the ballot box

You can artificially disqualify your opponents’ voters – and you can artificially inflate your own. The Caltech/MIT study [\[4\]](#ref4) examined 3rd-party registration: there are numerous organisations which register voters en-bloc – people who have not voted before, residents of old-folks’ homes, immigrant groups, etc. – and handle the voting process for them. It stands to reason that such blocs of voters are more likely to vote the way they are told than free independent citizens. There is also the matter of sheer fraud. Over a 20 year period, 5,400 dead people were discovered to have voted in the state of Georgia alone [\[4\]](#ref4), however it’s not reported whether the dead mostly vote Republican or Democrat. One needs a sixth-sense to know that.

Registration is designed to combat blatant fraud, but the problems of scale with the registration system is identified as one of the major issues bedevilling the voting process in the USA. The statistics quoted in [\[4\]](#ref4) are disturbing. Thus the National Voter Registration Act of 1993 (called “Motor Voter ” for its linking of voter registration with the renewal of drivers’ licences) led to a big increase in “inactive” registrants, from 1.7 million in 1994 to 14.6 million in 1998. That’s a lot of spooky voters out there. It just needs some Aragorn to go exploring these “Paths of the Dead” for a legion of hitherto unsuspected supporters to burst upon the scene and tip the balance for one party or the other.

VOTE does not try to model this issue. Mathematically it is a trivial one, but in practice it is vastly important.

## R4: Selective discouragement

Unlike the R3 techniques, seeking to discourage your opponent’s supporters has the advantage of being free from the smell of fraud. But running VOTE in cases where the PROs and CONs are evenly balanced shows just how sensitive the outcome is to even a single-point change in the turnout rate of your supporters. With the model parameters set up as shown in VOTE.INI below, increasing the PROs’ no-vote rate from 10% to 11% (click combo “nv/PRO”, then click “poll” and “vote” again) tends to cost you around 10 wards, the random swing being about 5-6 wards either way, so it signifies almost certain defeat. Tony Blair has recently warned his supporters that the outcome of the May 2005 election is likely to be decided on a few hundred votes in a handful of seats.

The Week [\[3\]](#ref3) quotes Michael Howard’s advisers as telling him he needed just 838,000 people in 165 marginal Labour seats to switch their allegiance from Labour to Conservative in order to win an overall majority. You can be sure that these figures have not been dragged out of the air. But if they are wise they’ll advise him to pursue a policy of discouragement through “negative campaigning” – e.g. impugning the character of his opponents – rather than trying to woo the opposition, to whom he may well have nothing to offer.

The gerrymander in Texas and elsewhere was a costly things to do if its only aim was to capture a few districts. Of far more value was the effect it undoubtedly had on the poorer voters, predominantly Democrat supporters, to convince them that everything was stitched up and they hadn’t a chance of their votes counting (even if they were actually counted!) So it may actually have been an application of R4 rather than R1.

The Apples likewise think it is too costly to woo voters, who might demand that they introduce measures they’d rather not as the price of defection. Each defector is worth two votes, one lost to Pears, one gained by Apples. A couple of Pear supporters who’ve been given the pip is worth exactly the same and is a lot cheaper to achieve. It is known, for instance, that younger people are more likely to be impatient with the status-quo and thus vote for change, so it serves the purpose of the Apples to spread the word that politics is a dirty game and there’s far more fun to be had on a rainy afternoon than queuing for hours at some draughty polling station.

Shady tactics – but is it rigging, or just robust campaigning? Arguably it’s a case of R4, if you use demographic data to target your selective discouragement activities, even if the tactics you then employ are a battery of miscellaneous measures. If you don’t act heavy in too many wards, international observers invited-in from Poland and elsewhere will have little cause to accuse you of “serious” or “widespread” abuse (to them that means bodies with bullets in the back) so R4 can be a stealth tactic too.

## Why should we worry?

In July 2001, the Caltech/MIT Voting Technology Project delivered its report [\[4\]](#ref4) on the state of the nation’s voting arrangements. One of the intriguing things they found was that the number of lost votes across the nation was 2%-3% in presidential elections, and they compared this to the fact that in 2000 the winner’s margins in 4 states: Florida, Iowa, New Mexico and Wisconsin, was less than 0.5%. In response the federal government saw fit to mount a massive program to restore public confidence in the electoral process (HAVA: Help America Vote Act, 2002 [\[5\]](#ref5)).

There will be calls for much the same in Britain after the May 2005 elections. But perhaps we should not be demanding yet another exercise in restoring public confidence, but instead aim to construct a voting system which merits our confidence. This should not be beyond the wit of man to devise, considering the clever things that man can already do to subvert the existing apparatus. We should consider whether the following activities are now so dangerous to our democratic way of life, thanks to advances in IT, that they should either be outlawed or be strictly reserved to an impartial and independent agency:

- third-party registration of voters (R2, R3)
- collecting and processing data on individuals’ party-political preferences and voting behaviour (R1, R2, R3, R4)
- partial publication of results while voting is underway (R2, R3)
- exit polls and other unofficial polls while voting is underway (R4)
- issuing new postal votes (R2, R3)
- canvassing postal votes from people who have never yet voted (R2, R3)
- publicly inviting certain classes of people to stay away from the polling booths on voting day (R2, R3, R4)
- publicly impugning the character (as distinct from the policies) of a candidate in the two weeks preceding an election (R4).

After each suggestion the rigging techniques it combats are shown in brackets.

## Conclusion

Democracy. For all its imperfections, it is still the most effective way mankind has discovered to curb the persistent abuse of power – and free and fair elections are the sole means to preserve it. You either believe this or you don’t.

Loss of belief in it led Benito Mussolini, an ardent socialist, to forsake the people’s cause and become the first fascist. He was, and still remains, fascism’s most articulate theorist, in spite of his more notorious (and undeniably articulate) imitator in Germany. It is good to read Mussolini’s article [\[6\]](#ref6) defining the nature of fascism: the notion being touted in the USA of running government like a business corporation is clearly seen to be his – and it is a central one to his ideas.

But the democratic process, as we show with our VOTE model, is a chain which is no stronger than its weakest link. We should not wait for voting abuses to become “widespread” before we start to worry, but attack irregularities wherever they occur, not leave the fight to a few locals who realise what is going on. Otherwise who’s to blame if we wake up to find ourselves no longer living in a democracy, but an oligarchy flaunting a few democratic customs?

There is no such thing as a small fire in the kitchen. There is a major case for stern measures whenever voting abuses occur: they should be investigated for complicity at the highest level – and those found guilty never again entrusted with public office. This may mean disqualifying one or two charismatic leaders, but others will rise to fill their places. All too readily, the leaders may think. We should reject the claim some will make that the brilliant brains of certain individuals would be a loss to the nation – this is to succumb to the cult of personality. It must remain a central principle of democracy that the will of the people, fairly expressed, takes precedence over the will of any one individual, however well-endowed, well-educated, or well-connected.

## References

<a id="ref1"></a>\[1\] Orwell, George (1948), Nineteen Eighty-Four. [http://www.abebooks.com](http://www.abebooks.com)

<a id="ref2"></a>\[2\] Machiavelli, Niccolò, Discourses on Livy. University of Chicago Press (1996).  
ISBN 0-226-50036-5

<a id="ref3"></a>\[3\] THE WEEK (9 Apr 2005) Blair calls an election, p3. [http://www.theweek.co.uk](http://www.theweek.co.uk)

<a id="ref4"></a>\[4\] Caltech/MIT Voting Technology Project (2001), Voting: What Is /What Could Be. [http://www.vote.caltech.edu](http://www.vote.caltech.edu)

<a id="ref5"></a>\[5\] Help America Vote Act (2002). [http://www.fec.gov/hava/hava.htm](http://www.fec.gov/hava/hava.htm)

<a id="ref6"></a>\[6\] Mussolini, Benito, (1932), The Doctrine Of Fascism. [http://www.worldfuturefund.org/wffmaster/Reading/Germany/mussolini.htm](http://www.worldfuturefund.org/wffmaster/Reading/Germany/mussolini.htm)

<a id="ref7"></a>

\[7\] Clark, I.A. (1996), KPS: Beyond the Spreadsheet. VECTOR 12.3, p55.

***

## Partial listing of the VOTE model

VOTE is available by [email](mailto:earthspot@yahoo.com) from the author. It is a workspace running under APL+Win v4.

<pre><tt>
    ∇ populate;totpop
[1]   ⍝populate the wards
[2]   POPN←PRO←CON←UND←WARDS⍴0 ⍝--setup the working arrays
[3]
[4]   :Select POPULATE_MODE
[5]
[6]   :Case 1
[7]       mode 'equal number of voters in each ward'
[8]       POPN[]←VOTERS
[9]
[10]  :Case 2
[11]      mode 'ward populations skewed by Zipf Law'
[12]      totpop←WARDS×VOTERS
[13]      POPN←rounded VOTERS÷⍳WARDS ⍝--Zipf
[14]      POPN←rounded POPN×(totpop÷+/POPN) ⍝--boost in proportion
[15]      POPN←totpop padup POPN ⍝--make up the small discrepancy to totpop
[16]  ⍝    POPN←shuffled POPN
[17]      POPN←⌽POPN ⍝--biggest at the back, for ko to work well
[18]
[19]  :EndSelect
    ∇

    ∇ support;t;z
[1]   ⍝allocate support for/against
[2]   :Select SUPPORT_MODE
[3]
[4]   :Case 1
[5]       mode 'fixed % of voters in each ward are PRO'
[6]       PRO←PERCENT[1] percent_of POPN
[7]
[8]   :Case 2
[9]       mode 'balanced: all voters toss a balanced coin'
[10]      PRO←POPN coins¨ 50
[11]      balance
[12]
[13]  :Case 3
[14]      mode 'fixed % support for PRO &amp; CON, residue toss (fixed-biassed) coin'
[15]      PRO←PERCENT[1] percent_of POPN
[16]      z←POPN-PERCENT[2] percent_of POPN
[17]      PRO←PRO+(z coins¨ TENDENCY[1])
[18]
[19]  :Case 4
[20]      mode 'max % PRO support irrespective of size of ward'
[21]      PRO[]←POPN ⌊ PERCENT[1] percent_of VOTERS
[22]
[23]  :Case 5
[24]      mode 'fixed PRO support is all of smallest ward'
[25]      PRO[]←⌊/POPN
[26]
[27]  :Case 6
[28]      mode 'voters toss a coin biassed by size of ward'
[29]      PRO←POPN coins¨ TENDENCY percent_by POPN
[30]
[31]  :EndSelect
[32]
[33]  CON←POPN-PRO ⍝--CON is everybody else (2-party only)
[34]
[35]  report logex '+/PRO' '+/CON'
    ∇

    ∇ poll
[1]   ⍝conduct the poll (c/f: vote)
[2]   default'POLL_MODE←1'
[3]   mode 'conducting the poll'
[4]   pll POLL_MODE
    ∇

    ∇ pll flag;z
[1]   ⍝send voters to the polls, in mode: flag
[2]   ⍝(called by: poll. Runs: vte, timer)
[3]   ⍝NB: Cases 1 and 2 are POLL_MODE settings, for calling with a single button
[4]   ⍝ Other cases are service-calls, recursive calls, or calls by the timer
[5]
[6]   :Select flag
[7]
[8]   :Case 1 ⍝--one-shot poll (POLL_MODE setting)
[9]       pll "vars"
[10]      vote
[11]
[12]  :Case 2 ⍝--stepped poll (POLL_MODE setting)
[13]      timer "toggle" ⍝--timer callback fn: timer_action calls: pll
[14]
[15]  :Case "vars" ⍝--compute working vars
[16]      z←DEFECT+NOVOTE
[17]      NPRO←PRO coins¨ 1⊃z ⍝--loss to PRO by defection &amp; non-voting
[18]      NCON←CON coins¨ 2⊃z ⍝--loss to CON by defection &amp; non-voting
[19]      z←100×DEFECT÷z ⍝--of which defectors AS A %age
[20]      DP2C←NPRO coins¨ 1⊃z ⍝--of which: defections PRO→CON
[21]      DC2P←NCON coins¨ 2⊃z ⍝--of which: defections CON→PRO
[22]      VPRO←0⌈PRO+DC2P-NPRO ⍝--actual turnout in favour of PRO
[23]      VCON←0⌈CON+DP2C-NCON ⍝--actual turnout in favour of CON
[24]      (VPRO0 VCON0)←VPRO VCON ⍝--saved for debugging: ko
[25]
[26]  :Case "part" ⍝--zero the future part of VRPO VCON...
[27]      (VPRO1 VCON1)←WARDS↑¨NTICK↑¨(VPRO VCON)
[28]      vte 2 ⍝--partial estimate
[39]
[30]  :Case "final"
[31]      vte 1
[32]      timer "stop"
[33]
[34]  :Endselect
    ∇

    ∇ ko;i
[1]   ⍝discard votes from ward with biggest CON majority
[2]   i←↑⍒VCON-VPRO
[3]
[4]   :If VPRO[i]=0
[5]   :AndIf VCON[i]=0
[6]       report myname "&gt;&gt;&gt; no votes anyway from ward:" i
[7]   :Else
[8]       timer "pause"
[9]       VPRO[i]←VCON[i]←0
[10]      report myname "&gt;&gt;&gt; all votes discarded from ward:" i
[11]  :End
    ∇</tt></pre>

## Contents of: VOTE.INI as distributed

<pre><tt>
[GENERAL]
WARDS=100

VOTERS=1000
;--basic number of voters in each ward

INTERVAL=2000
;--stepping interval in millisecs

[PERCENT]
;Vector %s for allegiance turnout and defection of PRO &amp; CON respectively...
PERCENT=50 50
NOVOTE=10 10
DEFECT=5 5
TENDENCY=50 50
;--tendency to vote PRO (for smallest/largest wards)

[MODE]
BALANCE_MODE=1
POPULATE_MODE=2
SUPPORT_MODE=2
POLL_MODE=1</tt></pre>

***
