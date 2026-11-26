---
vid: '10001490'
title: The Ideal Screen Editor
authors:
- Anthony Camacho
volume: '7'
issue: '4'
page: '78'
status: not online
issue_tab: n4-april-1991
---

Anthony Camacho
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 78](../7/4/VOL.7-NO.4-APRIL-1991.pdf#page=80)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.7 No.4</p>
<p>The Ideal Screen Editor a talk given to the BAA on 23 November 1990 by Anthony Camacho</p>
<p>Five overhead projector foils were shown asan introduction to the screens which madedefining a screen easy. Foil 1 was an introduction.</p>
<p>WHY A NEW SCREEN SYSTEM? [foil 2]</p>
<p>Mostapplications need one Vendors systems incompatible Takes too long to write Wish to make APL widely used Wishto restore APL s advantages Wishfor screen to respondfast Cannot spare much workspace</p>
<p>The motivesfor writing a screen system were to provide something independent of vendors that required aslittle beyond ISO APLas possible, written in APL so that anyone could amendit and straightforward enough to be used without a great deal of effort. Once, APL gave its users the best interface for interaction between the keyboard operator and the system; it would be nice to restore the advantage. Also a system would haveto be fast enough anduse little enough workspace so that it would be practical even in I-APL.</p>
<p>ORIGINS AND THE IDEA {foil 3]</p>
<p>Paul Chapman s port from Viz::APL Use only window get/ put cursor All screen input/output is character Pre-calculate variables for speed Hold itemsonfile to reduce space</p>
<p>To use: R«SNAME AFS CHARVEC SNAME «&lt;+screen name on file BDRDD CHARVEC ++chars for fields R «&gt; chars from fields</p>
<p>78</p>
<p>VECTOR Vol.7 No.4</p>
<p>The idea wasthat a perfectly adequate screen system can be written using only four special functions, which are available in most versions of APL on micro- computers. The originator of the idea was Paul Chapman, who had to port a system from Viz::APL to APL*PLUS/PC in a hurry. He never completed or documented what he wrote, but it was in daily use for several years and the response was adequate even on an 8088 at 4.77MHz. Most data was held in vectors and as much aspossible was pre-calculated. I learned about it by having to enhance the system that used it and the Ideal Screen Editor (ISE) was born when I considered how Paul's screen system could be adapted to meet the objectives mentioned.</p>
<p>The main limitation of ISE is that all input and output of the screen display functions is in characters. The file referred to on the foil is tied to ASTIE. A secondary limitation is that the main function 4FS has to call subsidiary functions for any field which has more than oneline.</p>
<p>FEATURES OF THE DESIGN [foil 4]</p>
<p>Three functions for simplicity</p>
<p>AFS Full screen (many fields} AMA Matrix amend (one fld only) ABB Bounce bar (one field only)</p>
<p>AMA &amp; ABB handle multi-linefields</p>
<p>For each AF Sthefile has 9 components</p>
<p>The subsidiary functions are AMA which displays any matrix for amendmentin a window (which may be a pop-up window) and ABB which handles the bounce- bar method of choosing an option; the choices may be scrolled past the window and selected with the highlight bar or,if the lines are numbered, by entering the option number.</p>
<p>The pre-calculated variables are held on a component file (or a pseudo- componentfile) in nine components, containing:</p>
<p>The design so thatit can be re-entered with variations The window for the screen</p>
<p>ae Yh Theinitial contents of the screen Theindices of the characters that may be varied The attributesof theinitial contents</p>
<p>79</p>
<p>VECTOR Vol.7 No.4</p>
<p>table giving details of each field</p>
<p>eo ONS Asmail matrix of executable lines (mainly standard validations) A vectorof the codes from keys that cause special actions A corresponding character vector oflabels (the action for the key)</p>
<p>Thefield table contains the window for eachfield, the navigation from field to field, three attributes ofthe field:</p>
<p>on display, whenhighlighted with the cursorin it and whenvalidation hasfailed and it must be re-entered.</p>
<p>It also contains a pointer to the validation required, a pointer to any special action required on moving the cursor into the field, and, for multi-line fields, details of whatis required.</p>
<p>To get the system into use, onceit was established that it was feasible, the author hadto do three things:</p>
<p>1. Produce an example to show howto use the screen functions 2, Demonstrate that the system was robust with a demandingtest 3. Provide the potential users with an easy way of entering screens</p>
<p>These three needs could all be metif the screen system could prove its worth by using it to write the screen entry system. The screen entry system is the most important part of the design.If it is really easy to use then people may useit evenif other featuresare less than perfect. On the other handif the entry method is difficult to use then it probably will get ignored.</p>
<p>The author s purpose this afternoon therefore was to describe the entry procedures and to ask the BAA members present whether they could think of a simpler way of doing any of the tasks that are an essential part of entering a screen design.</p>
<p>WHATIS THE EASIEST WAY? [foil 5]</p>
<p>Enter by amending a similar screen Defaults (sensible) for everything Never a need to count(characters) Specify position by cursor Choosefrom lists where possible Showeffects as soon as possible Allow easycorrections</p>
<p>80</p>
<p>VECTOR Vol.7 No.4</p>
<p>The simplest way to enter a new screen design would be to amend one which wassimilar. It is easier to alter a heading and an instruction or two thanto key in a complete design. The simplest way to enter colours, validations, field sequences and the action to be taken on each kind of key depression is to have them all default to standard settings. The standards to which everything defaults should themselves be settable so that all the screens in an application can be given a distinctive house style. At every stage the delay between specifying something and showing the result should be as short as possible and the sequenceto respecify it if needed should also be short and quick.</p>
<p>A series of draft screen designs were then shownand the proposed method of use for each described.In every case the aim wasto find the entry method which would give the least trouble to the keyboard operator entering the design.</p>
<p>Fields are specified by putting special characters at the endsofthefield (outside it so that default contents could be entered within the field). Ruled lines are specified by their ends, boxes by their corners and multi-line fields by marking the first and lastlines after the fashion ofsingle fields but with special characters. The screen maybe framed with single or doublelines by entering + or # in the top left corner.</p>
<p>Pop-up fields are specified by moving the cursor to the top left, marking the spot, and then to bottom right and fixing that. The same procedure is used to specify colour for the areas of the screen outside the fields. Fields themselves have three colours; all may beleft to default values, the defaults may be changed and anyfield may have special colours set by moving the cursor into thefield and specifying the colour.</p>
<p>Validations are specified by entering a character corresponding to one of the six standard validations orto a special validation in thefield. If a special characteris used then the validation required is entered as a line of APL (best to give a function name) which returns the validity as a boolean.</p>
<p>The fields may be navigated in two sequences andthere is a third sequence for inserting the variable data into fields. The sequences have defaults. The default for any sequence may be amended by entering a decimal numberin a field to give its new position in the sequence(like inserting a line underthe del editor). On moving the cursor from the field the new sequence is calculated and displayed.</p>
<p>The allocation of actions to keys is done by displaying two bounce-bars. In one the key and a twoletter codefor its current action is shown;in the other a list of</p>
<p>81</p>
<p>VECTOR Vol.7 No.4</p>
<p>possible actions is shown.Toallocate a different action to a key,first highlightit, then highlight or enter the new action, then press an F-key.</p>
<p>During the discussion afterwards only one improvement to ease of entry was proposed, by Peter Branson. Peter suggested that the method usedfor specifying a pop-upfield should be usedto specify multi-line fields. The author was glad to accept it and hoped that, as nobody could think of any further improvements, the design was now as goodasit could be.</p>
<p>APL.68000 LevelIl is a full</p>
<p>APL.68000 specification APL2 for the 68000 family of processors. It incorporates all the user-</p>
<p>Level Il oriented enhancements of APL.68000 Level | as well as MicroAPL s acclaimed windowing interface. APL.68000 Level Il is available for computers such as the NCR Tower, Sun, HP, Bull, Nixdorf, Wicat, Mac, Amiga and ST. Features include: Nested and mixedarrays User-defined operators Selective specification Upwards compatibility with APL.68000 Level | For more details contact: MicroAPL Ltd., South Bank Technopark, 90 London Road, London, SE1 6LN. 071-922-8866</p>
<p>82</p>
</details>
