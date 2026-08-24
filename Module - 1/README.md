# Introduction to Verilog RTL Design & Synthesis

## Overview

This module introduces the basic concepts involved in **Verilog RTL design, functional simulation, waveform analysis, and logic synthesis**. The practical session focuses on understanding how an RTL description is written, tested, simulated, and finally converted into a gate-level representation.

The tools used in this module are **Icarus Verilog (iverilog)** for simulation, **GTKWave** for viewing signal waveforms, and **Yosys** for RTL synthesis and technology mapping.

The overall flow followed in this module is:

**Verilog RTL → Testbench → Simulation → Waveform Analysis → Synthesis → Technology Mapping → Gate-Level Netlist**

---

# Table of Contents

1. Simulator, Design, and Testbench
2. Introduction to Icarus Verilog
3. 2-to-1 Multiplexer Simulation
4. Verilog Multiplexer Analysis
5. Introduction to Yosys
6. RTL Synthesis and Technology Mapping
7. Gate-Level Netlist Generation
8. Complete Synthesis Flow
9. Learning Outcomes

---

# 1. Simulator, Design, and Testbench

## 1.1 Simulator

A simulator is a software-based environment used to check the behavior of a digital circuit without physically building the hardware.

It applies input signals to the design and calculates the corresponding outputs according to the Verilog description. Simulation helps identify functional errors at an early stage of the design process.

In this module, **Icarus Verilog** is used as the simulation tool.

---

## 1.2 Design

The design represents the actual digital hardware functionality that needs to be implemented.

In an RTL-based design flow, the hardware behavior is described using Verilog HDL. The RTL code defines the relationship between inputs, internal signals, and outputs.

For example, a multiplexer can be described using Verilog statements that determine which input is transferred to the output.

---

## 1.3 Testbench

A testbench is a separate Verilog program used to verify the functionality of the design.

It generates different input combinations and supplies them to the **Design Under Test (DUT)**. The resulting outputs are then observed to determine whether the circuit is functioning correctly.

A typical testbench contains:

* Input stimulus generation
* DUT instantiation
* Output observation
* Simulation timing
* Waveform generation

The testbench is used only for verification and is not synthesized as part of the final hardware.

---

# 2. Introduction to Icarus Verilog

**Icarus Verilog (iverilog)** is an open-source tool used to compile and simulate Verilog HDL designs.

It takes the Verilog design and testbench as input and produces a simulation executable. During simulation, signal changes can be recorded into a **VCD (Value Change Dump)** file.

The VCD file can then be opened in GTKWave for graphical waveform analysis.

### Simulation Flow

**Design + Testbench → Icarus Verilog → Simulation → VCD File → GTKWave**

This process makes it possible to verify the logical behavior of a design before moving toward synthesis and hardware implementation.

---

# 3. Lab: Simulation of a 2-to-1 Multiplexer

The practical exercise in this module involves designing and simulating a **2-to-1 multiplexer**.

A 2:1 multiplexer contains two data inputs, one select input, and one output.

### Signals

* `i0` – First input
* `i1` – Second input
* `sel` – Select signal
* `y` – Output

### Functional Operation

| `sel` | Selected Input | Output   |
| ----- | -------------- | -------- |
| 0     | `i0`           | `y = i0` |
| 1     | `i1`           | `y = i1` |

Therefore, the multiplexer can be represented by:

**y = sel ? i1 : i0**

---

## Step 1: Install Required Tools

The required simulation and waveform tools can be installed in Ubuntu/Linux using:

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

Icarus Verilog is used for compiling and simulating the Verilog files, while GTKWave is used to inspect the generated waveforms.

---

## Step 2: Compile the Design

After creating the multiplexer design file and its testbench, both files can be compiled using:

```bash
iverilog good_mux.v tb_good_mux.v
```

This command compiles the Verilog source files and creates an executable simulation file.

---

## Step 3: Run the Simulation

The compiled simulation can be executed using:

```bash
./a.out
```

The testbench applies different combinations of inputs and select signals during the simulation.

---

## Step 4: View the Waveform

If the simulation produces a VCD file named `tb_good_mux.vcd`, the waveform can be opened using:

```bash
gtkwave tb_good_mux.vcd
```

GTKWave displays the signal transitions against simulation time.

By examining the waveform, we can confirm that the output follows the selected input correctly.

---

# 4. Verilog Code Analysis

## 4.1 Multiplexer Design

The multiplexer RTL describes the relationship between the two input signals, the select signal, and the output.

The important signals are:

* **`i0`** – Data input 0
* **`i1`** – Data input 1
* **`sel`** – Control/select input
* **`y`** – Output signal

### Working Principle

When the select signal is low:

```text
sel = 0
```

the output is connected to the first input:

```text
y = i0
```

When the select signal is high:

```text
sel = 1
```

the second input is selected:

```text
y = i1
```

Thus, the select line determines which input reaches the output.

---

# 5. Introduction to Yosys and Logic Synthesis

## 5.1 Purpose of Synthesis

Simulation verifies the logical behavior of an RTL design, but it does not create the actual hardware structure.

**Synthesis** converts RTL code into a lower-level hardware representation. During this process, the RTL description is analyzed, optimized, and converted into logic structures that can be implemented using cells from a target technology library.

**Yosys** is an open-source RTL synthesis framework that can perform these operations.

For this module, Yosys is used with the **SKY130 standard-cell library**.

### Basic Synthesis Flow

**Verilog RTL → Yosys → Synthesis → Technology Mapping → Gate-Level Netlist**

---

# 6. RTL Synthesis and Technology Mapping

## 6.1 Launching Yosys

Yosys can be started from the terminal by entering:

```bash
yosys
```

This opens the Yosys command-line environment.

---

## 6.2 Loading the SKY130 Library

The standard-cell library is loaded using:

```bash
read_liberty -lib my_lib/lib/sky130_fd_sc_hd__*.lib
```

The Liberty library provides information about the available standard cells that can be used during technology mapping.

---

## 6.3 Reading the Verilog File

The RTL design is loaded into Yosys using:

```bash
read_verilog good_mux.v
```

Yosys reads the Verilog source and creates an internal representation of the circuit.

---

## 6.4 Running Synthesis

The design is synthesized by specifying the top-level module:

```bash
synth -top good_mux
```

The synthesis process converts the RTL description into a simplified logic representation suitable for further processing.

---

## 6.5 Technology Mapping

The synthesized logic is mapped to cells from the SKY130 library using:

```bash
abc -liberty my_lib/lib/sky130_fd_sc_hd__*.lib
```

This step replaces generic logic structures with suitable cells from the selected standard-cell library.

---

## 6.6 Viewing the Gate-Level Schematic

The resulting synthesized circuit can be displayed using:

```bash
show
```

The generated schematic provides a visual representation of the synthesized hardware.

It shows how the RTL functionality has been transformed into lower-level logic and standard-cell structures.

---

# 7. Generating the Gate-Level Netlist

After synthesis and technology mapping, the resulting circuit can be exported as a Verilog netlist.

The command used is:

```bash
write_verilog -noattr good_mux_netlist.v
```

This creates the file:

```text
good_mux_netlist.v
```

The generated file contains the synthesized hardware representation.

Unlike the original RTL code, which describes the functionality at a higher level, the gate-level netlist describes the circuit using technology-specific cells and their interconnections.

This provides a closer representation of how the design can be implemented physically using the selected standard-cell library.

---

# 8. Complete Synthesis Flow

The complete process followed in this module can be summarized as:

```text
Verilog RTL Design
        ↓
Load SKY130 Library
        ↓
Read Verilog Source
        ↓
RTL Synthesis
        ↓
Logic Optimization
        ↓
Technology Mapping
        ↓
Gate-Level Schematic
        ↓
Gate-Level Netlist
```

This flow demonstrates the transformation of a high-level RTL description into a hardware-oriented gate-level representation.

The simulation stage is used to verify functionality, while the synthesis stage converts the verified RTL into a form suitable for physical hardware implementation.

---

# 9. Learning Outcomes

After completing this module, I gained an understanding of:

* The concept and purpose of **RTL design**.
* The role of a **digital simulator**.
* The difference between a **design and a testbench**.
* The working procedure of **Icarus Verilog**.
* Compilation and execution of Verilog designs.
* Generation of **VCD waveform files**.
* Waveform analysis using **GTKWave**.
* Functional operation of a **2-to-1 multiplexer**.
* The purpose and importance of **RTL synthesis**.
* The basic operation of **Yosys**.
* Loading and using the **SKY130 standard-cell library**.
* Performing RTL synthesis.
* Mapping synthesized logic to standard cells.
* Viewing a synthesized gate-level schematic.
* Generating a gate-level Verilog netlist.
* Understanding the complete **RTL-to-gate-level design flow**.

## Conclusion

This module provided a practical understanding of the initial stages of the RTL design flow. The 2-to-1 multiplexer was used to demonstrate Verilog coding, simulation, testbench-based verification, and waveform analysis.

The synthesis section introduced Yosys and demonstrated how an RTL description can be transformed into a gate-level implementation using the SKY130 standard-cell library.

Overall, the module established a strong foundation in **Verilog RTL design, simulation, waveform verification, synthesis, and technology mapping**, which are essential concepts in digital VLSI design.
