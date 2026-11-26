---
vid: '10500050'
title: 'Functional calculation 2: The year 1997'
subtitle: '2: The year 1997'
authors:
- Neville Holmes
byline: by Neville Holmes (neville.holmes@utas.edu.au)
volume: '24'
issue: '1'
page: '82'
description: Article submitted to vector.org.uk
abstract: Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. In preceding articles an introduction was given to what could be done with one commonly available tool for functional calculation, using a notation called J, then details were given of simple numerical calculation. This article is intended to allow the reader to consider how simple numerical calculation can be done in J by showing numeric expressions to produce whole numbers below 100 starting from the digits of the number 1997.
source: content/printed/241/fncalc2.htm
converter: vecrescue 0.1.0
issue_tab: n1-december-2008
tags:
- education
- mathematics and statistics
---

2: The year 1997
{ .subtitle }

by Neville Holmes (neville.holmes@utas.edu.au)
{ .byline }

Functional calculation does with operations applied to functions and numbers what numerical calculation does with functions applied to numbers. In preceding articles an introduction was given to what could be done with one commonly available tool for functional calculation, using a notation called J, then details were given of simple numerical calculation. This article is intended to allow the reader to consider how simple numerical calculation can be done in J by showing numeric expressions to produce whole numbers below `100` starting from the digits of the number `1997`.
{ .abstract }

## Numerical calculations

Preceding articles have introduced numerical calculation using the interpreter for the J notation. This article gives a change of pace in which the notation already introduced is used in modest examples so that the reader can get used to its differences from the more usual (though inconsistent) mathematical notation.

To set up some simple examples, expressions to produce all the non-negative integers of fewer than three digits are to be sought. There are several restrictions.

Firstly, only the following scalar primitive functions are to be used, though clearly many of them are not very useful – there are more comparisons than are needed, and *nand* and *nor* have very restricted domains.

<table class="bdr center left">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
    	<col style="background: #DDD; border-left: 1px solid gray; text-align: left;" class="code">
    	<col style="text-align: left" span="2">
      <tbody style="vertical-align: bottom">
        <tr>
          <td>+</td>
          <td>conjugate</td>
          <td>add</td>
          <td>+.</td>
          <td></td>
          <td>GCD</td>
          <td>+:</td>
          <td>double</td>
          <td>nor</td>
        </tr>
        <tr>
          <td>-</td>
          <td>negate</td>
          <td>subtract</td>
          <td>-.</td>
          <td>not</td>
          <td></td>
          <td>-:</td>
          <td>halve</td>
          <td></td>
        </tr>
        <tr>
          <td>*</td>
          <td>signum</td>
          <td>times</td>
          <td>*.</td>
          <td></td>
          <td>LCM</td>
          <td>*:</td>
          <td>square</td>
          <td>nand</td>
        </tr>
        <tr>
          <td>%</td>
          <td>reciprocal</td>
          <td>divide</td>
          <td></td>
          <td></td>
          <td></td>
          <td>%:</td>
          <td>square root</td>
          <td>root</td>
        </tr>
        <tr>
          <td>|</td>
          <td>magnitude</td>
          <td>residue</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr style="border-bottom: 1px solid gray">
          <td>^</td>
          <td>exp</td>
          <td>power</td>
          <td>^.</td>
          <td>log<sub>e</sub></td>
          <td>logarithm</td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>=</td>
          <td></td>
          <td>equal</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td>&lt;</td>
          <td></td>
          <td>less than</td>
          <td>&lt;.</td>
          <td>floor</td>
          <td>lesser</td>
          <td>&lt;:</td>
          <td>decrement</td>
          <td>not more</td>
        </tr>
        <tr>
          <td>&gt;</td>
          <td></td>
          <td>more than</td>
          <td>&gt;.</td>
          <td>ceiling</td>
          <td>greater</td>
          <td>&gt;:</td>
          <td>increment</td>
          <td>not less</td>
        </tr>
        <tr style="border-bottom: 1px solid gray">
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td>~:</td>
          <td></td>
          <td>not equal</td>
        </tr>
        <tr>
          <td>!</td>
          <td>factorial</td>
          <td>choices</td>
          <td>o.</td>
          <td>pi times</td>
          <td>circular</td>
          <td>p:</td>
          <td>prime</td>
          <td></td>
        </tr>
        <tr>
          <td>[</td>
          <td>(same)</td>
          <td>(left)</td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
        </tr>
      </tbody>
    </table>

Secondly, only the digits `1 9 9` and `7` must be used as arguments, all those digits, and only in the sequence given. Thus up to four arguments may be used in an expression, but no argument may be a list, that is, all arguments must be scalar. The significance of this will be clearer later.

In the following, two expressions are given for each number, both because there is room for them, and to add interest. The reader should, as an exercise, look for solutions better in some way than those given – there should be plenty, and looking for them should lead to insights, whether the search is successful or not.

One avenue to explore is the use of numbers other than simple integers. These are avoided in the examples but, for instance, `19+<.|9j7` gives `30` using only three functions, which is fewer than in the expressions given below. Similarly, `>.%:1p9-9p7` uses fewer functions to get `52` than do the examples.

## Making 1997 give 0 to 19

Taking the four digits `1 9 9` and `7`, in that sequence, combine them using J functions and operations, in as short and simple an expression as possible, to yield each of the numbers between `0` and `19` (here, `99` later), and to yield them as scalars using only scalar arguments and scalar functions. (As a matter of aesthetics, parentheses and decimal points are also avoided as far as possible. Also as a matter of style, the negative sign is avoided and subtraction or negation is used to similar effect.)

<table class="center code left">
    	<col span="1" style="background: #DDD">
    	<col span="2">
    	<col span="1" style="background: #DDD">
    	<col span="2">
      <tbody>
        <tr>
          <td>0</td>
          <td>19=97</td>
          <td>ˆ.*1997</td>
          <td>10</td>
          <td>19-9&gt;.7</td>
          <td>|1-9+9-7</td>
        </tr>
        <tr>
					<td>1</td>
          <td>*1997</td>
          <td>199&gt;7</td>
          <td>11</td>
          <td>19-9-*7</td>
          <td>19--:9+7</td>
        </tr>
        <tr>
					<td>2</td>
          <td>19|97</td>
          <td>+:*1997</td>
          <td>12</td>
          <td>1+9+9-7</td>
          <td>19-9|7</td>
        </tr>
        <tr>
					<td>3</td>
          <td>19-9+7</td>
          <td>199-*:+:7</td>
          <td>13</td>
          <td>-:19+9|7</td>
          <td>1*9+%:9+7</td>
        </tr>
        <tr>
					<td>4</td>
          <td>19|9ˆ7</td>
          <td>1+%:9s&lt;.97</td>
          <td>14</td>
          <td>199|+:7</td>
          <td>1+9+%:9+7</td>
        </tr>
        <tr>
					<td>5</td>
          <td>19-+:9|7</td>
          <td>19|&gt;.ˆ.97</td>
          <td>15</td>
          <td>&gt;.19%9%7</td>
          <td>1+9+&gt;.ˆ.97</td>
        </tr>
        <tr>
        <td>6</td>
          <td>19|9*7</td>
          <td>1+9-%:9+7</td>
          <td>16</td>
          <td>19|9+7</td>
          <td>&lt;.19*9ˆ.7</td>
        </tr>
        <tr>
        <td>7</td>
          <td>1!9-9-7</td>
          <td>19|9!+:7</td>
          <td>17</td>
          <td>1+9+9|7</td>
          <td>19-9-7</td>
        </tr>
        <tr>
        <td>8</td>
          <td>1+9|97</td>
          <td>19|%:&gt;:9*7</td>
          <td>18</td>
          <td>19-*97</td>
          <td>1+9+9-*7</td>
        </tr>
        <tr>
        <td>9</td>
          <td>19&lt;.9&gt;.7</td>
          <td>19-9+*7</td>
          <td>19</td>
          <td>1*p:9-9-7</td>
          <td>1+9+9&gt;.7</td>
        </tr>
      </tbody>
    </table>

Two expressions are given in the table for each number. Several expressions use only the one primitive function, but none needs more than four.

Expressions abound for `0` and `1`, particularly expressions using functions that yield logical values. Thus `1>997` and `199<7` yield `0`, while `19<97` and `19+.97` yield `1`. The restriction of yielding a scalar rules out `/:1997` and `=1997` which otherwise give a quite respectable `0` and `1`. Also, `#1997` gives a scalar `1`, and `#19 97` gives a scalar `2`, but the restriction on scalar components rules the second of these two possibilities out of order.

Certain components of expressions are repeatedly used in the table above. Thus, `199|` (and `19|`) is used to pass over the digits of the left argument of `|` when the value of its right argument is lower than that of its left. In particular, `9|7` simply yields `7`. Otherwise, `<.` or `>.` are widely used where the digits to the right or left of the `<.` or `>.` are to be ignored and the simpler and more pleasing `|` won’t serve instead. Notice, though, that the instances of `19|` in `19|9ˆ7` and `19|9*7` and `19|9!+:7` are not ignorant.

Expressions starting with monadic increment (`>:`) and decrement (`<:`) functions can often make a shorter expression than the example given, but since these will only be trivially different from neighbouring expressions they are avoided if possible. Monadic halve (`-:`) and double (`+:`) are useful, but these are also a bit trivial for generating smaller or larger numbers.

The year 1997 is interesting in the scope it gives for the square root function (`%:`). In the above, `%:9<.97` is used to give `3`, `%:9+7` is used to give `4`, and `%:>:9*7` is used to give `8`. In fact `-:9+7` and `9-*7` also give `8`, but maybe not so cutely. Otherwise, `>.*:1.997` could have been used for `4` and `>.ˆ.1997` for `8`, both of which are quite interesting.

## Making 1997 give 20 to 99

Making numbers beyond 19 follows a similar pattern, and it is convenient here to take them twenty at a time. Of course, expressions starting 19+ will be common in the next table.

<table class="center code left">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<tr><td>20</td><td>19+9&gt;7</td><td>19+*97</td><td>30</td><td>19+9++:*7</td><td>1+9++:9+*7</td></tr>
			<tr><td>21</td><td>19+9-7</td><td>19+&gt;:*97</td><td>31</td><td>&lt;.19*9%:7</td><td>&lt;.%:-:1997</td></tr>
			<tr><td>22</td><td>&lt;.-:%:1997</td><td>&gt;:19+9-7</td><td>32</td><td>1+&lt;.%:997</td><td>+:19|9+7</td></tr>
			<tr><td>23</td><td>19+%:9+7</td><td>p:1+9-9-7</td><td>33</td><td>&gt;.19ˆ.ˆ97</td><td>1+9&gt;.+:9+7</td></tr>
			<tr><td>24</td><td>19-9-+:7</td><td>19+p:9+7</td><td>34</td><td>+:19-9-7</td><td>1!&lt;.9*9ˆ.!7</td></tr>
			<tr><td>25</td><td>19+9|&lt;:7</td><td>1!9+9+7</td><td>35</td><td>19+9+7</td><td>&gt;.19+ˆ.9ˆ7</td></tr>
			<tr><td>26</td><td>1+9+9+7</td><td>19+9|7</td><td>36</td><td>+:19-*97</td><td>1!9*&gt;.9ˆ.!7</td></tr>
			<tr><td>27</td><td>19+-:9+7</td><td>19+9-*7</td><td>37</td><td>1+-:9+9*7</td><td>&gt;.ˆ.199ˆ7</td></tr>
			<tr><td>28</td><td>19+9&gt;.7</td><td>1+99|!&lt;:7</td><td>38</td><td>1+p:9+9-7</td><td>19*9-7</td></tr>
			<tr><td>29</td><td>&gt;.199%7</td><td>19+9+*7</td><td>39</td><td>-:-19-97</td><td>&lt;:+:19+9&gt;7</td></tr>
		</table>

At a pinch, all the numbers here, and in the next table for that matter, can be constructed from the solutions of the previous table by doubling (monadic `+:`) possibly combined with incrementing (monadic `>:`) or decrementing (monadic `<:`). However, these solutions will only reluctantly be used here, in the absence of some other expression.

Getting these larger values is somewhat more difficult, so it useful more often to go to non-integer intermediate values and then use the floor (`<.`) or ceiling (`>.`) function to get an integer. There are no solutions given in the 20-39 table which need only the one function, but quite a few need only two. On the other hand, no solutions need more than five functions.

<table class="center code left">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<tr><td>40</td><td>+:19+*97</td><td>1-9-&lt;.-:97</td><td>50</td><td>-:1+99&gt;.7</td><td>1-9-9+*:7</td></tr>
			<tr><td>41</td><td>1!&lt;.9*ˆ.97</td><td>1+9+-:&lt;:9*7</td><td>51</td><td>1+99-*:7</td><td>19++:9+7</td></tr>
			<tr><td>42</td><td>+:19+9-7</td><td>19+9++:7</td><td>52</td><td>+:1+9+9+7</td><td>1+&gt;.99%ˆ.7</td></tr>
			<tr><td>43</td><td>&gt;.%:19*97</td><td>1*&lt;.9*9ˆ.!&gt;:7</td><td>53</td><td>1*-:99+7</td><td>|1+9-9*7</td></tr>
			<tr><td>44</td><td>-19-9*7</td><td>&lt;.%:1997</td><td>54</td><td>1+-:9+97</td><td>-.1+9-9*7</td></tr>
			<tr><td>45</td><td>&gt;.%:1997</td><td>&gt;.ˆ.19!97</td><td>55</td><td>1-9-9*7</td><td>-.1*9-9*7</td></tr>
			<tr><td>46</td><td>1!-:99-7</td><td>19+9+p:7</td><td>56</td><td>1+-.9-9*7</td><td>19+&gt;:9*-:&gt;:7</td></tr>
			<tr><td>47</td><td>1+-:99-7</td><td>1!&gt;.9*ˆ.+:97</td><td>57</td><td>-:1+99++:7</td><td>19*&lt;.9ˆ.!7</td></tr>
			<tr><td>48</td><td>1+&gt;.*:ˆ.997</td><td>-1-9-9-*:7</td><td>58</td><td>-:19+97</td><td>19&gt;.9+*:7</td></tr>
			<tr><td>49</td><td>*:199&lt;.7</td><td>-:1+9&gt;.97</td><td>59</td><td>1+9+-:&gt;:97</td><td>19-9-*:7</td></tr>
		</table>

For the 40s and 50s, `9*7` gives 63 to work down from, and `-:97` and `-:99` gets into the high 40s, as does `*:7`. At least one expression is given for each of these numbers which does not need more than four primitive functions.

<table class="center code left">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<tr><td>60</td><td>&gt;.19*o.9&gt;7</td><td>1+&lt;.+:%:9*97</td><td>70</td><td>+:19+9+7</td><td>|1-&lt;.9*%:9*7</td></tr>
			<tr><td>61</td><td>-:199|!&gt;:7</td><td>&gt;.%:+:19*97</td><td>71</td><td>-1-9+9*7</td><td>-:&gt;.ˆ.%:19ˆ97</td></tr>
			<tr><td>62</td><td>19&gt;.&lt;:9*7</td><td>-:&gt;:199|!&lt;:7</td><td>72</td><td>-.1-9+9*7</td><td>1*-:9*9+7</td></tr>
			<tr><td>63</td><td>1&gt;.9*9&lt;.7</td><td>-.1-9&gt;.9*7</td><td>73</td><td>1+9+9*7</td><td>19+9*&lt;:7</td></tr>
			<tr><td>64</td><td>1+9&gt;.9*7</td><td>1+9+9*&lt;:7</td><td>74</td><td>&gt;.19*9ˆ.!7</td><td>&lt;.19++:ˆ.9!7</td></tr>
			<tr><td>65</td><td>199|!7</td><td>199|!+:7</td><td>75</td><td>-:199-*:7</td><td>19+9+p:+:7</td></tr>
			<tr><td>66</td><td>-1-9+9+*:7</td><td>1*&lt;.9*%:9*&lt;:7</td><td>76</td><td>19*%:9+7</td><td>19*&gt;.9ˆ.!7</td></tr>
			<tr><td>67</td><td>19+&lt;.-:97</td><td>19+-:&lt;:97</td><td>77</td><td>&gt;.19*o.9%7</td><td>&gt;.+:1+%:99*7</td></tr>
			<tr><td>68</td><td>19+&gt;.-:97</td><td>19+-:&gt;:97</td><td>78</td><td>-19-97</td><td>19+p:9+7</td></tr>
			<tr><td>69</td><td>&gt;.19*ˆ9%7</td><td>19++:9++:7</td><td>79</td><td>-.19-97</td><td>&gt;.19*ˆ.9*&gt;:7</td></tr>
		</table>

For the 60s and 70s, `9*7` provides a good starting point. Of interest are the two expressions for 65, which look the same but are interestingly different, and otherwise only 78 and 79 have expressions with only two primitive functions. There is plenty of room for improvement here, though only 66 is shown as needing more than five primitive functions.

<table class="center code left">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<tr><td>80</td><td>1-+:9--:97</td><td>1*99-p:7</td><td>90</td><td>&gt;.+:%:1997</td><td>19+&lt;.9ˆˆ.7</td></tr>
			<tr><td>81</td><td>1&gt;.9*9&gt;.7</td><td>-.1-9+9*&gt;:7</td><td>91</td><td>1+99|!7</td><td>-1-99-7</td></tr>
			<tr><td>82</td><td>19+9*7</td><td>1+9+9*&gt;:7</td><td>92</td><td>1!99-7</td><td>-.1-99-7</td></tr>
			<tr><td>83</td><td>&gt;:19+9*7</td><td>1++:&lt;.9*ˆ.97</td><td>93</td><td>1+99-7</td><td>1--:&gt;:9-+:97</td></tr>
			<tr><td>84</td><td>+:+:19+9-7</td><td>1+&gt;.+:9*ˆ.97</td><td>94</td><td>&gt;:1+99-7</td><td>1--:&lt;:9-+:97</td></tr>
			<tr><td>85</td><td>1!99-+:7</td><td>&lt;.19*ˆ9%&lt;:7</td><td>95</td><td>19*&gt;.ˆ.97</td><td>19*&gt;:%:9+7</td></tr>
			<tr><td>86</td><td>&lt;.19*ˆ.97</td><td>1+99-+:7</td><td>96</td><td>-1-9&gt;.97</td><td>-:199-7</td></tr>
			<tr><td>87</td><td>|1+9-97</td><td>&gt;.19*ˆ.97</td><td>97</td><td>19&gt;.97</td><td>|1-99-*7</td></tr>
			<tr><td>88</td><td>-.1+9-97</td><td>&gt;.1*9*%:97</td><td>98</td><td>1+9&gt;.97</td><td>-.1-99-*7</td></tr>
			<tr><td>89</td><td>1-9-97</td><td>-:&lt;:199|!&gt;:+:7</td><td>99</td><td>1&gt;.99&gt;.7</td><td>-:199-*7</td></tr>
		</table>

The 80s and 90s often use 99 or 97 and work down from there. This gives the quite short expressions shown here for 89 and 97, though `9*7` and 199 find occasional use.

In this group of numbers there is the one expression with only one primitive function, seven with two, lots with three, but the improvement to be sought is in those with five or six primitive functions.

## Further examples

The examples given above can only suggest how arithmetic functions can be used in a simple to produce a variety of numbers. The reader is urged to consider the examples above with a J interpreter to hand, to try the examples out, to check them, and to try to find expressions that are better or in some way more interesting than those given here. When generating these numbers begins to pall, the reader perhaps should go on to consider how to generate the three digit numbers using the same rules. This could start `1+99>.7` then `199-+:*:7`.

Alternatively, expressions might be sought for other years. Some years will present special challenges. The following table gives a start for the year 2000, in which a choice is made between `0=0`, `0!0` and `0ˆ0` to give a 1 largely on aesthetic grounds.

<table class="center code left">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<tr><td>0</td><td>200=0</td><td>ˆ.*2000</td><td>10</td><td>20%+:0=0</td><td>20-&gt;.o.o.0!0</td></tr>
			<tr><td>1</td><td>*2000</td><td>200ˆ0</td><td>11</td><td>-:20++:0!0</td><td>&lt;.ˆ+:*:200+0</td></tr>
			<tr><td>2</td><td>2+0-0*0</td><td>+:*2000</td><td>12</td><td>20-&lt;.o.ˆ0=0</td><td>+:&lt;.%:%:2000</td></tr>
			<tr><td>3</td><td>2+0=0+0</td><td>2!&gt;.0+ˆ0=0</td><td>13</td><td>&lt;.+:%:%:2000</td><td>p:&lt;.ˆ.200-0</td></tr>
			<tr><td>4</td><td>2*+:0&lt;0!0</td><td>&gt;.ˆ.20+0=0</td><td>14</td><td>&lt;.%:200+0</td><td>&lt;.ˆ.-:*:2000</td></tr>
			<tr><td>5</td><td>&lt;.ˆ.200+0</td><td>20-&lt;.ˆˆ0=0</td><td>15</td><td>&gt;.%:200-0</td><td>&lt;.+:ˆ.2000</td></tr>
			<tr><td>6</td><td>&lt;.%:%:2000</td><td>&gt;.ˆ.200+0</td><td>16</td><td>&gt;.ˆ.*:2000</td><td>20-&gt;.o.0=0</td></tr>
			<tr><td>7</td><td>&lt;.ˆ.2000</td><td>&gt;.ˆ.-:2000</td><td>17</td><td>20-&gt;:+:0!0</td><td>&gt;.+:ˆ.+:2000</td></tr>
			<tr><td>8</td><td>2ˆp:0+0!0</td><td>2ˆ0]&gt;:+:0=0</td><td>18</td><td>20-+:0=0</td><td>&gt;.!o.%:2000</td></tr>
			<tr><td>9</td><td>&lt;.-:20-0!0</td><td>&gt;.ˆ.p:2000</td><td>19</td><td>20-0!0</td><td>&gt;.ˆ.p:*:2000</td></tr>
		</table>

Apart from reducing the number of possibilities, having all those nought digits tends to give unsightly numbers like `00` and `000` which are therefore avoided here as far as is convenient. Thus, although `2+000` would be technically correct for 2, the expression `2+0-0*0` is shown, though many similar ones would be just as satisfactory. Not having a 1 at the front also reduces the possibilities a lot!

Another amusing possibility, though ultimately monotonous because expressions are restricted to monadic functions, is to try to develop all the numbers from only a single zero. A start to this is given in the following, and the aesthetic choice is between `!0`, `ˆ0`, `>:0`, and `-.0` to get the initial 1, or `p:0` to get an initial 2.

<table class="center code left">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<col span="1" style="background: #DDD">
			<col span="2">
			<tr><td>0</td><td>|0</td><td>*0</td><td>10</td><td>&gt;.*:o.!0</td><td>&gt;.o.o.&gt;:0</td></tr>
			<tr><td>1</td><td>!0</td><td>ˆ0</td><td>11</td><td>&lt;.-:ˆo.ˆ0</td><td>p:+:p:0</td></tr>
			<tr><td>2</td><td>+:-.0</td><td>p:0</td><td>12</td><td>-:!+:+:!0</td><td>-:&gt;.ˆo.-.0</td></tr>
			<tr><td>3</td><td>p:-.0</td><td>&gt;.ˆˆ0</td><td>13</td><td>&lt;.*:&gt;:ˆˆ0</td><td>p:p:p:0</td></tr>
			<tr><td>4</td><td>*:+:!0</td><td>&gt;.o.&gt;:0</td><td>14</td><td>+:&lt;.!o.!0</td><td>+:&lt;.*:ˆˆ0</td></tr>
			<tr><td>5</td><td>&gt;:+:+:&gt;:0</td><td>p:p:0</td><td>15</td><td>&lt;.ˆˆˆ0</td><td>&lt;:*:*:+:!0</td></tr>
			<tr><td>6</td><td>!&gt;:+:!0</td><td>+:&gt;:+:-.0</td><td>16</td><td>*:*:&gt;:&gt;:0</td><td>*:+:+:-.0</td></tr>
			<tr><td>7</td><td>&lt;.ˆ+:ˆ0</td><td>p:p:ˆ0</td><td>17</td><td>&lt;.o.+:ˆˆ0</td><td>p:+:p:ˆ0</td></tr>
			<tr><td>8</td><td>&lt;.o.ˆˆ0</td><td>&gt;.ˆp:0</td><td>18</td><td>+:&gt;.o.ˆˆ0</td><td>+:*:&gt;:+:!0</td></tr>
			<tr><td>9</td><td>*:&gt;:+:!0</td><td>&gt;.o.ˆˆ0</td><td>19</td><td>&lt;:&lt;.ˆ&gt;.ˆ!0</td><td>p:p:&gt;:p:0</td></tr>
		</table>

The shortness of some of expressions given here comes often from using the `o.` and `ˆ` functions to bump up values quickly, then using floor (`>.`) or ceiling (`<.`) functions to get integers. Otherwise `p:` can be used most conveniently. Of course, where an expression starts with `<.` the next higher number can be got by substituting a `>.` and vice versa, mutatis mutandis.

The expressions given in these last two tables were selected unsystematically and hastily. The assiduous reader should have an interesting but fruitful time looking for improved and extended expressions.
