---
vid: '10008500'
title: 'Hacker’s Corner: Minding Your .INIs'
authors:
- Adrian Smith
volume: '10'
issue: '3'
page: '96'
status: not online
issue_tab: n3-january-1994
---

Adrian Smith
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 96](../10/3/VOL.10-NO.3-JANUARY-1994.pdf#page=98)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.10 No.3</p>
<p>Hackers Corner: Minding your .INIs by Adrian Smith</p>
<p>Introduction Some Windowsapplications are better behaved than others. Winwordis one of the worst it writes about 100 lines of junk all over your WIN.INI, which is a wonderful way of slowing the Windowsstartup. A well-behaved application (such as Dyalog APL/W) justslots in a minimalentry: (DyalogAPL] dyalog=c: \wdyalog inifile=c:\wdyalog\apl.ini sound=quack</p>
<p>to tell it where to go andlook for a private .INI file, which can be hidden awayin its own directory. (As for how that spurious sound= entry gotthere ... well, you will have to read onto find out.)</p>
<p>Manyapplications compromise, and maintain a private .INI, butstick it in the Windowsdirectory where they can be sure to find it. MS-mail is typical, as is CLOCK,which leaves behind a CLOCK.INIlooking somethinglike this:</p>
<p>[Clock] Maximized=0 Options=1,1,0,0,0,0 Position=119, 708,155,744</p>
<p>Which leaves you with a couple of choices to make, and little gentle research to do to find out how to maintain thesefiles safely from your APL application,</p>
<p>Choices Here are my viewson the three methodsoutlined above:</p>
<p>1. Sticking everything into a [section] in WIN.INI. This is for lazy prograrnmers only.It is definitely the soft option, and Windowsgives you a simple utility which doesjustthat. It has the unfortunate side-effect of adding to an already overloadedfile, and also makes your application a pain to un-install. I suggest you don tdo it. 2. Making an APPL.INIfile in the Windowsdirectory. This is more reasonable, as this is where most simple folk start looking for .INIfiles. Again, there are simpleutilities available to help you. The only disadvantageis that your</p>
<p>96</p>
<p>VECTOR Vol.10 No.3</p>
<p>application is now scattered over a couple of directories, and someone may re- install Windows andclean you out accidentally.</p>
<p>3. Making an APPL.INI file in your own homedirectory, andinserting a pointer to itin WIN.INILYoucould use the existing [DyalogAPL]section, or you could start an [APLApps] section on the assumption that you won't be the only APL program onthe machine.| think I like this option best, andit is whatthe better-behaved Windows programsare nowtending to do.</p>
<p>Assuming that you want to do something sensible to record your startup parameters (no, don t put them in a componentfile!) where they can be fixed up with any old text editor read on.</p>
<p>Option-1: Infesting WIN.INI Before you go updating things, make sure you can read them! This is thefirst function you need to know about:</p>
<p>Yo reGPS arg;gps;section;entry ca] a Access to profile string from WIN, INI C23 a e.g. GPS 'dyalogAPL' inifile! C3] gps'OWA'T kernel, Pi6|GetProfileString &lt;0? &lt;0? &lt;oF &gt;0or I! [4] a Usage &lt;gps 'DyalogAPL' inifile' default value' 40 39&gt; Cs] section entry+arg t6J re2sgps section entry'(undefined)! 256 255 v</p>
<p>This takes a vector of two elements,the first of whichis the section (but don t put the [ ] aroundit); the second is the parameter you want to know about.It returns a simple vector, with the parameter value, or (undefined) if it failed to find anything. For example: GPS 'Colors' 'ButtonFace' 192 492 192</p>
<p>Anything you can get, youcanset: V {r}«WPS argswps;section;entry;value [42 a Weite info to profile string in WIN.INI [23 A xxanee USE WITH CAREY! *xx0wn [3] na e.g. WPS 'DyalogAPL' sound' quack! C4] a [5] wps'ONA'I Kernel.Pi6|WriteProfileString &lt;0? &lt;oP &lt;of' [él e Usage &lt;wps 'section' entry! new value'&gt; [7] section entry value+arg [8] r+wps section entry value v</p>
<p>97</p>
<p>VECTOR Vol.10 No.3</p>
<p>. which is how that quack got in there! If you use a new section name, Windowscreates the section for you, if you use a new parameter name,it is added automatically to the section.It really is amazingly helpful, which probably explains why the average WIN.INIis so full of junk.</p>
<p>Option-2: Infesting the Windows Directory For simple applications, this is probably a good compromise. This time, to create your.INIfile, you use a related Windowscall:</p>
<p>VY ir}+fi WPPS argiwpps;section;entry;vaive taj a Write info to private profile string such as appl.ini C2) a e.g. 'elock' GPPS 'clock' 'position '200,200,100,100" C3] na Assumes '.ini' unless file extension given. Cay A [53 "vpps'OWA'Y kernel, P16|WritePrivateProfileString &lt;Of &lt;OT &lt;0T &lt;oT! [6] a Usage &lt;gpps 'section entry! 'new value! 'inifile' &gt; [7] fitfi,(~'.'efi}/'.dni' © section entry value+arg [8] revpps section entry value fi v</p>
<p>This continues the helpful approach by creating the entire file for youif it didn t already exist. For example: fluff WPPS Options' colour! '255 255 255! fluff WPPS Options' 'size' 'small' fluff! WPPS 'Toolbar' setting off'</p>
<p>results in the creationof a fluff.ini file which looks like: [options] colour=255 255 255 size=small</p>
<p>[Toolbar] settingzoft</p>
<p>... Which canlater be recovered just as straightforwardly with: Vo refi GPPS arg:gpps;section;entry ca] a Access to private profile string such as clock.ini C2] a e.g. clock GPPS 'clock' options' £3] ne Assumes '.ini, unless file extension given. C4) oa £5] gpps'DNA'I kernel.Pit|GetPrivateProfileString &lt;0? &lt;OT &lt;oF &gt;of I &lt;or' £6] a Usage &lt;gpps section 'entry' default value' 40 39 'inifile'&gt; c7] fieti,(~'.'efi)/'.ini' o section entry+arg Es) r+22gpps section entry'(undefined)' 50 49 fi v</p>
<p>98</p>
<p>VECTOR Vol.10 No.3</p>
<p>You might even find these handyforfixing the settings on some of the standard applications, for exampleto start a scientific calculator from an APL button.</p>
<p>Option-3: Hiding the .INI in your own Directory This is really just a combinationof thefirst two. J think the best compromiseis to add an entry to an [APLApps] section:</p>
<p>WPS APLApps' Fluff! c:\apps\fluff\fluff.ini'</p>
<p>.. and to maintain FLUFF.INI as above with WPPS. This does mean one extra lookup when youstart, but you can be sure the CAPLApps] sectionis there, and you have generatedtheleast possible clutter in the Windowsdirectory.</p>
<p>Summary Therereally is no excuse for not following the way of Microsoft, and putting all your startup options in an editable .INI file. You don t even need to write the editor!</p>
<p>Postscript VY {r}+(flg} PLAY sad;sps (13 a Play a .WAV file (asynchronous by default) 23 a flg++/O(synch) 1(asynch) 2(no defit) 8(repeat) 16(nostop) £3} sndesnd,(~'.'esnd)/'.WAV' £43 sps'OWA'I mmsystem.P1i6|sndPlaySound &lt;of U' [s] 2(0=UNC' flg'}/'figes! (67 resps snd flg v</p>
<p>PLAY 'c;\dash\dog' a for the friendly Dashboard dog or PLAY 'c:\windows\sndbits\music\didjeri' a (Sic)</p>
<p>.. for that true Australian flavour! Does anyone know of a good duck?</p>
</details>
