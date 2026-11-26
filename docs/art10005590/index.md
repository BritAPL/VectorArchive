---
vid: '10005590'
title: 'At Play With J: We’ll Cross that Bridge when we Come to it'
authors:
- Eugene McDonnell
byline: by Eugene McDonnell
volume: '16'
issue: '3'
page: '126'
source: trad/v163/gene163.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2000
tags:
- competitions and puzzles
- programming techniques
---

*by Eugene McDonnell*
{ .byline }

Bjorn Helgason, from Iceland, submitted this problem to the Internet J Forum:

Four people, A, B, C, and D, come to a bridge at night, with only a flashlight to guide them. The time each one takes to cross the bridge is: A in 1 minute, B in 2 minutes, C in 5 minutes, and D in 10 minutes. The bridge will only support two of them at a time, and the time to cross is, of course, that of the slower walker. The flashlight must be carried on any crossing. They want to get across the bridge as quickly as possible. Since they have a palmtop computer with J installed, they write a program that tells them what the minimum time is, and in what order they should go forth and back over the bridge. Your problem is to write an equivalent program. To help you get started, the program found that the minimum time is seventeen minutes.

The first response to Bjorn’s problem said that there was no need for a program to be written, because it could be solved in one’s head; and complained, furthermore, that seventeen minutes was impossible; it was demonstrable that the minimum time was nineteen minutes: the fastest one, A, going across first with B and the flashlight, leaving B on the other side, coming back with the flashlight to go across with C, then coming back with the flashlight again to go across with D: two plus one plus five plus one plus ten, making nineteen, and that the order was unimportant so long as A was the constant companion. It couldn’t be done more quickly, since the fastest man was always the one to go back. However, Helgason kept insisting that seventeen was the minimum, and offered to send a private message to the complainer giving such a solution.

After a fair amount of back and forth between Helgason and the disbeliever, the issue was resolved when Kirk Iverson finally submitted a solution from Toronto which gave the seventeen-minute solution for the problem. He admitted that he wasn’t sure whether the program would give the minimum answer for all possible combinations of number of people per crossing and times for each to cross, but it would handle many cases. \[Yes, Kirk is related to Ken; he’s a nephew.\] The disbeliever was converted, of course, and made a handsome apology.

See whether you can arrive at a solution, either to this specific problem, or to the more general one solved by Kirk. When you’re satisfied that you either can solve the problem, or have thrown up your hands in frustration, or else are just too lazy to give it any more thought, you can go on to read Kirk Iverson’s solution. From here on, it is his text, slightly amended, with additional comments by me shown in square brackets.

First, the “compiled” code, in case anyone wants to run this without seeing what it does:

```
NB. ---- copy into ijs window and execute --------------------
".(noun define)-.LF
bridge=:(<./@(>@(2&{))({.@],(1&{@],&.><@[),(2&{@]-.&.><@[),3&}
.@],<@[)])^:(*@#@(>@(1&{)))@(((((>@{.@[-.]);>@{:@[)}.@(([-.[-.
])/))@(>@(0&{)(([<.#@]){.])"1(,:|.)@(/:~)@(>@(1&{)))([>@{~(>@(
2&{)@]<&(<./)>@{:@[)+.#@(>@{:@[)=#@(>@(1&{))@])])({.@],(1&{@]-
.&.><@[),(2&{@],&.><@[),3&}.@],<@[)])^:(*@#@(>@(1&{)))^:_&.(({
.;}.;''"_) :.((+/@:(>./@>);])@(3&}.)))@,
)
NB. ----------------------------------------------------------
```

\[I speculate that Kirk first wrote a series of verbs to accomplish the objective, then obtained this unreadable mess by applying the fix adverb (`f.`) to the main verb. Doing this replaced all the subordinate verbs by their definitions, yielding the aforesaid mess. He probably then obtained the character string corresponding to the fixed function, using the `5!:5` form of the foreign conjunction, and displayed this in a field 62 characters wide, giving the six lines shown above, and copied them. The line following the first `NB.` line contains three items that are defined in a script loaded when a J session starts: the variable *noun* has the value `0`; the adverb *define* has the value `: 0` (explicit definition script form) and the variable `LF` is the linefeed character, defined as `0{a.` .The (noun define) in the first line after the comment line (beginning with `NB.`) permits entry of multiple lines of text. Kirk pasted in the six lines, then typed a right parenthesis on the next line, and hit enter, thus causing entry to terminate, and the (noun define) was replaced by the six lines shown. The `-.LF` removed the linefeeds from this text, and the `".` executed the line, causing a verb named bridge to be defined.

I copied the bridge verb, and pasted it into a J session, and it worked as advertised.\]

The left argument to bridge is the maximum number of people who can walk on the bridge at the same time; the right argument is a list of the length of time each person needs to cross.

The result is a boxed list of total elapsed time, followed by all moves:

```
   2 bridge 1 2 4
+-+---+-+---+
|7|1 2|1|4 1|
+-+---+-+---+
```

The result signifies 7 minutes total time; 1 and 2 cross; 1 returns; 4 and 1 cross.

\[This is your last chance to try figuring out how to solve our problem, because the solution Kirk obtained is now going to be shown.\]

<pre>NB. -----------------------------------------------------------------------
NB. Crossing the bridge.

NB.  Rules
NB.     Move all people from one side of bridge to other.
NB.     Each person has an individual time it takes to cross.
NB.     At most MAXLOAD number of people on bridge.
NB.     Torch must always be with a group on the bridge.
NB.     Speed of a group is the speed of the slowest member.
NB.  Strategy
NB.     Overall strategy is to pick a good group to go over, and
NB.     then have the fastest person to return with the torch.  Wash,
NB.     rinse, repeat.  I call the guy to return the torch the "runner".
NB.
NB.     We attempt to move the slowest people together as a group,
NB.     rather than split them up to slow down all groups.  We move
NB.     the slowest ones over whenever there is a suitable runner
NB.     to return, i.e., none of this slow group will have to return.
NB.
NB.     If there is no good runner, we select the fastest to go over
NB.     and act as runners.  We only include enough runners which are
NB.     necessary to bring in the rest of the people.
NB.  Notes
NB.     Iteration is done by repeatedly applying a function to
NB.     the full set of data until it results in everyone moved.
NB.     Data is   maxload;unmoved;moved;move0;move1;move2;...
NB. Verbs to access pieces from the data

maxload=: &gt;@(0&amp;{)   NB. maximum number of people on the bridge
unmoved=: &gt;@(1&amp;{)   NB. people not yet moved
moved=:   &gt;@(2&amp;{)   NB. people already moved
moves=:   3&amp;}.      NB. record of all moves
more=: *@#@unmoved  NB. are there more to move?
NB. move/unmove  move people across bridge
NB. x. is list of people to move; y. is data
move=:   {.@] ,(1&amp;{@] less each &lt;@[),(2&amp;{@]    , each &lt;@[), 3&amp;}.@] , &lt;@[
unmove=: {.@] ,(1&amp;{@]    , each &lt;@[),(2&amp;{@] less each &lt;@[), 3&amp;}.@] , &lt;@[
NB. Strict &lt;less&gt;   Similar to -. but removes from x only the number
NB. of matching items found in y.  All items in y are expected to be
NB. found in x, and the items in the result are in the same order as
NB. they appear in x.  Universe is ~.x, and is catenated in front of
NB. y, therefore count (#/.~) of each returns the counts of the
NB. respective items.  The difference (incremented to compensate for
NB. the catenation of the universe onto y) is used to copy items from
NB. the universe.
        less=: universe #~ [ &gt;:@-&amp;count universe , ]
                universe=: ~.@[
                count=: #/.~
NB. Pick the group to go across.  Runners are the fast group to
NB. shuttle the rest over; waddlers are the slower group put together
NB. to capitalize on the slowest of the bunch.
groups=: sort@unmoved (runners ; waddlers) ]
        runners=: [ maxtake~ required &lt;. maxload@]
                required=: 1: &gt;:@&gt;. #@[ &lt;:@&gt;.@% &lt;:@maxload@]
        waddlers=: maxload@] maxtake |.@[

pickslow=: nogoodrunner +. lastgroup    NB. 0-runners; 1-waddlers
        nogoodrunner=: moved@] &lt;:&amp;fastest slower
        lastgroup=: #@slower = #@unmoved@]
NB. fastest/slowest in a group
fastest=: &lt;./
slowest=: &gt;./
slower=: &gt;@{:@[
maxtake=: ([ &lt;. #@]) {. ]   NB. take, but don't overtake

pick=: groups (pickslow &gt;@{ [) ]
NB. forward - pick group to go and move them
NB. return - pick fastest runner and move back, if more to move
forward=: pick move ]
return=: (fastest@moved unmove ])^:more
NB. iterate repeats the forward/return action until there are no more.
iterate=: return@forward^:more^:_
NB. assemble the argument into the data structure; inverse
NB. computes total trip time from moves, and returns that along
NB. with the moves.
assemble=: ({. ; }. ; ''"_) :. ((+/@:(slowest@&gt;) ; ])@moves)
NB.  maxload play speeds
bridge=: (iterate&amp;.assemble)@,

NB. -----------------------------------------------------------------------
<p>[And now for our problem:]</p>
   2 bridge 1 2 5 10
+--+---+-+----+-+---+
|17|1 2|1|10 5|2|2 1|
+--+---+-+----+-+---+</pre>

The total time is 2+1+10+2+2, or 17, as was required. An alternative would have 2 return the first time, and 1 the second time, and this also is minimum.

Kirk’s J implementation consists of two dozen or so verbs, all in tacit form, so this is a functional programming solution. The iterate verb continues until there is no more to be done, using power to the limit, denoted by infinity (`^:_`).

As this article goes to press, it is recognized that Kirk’s solution is not guaranteed to give the minimum result for all cases; for example, it fails for 1 10 11 12; but it was the first solution that gave the minimum result for the given case 1 2 5 10.

***
