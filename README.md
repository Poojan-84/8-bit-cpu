# Shrike-8: Minimalist 8-Bit Accumulator CPU

A compact, 8-bit RISC-style processor designed to fit within the extremely tight resource constraints of the **Vicharak Shrike Lite FPGA** (targeting < 140 CLBs). 

This project demonstrates how to implement a functional 32-ISA architecture using an accumulator-based design, managed via an SPI-stepped execution protocol.

---

## 🏗️ Architecture Overview

The **Shrike-8** uses a "One-Bus" minimalist architecture to minimize multiplexer overhead, which is the primary consumer of CLBs in small FPGAs.

### Core Features:
- **8-Bit Data Path:** Processes values from 0-255.
- **32-Instruction ISA:** 5-bit opcodes for arithmetic, logic, shifts, and branching.
- **Accumulator-Based:** All ALU operations target a single `ACC` register to save register file logic.
- **SPI Step-Protocol:** Instructions are fed externally via SPI, allowing MicroPython or a PC to act as the "Instruction Memory."
- **Resource Optimized:** Fits in approximately **115-120 CLBs**.



---

## 🕹️ Instruction Set (Partial List)

The CPU supports 32 instructions. Key operations include:

| Opcode | Mnemonic | Description |
| :--- | :--- | :--- |
| `0x01` | **LDA** | Load Accumulator with 8-bit Data |
| `0x02` | **ADD** | Add 8-bit Data to Accumulator |
| `0x0B` | **INC** | Increment Accumulator by 1 |
| `0x07` | **LSL** | Logical Shift Left |
| `0x09` | **ROL** | Rotate Left |
| `0x0E` | **JZ** | Jump to Address if Zero Flag is set |

---

## 📂 Project Structure

* `/rtl`: Contains the Verilog source files.
    * `top.v`: Top-level module with Shrike Lite I/O attributes and SPI packet assembly.
    * `cpu_core.v`: The main CPU state machine and register logic.
    * `alu_8bit.v`: Combinational ALU supporting 8-bit arithmetic and bitwise ops.
* `/firmware`: MicroPython testing suite for board verification.
* `/docs`: Architecture diagrams and timing analysis.

---

## 🚀 Hardware Deployment

### Requirements:
- **Board:** Vicharak Shrike Lite
- **Toolchain:** Shrike IDE / Yosys & Nextpnr
- **Interface:** SPI (SCK, CS, MOSI, MISO)

### FPGA Header Syntax
The project utilizes the specific Shrike attribute syntax for I/O mapping:
```verilog
(* top *) module top (
    (* iopad_external_pin, clkbuf_inhibit *) input clk,
    (* iopad_external_pin *) input rst_n,
    // SPI pins...
);
