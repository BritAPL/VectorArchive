---
vid: '10500370'
title: 'Functional Calculation 4: The year 1998'
subtitle: '4: The Year 1998'
authors:
- Neville Holmes
byline: Neville Holmes(Neville.Holmes@utas.edu.au)
volume: '24'
issue: '4'
page: '110'
online: '2010-06-22'
description: The fifth instalment of the author’s series “Functional calculation in J”
abstract: Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. In preceding articles an introduction was given to what could be done with one commonly available tool for functional calculation, using a notation called J, then details were given of simple numerical calculation, and then simple structural calculation. This article is intended to allow the reader to consider how simple structural calculation can be done in J by showing numeric expressions to produce whole numbers below 100 starting from the enlisted digits of the number 1998.
source: content/printed/244/holmesfc4.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2010
tags:
- education
- mathematics and statistics
---

4: The Year 1998
{ .subtitle }

Neville Holmes  
(Neville.Holmes@utas.edu.au)
{ .byline }

Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. In preceding articles an introduction was given to what could be done with one commonly available tool for functional calculation, using a notation called J, then details were given of simple numerical calculation, and then simple structural calculation. This article is intended to allow the reader to consider how simple structural calculation can be done in J by showing numeric expressions to produce whole numbers below 100 starting from the enlisted digits of the number 1998.
{ .abstract }

## Calculation

Preceding articles have been introducing numerical calculation and structural calculation using the interpreter for the J notation. This article reverts to the pattern of the third one, showing how the whole numbers from 0 to 99 can be constructed from the digits of a year ‒ the year 1998. This time, however, digits are to be used in lists or in a list, so that the structural functions reviewed in the previous articles can be used.

Of course, the scalar functions used in the previous exercises can also be used here, but the following structural functions are to be preferred. The first table shows structural functions used for building lists. These are a fairly mixed bunch, and indeed the shape and tally functions are not actually used for building but for reporting what has been built.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>$</code></td><td>shape</td><td>reshape</td>
				</tr>
				<tr>
					<td><code>#</code></td><td>tally</td><td>copy</td>
					<td><code>#.</code></td><td>unbits</td><td>undigits</td>
					<td><code>#:</code></td><td>bits</td><td>digits</td>
				</tr>
				<tr>
					<td><code>?</code></td><td>deal</td>
				</tr>
				<tr>
					<td><code>&lt;</code></td><td>box</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>+.</code></td><td>cartesian</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>":</code></td><td>format</td><td>format</td>
				</tr>
				<tr>
					<td><code>&gt;</code></td><td>unbox</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>*.</code></td><td>polar</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>;:</code></td><td>words</td>
				</tr>
				<tr>
					<td><code>;</code></td><td>raze</td><td>link</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>i.</code></td><td>integers</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>q:</code></td><td>factors</td>
				</tr>
			</tbody>
		</table>

The second table shows structural functions used for extracting or rearranging the values in a list. Again, these functions are a mixed bunch and not all will be found useful in the task of generating numbers.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td></td><td></td><td></td>
					<td><code>|.</code></td><td>reverse</td><td>rotate</td>
					<td><code>|:</code></td><td>transpose</td><td>transpose</td>
				</tr>
				<tr>
					<td><code>,</code></td><td>ravel</td><td>append</td>
					<td><code>,.</code></td><td>knit</td><td>stitch</td>
					<td><code>,:</code></td><td>itemise</td><td>laminate</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>/:</code></td><td>sort up</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>\:</code></td><td>sort down</td>
				</tr>
				<tr>
					<td><code>{</code></td><td>from</td><td></td>
					<td><code>{.</code></td><td>head</td><td>take</td>
					<td><code>{:</code></td><td>tail</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>}.</code></td><td>behead</td><td>drop</td>
					<td><code>}:</code></td><td>curtail</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>~.</code></td><td>nub</td>
				</tr>
			</tbody>
		</table>

The third table shows some miscellaneous functions which extract data about their arguments.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td></td><td></td><td></td>
					<td><code>%.</code></td><td>invert</td><td>project</td>
				</tr>
				<tr>
					<td><code>=</code></td><td>classify</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>-:</code></td><td>match</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>e.</code></td><td>raze in</td><td>member</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>~:</code></td><td>sieve</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td><code>i.</code></td><td>index</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>/:</code></td><td>grade up</td>
				</tr>
				<tr>
					<td></td><td></td><td></td>
					<td></td><td></td><td></td>
					<td><code>\:</code></td><td>grade down</td>
				</tr>
			</tbody>
		</table>

Finally, there are two operations which are so useful for working with structures that it would be masochistic to ignore them. Both these operations, whose symbols are `~` and `/`, are monadic, which means that they suffix a function which is the target of their operation. The resulting function may be used monadically or dyadically.

For example, `2%~3` will divide `2` *into* `3` rather than *by* `3` (its arguments are reversed), while `ˆ~3` raises `3` to the power `3` (its left argument is copied from its right argument).

Further, `+/i.100` will add up the first hundred integers, `2 3+/4 5 6` will give a table with two rows and three columns containing the sum of each of `2 3` with each and every `4 5 6`, and `*/~i.12` will give a ‘times’ table for the first twelve integers, or rather `*/~>:i.12` will give the more familiar table.

## Making 1998 give 0 to 19

The four digits of 1998 may be used as a single list, that is, as `1 9 9 8`, `19 9 8`, `1 99 8`, `19 9 8`, `199 8`, `19 98`, or `1 998`. Otherwise two lists may be used, that is, the two lists `1 9` and `9 8`, or a scalar and a list, such as `1` and the list `9 98` or as the list `1 9 9` and the scalar `8`.

The task is to combine them in as short and simple an expression as possible, to yield each of the numbers between `0` and `19` (here, `99` later), and to yield them as scalars.

As a matter of aesthetics, parentheses are avoided as far as possible. Also as a matter of style, the negative sign is avoided and subtraction or negation is used to similar effect.

To save listing space, in the following it will be assumed that

```
                 x =: 1 9 9 8
```

has been issued. Expressions that use `x` are preferred as using the simplest but most extensive list of integers.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>0</code></td>
					<td><code>|/x</code></td>
					<td><code>=/19 98</code></td>
					<td><code>10</code></td>
					<td><code>{.1 9+9 8</code></td>
					<td><code>#1 9":9 8</code></td>
				</tr>
				<tr>
					<td><code>1</code></td>
					<td><code>1[9 9 8</code></td>
					<td><code>&lt;./x</code></td>
					<td><code>11</code></td>
					<td><code>--~/x</code></td>
					<td><code>+/*:i.~x</code></td>
				</tr>
				<tr>
					<td><code>2</code></td>
					<td><code>1 9 i.98</code></td>
					<td><code>#1 9;9 8</code></td>
					<td><code>12</code></td>
					<td><code>-.-~/x</code></td>
					<td><code>+/19 9-8</code></td>
				</tr>
				<tr>
					<td><code>3</code></td>
					<td><code>1 9 9 i.8</code></td>
					<td><code>#~.x</code></td>
					<td><code>13</code></td>
					<td><code>&lt;.+/-:x</code></td>
					<td><code>&gt;.&gt;./ˆ.!x</code></td>
				</tr>
				<tr>
					<td><code>4</code></td>
					<td><code>#x</code></td>
					<td><code>+/*x</code></td>
					<td><code>14</code></td>
					<td><code>&gt;.*/ˆ.19 9 8</code></td>
					<td><code>+/&gt;:19 9-8</code></td>
				</tr>
				<tr>
					<td><code>5</code></td>
					<td><code>+/i.~x</code></td>
					<td><code>&gt;.{:!%:x</code></td>
					<td><code>15</code></td>
					<td><code>#.*x</code></td>
					<td><code>19|*/9 8</code></td>
				</tr>
				<tr>
					<td><code>6</code></td>
					<td><code>+//:x</code></td>
					<td><code>&gt;./!%:x</code></td>
					<td><code>16</code></td>
					<td><code>#./:x</code></td>
					<td><code>+/19 9|8</code></td>
				</tr>
				<tr>
					<td><code>7</code></td>
					<td><code>-/|.x</code></td>
					<td><code>#":x</code></td>
					<td><code>17</code></td>
					<td><code>+/1 9]9 8</code></td>
					<td><code>&gt;.%/19 9 8</code></td>
				</tr>
				<tr>
					<td><code>8</code></td>
					<td><code>{:x</code></td>
					<td><code>%%/x</code></td>
					<td><code>18</code></td>
					<td><code>+/~.x</code></td>
					<td><code>-/19 9 8</code></td>
				</tr>
				<tr>
					<td><code>9</code></td>
					<td><code>&gt;./x</code></td>
					<td><code>1{9 9 8</code></td>
					<td><code>19</code></td>
					<td><code>+/}:x</code></td>
					<td><code>{.19 9 8</code></td>
				</tr>
			</tbody>
		</table>

Two expressions are given in the table for each number. Note carefully that at least one of the arguments in each expression is a list.

## Making 1998 give 20 to 99

Making numbers beyond `19` follows a similar pattern, and it’s convenient here to take them twenty at a time.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>20</code></td>
					<td><code>--~/19 9 8</code></td>
					<td><code>#.&lt;:-:x</code></td>
					<td><code>30</code></td>
					<td><code>#.~.x</code></td>
					<td><code>+/1,&gt;:9 8</code></td>
				</tr>
				<tr>
					<td><code>21</code></td>
					<td><code>19+#9 8</code></td>
					<td><code>&lt;.+/|1 9 j.9 8</code></td>
					<td><code>31</code></td>
					<td><code>#.}:x</code></td>
					<td><code>+/&gt;:x</code></td>
				</tr>
				<tr>
					<td><code>22</code></td>
					<td><code>#.\:x</code></td>
					<td><code>+/,~&gt;:#.x</code></td>
					<td><code>32</code></td>
					<td><code>-:*/19 9|8</code></td>
					<td><code>*/&lt;.+:%1 o.9 9 8</code></td>
				</tr>
				<tr>
					<td><code>23</code></td>
					<td><code>+/&lt;:x</code></td>
					<td><code>p:{:x</code></td>
					<td><code>33</code></td>
					<td><code>+/&lt;:19 9 8</code></td>
					<td><code>#":*:*:,~x</code></td>
				</tr>
				<tr>
					<td><code>24</code></td>
					<td><code>#.1 9 9|8</code></td>
					<td><code>!-:{:x</code></td>
					<td><code>34</code></td>
					<td><code>&lt;.!%:+:&gt;/x</code></td>
					<td><code>+/,~}.19 9 8</code></td>
				</tr>
				<tr>
					<td><code>25</code></td>
					<td><code>--~/|.x</code></td>
					<td><code>&lt;.*/%:x</code></td>
					<td><code>35</code></td>
					<td><code>+/,q:19 98</code></td>
					<td><code>#.-:x</code></td>
				</tr>
				<tr>
					<td><code>26</code></td>
					<td><code>&lt;.#.%:19 9 8</code></td>
					<td><code>1#.9 9 8</code></td>
					<td><code>36</code></td>
					<td><code>+/19 9 8</code></td>
					<td><code>&lt;/*/%:19 9 8</code></td>
				</tr>
				<tr>
					<td><code>27</code></td>
					<td><code>+/x</code></td>
					<td><code>19+{:9 8</code></td>
					<td><code>37</code></td>
					<td><code>&gt;.*/%:19 9 8</code></td>
					<td><code>#.1 9+9 8</code></td>
				</tr>
				<tr>
					<td><code>28</code></td>
					<td><code>{.+/1 9+/9 8</code></td>
					<td><code>+//:,~x</code></td>
					<td><code>38</code></td>
					<td><code>{.+:19 9 8</code></td>
					<td><code>+/19&gt;&gt;.9 8</code></td>
				</tr>
				<tr>
					<td><code>29</code></td>
					<td><code>&gt;./p:x</code></td>
					<td><code>+/1+9 9 8</code></td>
					<td><code>39</code></td>
					<td><code>+/&gt;:19 9 8</code></td>
					<td><code>-:-&gt;:-/19 98</code></td>
				</tr>
			</tbody>
		</table>

Once into the twenties, the integer `19` becomes very useful, though it was also handy in the teens.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>40</code></td>
					<td><code>&lt;.*/-:x</code></td>
					<td><code>+/-:&lt;:p:x</code></td>
					<td><code>50</code></td>
					<td><code>&lt;.+:%:*/x</code></td>
					<td><code>#.&gt;:-:x</code></td>
				</tr>
				<tr>
					<td><code>41</code></td>
					<td><code>&lt;.!%:{.19 9 8</code></td>
					<td><code>-/1 9,-:98</code></td>
					<td><code>51</code></td>
					<td><code>#.-:19 9 8</code></td>
					<td><code>-:1+99+#,~8</code></td>
				</tr>
				<tr>
					<td><code>42</code></td>
					<td><code>+/#.|:+:#:x</code></td>
					<td><code>+/-:p:x</code></td>
					<td><code>52</code></td>
					<td><code>+/+:}.x</code></td>
					<td><code>&lt;.+/!%:19 9 8</code></td>
				</tr>
				<tr>
					<td><code>43</code></td>
					<td><code>&lt;:+/+:+/q:&gt;:x</code></td>
					<td><code>+/1 9 9+8</code></td>
					<td><code>53</code></td>
					<td><code>&gt;./&gt;.!-:x</code></td>
					<td><code>1{+/+/~9 9 8</code></td>
				</tr>
				<tr>
					<td><code>44</code></td>
					<td><code>+/1 9,,~9 8</code></td>
					<td><code>+/19 9+8</code></td>
					<td><code>54</code></td>
					<td><code>&lt;.ˆ#x</code></td>
					<td><code>+/+:x</code></td>
				</tr>
				<tr>
					<td><code>45</code></td>
					<td><code>#.#.=x</code></td>
					<td><code>*/&gt;.%:19 9 8</code></td>
					<td><code>55</code></td>
					<td><code>+/19+9 8</code></td>
					<td><code>#.&lt;:x</code></td>
				</tr>
				<tr>
					<td><code>46</code></td>
					<td><code>+:p:{:x</code></td>
					<td><code>+/}:,~x</code></td>
					<td><code>56</code></td>
					<td><code>&lt;.o.-/19 9 8</code></td>
					<td><code>+/,~}:19 9 8</code></td>
				</tr>
				<tr>
					<td><code>47</code></td>
					<td><code>&lt;.*/+:%1 o.9 9 8</code></td>
					<td><code>p:+:#":x</code></td>
					<td><code>57</code></td>
					<td><code>1+*./&lt;:9 9 8</code></td>
					<td><code>+/19+&gt;:9 8</code></td>
				</tr>
				<tr>
					<td><code>48</code></td>
					<td><code>*/#.=x</code></td>
					<td><code>#.&lt;.-:19 9 8</code></td>
					<td><code>58</code></td>
					<td><code>+/&gt;:+:x</code></td>
					<td><code>&lt;:#.}:|.x</code></td>
				</tr>
				<tr>
					<td><code>49</code></td>
					<td><code>-:{:1 9 98</code></td>
					<td><code>-/*:1-9 9 8</code></td>
					<td><code>59</code></td>
					<td><code>-:&gt;:+/19 98</code></td>
					<td><code>+/1 9,-:98</code></td>
				</tr>
			</tbody>
		</table>

From the forties on, the integer `98` comes into play more often.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>60</code></td>
					<td><code>-:#.1 9 98</code></td>
					<td><code>&gt;.o.{19 9 8</code></td>
					<td><code>70</code></td>
					<td><code>#.x</code></td>
					<td><code>&lt;:p:&gt;:-/19 9 8</code></td>
				</tr>
				<tr>
					<td><code>61</code></td>
					<td><code>#.1 C.9 9 8</code></td>
					<td><code>+/p:}:x</code></td>
					<td><code>71</code></td>
					<td><code>+/1 9#&lt;:9 8</code></td>
					<td><code>p:{.19 9 8</code></td>
				</tr>
				<tr>
					<td><code>62</code></td>
					<td><code>1*#.9 9 8</code></td>
					<td><code>#.}.x</code></td>
					<td><code>72</code></td>
					<td><code>*./x</code></td>
					<td><code>&gt;./1 9*9 8</code></td>
				</tr>
				<tr>
					<td><code>63</code></td>
					<td><code>-/*:|.x</code></td>
					<td><code>-/p:&lt;:19 9 8</code></td>
					<td><code>73</code></td>
					<td><code>1+*./9 9 8</code></td>
					<td><code>p:19+##9 8</code></td>
				</tr>
				<tr>
					<td><code>64</code></td>
					<td><code>|/*:19 9 8</code></td>
					<td><code>-.-/*:x</code></td>
					<td><code>74</code></td>
					<td><code>+/#.|:+:&gt;:#:~.x</code></td>
					<td><code></code></td>
				</tr>
				<tr>
					<td><code>65</code></td>
					<td><code>+/*:1 9-9 8</code></td>
					<td><code>-/p:19 9 8</code></td>
					<td><code>75</code></td>
					<td><code>19+*/&lt;:9 8</code></td>
					<td><code>#.,~1 9-~9 8</code></td>
				</tr>
				<tr>
					<td><code>66</code></td>
					<td><code>+/+:&lt;:19 9 8</code></td>
					<td><code>#.19 _9 8</code></td>
					<td><code>76</code></td>
					<td><code>+/1 9 9#-:8</code></td>
					<td><code>+/1 9 9*-:8</code></td>
				</tr>
				<tr>
					<td><code>67</code></td>
					<td><code>p:-/19 9 8</code></td>
					<td><code>#.-:&gt;:|.x</code></td>
					<td><code>77</code></td>
					<td><code>|-/*:&gt;:x</code></td>
					<td><code>+/,1 9$9 8</code></td>
				</tr>
				<tr>
					<td><code>68</code></td>
					<td><code>-:#.19 98</code></td>
					<td><code>+/,1 9$&lt;:9 8</code></td>
					<td><code>78</code></td>
					<td><code>-.-/*:&gt;:x</code></td>
					<td><code>+:+/&gt;:19 9 8</code></td>
				</tr>
				<tr>
					<td><code>69</code></td>
					<td><code>#.|.19 9 8</code></td>
					<td><code>#.1+9 9 8</code></td>
					<td><code>79</code></td>
					<td><code>-~/19 98</code></td>
					<td><code>1 9#.&lt;:9 8</code></td>
				</tr>
			</tbody>
		</table>

In the sixties and seventies, squaring (`*:`) becomes useful.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>80</code></td>
					<td><code>*/&lt;:+/&gt;:|:=x</code></td>
					<td><code>-.-/19 98</code></td>
					<td><code>90</code></td>
					<td><code>-/1 9 98</code></td>
					<td><code>*./&gt;:x</code></td>
				</tr>
				<tr>
					<td><code>81</code></td>
					<td><code>1 9#.|.9 8</code></td>
					<td><code>+/1 9#9 8</code></td>
					<td><code>91</code></td>
					<td><code>-/1+99 8</code></td>
					<td><code>19+*/9 8</code></td>
				</tr>
				<tr>
					<td><code>82</code></td>
					<td><code></code></td>
					<td><code>+/1 9,*/9 8</code></td>
					<td><code>92</code></td>
					<td><code>-/&gt;:-:1 99 8</code></td>
					<td><code>+/&lt;:p:&gt;:x</code></td>
				</tr>
				<tr>
					<td><code>83</code></td>
					<td><code>+/&lt;:#.&gt;:#:x</code></td>
					<td><code>#.&lt;:#.+:=x</code></td>
					<td><code>93</code></td>
					<td><code>-.-~/1 99 8</code></td>
					<td><code>*/p:1 9!&gt;:9 8</code></td>
				</tr>
				<tr>
					<td><code>84</code></td>
					<td><code>%:*/+:1 9 98</code></td>
					<td><code>+/p:x</code></td>
					<td><code>94</code></td>
					<td><code></code></td>
					<td><code>gt;.-/%-.1 o.9 9 8</code></td>
				</tr>
				<tr>
					<td><code>85</code></td>
					<td><code>#.&gt;:x</code></td>
					<td><code>-:+/,1 9*/9 8</code></td>
					<td><code>95</code></td>
					<td><code>&lt;:*/&gt;.%:&gt;:x</code></td>
					<td><code>.&lt;:19 9 8</code></td>
				</tr>
				<tr>
					<td><code>86</code></td>
					<td><code></code></td>
					<td><code>/,1 9$&gt;:9 8</code></td>
					<td><code>96</code></td>
					<td><code>-:&gt;:-/199 8</code></td>
					<td><code>*./#.*:+:=x</code></td>
				</tr>
				<tr>
					<td><code>87</code></td>
					<td><code></code></td>
					<td><code>/#.&gt;:#:x</code></td>
					<td><code>97</code></td>
					<td><code>&lt;:1 9]98</code></td>
					<td><code>#.&gt;:#.+:=x</code></td>
				</tr>
				<tr>
					<td><code>88</code></td>
					<td><code>&lt;.+/&gt;:o.x</code></td>
					<td><code>+/&gt;:p:x</code></td>
					<td><code>98</code></td>
					<td><code>{:1 9 98</code></td>
					<td><code>+/,1 9*./9 8</code></td>
				</tr>
				<tr>
					<td><code>89</code></td>
					<td><code>1 9#.9 8</code></td>
					<td><code>-/1-9 98</code></td>
					<td><code>99</code></td>
					<td><code>1{.99 8</code></td>
					<td><code>1 9#.&gt;:9 8</code></td>
				</tr>
			</tbody>
		</table>

Once in eighties, simpler expressions become possible because integers like `98`, `99`, and `199` can be brought into play.

## Further examples

The examples given above can only suggest how arithmetic functions can be used in a simple way to produce a variety of numbers. The reader is urged to consider the examples above with a J interpreter to hand, to try the examples out, to check them, and to try to find expressions that are better or in some way more interesting than those given here. When generating these numbers begins to pall, the reader perhaps should go on to consider how to generate the three digit numbers using the same rules. This could start `+/}:1 99 8` then `+/1 99,*8`.

Alternatively, expressions might be sought for other years. Some years will present special challenges. The following table gives a start for the year `2000`, in which a choice is made between `0=0`,<code> 0!0 <code>and</code> 0ˆ0 </code>to give a `1` largely on æsthetic grounds.

In the following table it has been assumed that

```
                 x =: 2 0 0 0
```

has been issued.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>0</code></td>
					<td><code>&lt;./x</code></td>
					<td><code>*/x</code></td>
					<td><code>10</code></td>
					<td><code>*/*:&gt;:~.x</code></td>
					<td><code>20%#0 0</code></td>
				</tr>
				<tr>
					<td><code>1</code></td>
					<td><code>|./x</code></td>
					<td><code>2 0 0 i.0</code></td>
					<td><code>11</code></td>
					<td><code>&gt;.+/ˆx</code></td>
					<td><code>#.2 0 0&gt;.-.0</code></td>
				</tr>
				<tr>
					<td><code>2</code></td>
					<td><code>+/x</code></td>
					<td><code>-/x</code></td>
					<td><code>12</code></td>
					<td><code>+/*:&gt;:x</code></td>
					<td><code>+/,&gt;:=x</code></td>
				</tr>
				<tr>
					<td><code>3</code></td>
					<td><code>&gt;:{.x</code></td>
					<td><code>*/&gt;:x</code></td>
					<td><code>13</code></td>
					<td><code>&gt;.+/o.&gt;:x</code></td>
					<td><code>20-&lt;.ˆ+/&gt;:0 0</code></td>
				</tr>
				<tr>
					<td><code>4</code></td>
					<td><code>#x</code></td>
					<td><code>+/*:x</code></td>
					<td><code>14</code></td>
					<td><code>#.2+0 0 0</code></td>
					<td><code>20-&lt;.o.+/&gt;:0 0</code></td>
				</tr>
				<tr>
					<td><code>5</code></td>
					<td><code>2+#0 0 0</code></td>
					<td><code>2++/&gt;:0 0 0</code></td>
					<td><code>15</code></td>
					<td><code>+/&lt;.o.&gt;:}:x</code></td>
					<td><code>#.=~x</code></td>
				</tr>
				<tr>
					<td><code>6</code></td>
					<td><code>+/&gt;:x</code></td>
					<td><code>+/2+0 0 0</code></td>
					<td><code>16</code></td>
					<td><code>#.x</code></td>
					<td><code>ˆ/2&gt;.0 0 0</code></td>
				</tr>
				<tr>
					<td><code>7</code></td>
					<td><code>2#.-.0 0 0</code></td>
					<td><code>+/*:2,&gt;:0 0 0</code></td>
					<td><code>17</code></td>
					<td><code>&lt;.-/ˆ&gt;:~.x</code></td>
					<td><code>&gt;.2*+/ˆ&gt;:0 0 0</code></td>
				</tr>
				<tr>
					<td><code>8</code></td>
					<td><code>#.,#:~.x</code></td>
					<td><code>-/*:&gt;&gt;x</code></td>
					<td><code>18</code></td>
					<td><code>20-#0 0</code></td>
					<td><code>+/&lt;.o.2+0 0 0</code></td>
				</tr>
				<tr>
					<td><code>9</code></td>
					<td><code>*/*:&gt;:x</code></td>
					<td><code>+/2+&gt;:0 0 0</code></td>
					<td><code>19</code></td>
					<td><code>20-=/0 0</code></td>
					<td><code>&gt;.+/o.2+0 0 0</code></td>
				</tr>
			</tbody>
		</table>

Another amusing possibility, though ultimately monotonous because expressions are restricted to monadic functions, is to try to develop all the numbers from four zeroes in at least one list and at most one scalar.

In the following table it has been assumed that

```
                 z =: 0 0 0 0
```

has been issued.

<table class="left">
			<tbody valign="bottom">
				<tr>
					<td><code>0</code></td>
					<td><code>~.z</code></td>
					<td><code>0{0 0 0</code></td>
					<td><code>10</code></td>
					<td><code>+/!/:z</code></td>
					<td><code>&gt;.+/o.0=0 0 0</code></td>
				</tr>
				<tr>
					<td><code>1</code></td>
					<td><code>=/z</code></td>
					<td><code>0 e.0 0 0</code></td>
					<td><code>11</code></td>
					<td><code>#./:z</code></td>
					<td><code>&gt;.+/ˆ-.z</code></td>
				</tr><tr>
					<td><code>2</code></td>
					<td><code>--//:z</code></td>
					<td><code>+/0 0=0 0</code></td>
					<td><code>12</code></td>
					<td><code>+/+:/:z</code></td>
					<td><code>*/!//:z</code></td>
				</tr><tr>
					<td><code>3</code></td>
					<td><code>{:/:z</code></td>
					<td><code>+/0=0 0 0</code></td>
					<td><code>13</code></td>
					<td><code>+/&gt;:+:#:}:/:z</code></td>
					<td><code>&gt;.+/o.-.z</code></td>
				</tr><tr>
					<td><code>4</code></td>
					<td><code>#z</code></td>
					<td><code>+/&lt;.ˆ0 0=0 0</code></td>
					<td><code>14</code></td>
					<td><code>+/*:/:z</code></td>
					<td><code>#.&gt;:0=0 0 0</code></td>
				</tr><tr>
					<td><code>5</code></td>
					<td><code>-.-/+:/:z</code></td>
					<td><code>&lt;.+/ˆ0 0=0 0</code></td>
					<td><code>15</code></td>
					<td><code>#.-.z</code></td>
					<td><code>+/&gt;.!ˆ0=0 0 0</code></td>
				</tr><tr>
					<td><code>6</code></td>
					<td><code>+//:z</code></td>
					<td><code>+/&lt;.o.0 0=0 0</code></td>
					<td><code>16</code></td>
					<td><code>+/&lt;.!ˆ-.z</code></td>
					<td><code>&gt;.ˆ/ˆ0 0=0 0</code></td>
				</tr><tr>
					<td><code>7</code></td>
					<td><code>#.0=0 0 0</code></td>
					<td><code>-.-/*:/:z</code></td>
					<td><code>17</code></td>
					<td><code>+/}.&gt;.}.o./:z</code></td>
					<td><code>&lt;.*/&gt;:o.0 0=0 0</code></td>
				</tr><tr>
					<td><code>8</code></td>
					<td><code>&gt;.-.-/o./:z</code></td>
					<td><code>+/+:-.z</code></td>
					<td><code>18</code></td>
					<td><code>+:*:{./:z</code></td>
					<td><code>+/&lt;.o./:z</code></td>
				</tr><tr>
					<td><code>9</code></td>
					<td><code>*:{:/:z</code></td>
					<td><code>*:+/0=0 0 0</code></td>
					<td><code>19</code></td>
					<td><code>&gt;.+/o./:z</code></td>
					<td><code>&lt;:+/,~!/:z</code></td>
				</tr>
			</tbody>
		</table>

## Postscript

This series of instructional essays were the course material used by the author over several years for teaching tacit use of J, as described in [\[1\]](#ref1).

The particular nature of the essay above was to illustrate the functions explained in the preceding essay [\[2\]](#ref2) and to explain the students’ assigned task for the following week.

This weekly task, which counted substantially towards a student's overall mark, was to do for their student identification numbers what the text of this essay shows for the year `1998`. They were expected to provide 200 expressions, and their mark depended on the richness and brevity of the code in their work, which was submitted as a J file and analysed by J code.

## References

1. <a id="ref1"></a> Holmes, N., “Tacit J and I”, *Vector*, Vol.23, No.3, pp.52-56  
    [http:/vector.org.uk/?vol=23&no=3&art=holmes](../art10011810/)
2. <a id="ref2"></a> Holmes, N., “Structural Ingredients”, *Vector*, Vol.24, No.2  
    [http:/vector.org.uk/?vol=24&no=2&art=holmes](../art10500320/)
