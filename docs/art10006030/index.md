---
vid: '10006030'
title: 'ASLGREG: Predictions with Confidence Limits'
authors:
- Ellis Morgan
byline: by Ellis Morgan
volume: '10'
issue: '3'
page: '81'
source: trad/v103/morgan103_81.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-1994
tags:
- mathematics and statistics
- programming techniques
---

by Ellis Morgan <span class="affil"></span> <span class="email"></span>
{ .byline }

I present a possible approach to adding predictions to the ASLGREG Regression and Generalised Linear Models Workspace. I include ideas to increase robustness and a method that allows `STEPWISE` to return a linear predictor equation that can be passed to `FIT` and other ASLGREG functions.

Many of the good ideas behind this are due to Alan Sykes and I am grateful for his help and his tolerance as I change his code and workspace structure. I propose that my extensions should be added to ASLGREG and welcome comments and suggestions for improvement.

### ASLGREG

My descriptions of the following examples are sparse where they cover old ground, see: ASLGREG by Alan Sykes (Vector 7.4); the ASLGREG manual (available from BAA Administration); ASLGREG reviewed by Jon Sandles (Vector 9.2).

```
      HOUSEDATA
      ⍴PRICE
29
      PRICE
60500 47500 46000 110000 148000 65000 79950 54000 120000
      85000 65000 55000 70000 82500 78500 49950 46950
      46950 77000 102000 37500 68000 65000 145000 98000
      75000 96500 53950 75000
```

The workspace is `ASLGREG`, `HOUSEDATA` sets up Alan Sykes data on 29 houses. Other variables are `TYPE` (1 terraced, 2 semi-detached, 3 detached, 4 bungalow), `AGE`, `BEDS`(number of bedrooms) and `AREA`.

```
      START
Regression Volume 1 Version 2
A.M.Sykes & E.P.Morgan, February 1993
Copyright(C) British APL Association 1991-1993

      YVAR'PRICE÷1000'
Y variable is PRICE÷1000
Units set to 29
      FACTOR'TYPE,BEDS'
      1 LP'GM+AGE+BEDS+TYPE+AREA+TYPE.AREA+BEDS/TYPE'
      TABLES 0
      LPA←STEPWISE 3 3
      LPA
GM+AREA+(TYPE=1)×AREA+(BEDS=4)×TYPE=3
```

`STEPWISE` has selected this model to estimate price and in the new version you can now write functions that call `STEPWISE` and go on to use the result in `FIT` and `PREDICT`. It can be dangerous to proceed from `STEPWISE` to `PREDICT` without thought but, provided diagnostics are displayed too, it is good to be able to do so. In the new workspace `LP` is ambivalent and the left argument, when present and equal to one, makes each item in the linear predictor an APL expression. With any other left argument the result is the same as in the original version.

```
      TABLES 1
Output in Table form
      FIT LPA

Source              ss          df     ms
------              --          --     --
Due to model        1.861E4      3     6205
Residual            3.826E3     25      153.1
---------------------------------------------
Total Corrected     2.244E4     28      801.5

Percentage Variation Accounted for =  82.95
F-statistic =                         40.54
p-value=                               0.00
      DISPLAY 'ESTP'
```

```
                                                                  Var                Est        Std Err    t-stat    p-val                            ---                ---        --- ---    ------    -----                            GM                31.06       6.095       5.096    2.909E¯5                    AREA               0.05541    0.007573    7.317    1.149E¯7                    (TYPE=1)×AREA     ¯0.02236    0.005579   ¯4.007    4.861E¯4    (BEDS=4)×TYPE=3   38.67       8.292       4.664    8.906E¯5
```

This model estimates the average house price at £31060 plus £55.4 per square foot, but subtract £22.4 per square foot if it is a terraced house and add on £38670 if it is a four bedroomed detached house.

```
     +/(BEDS=4)×TYPE=3
3
     +/BEDS=4
6
     +/TYPE=3
5
```

There are only 5 detached houses (three of which are 4 bedroomed) and 6 four bedroomed houses, obviously you would want to gather more evidence before using this predictor.

```
      ⍴8⍴≡DIAGNOSTICS 'XFO'
```

<table class="aplu"><tb>
<tr>
<td>Unit  </td>
<td>AREA  </td>
<td>(TYPE=1)<br>×AREA  </td>
<td>(BEDS=4)<br>×TYPE=3  </td>
<td>Fitted<br>Value  </td>
<td>Observed  </td>

</tr>
<tr>
<td>----  </td>
<td>----  </td>
<td>--------  </td>
<td>--------  </td>
<td>------  </td>
<td>--------  </td>

</tr>
<tr>
<td>1  </td>
<td>1326  </td>
<td>1326  </td>
<td>0  </td>
<td>74.89  </td>
<td>60.5  </td>

</tr>
<tr>
<td>2  </td>
<td>782  </td>
<td>0  </td>
<td>0  </td>
<td>74.39  </td>
<td>47.5  </td>

</tr>
<tr>
<td>3  </td>
<td>312  </td>
<td>0  </td>
<td>0  </td>
<td>48.35  </td>
<td>46  </td>

</tr>
<tr>
<td>4  </td>
<td>1460  </td>
<td>0  </td>
<td>0  </td>
<td>112  </td>
<td>110  </td>

</tr>
<tr>
<td>5  </td>
<td>1056  </td>
<td>0  </td>
<td>1  </td>
<td>128.2  </td>
<td>148</td>

</tr>
</tb></table>

I have shown the first eight lines of the diagnostics, you will see that I am using dyalog APL. Some new right arguments have been added to `DIAGNOSTICS`, `X` calls for all the explanatory variables.

### PREDICTING from the original data

```
      ⍴8⍴≡PREDICT 'SYMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>AREA  </td>
<td>BEDS  </td>
<td>TYPE  </td>
<td>Observed  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>----  </td>
<td>----  </td>
<td>----  </td>
<td>--------  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>1326  </td>
<td>4  </td>
<td>1  </td>
<td>60.5  </td>
<td>74.89  </td>
<td>82.3  </td>
<td>67.48  </td>

</tr>
<tr>
<td>2  </td>
<td>782  </td>
<td>3  </td>
<td>2  </td>
<td>47.5  </td>
<td>74.39  </td>
<td>78.24  </td>
<td>70.55  </td>

</tr>
<tr>
<td>3  </td>
<td>312  </td>
<td>1  </td>
<td>2  </td>
<td>46  </td>
<td>48.35  </td>
<td>53.83  </td>
<td>42.87  </td>

</tr>
<tr>
<td>4  </td>
<td>1460  </td>
<td>3  </td>
<td>3  </td>
<td>110  </td>
<td>112  </td>
<td>120.3  </td>
<td>103.6  </td>

</tr>
<tr>
<td>5  </td>
<td>1056  </td>
<td>4  </td>
<td>3  </td>
<td>148  </td>
<td>128.2  </td>
<td>137.7  </td>
<td>118.8</td>

</tr>
</tb></table>

`PREDICT` is similar to `DIAGNOSTICS` in that the right argument specifies the columns to print. In this case I am calling for: `S` the explanatory variables before interactions, nesting and other equations; `Y` for the observed Y value; `M` for the predicted Mean value; `U` and `L` for the Upper and Lower confidence limits of the mean. The optional left argument of `PREDICT` is described below and specifies the confidence level (default is 80%) and the type of limit (default is independent for each prediction).

### PREDICTING for a few extra observations

The simplest way is to add a few extra observations to your data, giving the response variable a value of zero and then keep them out of the fitting process by giving them a weight of zero. All real cases will have a weight of one.

```
      PRICE←0 0,PRICE
      TYPE←3 4,TYPE
      BEDS←4 3,BEDS
      AREA←1200 800,AREA
```

Two new “cases” at the beginning.

```
      YVAR'PRICE÷1000'
Y variable is PRICE÷1000
Units set to 31

      W←0 0,29⍴1
      PRIORWEIGHT 'W'
Prior Weight Variable is W

      SINK←FIT LPA
      DISPLAY 'ESTP'
```

<table class="aplu"><tb>
<tr>
<td>Var  </td>
<td>Est  </td>
<td>Std Err  </td>
<td>t-stat  </td>
<td>p-val  </td>

</tr>
<tr>
<td>---  </td>
<td>---  </td>
<td>--- ---  </td>
<td>------  </td>
<td>-----  </td>

</tr>
<tr>
<td>GM  </td>
<td>31.06  </td>
<td>6.095  </td>
<td>5.096  </td>
<td>2.909E¯5  </td>

</tr>
<tr>
<td>AREA  </td>
<td>0.05541  </td>
<td>0.007573  </td>
<td>7.317  </td>
<td>1.149E¯7  </td>

</tr>
<tr>
<td>(TYPE=1)×AREA  </td>
<td>¯0.02236  </td>
<td>0.005579  </td>
<td>¯4.007  </td>
<td>4.861E¯4  </td>

</tr>
<tr>
<td>(BEDS=4)×TYPE=3  </td>
<td>38.67  </td>
<td>8.292  </td>
<td>4.664  </td>
<td>8.906E¯5</td>

</tr>
</tb></table>

The same answer as before.

```
      ⍴10⍴≡PREDICT 'SYWMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>AREA  </td>
<td>BEDS  </td>
<td>TYPE  </td>
<td>Observed  </td>
<td>Priorwt  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>----  </td>
<td>----  </td>
<td>----  </td>
<td>------  </td>
<td>-----  </td>
<td>------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>0  </td>
<td>0  </td>
<td>136.2  </td>
<td>145.7  </td>
<td>126.8  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>4  </td>
<td>0  </td>
<td>0  </td>
<td>75.39  </td>
<td>79.27  </td>
<td>71.51  </td>

</tr>
<tr>
<td>3  </td>
<td>1326  </td>
<td>4  </td>
<td>1  </td>
<td>60.5  </td>
<td>1  </td>
<td>74.89  </td>
<td>82.3  </td>
<td>67.48  </td>

</tr>
<tr>
<td>4  </td>
<td>782  </td>
<td>3  </td>
<td>2  </td>
<td>47.5  </td>
<td>1  </td>
<td>74.39  </td>
<td>78.24  </td>
<td>70.55  </td>

</tr>
<tr>
<td>5  </td>
<td>312  </td>
<td>1  </td>
<td>2  </td>
<td>46  </td>
<td>1  </td>
<td>48.35  </td>
<td>53.83  </td>
<td>42.87  </td>

</tr>
<tr>
<td>6  </td>
<td>1460  </td>
<td>3  </td>
<td>3  </td>
<td>110  </td>
<td>1  </td>
<td>112  </td>
<td>120.3  </td>
<td>103.6  </td>

</tr>
<tr>
<td>7  </td>
<td>1056  </td>
<td>4  </td>
<td>3  </td>
<td>148  </td>
<td>1  </td>
<td>128.2  </td>
<td>137.7  </td>
<td>118.8</td>

</tr>
</tb></table>

Now printing out the first ten lines with an extra column `W` to show the prior weights we see from row one that the average 1200 square foot, four bedroomed, detached house should sell for between 127 and 145 thousand pounds.

### PREDICTING for a table of new observations

Any number of sets of explanatory data can be used for predicting.

```
      A←800 800 1000 1000 1000 1200 1200
      B←2 3 3 4 5 4 5
      T←1 2 2 3 3 3 4
      SUBSTITUTE'BEDS,B:AREA,A:TYPE,T'
```

Here I have associated the variables `A`, `B` and `T` with the original explanatory variables using the function `SUBSTITUTE`.

```
      REFRESH 7
Prediction New offset Variable Reset to the Default
```

`REFRESH` stores the new data in a “predicting matrix”, a global variable called `newmat` which `PREDICT` will use in much the same way as `DIAGNOSTICS` uses the variable desmat set by `FIT`. The `REFRESH` right argument is the number of new cases, if any of your new data is the wrong length a message results. Of course in a function you would call it with something like `REFRESH ⍴A`. Here you see evidence that `ASLGREG` does generalised as well as ordinary linear regression, the message tells you that the default value is used for the offset associated with this set of explanatory variables (zero in this case as I have set no offset).

```
      PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>62.38  </td>
<td>52.63  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>79.27  </td>
<td>71.51  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>91.23  </td>
<td>81.72  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>134.6  </td>
<td>115.7  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>91.23  </td>
<td>81.72  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>145.7  </td>
<td>126.8  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>103.7  </td>
<td>91.38</td>

</tr>
</tb></table>

Now `PREDICT` detects that `newmat` is present and uses it to calculate predictions and limits. Note that `newmat` is expunged by `YVAR` or by `SUBSTITUTE ''`. This suggests the following sequence of functions:

To predict with the original data

```
SUBSTITUTE '', PREDICT
```

To recalculate the equation and predict

```
FIT, PREDICT
```

To change one of the explanatory variables and predict again

```
A[2]←900 (say), REFRESH, PREDICT
```

To use a different set of explanatory variables

```
SUBSTITUTE, REFRESH, PREDICT
```

### Confidence Limit Options

```
      PREDICT'SMTK'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper New  </td>
<td>Lower New  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>----- ---  </td>
<td>----- ---  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>74.51  </td>
<td>40.5  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>92.13  </td>
<td>58.65  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>103.4  </td>
<td>69.51  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>144  </td>
<td>106.3  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>103.4  </td>
<td>69.51  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>155.1  </td>
<td>117.4  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>115  </td>
<td>80.14</td>

</tr>
</tb></table>

You can calculate confidence limits for new observations (`T`, `K`).

```
      90 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>63.84  </td>
<td>51.17  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>80.43  </td>
<td>70.36  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>92.64  </td>
<td>80.3  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>137.4  </td>
<td>112.8  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>92.64  </td>
<td>80.3  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>148.5  </td>
<td>124  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>105.6  </td>
<td>89.54</td>

</tr>
</tb></table>

You can change the confidence level (90% in this case).

```
      80 2 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>66.12  </td>
<td>48.88  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>82.24  </td>
<td>68.54  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>94.87  </td>
<td>78.07  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>141.9  </td>
<td>108.4  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>94.87  </td>
<td>78.07  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>152.9  </td>
<td>119.6  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>108.5  </td>
<td>86.65</td>

</tr>
</tb></table>

You can calculate simultaneous confidence limits, in this case Bonferroni limits (2 in left argument).

```
      80 3 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>67.08  </td>
<td>47.93  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>83  </td>
<td>67.78  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>95.8  </td>
<td>77.15  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>143.7  </td>
<td>106.6  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>95.8  </td>
<td>77.15  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>154.8  </td>
<td>117.7  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>109.7  </td>
<td>85.45</td>

</tr>
</tb></table>

Or use Scheffe simultaneous limits (3 in left argument). As you would expect with so few cases the Bonferroni limits are narrower.

```
      80 4 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>66.12  </td>
<td>48.88  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>82.24  </td>
<td>68.54  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>94.87  </td>
<td>78.07  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>141.9  </td>
<td>108.4  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>94.87  </td>
<td>78.07  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>152.9  </td>
<td>119.6  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>108.5  </td>
<td>86.65</td>

</tr>
</tb></table>

Or get the program to choose the narrowest for you (4 in left argument).

```
      80 3 PREDICT 'SMTK'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper New  </td>
<td>Lower New  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>----- ---  </td>
<td>----- ---  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>100  </td>
<td>15  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>117.3  </td>
<td>33.53  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>128.9  </td>
<td>44.05  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>172.3  </td>
<td>78.03  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>128.9  </td>
<td>44.05  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>183.3  </td>
<td>89.16  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>141.1  </td>
<td>54.01</td>

</tr>
</tb></table>

Scheffe simultaneous limits for new observations.

```
      A←28⍴A⋄B←28⍴B⋄T←28⍴T
      REFRESH 28
Prediction New offset Variable Reset to the Default
```

Increase the number of cases from 7 to 28.

```
      ⍴12⍴≡80 2 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>68.39  </td>
<td>46.62  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>84.05  </td>
<td>66.73  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>97.08  </td>
<td>75.87  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>146.3  </td>
<td>104  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>97.08  </td>
<td>75.87  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>157.3  </td>
<td>115.2  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>111.3  </td>
<td>83.78  </td>

</tr>
<tr>
<td>8  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>68.39  </td>
<td>46.62  </td>

</tr>
<tr>
<td>9  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>84.05  </td>
<td>66.73</td>

</tr>
</tb></table>

The first 12 rows of Bonferroni simultaneous limits for the mean which get wider as the number of cases increase.

```
      ⍴12⍴≡80 3 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>67.08  </td>
<td>47.93  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>83  </td>
<td>67.78  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>95.8  </td>
<td>77.15  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>143.7  </td>
<td>106.6  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>95.8  </td>
<td>77.15  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>154.8  </td>
<td>117.7  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>109.7  </td>
<td>85.45  </td>

</tr>
<tr>
<td>8  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>67.08  </td>
<td>47.93  </td>

</tr>
<tr>
<td>9  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>83  </td>
<td>67.78</td>

</tr>
</tb></table>

Scheffe simultaneous confidence limits for the mean are independent of the number of cases.

```
      ⍴12⍴≡80 4 PREDICT'SMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>67.08  </td>
<td>47.93  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>83  </td>
<td>67.78  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>95.8  </td>
<td>77.15  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>143.7  </td>
<td>106.6  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>95.8  </td>
<td>77.15  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>154.8  </td>
<td>117.7  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>109.7  </td>
<td>85.45  </td>

</tr>
<tr>
<td>8  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>67.08  </td>
<td>47.93  </td>

</tr>
<tr>
<td>9  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>83  </td>
<td>67.78</td>

</tr>
</tb></table>

For this model with 28 cases the Scheffe limits are narrowest for simultaneous means. Remember that these limits are approximations see, for example, page 152 of “*Introduction to Linear Regression Analysis*”, DC Montgomery and EA Peck, Second Edition, John Wiley ISBN 0-471-53387-4.

```
      ⍴12⍴≡80 3 PREDICT'SMTK'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>A  </td>
<td>B  </td>
<td>T  </td>
<td>Predicted  </td>
<td>Upper New  </td>
<td>Lower New  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>-  </td>
<td>-  </td>
<td>---------  </td>
<td>----- ---  </td>
<td>----- ---  </td>

</tr>
<tr>
<td>1  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>138.3  </td>
<td>¯23.31  </td>

</tr>
<tr>
<td>2  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>155  </td>
<td>¯4.186  </td>

</tr>
<tr>
<td>3  </td>
<td>1000  </td>
<td>3  </td>
<td>2  </td>
<td>86.47  </td>
<td>167.1  </td>
<td>5.833  </td>

</tr>
<tr>
<td>4  </td>
<td>1000  </td>
<td>4  </td>
<td>3  </td>
<td>125.1  </td>
<td>214.7  </td>
<td>35.59  </td>

</tr>
<tr>
<td>5  </td>
<td>1000  </td>
<td>5  </td>
<td>3  </td>
<td>86.47  </td>
<td>167.1  </td>
<td>5.833  </td>

</tr>
<tr>
<td>6  </td>
<td>1200  </td>
<td>4  </td>
<td>3  </td>
<td>136.2  </td>
<td>225.7  </td>
<td>46.76  </td>

</tr>
<tr>
<td>7  </td>
<td>1200  </td>
<td>5  </td>
<td>4  </td>
<td>97.56  </td>
<td>180.3  </td>
<td>14.77  </td>

</tr>
<tr>
<td>8  </td>
<td>800  </td>
<td>2  </td>
<td>1  </td>
<td>57.5  </td>
<td>138.3  </td>
<td>¯23.31  </td>

</tr>
<tr>
<td>9  </td>
<td>800  </td>
<td>3  </td>
<td>2  </td>
<td>75.39  </td>
<td>155  </td>
<td>¯4.186</td>

</tr>
</tb></table>

For new observations (unlike means) the Scheffe simultaneous limits do depend on the number of observations. On the subject of simultaneous limits for many cases Montgomery and Peck warn “the individual intervals may be so wide that they are practically useless”. What this tells me is that there is so much variation in house prices not explained by this model that it is not practical to draw a boundary below which not even one out of 28 houses could be expected to fall.

### A General Model

```
      KILLED←0 5 15 28 37 49 50
      DOSE←⍳7
      START
Regression Volume 1 Version 2
A.M.Sykes & E.P.Morgan, February 1993
Copyright(C) British APL Association 1991-1993

      YVAR'KILLED'
Y variable is KILLED
Units set to 7

      ERROR 3
Error Chosen is BINOMIAL
Use NVAR to declare the N-variable

Link selected is LOGIT
      NVAR'50+DOSE-DOSE'
```

Fairly standard up to the last command, Alan Sykes's data on 50 insects exposed to different dose levels of insecticide. Alan would have said `N←7⍴50` and `NVAR'N'`. If I specify an APL expression that returns the right number of 50s based on the length of `DOSE` then, if I later substitute `DOSE` with another variable for predicting, I get an appropriate new NVAR automatically. It is not necessary but by taking pains now I can be lazy later on.

```
      FIT'GM+DOSE'
Cycle     Deviance     df     Scale Parameter
-----     --------     --     ----- ---------
4         8.343        5      1
      DISPLAY 'ESTP'
Var       Est        Std Err     t-stat     p-val
---       ---        --- ---     ------     -----
GM        ¯5.089     0.5356      ¯9.503     0.0002182
DOSE       1.333     0.133       10.03      0.0001687
      DIAGNOSTICS'XNFO'
Unit     DOSE     Out of     Fitted Value     Observed
----     ----     --- --     ------ -----     --------
1        1        50          1.142            0
2        2        50          4.073            5
3        3        50         12.59            15
4        4        50         28.04            28
5        5        50         41.44            37
6        6        50         47.42            49
7        7        50         49.29            50
      SUBSTITUTE'DOSE,NEWDOSE'
      NEWDOSE←¯0.5+⍳8
      REFRESH 8
Prediction New offset Variable Reset to the Default

      newnname
50+NEWDOSE-NEWDOSE
```

I have set up `NEWDOSE` to get a table of predicted results for the intervening extra half a dose strengths and `REFRESH` has inspected `nname` and made the substitution to create `newnname` that will provide an appropriate n-variable for use with `NEWDOSE`.

```
      PREDICT'XNPUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>DOSE  </td>
<td>Out of  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>----  </td>
<td>--- --  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>0.5  </td>
<td>50  </td>
<td>0.593  </td>
<td>1.186  </td>
<td>0.2947  </td>

</tr>
<tr>
<td>2  </td>
<td>1.5  </td>
<td>50  </td>
<td>2.178  </td>
<td>3.57  </td>
<td>1.313  </td>

</tr>
<tr>
<td>3  </td>
<td>2.5  </td>
<td>50  </td>
<td>7.366  </td>
<td>9.931  </td>
<td>5.374  </td>

</tr>
<tr>
<td>4  </td>
<td>3.5  </td>
<td>50  </td>
<td>19.8  </td>
<td>22.91  </td>
<td>16.85  </td>

</tr>
<tr>
<td>5  </td>
<td>4.5  </td>
<td>50  </td>
<td>35.66  </td>
<td>38.34  </td>
<td>32.65  </td>

</tr>
<tr>
<td>6  </td>
<td>5.5  </td>
<td>50  </td>
<td>45.21  </td>
<td>46.72  </td>
<td>43.1  </td>

</tr>
<tr>
<td>7  </td>
<td>6.5  </td>
<td>50  </td>
<td>48.64  </td>
<td>49.23  </td>
<td>47.62  </td>

</tr>
<tr>
<td>8  </td>
<td>7.5  </td>
<td>50  </td>
<td>49.63  </td>
<td>49.83  </td>
<td>49.22</td>

</tr>
</tb></table>

```
      PREDICTED
0.593 2.178 7.366 19.8 35.66 45.21 48.64 49.63
```

The function `PREDICTED` returns a vector of predictions.

```
      NEWDOSE←3 4 5
      NEWNVAR'100 200 300'

      REFRESH 3
Prediction New offset Variable Reset to the Default
      PREDICT'XNPUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>DOSE  </td>
<td>Out of  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>----  </td>
<td>--- --  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>3  </td>
<td>100  </td>
<td>25.18  </td>
<td>31.15  </td>
<td>20.02  </td>

</tr>
<tr>
<td>2  </td>
<td>4  </td>
<td>200  </td>
<td>112.2  </td>
<td>124.1  </td>
<td>99.81  </td>

</tr>
<tr>
<td>3  </td>
<td>5  </td>
<td>300  </td>
<td>248.7  </td>
<td>261.5  </td>
<td>232.7</td>

</tr>
</tb></table>

You can take control of the n-variable used for predicting by specifying it explicitly.

### <h3>Using offsets</h3>

```
      Y←2 1 3 2 5 10
      X←⍳6
      SINK←START
      SINK←YVAR'Y'
      ERROR'BIN'
Error Chosen is BINOMIAL
Use NVAR to declare the N-variable

Link selected is LOGIT
      NVAR'10+X-X'

      OFFSET'.2×X'
Offset Variable is .2×X

      FIT'GM+X'
Cycle,Deviance,df,Scale Parameter
-----,--------,--,----- ---------
4,9.979,4,1
      SUBSTITUTE'X,NEWX'

      NEWX←6+⍳4
      REFRESH 4
Prediction New offset Variable is .2×NEWX

      newoffname
.2×NEWX
      newnname
10+NEWX-NEWX
      PREDICT'XNZMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>X  </td>
<td>Out of  </td>
<td>Offset  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>--- --  </td>
<td>------  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>7  </td>
<td>10  </td>
<td>1.4  </td>
<td>8.797  </td>
<td>9.575  </td>
<td>7.035  </td>

</tr>
<tr>
<td>2  </td>
<td>8  </td>
<td>10  </td>
<td>1.6  </td>
<td>9.397  </td>
<td>9.849  </td>
<td>7.88  </td>

</tr>
<tr>
<td>3  </td>
<td>9  </td>
<td>10  </td>
<td>1.8  </td>
<td>9.707  </td>
<td>9.948  </td>
<td>8.523  </td>

</tr>
<tr>
<td>4  </td>
<td>10  </td>
<td>10  </td>
<td>2  </td>
<td>9.86  </td>
<td>9.982  </td>
<td>8.991</td>

</tr>
</tb></table>

```
      NEWOFFSET'2 3 4 5'
Prediction New offset Variable is 2 3 4 5

      NEWNVAR'12 14 16 18'

      REFRESH 4
Prediction New offset Variable is 2 3 4 5

      PREDICT'XNZMUL'
```

<table class="aplu"><tb>
<tr>
<td>Row  </td>
<td>X  </td>
<td>Out of  </td>
<td>Offset  </td>
<td>Predicted  </td>
<td>Upper  </td>
<td>Lower  </td>

</tr>
<tr>
<td>---  </td>
<td>-  </td>
<td>--- --  </td>
<td>------  </td>
<td>---------  </td>
<td>-----  </td>
<td>-----  </td>

</tr>
<tr>
<td>1  </td>
<td>7  </td>
<td>12  </td>
<td>2  </td>
<td>11.16  </td>
<td>11.71  </td>
<td>9.746  </td>

</tr>
<tr>
<td>2  </td>
<td>8  </td>
<td>14  </td>
<td>3  </td>
<td>13.78  </td>
<td>13.95  </td>
<td>13.13  </td>

</tr>
<tr>
<td>3  </td>
<td>9  </td>
<td>16  </td>
<td>4  </td>
<td>15.95  </td>
<td>15.99  </td>
<td>15.7  </td>

</tr>
<tr>
<td>4  </td>
<td>10  </td>
<td>18  </td>
<td>5  </td>
<td>17.99  </td>
<td>18  </td>
<td>17.9</td>

</tr>
</tb></table>

If you specify your original `offset` carefully then `REFRESH` will take care of it for you but you can take control if you need to.

### Robustness

A number of places where you used to get APL errors are now trapped. I agree with Jon Sandles that when this happens something is terribly wrong with what you are trying to do, I try to tell you why.

<table><tb>
<tr>
<th><tt class="aplu">Function            </tt></th>
<th>Protect against (and stop with explanation if necessary):</th>

</tr>
<tr>
<td><tt class="aplu">COOKSDIST, PRESS, ST</tt></td>
<td><tt class="aplu">LEVERAGE = 1</tt></td>

</tr>
<tr>
<td><tt class="aplu">HATMATRIX           </tt></td>
<td><tt class="aplu">scp=0</tt></td>

</tr>
<tr>
<td><tt class="aplu">FIT                 </tt></td>
<td>The case(s) causing the problem are indicated when the solution goes outside the domain of the link function.</td>

</tr>
<tr>
<td><tt class="aplu">BOXCOXLOGLIKE       </tt></td>
<td>Y not positive. Also adjust for <tt class="aplu">alias</tt>, <tt class="aplu">gmflg</tt> and <tt class="aplu">priorwt&gt;0</tt></td>

</tr>
<tr>
<td><tt class="aplu">Several             </tt></td>
<td>Divide by zero</td>

</tr>
</tb></table>

### Corrections to version 1

<table><tb>
<tr>
<td><tt class="aplu">START:              </tt></td>
<td>“<tt class="aplu">labels</tt>” not “<tt class="aplu">lables</tt>” and include “<tt class="aplu">forder</tt>” when setting up “<tt class="aplu">sysvar</tt>” near the top.</td>

</tr>
<tr>
<td><tt class="aplu">YVAR:               </tt></td>
<td>“<tt class="aplu">labels</tt>” not “<tt class="aplu">lables</tt>” and <tt class="aplu">/</tt> before “<tt class="aplu">alias</tt>” when expunging variables near the top.</td>

</tr>
<tr>
<td><tt class="aplu">OFFSET              </tt></td>
<td>“<tt class="aplu">offset</tt>” defaults to zero not one, look for <tt class="aplu">offset←units⍴1</tt> and change to <tt class="aplu">offset←units⍴0</tt></td>

</tr>
<tr>
<td><tt class="aplu">¨MU5                </tt></td>
<td>make <tt class="aplu">RES</tt> a local variable</td>

</tr>
</tb></table>

### Implementation

I have tried to make sure that user functions written for version 1 will work with version 2. This will be completely true if you use only those functions defined in the ASLGREG manual with a version of APL that allows ambivalent functions. The functions `LP`, `FIT` and `TESTHYP` are now ambivalent, providing a left argument of zero in you current functions is all you need to do. If you are using any subroutines (that is functions whose name begins with `¨`) then you may find they have changed, in particular the link and error functions have changed and your own new link functions will need modifying.

Here are `DIAGNOSTICS` and `PREDICT` right arguments:

<table class="aplu"><tb>
<tr>
<td>  </td>
<td>DIAGNOSTICS  </td>
<td>PREDICT  </td>

</tr>
<tr>
<td>A  </td>
<td>  </td>
<td>Variance  </td>

</tr>
<tr>
<td>C  </td>
<td>Cooks Distance  </td>
<td>Cumulative  </td>

</tr>
<tr>
<td>D  </td>
<td>Dev Residual  </td>
<td>Cum Variance  </td>

</tr>
<tr>
<td>E  </td>
<td>  </td>
<td>Upper Cum  </td>

</tr>
<tr>
<td>F  </td>
<td>FittedValue  </td>
<td>Lower Cum  </td>

</tr>
<tr>
<td>G  </td>
<td>  </td>
<td>Upper Cum New  </td>

</tr>
<tr>
<td>H  </td>
<td>  </td>
<td>Lower Cum New  </td>

</tr>
<tr>
<td>K  </td>
<td>  </td>
<td>Lower New Obs  </td>

</tr>
<tr>
<td>L  </td>
<td>Leverage  </td>
<td>Lower limit  </td>

</tr>
<tr>
<td>M  </td>
<td>  </td>
<td>Predicted  </td>

</tr>
<tr>
<td>N  </td>
<td>Outof  </td>
<td>Out of  </td>

</tr>
<tr>
<td>O  </td>
<td>Observed  </td>
<td>Observed  </td>

</tr>
<tr>
<td>P  </td>
<td>  </td>
<td>Predicted  </td>

</tr>
<tr>
<td>R  </td>
<td>Residual  </td>
<td>  </td>

</tr>
<tr>
<td>S  </td>
<td>Std Residual  </td>
<td>All substitutes  </td>

</tr>
<tr>
<td>T  </td>
<td>t-Residual  </td>
<td>Upper New Obs  </td>

</tr>
<tr>
<td>U  </td>
<td>  </td>
<td>Upper limit  </td>

</tr>
<tr>
<td>V  </td>
<td>Valid Fit  </td>
<td>Valid  </td>

</tr>
<tr>
<td>W  </td>
<td>Priorwt  </td>
<td>Priorwt  </td>

</tr>
<tr>
<td>X  </td>
<td>All explanatory  </td>
<td>All explanatory  </td>

</tr>
<tr>
<td>Y  </td>
<td>  </td>
<td>Observed  </td>

</tr>
<tr>
<td>Z  </td>
<td>Offset  </td>
<td>Offset  </td>

</tr>
<tr>
<td>1  </td>
<td>X  </td>
<td>X  </td>

</tr>
<tr>
<td>?  </td>
<td>This list  </td>
<td>This list</td>

</tr>
</tb></table>

The `V` for valid option confirms that these explanatory values lead to a valid prediction or fitted value when the link function is applied. The numbers 1, 2, ... refer to the first, second, ... explanatory variable and when you use argument ? their names are listed.

(webpage generated: 18 February 2006, 02:17)
