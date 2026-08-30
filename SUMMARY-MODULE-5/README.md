# 📘 Module 5 – Summary

## 🔎 Overview

Module 5 focused on **RTL coding styles and synthesis optimization**. It explained how Verilog coding styles can affect the hardware generated during synthesis.

The main topics covered were **latch inference, complete combinational logic, `case` statements, procedural `for` loops, `for generate`, hierarchical design, and compilation of multiple modules**.

## ⚠️ Latch Inference

Incomplete `if` or `case` statements can cause **unintended latch inference**.

For combinational logic, every possible condition should assign a value to the output.

```text
Incomplete condition
       ↓
No output assignment
       ↓
Previous value retained
       ↓
Latch may be inferred
```

Using an `else`, default assignment, or `default` case helps avoid this problem.

## 🔢 Case Statements

A `case` statement should properly cover all possible input conditions.

A `default` case can be used to handle unspecified conditions:

```verilog
case(sel)
    2'b00 : y = i0;
    2'b01 : y = i1;
    default : y = i2;
endcase
```

Wildcard conditions should also be used carefully to avoid **overlapping cases** and unintended priority behavior.

## 🔄 `for` Loop and `for generate`

A procedural `for` loop is used to **repeat operations** inside an `always` or `initial` block.

A `for generate` construct is used to **replicate hardware structures**, such as multiple Full Adders in a Ripple Carry Adder.

```text
for
 ↓
Repeat operations

for generate
 ↓
Replicate hardware
```

## 🧩 Hierarchical Design

Large designs can be divided into smaller reusable modules.

For example:

```text
fa.v
  ↓
rca.v
  ↓
tb_rca.v
```

Hierarchical design makes the circuit easier to **understand, debug, reuse, and verify**.

## 💻 Module Compilation

When one Verilog module depends on another, all required source files must be compiled together.

```bash
iverilog fa.v rca.v tb_rca.v
```

The generated simulation can then be analyzed using GTKWave.

## 🎯 Key Learnings

* Understood **latch inference**.
* Learned how to write **complete combinational RTL**.
* Studied `case` and `default` statements.
* Learned about **procedural `for` loops**.
* Understood **`for generate` for hardware replication**.
* Learned about **hierarchical Verilog design**.
* Understood compilation of **multiple dependent modules**.
* Learned how coding style affects **synthesis results**.

## ✅ Conclusion

Module 5 emphasized the importance of writing **clean, complete, and synthesis-friendly RTL**. Proper use of `if`, `case`, loops, generate constructs, and hierarchy helps prevent unintended hardware and produces **predictable synthesis results**.
