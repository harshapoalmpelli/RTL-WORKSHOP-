# Module 5 – Optimization in Synthesis

## 📖 Overview

This module focuses on **synthesis optimization techniques in Verilog HDL** using the **Yosys synthesis tool**.

Various combinational and sequential digital circuits were designed, simulated, synthesized, and analyzed to understand how synthesis tools optimize RTL designs by removing redundant logic, simplifying expressions, minimizing hardware resources, and generating efficient hardware implementations.

The experiments also demonstrate the importance of writing complete and synthesizable RTL code to avoid unintended hardware such as latches.

---

# 📑 Table of Contents

1. [Incomplete IF Statement](#1--incomplete-if-statement)
2. [RTL Schematic of Incomplete IF Statement](#2--rtl-schematic-of-incomplete-if-statement)
3. [RTL Simulation of Incomplete IF Statement](#3--rtl-simulation-of-incomplete-if-statement)
4. [RTL Schematic of Incomplete IF-ELSE Statement](#4--rtl-schematic-of-incomplete-if-else-statement)
5. [Incomplete Case Statement](#5--incomplete-case-statement)
6. [Complete Case Statement](#6--complete-case-statement)
7. [Partial Case Assignment](#7--partial-case-assignment)
8. [Bad Case Assignment](#8--bad-case-assignment)
9. [Multiplexer Verification](#9--multiplexer-mux-verification)
10. [Demultiplexer Verification](#10--demultiplexer-demux-verification)
11. [Ripple Carry Adder](#11--ripple-carry-adder)
12. [Overall Results](#-overall-results)
13. [Conclusion](#-conclusion)

---

# 🎯 Objectives

The main objectives of this module are:

* To understand synthesis optimization techniques.
* To design and simulate digital circuits using Verilog HDL.
* To perform RTL synthesis using Yosys.
* To observe optimization performed by synthesis tools.
* To analyze generated RTL schematics.
* To identify unintended latch inference.
* To verify circuit functionality using GTKWave.
* To understand complete and incomplete conditional assignments.
* To study case statement implementation.
* To analyze multiplexer and demultiplexer behavior.
* To verify an 8-bit Ripple Carry Adder.
* To understand optimized hardware implementation.

---

# 🛠️ Tools & Technologies Used

| Tool / Technology                | Application                   |
| -------------------------------- | ----------------------------- |
| **Verilog HDL**                  | Hardware Description Language |
| **Icarus Verilog**               | Compilation & Simulation      |
| **GTKWave**                      | Waveform Analysis             |
| **Yosys**                        | RTL Synthesis & Optimization  |
| **SKY130 Standard Cell Library** | Technology Mapping            |
| **Ubuntu Linux**                 | Development Environment       |

---

# 1. ⚠️ Incomplete IF Statement

## 📖 Overview

This experiment demonstrates the behavior of an **incomplete IF statement** in Verilog HDL.

An incomplete IF statement assigns the output only when a particular condition is satisfied. If the condition is false and no alternative assignment is provided, the output retains its previous value.

During synthesis, this behavior causes the synthesis tool to infer a **latch** because the hardware must preserve the previous output value.

This experiment demonstrates why complete assignments are important when designing combinational logic.

## ⚙️ Simulation Commands

```bash
iverilog -o incom_if incom_if.v incom_if_tb.v
./incom_if
gtkwave incom_if.vcd
```

## 📷 Experimental Output

> Add your **GTKWave simulation screenshot** here.

## 📊 Observation

The output changes only when the select signal `sel` is high.

When `sel` is low, no new value is assigned to the output. Therefore, the output retains its previous value.

This behavior indicates storage or memory retention and can result in **latch inference during synthesis**.

## ✅ Result

The RTL simulation successfully demonstrated the behavior of an incomplete IF statement. The waveform confirmed that the output retains its previous value whenever no assignment is made.

---

# 2. ⚙️ RTL Schematic of Incomplete IF Statement

## 📖 Overview

This experiment focuses on synthesizing the incomplete IF statement using **Yosys**.

Since the output is not assigned for every possible input condition, Yosys infers a latch to preserve the previous output value.

The generated RTL schematic provides a visual representation of the inferred storage element.

## ⚙️ Synthesis Commands

```bash
yosys
```

```yosys
read_verilog incom_if.v
synth -top incom_if
show
```

## 📷 RTL Schematic

> Add your **Yosys RTL schematic screenshot** here.

## 📊 Observation

The generated RTL schematic contains a **latch**.

The latch stores the previous output value whenever the IF condition is not satisfied.

This confirms that incomplete combinational assignments can introduce unintended storage elements during synthesis.

## ✅ Result

The RTL schematic successfully demonstrated **latch inference** caused by the incomplete IF statement.

---

# 3. 🧪 RTL Simulation of Incomplete IF Statement

## 📖 Overview

This experiment analyzes the RTL simulation waveform of the incomplete IF statement.

The objective is to observe the output behavior when all possible input conditions are not explicitly assigned.

When the specified condition is false, the output retains its previous value instead of receiving a new value.

## ⚙️ Simulation Commands

```bash
iverilog -o incom_if incom_if.v incom_if_tb.v
./incom_if
gtkwave incom_if.vcd
```

## 📷 Experimental Output

> Add your **GTKWave waveform screenshot** here.

## 📊 Observation

The waveform shows that:

* The output changes when `sel` is active.
* The output retains its previous value when `sel` is inactive.
* The output is therefore not assigned under every possible condition.
* This behavior results in latch inference during synthesis.

## ✅ Result

The RTL simulation successfully demonstrated the behavior of an incomplete IF statement and confirmed the importance of complete assignments in combinational RTL.

---

# 4. ⚙️ RTL Schematic of Incomplete IF-ELSE Statement

## 📖 Overview

This experiment demonstrates the synthesis of an **incomplete IF-ELSE statement** using Yosys.

The RTL description is synthesized to observe how incomplete conditional assignments affect the generated hardware.

If the output is not assigned for every possible condition, the synthesis tool can infer a latch to preserve the previous output value.

## ⚙️ Synthesis Commands

```bash
yosys
```

```yosys
read_verilog incom_if2.v
synth -top incom_if2
show
```

## 📷 RTL Schematic

> Add your **Yosys RTL schematic screenshot** here.

## 📊 Observation

The generated schematic represents the synthesized hardware for the IF-ELSE design.

The synthesis process analyzes the conditional assignments and generates the corresponding hardware logic. If any condition does not assign the output, storage behavior may be inferred.

## ✅ Result

The RTL schematic was successfully generated using Yosys and demonstrated how conditional RTL coding affects the synthesized hardware implementation.

---

# 5. 🔀 Incomplete Case Statement

## 📖 Overview

This experiment demonstrates the synthesis of an **incomplete case statement**.

A case statement is incomplete when all possible values of the select signal are not covered.

If the output is not assigned for every possible condition, the synthesis tool may infer a latch to preserve the previous output value.

## 💻 Verilog Code

```verilog
module incomp_case (
    input i0,
    input i1,
    input i2,
    input [1:0] sel,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
    endcase
end

endmodule
```

## 📷 Synthesized Netlist

> Add your **Yosys synthesized schematic screenshot** here.

## 📊 Observation

The synthesized circuit shows a latch because the case statement does not define the output for all possible values of `sel`.

For example, the conditions `2'b10` and `2'b11` are not explicitly assigned.

## 🧪 Simulation Waveform

> Add your **GTKWave waveform screenshot** here.

The waveform shows that the output changes according to the defined case conditions while retaining its previous value for undefined conditions.

## ✅ Result

The incomplete case statement resulted in latch inference because the output was not assigned for every possible input combination.

---

# 6. ✅ Complete Case Statement

## 📖 Overview

This experiment demonstrates a **complete case statement**, where every possible value of the select signal is covered.

Complete assignment ensures that the output receives a value for every input condition, allowing the synthesis tool to generate purely combinational hardware.

## 💻 Verilog Code

```verilog
module comp_case (
    input i0,
    input i1,
    input i2,
    input [1:0] sel,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        default: y = i2;
    endcase
end

endmodule
```

## 📷 Synthesized Netlist

> Add your **Yosys synthesized schematic screenshot** here.

## 📷 Simulation Waveform

> Add your **GTKWave waveform screenshot** here.

## 📊 Observation

All possible select conditions are covered by the case statement.

Therefore, the output does not need to retain a previous value and no latch is required.

## ✅ Result

The synthesized design contains combinational logic without unintended latch inference. Complete case assignments provide predictable and synthesizable hardware.

---

# 7. 🔄 Partial Case Assignment

## 📖 Overview

This experiment demonstrates the effect of **partial assignments** inside a case statement.

Different output variables are assigned in different case branches. When an output is not assigned in every branch, the synthesis tool may infer a latch for that output.

## 💻 Verilog Code

```verilog
module partial_case_assign (
    input i0,
    input i1,
    input i2,
    input [1:0] sel,
    output reg y,
    output reg x
);

always @(*) begin
    case(sel)

        2'b00: begin
            y = i0;
            x = i2;
        end

        2'b01:
            y = i1;

        default: begin
            x = i1;
        end

    endcase
end

endmodule
```

## 📷 Synthesized Netlist

> Add your **Yosys synthesized schematic screenshot** here.

## 📊 Observation

The assignments to `y` and `x` are incomplete across the different case branches.

Therefore, the synthesis tool may infer latch elements for the outputs whose values are not assigned under every condition.

## 🧪 Simulation Waveform

> Add your **GTKWave waveform screenshot** here.

## 🔍 Observation

The waveform shows that an output can retain its previous value whenever the corresponding case branch does not provide a new assignment.

## ✅ Result

The experiment demonstrated how partial assignments can introduce unintended storage elements and why all outputs should be assigned for every combinational condition.

---

# 8. ⚠️ Bad Case Assignment

## 📖 Overview

This experiment verifies the RTL functionality of a case-based multiplexer using GTKWave.

Different input combinations are applied through the testbench to verify whether the output correctly follows the selected input.

## 💻 Verilog Code

```verilog
module bad_case (
    input i0,
    input i1,
    input i2,
    input i3,
    input [1:0] sel,
    output reg y
);

always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b11: y = i3;
    endcase
end

endmodule
```

## 📷 Simulation Waveform

> Add your **GTKWave waveform screenshot** here.

## 📊 Observation

The waveform shows that the output changes according to the selected input for all four possible values of `sel`.

## ✅ Result

The RTL simulation successfully verified the functional behavior of the case-based multiplexer.

---

# 9. 🔀 Multiplexer (MUX) Verification

## 📖 Overview

A **multiplexer (MUX)** is a combinational circuit that selects one input from multiple input signals and connects the selected signal to the output.

The select lines determine which input is passed to the output.

## 📊 Observation

The waveform shows that the output follows the selected input as the select signal changes.

## 📷 Simulation Waveform

> Add your **GTKWave MUX waveform screenshot** here.

## ✅ Result

The multiplexer operates correctly, and the simulation confirms proper data selection based on the select signal.

---

# 10. 🔀 Demultiplexer (DEMUX) Verification

## 📖 Overview

A **demultiplexer (DEMUX)** performs the opposite operation of a multiplexer.

It accepts a single input and routes it to one of several output lines according to the select signal.

## 📊 Observation

The waveform shows that only the selected output receives the input signal while the remaining outputs remain inactive.

## 📷 Simulation Waveform

> Add your **GTKWave DEMUX waveform screenshot** here.

## ✅ Result

The demultiplexer successfully routes the input signal to the selected output according to the select lines.

---

# 11. ➕ Ripple Carry Adder

## 📖 Overview

This experiment verifies the functionality of an **8-bit Ripple Carry Adder (RCA)**.

An 8-bit Ripple Carry Adder is constructed using multiple full-adder stages. The carry generated by one stage is propagated to the next stage.

```text
A[7:0] ───────────────┐
                      │
B[7:0] ───────────────┤
                      ↓
                8-bit Ripple
                 Carry Adder
                      │
                      ├──→ Sum[7:0]
                      │
                      └──→ Carry
```

## 📊 Observation

The waveform confirms that:

* The binary inputs are added correctly.
* The sum output is generated correctly.
* Carry propagates between successive full-adder stages.
* The final carry output is generated correctly.

## 📷 Simulation Waveform

> Add your **GTKWave Ripple Carry Adder waveform screenshot** here.

## ✅ Result

The 8-bit Ripple Carry Adder successfully performs binary addition, and the simulation confirms correct sum and carry generation for the tested input combinations.

---

# 📊 Overall Results

The experiments in Module 5 were successfully simulated and synthesized using **Verilog HDL, Icarus Verilog, GTKWave, and Yosys**.

The experiments demonstrated the following:

* ✅ Studied incomplete IF statements.
* ✅ Observed latch inference caused by incomplete assignments.
* ✅ Generated RTL schematics using Yosys.
* ✅ Studied incomplete and complete case statements.
* ✅ Analyzed partial case assignments.
* ✅ Verified multiplexer functionality.
* ✅ Verified demultiplexer functionality.
* ✅ Implemented and verified an 8-bit Ripple Carry Adder.
* ✅ Observed how RTL coding style affects synthesized hardware.
* ✅ Understood the importance of complete combinational assignments.
* ✅ Studied synthesis optimization and hardware simplification.

The experiments demonstrated that proper RTL coding allows synthesis tools to generate efficient and predictable hardware while avoiding unintended storage elements.

---

# 📝 Conclusion

Module 5 provided practical experience with **synthesis optimization using Verilog HDL and Yosys**.

The experiments demonstrated how incomplete conditional statements and case assignments can lead to **latch inference**, while complete assignments allow synthesis tools to generate purely combinational logic.

Multiplexer, demultiplexer, and Ripple Carry Adder experiments further strengthened the understanding of combinational circuit implementation and functional verification.

The overall RTL-to-hardware flow was verified using:

```text
Verilog RTL
     ↓
Icarus Verilog
     ↓
RTL Simulation
     ↓
GTKWave
     ↓
Yosys Synthesis
     ↓
Logic Optimization
     ↓
Technology Mapping
     ↓
Optimized Hardware
```

Overall, this module strengthened the understanding of **RTL coding practices, synthesis optimization, latch inference, combinational logic design, and efficient hardware implementation**.
