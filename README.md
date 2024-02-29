# TICK
A minimalist 8-bit "TTL" microcomputer based on a bit-serial ALU 

TICK is a variation of my MITE and SPIDER bit serial machines.  

SPIDER provided the test bed for the discrete logic, bit-serial ALU. 

MITE further refined the ALU and timing pulse generator into just a 6 package module, and equipped it with 8-bit parallel interfaces, suitable for connecting with a standard parallel, tristate memory data bus.

The next stage is to develop an efficient, parallel memory sub-section, to accompany the compact MITE bit serial ALU. This is the primary purpose of the TICK project.

If we consider a computer designed around one of the classic 8-bit microprocessors (6502, 6809, Z80, 1802) it would obviously require support chips, including ROM, RAM, some GPIO and likely a serial terminal interface. There would be some "glue logic" and possibly an address latch (1802).

Grant Searle has published minimum designs for these CPUs (except the RCA 1802). They can be implemented in between 6 to 8 packages (8 or 9 if you want some parallel I/O ports too). 

The minimum 1802 board can be done in 5 chips, provided that you use a USB to serial "FTDI" cable.

http://searle.x10host.com/6502/Simple6502.html

So, with any "TTL" computer, when assessing the chipcount, we can subtract the ROM, RAM, address decoder and any 8-bit input and output ports or serial interface - as these would be needed anyway, for any simple 8-bit microcomputer.

This effectively reduces the CPU, to ALU, Accumulator, Bus register, Address register, Program Counter and control unit, similar to that proposed in the NAND to TETRIS, "Hack" design.

The latest variant of the 8-bit MITE, is a cut down version called TICK (all my designs are named after arachnids - only SCORPION left).

Making the deductions for external ROM, RAM, glue and I/O - I believe I can get the actual CPU down to just 12 or 13 packages, with the whole design easily fitting on a 10 x 10 cm pcb.
