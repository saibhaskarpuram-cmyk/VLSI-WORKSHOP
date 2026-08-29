# Module 4 — Gate-Level Simulation, Blocking vs Non-Blocking Assignments & Synthesis-Simulation Mismatch

## 📌 Overview

This module explains **Gate-Level Simulation (GLS)**, the proper use of **blocking (`=`)** and **non-blocking (`<=`) assignments**, and common reasons for **synthesis-simulation mismatch** in Verilog designs.

The practical exercises include RTL simulation, synthesis with **Yosys**, creation of gate-level netlists, GLS using **Icarus Verilog**, and waveform inspection through **GTKWave**. The experiments also show how incomplete sensitivity lists and unsuitable assignment styles can make simulation results differ from the expected hardware behavior.

---

## 1. Gate-Level Simulation (GLS)

**Gate-Level Simulation (GLS)** is the process of verifying a synthesized gate-level netlist with a testbench rather than directly simulating the original RTL.

### Basic Flow

```text
RTL Design + Testbench
          ↓
    RTL Simulation
          ↓
       Synthesis
          ↓
   Gate-Level Netlist
          ↓
 Gate-Level Simulation
```

The same testbench can be reused to compare RTL behavior with the synthesized gate-level implementation.

### Why GLS?

GLS is useful to:

* Check the functionality of the synthesized circuit.
* Ensure synthesis has preserved the intended behavior.
* Detect synthesis-simulation mismatches.
* Validate the gate-level implementation.
* Perform timing verification when delay information is available.

> **Note:** Timing-aware GLS needs delay-annotated gate-level models.

---

## 2. RTL vs Gate-Level Netlist

### RTL Description

RTL represents the intended hardware behavior using constructs such as:

* `always`
* `assign`
* `if-else`
* `case`
* Logical operators
* Arithmetic operators

Example:

```verilog
assign y = (a & b) | c;
```

### Gate-Level Netlist

After synthesis, RTL is converted into logic gates or technology-specific standard cells.

```text
a ───┐
     AND ───┐
b ───┘      │
            OR ─── y
c ──────────┘
```

The generated netlist describes the implementation using gates or cells instead of high-level RTL constructs.

---

## 3. GLS Using Icarus Verilog

A typical GLS flow using **Icarus Verilog** is:

```text
              RTL Design
                  ↓
               Yosys
                  ↓
         Gate-Level Netlist
                  ↓
             Icarus Verilog
                  ↑
              Testbench
                  ↓
                 VCD
                  ↓
              GTKWave
```

### Simulation Flow

```text
Gate-Level Netlist
        +
    Testbench
        ↓
     IVerilog
        ↓
       VCD
        ↓
    GTKWave
```

The resulting waveform can be compared against the waveform produced by RTL simulation.

---

## 4. Example: 2:1 MUX

A simple combinational MUX can be represented as:

```verilog
assign y = (a & b) | c;
```

The corresponding gate-level structure is:

```text
a ───┐
     AND ───┐
b ───┘      │
            OR ─── y
c ──────────┘
```

The same verification approach can be extended to more complex combinational and sequential designs.

---

## 5. Synthesis-Simulation Mismatch

A **synthesis-simulation mismatch** happens when the behavior seen during RTL simulation is different from the behavior of the synthesized hardware.

```text
RTL Simulation
      ≠
Gate-Level Simulation
```

Typical causes include:

* Incomplete sensitivity lists.
* Improper use of blocking assignments.
* Improper use of non-blocking assignments.
* Incorrect sequential-logic coding.
* RTL constructs that fail to represent the intended hardware accurately.

---

## 6. Incomplete Sensitivity Lists

Consider the following MUX:

```verilog
module mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(sel)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end

endmodule
```

The sensitivity list contains only:

```verilog
@(sel)
```

However, the output depends on:

```text
sel
i0
i1
```

Therefore, changes to `i0` or `i1` may not activate the `always` block.

### Example

Suppose:

```text
sel = 0
i0  = 0
i1  = 1
```

Then:

```text
y = i0 = 0
```

If `i0` changes from `0` to `1` while `sel` remains `0`, the block may not run again, so `y` can incorrectly remain `0` in simulation.

### Correct Approach

Use:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

`@(*)` automatically includes the signals used by the combinational block.

---

## 7. `always @(sel)` vs `always @(*)`

| Feature                     | `always @(sel)` | `always @(*)` |
| --------------------------- | --------------- | ------------- |
| Executes when `sel` changes | Yes             | Yes           |
| Executes when `i0` changes  | No              | Yes           |
| Executes when `i1` changes  | No              | Yes           |
| Suitable for this MUX       | No              | Yes           |
| Simulation mismatch risk    | Higher          | Lower         |

### Rule

For combinational procedural logic:

```verilog
always @(*)
```

is preferred instead of an incomplete sensitivity list.

---

## 8. Blocking Assignments

The blocking assignment operator is:

```verilog
=
```

Example:

```verilog
q0 = d;
q  = q0;
```

Blocking assignments execute immediately in procedural order.

Conceptually:

```text
Statement 1
    ↓
Statement 2
    ↓
Statement 3
```

Each following statement can observe the value assigned by the previous statement.

### Typical Use

Blocking assignments are generally used for:

* Combinational logic.
* Intermediate calculations in combinational blocks.
* Procedural combinational descriptions.

---

## 9. Non-Blocking Assignments

The non-blocking assignment operator is:

```verilog
<=
```

Example:

```verilog
q0 <= d;
q  <= q0;
```

With non-blocking assignments:

1. Right-hand-side values are evaluated.
2. Left-hand-side updates are scheduled.
3. The scheduled updates occur after the procedural evaluation.
4. Multiple registers can represent parallel clocked behavior.

Conceptually:

```text
Evaluate RHS values
        ↓
Schedule updates
        ↓
Update registers
```

This makes non-blocking assignments appropriate for sequential logic.

---

## 10. Blocking vs Non-Blocking

| Blocking `=`                            | Non-Blocking `<=`                                                |
| --------------------------------------- | ---------------------------------------------------------------- |
| Executes immediately                    | Update is scheduled                                              |
| Procedural order affects the result     | Represents parallel register updates                             |
| Later statements can see updated values | Later statements see previous values during the same clock event |
| Commonly used for combinational logic   | Commonly used for sequential logic                               |
| Can introduce issues in clocked logic   | Preferred for flip-flop/register modeling                        |

### General Rule

```text
Combinational Logic → Blocking `=`

Sequential Logic → Non-Blocking `<=`
```

---

## 11. Blocking Assignment in Sequential Logic

Consider two flip-flops connected in sequence:

```text
d ───► FF1 ───► FF2 ───► q
       q0
```

The intended behavior is:

```text
q0 ← d
q  ← old q0
```

A problematic implementation is:

```verilog
always @(posedge clk)
begin
    q0 = d;
    q  = q0;
end
```

Since blocking assignments execute immediately:

```text
q0 = d
   ↓
q = q0
```

The second statement observes the newly assigned value of `q0`.

This can make RTL simulation appear as if data passes through both registers during one clock event.

---

## 12. Correct Sequential Coding

The preferred implementation is:

```verilog
always @(posedge clk)
begin
    q0 <= d;
    q  <= q0;
end
```

Both RHS expressions are evaluated before the register values are updated.

```text
Clock 1:
q0 gets d
q gets old q0

Clock 2:
q0 gets new d
q gets previous q0
```

This correctly models two cascaded flip-flops.

### Recommended Style

```text
Flip-Flops
Registers
Counters
State Machines
Clocked Logic
        ↓
Use <=
```

---

## 13. Why Blocking Assignments Can Cause Mismatch

Consider:

```verilog
always @(posedge clk)
begin
    q0 = d;
    q  = q0;
end
```

### RTL Simulation

The simulator follows the procedural sequence:

```text
q0 = d
 ↓
q = q0
```

Therefore, `q` can observe the newly assigned value of `q0`.

### Synthesized Hardware

Synthesis interprets the clocked logic as sequential hardware and may infer:

```text
d → FF(q0) → FF(q)
```

The two flip-flops operate on successive clock events.

Thus, the simulation result can differ from the intended hardware behavior.

> This is why non-blocking assignments are the recommended style for sequential logic.

---

## 14. Order Dependence of Blocking Assignments

Consider:

```verilog
always @(posedge clk)
begin
    q0 = a | b;
    y  = q0 & c;
end
```

The simulator executes:

```text
1. q0 = a | b
2. y  = q0 & c
```

So `y` uses the newly calculated value of `q0`.

If the statements are reversed:

```verilog
always @(posedge clk)
begin
    y  = q0 & c;
    q0 = a | b;
end
```

`y` uses the previous value of `q0`.

Therefore:

```text
Statement Order
      ↓
Different Simulation Behavior
```

This order dependence is undesirable for sequential hardware modeling.

---

## 15. Blocking Assignments for Combinational Logic

Blocking assignments are suitable for procedural combinational descriptions.

Example:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

The behavior is:

```text
Input changes
      ↓
Combinational block executes
      ↓
Output is calculated
```

Therefore:

```text
Combinational → always @(*) + =
Sequential    → always @(posedge clk) + <=
```

---

## 16. Ternary Operator MUX

A 2:1 MUX can also be described with the ternary operator:

```verilog
assign y = sel ? i1 : i0;
```

Meaning:

```text
sel = 0 → y = i0
sel = 1 → y = i1
```

Equivalent procedural description:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

Both descriptions represent the same 2:1 MUX.

### MUX Representation

```text
        ┌─────┐
i0 ────►│     │
        │ MUX │────► y
i1 ────►│     │
        └──┬──┘
           │
          sel
```

---

## 17. Ternary MUX Implementation

Example RTL:

```verilog
module ternary_operator_mux (
    input  i0,
    input  i1,
    input  sel,
    output y
);

assign y = sel ? i1 : i0;

endmodule
```

### Expected Behavior

```text
sel = 0 → y = i0
sel = 1 → y = i1
```

A testbench can apply several combinations of `i0`, `i1`, and `sel` to verify the output.

---

## 18. RTL Simulation Using Icarus Verilog

The basic simulation sequence is:

```text
Verilog Design
      +
Testbench
      ↓
Icarus Verilog
      ↓
Simulation / VCD
      ↓
GTKWave
```

### Compile

```bash
iverilog -o a.out ternary_operator_mux.v tb_ternary_operator_mux.v
```

### Run

```bash
./a.out
```

### View Waveform

```bash
gtkwave tb_ternary_operator_mux.vcd
```

The waveform can be checked to confirm that the MUX output follows the selected input.

---

## 19. Yosys Synthesis Flow

**Yosys** can synthesize the RTL and produce a gate-level netlist.

Typical commands are:

```text
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty <library>.lib
write_verilog -noattr ternary_operator_mux.net.v
show
```

### Synthesis Flow

```text
Read RTL
   ↓
Synthesis
   ↓
Technology Mapping
   ↓
Gate-Level Netlist
   ↓
View Synthesized Circuit
```

---

## 20. Gate-Level Netlist and GLS

After synthesis, a gate-level netlist such as:

```text
ternary_operator_mux.net.v
```

can be simulated using the same testbench.

### GLS Flow

```text
RTL
 ↓
Yosys
 ↓
Gate-Level Netlist
 ↓
Icarus Verilog
 ↓
VCD
 ↓
GTKWave
```

Using the same testbench simplifies comparison between RTL and synthesized behavior.

---

## 21. GLS with Gate-Level Libraries

When the synthesized netlist contains standard-cell instances, the matching Verilog library models must also be included during simulation.

A typical setup may contain:

```text
my_lib/
└── verilog_model/
    ├── primitives.v
    └── sky130_fd_sc_hd.v

gate-level-netlist.v
testbench.v
```

Representative compilation:

```bash
iverilog \
  my_lib/verilog_model/primitives.v \
  my_lib/verilog_model/sky130_fd_sc_hd.v \
  gate-level-netlist.v \
  testbench.v
```

Run the simulation:

```bash
./a.out
```

View the waveform:

```bash
gtkwave testbench.vcd
```

---

## 22. Blocking-Statement GLS Experiment

This experiment demonstrates how an unsuitable blocking-assignment style in sequential logic can contribute to synthesis-simulation differences.

### RTL Simulation

```bash
iverilog blocking_caveat.v tb_blocking_caveat.v
```

Run:

```bash
./a.out
```

View:

```bash
gtkwave tb_blocking_caveat.vcd
```

### Synthesis

Using Yosys:

```text
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty <library>.lib
write_verilog -noattr blocking_caveat.net.v
show
```

### Gate-Level Simulation

```bash
iverilog \
  my_lib/verilog_model/primitives.v \
  my_lib/verilog_model/sky130_fd_sc_hd.v \
  blocking_caveat.net.v \
  tb_blocking_caveat.v
```

Run:

```bash
./a.out
```

View:

```bash
gtkwave tb_blocking_caveat.vcd
```

The RTL and GLS waveforms can then be compared to identify behavioral differences.

---

## 23. RTL Simulation vs Gate-Level Simulation

| RTL Simulation                               | Gate-Level Simulation                               |
| -------------------------------------------- | --------------------------------------------------- |
| Uses RTL source code                         | Uses synthesized gate-level netlist                 |
| High-level hardware description              | Gate/cell-level implementation                      |
| Usually faster                               | Usually slower                                      |
| Checks intended RTL functionality            | Checks the synthesized implementation               |
| Normally excludes physical gate delays      | Can include delays when information is available    |
| Performed before synthesis                   | Performed after synthesis                           |

### Simple Comparison

```text
RTL Simulation
      ↓
Check RTL Functionality

Gate-Level Simulation
      ↓
Check Synthesized Implementation
```

---

## 24. Functional GLS vs Timing GLS

### Functional GLS

Functional GLS checks the logical behavior of the synthesized netlist.

```text
Gate-Level Netlist
       +
Testbench
       ↓
Functional GLS
```

### Timing GLS

Timing GLS incorporates delay information along with the gate-level design.

```text
Gate-Level Netlist
       +
Delay Information
       +
Testbench
       ↓
Timing GLS
```

Timing GLS is useful when propagation delays need to be considered during verification.

---

## 25. Important Commands

### Open Verilog Files

```bash
gvim <design>.v
```

### Compile with Icarus Verilog

```bash
iverilog -o a.out <design>.v <testbench>.v
```

### Run Simulation

```bash
./a.out
```

### View Waveform

```bash
gtkwave <waveform>.vcd
```

### Yosys Synthesis

```text
read_verilog <design>.v
synth -top <top_module>
abc -liberty <library>.lib
write_verilog -noattr <netlist>.v
show
```

---

## 26. Complete Practical Flow

The complete workflow used in this module is:

```text
                    RTL Design
                        │
                        ↓
                 RTL Simulation
                        │
                        ↓
                    Synthesis
                        │
                        ↓
              Gate-Level Netlist
                        │
              ┌─────────┴─────────┐
              ↓                   ↓
       Functional GLS        Timing GLS
              │                   │
              ↓                   ↓
             VCD                 VCD
              │                   │
              └─────────┬─────────┘
                        ↓
                    GTKWave
```

### Step-by-Step

```text
1. Write RTL
       ↓
2. Create Testbench
       ↓
3. Run RTL Simulation
       ↓
4. Check Waveform
       ↓
5. Synthesize Using Yosys
       ↓
6. Generate Gate-Level Netlist
       ↓
7. Include Required Gate/Cell Libraries
       ↓
8. Run GLS With the Same Testbench
       ↓
9. Generate VCD
       ↓
10. Inspect Waveform Using GTKWave
       ↓
11. Compare RTL and GLS Results
```

---

## 27. Key Takeaways

* **GLS** checks the behavior of a synthesized gate-level netlist.
* The **same testbench** can be reused for RTL simulation and GLS.
* **Yosys** performs RTL synthesis and generates the gate-level netlist.
* **Icarus Verilog** can simulate both RTL and gate-level Verilog.
* **GTKWave** is used to analyze simulation waveforms.
* Incomplete sensitivity lists can cause incorrect RTL simulation results.
* Use **blocking (`=`)** for combinational procedural logic.
* Use **non-blocking (`<=`)** for sequential and clocked logic.
* Timing GLS requires **delay-annotated gate-level models**.
* Comparing RTL and GLS waveforms helps identify synthesis-simulation mismatches.

---

## 🎯 Final Rule to Remember

```text
Combinational Logic
        ↓
always @(*)
        ↓
Blocking Assignment (=)

Sequential Logic
        ↓
always @(posedge clk)
        ↓
Non-Blocking Assignment (<=)

Gate-Level Verification
        ↓
Synthesized Netlist
        +
Same Testbench
        ↓
GLS

Timing Verification
        ↓
Gate-Level Netlist
        +
Delay Information
        ↓
Timing GLS
```

> **Describe the intended hardware clearly, follow the correct assignment style, and verify the synthesized design through GLS.**

## Conclusion

This module gives a practical view of how RTL designs progress from functional simulation to synthesized gate-level verification. Understanding sensitivity lists, blocking and non-blocking assignments, and GLS makes it easier to detect coding practices that may create differences between simulation and hardware behavior.

The experiments using **Icarus Verilog, GTKWave, and Yosys** demonstrate the verification flow and emphasize the importance of writing synthesis-friendly, hardware-oriented Verilog.

Overall, these practices help improve confidence in the synthesized implementation and reduce the possibility of synthesis-simulation mismatches.

