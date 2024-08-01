# TICK


A minimalist 8-bit "TTL" microcomputer based on a bit-serial ALU.

TICK is a variation of THE 16-bit MITE, bit serial machine.

The Accumulator and RAM bus have been truncated to 8-bits. However the Program Counter and the B-Register remain at 16-bits wide.

MITE provided the test bed for the discrete logic and bit-serial ALU. 

MITE further refined the ALU and timing pulse generator into just a 6 package module, and equipped it with 8-bit parallel interfaces, suitable for connecting with a standard parallel, tristate memory data bus.

Only so much can be some in the "Digital" simulator, so now I have begun to build up the design on breadboard, using currently available 74HCxx series logic devices.

This will include an efficient, parallel memory sub-section, to accompany the compact TICK bit serial ALU and a means of programming the contents of the SRAM using an Arduino NANO or Raspberry Pi Pico - or clone.


TICK - An Experimental 8-bit Bit Serial CPU.


TICK is an 8-bit, bit serial machine, with a modified Harvard architecture. This means that a ROM provides instructions and data, and volatile storage of variables is from RAM - in this case a 32K x 8-bit SRAM.

TICK is based around a bit-serial architecture, consisting of an ALU, an Accumulator AC, a Memory Buffer Register B, and a Program Counter PC.

In addition to these registers there are functional blocks of the Clock Sequencer and Timing Pulse Generator CS and TPG.

The advantage of the bit-serial design is that it can easily be extended from 8-bits to 16-bits or even 32 bits with very little additional hardware.

TICK is an experimental machine, and with only about 20 ICs, it may be prototyped onto about 8 standard breadboards, or fit onto a low-cost 10 x 10 cm pcb.

![Breadboards](https://github.com/monsonite/TICK/assets/758847/54edd090-5425-4285-8618-5579dcbcf4ba)

![SRAM_NANO](https://github.com/monsonite/TICK/assets/758847/4aa6a902-1ecf-499a-a364-40c40dc478d5)



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

This section is the timing generator at the top 4017 decade counter and 74xx00 acting as SR latch. The actual prototype will also use a 74HC4060 14 stage counter to act as clock generator.

In the middle is the ALU. This a much tried and tested design consisting of a 74xx86 quad 2-input XOR and a pair of 74xx00 quad 2-input NANDs.

The first pair of XOR gates allow the A and B inputs to be selectively inverted for subtraction, negation and some of the logic "inverse" functions.
The next pair of XORs and NANDs form a full adder to give the Sum and Carry produced from the A and B inputs.

Three NAND gates forms a multiplexer, to choose between the Sum of A and B (A XOR B) and the Carry (A AND B). These are re-used as the logical operations.

During a logical operation, and Carry should be suppressed - hence another pair of NANDs.

Any Carry generated is retained in the 74HC74 flipflop U12A. It is then presented to the ALU when the next pair of bits are added together - so it can be included in the sum.
At the bottom in U15 and U16 is a half adder. This is used to increment the program counter after each instruction or jump to a new address. 

Any carry generated by the PC is handled in the other half of the D-type flipflop U12B.

Apart from the counters and flipflops - much of this sheet is combinational logic and could easily be replaced by a look-up table in a very small PROM.


![ALU_Timing_generator](https://github.com/monsonite/TICK/assets/758847/b7a641b4-8933-44d6-ac6f-a9e7afd56d5a)


3. Program Counter.

The 16-bit Program Counter is based around a pair of 74HC595 shift registers.

They are combined with the half adder - on the ALU sheet, so that they increment every instruction cycle, or can be loaded from serial data from the Accumulator.

The output is a 16-bit parallel word used to address the SRAM.

It could be argued that the parallel output register in the 74HC595 is doubling up as the Memory Address register MA.

The 74HC595 is a very versatile chip for interfacing to parallel RAM or parallel peripherals.

![Program_Counter](https://github.com/monsonite/TICK/assets/758847/5993c8f1-259d-4a37-9df5-f91019604054)

4. Memory Buffer Register MB and Switch Register SR

This is  the "Switch Register" and the "B" Shift Register which is a 74HC165 U3.

In the simulator the switches are just a convenient way to enter a byte of data - just like the front panel switches on old minicomputers.

The 74HC165 loads in the parallel word at the start of each instruction cycle and converts it to the serial pulse train Bout - which is then fed into the ALU.

![Registers_MB_SR](https://github.com/monsonite/TICK/assets/758847/16103b81-338e-417a-8a67-092e72e723c1)



5. Accumulator AC

The 8-bit Accumulator based around a single 74HC595 8-bit shift register.

If you want a 16-bit machine you just chain another 74HC595 in series with this.

It is a versatile shift register because it has a parallel output latch, so you can store the final result in parallel form - possibly for writing to a memory bus.

Fout is the 8-bit serial function that is output from the ALU.

Aout is the output of the Accumulator which is recirculated through the ALU, for example during addition.

T8 is the ninth timing pulse (they start at T0). It latches the contents of the Accumulator shift-register onto the parallel bus at the end of the completed instruction cycle.

The diode array is just an 8-input OR, it is used to detect zero.

![Accumulator_PC](https://github.com/monsonite/TICK/assets/758847/b93d1b46-97f2-4d88-8877-a829fa67bb03)

6. Instruction Decoder.

This the Instruction Decoder - this was inspired by Marcel van Kervinck who created the Gigatron kit.

It is essentially a tiny ROM based on a diode matrix.

The instruction field is only 3 bits wide, needed to encode the 8 possible instructions.

These three bits are fed into U1 - which is a 74HC138, 3-8 line decoder.

The 3 bit input cause one of the outputs of the 74HC138 to go to logic low, the rest remaining high. From the image you can see that the 8 outputs identify the individual instruction group. Some instructions have multiple options such as choice of operands, destination etc.

LOAD

AND

OR

XOR

ADD

SUB

STORE

JUMP

Further instructions may be added later by decoding a fourth instruction bit.



In the image I apply the ADD instruction 100 in binary.

The 74HC138 selects one horizontal line of the ROM and any diodes connected to that line are pulled low. The resulting bit pattern is fed to U2 which is a 74HC540 octal inverter/driver.

This converts the instruction into a 7-bit pattern, in this case 21H.

The 7 bits define the control signals that are applied to the ALU, such as Carry, I0 and I1 select which function is chosen by the ALU output multiplexer, as follows.

00 Zero

01 XOR

10 AND

11 OR

Zero is a good way to clear the Accumulator before starting a new operation.

This circuit is entirely combinational and could be reduced to a look up table included in the same small PROM that holds the ALU.

As the ALU has 7 input control signals there are theoretically 128 possible instructions - this decoder just picks the most useful 8!

![Instruction_Decoder](https://github.com/monsonite/TICK/assets/758847/ba5bc4b1-c600-4263-8cd9-558a6c101e6e)

# Update 28-04-24

Some more progress on the MITE bit serial computer.

I can now load the Memory Buffer (MB) register from RAM, in parallel, and then with the one press of a single stepping switch it copies its contents into the Accumulator.

The serial output of the MB register goes directly to the serial input of the Accumulator.  The next stage is to pass this output through the ALU so that can be tested.

Below is the overall view of the device which now takes up 7, sparsely populated breadboards.

The IC count is 17 74HCxx packages and the SRAM. One of the Arduinos will be retained as the laptop interface to the computer memory.
You will have to open the image to view all the boards.

From the top:

1. LED bar displays to show the contents of the MB register (left) and the Accumulator (right).
 
2. MB (74HC165) left and Accumulator (74HC595) right.

3. SRAM Access. 62256 SRAM, 74HC245 bi-directional tristate buffer, 2 x 74HC595 ( Memory Access Register/ PC) Arduino support module to allow access to contents of RAM.
 
4. LED bar displays to show the contents of the address and data registers. MAR and external bus.
 
5. Clock pulse generator and timing. A 74HC4060 is an oscillator with a 14-stage binary divider. I am currently only clocking it at around 500Hz - so I can see what is happening. You can choose whichever clock division ratio you wish.

The 74HC4060 feeds a 74HC4017 which is a decoded, decade counter. This produces a gating signal that allows a trains of 8 clock pulses to be sent to the shift-registers to co-ordinate their data exchanges.

6. ALU termination connectors and another Arduino module to machine test the ALU.
The ALU has 8 inputs, of which 3 are the Ain, Bi and Cin from the shift registers
The Arduino is just a convenient way to dial in an instruction and test data to ensure the output is correct.

7. ALU and PC incrementer, plus any additional combinational glue logic. The PC consists of a pair of 74HC595 shift registers incremented or loaded from the MB register - in a jump condition.

![image](https://github.com/monsonite/TICK/assets/758847/e6eb407d-dcd4-4c1c-9c7a-9980ada55645)


