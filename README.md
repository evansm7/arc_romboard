# Acorn Archimedes ROM/flash carrier board

v1, 31 July 2021
(c) 2021 Matt Evans


Here lies an Eagle PCB design for a ROM carrier board for older Acorn Archimedes machines.  This board allows ROMs larger than the original RISC OS 2.00 DIP28 1Mbit ROMs to be fitted, including for RISC OS 3.11.

Typically, these carrier boards aren't needed for the A3000, A440/1 and later machines, but are used on A3xx/A4xx/R140 machines.  I will refer to this generation of machine as "A440".

This board also fettles pins that are write-enable on flash parts, so might also be useful for fitting flash to machines that ordinarily don't need a carrier board (such as A3000).

This board is verified working with DIP28 1Mbit ROMs (e.g. RISC OS 2.00), JEDEC DIP32 4Mbit flash (SST39SF040) and JEDEC DIP32 4Mbit ROMs (e.g. RISC OS 3.xx).


# Construction

## Materials required

  * One 3x1 0.1" pin header (JP1)
  * One 3x2 0.1" pin header (JP2)
  * Four good-quality turned-pin DIP32 sockets
  * Good-quality turned-pin SIL pins: four lots of 32 pins (128 total)
	* A minimalist version could use about 70
  * One 3x1 0.1" right-angle pin header (LA)
  * One SMT tantalum decoupling cap (optional)


## Circuit

This board is more or less the same as the commercial/Acorn ROM carrier boards, except this board also supports 4 megabit flash chips having /WE on pin 31, pulled high.  (Some commercial boards may do, too?)

Three address lines, LA[20:18], need to be provided from the motherboard.  Although LA18 might be provided from the ROM sockets already, its presence and location depends on the configuration of the motherboard jumpers (LK12 on A440).  It is easier/cleaner to just provide one more wire and not depend on correct jumper configuration.

The motherboard LK2 and LK6 links (on A440) are assumed to be in their default "b" position, providing LA16 and LA17 to the ROM sockets.


## Soldering

You will notice that there's a chicken and egg problem:  if you solder on the sockets first, you can't reach under the sockets (unless you use SIL turned-pin sockets) to solder the SIL pins.

Solder the SIL pins in first, taking care to keep them straight.  Temporarily plugging them into a spare DIL socket whilst soldering them can help get the spacing and alignment correct.  You then have to solder some of the DIL socket pins _between_ adjacent rows of SIL pins, which is kinda tricky.  You'll need a narrow bit -- for example, a Weller 0.5mm bit is pointy enough to reach, just.

Note:  Not all 'downward' SIL pins are required, as most of the ROM socket pins are common except for the per-chip data pins.  More is better for mechanical stability and redundancy in crusty 1980s sockets, but for example one could fully populate the left and right sockets, then fill only the data pins (pins 13-21 including ground) in the middle two sockets.  That makes 82 total.

Remember to fill the solder jumper on the bottom (on the side of the silkscreen bar)!


## Configuring jumpers

The following memory types are supported:

  * Non-JEDEC DIP28 1Mbit ROM
	* Pin 24 (A16) = LA18, pin 30 = Vcc
  * JEDEC DIP32 4Mbit ROM
	* Pin 24 (/OE) = Gnd, pin 30 (A17) = LA19, pin 1 (Vpp) = Vcc, pin 31 (A18) = LA20
  * JEDEC DIP32 4Mbit flash
	* Pin 24 (/OE) = Gnd, pin 30 (A17) = LA19, pin 1 (A18) = LA20, pin 31 (/WE) = Vcc

Generally, pin 2 is A16 (connected to LA18 via a semi-permanent solder jumper), but some DIP32 1Mbit ROMs (e.g. 27C301) use /OE on pin 2.
It's not worth having a regular jumper for these (1Mb pffft), but the option's there via a bodge wire from the solder jumper.


# Installation

The SIL pins on the bottom plug into the four ROM sockets, taking care with alignment and orientation.  The LA pin header should point "south" on the motherboard (see the nearby legend).

Three jumper wires are required to be soldered to the motherboard to pick up LA[20:18].

On an A440, this is most easily achieved using short wires to IC28 (to the south of the backplane socket):

   * IC28 pin 18 = LA18
   * IC28 pin 17 = LA19
   * IC28 pin 16 = LA20

Wire these, via about 5cm of wire, to a 3-way socket e.g. of the crimp pin/"DuPont" variety.

Don't worry, if you get the address wires plugged on backwards, nothing will explode.  But, the machine will only boot if it's installed correctly.  :-)


# License

This work is licenced under the Creative Commons [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) license.


