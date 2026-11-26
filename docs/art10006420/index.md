---
vid: '10006420'
title: A Standard Font Dialogue Box using •NA
authors:
- Duncan Pearson
volume: '10'
issue: '1'
page: '117'
status: not online
issue_tab: n1-july-1993
---

Duncan Pearson
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 117](../10/1/VOL.10-NO.1-JULY-1993.pdf#page=119)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.1</p>
<p>A Standard Font Dialogue Box from Dyalog APL/W using UNA by Duncan Pearson</p>
<p>The following is a description of the use from Dyalog APL/W ofthe common dialogue box for choosing fonts within Windows 3.1, Dyalog APL/W does not provide a means of doing this within its GUI interface, but with OWA it is possible to call any function in a dynamic link library. The sets of functions which drive Windowsare all located in DLLs and so with access to the Software Developer's Kit manuals, or their equivalent, we can use parts of the Windows GUI that Dyadic did not include in Dyalog APL, perhaps for reasons of compatibility with Motif under UNIX.</p>
<p>Theprocess of using a standard font dialogue boxisin five parts: 1) Geta block of memory in which to put the current font definition. We usethe GlobalAl loc function to get the memory and the Global Lock function to lock it into place andtell us whereit is.</p>
<p>2) Put the currentfontdata into the block using the hmemcpy function that copies data from onelocation to another.</p>
<p>3) Pass the addressof the block to the ChooseF ont function as part ofa complicated data structure.</p>
<p>4) Read the changed data from the blockinto APL using the same hnemepy function that we used to copyit into the block.</p>
<p>5) Free the memory for use by other applications, using the GlobalFree function which takes the identifier (returned by GlobalA1 loc) and returns the memory to the pool.</p>
<p>All the functions used are in the KERNEL.EXE dynamic link library except ChooseFont which is in COMMDLG.DLL. Both of these DLLs are part of Windows.</p>
<p>The font data structure, in which the font specification is passed to and from the ChooseFont function,is listed in the appendix butit is basically five integers (two bytes each) followed by eight single byte values followed by a null terminated string. Thefirst andlast of the integers are the size and weightof the font, the first three of the byte values, if they are non-zero, indicate bold, underline and strikeout and the null terminated string is the font name.</p>
<p>117</p>
<p>VECTOR Vol.10 No.1</p>
<p>The way that we use these C functionsis to associate them with function names in the workspace. Weuse the OWA function which associates a namein the APL workspace(the left argument) with a function in a DLL, specified in the right argument, That name becomesofclass 3 (a function) and whenever weuse that namein an APL expression the function in the DLLis called. The way that the arguments to the APL function are translated into the arguments of the DLL function and that the result of the DLL function is translated into an APL object is determinedbythefirst and last parts of the right argumentto DWA.</p>
<p>Memory Allocation For example:</p>
<p>alloc'QWA'U2 kernel.exe.P16|GlobalAlloc U2 U4!</p>
<p>associates the name alloc in the workspace with the DLL function GlobalAl loc. If we call al loc with twointeger arguments</p>
<p>handletalloc 0 50</p>
<p>the first will be converted into a two-byte unsigned number(the memory type) and the second into a four-byte unsigned number (how many bytes we want). The two-byte result from Globalalloc will be converted back into a positive integer.</p>
<p>Theeffect of the function is to reserve 50 contiguousbytes of global memory for our use, the returned result being an identifier for that memory. We needto fix the location of that memory so that we can write data into it. GlobalLock is the Windowsfunction that fixes memory andit returns the address at whichit has fixed it. We pass the two-byte positive integer identifier and get back a four-byte integer pointer.</p>
<p>' lock'QWA'TY kernel.exe.Pi6|GlobalLock U2! ptr+lock handle</p>
<p>Copyingthe Font Data To read from andwrite to ourlittle block we use a function called hmemcpy.It is avery simple function butit is very versatile. It takes two pointers, a source and a destination, and a numberof bytes to copy.It copies the specified number of bytes from the source to the destination. The versatility comes in the way we associate the function.</p>
<p>118</p>
<p>VECTOR Vol.10 No.1</p>
<p>Here wewill copy the integers to the memory area. 'putints'QNA 'kernel.exe.Pi6|fmemcpy I4 &lt;I20) r4! putints ptr(~36 0 0 0 400)10</p>
<p>The arguments to the function are, from left to right, the destination pointer, the source pointer and the numberof bytes to copy.In thefirst association the source is defined as an input array of two byte integers. On execution the interpreter will take the integer vector passed,create a C array of two-byte integers and pass a pointer to that array to the DLL function. The third argument should be twice the length of the vector, as that is the actual numberof bytes. The negative numbergivenasthesize is a value in points.</p>
<p>Then we copy the byte values:</p>
<p>putbytes'QWA kernel.exe.Pié|Amemepy Iu &lt;U1f} ra! putbytes(ptr+10)(1 10 00 0 0 0) 8</p>
<p>Here weare saying that the second element of the argument, the source data, should be translated as an array of bytes. In APL parlance they are indices into AY. Notice that when wecall the function we pass not the address of the memory butthe addressof the eleventh byte of the memory, where we wantit to put the byte values.</p>
<p>Lastly we insert the nameof the font:</p>
<p>putstring' DNA \kernel.exe.Piés{hmemepy I4 &lt;o7 ru! putstring{ptr+i8)'!Times New Roman 32</p>
<p>The interesting feature of this association is the &lt;OT which specifies a translated null terminated string. When the function is called we pass a text vector whichis translated from the DAV to the normal ASCII character set and copied at the pointer. After the last character of the string the interpreter places a 0. This is included in the numberof bytes to copy so our name must be no more than 31 characters long.</p>
<p>Running the ChooseFont function we Weare now ready to call the ChooseFont function, well, nearly ready. All need to do is associate the function and run it. Unfortuna tely, while the function one only takes one argument, it is a pointer to a horrendous C data structure, part of which is the pointer to our font data. However,i f we define the structure when we associate the function, we can pass a nested array correspondingto it. As with the simple arrays, the interpreter will pass a pointer to it. The official</p>
<p>119</p>
<p>VECTOR Vol.10 No.1</p>
<p>structure definition is part of the appendix but the line below defines it well enough for the association. choose_struct+'{T4 I2 72 Iu r2 T4 vita] Iu ry ry r2 re g2 I2 52}! changefont'ONA'U commdlg.d11.P16|ChooseFont =',choose_struct</p>
<p>Things to note are that the U1[4]is an array of red, green and blueintensities for the display, the second 14is the pointer to our font data and the third Iis the combination of some Boolean valuestotell the function whatfonts to allow and whether there is meaningful data in the font structure. The = before the font structure in the association is an indication to the interpreter that when the function has run we would like to see what it has done to the data in the structure, We are most interested here in the colour part.</p>
<p>The definition of the individual bits is included in the appendix as a set of #define CF_*** 0x00*#* statements thatare pretty self-explanatory. Weare going to use CF_SCREENFONTS, CF_INITTOLOGFONTSTRUCT and CF_EFFECTS, the last of these to enable the user to choose underline and colour.</p>
<p>flags+2i1 010000012</p>
<p>Thefirst element of the structureis its length in bytes and everythingelse in it that I haven t mentioned canbeset to 0. Thusthe functioncall looks like this.</p>
<p>changefontci5 46 0 0 ptr 0 flags(0 128 128 0}</p>
<p>Weget a dialogue boxthat lookslike this:</p>
<p>Eont: FontStyle: iii latic A : Regular Terminal Bold Times New Roman MESES Elfects Sample O Stikeout Ed Undestine Color Mi Teal |</p>
<p>120</p>
<p>VECTOR Vol.10 No.1</p>
<p>and the result: 1 86 0 0 137822208 360 3921 128 0128 0 09000 0 8452 00</p>
<p>Because wespecified one of the arguments as = the data for that argumentis returned with the result of the function in a nested array, the first element being the result of the function and the second, the contents of the structure. Theresult of the function is non-zero,indicating that the user pressed OK, and the seventh elementof the structure is 128 0 128 0, indicating that he changedthe colour from teal to purple (takesall sorts to make a world). Wecan use this indication to set the colour of any text associated with this font.</p>
<p>Retrieving the Font Data Wenowneedto query the fontstructure to find whatfontattributes the user has changed. To get data out of our block of memory weuse the same function that we used to get data in. This time we specify an array as the destination (first argument) and the pointer to the memory blockas the source (second argument).</p>
<p>getints'ONA 'kernel.exe.Pi6|hmemcpy &gt;I2C] Ie ru! getbytes'OWA 'kernel.exe,Pié|hmemcpy &gt;UiC] Is rH! getstring'OWA 'kernel.exe.Pib|hmemcpy &gt;0T Iu r4'</p>
<p>Instead of &lt;I2[ ] (correspondingto an integer vector argument) which means</p>
<p>- take a vector - turn it into integers - pass its address to the function - then forgetit</p>
<p>wehave &gt;12[ ] (corresponding to an integer argument) which means</p>
<p>- allocate space for the specified numberofintegers . ~ pass its address to the function ~ convert whatthe function leaves there into an int. vector, 2 bytes per number - return the vector</p>
<p>Whatwepassas this argumentis the expected length of the result. getints 5 ptr 10 8B 0 0 0 700</p>
<p>In thecaseof the integers we expectedfive integers(first argument) which meant that it had to copy ten bytes (third argument). Thefirst integer is the new point size and thefifth is the weight.</p>
<p>121</p>
<p>VECTOR Vol.10 No.1</p>
<p>Wethengetthe attributes. We can only set the underline anditalic but we might wantto knowif the user hasset strikeout in order to warn him thatit will not be implemented. getbytes a(ptr+10)3 oo4</p>
<p>Finally we get the name: getstring 32(ptr+i8)32 Courier New</p>
<p>Cleaning Up It is good practice to give the memory back to Windowsevenif it is only 50 bytes, and for this we use the Global Free function.</p>
<p>free'QNA'U2 kernel.exe.P16|GlobalFree U2" free handle Q</p>
<p>As youcan seeit takes a memory identifier and returns 0 if it managed to free the memory.If it had failed it would have returned the handle. So that was easy wasn t it? You would be surprised what you can do with the DLLfunctions. You can be serious and write a low-levelinterface to Oracle using the functions in the ORA6WIN.DLL supplied with EXCELor you can beflippant and start writing raw graphicsall over other applications windows, whatever flips your switch.</p>
<p>Font Sizes Ashort note aboutfont sizes. You pass a pointsize to the font structure and get a point size out. Font sizes in Dyalog APL/W arespecified in pixels. There are 72 points to the inch and you can calculate the numberofpixels to the inch from the DEVCAPS property of the root object. This is unfortunately notall. For some reason, best known to the morons at Microsoft, when you specify 36 point in the font structure it showsthe fontin 36 point (accordingto the above definition) but says thatit is in 27 point. Likewise, if the user chooses 36 point in the box, the numberin the font structure is -48 and the user sees 48 point. Don t ask me to explain it.</p>
<p>122</p>
<p>VECTOR Vol.10 No.1</p>
<p>Appendix Thefull APLsessiontranscript:</p>
<p>alloc'GWA'U2 dkernel.exe.P16|GlobaldAlloc U2 Ua! handletalloc 0 56 lock'OWA'IG kernel.exe.Pi6{GlobaiLock U2! ptr+lock handle putints'DWA 'kernel.exe.Pi6|hmemcpy Ira &lt;I2C} It putints ptr(~36 0 0 0 400)10 putbytes'OWA kernel,exe.Pis|hmemepy ru &lt;U1iC} rat putbytes(ptr+10)(1 1000000) 8 putstring'QWA 'Aernel,exe.Pi6|hmemcpy I4 &lt;7 Iut putstring(ptr+18)'Times Yew Roman! 32 choose_struct+'{Ie ¥2 22 I4 I2 r4 ULC4] Ia In Fy 2 4 U2 2 I2}' "changefont'OWA'Y commdig.dl1.Pi6|ChooseFont =',choose_struct flagsezsioi1aoacooods changefontci5 46 0 0 ptr 6 flags(G 128 128 0) 4 46 0 6 4137822208 360 324 128 0 1280 90060 4452 00 getints'GVA 'kernel.exe.P16|hmemcpy »I2C] Iu ru! 'getbytes'QNA 'hernel.exe.P16{hmemcpy &gt;U1C] r4 ra! tgetstring'OWA 'kernel.exe.Pic|hmemcpy &gt;0T r4 Int getints 5 ptr 10 58 0 0 0 700 getbytes 3(ptr+10)3 ood getstring 32(ptr+18)32 Courier Nev 'free'QWA'U2 Aernel.exe.P16|GiobalFree U2! free handle 0</p>
<p>Thefont data structure from WINDOWS.H /* Logical Font */ #define LF_FACESIZE32 typedef struct tagLOGFONT { int lfHeight; int lfWidth; int lfEscapement; int 1fOrientation; int ifWeight; BYTE lfItaliec; BYTE 1fUnderline; BYTE IfStrikeGut; BYTE ifCharSet; BYTE 1fOutPrecision; BYTE IfclipPrecision; BYTE \fQuality; BYTE 1fPitehAndFamily; char ifFaceName(LF_FACESIZEJ; } LOGFONT;</p>
<p>123</p>
<p>VECTOR Vol.10 No.1</p>
<p>Thedata structure to pass to the ChooseFont function from COMMDLG.H typedef struct tagCHOOSEFONT { DWORD iStructSize; faa HWND hwndOvner; /* callerts window handle */ HDC HDC; /* printer DC/IC or NULL «f LOGFONT FARe IpLogFont; /* ptr. to a LOGFONT struct */ int iPointSize; /* 10 « size in points of selected font */ DWORD Flags; /* enum. type flags af COLORREF rgbColors; /* returned text color wf LPARAM 1CustData; /« data passed to hook fn. «/ UINT {CALLBACK Ipfniook)(HWND, UINT, WPARAM, LPARAM); /* ptr. to hook function af LPCSTR IpTemplateName; /» custom template name af HINSTANCE hinstance; /*« instance handle of.EXE that » contains cust. dlg. template */ LPSTR lpszStyle; /» return the style ffeld here » must be LF_FACESIZE or bigger »/ UINT nFontType; /* same value reported to the EnunFonts * call back with the extra FONTTYPE_ x bits added «/ int nSizeMin; /*« minimum pt size allowed 2 */ int nSizeMax; /* max pt size allowed if af /* CF_LIMITSIZE is used af } CHOOSEFONT;</p>
<p>Thesettings for the flags in the CHOOSEFONT structure - from COMMDLG.H. #define CF_SCREENFONTS oxo0000001 #define CF_PRINTERFONTS 0x00000002 #define CF_BOTH (CF_SGREENFONTS C¥_PRINTERFONTS) adefine CF_SHOWHELP oxoo00cco4L 4define CF_ENABLEHOOK oxoocco00aL édefine CF_ENABLETEMPLATE oxoo000010L a@define CF_ENABLETEMPLATEHANDLE 0x000000201 édefine CF_INITTOLOGFONTSTRUCT oxoc0cc040L #define CF_USESTYLE ox00000080L #define CF_EFFECTS ox00000100L #define CF_APPLY 0x00000200L #define CF_ANSIONLY 0x00000400L #define CF_NOVECTORFONTS 0x00000800L a#define CF_NOOEMFONTS CF_NOVECTORFONTS a@define CF_NOSIMULATIONS 0x00001000L adefine CF_LIMITSIZE 0x00002000L adefine CP_FIXEDPITCHONLY ox0o004000L #define CF_WYSIWYG ex00008000L /* must also have CF_SCREENFONTS « &amp; CF_PRINTERFONTS +/ #define CF_FORCEFONTEXIST Ox00010000L a4define CF_SCALABLEONLY 0x00020000L #define CF_TTONLY Ox000K0000L adefine CF_NOFACESEL Ox00080000L #define CF_NOSTYLESEL Ox00100000L #define CF_NOSIZESEL Oxd0z0000cL</p>
<p>124</p>
<p>VECTOR Voi.10 No.1</p>
<p>The MAGIC goes away - Opening the Boxes by Mike Day</p>
<p>Introduction A year ago Bill McLean brought the game of MAGICto the pages of Education Vector, Vector Vol.9, No.1 (pp.25-29). In the next issue, 9.2, Dave Ziemann discussed the game, and presented a method for solution and a set of APL*PLUS Tl functions to do the job. It was a quite brilliant treatment, but as Dave said, 1 am left with the feeling that there s probably a very simple way of knowing what key to press from any given state, but it eludes me for the moment. Hereis one way of doing just that. I want to share it with you because it throws light on someaspects of A-level-ish Mathematics. The APL is quite neat as well.</p>
<p>In case you don t remember, in MAGIC youare given a starting 3*3 matrix or box of random 0s and 1s. You are asked to pick one or more of nine given standard boxes I'll call them MagicBoxes to complementthe starting box to achievethe target box, which wecall win.</p>
<p>Bill s article described a program to interact with the player, presenting the random starting box and showing the current position. Dave responded to his invitation to readers to providesolution algorithms.</p>
<p>Like Dave Ziemann, I ve developed my solution mostly in terms of direct definition, but usingfirst generation APL*PLUSversion8.I've also developed an analogous solutionin J(4.1) rather to my surprise.</p>
<p>Representation As in Dave's treatment, I take a global zero index origin. I don t wantto rewrite all those parts of Dave's article that apply here as well. He showedthatit s useful to be able to enumerate the 512 different possible states in a natural way as either decimal numbers 0 to 511 or as binary numbers 0 to 111111111 using some simple functions.</p>
<p>125</p>
<p>VECTOR Vol.10 No.4</p>
<p>Tve cribbed his functions DB and BD :</p>
<p>DB:1=(9p2)T w BD:21</p>
<p>BD OG11l41041 123</p>
<p>DB 123 oo01141414011</p>
<p>The functions SHOWBool and SHOWDec showstates as 3x3 boxes: ©</p>
<p>sym 'O1' o DIE + ' ft</p>
<p>SHOWBoOOL: 3 0 thlk:0ex/p w rsyml3 3 py w J,b1k,SHOWBool O14 w SHOWDec:SHOWBoo!l DB, w</p>
<p>SHOWBool 9 1p00144441060 14 08 FN expects a matrix argument 001 "444 O11</p>
<p>SHOWDec 123 O01 ait oat</p>
<p>But I'm moreinterested in the methodof solution than in presentation.</p>
<p>Why use McLean s MagicBoxes? Thespecified set of building blocks or MagicBoxes is</p>
<p>SHOWBool MagicBoxes 110 114 011 100 0610 001 000 Goo O00 110 000 011 100 414 004 110 O00 0114 0GO 000 000 100 010 001 110 111 O11</p>
<p>Dave Ziemann presents a short MAGIC game . Firstly, the function Set Vars sets the scene, including choosing which boxes to play with in this case MagicBoxes.</p>
<p>126</p>
<p>VECTOR Vol.10 No.1</p>
<p>SetVars: key+BDsboxes+w © sym+'01',bIk+' '9 wineh95 © w+0tDEX'BaseTokey'</p>
<p>SetVars MagicBoxes'! © 32 SHOWPATH win</p>
<p>9 5 6</p>
<p>110 001 000 410 001 110 000 OO4 110</p>
<p>000 110 111 41141 100 010 011 104 000 9000 001 111</p>
<p>BaseBoxes ' But suppose we hada different set of boxes; one I call BaseBoxes:</p>
<p>SHOWBOol BaseBoxes 100 040 001 000 000 O00 Goo 000 000 000 G00 000 100 610 001 000 000 000 000 000 000 G00 000 O00 100 010 901</p>
<p>Wecaneasily see that the same problem can be solved in more steps in this case:</p>
<p>SetVars 'BaseBoxes' 9 32 SHOWPATH win</p>
<p>0 1 2 5 6 7 8</p>
<p>100 010 004 000 O00 000 000 G00 000 O00 001 000 G00 000 000 000 GOO 000 100 010 004</p>
<p>000 100 410 111 114 111 111 111 100 100 100 100 401 101 101 101 000 0660 000 000 000 100 140 141</p>
<p>It is pretty obvious with this set of building blocks that we never need use more than nine moves. You can probably see quite easily that there is only onestarting state for which nine is the minimum numberof boxes, namely the complement of win,</p>
<p>127</p>
<p>VECTOR Vol.10 No.1</p>
<p>SetVars BaseBoxes' o 16 SHOWPATH win</p>
<p>Q 4 2 3 4 5 6 7 8</p>
<p>100 G10 001 000 000 000 O00 900 000 000 000 GOO 100 010 G01 000 000 000 000 000 000 G00 000 000 100 610 001</p>
<p>000 100 4110 4114 411 111 444 1141 411 111 O10 O10 010 010 116 100 101 101 101 101 000 000 000 000 006 O00 000 100 110 114</p>
<p>Wecan use more than the minimum numberof moves for anysolution, but only by adding some box 2 or4 or 6 or any positive even number of times more than necessary.</p>
<p>Solution with BaseBoxes The solution is intuitively obvious when we use BaseBoxes to provide the building blocks. Wecan also express it mathematically. Given:</p>
<p>BD START + 000100000 a2 BDWIN«e+x, 111410141244 495</p>
<p>+BD O+ DIFF + ADD START, (0.5) WIN tiaooririn 463</p>
<p>where</p>
<p>ADD: # / w</p>
<p>So we can easily solve a MAGIC puzzle using the unofficial set called BaseBoxes. But how do wesolve a real puzzle with Bill McLean s sct, MagicBoxes?</p>
<p>128</p>
<p>VECTOR Vol.10 No.1</p>
<p>Plus and Times for Boolean Numbers Wecan think of # for Booleans as equivalent to + (and * as well) for ordinary decimal numbers,since (for &lt;=&gt; read is equivalentto ):</p>
<p>xX # y &lt;=&gt; 2/)x + y &lt;o&gt; 2[x - y</p>
<p>The dyadic operation # is the sum for the Vector Space to which the Binary representationsof puzzle states belong.</p>
<p>It is obvious with either BaseBoxes or the original MagicBoxes that you can add them together any number of times, but that all even numbers are equivalent to doing nothing, while all odd numbersare just the same as only adding once. So weare discovering a system for scaling Boolean vectors x with integer numbers k such that X+ ky &lt;=&gt; x ifo &lt;=&gt; x 4 y if 1 not NN By by</p>
<p>Suppose we know x and k and that X &lt;a&gt; kz Whatis z?</p>
<p>If kis odd,ie.1 &lt;=&gt; 2 | &amp;,then</p>
<p>zZ&lt;=&gt; Xx However, if k is even, both x and z must be null vectors. k cannot be even for non-zero x. This is not quite the same as when</p>
<p>X = 0Z implies that Z is infinite unless X = 0.</p>
<p>Wecan in fact extend our system of scalar multipliers to include rational numbers of the form (p/q) where p and are integers. If p/q is reducedtoits lowest terms, p/q &lt;=&gt; 1 if both p and q are odd, &lt;=&gt; infinity if either is even.</p>
<p>129</p>
<p>VECTOR Vol.10 No.1</p>
<p>Using BaseBoxesas a Basis Let's now return to our two sets of boxes. When we don t use the SHOWBool function they appear as two 9x9 Boolean matrices:</p>
<p>(¥MagicBoxes),(9 10p' '),+BaseBoxes 1ioiroo000dd ioo0oagoov0d00g000 1idaoiroood oi1io00g0 0000 Oo1i10ocg 1000 Qoo0tooogadg ato00112 0100 ooo0100000 P2oa0c0a01021 ooaogirtooddn ootodo 41001 ooo000 14000 oootoogiiond oogeo0 100 oooo1t04 A121 ooco0og oo 10 oo0o0cdg 010141 ooo0oood0Gg G01</p>
<p>Consider the third column of MagicBoxes</p>
<p>SHOWBool 9 1p O + MagicBoxes{;2] o1if01t12000 O14 O14 000</p>
<p>This Boolean vector may be seen as signifying that the third Magic Box consists of the following elements of BaseBoxes:</p>
<p>o1io0iitooo/f/isg 1245</p>
<p>SHOWBool 3 ip * / BaseBoxes [; 1 2 &amp; 5] oat O11 000</p>
<p>So we now know trivially really how to express MagicBoxes in terms of sums of BaseBoxes. And if we have a number of MagicBoxes e.g.0 5 6 as in Dave's first example game wecantaketheir sum * / MagicBoxes C; 0 5 6] aaa ooiliaid</p>
<p>It is equivalent to the sum of BaseBoxes numbers 0 125 6 7 8 as wesawearlier: # / BaseBoxes [; 012567 8] tiraooitiira</p>
<p>130</p>
<p>VECTOR Vol.10 No.1</p>
<p>Using MagicBoxesas a Basis We now need to be able to INVERT the process. Instead of expressing combinations of MagicBoxes in terms of BaseBoxes, we need to express BaseBoxes in terms of MagicBoxes. For example, starting from the knowledge that the set 0 1 2 5 6 7 8 of BaseBoxes solves a problem, how can we deducethat the set 0 5 6 of MagicBoxes doesthe samejob?</p>
<p>Well, the 9x9 matrix MagicBoxes is really a transformation matrix from a representation with respect to the basis MagicBoxes to one in terms of BaseBoxes. For example, the vector</p>
<p>me+1i0o000041%4100nA &lt;=&gt; take MagicBoxes 0 5 6</p>
<p>is transformedto the vector bBeiitiooi2441a &lt;=&gt; take BaseBoxes 012567 8</p>
<p>using a Boolean Matrix multiplication function MULT MagicBoxes MULT mn tTiaocaizriiada</p>
<p>Wecan define MULT either by MULT: a # . A w A using Boolean operations</p>
<p>or by MULT; 2 | a+. * wa using numeric add and multiply</p>
<p>Boolean Matrix Inversion So let's find a Matrix Inverse for the array MagicBoxes the usual APL @ almostdoesthe trick, but not quite.</p>
<p>The second form of definition for MULT suggests thatif</p>
<p>x= My, for Boolean x and y and using Boolean matrix multiplication then X =MY, for numeric X,Y, 2 | X &lt;=&gt; x, 2 | Y &lt;=&gt; y and ordinary matrix multiplication, (+. *). If there is a Boolean inverse IM for M, then Y = IM X</p>
<p>so y=2| (MM)(y + 2W) for someinteger W</p>
<p>131</p>
<p>VECTOR Vol.10 No.1</p>
<p>Le. y+2Z=(IMM)jy + 2W) for someinteger Z.</p>
<p>But if IM = &amp; M then this condition is met as (IM M)is an identity matrix. HoweverIM can contain non-integerrationals:</p>
<p>+IM + HOM + MagicBoxes 0.2 0.4 0.2 O.4 0.4 0.6 0.2 0.6 0.8 0.6 0.2 0.6 0.2 0.2 0.2 0.8 0.8 0.4 0.2 0.4 0.2 0.6 O.4 O.4 0.8 0.6 0.2 0.6 0.2 0.4 0.2 0.2 0.8 0.6 0.2 0.4 0.2 0.4 0.2 6.4 0.6 0.4 0.2 0.4 0.2 O.4 0.2 0.6 0.8 0.2 0.2 0.4 0.2 0.6 0.2 0.6 0.8 0.46 0.4 0.6 0.2 6.4 0.2 O.4 0.8 6.4 0.2 0.2 0.2 0.6 0.2 0.6 0.8 0.6 0.2 0.6 90.4 0.8 0.2 0.4 0.2</p>
<p>Wenowrecall that any odd scalar multiple of a Boolean vector is equivalent to the vector itself in our nine-dimensional vector space. The same applies to matrices:</p>
<p>Let: +IM1 + 8 MagicBoxes + 5 i 2712 2 279 7173 4&amp; 371 3 71 71 7172 4 72 4 272739 2 2 4 73 74 a4 72 71 4 4 3 71 72 1 274 273 271 273 271 3 4 71 "172 71 ~=9 273 4 2 2739 4 274 2 472 "1°14 "4 39°71 «8 47374 73 2 271 274</p>
<p>The divisor 5 is in fact the determinant of the matrix MagicBoxes!</p>
<p>11120012111</p>
<p>2 | TMi +.* ba which just happens to be the same as toaoooiiaos</p>
<p>mat 1tooogiaod</p>
<p>Unfortunately I've cheated. While we could in general take the ordinary inverse and try different odd integer scalars until we light on the right one, westrictly oughtto calculate either the determinant of MagicBoxes:</p>
<p>132</p>
<p>VECTOR Vol.10 No.1</p>
<p>DET MagicBoxes 5</p>
<p>(It is interesting that the J 4.1 determinant (-/ . *) threatens to take over four hours on a portable PC (Toshiba 1850) for this problem.) ... or to gain perhaps even more insight by using a special-purpose function</p>
<p>BINV MagicBoxes taa</p>
<p>OeCkKkKORRRER hoorMonre BROCOREHA OPArRKROMSG</p>
<p>PRORPRMOQROHRS orPRROoOoOL PRROnRHGO SorPorPoRHKOA PREBRORGO</p>
<p>Icobbled BINY together from Keith Smillie s INV function in STATPACK2</p>
<p>VBINVIOIY col BeBINV a;k;sipsiz:is;ts;u;OF0 (1 a INVERT a SQUARE BINARY MATRIX... AFTER KW SMILLIE's STATPACK2 {2] A NB REAL ADD/MINUS BOTH BECOME * [3] a REAL TIMES BECOMES a Ca] a REAL DIVIDE SHOULDN'T BE NECESSARY AS DIVISOR is O OR 1 C54 a C6] a AN ALTERNATIVE METHOD is TO TAKE 2 | (DETERMINANT a) x H a C7] 0 BUT THAT NEEDS a DETERMINANT FUNCTION OF SIMILAR COMPLEXITY Cal a C9) UlO+4 © +((x/x/baae2| bea) a(2=ppa)as/pa)/OX a CHECK a is OK C103 (111 NOINY:'NO INVERSE! 0 +~bei C12) C13) OK: ptiseisek+(/pa © aralip,1] f14) 15) LooP:aC;4+s]+2&gt;is o ictul/tealitzgi] a FIND NEXT ROW {161 plte1,f]+pCu+i,1] © alt;isl+alu;is] a SWAP ROWS ca7d +(O=a[1;1])/NOIWY a SINGULAR MATRIX Cie] a DONT WEED ... ali;]+alis]l+aCi;1] DIVISOR ALWAYS UNITY IF REACH HERE Cis] atae(al;l]ai&lt;is)s.salis] a WAS a - (atsi] x 1 &lt; is)é. alts] C207 a+a(t+its|issitis,0] 9 p+p(t} a SHIFT ROWS AND COLUMNS Cas] (223 *(O&lt;k+k-1)/LOOP 0 (23) beal;piis}] a REORDER FOR ANSWER</p>
<p>It is of course very similar to Smillie s original, just replacingall x by a and both + and - by # . Note that I disabled the line involving division by the pivotcell, since we have seen that Boolean division yields either no change or an infinity and we expect the argumentto be non-singular as we are dealing with basis representations.</p>
<p>133</p>
<p>VECTOR Vol.10 No.1</p>
<p>Solution It just remains to fit the various elements together. The function PATH does the job. PATH: (KEY ADD DB w , a )/ipkey</p>
<p>The arguments a and w are the decimal representations of the start and target boxes. ADD DB w , a finds the solution in terms of BaseBoxes.</p>
<p>ADD DB win, 32 t1iaoo0riaiaida</p>
<p>The function KEY multiplies this by the Boolean Inverse of MagicBoxes, BaseTokey,calculating it in the function BaseRepYoKeyRepif it does not exist. (We see now why Set Yars expunges BaseTokey it is in case a new set of Mag icBoxes is invoked asthe building blocks.)</p>
<p>BaseRepTokeyRep: BaseTokey: 0=(NC' BaseToKey':BaseToKey+BINV DB key KEY:BaseRepfokeyRep MULT w</p>
<p>SetVars MagicBoxes' 9 +m « KEY ADD DB win, 32 100001210090</p>
<p>Finally, the resulting Boolean vectoris used to select the MagicBoxes, m/ipkey 95 6 32 PATH win 05 6</p>
<p>The remaining functions arejustutilities or display routines.</p>
<p>Conclusions The game of MAGICas presented confuses the issue (probably intentionally!) in being apparently a two-dimensional problem with 512 states. However, as Dave Ziemann showed, wereally have a 9-dimensional set of Boolean vectors. The decimal representation is helpful to my mind only to make the notation more concise. It does not add to our understandingof the transitions as Dave found. I suspect that instructive analogies might be drawn with other systems.</p>
<p>For example, theoretical physicists describe representations of microscopic space-time in terms of multiple (11?) dimensional strings embeddedin 4 real</p>
<p>134</p>
<p>VECTOR Vol.10 No.1</p>
<p>dimensions. A Boolean numberis a numericalrealisation of the smallest possible non-trivial commutative group of order 2, so that our MAGIC gameproblem is a little like a simple physics with 9 different quantum properties, each with 2 alternative states. | gather that elementary particle theory teems with interacting symmetry groups, mostly built up from simple subgroupsof order2 or 3. Casting aside speculation, we have found concise, quick methodof solution for MAGIC puzzles. There are symmetries which I have not pursued the two- dimensional view mayafterall be useful! e.g. the solutions for decimal 1, 4, 64 and 256are all identical apart from rotation. (There are four corner boxes and four edge boxes and one middle. Various symmetry groups order four and order two relate corners together and edges together.) It s possible that consideration of these symmetries might lead to an improvementin performance of Dave's algorithm, but my method is so quick and simple that I haven't followed upthis particular line.</p>
<p>Duncan Pearson wondered if a similar vector space treatment involving inversion of a transformation matrix could be applied to the N-Queens problem. I don t think so at least I haven t found it the reason probably being that you can t realise a presumably N-dimensional basis corresponding to the operation of adding a Queen.</p>
<p>However,the ubiquitous not-equal scan does make an appearance, even if not providing as much ofthe solution as Gérard Langlet s views on the powerofthis construction would lead usto believe(e.g. Vector 9, No 3).</p>
<p>APL Functions Hereisa listing of all the APL functions involved.It s only those few listed under SolutionFunctions that actually find the answer. The others are to do with setting up the problem (SetVars) and displaying one or more states (SHOWBool and SHOWDec)or the whole problem (SHOWPATH).</p>
<p>My version of Direct Definition it s not part of APL*PLUS as supplied allowslocal variables, x, y and 2, in addition to the arguments w (anda if the function is dyadic). The direct definition interpreter expects its functions to produceresults.</p>
<p>Utility Functions LAM: « ,(0Z0-0.5] w a LAMINATE IN WEW 1ST DIMENSION SetVars: key+BD 4 boxes+w o sym+'01',bIk+!' ' o wint495 o w +O40EX 'Base?oKey'</p>
<p>135</p>
<p>VECTOR Vol.10 No.1</p>
<p>SolutionFunctions ADD: 4/ w BaseRepTokeyRep:Basefoxey:0=0NC 'BaseTokey':BaseToKey+BINV DB key</p>
<p>BDi2i w DB:1=(9p2)7 w</p>
<p>KEY:BaseRepTfokeyRep MULT w MULT; a #.0 w PATH: (KEY ADD DB w , a )/ipkey</p>
<p>Display Functions KEYPath:(0, » })LAM a ,keyCw + a PAPE w J SHOWBool: 3 0 +blk:02x/p w :sym(3 3 p&amp; w ],bIk,SHOWBool 0 1 + w SHOWDec:SHOWBool DB, w SHOWPATH: (SHPO w (1 4 pUZ0;1]);SHP1(w + a KEYPath w }[1+0I0;] SHPO: i¢(1+0TO]2¢(- 0 24 +p w tw 4OtrOOLe w</p>
<p>SHP1:((3 4 pbik),SHOWBoo! DB,i+ w )LAM SHOWBool scanADD DB w scanADD:#\ w</p>
<p>VDETCOIY [0] z+DET m;isj;n;a;b;0I0 [1] a FIND THE DETERMINANT OF m (2) @ A METHOD THAT WORKS ..,. NOP OPTIMAL BUT &gt;&gt; THAN RECURSION! C3] a DET m EQUIV DET m' WHERE m'Cis] = mli3;] - &amp; x m(73J C4] a FRIES TO GET ZERO'S IN LOWER TRIANGLE (OFF DIAGONAL) (sJ [6] a DET OF A TRIANGULAR MATRIX IS JUST THE PRODUCT OF THE {7} a DIAGONAL ELEMENTS Cal C9] +( A/s/mem+. xm) /ENDI,OT0+1 [10] i+(0,n+"1t+pm){1] (11) LOOPI:&gt;(nsi+i+1)/ENDI [12] jeitrvali (133 +(1E£7202|m(i;i])/ZERO C144] mUfs]«mCj;]-mla;]xm[7; bismlaci+b; ben l1]pi} £15] +LOOPI [16] ZERO:+(a/b«0=i4m[0;1])/LOOPI £17] mCisdemlis+,(&lt;\~b)/C11 (1, 0)4m C18] isd-a (i9] +L00PI (20) ENDI: [21] a m SHOULD BE IN UPPER RIANGULAR FORM (22) z+x/ 11 am</p>
<p>136</p>
</details>
