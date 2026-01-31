# Digital_VLSI_SoC_Design_VSD


Welcome to the Digital VLSI System-on-Chip (SoC) Design repository, documenting my learning and hands-on work from the VSD IAT Digital VLSI SoC Design course. This repository captures five days of structured theory, practical labs, and an end-to-end OpenLANE RTL-to-GDSII ASIC implementation flow with the SkyWater 130nm process design kit (PDK).
This course introduced modern open-source electronic design automation (EDA) tools and guided them through complete digital SoC design concepts: from foundational VLSI principles and floorplanning to physical synthesis, placement, routing, static timing analysis, and final GDS layout generation via the OpenLANE automated flow.

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
OpenLANE ASIC Flow (Overview)

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

## Day 3 – CMOS Inverter Design, G-SPICE Simulation & CMOS Fabrication Basics

Day 3 focused on the **CMOS inverter**, starting from circuit-level understanding to **G-SPICE simulation**, followed by an introduction to the **CMOS fabrication process**. The objective was to understand how a simple inverter is modeled, simulated, and how device-level decisions impact circuit behavior.

---
<details><summary>Theory for DAY 3</summary> 
### CMOS Inverter – G-SPICE Simulation

The first lab involved simulating a **CMOS inverter** using G-SPICE. The inverter consists of two transistors: **M1 (PMOS)** connected to VDD and **M2 (NMOS)** connected to ground. To simulate this circuit, we first created a **SPICE deck**, which is the textual representation of the circuit.

To construct a valid SPICE deck, four key elements are required:
- **Component connectivity**
- **Component values**
- **Node identification**
- **Node naming**

Each MOSFET line in SPICE follows a fixed terminal order:  
**Drain, Gate, Source, Substrate**

For the CMOS inverter:
- PMOS (M1): drain = `out`, gate = `in`, source = `VDD`, substrate = `VDD`
- NMOS (M2): drain = `out`, gate = `in`, source = `0`, substrate = `0`

This allows the inverter topology to be accurately captured in text form.

---

### SPICE Deck Structure (Conceptual)

The MOSFETs are defined with their respective **W/L values**, followed by a **load capacitance** connected at the output node to model realistic delay effects. Power supply and input sources are then defined to provide operating voltages.

A **DC sweep simulation** is used to analyze inverter behavior:
- Input voltage is swept from **0 V to 2.5 V**
- Step size of **0.05 V**

A **TSMC technology model file** is included to ensure realistic transistor characteristics during simulation.

---

### Effect of Transistor Sizing

By increasing the **PMOS width (Wp)** to approximately **2.5× the NMOS width (Wn)**, the inverter transfer curve retains its shape but shifts slightly. This demonstrates how **transistor sizing directly affects switching characteristics**, making CMOS design highly flexible and tunable for performance, power, and noise margins.

---

### Static Behavior of CMOS Inverter

The key static parameter analyzed is the **switching threshold (Vm)**.

- **Vm** is defined as the point where **Vin = Vout**
- At this point, **both PMOS and NMOS are ON**
- This results in **short-circuit (leakage) current**

This operating point is critical because it directly impacts **static power dissipation** and overall inverter efficiency.

---

### CMOS Fabrication Process (16-Mask Flow)

The lab concluded with an introduction to the **CMOS fabrication process**, emphasizing how physical layers are built step-by-step using multiple masks.


**1. Substrate Preparation**  
A lightly doped silicon wafer is cleaned and prepared as the base material.

**2. Well Formation (N-well / P-well)**  
Ion implantation and diffusion create wells to house PMOS and NMOS devices.

**3. Field Oxide Formation**  
Thick oxide regions are grown to electrically isolate devices.

**4. Gate Oxide Growth**  
A thin silicon dioxide layer is thermally grown to form the MOS gate dielectric.

**5. Polysilicon Deposition**  
Polysilicon is deposited and patterned to form the gate electrodes.

**6. Source and Drain Implantation**  
Dopants are implanted to form source and drain regions.

**7. Spacer Formation**  
Sidewall spacers are created to control short-channel effects.

**8. Contact Formation**  
Contact holes are etched to connect metal layers to devices.

**9. Metal Deposition**  
Aluminum or copper is deposited to form interconnects.

**10. Silicon Nitride Deposition**  
Silicon nitride is deposited using **sputtering or CVD** to act as a passivation and protective layer, preventing contamination and mechanical damage.

**11. Passivation Layer**  
Final protective coating is applied to complete fabrication.


</details>
---


## Lab for Day 2 :
IO placing steps / command :
<img width="1280" height="768" alt="Screenshot from 2026-01-28 02-43-53" src="https://github.com/user-attachments/assets/a627d8dc-2ec7-4c74-b811-ad81c56151f8" />
Change variable size of io:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 02-52-09" src="https://github.com/user-attachments/assets/aa4d1a90-2274-4f03-847f-948387e23681" />
<img width="1280" height="768" alt="Screenshot from 2026-01-28 02-57-45" src="https://github.com/user-attachments/assets/ac02ea58-a560-44a3-9cc7-0f5512909c81" />
Clone github repo:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 03-09-05" src="https://github.com/user-attachments/assets/a5b04276-eb6c-41b4-889c-e3900ba6af90" />
to access tech file now:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 03-34-16" src="https://github.com/user-attachments/assets/57493333-18f8-477f-aa02-d162dfb698e5" />
Magic for inverter opens:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 03-37-44" src="https://github.com/user-attachments/assets/498ba843-be75-491b-87a4-a420c04bece3" />
can see what the pointer points at using what:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 03-59-16" src="https://github.com/user-attachments/assets/d0bb2e46-1e0d-49bb-b2c5-2b5bcbc5598f" />
opening the vim filw to make the spice deck:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 04-00-28" src="https://github.com/user-attachments/assets/321ef21f-9db0-4734-845d-82c7c9837c70" />
look for model actual name and change it in deck:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 04-12-01" src="https://github.com/user-attachments/assets/580439d7-3335-4160-9a2a-51b3ee49a21c" />
Correct .spice file :
<img width="1280" height="768" alt="Screenshot from 2026-01-28 06-01-09" src="https://github.com/user-attachments/assets/5e4ccf7e-3eaa-408e-bacb-eded8dba93d4" />
Running ngspice and plotting it:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 05-58-14" src="https://github.com/user-attachments/assets/be1af30a-5f46-4272-9665-a03c3d577424" />
<img width="1280" height="768" alt="Screenshot from 2026-01-28 05-59-44" src="https://github.com/user-attachments/assets/023da87f-4313-4e19-91cd-b909cc69eb15" />
zoomed till need for characterization:
<img width="1280" height="768" alt="Screenshot from 2026-01-28 06-04-43" src="https://github.com/user-attachments/assets/9e86144d-9f6e-49da-ae03-a8b7a06b64de" />


### Key Takeaway

This day bridged the gap between **circuit simulation and physical realization**, highlighting how **SPICE modeling, device sizing, static behavior, and fabrication steps** are all tightly coupled in CMOS VLSI design.

# 📅 Day 4 – Clock Tree Synthesis & Timing Analysis

## 📌 Overview
Day 4 focuses on **Clock Tree Synthesis (CTS)** with emphasis on **power-aware buffering, clock integrity, glitch impact, and post-CTS timing analysis**. The objective is to understand how clock distribution, buffering strategy, and decoupling techniques affect **skew, setup/hold timing, and functional correctness**.

---

## 1️⃣ Power Aware Clock Tree Synthesis (CTS)

Clock Tree Synthesis is the process of distributing the clock signal from the source to all sequential elements while minimizing **clock skew**, controlling **latency**, and optimizing **power consumption**.  
Power-aware CTS ensures that buffers are inserted only where required and that all clock branches drive **balanced capacitive loads**.

Key ideas covered:
- Multi-level clock buffering
- Identical buffers at the same hierarchy level
- Equal load driven at every node
- Delay dependency on input slew and output load

<img width="1791" height="1074" alt="Screenshot 2026-02-01 000458" src="https://github.com/user-attachments/assets/20950378-d679-4bd9-84dc-aa591e02749b" />

---

## 2️⃣ Buffer Delay Characterization using Liberty Tables

Clock buffer delay is characterized using **Liberty delay tables**, where delay is a function of:
- **Input slew**
- **Output load capacitance**

These tables help CTS tools choose the appropriate buffer size to maintain clock edge quality while avoiding unnecessary power overhead.

Observations:
- Increasing output load increases propagation delay
- Poor input slew degrades clock transition quality
- Proper buffer sizing avoids over-buffering and excess dynamic power

---

## 3️⃣ Load Balancing in Clock Buffer Trees by H tree

A symmetric buffer tree was constructed to ensure that:
- Each node at the same level drives the same load
- Identical buffers are used at identical hierarchy levels

Assumptions:
- C1 = C2 = C3 = C4 = 25 fF  
- Cbuf1 = Cbuf2 = 30 fF  

Calculated capacitances:
- Total capacitance at node A = 60 fF  
- Total capacitance at node B = 50 fF  
- Total capacitance at node C = 50 fF  

This symmetry helps minimize clock skew and ensures predictable clock latency.



<img width="903" height="751" alt="Screenshot 2026-02-01 000619" src="https://github.com/user-attachments/assets/5b9bd6ed-076d-43c7-b2c4-047c6d605c49" />


---

## 4️⃣ Role of Decoupling Capacitors (DECAP)

Decoupling capacitors (DECAPs) are inserted near clock buffers and logic blocks to:
- Stabilize supply voltage
- Reduce IR drop
- Absorb transient switching currents
- Improve clock signal integrity

Proper DECAP placement is critical in high-frequency clock networks to prevent voltage droop and jitter.




---

## 5️⃣ Clock Glitches and Their Impact on Functionality

Clock glitches can cause unintended switching of sequential elements, leading to **incorrect data being latched**.  
In memory and control logic, even a small glitch can corrupt stored data and result in functional failure.

This section highlights how coupling capacitance and improper buffering can introduce glitches into the clock path.

<img width="1089" height="717" alt="Screenshot 2026-02-01 000643" src="https://github.com/user-attachments/assets/75956afa-08fb-478d-b727-40129175d3fb" />

---

## 6️⃣ Post-CTS Static Timing Analysis (STA)

After CTS, **Static Timing Analysis (STA)** is performed to validate setup and hold constraints.

Steps performed:
- Read CTS netlist
- Read max and min liberty files
- Apply SDC constraints
- Set propagated clocks
- Analyze timing paths

Key parameters analyzed:
- Data arrival time
- Data required time
- Setup and hold slack
- Clock latency and skew



---

## 7️⃣ Setup and Hold Slack Analysis

Both **setup** and **hold** timing were analyzed to ensure timing closure.

- Setup slack violations indicate data arriving late
- Hold slack violations indicate data arriving too early

The reports show both **violated** and **met** paths, helping identify critical timing paths in the design.



---

## 8️⃣ Clock Skew Analysis

Clock skew was evaluated using:
- `report_clock_skew -setup`
- `report_clock_skew -hold`

Observations:
- Clock latency differences across endpoints
- Clock reconvergence pessimism (CRPR)
- Skew maintained within acceptable limits

Balanced CTS ensures skew remains controlled across the design.

#Lab for day 4

<img width="1004" height="202" alt="Screenshot 2026-02-01 000816" src="https://github.com/user-attachments/assets/e1fe9c39-cdd6-4612-852c-3fcbf8668640" />
<img width="587" height="190" alt="Screenshot 2026-02-01 000859" src="https://github.com/user-attachments/assets/06739c73-5317-439f-81f0-1fe9565edfbd" />
<img width="662" height="827" alt="Screenshot 2026-02-01 000910" src="https://github.com/user-attachments/assets/d2543bf7-0fbb-46fe-83fd-449c3a970ce5" />
<img width="826" height="744" alt="Screenshot 2026-02-01 001005" src="https://github.com/user-attachments/assets/8f23d652-aa73-4c48-83ee-5235de395a6c" />



---

## 📘 Key Learnings from Day 4
- Importance of power-aware CTS in modern VLSI designs
- Role of buffer sizing and load balancing in skew reduction
- Impact of glitches on functional correctness
- Significance of DECAP placement in clock networks
- Interpreting post-CTS STA reports for timing closure


