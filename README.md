# Digital_VLSI_SoC_Design_VSD

This repository documents my learning from the **VSD (VLSI System Design) ASIC Design Course**. Focused on building a *complete conceptual foundation*—from application software down to silicon layout—using **RISC‑V**, **open‑source EDA tools**, and the **OpenLANE ASIC flow**.

---
# DAY 1
<details><summary>Theory for DAY1 </summary> 
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
</details>
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

# Day 2 – Floorplanning, Power Integrity, Standard Cells, and Characterization
<details><summary>Theory for DAY 2</summary> 
1. Core and Die Planning
<img width="1434" height="619" alt="Screenshot 2026-01-26 042855" src="https://github.com/user-attachments/assets/aa0f9b8a-b489-446f-a9a7-7ca9c34181b7" />


This stage defines the physical boundary of the chip and the region where logic will actually be placed. The die is the total silicon area, while the core is the active region reserved for standard cells. Decisions made here silently control congestion, timing feasibility, and routing success later in the flow.

Core size is derived from cell area and utilization factor, not guessed

Aspect ratio (height/width) impacts wirelength and clock distribution

Lower utilization improves routability but increases area

It was implented and found out as below:
Total number of cells are highlighted here:
<img width="1280" height="575" alt="Screenshot from 2026-01-25 03-52-37" src="https://github.com/user-attachments/assets/de0df7e6-a6a4-4778-9a8e-163522b5d49d" />
Total number of DFF are:
<img width="1280" height="575" alt="Screenshot from 2026-01-25 03-52-37" src="https://github.com/user-attachments/assets/128c8c2e-d4ca-4d7b-8737-0e4f6f9450f7" />
Thus the Utilization factor comes as: 1613/14876 = 0.1084296
Total area came as:
<img width="1280" height="268" alt="Screenshot from 2026-01-25 03-51-37" src="https://github.com/user-attachments/assets/82f97e71-9135-461d-b561-9a0a504012ab" />

2. Power Delivery and Switching Current

<img width="1448" height="1096" alt="Screenshot 2026-01-26 043002" src="https://github.com/user-attachments/assets/65ab74b6-5237-48d2-8aa6-49dee85d4a40" />


As designs grow complex, simultaneous switching creates large transient current demands. Power is not ideal; resistance and inductance in the supply path cause voltage droop and ground bounce. This makes power planning a functional requirement, not just a reliability concern.

High di/dt during clock edges stresses VDD/VSS

IR drop and Ldi/dt directly affect logic correctness

Power grid and decoupling are planned early to avoid failures

3. Noise Margin and Signal Integrity

<img width="1482" height="735" alt="Screenshot 2026-01-26 043014" src="https://github.com/user-attachments/assets/76193c93-e4e7-47e4-a4c9-7bd0066c7b7a" />


Real digital signals contain noise bumps rather than clean transitions. Noise margins define how much disturbance a signal can tolerate before logic interpretation becomes unstable. This concept explains why power integrity, routing, and cell choice are tightly linked.

Signals between VIL and VIH enter undefined behavior

Small noise is acceptable if it stays within margin limits

Poor margins lead to random logic failures, not timing errors

4. Logical Cell Placement Blockages


<img width="1131" height="788" alt="Screenshot 2026-01-26 043233" src="https://github.com/user-attachments/assets/9c391314-ff85-4c11-96dc-f608892810d5" />

Placement blockages guide the placer by reserving regions where standard cells must not be placed. These are typically around macros, clock structures, or routing-critical zones. Proper blockage planning prevents congestion and CTS issues later.

Prevents cells from occupying sensitive regions

Improves routing predictability

Makes the design placement-ready

5. Standard Cell Library Structure


<img width="1392" height="826" alt="Screenshot 2026-01-26 043530" src="https://github.com/user-attachments/assets/814d81b9-e808-4f4e-b1f2-5fddc5477a79" />

Standard cells are provided in multiple versions of the same logic. Each variant trades off speed, power, leakage, and area. Physical design tools rely on these choices to meet timing and power goals automatically.

Same logic function, different drive strengths (size1, size2, size3)

Multiple threshold voltages (HVT, LVT, etc.)

Cell choice directly affects timing and leakage

6. Cell Design Flow


<img width="1301" height="818" alt="Screenshot 2026-01-26 043636" src="https://github.com/user-attachments/assets/37df0f7c-3a3d-4069-b870-ae520b080c1e" />

Cell design begins with the PDK and ends with a fully characterized library. Only after layout correctness and electrical extraction can a cell be used safely by synthesis and P&R tools.

Inputs: PDK rules, SPICE models, design specs

Steps: circuit design, layout, verification, characterization

Outputs: LEF, GDS, CDL, SPICE, timing and power libraries

7. Timing and Cell Characterization


<img width="1325" height="818" alt="Screenshot 2026-01-26 043706" src="https://github.com/user-attachments/assets/45923449-39f9-4512-93bc-cbbe9d5b06a1" />

Characterization converts transistor-level behavior into numbers usable by digital tools. Controlled input pulses, loads, and supplies are used to observe real waveforms and extract delay and power data.

Input stimulus applied under defined conditions

Output response measured at threshold crossings

Data feeds timing and power models

8. Timing Thresholds and Propagation Delay


<img width="1060" height="730" alt="Screenshot 2026-01-26 043724" src="https://github.com/user-attachments/assets/68a452f9-4c8d-475d-820d-1136eaf2e77b" />

Timing is measured using defined voltage thresholds rather than ideal transitions. Propagation delay is calculated as the time difference between input and output threshold crossings, which explains why delays vary with slew and load.
<img width="768" height="612" alt="Screenshot 2026-01-26 043814" src="https://github.com/user-attachments/assets/8d22973c-330d-4200-b6c2-92f2c6d1acb3" />

Separate thresholds for rise and fall

Delay measured between input and output crossings


Forms the basis of .lib timing tables
</details>



Practical Lab Screenshots for DAY 2:

Variables looked up and read the files :
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-15-52" src="https://github.com/user-attachments/assets/d80ef436-1471-41d8-ba29-f3d755c88ecd" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-18-01" src="https://github.com/user-attachments/assets/388dc084-35ad-452e-b90e-95b4cda7c736" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-18-34" src="https://github.com/user-attachments/assets/ddc2ee3e-2538-4773-ab10-cff72293ba99" />
Config.tcl file:
<img width="1280" height="377" alt="Screenshot from 2026-01-27 02-21-30" src="https://github.com/user-attachments/assets/4f6567f5-0326-4d68-bb74-a4a474f3673a" />
Making Docker (command is highlighted):
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-24-26" src="https://github.com/user-attachments/assets/ec5c1053-7ddb-46b6-b69d-bb1d0a826e0c" />
Running synthesis:
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-26-47" src="https://github.com/user-attachments/assets/4bb66022-e0f9-49bc-a840-0d7c2374b346" />
Synthesis was complete:
<img width="1280" height="768" alt="Screenshot from 2026-01-25 03-51-45" src="https://github.com/user-attachments/assets/9f22a9e5-15bb-429f-8d53-31d9d9d1f2e7" />
STA analysis reports:
<img width="1280" height="768" alt="Screenshot from 2026-01-25 03-57-10" src="https://github.com/user-attachments/assets/03cddabd-b41d-4961-a680-e3f4c613030c" />
Floorplanning was completed:
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-29-09" src="https://github.com/user-attachments/assets/f9296614-7390-4f1c-8cf1-caddaf1ee9b7" />
Checking values for V,H IO PINS metal layer / Core utilization :
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-35-27" src="https://github.com/user-attachments/assets/d9bb68c0-882b-4041-8892-39de5b7d1a6d" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-38-01" src="https://github.com/user-attachments/assets/3637161c-7405-46ad-b5c6-232da7c7ed42" />
Command to open MAGIC tool:
<img width="1280" height="222" alt="Screenshot from 2026-01-27 02-55-28" src="https://github.com/user-attachments/assets/9150077a-6ce8-45ce-9c69-558916002de9" />
MAGIC:

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-55-36" src="https://github.com/user-attachments/assets/13906016-deef-4aee-b16e-6d05aa23c143" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-55-48" src="https://github.com/user-attachments/assets/dd29ac59-8e96-4b00-88c6-e1c057cfa280" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-56-12" src="https://github.com/user-attachments/assets/21e24e35-4083-47f7-9d01-f0eea07c2206" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-56-19" src="https://github.com/user-attachments/assets/c5b21107-7949-4eca-b1db-d5dd2487c189" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-55-48" src="https://github.com/user-attachments/assets/21499f82-5b9e-434a-bbf1-00ec5f6f3961" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-56-12" src="https://github.com/user-attachments/assets/1d0ac1d0-db8d-4455-9ec0-4539d95bd3bf" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-59-17" src="https://github.com/user-attachments/assets/7eb21d3d-9afa-4757-81f7-ad7044207497" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-59-45" src="https://github.com/user-attachments/assets/c969e465-3b78-4c3f-b309-cc5b873df466" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-59-57" src="https://github.com/user-attachments/assets/4c1ead56-8b14-4400-b063-3e3e3d187c27" />

<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-00-14" src="https://github.com/user-attachments/assets/10409991-b9d5-497d-a21c-367c8ddde743" />

NOTE: Some error occured due to wrong command and SS for abnormal ouput is:
wrong command:
<img width="1280" height="218" alt="Screenshot from 2026-01-27 02-43-51" src="https://github.com/user-attachments/assets/51de1c62-1296-439d-9712-c34b166a2436" />
output :
<img width="1280" height="768" alt="Screenshot from 2026-01-27 02-44-48" src="https://github.com/user-attachments/assets/b93c2503-1876-434d-8346-6750aa2c3307" />

Now Routing was done:
Using the following command: run_placement
Global routing was hence done
<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-04-50" src="https://github.com/user-attachments/assets/da008ddb-6a3c-474e-b56d-f304792ef222" />
Placement analysis:
<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-05-06" src="https://github.com/user-attachments/assets/88a4df0d-c2d0-46a6-91ed-14b1dd316bf5" />
Using the following command routing was visualized on MAGIC:
<img width="1280" height="226" alt="Screenshot from 2026-01-27 03-07-06" src="https://github.com/user-attachments/assets/5c706d88-ffe1-4060-8230-5347d143a5fe" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-09-25" src="https://github.com/user-attachments/assets/a2187afb-e815-412c-abcf-61e02209c97f" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-07-13" src="https://github.com/user-attachments/assets/37a4a47e-8a71-4a6e-ab27-f866b96510b1" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-09-06" src="https://github.com/user-attachments/assets/0dfbed31-3621-456f-b6c6-efbfbc0f257e" />
<img width="1280" height="768" alt="Screenshot from 2026-01-27 03-08-08" src="https://github.com/user-attachments/assets/fd474b52-66b6-451a-9e56-822eb4b7119d" />

Status: Day 2 Completed

