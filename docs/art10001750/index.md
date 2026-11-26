---
vid: '10001750'
title: Mandelbrot Sets
authors:
- Ray Cannon
volume: '7'
issue: '4'
page: '110'
status: not online
issue_tab: n4-april-1991
---

Ray Cannon
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 110](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=112)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>Mandelbrot Sets by Ray Cannon</p>
<p>Let me sayfirst of all, that this article has very little to do with APL. I did write some Mandelbrot code in APL for an IBM PC compatible, butit took too long to calculate even the simplestareas.</p>
<p>Thereasonit takes so long is that a single image 480 x 480 pixels in size, requires 1,200,000,000 oddfloating points instructions.</p>
<p>Question: Whatis the Mandelbrot set? Answer: It is the set of all the connected Juliansets. (Ask a silly question and...)</p>
<p>OK,think of a number, square it, add the number you originally thought of (2,4,6), square it, add first number again (36,38), square it , add the first number (1444,1446)... Nothing much strange there, the result rapidly rises. Try -1 (- 1,1,0,0,-1,1,0...), or zero (0,0,0,0...). All numbers seem tofall into these 2 groups. Hither the number shoots off towards infinity, or the result stabilises in a cycle (which mayonly have one element).</p>
<p>Nowtry it with complex numbers, plotting each starting point on the complex plane (x/y graph)as either black (cycles) or white (shoots off). The result is a Mandelbrot diagram. (To speed things up, please note that ALL points outside the circle of radius 2 starting at the origin 0,0 are white, so only points INSIDE this circle need be considered.)</p>
<p>Thus the Mandelbrot set can be described via the transformation of z goes to z squaredplus c, where ¢ is a complex number.</p>
<p>To producea picture of part of the Mandelbrotset, follow the following steps:</p>
<p>1) Decide on the numberof(pixels) points in your picture (say 480x480) and map these onto a part of the complex plane within the region -2.0 to +0.5 on the real(x) axis, and -1.25 to +1.25 on the imaginary (y) axis.</p>
<p>2) Each pixel represents a point within a small section on the complexplane. Loopforeach pointin turn, taking its co-ordinates as the value for c , witha starting valuefor z of zero.</p>
<p>3) Por each ce , calculate the value of z1 by squaring z and adding c.</p>
<p>4) Reset z to the value of z1 andrepeatstep 3).</p>
<p>110</p>
<p>VECTOR Vol.7 No.4</p>
<p>5) Continue doing steps 3) and 4)until either the absolute valueofz is greater than 2, or the loop countis greater than an arbitrary value (say 256).</p>
<p>6) Set the colourof the pixel dependenton the loop count. Plot the point in black if it repeated, or reached the maximum value chosen.</p>
<p>7) Go on to the next point.</p>
<p>See the function called MANDOT fora specification in APL ofsteps 3, 4 and5. (I have written MANDO? in a very simple manner which may become apparent later.)</p>
<p>One methodof speeding things upis to utilise the fact that large areas in any one picture contain the same value, and as the MANDELBROTset is connected there are no local maxima or minima . (Please don t ask me to prove that statement.)</p>
<p>From this it became apparent to me,thatif all the points on the boundary of an area have the same value, then all the points within the boundary have the SAMEvalue asthe boundary. This can be usedin a kind of binary search type algorithm.</p>
<p>So, define a square within the area to be mapped, (say 128 x 128 pixels). Calculate the values on the boundary, andif they are all the same,fill the complete area with that value.</p>
<p>If boundary has notthe same value, divide the square into 4 smaller squares (64 x 64) andrepeat the process on each of the smaller squares. Although the process can be repeated until the square is 2x2, due to the overheads involved with this process, squares of about 8 x 8 may not be worth sub-dividing.</p>
<p>This method reduces the number of calculations required in the BLACK area from an X squared to an X times 2 (plus) where X is the size of the square filled . (I know the square has4 sides, not 2, but boundaries between adjacent squares are commonand only need to be calculated once.)</p>
<p>As [have said, APL is to slow to do this work in a reasonable length of time. So I wrote my MANDELBROTcalculator in C with the MANDOTfunction written in PC assembler usingthe floating point maths co-processor.</p>
<p>I did find it useful however, to model the 80387 floating point co-processor assembler code in APL. This let me write and test the basic assembler code within an APL environment. Thetop level function MAN can be compared with the resulting assemblerlisting.</p>
<p>111</p>
<p>VECTOR Vol.7 No.4</p>
<p>(NOTEon 80387 maths chip. The 80387 chip works ontheprinciple of a floating stack, and has8 internal stack registers. Most instructions act between the top of the stack and other stack registers. Programming it is a bit like programming a pocket calculator in reverse polish notation .)</p>
<p>The APLlisting and the resulting assembler code are shown below.</p>
<p>Notes on Functions Functions Comment</p>
<p>MANDOT Noxmal (but very simple) APL MAN Produces same result as MANDOT, calls FP fns FADD Floating point FADD machine code instruction simulator FLD Floating point FLD (load from stack) FLDM Floating point FLD (load from memory) etc.</p>
<p>assembler to do the MANDELBROT calculation. called from ¢ #C calling syntax man(x,y)" }xeturn mandot value for given U and V values @CODE SEGMENT BYTE PUBLIC CODE</p>
<p>push bp MAN PROC near</p>
<p>nov bp, sp Eninit jelear stack Kor ax,ax solear AX yatack constant 4 fld q{val4] #Push 4 into stack 4 iget x,y u and v from parameter string fld q{bp+oc] : atack ye fld q[bp+04] : stack xyda fld it : stack vxyd fld 1 ? stack uvxy4 XOX Cx,cx :Clear registers xor dx,dx mov dl,off Initialise loop control mov bx,1 Main loop stack uyayd tpush y stack yuvay4 ¥2 ptack {y2) uvxy4 scale x2 spush x stack x(y2) uvxy4 1x2 atack (x2) (y2) uvay4 vexchange stack xlyZ)uv(x2)y4 new ¥ value calculations calc 2x stack (2x) (y2) av (xady4 teale 2xy stack (2xy) (y2)uv(x2)y4 scale axy+v stack ¥(y2)uv (x2) y4 jetore new Y atack (y2) uv (x2) ¥4 wnew X value calculations spush x2 stack (x2) (y2)uv (xa) ¥4 feale x2-y2 stack (x2-y2) (y2)uv(x2)¥4 sealc x2-y2+u stack Xty2)uv(x2)¥4 store new K stack (x2) (y2) uvxya feompare x2+y2 &gt; 4.0 reale y2+x2 stack (y2+x2) uvxY4 jcomp vs 4,0 stack uvxYa check answer and loop or exit fnstew ax</p>
<p>112</p>
<p>VECTOR Vol.7 No.4</p>
<p>and ax,041¢0 jz FOUND :Found to be 4 or more inc bx nerement loop control cmp bx,dx est against loop max ext loop jot found go return 0</p>
<p>j7Found so return count</p>
<p>;eleax return value ignore top byte</p>
<p>APL Functions</p>
<p>Vv FADD ARGS Vv FADDP ARGS;RES (1] #30387 FADD [1] 930387 FADDP [2] ARGS+~2+S5P[01,STLARGS] [2] RES++/STCARGS] (3] SPACK[0]++/ARGS [3] STACKCARGS[0]]+RES (4] SP+s+STACK C4] STACK+14SPACK v (5) ST+8+STACK v</p>
<p>v FLD MEM V FLDM VAL [1] 8 30387 PLD {1] 930387 LOAD FROM MEMORY [2] STACK+ST(MEM] , STACK (2] SPACK+VAL, STACK [3] SP+atSPACK [3] ST+B4STACK v v</p>
<p>Vv FLDZ v FMUL ARGS [1] 30387 FLDZ [1] 930387 FMUL [2] STACK+0, STACK [2} ARGS+2+ST[01, STLARGS] [3] SP+e+STACK [3] SPACKLO]+*/ARGS v [4] ST+B+STACK v</p>
<p>v FNINIT v FSTP ARG [1] 030387 FINIT [4] 030387 FSPP [2] aINIT FLOATING POINT (2] SPACKCARG]&lt;STACK[0] [3] SPACK+.0 [3] SPACK+1+STACK [4] ST+8t+STACK [4] SP«B+STACK v v</p>
<p>113</p>
<p>VECTOR Vol.7 No.4</p>
<p>vy FSTPM NAME v FSUB ARGS Cal n30397 FSTP STR TO MEMORY {11 A30387 FSUB C2] 4&amp;NAME,'«ST[O]' C2] ARGS+ 24+ST[0],ST[LARGS] C3] STACK+1+STACK £3] STACK(OJ+-/ARGS C4) ST+84+STACK C4] SI+84+ STACK v</p>
<p>v FXCH ARGS [1] a30387 FXCH [2] STACK (ARGS[0]3+SP[ARGS(1]] [3] STACK LARGS(1]}+ST[ARGS(O]} C4] ST+8+ STACK v</p>
<p>V R&lt;U MAN V;MAT; COUNT; ST; STACK Y RY MANDO? U;X;¥;X2;Y2;AL C13 ASIMULATE ASS MANDELBROT C17 ARETURN MBP VALUE FOR U V [2] Reo (2) ALONG WINDED LISTING - C3] MAT+*O [3] RCOMPARE AGAINST MAN C4] COUNT+O £4] AL+t cs] FNINIT £5] XeX24U 6] PIDM 4 c6] Y¥Y2+V {7J FLIDZ C7] EP:X2*+X2X C8] FLD 0 CsJ Y2+Y2xy C9] FLDM U [9] Y+yxX [10] FLEDM ¥ C10) X+Y (14) LP: C11) Y«¥+X (12) FLD 3 (12) YeY+V (13) FMUL 0 0 [13] X+X2 [14] FLD 3 C14] X+X-Y¥2 C15] FMUL [15] X+X+0 (16) PXCH [16] uadveoso X2*X2+Y2 [17] FADD £17] X2+X2-4 £18] FMUL 18] +(X220)/END nore (19) FADD C19] AL*AL+1 £20] FSTP (20) &gt;+(AL&gt;255)/END (21) FLD 3 [24] X2+X £22] FSUB 0 1 [22] Y2ey {23] FADD 2 [23] +LP £24] FXCH O 4 [249 END: R+AL C25] FADDP 1 0 v (26) FSUB 5 [27] FSTPM' MAT! [28] + (MAT20) /FOUND [29] COUNT+COUNT+1 [30] +(COUNT&lt;256)/LP [34] FOUND: R+COUNT v</p>
<p>144</p>
<p>VECTOR Vol.7 No.4</p>
<p>TECHNICAL SECTION This section of VECTORis aimedprincipally at those of our readers who already know APL. It will contain items to interest people with differing degrees of fluency in APL.</p>
<p>Contents NED:A nesting editor for APL*PLUS/PC Olle Evero 116</p>
<p>Thoughts ond+f g A Maurice Jordan 119</p>
<p>Full Screen Methods with APL2 Peter Branson 126</p>
<p>A Note on the Match Function in APL Joseph L.F. De Kerf 133</p>
<p>The Steam-Hammerand the Fly Gerard A Langlet 138</p>
<p>115</p>
</details>
