8-bit 4-Stage Pipelined RISC Processor

This project details the Verilog HDL implementation of an 8-bit RISC microprocessor. The design is optimized for efficient instruction execution through a 4-stage pipeline and a 16-bit instruction set.

Table of Contents

Overview

Specifications

Getting Started

Prerequisites

Setup Instructions

Pipeline Architecture

Instruction Fetch (IF)

Instruction Decode (ID)

Execute (EX)

Writeback (WB)

Instruction Set Architecture (ISA)

Instruction Opcodes

Instruction Formats

Hazards and Mitigation Methods

Hazards

Mitigation Methods

Modules

VERILOG Schematic

Simulation results

Contributing

License

Overview

This project involves designing an 8-bit microprocessor using Verilog HDL, optimized with a 4-stage pipeline and a 16-bit instruction set. The microprocessor features a Harvard architecture and RISC design, offering an efficient execution of instructions through stages: Instruction Fetch (IF), Instruction Decode (ID), Execute (EX), and Writeback (WB). With a data width of 8 bits and a clock frequency of 500MHz, the processor supports various addressing modes, including Register Direct and Absolute Addressing, and a wide range of operations such as arithmetic, logical, and control instructions.

The project includes detailed modules for each stage, a robust hazard mitigation strategy to handle data, control, and structural hazards, and comprehensive testing to ensure functionality and efficiency. By integrating all components, the microprocessor ensures smooth data flow and control signal propagation across the pipeline, making it a versatile and powerful processor design.

Specifications

Microprocessor-Architecture: RISC

Memory-Architecture: Harvard Architecture

Data Width: 8 bits

Instruction Width: 16 bits

Instruction Memory: 64 x 16 bits

Data Memory: 16 x 8 bits

Register File: 8 x 8 bits

Program Counter: 6 bits

Clock Frequency: 500MHz

Addressing Modes:

Register Direct Addressing

Absolute Addressing

Getting Started

Prerequisites

Please install the following tools on your system to begin:

Icarus Verilog (iverilog): A Verilog compiler. Available at: https://bleyer.org/icarus/

GTKWave: A waveform viewer used for simulation and analysis.

Visual Studio Code (Recommended):

Ensure the iverilog runtime is available in your system's environment path.

Install the Verilog-HDL/SystemVerilog extension for syntax highlighting and formatting.

Setup Instructions

Run the following commands in your terminal to compile and simulate the processor:

# Compile the Verilog source files
iverilog -o processor.vvp processortb.v

# Execute the compiled testbench
vvp processor.vvp

# Open the waveform viewer to analyze the results
gtkwave


Pipeline Architecture

Instruction Fetch (IF)

Action: Fetches the instruction from memory.

Description: This stage retrieves the instruction located at the address specified by the program counter (PC). The PC is then incremented to point to the next instruction. This stage is crucial for ensuring that instructions are sequentially accessed and prepared for decoding.

Instruction Decode (ID)

Action: Decodes the fetched instruction.

Description: In this stage, the fetched instruction is decoded to identify the opcode, which specifies the operation to be performed. It also determines the source operands, destination register, memory address, and addressing mode.

Execute (EX)

Action: Performs the required operation on the operands.

Description: This stage utilizes the arithmetic logic unit (ALU) to execute arithmetic and logical operations. It also calculates memory addresses for load and store instructions and evaluates branch conditions.

Writeback (WB)

Action: Writes the result back to the register file or memory.

Description: In this final stage, the result of the execution is written back to the destination register. If the instruction was a memory operation, data is written to or read from memory. This ensures the result is stored and available for subsequent instructions.

Instruction Set Architecture (ISA)

Instruction Opcodes

The microprocessor supports a variety of operations through its 16-bit instructions.

Opcode

Operation

Opcode

Operation

00000

MOVE

10001

ARITHMETIC RIGHT SHIFT

00001

ADD

10010

LOGICAL LEFT SHIFT

00010

SUBTRACT

10011

LOGICAL RIGHT SHIFT

00011

MULTIPLY

10100

ROTATE LEFT

00100

DIVIDE

10101

ROTATE RIGHT

00101

INCREMENT

10110

BRANCH (CARRY FLAG)

00110

DECREMENT

10111

BRANCH (AUXILIARY FLAG)

00111

AND

11000

BRANCH (PARITY FLAG)

01000

OR

11111

HALT

01001

NOT





01010

XOR





01011

LOAD





01100

STORE





01101

JUMP





01110

BRANCH (ZERO FLAG)





Instruction Formats

R Type:

MOVE:

AM = 0: |opcode(5)|0|rd(3)|rs(3)|0000|

AM = 1: |opcode(5)|1|rd(3)|mem_add(4)|000|

ADD,SUB,MUL,DIV,AND,OR,XOR,COMP:

AM = 0: |opcode(5)|0|rd(3)|rs1(3)|rs2(3)|0|

AM = 1: |opcode(5)|1|rd(3)|rs1(3)|mem_add(4)|

INCR,DEC,NOT,all shift and rotate:

AM = 0: |opcode(5)|0|rd(3)|s_r_amount(3)|0000|

AM = 1: |opcode(5)|1|data_mem(4)|s_r_amount(3)|000|

LOAD(mem -> reg):

|opcode(5)|X|rd(3)|data_mem(4)|000|

STORE (reg -> mem):

|opcode(5)|X|data_mem(4)|rd(3)|000|

J Type:

JUMP AND BRANCH:

|opcode(5)|X|instr_mem(6)|0000|

HALT:

|opcode(5)|X|0000000000|

Hazards and Mitigation Methods

Hazards

Pipelining can introduce several types of hazards:

Data Hazards: Occur when an instruction depends on the result of a previous, incomplete instruction.

Read After Write (RAW): An instruction tries to read a register before a previous instruction has finished writing to it.

Write After Read (WAR): An instruction writes to a register before a previous instruction has finished reading from it.

Write After Write (WAW): Two instructions write to the same register, and the writes could complete out of order.

Control Hazards: Occur due to branch or jump instructions that change the program's execution flow.

Structural Hazards: Occur when multiple instructions require the same hardware resource at the same time.

Mitigation Methods

Data Hazards Mitigation:

RAW hazard: Mitigated by using level-triggered pipeline stages. This ensures that if a register value changes mid-execution, the result in subsequent stages is updated accordingly.

WAR & WAW hazards: These hazards do not occur in this design due to the sequential flow of the pipeline. Writes to the register file and data memory only happen in the Writeback stage, which processes one instruction at a time, ensuring program order is maintained.

Control Hazards Mitigation:

Pipeline Flushing: When a jump or a taken branch instruction reaches the Writeback stage, it signals the controller to flush the pipeline. The controller then resets the intermediate pipeline latches (IF/ID, ID/EX, EX/WB), discarding the incorrectly fetched instructions.

Structural Hazards Mitigation:

Resource Isolation: Hazards on the register file and memory bank are mitigated by isolating read and write operations. Reads are performed only in the Execute stage, and writes are performed only in the Writeback stage. Since these stages operate on different clock signals, resource conflicts are avoided.

Modules

processor.v: The top-level module that integrates all components.

controller.v: Generates control signals and manages hazard resolution.

pc.v: Manages the Program Counter.

instmem.v / memoryBank.v: Instruction and data memory modules.

regFile.v: The 8x8-bit register file.

instfetch.v / decoder2.v / executestage.v / write_back.v: Modules for the IF, ID, EX, and WB stages.

Latch_IF_ID.v / latch_ID_EX.v / EX_WB_Latch.v: Pipeline latches that hold data between stages.

VERILOG Schematic

Simulation results

Hazard Detection

|----------------|
|0000100010100110|
|0000111000010010|
|1001101110010000|
|1011000010000000|
|1111100000000000|
|1111100000000000|
|1111100000000000|
|1111100000000000|
|1001101110010000|
|1111100000000000|


Arthimetic and logical operations

|----------------|
|0000001001011110| 
|0101001000101110|
|0000111001010000|


Shift and Flag Instructions

|----------------|
|1000001110010000|
|1000011111001000| 
|1001101110100000|
|1001111111010000|
|0001111101101111|
|1001001110010000|
|1011000100010000|


Jump Instructions

|----------------|
|0000001011000000|
|0000001111010000|
|0101101101110000|
|1100000111000000|
|0000001011000000|
|0000001111010000|
|0000001011000000|
|0110011110100000|
|0110100011000000|
|0000001111010000|
|0000101011100000|
|0000101011100000|
|1111100000000000|


Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change. Please make sure to update tests as appropriate.

License

This project is licensed under the MIT License - see the LICENSE.md file for details.
