Below is a **clean GitHub-ready Markdown version** with some useful extra information, while keeping the content organized and professional.

# 📘 VLSI RTL Design and Synthesis

This repository contains my hands-on work completed during **VLSI RTL Design and Synthesis training**.

The training covers the complete **RTL-to-Gate-Level design flow**, including Verilog RTL coding, functional simulation, waveform analysis, synthesis, optimization, timing libraries, hierarchical design, sequential logic, technology mapping, Gate-Level Simulation (GLS), and synthesized hardware analysis using **open-source EDA tools** and the **SKY130 standard-cell library**.

The repository currently contains practical work from **Modules 1–5**, followed by an integrated **VSDBabySoC** project demonstrating RTL-to-post-synthesis verification.

---

# 📚 Training Modules

## Module 1 – Introduction to Verilog RTL Design and Synthesis

Module 1 introduced the fundamentals of **Verilog RTL design, simulation, waveform analysis, and synthesis**.

A **2:1 Multiplexer (MUX)** was used as the primary example to understand how RTL code is converted into a synthesized gate-level implementation.

### 🔹 Work Completed

* Introduction to RTL design using Verilog
* RTL and testbench structure
* Combinational logic implementation
* 2:1 MUX design
* Testbench development
* Functional simulation using Icarus Verilog
* VCD waveform generation
* Waveform analysis using GTKWave
* Introduction to Yosys
* RTL analysis using Yosys
* Logic synthesis
* `.lib` standard-cell libraries
* Introduction to PVT concepts
* SKY130 standard-cell library
* Technology mapping using ABC
* Synthesized netlist generation
* Gate-level netlist inspection
* RTL-to-netlist conversion

### 🔄 Basic Flow

```text
Verilog RTL
    ↓
Testbench
    ↓
Icarus Verilog
    ↓
VCD Generation
    ↓
GTKWave
    ↓
Yosys Synthesis
    ↓
ABC Technology Mapping
    ↓
SKY130 Standard Cells
    ↓
Gate-Level Netlist
```

---

# Module 2 – Timing Libraries, Hierarchical Design and Sequential Logic

Module 2 focused on how RTL descriptions are converted into **technology-dependent hardware**.

It introduced timing libraries, standard-cell information, hierarchical design, sequential logic, asynchronous set/reset behavior, and arithmetic circuits.

### 🔹 Work Completed

* Timing concepts
* Standard-cell libraries
* Liberty (`.lib`) files
* Cell timing characteristics
* Propagation delay
* Setup and hold time
* Clock-to-Q delay
* Input transition
* Output load
* RTL-to-netlist conversion
* Yosys synthesis
* Hierarchical RTL design
* Submodule creation and interconnection
* D Flip-Flop implementation
* Different Flip-Flop coding styles
* Asynchronous reset
* Asynchronous set
* Sequential circuit simulation
* Clock and reset waveform analysis
* Standard-cell Flip-Flop mapping
* Synthesized sequential hardware analysis
* Multiplier synthesis
* Technology mapping

### 🧠 Key Concept

Timing libraries provide information required by synthesis and timing-analysis tools to understand the behavior of standard cells.

Important timing parameters include:

```text
Input Transition
       ↓
   Cell Delay
       ↓
Output Transition
```

For sequential cells:

```text
Clock
  ↓
Clock-to-Q Delay
  ↓
Output

Data + Setup/Hold Requirements
```

---

# Module 3 – Combinational and Sequential Logic Optimization

Module 3 focused on how synthesis tools **optimize RTL descriptions** while preserving their required functionality.

The experiments demonstrated how redundant logic, constant values, unnecessary registers, and unused counter bits can be simplified or removed.

### 🔹 Work Completed

* Introduction to synthesis optimization
* Boolean logic simplification
* Constant propagation
* Redundant logic removal
* Conditional logic simplification
* Nested conditional optimization
* Combinational RTL optimization
* Sequential logic optimization
* D Flip-Flop optimization
* Constant-driven Flip-Flop optimization
* Removal of unnecessary sequential logic
* Sequential constant propagation
* Register dependency analysis
* Counter optimization
* Unused counter-bit removal
* Comparison logic optimization
* RTL simulation
* Waveform verification
* Optimized synthesis
* Synthesized circuit inspection
* RTL versus synthesized hardware comparison

### 💡 Important Learning

Synthesis tools do not necessarily implement every line of RTL as separate hardware.

For example:

```text
RTL Description
      ↓
Logic Analysis
      ↓
Optimization
      ↓
Simplified Hardware
```

Therefore, **RTL complexity and synthesized hardware complexity can be different**.

---

# Module 4 – Gate-Level Simulation and RTL Coding Practices

Module 4 focused on **RTL coding styles, simulation behavior, synthesis, and Gate-Level Simulation**.

The module also explored blocking and non-blocking assignments and their effect on procedural simulation behavior.

### 🔹 Work Completed

* Different MUX coding styles
* Ternary/conditional MUX
* MUX using `always` blocks
* Sensitivity lists
* Incomplete sensitivity lists
* Simulation-modeling issues
* `always @(*)`
* Introduction to `always_comb`
* Blocking assignments
* Non-blocking assignments
* Procedural execution order
* Simulation ordering
* Sequential coding practices
* RTL functional simulation
* Yosys synthesis
* Gate-level netlist generation
* SKY130 technology mapping
* Gate-Level Simulation (GLS)
* Standard-cell functional models
* RTL versus GLS waveform comparison
* Synthesized hardware verification

### ⚡ Blocking vs Non-Blocking

| Assignment | Common Usage        |
| ---------- | ------------------- |
| `=`        | Combinational logic |
| `<=`       | Sequential logic    |

A common RTL coding guideline is:

```verilog
always @(*) begin
    y = a & b;
end
```

for combinational logic, and:

```verilog
always @(posedge clk) begin
    q <= d;
end
```

for sequential logic.

---

# Module 5 – Synthesis-Oriented RTL Coding

Module 5 focused on how **RTL coding style affects the hardware inferred during synthesis**.

The main topics included incomplete conditional statements, latch inference, `case` statements, MUX/DEMUX implementations, `generate` constructs, and ripple-carry adders.

### 🔹 Work Completed

* Synthesis-oriented RTL coding
* Combinational RTL description
* Incomplete `if` statements
* Incomplete `if-else` statements
* Incomplete `case` statements
* Latch inference
* Unintended storage behavior
* Complete combinational logic
* Complete `case` statements
* `default` branches
* Case-based MUX
* Generate-based MUX
* Case-based DEMUX
* Generate-based DEMUX
* Repeated hardware structures
* `generate` constructs
* Ripple-carry adder
* Repeated full-adder structures
* RTL simulation
* Yosys synthesis
* Synthesized hardware inspection
* RTL coding-style comparison

### ⚠️ Latch Inference

Incomplete combinational assignments can cause synthesis tools to infer latches.

For example:

```verilog
always @(*) begin
    if (sel)
        y = a;
end
```

When `sel` is false, `y` does not receive a new value. This can result in storage behavior.

A complete description should provide an assignment for every possible condition.

---

# 🔄 Overall Training Flow

Across Modules 1–5, the training covered the following digital design flow:

```text
RTL DESIGN
    ↓
VERILOG CODING
    ↓
TESTBENCH DEVELOPMENT
    ↓
FUNCTIONAL SIMULATION
    ↓
VCD GENERATION
    ↓
GTKWAVE ANALYSIS
    ↓
YOSYS SYNTHESIS
    ↓
LOGIC OPTIMIZATION
    ↓
TECHNOLOGY MAPPING
    ↓
SKY130 STANDARD CELLS
    ↓
GATE-LEVEL NETLIST
    ↓
GATE-LEVEL SIMULATION
    ↓
WAVEFORM VERIFICATION
```

---

# 🛠️ Tools and Technologies

| Tool / Technology  | Purpose                                             |
| ------------------ | --------------------------------------------------- |
| **Verilog HDL**    | RTL design and hardware description                 |
| **Icarus Verilog** | Functional and gate-level simulation                |
| **GTKWave**        | Waveform visualization and analysis                 |
| **Yosys**          | RTL synthesis and optimization                      |
| **ABC**            | Logic optimization and technology mapping           |
| **SKY130**         | Open-source standard-cell technology                |
| **Liberty `.lib`** | Cell timing, area, power and functional information |
| **Linux**          | Development environment                             |

---

# 🎯 Overall Learning Outcomes

The training provided practical understanding of:

* RTL design and Verilog coding
* Testbench development
* Functional simulation
* VCD waveform generation
* Waveform-based verification
* RTL-to-netlist conversion
* Standard-cell libraries
* Timing library concepts
* Hierarchical RTL design
* Sequential logic
* Flip-Flops
* Asynchronous set/reset
* Combinational optimization
* Sequential optimization
* Synthesis-oriented coding
* Latch inference
* MUX and DEMUX implementation
* `generate` constructs
* Ripple-carry adder design
* Technology mapping
* Synthesized netlist analysis
* Gate-Level Simulation
* RTL versus synthesized hardware verification

---

# 🚀 VSDBabySoC – Integrated RTL-to-Gate-Level Verification

As an integrated practical exercise, the **VSDBabySoC** design was taken through **pre-synthesis and post-synthesis verification**.

The objective was to understand how a small RISC-V-based SoC moves from RTL to a technology-mapped gate-level implementation and how its functionality can be verified after synthesis.

---

# BabySoC – RTL to Post-Synthesis Gate-Level Verification

## 📌 Introduction

BabySoC is a small **RISC-V-based System-on-Chip** used to demonstrate an ASIC front-end design flow.

The design consists of three major hardware blocks:

| Block       | Function                                                |
| ----------- | ------------------------------------------------------- |
| **RVMyth**  | RISC-V processor responsible for digital processing     |
| **AVSDPLL** | Generates the clock used by the processor               |
| **AVSDAC**  | Converts digital processor output into an analog output |

---

# 🧩 BabySoC Architecture

The top-level design is organized as:

```text
                 vsdbabysoc
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
     AVSDPLL       RVMyth       AVSDAC
        │            │            ▲
        │            │            │
        │           CLK      RV_TO_DAC[9:0]
        │            │            │
        └────────────┘            │
                                  │
                                  └── Digital Output
```

### Design Hierarchy

```text
vsdbabysoc
├── avsddpll
├── rvmyth
└── avsddac
```

### Signal Flow

```text
REF / PLL Control
       ↓
    AVSDPLL
       ↓
      CLK
       ↓
    RVMyth
       ↓
 RV_TO_DAC[9:0]
       ↓
    AVSDAC
       ↓
      OUT
```

---

# 🔧 ASIC Design Flow

The BabySoC experiment follows the front-end portion of the ASIC design flow.

```text
RTL Design
    ↓
Pre-Synthesis Simulation
    ↓
Logic Synthesis
    ↓
Logic Optimization
    ↓
SKY130 Technology Mapping
    ↓
Gate-Level Netlist
    ↓
Post-Synthesis Simulation
    ↓
Static Timing Analysis
    ↓
Physical Design
    ↓
GDSII
```

### Project Status

| Design Stage              | Status      |
| ------------------------- | ----------- |
| RTL Design                | ✅ Completed |
| Pre-Synthesis Simulation  | ✅ Completed |
| Yosys Synthesis           | ✅ Completed |
| SKY130 Technology Mapping | ✅ Completed |
| Gate-Level Netlist        | ✅ Completed |
| Post-Synthesis Simulation | ✅ Completed |
| Functional Verification   | ✅ Completed |
| Static Timing Analysis    | 🔜 Next     |
| Floorplanning             | ⏳ Upcoming  |
| Placement                 | ⏳ Upcoming  |
| Clock Tree Synthesis      | ⏳ Upcoming  |
| Routing                   | ⏳ Upcoming  |
| Physical Verification     | ⏳ Upcoming  |
| GDSII Generation          | ⏳ Upcoming  |

---

# 1. 🧪 Pre-Synthesis Simulation

Before synthesis, the original RTL implementation was simulated to verify its expected functionality.

Simulation was performed using **Icarus Verilog**, and the generated waveform was analyzed using **GTKWave**.

### Important Signals

* `CLK`
* `REF`
* `reset`
* `VCO_IN`
* `VREFH`
* `RV_TO_DAC[9:0]`
* `OUT`

The pre-synthesis waveform acts as the **reference behavior** for comparison with the synthesized design.

![Pre-Synthesis Waveform](image.png)

---

# 2. ⚙️ Synthesis Using Yosys

After RTL verification, the BabySoC design was synthesized using **Yosys**.

The design was targeted to the **SKY130 high-density standard-cell library**.

### Technology Library

```text
Library:
sky130_fd_sc_hd

Liberty:
sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Important Yosys Operations

| Yosys Operation     | Purpose                             |
| ------------------- | ----------------------------------- |
| `read_verilog`      | Reads RTL source files              |
| `dfflibmap`         | Maps Flip-Flops to library cells    |
| `opt`               | Performs logic optimization         |
| `abc`               | Technology mapping and optimization |
| `flatten`           | Removes module hierarchy            |
| `setundef -zero`    | Resolves undefined signals          |
| `clean -purge`      | Removes unused logic                |
| `rename -enumerate` | Renames internal signals            |
| `write_verilog`     | Generates synthesized netlist       |
| `show`              | Displays schematic representation   |

![Yosys Synthesis](image-1.png)

---

# 3. 📊 Synthesis Statistics

After synthesis and optimization, Yosys generates statistics describing the resulting hardware.

These statistics can be used to understand:

* Number of cells
* Types of cells used
* Number of Flip-Flops
* Combinational logic
* Multiplexers
* Gates
* Overall synthesized design complexity

![Synthesis Statistics](image-2.png)

---

# 4. 🔬 SKY130 Technology Mapping

After logical optimization, the design was mapped to cells available in the **SKY130 standard-cell library**.

The abstract RTL operations are now represented using actual technology-specific standard cells.

### Example Mapped Cells

```text
sky130_fd_sc_hd__nand2_1
sky130_fd_sc_hd__nor2_1
sky130_fd_sc_hd__and2_0
sky130_fd_sc_hd__mux2_1
sky130_fd_sc_hd__xor2_1
sky130_fd_sc_hd__dfrtp_1
```

This stage connects the **technology-independent RTL description** with a specific semiconductor technology.

---

# 5. 🏗️ Technology-Mapped Netlist

The synthesized BabySoC can be inspected at different hierarchical levels.

## Top-Level BabySoC Netlist

![Top-Level BabySoC Netlist](image-3.png)

## RVMyth CPU Netlist

![RVMyth CPU Netlist](image-4.png)

## Expanded RVMyth Netlist

![Expanded RVMyth Netlist](image-5.png)

## Clock-Gating Netlist

The synthesized netlist views help visualize how the original RTL hierarchy is transformed into **standard-cell based hardware**.

---

# 6. 🧪 Post-Synthesis Gate-Level Simulation

After generating the technology-mapped netlist, the synthesized hardware was simulated again.

Unlike RTL simulation, Gate-Level Simulation uses the synthesized gate-level implementation.

### GLS Components

```text
Synthesized Gate-Level Netlist
             +
SKY130 Cell Models
             +
Original Testbench
             ↓
       Icarus Verilog
             ↓
     post_synth_sim.vcd
             ↓
          GTKWave
```

The simulation was performed using:

```text
-DPOST_SYNTH_SIM
-DFUNCTIONAL
-DUNIT_DELAY=#1
```

![Post-Synthesis Gate-Level Simulation](image-6.png)

---

# 7. 🔍 RTL vs Gate-Level Verification

One of the most important objectives of the experiment was to compare the behavior of:

```text
Original RTL
     VS
Synthesized Gate-Level Design
```

The comparison focused on:

* `CLK`
* `REF`
* `reset`
* `RV_TO_DAC[9:0]`
* `OUT`

The `RV_TO_DAC[9:0]` signal is particularly useful because it represents the digital information transferred from the processor toward the DAC.

### Verification Concept

```text
RTL Simulation
      ↓
Reference Behavior
      ↓
Synthesis
      ↓
Gate-Level Simulation
      ↓
Waveform Comparison
      ↓
Functional Verification
```

If the synthesized implementation produces the expected behavior for the same testbench, it provides evidence that synthesis preserved the intended functionality.

### RTL Waveform

```markdown
![RTL Waveform](images/pre_synth_babysoc.png)
```

---

# 8. ⚡ Functional GLS vs Timing GLS

Gate-Level Simulation can be performed in different ways.

## Functional Gate-Level Simulation

Functional GLS checks the logical behavior of the synthesized gates.

```text
RTL
 ↓
Synthesis
 ↓
Gate-Level Netlist
 ↓
Functional GLS
 ↓
Logical Verification
```

This is the type of GLS performed in the current project.

## Timing Gate-Level Simulation

Timing GLS additionally considers delays associated with cells and interconnects.

It can be used to investigate timing-related behavior such as:

* Setup violations
* Hold violations
* Clock delays
* Propagation delays
* Timing-dependent behavior

Timing analysis is planned for a later stage using **Static Timing Analysis (STA)**.

---

# 9. 🛠️ BabySoC Tools and Technologies

| Tool / Technology  | Usage                               |
| ------------------ | ----------------------------------- |
| **Verilog HDL**    | RTL design                          |
| **Icarus Verilog** | RTL and gate-level simulation       |
| **GTKWave**        | Waveform analysis                   |
| **Yosys**          | Logic synthesis                     |
| **ABC**            | Optimization and technology mapping |
| **SKY130**         | Standard-cell technology            |
| **Liberty `.lib`** | Cell and timing information         |
| **Linux**          | Development environment             |

---

# 10. 📈 Current Project Status

The BabySoC project has successfully completed the RTL-to-functional-GLS flow:

```text
BabySoC RTL
     ↓
Pre-Synthesis Simulation
     ↓
Yosys Synthesis
     ↓
Logic Optimization
     ↓
SKY130 Technology Mapping
     ↓
Gate-Level Netlist
     ↓
Post-Synthesis Simulation
     ↓
RTL vs GLS Comparison
     ↓
Functional Verification ✓
```

### Current Milestone

> **RTL → Post-Synthesis Gate-Level Simulation ✅**

---

# 11. 🔜 Next Steps

The next stages of the ASIC flow are:

```text
Static Timing Analysis
        ↓
Floorplanning
        ↓
Power Planning
        ↓
Placement
        ↓
Clock Tree Synthesis
        ↓
Routing
        ↓
Physical Verification
        ↓
GDSII Generation
```

These stages will move the project from **logical design and verification** toward **physical implementation**.

---

# 12. 💡 Key Learnings

### 1. RTL Is an Abstraction

Verilog RTL describes the required hardware behavior, but it does not directly specify the final physical implementation.

```text
RTL
 ↓
Synthesis
 ↓
Optimized Logic
 ↓
Standard Cells
```

### 2. Synthesis Performs Optimization

Synthesis tools analyze RTL and remove or simplify hardware that is unnecessary for the required functionality.

### 3. Technology Mapping Makes RTL Concrete

Technology mapping converts generic logic into cells from a specific technology library such as SKY130.

### 4. Hierarchy Helps Debugging

Understanding the relationships between the CPU, PLL, DAC, and top-level module makes it easier to trace signals and debug the design.

### 5. Pre-Synthesis Simulation Is Important

RTL simulation establishes the expected behavior before synthesis.

### 6. Post-Synthesis Simulation Provides Additional Confidence

GLS verifies that the synthesized implementation still behaves correctly using the generated gate-level netlist.

### 7. Functional Verification Is Different From Timing Verification

Functional GLS checks logical correctness, while STA and timing GLS are used to investigate timing behavior.

---

# 🎯 Conclusion

The **VLSI RTL Design and Synthesis training** provided practical exposure to the complete front-end digital design flow.

The training progressed from basic Verilog RTL coding and simulation to synthesis, optimization, technology mapping, sequential logic, hierarchical design, and Gate-Level Simulation.

The integrated **VSDBabySoC project** extended this knowledge to a small RISC-V-based SoC and demonstrated how the design can be taken from RTL through synthesis and SKY130 technology mapping to a post-synthesis gate-level implementation.

The overall learning flow can be summarized as:

```text
Verilog RTL
     ↓
Functional Simulation
     ↓
Waveform Verification
     ↓
Yosys Synthesis
     ↓
Logic Optimization
     ↓
Technology Mapping
     ↓
SKY130 Standard Cells
     ↓
Gate-Level Netlist
     ↓
Gate-Level Simulation
     ↓
RTL vs GLS Verification
     ↓
Static Timing Analysis
     ↓
Physical Design
     ↓
GDSII
```

This work established a strong practical foundation for understanding how **RTL descriptions are transformed, optimized, mapped to standard-cell hardware, synthesized into gate-level netlists, and verified before entering the physical-design stage**.
