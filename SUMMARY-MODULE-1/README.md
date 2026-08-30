# 📘 Module 1 – Summary

## 🔎 Overview

Module 1 focused on understanding the **basic RTL design and synthesis flow** used in VLSI digital design. A **2:1 Multiplexer (MUX)** was used as a practical example to understand how a digital circuit is designed using Verilog, verified through simulation, synthesized, and converted into a gate-level netlist.

## 🧩 MUX Design

A **2:1 Multiplexer** consists of two data inputs (`i0` and `i1`), one select input (`sel`), and one output (`y`).

The select signal determines which input is connected to the output:

```text
sel = 0  →  y = i0
sel = 1  →  y = i1
```

The MUX was implemented using **Verilog RTL** with an `if-else` statement.

## 💻 RTL Design and Testbench

The RTL design was written in the `good_mux.v` file. A separate testbench, `tb_good_mux.v`, was created to apply different combinations of inputs and verify the functionality of the MUX.

The testbench generated a **VCD (Value Change Dump)** file containing the signal transitions during simulation.

## 🧪 RTL Simulation

The Verilog RTL and testbench were simulated using **Icarus Verilog**. The generated VCD file was opened in **GTKWave** to analyze the waveforms.

The waveform verification confirmed that:

* When `sel = 0`, output `y` follows `i0`.
* When `sel = 1`, output `y` follows `i1`.

This verified that the RTL design behaves as expected.

## ⚙️ Synthesis Using Yosys

After successful RTL simulation, the design was synthesized using **Yosys**, an open-source RTL synthesis tool.

Yosys converted the Verilog RTL description into a **gate-level netlist**. The synthesized netlist was saved as:

```text
good_mux_netlist.v
```

This demonstrated how RTL code is transformed into a hardware representation during synthesis.

## 🔬 Netlist Verification

The synthesized netlist was simulated again using the testbench and **Icarus Verilog**.

The resulting waveform was analyzed using GTKWave to verify that the synthesized circuit produced the same functionality as the original RTL design.

This step demonstrated the importance of **post-synthesis verification**.

## 🔄 Complete Design Flow

```text
Verilog RTL
     ↓
Testbench
     ↓
Icarus Verilog Simulation
     ↓
VCD File
     ↓
GTKWave Waveform Analysis
     ↓
Yosys Synthesis
     ↓
Gate-Level Netlist
     ↓
Netlist Verification
```

## 🎯 Key Learnings

* Understood the fundamentals of **RTL design and synthesis**.
* Implemented a **2:1 MUX using Verilog**.
* Learned how to create a **Verilog testbench**.
* Performed functional simulation using **Icarus Verilog**.
* Generated and analyzed **VCD waveforms using GTKWave**.
* Understood the basic concept of **RTL synthesis**.
* Used **Yosys** to generate a gate-level netlist.
* Learned how to perform **post-synthesis verification**.
* Understood the relationship between **RTL, simulation, synthesis, netlist, and verification**.

## ✅ Conclusion

Module 1 provided a practical introduction to the **RTL-to-netlist flow** using a 2:1 MUX. The complete process covered **Verilog RTL design, testbench development, simulation, waveform analysis, synthesis using Yosys, netlist generation, and netlist verification**.

The hands-on implementation helped in understanding how a **Verilog RTL description is converted into a synthesized hardware implementation** while ensuring that the final circuit maintains the intended functionality.
