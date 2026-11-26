---
vid: '10013040'
title: No SX please – we’re APLers
authors:
- Ray Cannon
byline: by Ray Cannon, . (ray_cannon@compuserve.com)
volume: '11'
issue: '2'
page: '143'
source: trad/v112/cannon112_143.htm
converter: vecrescue 0.1.0
issue_tab: n2-october-1994
tags:
- performance
- implementations
---

by Ray Cannon, <span class="affil">.</span> <span class="email">(<a href="mailto:ray_cannon@compuserve.com">ray\_cannon@compuserve.com</a>)</span>
{ .byline }

During a recent meeting a question arose about which Intel 486 chip was best for running APL, a 486DX or 486SX. In reply, I will attempt to answer by explaining the Intel chip-set naming scheme.

In the beginning, Intel created the 4040, but that was before my time.

Once upon a time, there were three famous 8-bit chips, the Zilog Z80, the Motorola 6502, and the Intel 8080. They all had a 16-bit address bus (which could access 64K bytes of memory), an 8-bit data bus (each read or write to memory consisted of 8 bits of data), and a mixture of 8 and 16-bit registers. (Both the Zilog and Intel had register pairs that could be used for some 16-bit operations.)

In the business market, the Z80 was king and ruled through the CPM operating system. It also saw the start of the home market with the Sinclair ZX80 and ZX81. However, the 6502 soon became popular in home computing (Apple, BBC, Commodore). The 8080 gained support in industrial control applications.

### The Genesis of the IBM PC Range

And someone said “Let there be 16-bit computing” and there was the Intel 8086 and IBM thought it was good, but the Intel 8088 was cheaper (and 20% slower), so they put that in their new PC instead.

Both the 8086 and the 8088 have the same instruction set, so they look identical to the programmer, but the 8086 has a 16-bit data bus (and a 6 byte queue) while the 8088 has a 8-bit data bus (and only a 4 byte queue), so the 8088 is slower. They could both run at a clock rate of 5MHz. (A newer member of the family, the 8086-2, could run at 8MHz.)

IBM used the 8088 in all their original PCs, XTs and portable PC (the PCjr). The first Compaq Deskpro used the 8086.

Both the 8086 and 8088 had a 20-bit address bus which could access up to 1Mb of memory. (Internally this 20-bit address is composed of 2 overlapping parts, a 16-bit segment address and a 16-bit offset. This leads to fast processing of data within a 64K address range, but real problems when trying to deal with data larger than 64K.)

Intel next produced the 80186 and 80188 (sometimes the leading “80” was dropped from the name). These were bigger, requiring less external circuitry, better as they had a few more instructions and most importantly faster as they ran at 8MHz. IBM did not bother with them, but Tandy sold lots of compatible PCs with them in.

Intel’s next processor was the 80286 (which by now almost everybody called the 286). This was designed to address up to 16Mb of memory (24-bit address bus) and could be fooled into believing it had up to one gigabyte via “virtual memory”. It could also multi-task and ran at 10Mhz. Like the 186 it has a 16-bit data bus and a 6 byte instruction queue.

IBM fell in love with the 286 and used it in their PC-ATs.

STSC, knowing a good thing when they see it, developed an APL for the PC and called it “APL\*PLUS/PC”.

### Moving to 32-bit Computing

In the meantime Dyadic were developing Dyalog APL under UNIX (on a Zilog Z8000 I think). When asked if they could port their APL to the IBM, they said “No, it needs a 32-bit machine.” Now Intel heard about this, and not wanting to lose the IBM PC contract to Zilog embarked on producing the 32-bit chip and called it the 386 \[Ed: A likely story!\].

The 386 is upwards compatible from the 8088 but has 32-bit registers and a 32-bit data path. Its address bus can access up to 4Gbytes of memory. It was designed to handle both “multi-tasking” and “multi-user”. Each user can have up to 64 terabytes of virtual-address space.

Now, producing circuit boards with 32-bit wide data paths is a lot harder than producing ones with 16-bit paths. So Intel came up with the idea of having a 32-bit width internally and a 16-bit external path (just like the old 8088 compared to the 8086). So, if they had had any sense, not to confuse anyone, they would have used the old naming standard and the 388 would have been born, except they didn’t. They actually called it the 386SX, and just in case anyone was confused, they started calling the 386 (with the full 32-bit data path) the 386DX.

The advantage of the 386SX was that you could take a basic 16-bit AT type board and plug in a 386SX chip to get (a substandard variety of) 32-bit computing at 16-bit prices.

### Adding Floating-point Maths

Now let’s “float” back to a “point” in time shortly after the 8086 first appeared. The 8086 only supports integer maths in its instruction set. It did not support “floating-point” maths. So, Intel produced a “8087 Maths Co-processor” which could be plugged in alongside the 8086, 8088, 80186 or 80188, to do the hard work. Intel also call the co-processor an NPX – Numeric Processor Extension, hence the “Intel 8087 NPX”.

To allow for the addition of a co-processor chip, most PC motherboards came equipped with an empty co-processor socket.

With an NPX, a machine instruction to multiply two real numbers could be processed. Without an NPX, this hardware instruction has to be emulated in software. The NPX hardware can perform this (and other maths) operations many times faster than the software. Also the NPX “can improve the speed of multiple-precision software algorithms for integer operations by 10 to 100 times” (Intel 387DX User’s Manual). To keep the 8086/8087 chips in step with each other, each NPX instruction had to be preceded with a “WAIT” instruction. Intel’s assembler ASM86 did this automatically.

With the advent of the 286, Intel produced a new co-processor, the 80287 NPX. The 80286/80287 operating in “Real-Address mode” will execute 8086/8087 programs without major modifications. However differences between the way 8087 and 80287 handle numeric exceptions may require different exception handling routines. Also, the 80287 does not require “WAIT” instructions. So reassembling using Intel’s ASM286 assembler will produce faster and more compact code.

For the 386DX they produced the 387 DX NPX which Intel now tend to call the “387 DX Math Co-processor” and for the 386SX the “387 SX Math Co-processor” (and of course a new assembler ASM386). Although compatible, the 387 differs from the 8087/80287 in some minor ways mainly to bring it into line with the IEEE Standard 754 for Binary Floating-Point Arithmetic.

Since the SX version was much cheaper to produce than the DX, IBM compatible 386SX PCs sold in their millions.

Now APL does a lot of high precision maths (both integer and real), so a 32-bit 386 PC with NDX is just what Dyadic needed for its Dyalog APL, and STSC needed for its new second-generation APL “APL\*PLUS II”.

### 486 – the Next Stage

The next stage in Intel’s plan for global domination was to merge the 386DX and 387DX into a single chip. This they called the 486DX or sometimes “i486”, but NEVER (by Intel) the 80486. (Intel’s internal code name for the range is P4.) These were sold to run at 20 (rare), 25, 33 and 50Mhz.

Just a quick note on processor speed. All these 486s are produced to the same specification, and are subsequently speed rated via testing, so a 486DX-25 is a 486DX which failed the 33MHz test but passed the 25MHz test.

When someone discovered you could run some of these chips at twice the external clock speed, the 486 DX2 was born. DX2s are available as 40, 50 and 66 MHz versions but not 100Mhz. When sold as the “OVERDRIVE” version they still use the DX (not DX2) name. The Overdrive socket is 486’s equivalent to the co-processor socket found on earlier PCs. (Note, Overdrive sockets now come in at least two colour-coded flavours, blue and white, but that’s jumping the gun a bit.)

Now, legend has it that one batch of 486DXs failed to pass Intel’s quality control, because the NDX part was not working. Rather than just binning them the marketing men at Intel came up with the idea of selling them under the name of 486 SX, under the impression that SX would sell anything.

An example might help here. My computer has an “80486” mainboard, which can (according to the “User’s Guide”) run at 25, 33 40 and 50 MHz. It can be configured with the following “169 pin PGA CPU” micro-processors:

80486SX-25, 80487SX-25, 80486DX2-50 CPU running at 25MHz  
80486DX-33, 80486DX2-66 CPU running at 33MHz  
80486DX-40 CPU running at 40 MHz  
80486DX-50 CPU running at 50 MHz

It can’t run the rare 486DX-20 or 486DX2-40.

What, you may ask, is an 80487SX-25? It took me some time to find out. It is a full i486DX-25 designed to be used in the OVERDRIVE socket of a motherboard fitted with an i486SX-25. It’s a co-processor with an SX name!

### Onward to the Pentium and P6

After the P4 comes P5 the “Pentium”. This Intel chip has 2 on-board 486s, with 64-bit addressing. Because Intel could not copyright the number 586, they coined the name Pentium. As you might expect, a Pentium runs about twice as fast as a 486 with the same clock speed.

Next comes the i486DX4. Knowing how well Intel have named their chips, you won’t be surprised to discover this is a clock-tripled 486, with a larger (16k) processor cache. Hands up all of you who thought Intel could count.

And for the future, the P6 which, as they say, will be history.

Oh, I forgot. The moral of this story is: if you want your APL to run fast on a PC with “Intel Inside”, get a Pentium, and if you can’t afford a Pentium get an i486DX4. Or of course you could wait for Intel’s next chip, the P6. But please, no SX, we’re APLers.

Other Great Titles in this Series:

- “Stop the Greenhouse Effect” Using a low power consumption i486SL
- “Clone Wars” The ADM invasion
- “Growing up with Cyrix” Replace your Intel 386SX or 386DX with Cyrix Cx486SRX2
- “Blue Lightning” IBM strikes back with the clock-tripled 486SX3-99
- “Anything Blue can do, ‘i’ can do better” Intel’s latest chips, the clock-tripled i486DX4

### Appendix: Power Saving Management System

To reduce the total power used in portable PCs, all Intel and Cyrix 486 chips now incorporate “System Management Mode” (SMM). This progressively reduces the power used by a processor by powering down parts of the chip which are not being used at the time.

When SMM was first introduced by Intel, the chips using it were designated as “486SL”. However, this feature has been incorporated in all Intel 486 chips since the start of 1994, and the SL named chips are no longer made.

All Cyrix 486 chips have SMM as standard.

### 486SLC and 486DLC

The IBM chip 486SLC is logically the same as an i486SX, (it has a cache controller but no co-processor) but has 386DX pin out. This means that it can be put onto boards designed for 386DX chips, providing PC manufacturers with a cheap 386 to 486 upgrade path.

The Cyrix 486DLC chip is like the IBM 486SLC but DOES have a co-processor. I have been told that Cyrix don’t actually make chips, they just design them and IBM actually produces them for Cyrix.

### Intel Math Co-processor Identification Notes

Every Intel Math Co-processor is marked to indicate its product number and maximum operating speed. Currently, Intel marks the top of each chip with the math co-processor product number, a dash, then a number representing the maximum speed of the chip. Usually this number represents the speed in megahertz (MHz), but Intel’s previous marking scheme was slightly different. The numbers in parentheses in the following chart are from this previous marking scheme.

     Product          Rated          Intel  
     Number          Speed          Model #  
     8087 (-3)          4.77MHz     BOX8087  
     8087-8 (-2)          8MHz          BOX8087-2  
     8087-10 (-1)          10MHz     BOX8087-1  
     80287 (-6 or -3)     6MHz          BOX287  
     80287-8          8MHz          BOX287-8  
     80287-10          10MHz     BOX287-10  
     80C287A          12MHz     DBOXC287A-12  
     387DX-16          16MHz     BOX387DX-16  
     387DX-20          20MHz     BOX387DX-20  
     387DX-25          25MHz     BOX387DX-25  
     387DX-33          33MHz     BOX387DX-33  
     387SX               16MHz     BOX387SX-16

<table><tb>
<tr>
<th>The Intel 486/Pentium &amp; Cyrix ranges:<br>(This will be incomplete and out of date by the time you read this article)</th>

</tr>
</tb></table>

### INTEL Chips

<table><tb>
<tr>
<th>Processor</th>
<th>MHz</th>
<th>Volts</th>
<th>Watts</th>
<th>Co-Processor</th>
<th>Intel code<br>i486 SX</th>
<th>16</th>
<th>5/3.3</th>
<th>1.7</th>
<th>i487SX-16 overdrive</th>
<th>P23-16<br>i486 SX</th>
<th>20</th>
<th>5/3.3</th>
<th>2.3</th>
<th>i487SX-20 overdrive</th>
<th>P23-20<br>i486 SX</th>
<th>25</th>
<th>5/3.3</th>
<th>2.8</th>
<th>i487SX-25 overdrive</th>
<th>P23-25<br>i486 SX</th>
<th>33</th>
<th>5/3.3</th>
<th>3.0</th>
<th>i487SX-33 overdrive</th>
<th>P23-33<br>i486 DX</th>
<th>25</th>
<th>5/3.3</th>
<th>2.8</th>
<th>Internal</th>
<th>P4-25<br>i486 DX</th>
<th>33</th>
<th>5/3.3</th>
<th>3.5</th>
<th>Internal</th>
<th>P4-33<br>i486 DX</th>
<th>50</th>
<th>5</th>
<th>4.0</th>
<th>Internal</th>
<th>P4-50<br>i486 SX2</th>
<th>50</th>
<th>5/3.3</th>
<th>3.5</th>
<th>i487SX2-50 overdrive</th>
<th>P23-50<br>i486 DX2</th>
<th>40</th>
<th>3.3</th>
<th>1.4</th>
<th>Internal</th>
<th>P24-40<br>i486 DX2</th>
<th>50</th>
<th>5/3.3</th>
<th>3.9</th>
<th>Internal</th>
<th>P24-50<br>i486 DX2</th>
<th>66</th>
<th>5</th>
<th>4.9</th>
<th>Internal</th>
<th>P24-66<br>i486 DX4</th>
<th>75</th>
<th>3.3</th>
<th>3.0</th>
<th>Internal</th>
<th>P24C-75<br>i486 DX4</th>
<th>100</th>
<th>3.3</th>
<th>4.0</th>
<th>Internal</th>
<th>P24C-100<br>Pentium </th>
<th>60</th>
<th>5 </th>
<th>12.0</th>
<th>Internal</th>
<th>P5-60<br>Pentium </th>
<th>66</th>
<th>5 </th>
<th>13.0</th>
<th>Internal</th>
<th>P5-66<br>Pentium </th>
<th>90</th>
<th>3.3 </th>
<th>4.0</th>
<th>Internal</th>
<th>P54C-90<br>Pentium </th>
<th>100</th>
<th>3.3 </th>
<th>4.0</th>
<th>Internal</th>
<th>P54C-100<br>???????      133-200</th>
<th>?</th>
<th>?</th>
<th>Internal</th>
<th>P6</th>

</tr>
</tb></table>

### CYRIX chips

<table><tb>
<tr>
<th>Processor</th>
<th>MHz</th>
<th>Volts</th>
<th>Watts</th>
<th>Co-Processor<br>Cx486DX-33</th>
<th>50</th>
<th>5</th>
<th>3.05</th>
<th>Internal<br>Cx486DX-V33</th>
<th>33</th>
<th>3</th>
<th>1.38</th>
<th>Internal<br>Cx486DX-40</th>
<th>40</th>
<th>5</th>
<th>3.42</th>
<th>Internal<br>Cx486DX-50</th>
<th>50</th>
<th>5</th>
<th>3.82</th>
<th>Internal<br>Cx486SLC-33</th>
<th>33</th>
<th>5</th>
<th>2.47</th>
<th>None?<br>Cx486SLC2-50</th>
<th>50</th>
<th>5</th>
<th>3.40</th>
<th>None?<br>Cx486DX2-50</th>
<th>50</th>
<th>5</th>
<th>3.82</th>
<th>Internal<br>Cx486DX2-V50</th>
<th>50</th>
<th>3</th>
<th>1.63</th>
<th>Internal<br>Cx486DX2-66</th>
<th>66</th>
<th>5</th>
<th>4.30</th>
<th>Internal<br>Cx486DX2-V66</th>
<th>66</th>
<th>3</th>
<th>1.85</th>
<th>Internal<br>Cx486DX2-V80</th>
<th>80</th>
<th>3</th>
<th>2.06</th>
<th>Internal</th>

</tr>
</tb></table>

(webpage generated: 18 October 2006, 03:48)
