# Digital_VLSI_SoC_Design_VSD

This repository documents my learning from the **VSD (VLSI System Design) ASIC Design Course**. Focused on building a *complete conceptual foundation*—from application software down to silicon layout—using **RISC‑V**, **open‑source EDA tools**, and the **OpenLANE ASIC flow**.

---
# DAY 1

## 1. Big Picture: From Application Software to Silicon

### Definition

A modern digital system is built as a **stack**, starting from high‑level application software and ending at **physical transistors on silicon**. Each layer abstracts complexity from the layer above it.

### Key Layers

1. **Application Software** – User‑level programs (e.g., stopwatch app)
2. **System Software** – OS, drivers, runtime
3. **Compiler & Assembler** – Convert C/C++ into ISA‑specific instructions
4. **Instruction Set Architecture (ISA)** – Contract between software and hardware
5. **Microarchitecture (RTL)** – CPU implementation in Verilog/VHDL
6. **Physical Design** – Layout, routing, timing closure
7. **Fabrication (Foundry)** – Manufacturing using PDK rules
 <img width="1459" height="825" alt="Screenshot 2026-01-26 042322" src="https://github.com/user-attachments/assets/60bc63be-bdc6-463e-825c-6a6d4a3a80d8" />  

### Important 
Insight

> Software never talks directly to transistors. **ISA is the legal interface** between software and hardware.

---<img width="1743" height="1086" alt="Screenshot 2026-01-26 042425" src="https://github.com/user-attachments/assets/96ae4ed1-bfa2-42fe-91ef-8101c32fa975" />


## 2. Application Software

### What it is

Programs written by users to perform tasks (e.g., stopwatch, calculator, browser).

### Characteristics

* Written in **high‑level languages** (C, C++, Java, Python)
* Hardware‑independent
* Relies on system software and compilers

### Example from Screenshot
<img width="1766" height="1110" alt="Screenshot 2026-01-26 042454" src="https://github.com/user-attachments/assets/dd80b539-d38f-4d59-ae6d-5a8be66c4271" />

* A **C program for a stopwatch**
* Uses functions like `sleep()`, loops, and formatted printing

---

## 3. System Software

### Definition

System software manages hardware resources and provides services to applications.

### Components

* **Operating System (OS)** – Linux, Windows
* **Device Drivers** – Interface with hardware
* **Memory Management** – Allocation, protection
* **I/O Handling** – Keyboard, display, serial, etc.

### Role in the Flow

* Acts as a **bridge** between application software and hardware
* Ensures portability and abstraction

---

## 4. Compiler and Assembler

### Compiler

* Translates **high‑level language (C/C++)** into **assembly code**
* Performs optimizations

### Assembler

* Converts **assembly instructions** into **machine code (binary)**

### Example Observed

* C code → RISC‑V assembly (`add`, `lw`, `sw`, `jal`, etc.)
* Output is an **ELF / executable file**

### Key Point

> Different ISAs require different compilers, but the *same C code* can run everywhere.

---

## 5. Instruction Set Architecture (RISC‑V)

### Definition

ISA defines **what instructions a processor can execute**, not *how* it is built.

### Why RISC‑V

* Open‑source
* Modular and extensible
* Industry‑relevant
* Ideal for academia and startups

### Observations from Screenshot

* RISC‑V disassembly (`addi`, `sd`, `ld`, `jal`)
* Stack pointer manipulation
* Function call conventions

### Key Insight

> ISA is the **contract**: software relies on it, hardware must faithfully implement it.

---

## 6. RTL Implementation (Microarchitecture)

### Definition

RTL (Register Transfer Level) describes **how data moves between registers** every clock cycle.

### Example Core Used

* **PicoRV32** – Lightweight RISC‑V CPU core

### Key RTL Concepts Seen

* Instruction decoding
* Always blocks triggered on `posedge clk`
* Enable flags for instruction types
* Parameterized design (configurable features)

### Important Note

> Multiple microarchitectures can implement the *same ISA*.

---

## 7. ASIC Floorplan and Layout

### Floorplanning

* Defines placement of major blocks (CPU, SRAM, IO)
* Optimizes area, power, and routing

### Layout (Qflow / OpenROAD)

* Standard cells placed and routed
* Clock trees, power grids
* DRC/LVS clean design

### Screenshot Highlights

* Standard cell rows
* Routing layers
* Timing‑driven placement

---

## 8. Macros and Foundry IPs

### Macros

* Pre‑designed blocks (SRAM, ADC, GPIO banks)
* Treated as black boxes at top level

### Foundry IPs

* Provided by foundry
* Optimized and verified
* Include:

  * SRAM
  * PLL
  * IO cells
  * Analog blocks

### Key Insight

> Digital designers integrate macros; **they do not redesign SRAM from scratch**.

---

## 9. Open‑Source ASIC Ecosystem

### Components
<img width="1259" height="710" alt="Screenshot 2026-01-26 042602" src="https://github.com/user-attachments/assets/3cdcef00-ad13-402d-bfd2-949d3dea48dd" />

* **RTL** – OpenCores, LibreCores, GitHub
* **EDA Tools** – OpenLANE, OpenROAD
* **PDK** – Google SkyWater SKY130

### Importance

* Democratizes chip design
* Enables learning without expensive licenses
* Industry‑grade flow

---

## 10. OpenLANE ASIC Flow (Overview)

### Definition
<img width="1302" height="712" alt="Screenshot 2026-01-26 042705" src="https://github.com/user-attachments/assets/516f74c5-622b-44cc-8535-a32df6ca80cf" />
OpenLANE is an **automated RTL‑to‑GDSII flow** for digital ASICs.

### Major Stages

1. RTL Synthesis (Yosys)
2. Static Timing Analysis (OpenSTA)
3. DFT checks
4. Floorplanning
5. Placement
6. Clock Tree Synthesis (CTS)
7. Routing (TritonRoute)
8. RC Extraction
9. Physical Verification (DRC/LVS)
10. GDSII generation

### Key Takeaway

> OpenLANE enables **tape‑out‑ready designs** using only open‑source tools.

---

## 11. Final Day‑1 Takeaways

* Software → Hardware is a **layered abstraction**
* ISA is the most critical boundary
* RTL implements ISA logic
* Physical design turns logic into silicon
* Open‑source tools make ASIC design accessible

---

**Status:** Day 1 Completed

Next: RTL Design, Synthesis, and Timing Fundamentals
