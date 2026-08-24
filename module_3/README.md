# 🛠️ Module 3 — Introduction to Combinational and Sequential Optimization

## 🎯 Objectives

This module focuses on understanding how **combinational and sequential digital circuits can be optimized during RTL synthesis**.

The practical exercises demonstrate how Yosys simplifies logic, removes unnecessary hardware, performs constant propagation, and maps RTL designs to the **SKY130 standard-cell library**.

### Main Activities

* Understanding the fundamentals of logic optimization
* Studying combinational logic optimization
* Exploring sequential logic optimization
* Synthesizing Verilog designs using Yosys
* Mapping RTL designs to SKY130 standard cells
* Simulating designs using Icarus Verilog
* Analyzing waveforms with GTKWave
* Studying constant propagation in D flip-flops
* Observing optimization of unused counter logic
* Examining synthesized gate-level netlists

---

## 🔧 Tools and Technologies

* **HDL:** Verilog
* **Simulator:** Icarus Verilog
* **Waveform Viewer:** GTKWave
* **Synthesis Tool:** Yosys
* **PDK:** SKY130
* **Operating System:** Linux / Ubuntu

---

# 📚 Table of Contents

1. [Introduction to Logic Optimization](#1--introduction-to-logic-optimization)
2. [Sequential Logic Optimization](#2--sequential-logic-optimization)
3. [AND Gate Optimization](#3--and-gate-optimization)
4. [OR Gate Optimization](#4--or-gate-optimization)
5. [Three-Input AND Gate Optimization](#5--three-input-and-gate-optimization)
6. [D Flip-Flop Constant Propagation](#6--d-flip-flop-constant-propagation)
7. [Simulation of `dff_const1`](#7--simulation-of-dff_const1)
8. [Simulation of `dff_const2`](#8--simulation-of-dff_const2)
9. [D Flip-Flop Netlist Before Optimization](#9--d-flip-flop-netlist-before-optimization)
10. [Sequential Optimization Result](#10--sequential-optimization-result)
11. [D Flip-Flop Constraint Simulation](#11--d-flip-flop-constraint-simulation)
12. [Synthesized D Flip-Flop](#12--synthesized-d-flip-flop)
13. [Counter Optimization](#13--counter-optimization)
14. [Counter Optimization Result](#14--counter-optimization-result)
15. [Optimized Counter Circuit](#15--optimized-counter-circuit)
16. [Optimized Counter Netlist](#16--optimized-counter-netlist)
17. [Overall Result](#17--overall-result)
18. [Conclusion](#18--conclusion)

---

# 1. 📚 Introduction to Logic Optimization

Logic optimization is an important part of the digital design and synthesis process.

The main purpose of optimization is to simplify a circuit while preserving its required functionality. Efficient optimization can reduce unnecessary hardware and improve the overall implementation of the design.

In this module, optimization techniques are explored using **Verilog HDL, Yosys, and the SKY130 standard-cell library**.

The experiments mainly demonstrate how synthesis tools identify constant values, simplify logic, and eliminate redundant hardware.

### Result

Constant propagation was studied to understand how synthesis tools can replace known constant signals and simplify the resulting hardware implementation.

---

# 2. 🔄 Sequential Logic Optimization

Sequential logic contains storage elements such as flip-flops and registers.

**Sequential optimization** focuses on simplifying sequential circuits while maintaining their intended behavior.

Some of the concepts introduced in this module include:

* Sequential constant propagation
* Register optimization
* State optimization
* Retiming
* Removal of redundant sequential logic

These techniques allow synthesis tools to reduce unnecessary hardware and produce a more efficient implementation.

### Result

Different sequential optimization concepts were studied to understand how synthesis can improve the hardware representation of sequential circuits without changing their required functionality.

---

# 3. ⚙️ AND Gate Optimization

A simple two-input AND operation was implemented in Verilog and synthesized using Yosys.

### Verilog Code

```verilog
module opt_check (
    input a,
    input b,
    output y
);

assign y = a & b;

endmodule
```

The output `y` becomes high only when both inputs `a` and `b` are high.

### Yosys Commands

```text
yosys
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Result

The Verilog AND operation was successfully synthesized and mapped to the corresponding **SKY130 `and2` standard cell**.

---

# 4. ⚙️ OR Gate Optimization

A two-input OR operation was used to demonstrate synthesis and technology mapping.

### Verilog Code

```verilog
module opt_check2 (
    input a,
    input b,
    output y
);

assign y = a | b;

endmodule
```

The output becomes high when either `a` or `b` is high.

### Yosys Commands

```text
yosys
read_verilog opt_check2.v
synth -top opt_check2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Result

The OR operation was synthesized successfully and mapped to the **SKY130 `or2` standard cell**.

---

# 5. ⚙️ Three-Input AND Gate Optimization

A three-input AND operation was implemented to observe how Yosys maps multi-input logic to the available standard-cell library.

### Verilog Code

```verilog
module opt_check3 (
    input a,
    input b,
    input c,
    output y
);

assign y = a & b & c;

endmodule
```

The output `y` becomes high only when all three inputs are high.

### Yosys Commands

```text
yosys
read_verilog opt_check3.v
synth -top opt_check3
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Result

The three-input AND circuit was successfully synthesized and mapped to the **SKY130 `and3` standard cell**.

---

# 6. 🔄 D Flip-Flop Constant Propagation

Constant propagation is an optimization technique in which known constant values are propagated through the logic during synthesis.

Two D flip-flop examples were created to observe how constant values affect sequential logic.

---

## 6.1 `dff_const1`

### Verilog Code

```verilog
module dff_const1(
    input clk,
    input reset,
    output reg q
);

always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end

endmodule
```

In this design:

* When `reset = 1`, `q` becomes `0`.
* When reset is inactive and a rising clock edge occurs, `q` becomes `1`.

---

## 6.2 `dff_const2`

### Verilog Code

```verilog
module dff_const2(
    input clk,
    input reset,
    output reg q
);

always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b1;
    else
        q <= 1'b1;
end

endmodule
```

Here, both conditions assign the same constant value:

```text
q = 1
```

This provides an opportunity for the synthesis tool to simplify the sequential logic.

### Commands

```text
vim dff_const1.v
vim dff_const2.v
```

---

# 7. 🧪 Simulation of `dff_const1`

The first D flip-flop was simulated to observe the behavior of the reset and clock signals.

### Code

```verilog
module dff_const1(
    input clk,
    input reset,
    output reg q
);

always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end

endmodule
```

### Simulation Commands

```text
iverilog -o dff_const1.out dff_const1.v tb_dff_const1.v
gtkwave tb_dff_const1.vcd
```

The generated waveform was used to observe the relationship between:

* Clock
* Reset
* Output `q`

### Result

The simulation waveform demonstrated the expected behavior of `dff_const1` for reset and clock transitions.

---

# 8. 🧪 Simulation of `dff_const2`

The second D flip-flop was simulated to observe the effect of assigning a constant value under both conditions.

### Code

```verilog
module dff_const2(
    input clk,
    input reset,
    output reg q
);

always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b1;
    else
        q <= 1'b1;
end

endmodule
```

### Simulation Commands

```text
iverilog -o dff_const2.out dff_const2.v tb_dff_const2_.v
gtkwave tb_dff_const2_.vcd
```

### Result

The waveform demonstrated that the output remains at the constant value defined by the RTL, illustrating the effect of constant propagation during synthesis.

---

# 9. 🔍 D Flip-Flop Netlist Before Optimization

The `dff_const1` design was synthesized to examine its gate-level representation before comparing it with the optimized constant version.

### Yosys Commands

```text
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The `show` command was used to visualize the synthesized circuit.

### Result

The synthesized representation showed the hardware required to implement the original D flip-flop behavior.

---

# 10. ⚡ Sequential Logic Optimization Result

The constant-valued D flip-flop was synthesized to demonstrate sequential constant propagation.

### Yosys Commands

```text
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Result

The synthesis process identified the redundant sequential behavior and simplified the resulting circuit.

This demonstrates how synthesis can remove unnecessary logic when the RTL description contains values that are fixed by the design conditions.

---

# 11. 🧪 D Flip-Flop Constraint Simulation

A third D flip-flop example was used to observe reset behavior when the reset condition is evaluated synchronously with the clock.

### Verilog Code

```verilog
module dff_const3(
    input clk,
    input reset,
    output reg q
);

always @(posedge clk)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end

endmodule
```

### Simulation Commands

```text
iverilog -o dff_const3.out dff_const3.v dff_const3_tb.v
gtkwave dff_const3.vcd
```

The waveform was examined to verify the behavior of the flip-flop during reset and clock transitions.

### Result

The simulation confirmed the expected clock-dependent behavior of the D flip-flop.

---

# 12. 🔧 Synthesized D Flip-Flop

The `dff_const3` design was synthesized using Yosys.

### Commands

```text
yosys
read_verilog dff_const3.v
synth -top dff_const3
show
```

The synthesized design was represented using the available hardware structures and standard cells.

### Result

The D flip-flop was successfully synthesized, and its gate-level representation was viewed using Yosys.

---

# 13. 🔢 Counter Optimization

This experiment demonstrates how synthesis can remove unnecessary logic from a counter when only one output bit is required.

### Verilog Code

```verilog
module counter_opt(
    input clk,
    input reset,
    output q
);

reg [2:0] count;

assign q = count[0];

always @(posedge clk, posedge reset)
begin
    if(reset)
        count <= 3'b000;
    else
        count <= count + 1;
end

endmodule
```

The counter internally contains three bits:

```text
count[2:0]
```

However, only:

```text
count[0]
```

is connected to the output.

### Yosys Commands

```text
yosys
read_verilog counter_opt.v
synth -top counter_opt
show
```

The synthesized circuit can then be examined to determine which portions of the original counter are actually required.

---

# 14. ⚡ Counter Optimization Result

The synthesis tool analyzes the counter and identifies logic that does not contribute to the output.

### Commands

```text
yosys
read_verilog counter_opt.v
synth -top counter_opt
show
```

### Result

The synthesized counter retains only the logic required to generate the connected output.

Unused counter outputs and their associated logic can therefore be eliminated during optimization.

This demonstrates the importance of identifying which RTL signals are actually used by the final design.

---

# 15. 🔧 Optimized Counter Circuit

After synthesis, the optimized circuit contains only the hardware necessary for the required output.

The optimized design can be exported using:

```text
write_verilog -noattr counter_opt_net.v
gvim counter_opt_net.v
```

The generated schematic can be inspected using Yosys.

### Result

The optimized circuit contains a reduced hardware structure compared with the original three-bit counter description because only the required output functionality is retained.

---

# 16. 📄 Optimized Counter Netlist

The synthesized gate-level netlist is generated using:

```text
write_verilog -noattr counter_opt_net.v
gvim counter_opt_net.v
```

The resulting file represents the optimized hardware implementation after synthesis.

### Result

The optimized netlist demonstrates how Yosys converts the original RTL counter into a reduced hardware representation by eliminating logic that does not affect the specified output.

---

# 17. 🎯 Overall Result

The experiments in this module were successfully completed using **Verilog HDL, Yosys, Icarus Verilog, GTKWave, and the SKY130 standard-cell library**.

The following results were obtained:

* ✅ Studied the basic principles of combinational logic optimization.
* ✅ Explored sequential logic optimization.
* ✅ Synthesized a two-input AND gate.
* ✅ Mapped the AND gate to the SKY130 `and2` cell.
* ✅ Synthesized a two-input OR gate.
* ✅ Mapped the OR gate to the SKY130 `or2` cell.
* ✅ Synthesized a three-input AND gate.
* ✅ Mapped the design to the SKY130 `and3` cell.
* ✅ Demonstrated constant propagation using D flip-flops.
* ✅ Simulated D flip-flop designs using Icarus Verilog.
* ✅ Verified signal behavior using GTKWave.
* ✅ Examined synthesized D flip-flop circuits.
* ✅ Studied optimization of unused counter logic.
* ✅ Generated an optimized counter netlist.
* ✅ Observed how synthesis reduces unnecessary hardware while maintaining the required functionality.

---

# 18. 📝 Conclusion

Module 3 provided practical experience with **combinational optimization, sequential optimization, constant propagation, synthesis, technology mapping, and gate-level analysis**.

The experiments demonstrated how simple RTL logic can be mapped to appropriate SKY130 standard cells and how synthesis tools identify and remove redundant hardware.

The D flip-flop experiments showed the effect of constant propagation, while the counter experiment demonstrated how unused logic can be eliminated during synthesis.

Overall, the module provided a better understanding of how RTL designs are transformed into **simplified and optimized gate-level hardware implementations** while preserving the required circuit behavior.
