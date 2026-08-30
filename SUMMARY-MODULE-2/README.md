# 📘 Module 2 – Summary

## 🔎 Overview

Module 2 focused on the next stage of the **RTL-to-netlist synthesis flow**, covering **timing libraries, PVT conditions, hierarchical and flat synthesis, flip-flop coding styles, reset techniques, and technology mapping**.

The practical work used the **Sky130 standard-cell library** along with **Yosys** to understand how RTL descriptions are converted into technology-specific gate-level implementations.

## ⏱️ Timing Libraries

A **timing library** provides important electrical and timing information to the synthesis tool for selecting suitable standard cells.

The library contains information such as:

* Logic function
* Propagation delay
* Power consumption
* Area
* Input capacitance
* Output drive strength

Libraries are characterized for different operating conditions such as **slow, fast, and typical process corners**.

## 🏭 Sky130 Standard-Cell Library

The module used the following Sky130 timing library:

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
```

This represents the **Sky130 130 nm CMOS technology** with the **High Density standard-cell library**.

The library is characterized at:

```text
tt_025C_1v80
```

which represents:

* **Typical process**
* **25°C temperature**
* **1.80 V supply voltage**

## 🌡️ PVT – Process, Voltage, Temperature

**PVT** represents the three major conditions that affect circuit behavior:

* **Process** – Manufacturing variations affect circuit characteristics.
* **Voltage** – Supply voltage affects delay, power, and switching behavior.
* **Temperature** – Temperature affects propagation delay, leakage, and power consumption.

PVT analysis helps ensure reliable circuit operation under different conditions.

## 🏗️ Hierarchical and Flat Synthesis

**Hierarchical synthesis** keeps the design divided into separate modules. This provides better organization, debugging, reuse, and scalability.

**Flat synthesis** removes module boundaries using the `flatten` command, allowing Yosys to optimize the complete design together.

```text
Hierarchical Synthesis
        ↓
Modules remain separate
        ↓
Clear design boundaries


Flat Synthesis
        ↓
Hierarchy removed
        ↓
Whole design optimized together
```

## 🔢 Flip-Flop and Reset Styles

The module introduced **flip-flops** as sequential elements used to store data between clock cycles.

Two reset styles were studied:

* **Synchronous Reset** – Reset is checked only at the active clock edge.
* **Asynchronous Reset** – Reset operates independently of the clock and can change the output immediately.

The module also covered **flip-flop initialization**, highlighting that a flip-flop can have an unknown (`X`) state during simulation if it is not properly initialized or reset.

## ⚙️ Yosys Synthesis Flow

The main Yosys synthesis flow covered in the module was:

```text
RTL Verilog
     ↓
read_verilog
     ↓
hierarchy
     ↓
flatten
     ↓
dfflibmap
     ↓
abc
     ↓
write_verilog
```

Important commands include:

* `read_verilog` – Reads the RTL design.
* `hierarchy` – Sets and checks the design hierarchy.
* `flatten` – Removes module hierarchy.
* `dfflibmap` – Maps flip-flops to library cells.
* `abc` – Optimizes and maps combinational logic.
* `write_verilog` – Generates the synthesized netlist.

## 🎯 Key Learnings

* Understood the importance of **timing libraries** in synthesis.
* Learned about **PVT conditions**.
* Studied the **Sky130 standard-cell library**.
* Understood **hierarchical and flat synthesis**.
* Learned about **submodule instantiation**.
* Studied **flip-flop fundamentals**.
* Understood **synchronous and asynchronous resets**.
* Learned about **flip-flop initialization**.
* Understood the Yosys synthesis flow.
* Learned the roles of **`dfflibmap` and `abc`**.
* Understood **technology mapping** and netlist generation.

## ✅ Conclusion

Module 2 provided a deeper understanding of the **RTL-to-netlist synthesis process** by introducing timing libraries, PVT conditions, synthesis hierarchy, flip-flop coding styles, reset techniques, and technology mapping.

Using **Yosys** and the **Sky130 standard-cell library**, the RTL design was processed through synthesis, hierarchy handling, flip-flop mapping, combinational optimization, and technology mapping to generate a **technology-specific netlist**.

Overall, the module demonstrated how **RTL coding choices and synthesis processes influence the final hardware implementation**. 
