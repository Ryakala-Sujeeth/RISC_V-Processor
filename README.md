# 8-bit 4-Stage Pipelined RISC Processor

This project details the Verilog HDL implementation of an **8-bit RISC microprocessor**.  
The design is optimized for efficient instruction execution through a **4-stage pipeline** and a **16-bit instruction set**.

---

## 🧭 Table of Contents

- [Overview](#overview)
- [Specifications](#specifications)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Setup Instructions](#setup-instructions)
- [Pipeline Architecture](#pipeline-architecture)
  - [Instruction Fetch (IF)](#instruction-fetch-if)
  - [Instruction Decode (ID)](#instruction-decode-id)
  - [Execute (EX)](#execute-ex)
  - [Writeback (WB)](#writeback-wb)
- [Instruction Set Architecture (ISA)](#instruction-set-architecture-isa)
  - [Instruction Opcodes](#instruction-opcodes)
  - [Instruction Formats](#instruction-formats)
- [Hazards and Mitigation Methods](#hazards-and-mitigation-methods)
  - [Hazards](#hazards)
  - [Mitigation Methods](#mitigation-methods)
- [Modules](#modules)
- [Verilog Schematic](#verilog-schematic)
- [Simulation Results](#simulation-results)
- [Contributing](#contributing)
- [License](#license)

---

## 📌 Overview

This project involves designing an 8-bit microprocessor using **Verilog HDL**, optimized with a **4-stage pipeline** and a **16-bit instruction set**.  
The processor uses **Harvard architecture** and RISC principles to provide high execution efficiency through:

- **IF** – Instruction Fetch  
- **ID** – Instruction Decode  
- **EX** – Execute  
- **WB** – Writeback  

Key features:

- Data width: **8 bits**  
- Clock frequency: **500 MHz**  
- Addressing modes: Register Direct, Absolute  
- Operations: Arithmetic, logical, control

---

## 🧾 Specifications

| Component               | Specification           |
|--------------------------|----------------------------|
| Microprocessor Architecture | RISC                        |
| Memory Architecture         | Harvard                     |
| Data Width                  | 8 bits                      |
| Instruction Width           | 16 bits                     |
| Instruction Memory          | 64 × 16 bits                |
| Data Memory                 | 16 × 8 bits                 |
| Register File               | 8 × 8 bits                  |
| Program Counter             | 6 bits                      |
| Clock Frequency             | 500 MHz                     |
| Addressing Modes            | Register Direct, Absolute   |

---

## 🧰 Getting Started

### Prerequisites

Install the following tools:

- [Icarus Verilog (iverilog)](https://bleyer.org/icarus/) — Verilog compiler
- [GTKWave](http://gtkwave.sourceforge.net/) — waveform viewer
- [Visual Studio Code](https://code.visualstudio.com/) (recommended)
  - Add Verilog-HDL/SystemVerilog extension

Ensure `iverilog` is in your system’s PATH.

### Setup Instructions

# Compile the Verilog source files
iverilog -o processor.vvp processortb.v

# Execute the compiled testbench
vvp processor.vvp

# Open the waveform viewer to analyze the results
gtkwave
