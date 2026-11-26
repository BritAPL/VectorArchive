---
vid: '10001840'
title: No SX please – we’re APLers
authors:
- Ray Cannon
volume: '11'
issue: '2'
page: '143'
status: not online
issue_tab: n2-october-1994
---

Ray Cannon
{ .byline }

The text of this article is not yet online.

[Read it in the PDF of the issue, page 143](../11/2/VOL.11-NO.2-OCTOBER-1994.pdf#page=145)

<details class="ocr">
<summary>Unedited OCR text, machine-read from the scan: it contains errors, and its APL is wrong. Shown for searching; read the PDF.</summary>
<p>VECTOR Vol.11 No.2</p>
<p>No SX please, we re APLers What your mother never told you about the namingof the Intel chip set by Ray Cannon</p>
<p>During a recent meeting a question arose about which Intel 486 chip was best for running APL, a 486DX or 486SX.In reply,I will attempt to answer by explaining the Intel chip-set naming scheme.</p>
<p>In the beginning, Intel created the 4040, but that was before my time.</p>
<p>Once upon a time, there were three famous 8-bit chips, the Zilog Z80, the Motorola 6502, and the Intel 8080. They all had a 16-bit address bus (which could access 64K bytes of memory), an 8-bit data bus (each read or write to memory consisted of 8 bits of data), and a mixture of 8 and 16-bit registers. (Both the Zilog andIntel had register pairs that could be used for some 16-bit operations.)</p>
<p>In the business market, the Z80 was king and ruled through the CPM operating system.It also saw thestart of the home market with the Sinclair ZX80 and ZX81. However, the 6502 soon became popular in home computing (Apple, BBC, Commodore). The 8080 gained supportin industrial control applications.</p>
<p>The Genesis of the IBM PC Range And someone said Let there be 16-bit computing and there was the Intel 8086 and IBM thoughtit was good, but the Intel 8088 was cheaper (and 20% slower), so they putthat in their new PCinstead.</p>
<p>Both the 8086 and the 8088 havethe sameinstruction set, so they look identical to the programmer, but the 8086 has a 16-bit data bus (and a 6 byte queue) while the 8088 has a 8-bit data bus (and only a 4 byte queue), so the 8088 is slower. They could both run at a clock rate of 5MHz. (A newer memberof the family, the 8086-2, could run at 8MHz.)</p>
<p>TBM used the 8088inall their original PCs, XTs and portable PC (the PCjr). The first Compaq Deskpro used the 8086.</p>
<p>Both the 8086 and 8088 had a 20-bit address bus which could access up to 1Mbof memory.(Internally this 20-bit address is composed of 2 overlapping parts, a 16- bit segment address and a 16-bit offset. This leads to fast processing of data within a 64K address range, but real problems when trying to deal with data larger than 64K.)</p>
<p>143</p>
<p>VECTOR Vol.11 No.2</p>
<p>Intel next produced the 80186 and 80188 (sometimes the leading 80 was dropped from the name). These were bigger, requiring less external circuitry, better as they had a few more instructions and most importantly faster as they ran at 8MHz. IBM did not bother with them, but Tandy soldlots of compatible PCs with them in.</p>
<p>Intel s next processor was the 80286 (which by now almost everybodycalled the 286). This was designed to address up to 16Mb of memory (24-bit address bus) and could be fooled into believing it had up to one gigabyte via virtual memory . It could also multi-task and ran at 10Mhz.Like the 186 it has a 16-bit data bus anda 6 byte instruction queue.</p>
<p>IBM fell in love with the 286 and usedit in their PC-ATs.</p>
<p>STSC, knowing a good thing whentheyseeit, developed an APL for the PC and called it APL*PLUS/PC .</p>
<p>Moving to 32-bit Computing In the meantime Dyadic were developing Dyalog APL under UNIX (on a Zilog Z8000 I think). When asked if they could port their APL to the IBM, they said No,it needs a 32-bit machine. Now Intel heard aboutthis, and not wanting to lose the IBM PC contract to Zilog embarked on producing the 32-bit chip and called it the 386 [Ed: A likely story!].</p>
<p>The 386 is upwards compatible from the 8088 but has 32-bit registers and a 32-bit data path.Its address bus can access up to 4Gbytes of memory. It was designed to handle both multi-tasking and multi-user . Each user can have up to 64 terabytes of virtual-address space.</p>
<p>Now, producing circuit boards with 32-bit wide data pathsis a lot harder than producing ones with 16-bit paths. So Intel came up with the idea of having a 32- bit width internally and a 16-bit external path (justlike the old 8088 compared to the 8086). So, if they had had any sense, not to confuse anyone, they would have used the old naming standard and the 388 would have been born, except they didn t. They actually called it the 386SX, and just in case anyone was confused, they started calling the 386 (with the full 32-bit data path) the 386DX. The advantageof the 386SX wasthat you could take a basic 16-bit AT type board and plug in a 386SX chipto get (a substandardvariety of) 32-bit computing at 16- bit prices.</p>
<p>144</p>
<p>VECTOR Vol.11 No.2</p>
<p>Adding Floating-point Maths Nowlet's float back to a point in time shortly after the 8086 first appeared. The 8086 only supports integer maths in its instruction set. It did not support floating-point maths. So, Intel produced a 8087 Maths Co-processor which could be plugged in alongside the 8086, 8088, 80186 or 80188, to do the hard work.Intel also call the co-processor an NPX Numeric Processor Extension, hencethe Intel 8087 NPX .</p>
<p>To allow for the addition of a co-processor chip, most PC motherboards came equipped with an empty co-processor socket.</p>
<p>With an NPX, a machineinstruction to multiply two real numbers could be processed. Without an NPX, this hardwareinstruction has to be emulated in software. The NPX hardware can perform this (and other maths) operations many times faster than the software. Also the NPX can improve the speed of multiple-precision software algorithmsfor integer operations by 10 to 100 times (Intel 387DX User s Manual). To keep the 8086/8087 chips in step with each other, each NPX instruction had to be preceded with a WAIT instruction. Intel s assembler ASM86 did this automatically.</p>
<p>With the adventof the 286, Intel produced a new co-processor, the 80287 NPX. The 80286/80287 operating in Real-Address mode will execute 8086/8087 programs without major modifications. However differences between the way 8087 and 80287 handle numeric exceptions may require different exception handling routines. Also, the 80287 does not require WAIT instructions. So reassembling using Intel s ASM286 assembler will produce faster and more compact code.</p>
<p>For the 386DX they produced the 387 DX NPX which Intel now tend to call the 387 DX Math Co-processor and for the 386SX the 387 SX Math Co-processor {and of course a new assembler ASM386). Although compatible, the 387 differs from the 8087/80287 in some minor ways mainly to bringit into line with the TEEE Standard 754 for Binary Floating-Point Arithmetic.</p>
<p>Since the SX version was much cheaper to produce than the DX, IBM compatible 386SX PCssold in their millions.</p>
<p>Now APL doesa lot of high precision maths (both integer and real), so a 32-bit 386 PC with NDX is just what Dyadic neededfor its Dyalog APL, and STSC needed forits new second-generation APL APL*PLUS II .</p>
<p>145</p>
<p>VECTOR Vol.11 No.2</p>
<p>486 the Next Stage The next stage in Intel s plan for global domination was to merge the 386DX and 387DX into a single chip. This they called the 486DX or sometimes i486 , but NEVER(byIntel) the 80486, (Intel's internal code name for the range is P4.) These weresold to runat20 (rare), 25, 33 and 50Mhz.</p>
<p>Just a quick note on processor speed.All these 486s are producedto the same specification, and are subsequently speedrated viatesting, so a 486DX-25 is a 486DX whichfailed the 33MHz test but passed the 25MHz test.</p>
<p>When someone discovered you could run some of these chips at twice the external clock speed, the 486 DX2 was born. DX2s are available as 40, 50 and 66 MHzversions but not 100Mhz. When sold as the OVERDRIVE version they still use the DX (not DX2) name. The Overdrive socket is 486 s equivalent to the co-processor socket found onearlier PCs. (Note, Overdrive sockets now come in at least two colour-coded flavours, blue and white, but that s jumping the gun a bit.)</p>
<p>Now,legendhasit that one batch of 486DXsfailed to pass Intel s quality control, because the NDX part was not working. Rather than just binning them the marketing men at Intel came up with the idea ofselling them under the nameof 486 SX, under the impression that SX would sell anything.</p>
<p>An example might help here. My computer has an 80486 mainboard, which can (according to the User's Guide ) run at 25, 33 40 and 50 MHz. It can be configured with the following 169 pin PGA CPU micro-processors:</p>
<p>80486SX-25, 804875X-25, 80486DX2-50 CPU running at 25MHz 80486DX-33, 80486DX2-66 CPU running at 33MHz 80486DX-40 CPU running at 40 MHz 80486DX-50 CPU running at 50 MHz</p>
<p>Tt can t run the rare 486DX-20 or 486DX2-40.</p>
<p>What, you mayask, is an 80487SX-25?It took me sometimeto find out. It is a full i486DX-25 designed to be used in the OVERDRIVE socket of a motherboard fitted with an i486SX-25.It s a co-processor with an SX name!</p>
<p>Onward to the Pentium and P6 After the P4 comes P5 the Pentium . This Intel chip has 2 on-board 486s, with 64-bit addressing. Because Intel could not copyright the number 586, they coined</p>
<p>146</p>
<p>VECTOR Vol.11 No.2</p>
<p>the name Pentium. As you might expect, a Pentium runs abouttwiceas fast as a 486 with the sameclock speed.</p>
<p>Next comes the i486DX4. Knowing how well Intel have namedtheir chips, you won't be surprised to discover this is a clock-tripled 486, with a larger (16k) processor cache. Handsup all of you whothought Intel could count.</p>
<p>Andfor the future, the P6 which, as they say,will be history.</p>
<p>Oh,I forgot. The moralofthis story is: if you want your APLto run fast on a PC with Intel Inside , get a Pentium, and if you can t afford a Pentium get an i486DX4.Or of course you could waitfor Intel s next chip, the P6, But please, no SX, we're APLers.</p>
<p>Other Great Titles in this Series: Stop the GreenhouseEffect Using a low power consumption i486SL</p>
<p>Clone Wars The ADMinvasion</p>
<p>Growing up with Cyrix Replace your Intel 386S5X or 386DX with Cyrix Cx486SRX2 Blue Lightning IBMstrikes back with the clock-tripled 486SX3-99</p>
<p>Anything Blue can do, i can do better Intel s latest chips, the clock-tripled i486DX4.</p>
<p>Appendix: Power Saving ManagementSystem To reduce the total power used in portable PCs, all Intel and Cyrix 486 chips now incorporate System Management Mode (SMM). This progressively reduces the power used by a processor by powering downparts of the chip which are not being used at the time.</p>
<p>When SMM wasfirst introduced by Intel, the chips using it were designated as 486SL . However, this feature has been incorporatedin all Intel 486 chips since the start of 1994, and the SL named chipsare no longer made.</p>
<p>All Cyrix 486 chips have SMM asstandard.</p>
<p>147</p>
<p>VECTOR Vol.11 No.2</p>
<p>486SLC and 486DLC The IBM chip 486SLCis logically the same as an i486SX,(it has a cache controller but no co-processor) but has 386DX pin out. This means thatit can be put onto boards designed for 386DX chips, providing PC manufacturers with a cheap 386 to 486 upgrade path.</p>
<p>The Cyrix 486DLCchipis like the IBM 486SLC but DOES have a co-processor.I have been told that Cyrix don t actually make chips, they just design them and TBM actually produces them for Cyrix.</p>
<p>Intel Math Co-processor Identification Notes Every Intel Math Co-processor is marked to indicate its product number and maximum operating speed, Currently, Intel marks the top of each chip with the math co-processor product number, a dash, then a number representing the maximum speed of the chip. Usually this number represents the speed in megahertz (MHz), but Intel s previous marking scheme wasslightly different. The numbers in parentheses in the following chart are from this previous marking scheme.</p>
<p>Product Rated Intel Number Speed Model # 8087 (-3) 4.77MHz BOX8087 8087-8 (-2) 8MHz BOX8087-2 8087-10 (-1) 10MHz BOX8087-1 80287(-6 or -3) 6MHz BOX287 80287-8 8MHz BOX287-8 80287-10 10MHz BOX287-10 80C287A 12MHz, DBOXC287A-12 387DX-16 16MHz BOX387DX-16 387DX-20 20MHz, BOX387DX-20 387DX-25 25MHz BOX387DX-25 387DX-33 33MHz, BOX387DX-33 3875X 16MHz BOX387SX-16</p>
<p>148</p>
<p>VECTOR Vol.11 No.2</p>
<p>The Intel 486/Pentium &amp; Cyrix ranges: (This will be incomplete and out of date by the time you read this article)</p>
<p>INTEL Chips Processor MHz Volts Watts Co-Processor Intel code i486 SX 16 5/3.3 1.7 i487SX-16 overdrive P23-16 i486 SX 20 5/3.3 2.3. i487SX-20 overdrive P23-20 i486 SX 25 5/3.3 2.8 i487SX-25 overdrive P23-25 i486 SX 33 5/3.3 3.0 i487SX-33 overdrive 23-33 i486 DX 2 5/3.3 28 Internal P4-25 i486 DX 33 5/3.3 3.5 Internal P4-33 i486 DX 50 5 4.0 Internal P4-50 1486 SX2 50 5/3.3 3.5 i487SX2-50 overdrive P23-50 i486 DX2 40 33 14 Internal P2440 i486 Dx2 50 5/3.3 3.9 Internal 24-50 i486 DX2 66 5 4.9 Internal P24-66 i486 DX4 75 3.3 3.0 Internal P24C-75 1486 DX4 100 33 4.0 Internal P24C-100 Pentium 60 5 12.0 Internal P5-60 Pentium 66 5 13.0 Internal P5-66 Pentium 90 3.3 4.0 Internal P54C-90 Pentium 100 3.3 464.0 Internal P54C-100 222272? 133-200 2 ? Internal P6</p>
<p>CYRIXchips Processor MHz Volts Watts Co-Processor Cx486DX-33 50 5 3.05 Internal Cx486DX-V33 33 3 138 Internal Cx486DX-40 40 5 3.42 Internal Cx486DX-50 50 5 3.82 Internal Cx486SLC-33 33 5 247 None? Cx486SLC2-50 50 5 3.40 None? Cx486DX2-50 50 5 3.82 Internal Cx486DX2-V50 50 3 1.63 Internal Cx486DX2-66 66 5 4.30 Internal Cx486DX2-V66 66 3 1.85 Internal Cx486DX2-V80 80 3 2.06 Internal</p>
<p>149</p>
</details>
