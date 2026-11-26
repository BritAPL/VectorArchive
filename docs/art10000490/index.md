---
vid: '10000490'
title: BAA-GUI Workshop Dyalog APL Sessions
volume: '10'
issue: '3'
page: '51'
status: not online
issue_tab: n3-january-1994
---

The text of this article is not yet online.

[Read it in the PDF of the issue, page 51](../10/3/VOL.10-NO.3-JANUARY-1994.pdf#page=53)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.3</p>
<p>BAA-GUI: APLs and The Windows GUI Dyalog APL Sessions notes by Jonathan Barman</p>
<p>The Dyalog APL stream for the GU] Programming day was split into two groups. Peter Donnelly of Dyadic Systems Ltd. took one group while Adrian Smith, wearing his APL-385 hat, took the other. At lunch-time the groups swapped over. I attended Peter's tutorial in the morning and Adrian's in the afternoon, so I have described the tutorials in that order.</p>
<p>Peter Donnelly Peter took us through the basic GUIfacilities in Dyalog APL. It would be too muchto reproduceit all here, but the following fragmentof his introduction will give you the flavour of the way in which he started very simply and then gradually introduced more complex examples:</p>
<p>'pete'Qwe'form! 'pete'Qwe'form' 'Hello World! pete'Dwe'form' Hello World'(10 30) (10 10)</p>
<p>Thefirst line creates a blank form with everythingleft as a default. The next line tedefines the windowwith title. The third line redefines it again with title, position and size. Peter continued this process with more and more examples until he had covered all of the 0 functions OWC OWG OWS OWN and DQ. The coverage was quite thorough and included howto create buttons, radio buttons, list boxes, groupsetc. etc. We were shown howto run callback functions which can be sparked off when usercarries out a particular eventsuchasclicking on a buttonorselecting a control.</p>
<p>I attemptedto type in the examples as Peter ran through them. I must admitthat I had sometrouble keeping up as Peteris quite a fast typist. Several peoplein the class also had trouble, so they wisely sat and watched carefully rather than try and reproduce everything at the terminal. Fortunately Peter s examples were almost the same as those in WINTRO, which is the introductory Windows workspace supplied with Dyalog APL, so one could always work through them at a more leisurely pace on one s own. In retrospect, I think that it would have been wiser to have got everyone to run the WINTRO workspace whereall one hasto do is to press the F1 key to get the exampletext. If you have Dyalog APL,</p>
<p>54</p>
<p>VECTOR Vol.10 No.3</p>
<p>then it is definitely worth while running through the examples in this workspace before attempting to write any code.</p>
<p>After about an hour we were given an exerciseto create a fairly complex dialog box with a numberof pop-up menusofalternatives:</p>
<p>Pickers</p>
<p>Breakfast terns rSunddes Osereat OD Iosst Sausage [Baton and E99 Watties Borange Juice OC yYoghurt Pencakes</p>
<p>Beverage Spectat Requirements</p>
<p>If you choose an item containing eggs, then the following dialog box should be presented: appearESET Makhice (onae</p>
<p>@ Over Easy</p>
<p>© SunnySide_Up</p>
<p>The exercise required similar dialog boxes to pop up to specify all the incredible set of alternatives that are possible when ordering an American breakfast. Milk or cream in your coffee, how you like your toast, how well cooked yoursteak, ete. The exercise was to be completed within an hour, but I don t think that anyone could really say that they managedto finish every detail. | have not really used Dyalog APL GUIfeatures in anger, and cannot say that I am an expert, but I was quite surprised how far 1 managed to getin the time. Naturally, I] took many</p>
<p>52</p>
<p>VECTOR Vol.10 No.3</p>
<p>short cuts that I would not normally want to take if | was developing a real system. Peter told us that he took 3 hours to reach the final solution, which sounds aboutright if one wascreating a real system. He distributed his solutions to all the exercises on a disk at the endof the day, so when I got homeI wasable to analyse the way in which hehadsolvedthe problem.I liked the way he used a generalised function to display the various alternatives that could be specified, which I attempted but was unable to complete in the time.</p>
<p>Wethen wenton to explore graphics, with Peter giving us a quick demonstration of the facilities available before we attempted the nextexercise, Scribble. A blank form had to be displayed on screen, and you had to makeit draw a line when you click and drag the left mouse button. On releasing the left button and clicking again the previous drawing hadto becleared. As this only involved an empty form with just two small functions to write, most people were able to complete the task quite quickly. The functions Peter distributed as the solution were:</p>
<p>VY MSGestart MSG C1] a Callback for EVENT 1 (MOUSE_DOWN) in 3 £2] +(14MSG(5])/0 [3] 'S.L'QWC'POLY'(1 2pMSGC3 4])} v</p>
<p>¥Y MSG+draw MSG;P c1] a Callback for EVENT 3 (MOUSE_MOVE) in § C23 +(1#MSG5])/0 C3] P&lt;'S.E'QWG'! POINTS| C4] P+P,C1]MSGL3 43 £5] 'S.L'OWS' POINTS'P v</p>
<p>Line [2] of each function is the key to the solution, which fortunately Peter emphasised before we started. This tests to see if the left mouse button is down. If it is, we go ahead and dothe processing, but otherwise the mass of mouse- move messages are ignored. Seeing that these functions are called for every mouse-move message, which can occur hundreds of times per second, the process works remarkably quickly.</p>
<p>Finally we explored the WDESIGN workspace and developed somesimple screen layouts using thefacilities available.</p>
<p>I felt that I had learnt quite a lot from my morning s endeavours. When I got homeI enjoyed exploring Peter s solutions onthe disk that was handedout, and was impressed with the amountof material available.</p>
<p>53</p>
<p>VECTOR _ Vol.10 No.3</p>
<p>Adrian Smith Adrian started off by giving us an overview of three things:</p>
<p>+ what we wouldgetoutofthe session</p>
<p>+ what Adriangets outofit</p>
<p>+ the design philosophy behind his GUI workspace</p>
<p>What wegot wasan excellent GUI design workspace with open code so that we can all do the usual APLthings of tweaking the bits we don tlike to turn it into a tool that really sings. What Adrian got out of it was feedback on his design, which will hopefully continue. This includes reports of any bugs in his workspace, which is a beta-test version, and comments or suggestions for improvements in the facilities available. Adrian also wants everyoneto have fun developing GUI systems, which should increase the use of APL generally.</p>
<p>The design philosophy behind the GUI workspace is sort of object oriented. Adrian did not want to use the word object becauseof all the connotations with Object Oriented Programming. The way in which he has got messages passing to windows when an APL variable changes does makeit feel a bit object-ish, thoughit obviously is not OOP.</p>
<p>I included in my review of Dyalog APL 6.3 (see Vector 10.1, July 1993, page 66) the initial version of the workspace, and was impressed then. This version is a great improvement. Adrian gave us the briefest of introductions on how the thing worked, and everyone seemedto beable to get going really fast creating GUI objects and getting quite complex things to work. 1 would have preferred a little more introduction to the features available, but that was probably becauseI had seen a prototype and immediately tried to do sometricky things where I had had somedifficulty.</p>
<p>VECTOR Vol.10 No.3</p>
<p>The design tool looks the same as I reported in Vector 10.1. In the following example I have defined a Scroll Bar and a Numeric Edit Box, both linkedto the variable Size, and bothto report any changein the variable to the other control:</p>
<p>[+ Duta variable (updated) ¥</p>
<p>= Update on achange to. 9°?</p>
<p>The pop up Data box is new.In it you can define the APL variables that are to be referenced (typically a format specification), shown, and watched for changes. When any monitored variable changes, the object is automatically updated. As you can see from the example, I specified the variable Size for the Edit box, which I hadalsospecified for the scroll bar. Havingset the variable Size in the workspaceI ran the whole thing by clicking on the Run button in the top right handcorner. This gave methefollowing: Tt]</p>
<p>Moving the slider updates the numberin the box, and changing the numberin the box updatestheslider. All done in a couple of minutes work.</p>
<p>55</p>
<p>VECTOR Vol.10 No.3</p>
<p>The array in which the specifications are held is now much simpler. The above example is a 3 by 10 matrix, containing the form, slider and edit box specifications. A simple function (s/ow)displays the matrix:</p>
<p>Ty Caption Pos Size Sty Ref Data Listen E.A.T. Id</p>
<p>FM (untitled) 228 264 164 416 PA 20 80 24 257 size size WA 60 BO 24 140 size size</p>
<p>Everyone I spoke to felt that Adrian s workspace made everything so much simpler. I shall certainly use it for any Dyalog APL development work. I particularly liked the way that Adrianhasleft the code openso that I can make minor changes. So far, the only thing that I have found that I would want to change is the function for displaying a pop-uplist box, where 1 would want to calculate the size depending on the numberofitemsto be selected.</p>
<p>Conclusion I felt that the Workshop wasa great success. Everyone had a PC to themselves so there was no problem in getting down and solving one s own problems. There were not too many people in each class so the tutors could give individual attention where necessary. The course material clearly set out the way in which GUI systems could and should be built. And, as an extra bonus, we all carried home disks filled with useful code. I would certainly attend similar tutorials which maybe held in the future.</p>
<p>Whatthe Presenter got outof it! by Adrian Smith This was the best usability lab 1 could have wished for. I was generally delighted with the ergonomics of the designer, but picked up an obvious need for caption , position , style to be grouped into a single Appearance dialogue .. done! Less obvious was the need for local variables at the form level. This makes it very mucheasier to leave thingsstrictly alone until OK is hit (mostly done). Some objects need to be more forgiving (fewer LENGTH ERRORS when your boolean doesn t match youroptions!), and also morecatholic in their tastes (nested vectors or simple rank-2 arraysin list boxes).</p>
<p>Anyone whodidn t get a copy, and could usefully play with this stuff, please send a disk to APL9385 with a return address and appropriate postage. Just don t build a real system yet the design is stabilising, but I don t wanta user- basejust yet.</p>
<p>56</p>
</details>
