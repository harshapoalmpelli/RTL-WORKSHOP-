# Module 4 – Blocking vs Non-Blocking Assignments and Synthesis-Simulation Mismatch

## 🎯 Objectives

The main objectives of this module are:

* To understand **blocking (`=`)** and **non-blocking (`<=`)** assignments in Verilog HDL.
* To study the execution behavior of blocking assignments.
* To understand proper RTL coding practices.
* To implement and simulate a **2×1 multiplexer**.
* To analyze an incorrectly coded multiplexer.
* To understand the effect of incomplete assignments in combinational logic.
* To study possible **synthesis-simulation mismatch** conditions.
* To verify RTL functionality using **Icarus Verilog** and **GTKWave**.
* To synthesize RTL designs using **Yosys**.
* To perform technology mapping using the **SKY130 standard-cell library**.
* To analyze the relationship between RTL simulation and synthesized hardware.

---

## 🛠️ Tools and Technologies

| Tool / Technology  | Application                |
| ------------------ | -------------------------- |
| **Verilog HDL**    | RTL Design                 |
| **Icarus Verilog** | Compilation and Simulation |
| **GTKWave**        | Waveform Analysis          |
| **Yosys**          | RTL Synthesis              |
| **SKY130**         | Standard-Cell Technology   |
| **Linux / Ubuntu** | Development Environment    |
| **gVim**           | Verilog File Editing       |

---

## 📚 Table of Contents

1. [RTL Simulation of a 2×1 Multiplexer](#1--rtl-simulation-of-a-2×1-multiplexer)
2. [Technology Mapping of the Multiplexer](#2--technology-mapping-of-the-multiplexer)
3. [Functional Verification Using Waveforms](#3--functional-verification-using-waveforms)
4. [Analysis of an Incorrect Multiplexer](#4-️-analysis-of-an-incorrect-multiplexer)
5. [Verification of Incorrect Multiplexer Behavior](#5--verification-of-incorrect-multiplexer-behavior)
6. [Blocking Assignment Simulation](#6--blocking-assignment-simulation)
7. [Synthesis of the Blocking Assignment Circuit](#7-️-synthesis-of-the-blocking-assignment-circuit)
8. [Blocking Assignment and Previous Values](#8--blocking-assignment-and-previous-values)
9. [Overall Result](#-overall-result)
10. [Conclusion](#-conclusion)

---

# 1. 🔀 RTL Simulation of a 2×1 Multiplexer

## 📖 Introduction

A **2×1 multiplexer (MUX)** was designed using Verilog HDL to demonstrate basic combinational RTL design.

A multiplexer selects one of two input signals based on the value of the select signal.

### Truth Table

| Select (`s`) | Selected Input | Output (`y`) |
| ------------ | -------------- | ------------ |
| `0`          | `i0`           | `i0`         |
| `1`          | `i1`           | `i1`         |

The RTL design was compiled and simulated using **Icarus Verilog**. The resulting waveform was analyzed using **GTKWave**.

## ⚙️ Simulation Commands

```bash
iverilog -o mux ternary_operator_mux.v tb_ternary_operator_mux.v
./mux
gtkwave ternary_operator_mux.vcd
```

## 📷 Simulation Output

> Add your **2×1 multiplexer simulation screenshot** here.

## ✅ Result

The RTL simulation successfully verified that the multiplexer output follows the selected input according to the select signal.

---

# 2. ⚙️ Technology Mapping of the Multiplexer

## 📖 Introduction

After verifying the functionality of the multiplexer through RTL simulation, the design was synthesized using **Yosys**.

Synthesis converts the Verilog RTL description into a gate-level representation. The synthesized logic can then be optimized and mapped to standard cells available in the **SKY130 technology library**.

## ⚙️ Yosys Commands

```bash
yosys
```

```yosys
read_verilog mux_generate.v
synth -top mux_generate
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

## 🔄 Synthesis Flow

```text
Verilog RTL
     ↓
   Yosys
     ↓
 RTL Synthesis
     ↓
Logic Optimization
     ↓
Technology Mapping
     ↓
SKY130 Standard Cells
     ↓
Gate-Level Circuit
```

## 📷 Synthesized Circuit

> Add your **Yosys synthesized schematic screenshot** here.

## ✅ Result

The multiplexer RTL was successfully synthesized and technology-mapped using the **SKY130 standard-cell library**.

---

# 3. 🧪 Functional Verification Using Waveforms

## 📖 Introduction

The functionality of the 2×1 multiplexer was verified by applying different combinations of input and select signals.

The generated waveform was analyzed using **GTKWave** to confirm that the output follows the selected input during simulation.

## 🔍 Signals Observed

The following signals were observed:

* `i0` – First input
* `i1` – Second input
* `s` – Select signal
* `y` – Output

## ⚙️ Simulation Commands

```bash
iverilog -o mux mux_generate.v tb_mux_generate.v
./mux
gtkwave mux_generate.vcd
```

## 📷 Output Waveform

> Add your **GTKWave waveform screenshot** here.

## ✅ Result

The waveform confirmed the expected operation of the 2×1 multiplexer for the applied input combinations.

---

# 4. ⚠️ Analysis of an Incorrect Multiplexer

## 📖 Introduction

An incorrectly coded multiplexer was studied to understand the effect of **incomplete assignments in combinational RTL**.

In a combinational circuit, the output should be assigned for every possible input condition. If an output is not assigned in some conditions, the synthesized hardware may infer a **latch** to preserve the previous value.

This experiment demonstrates the importance of writing complete and consistent RTL descriptions.

## ⚙️ Simulation Commands

```bash
iverilog -o bad_mux bad_mux.v tb_bad_mux.v
./bad_mux
gtkwave bad_mux.vcd
```

## 📷 Output

> Add your **incorrect multiplexer simulation screenshot** here.

## 🔍 Observation

The incorrectly coded multiplexer produced unexpected output behavior during simulation because the output was not assigned for every possible condition.

## ✅ Result

The experiment demonstrated how incomplete assignments in combinational RTL can lead to unintended storage behavior and possible latch inference during synthesis.

---

# 5. 🔍 Verification of Incorrect Multiplexer Behavior

## 📖 Introduction

The waveform generated from the incorrect multiplexer was analyzed to understand the effect of incomplete assignments.

When a combinational output is not assigned under every possible condition, the output may retain its previous value.

For example:

```text
Input Condition
      ↓
Output Assigned?
   ↙       ↘
 YES       NO
  ↓         ↓
New Value   Previous Value Retained
```

During synthesis, this behavior can cause the synthesis tool to infer a **latch**.

## ⚙️ Simulation Commands

```bash
iverilog -o bad_mux bad_mux.v tb_bad_mux.v
./bad_mux
gtkwave bad_mux.vcd
```

## 📷 Output Waveform

> Add your **GTKWave waveform screenshot** here.

## 🔍 Observation

The waveform showed that the output could retain its previous value when no new assignment was made.

## ✅ Result

The experiment demonstrated the importance of complete assignments in combinational RTL and showed how improper coding can result in unintended storage behavior.

---

# 6. 🔄 Blocking Assignment Simulation

## 📖 Introduction

A **blocking assignment** uses the `=` operator in Verilog HDL.

A blocking assignment executes immediately. Therefore, when a variable is updated, the next statement in the same procedural block can use the updated value.

### Example

```verilog
a = b;
c = a;
```

In this example, the assignment to `a` is completed before the next statement executes. Therefore, `c` can use the newly assigned value of `a`.

Blocking assignments are generally used for describing **combinational logic**.

However, using blocking assignments in sequential logic can cause simulation behavior that depends on statement ordering. For sequential logic, **non-blocking assignments (`<=`)** are generally preferred.

## ⚙️ Simulation Commands

```bash
iverilog -o blocking blocking_caveat.v tb_blocking_caveat.v
./blocking
gtkwave blocking_caveat.vcd
```

## 📷 Output Waveform

> Add your **blocking-assignment waveform screenshot** here.

## 🔍 Observation

The waveform demonstrated that blocking assignments update variables immediately within the procedural block.

## ✅ Result

The simulation successfully demonstrated the immediate execution behavior of blocking assignments.

---

# 7. 🏗️ Synthesis of the Blocking Assignment Circuit

## 📖 Introduction

The blocking-assignment RTL was synthesized using **Yosys** to observe how the Verilog description is converted into hardware.

The synthesized circuit was then mapped to the **SKY130 standard-cell library**.

## ⚙️ Synthesis Commands

```bash
yosys
```

```yosys
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

## 🔄 Synthesis Process

```text
RTL Description
      ↓
    Yosys
      ↓
 RTL Processing
      ↓
 Optimization
      ↓
Technology Mapping
      ↓
 SKY130 Cells
      ↓
Gate-Level Circuit
```

## 📷 Technology-Mapped Circuit

> Add your **Yosys technology-mapped circuit screenshot** here.

## ✅ Result

The blocking-assignment circuit was successfully synthesized and mapped to the target **SKY130 standard-cell technology**.

---

# 8. 🔄 Blocking Assignment and Previous Values

## 📖 Introduction

This experiment further examined how blocking assignments interact with previously assigned values.

Since blocking assignments update variables immediately, the **order of statements** inside a procedural block can affect the values used by subsequent statements.

This behavior is particularly important when writing sequential RTL.

For sequential circuits, non-blocking assignments are generally preferred because they model simultaneous register updates more appropriately.

### Blocking Assignment

```verilog
a = b;
c = a;
```

The second statement can immediately use the updated value of `a`.

### Non-Blocking Assignment

```verilog
a <= b;
c <= a;
```

The updates are scheduled so that both right-hand-side values are evaluated before the assignments take effect.

## ⚙️ Simulation Commands

```bash
iverilog -o blocking_past blocking_caveat.v tb_blocking_caveat.v
./blocking_past
gtkwave blocking_past.vcd
```

## 📷 Output Waveform

> Add your **blocking-assignment waveform screenshot** here.

## 🔍 Observation

The waveform demonstrated the immediate-update behavior of blocking assignments and showed how statement ordering can influence simulation values.

## ✅ Result

The experiment emphasized the importance of selecting the appropriate assignment type based on the RTL logic being designed.

---

# 🎯 Overall Result

The experiments in **Module 4** were successfully performed using:

* **Verilog HDL**
* **Icarus Verilog**
* **GTKWave**
* **Yosys**
* **SKY130 Standard-Cell Library**
* **Linux / Ubuntu**

The following concepts were successfully studied and verified:

* ✅ Implemented a 2×1 multiplexer using Verilog HDL.
* ✅ Verified multiplexer functionality through RTL simulation.
* ✅ Analyzed simulation waveforms using GTKWave.
* ✅ Synthesized the multiplexer using Yosys.
* ✅ Performed technology mapping using the SKY130 library.
* ✅ Studied an incorrectly coded multiplexer.
* ✅ Investigated the effect of incomplete assignments.
* ✅ Observed possible latch inference in combinational RTL.
* ✅ Studied the immediate execution behavior of blocking assignments.
* ✅ Analyzed the effect of statement ordering.
* ✅ Compared RTL simulation behavior with synthesized hardware.
* ✅ Understood the importance of proper RTL coding practices.

---

# 📝 Conclusion

Module 4 provided practical understanding of **blocking and non-blocking assignments, combinational RTL design, multiplexer implementation, waveform analysis, synthesis, and technology mapping**.

The multiplexer experiments demonstrated how combinational RTL can be designed, simulated, verified, and synthesized. The incorrect multiplexer experiment showed how incomplete assignments can result in unintended storage behavior and possible latch inference.

The blocking-assignment experiments demonstrated how immediate updates and statement ordering can influence simulation behavior. This highlighted the importance of choosing the correct assignment type when designing combinational and sequential RTL.

The complete RTL-to-hardware flow was demonstrated using:

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
SKY130 Technology Mapping
     ↓
Gate-Level Hardware
```

Overall, this module strengthened the understanding of writing **correct, predictable, synthesizable, and hardware-oriented Verilog RTL**.
