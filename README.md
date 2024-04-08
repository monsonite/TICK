# TICK


A minimalist 8-bit "TTL" microcomputer based on a bit-serial ALU.

TICK is a variation of my MITE bit serial machine.  

MITE provided the test bed for the discrete logic and bit-serial ALU. 

MITE further refined the ALU and timing pulse generator into just a 6 package module, and equipped it with 8-bit parallel interfaces, suitable for connecting with a standard parallel, tristate memory data bus.

Only so much can be some in the "Digital" simulator, so now I have begun to build up the design on breadboard, using currently available 74HCxx series logic devices.

This will include an efficient, parallel memory sub-section, to accompany the compact MITE bit serial ALU and a means of programming the contents of the SRAM using an Arduino NANO.

TICK - An Experimental 8-bit Bit Serial CPU.


TICK is an 8-bit, bit serial machine, with a von Neumann architecture. This means that all instructions and data come from the same memory space - in this case a 32K x 8-bit SRAM.

TICK is based around a bit-serial architecture, consisting of an ALU, an Accumulator AC, a Memory Buffer Register MB, a Memory Address Register MA, and a Program Counter PC.

In addition to these registers there are functional blocks of the Clock Sequencer and Timing Generator CS and TG.

The advantage of the bit-serial design is that it can easily be extended from 8-bits to 16-bits or even 32 bits with very little additional hardware.

TICK is an experimental machine, and with only about 20 ICs, which can be prototyped onto 5 to 8 standard breadboards, or fit onto a low-cost 10 x 10 cm pcb.

![Testboard_13](https://github.com/monsonite/TICK/assets/758847/551e335b-3515-4c64-a161-1538116b4a4d)


Breadboard #1	ALU and Carry flip-flop
Breadboard #2	Clock oscillator, clock sequencer and timing pulse generator
Breadboard #3	Accumulator AC, Zero Detector, Zero flag, control and jump logic
Breadboard #4	Memory Buffer Register MB
Breadboard #5	Memory Address Register MA
Breadboard #6	Program Counter PC, SRAM 
Breadboard #7	Input/Output
Breadboard #8	Expansion and interface to Arduino Nano - for programming

By putting the pricipal registers AC, MA, MB on their own breadboard, it means that they can easily be extended to 16-bit or 32- bit wordlength, just by "daisy-chaining" more shift register packages in a row, and modifying the clock sequencer for a longer bit-sequence.

1. ALU and Carry Flipflop

![3_package_bit_serial_ALU](https://github.com/monsonite/TICK/assets/758847/682fc201-4bd3-4901-9a1b-c02d3e5ac8fc)

2. Clock Sequencer and Timing Generator.
![ALU_Timing_generator](https://github.com/monsonite/TICK/assets/758847/b7a641b4-8933-44d6-ac6f-a9e7afd56d5a)


3. Program Counter.
![Program_Counter](https://github.com/monsonite/TICK/assets/758847/5993c8f1-259d-4a37-9df5-f91019604054)

4. Memory Buffer Register MB and Switch Register SR
![Registers_MB_SR](https://github.com/monsonite/TICK/assets/758847/16103b81-338e-417a-8a67-092e72e723c1)

5. Accumulator AC
![Accumulator_PC](https://github.com/monsonite/TICK/assets/758847/b93d1b46-97f2-4d88-8877-a829fa67bb03)




