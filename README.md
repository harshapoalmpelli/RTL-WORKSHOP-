# Sequence Detector 

## Introduction

This repository contains the work completed for Assessment 1 on the design, simulation, synthesis, and verification of a Sequence Detector using Verilog HDL.

The project covers RTL design, functional simulation, waveform analysis using GTKWave, logic synthesis using Yosys, Gate-Level Simulation (GLS), and comparison of RTL and GLS results.

## Objectives

* Design a sequence detector using Verilog HDL.
* Understand and implement a Finite State Machine (FSM).
* Perform RTL simulation using Icarus Verilog.
* Generate and analyze VCD waveforms using GTKWave.
* Synthesize the RTL design using Yosys.
* Inspect the synthesized netlist and synthesis statistics.
* Perform Gate-Level Simulation using the synthesized design.
* Compare RTL and GLS simulation results.

## Tools and Technologies

* Verilog HDL
* Icarus Verilog
* GTKWave
* Yosys
* Linux / VSDSquadron Virtual Machine
* Git and GitHub

## Table of Contents

1. [Sequence Detector Design](#1-sequence-detector-design)
2. [RTL Simulation](#2-rtl-simulation)
3. [Waveform Analysis](#3-waveform-analysis)
4. [RTL Synthesis](#4-rtl-synthesis)
5. [Gate-Level Simulation](#5-gate-level-simulation)
6. [RTL vs GLS Comparison](#6-rtl-vs-gls-comparison)
7. [Overall Result](#7-overall-result)
8. [Conclusion](#8-conclusion)

## 1. Sequence Detector Design

The sequence detector was designed using Verilog HDL based on a Finite State Machine (FSM) approach. An FSM uses states and transitions to track the incoming input sequence and generate the detection output.

### Design Files

* `sequence_detector.v` – RTL implementation of the sequence detector.
* `tb.v` – Testbench used to verify the design.
* `synthesized.v` – Synthesized gate-level netlist generated using Yosys.

## 2. RTL Simulation

The RTL design was simulated using Icarus Verilog to verify its functional behavior.

A testbench was used to apply input sequences and observe the `detected` output. The simulation helped verify the operation of the FSM and its response to the applied inputs.

## 3. Waveform Analysis

The simulation generated a Value Change Dump (VCD) file containing signal transitions over time. The VCD file was opened in GTKWave to inspect the clock, reset, input, and detection signals.

### RTL Waveform

![RTL Simulation Waveform](https://private-user-images.githubusercontent.com/256913859/643570813-56aa1233-1d42-4a3a-8d75-d1b79f2524b6.png)

The waveform was used to examine the input sequence and verify the corresponding output behavior.

## 4. RTL Synthesis

The RTL design was synthesized using Yosys to convert the Verilog description into a gate-level representation.

Synthesis performs logic optimization and generates a netlist representing the hardware implementation.

### Synthesis Statistics

![Synthesis Statistics](https://private-user-images.githubusercontent.com/256913859/643571379-44672b7b-9f76-48bb-ac29-4ce177b08794.png)

The synthesis statistics provide information about the hardware elements inferred by the tool.

### Synthesized Netlist

![Synthesized Netlist](https://private-user-images.githubusercontent.com/256913859/643571672-59fbfd01-a8d2-4953-b0cd-df0e86b893e4.png)

The synthesized netlist represents the circuit after synthesis and logic optimization.

## 5. Gate-Level Simulation

Gate-Level Simulation (GLS) was performed using the synthesized netlist to examine the behavior of the implemented logic.

The generated simulation waveform was opened in GTKWave to observe the signals and verify the sequence detector's operation after synthesis.

### GLS Waveform

![Gate-Level Simulation Waveform](https://private-user-images.githubusercontent.com/256913859/643571885-75d7d6d1-6825-459f-9f86-28b2284f20bc.png)

## 6. RTL vs GLS Comparison

The RTL and GLS waveforms were compared to evaluate whether the synthesized design preserved the expected functional behavior.

![RTL vs GLS Comparison](https://private-user-images.githubusercontent.com/256913859/643572975-ab30a174-6080-4512-b039-d2da8322c9b5.png)

Comparing the two simulations helps identify differences in signal behavior. When equivalent test inputs are applied, the detection output should follow the intended design behavior, accounting for any relevant simulation timing differences.

## 7. Overall Result

The Sequence Detector design was developed using Verilog HDL and evaluated through RTL simulation, synthesis, and Gate-Level Simulation.

The work provided practical experience in:

* FSM-based digital design.
* RTL simulation and waveform analysis.
* Logic synthesis using Yosys.
* Synthesized netlist inspection.
* Gate-Level Simulation.
* Comparison of RTL and GLS behavior.

## 8. Conclusion

Assessment 1 provided practical experience with the RTL-to-GLS design flow for a Verilog-based Sequence Detector. The project demonstrated the process of designing a digital circuit, verifying its functional behavior, synthesizing the RTL, and examining the synthesized implementation through Gate-Level Simulation.

This work strengthened my understanding of digital design, simulation, synthesis, and hardware verification using open-source EDA tools.
