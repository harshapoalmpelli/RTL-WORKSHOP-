# 🛠️ Module 2 — Timing Libraries, Synthesis Methods & Flip-Flop RTL

## 🎯 Objectives

This module explores the role of **timing libraries, the SKY130 PDK, RTL synthesis techniques, and different coding styles for D flip-flops**.

The practical work includes RTL simulation, waveform verification, synthesis, technology mapping, and basic RTL optimization.

### Main Activities

* Studying the SKY130 timing library
* Understanding `.lib` files and their operating conditions
* Exploring hierarchical and flattened synthesis
* Implementing different D flip-flop RTL coding styles
* Simulating Verilog designs with Icarus Verilog
* Analyzing waveforms using GTKWave
* Synthesizing RTL using Yosys
* Mapping designs to SKY130 standard cells
* Observing synthesis optimizations for constant multiplication

---

## 🔧 Tools and Technologies

* **HDL:** Verilog
* **Simulator:** Icarus Verilog
* **Waveform Viewer:** GTKWave
* **Synthesis Tool:** Yosys
* **PDK:** SKY130
* **Timing Library:** SKY130 Liberty (`.lib`)

---

## 📚 Table of Contents

1. [Timing Libraries](#1--timing-libraries)
2. [Hierarchical and Flattened Synthesis](#2--hierarchical-and-flattened-synthesis)
3. [Flip-Flop Coding Styles](#3--flip-flop-coding-styles)
4. [RTL Simulation and Synthesis Flow](#4--rtl-simulation-and-synthesis-flow)
5. [RTL Optimization](#5--rtl-optimization)
6. [Overall Results](#6--overall-results)
7. [Conclusion](#7--conclusion)

---

# 1. 📚 Timing Libraries

## 1.1 SKY130 PDK

The **SKY130 PDK** contains technology information, standard-cell definitions, and supporting files required for designing and implementing digital circuits using the SKY130 process.

For this module, the following timing library was used:

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
```

This Liberty file provides characterization information for the standard cells used during synthesis and technology mapping.

---

## 1.2 🔍 Understanding the Library Operating Conditions

The name of the Liberty file contains information about the conditions under which the cells have been characterized.

The selected library is:

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
```

It represents the selected process, temperature, and supply-voltage conditions associated with the library characterization.

Understanding these conditions is important because cell timing and power characteristics depend on the operating environment.

---

## 1.3 📂 Exploring the `.lib` File

A **Liberty (`.lib`) file** contains detailed descriptions of standard cells in a technology library.

It includes information such as:

* Standard-cell definitions
* Input and output pins
* Functional behavior
* Timing characteristics
* Power-related information
* Operating conditions

The `.lib` file was examined to understand how standard-cell information is organized and how this information is utilized during synthesis and technology mapping.

### Result

The SKY130 Liberty file was successfully examined, providing an understanding of the standard-cell characterization information available to the synthesis flow.

---

# 2. 🏗️ Hierarchical and Flattened Synthesis

Synthesis converts an RTL description into a hardware representation that can be implemented using logic cells.

Two synthesis approaches were studied in this module:

* Hierarchical synthesis
* Flattened synthesis

These approaches mainly differ in how the module structure of the original RTL is handled.

---

## 2.1 📁 Hierarchical Synthesis

In **hierarchical synthesis**, the structure of the original RTL design is retained.

Individual modules and their relationships remain identifiable after synthesis. This makes it easier to trace the design structure and understand how different blocks are connected.

### Advantages

* Preserves module boundaries
* Easier to trace individual blocks
* Useful for debugging
* Maintains the original design organization

### Result

The design was synthesized while maintaining its module hierarchy and the relationships between the different submodules.

---

## 2.2 🌐 Flattened Synthesis

In **flattened synthesis**, the separate module boundaries are removed and the complete design is represented as a unified structure.

This allows the synthesis tool to examine logic across module boundaries and perform optimization on the complete design.

The Yosys command used for flattening is:

```text
flatten
```

Flattening can provide additional opportunities for optimization because the synthesis tool can analyze the complete logic structure together.

---

## 2.3 ⚖️ Hierarchical vs Flattened Synthesis

| Feature                   | Hierarchical Synthesis | Flattened Synthesis |
| ------------------------- | ---------------------- | ------------------- |
| Module boundaries         | Preserved              | Removed             |
| Design structure          | Maintained             | Combined            |
| Debugging                 | Easier                 | More difficult      |
| Cross-module optimization | Limited                | Greater scope       |
| Design representation     | Block-oriented         | Unified             |

### Result

Both synthesis approaches were studied to understand how module hierarchy affects the representation, debugging, and optimization of a synthesized design.

---

# 3. 🔄 Flip-Flop Coding Styles

A **D flip-flop** is a sequential logic element used to store one bit of information.

Different reset mechanisms can be described in Verilog depending on how the reset or set signal interacts with the clock.

The following three implementations were studied:

1. Asynchronous Reset D Flip-Flop
2. Asynchronous Set D Flip-Flop
3. Synchronous Reset D Flip-Flop

---

## 3.1 🔴 Asynchronous Reset D Flip-Flop

An asynchronous reset can change the flip-flop output immediately when the reset signal is asserted. The output does not need to wait for a clock edge.

### Verilog Code

```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);

always @(posedge clk, posedge async_reset)
    if (async_reset)
        q <= 1'b0;
    else
        q <= d;

endmodule
```

### Working

When `async_reset` becomes `1`, the output `q` is immediately cleared to `0`.

When the reset is inactive, the input `d` is captured at the rising edge of `clk`.

### Simulation

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

### Result

The asynchronous-reset D flip-flop was simulated successfully, and its reset and clock-dependent behavior was checked through the GTKWave waveform.

---

## 3.2 🟢 Asynchronous Set D Flip-Flop

An asynchronous set causes the output of the flip-flop to become logic `1` immediately when the set signal is asserted.

The clock does not need to transition for the set operation to occur.

### Verilog Code

```verilog
module dff_async_set (
    input clk,
    input async_set,
    input d,
    output reg q
);

always @(posedge clk, posedge async_set)
    if (async_set)
        q <= 1'b1;
    else
        q <= d;

endmodule
```

### Working

When `async_set` is high:

```text
q = 1
```

The output changes immediately.

When `async_set` is inactive, the value of `d` is captured on the rising edge of the clock.

### Simulation

```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

### Result

The asynchronous-set D flip-flop was simulated and its expected behavior was confirmed using the generated waveform.

---

## 3.3 🔵 Synchronous Reset D Flip-Flop

A synchronous reset operates only at the active clock edge.

Unlike an asynchronous reset, changing the reset signal by itself does not immediately change the flip-flop output.

### Verilog Code

```verilog
module dff_syncres (
    input clk,
    input sync_reset,
    input d,
    output reg q
);

always @(posedge clk)
    if (sync_reset)
        q <= 1'b0;
    else
        q <= d;

endmodule
```

### Working

At every rising edge of `clk`:

* If `sync_reset = 1`, `q` becomes `0`.
* If `sync_reset = 0`, the value of `d` is stored in `q`.

The reset is therefore evaluated only when the clock edge occurs.

### Simulation

```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

### Result

The synchronous-reset D flip-flop was simulated successfully, and its clock-controlled reset behavior was verified through GTKWave.

---

# 4. 🧪 RTL Simulation and Synthesis Flow

After developing the RTL designs, simulation was performed to verify their functional behavior. The verified RTL was then processed through Yosys to obtain a synthesized representation.

### Complete Flow

```text
Verilog RTL
     ↓
Icarus Verilog
     ↓
Simulation
     ↓
VCD Waveform
     ↓
GTKWave
     ↓
RTL Verification
     ↓
Yosys Synthesis
     ↓
Gate-Level Representation
     ↓
SKY130 Technology Mapping
```

### Main Steps

1. Write the Verilog RTL.
2. Create the corresponding testbench.
3. Compile the design using Icarus Verilog.
4. Run the simulation.
5. Generate the VCD waveform.
6. Analyze the waveform using GTKWave.
7. Load the RTL into Yosys.
8. Process and optimize the RTL.
9. Perform technology mapping.
10. Obtain the synthesized hardware representation.

---

## 4.1 🧪 RTL Simulation Using Icarus Verilog

Icarus Verilog was used to compile and simulate the D flip-flop RTL along with its testbench.

### Compilation

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
```

### Run the Simulation

```bash
./a.out
```

The simulation produces a VCD file containing the changes in the design signals over simulation time.

### View the Waveform

```bash
gtkwave tb_dff_asyncres.vcd
```

The waveform was analyzed for:

* Clock signal
* Reset/set signal
* Data input
* Flip-flop output

### Result

The RTL simulation was completed successfully, and the waveform demonstrated the expected operation of the D flip-flop.

---

## 4.2 ⚡ RTL Synthesis Using Yosys

Yosys was used to convert the verified RTL into a synthesized gate-level representation.

### Start Yosys

```bash
yosys
```

### Read the RTL

```text
read_verilog dff_asyncres.v
```

### Select the Top-Level Module

```text
hierarchy -top dff_asyncres
```

### Process the RTL

```text
proc
```

### Optimize the Design

```text
opt
```

### Technology Mapping

```text
techmap
opt
```

These commands process the RTL, simplify the generated logic, and prepare the design for technology-specific implementation.

### Result

The D flip-flop RTL was successfully processed using Yosys, producing a synthesized gate-level representation of the original design.

---

# 5. ⚙️ RTL Optimization

RTL synthesis tools can simplify hardware descriptions while maintaining their intended functionality.

This section demonstrates optimization using constant multiplication operations.

---

## 5.1 `mul2` Optimization

### RTL Code

```verilog
module mul2 (
    input [2:0] a,
    output [3:0] y
);

assign y = a * 2;

endmodule
```

The design multiplies the input `a` by a constant value of `2`.

### Yosys Commands

```text
yosys
read_verilog mul2.v
prep -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_5v.lib
show
write_verilog -noattr mul2_net.v
gvim mul2_net.v
```

### Result

The `mul2` design was synthesized successfully. Since multiplication by `2` is equivalent to shifting the input by one position, the synthesis process can represent this operation using wiring rather than a dedicated multiplier structure.

The synthesized netlist was generated and examined.

---

## 5.2 `mult8` Optimization

### RTL Code

```verilog
module mult8 (
    input [2:0] a,
    output [5:0] y
);

assign y = a * 9;

endmodule
```

Here, the input `a` is multiplied by the constant value `9`.

### Yosys Commands

```text
yosys
read_verilog mult8.v
prep -top mult8
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_5v.lib
show
write_verilog -noattr mult8_net.v
gvim mult8_net.v
```

### Result

The `mult8` design was synthesized successfully. Yosys optimized the constant multiplication and generated a synthesized netlist representing the required hardware more efficiently.

---

## 5.3 📄 Generated Synthesized Netlists

The synthesized netlists were generated using:

```text
write_verilog -noattr mul2_net.v
write_verilog -noattr mult8_net.v
```

The generated files were then examined using:

```text
gvim mul2_net.v
gvim mult8_net.v
```

### Result

The synthesized netlists provided a lower-level representation of the original RTL designs and demonstrated how synthesis tools optimize constant arithmetic operations.

---

# 6. 🏁 Overall Results

The following tasks were successfully completed during Module 2:

* ✅ Studied the SKY130 timing library and its operating conditions.
* ✅ Examined the contents and purpose of the Liberty `.lib` file.
* ✅ Compared hierarchical and flattened synthesis.
* ✅ Implemented an asynchronous-reset D flip-flop.
* ✅ Implemented an asynchronous-set D flip-flop.
* ✅ Implemented a synchronous-reset D flip-flop.
* ✅ Simulated the RTL designs using Icarus Verilog.
* ✅ Verified signal behavior using GTKWave.
* ✅ Processed RTL using Yosys.
* ✅ Performed synthesis and technology mapping.
* ✅ Generated synthesized Verilog netlists.
* ✅ Studied constant multiplication optimization for `×2` and `×9`.
* ✅ Observed how synthesis transforms RTL into optimized hardware structures.

---

# 7. 📌 Conclusion

Module 2 provided practical exposure to important stages of the digital VLSI design flow.

The module covered **SKY130 timing libraries, Liberty files, synthesis hierarchy, D flip-flop RTL coding styles, simulation, waveform analysis, Yosys synthesis, technology mapping, and RTL optimization**.

The experiments demonstrated how different RTL descriptions are processed by synthesis tools and converted into optimized hardware representations.

Overall, this module helped strengthen the understanding of the transition from:

```text
RTL Description
      ↓
Functional Verification
      ↓
Synthesis
      ↓
Optimization
      ↓
Technology Mapping
      ↓
Gate-Level Hardware
```

This provides a strong foundation for further work in **RTL design, digital verification, synthesis, and VLSI implementation**.
