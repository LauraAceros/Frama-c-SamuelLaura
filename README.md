# *Team Members:*  
Samuel Hernando Echeverri Castrillón & Laura Sofía Aceros Monsalve

## Contract (Specification) in ACSL for Exercise 5

*Exercise 5*  Demonstrate:  

[i * j + 2 * j + 3 * i = 0^]  j := j + 3; i := i + 2  [i * j = 6]

---

## Detailed Instructions for Running Your Solution

To run the code, you must first have Frama-C installed. After doing this, run Frama-C from the console with the C code file using the command frama-c-gui-wp. This will open the Frama-C interface with the .c file ready for selection.
## Instructions to Run

1. Open Frama-C GUI or run via terminal.
2. Use the following command:

bash
frama-c -wp -wp-rte prueba.c


---

## Versions Used

1. **Frama-C Version**: 26.1 (Calcium)  
2. **Operating Systems:**
   - Ubuntu 20.04.6
   - Windows 11 Versión 10.0.26100
3. **Plugins Used**: `-wp`, `-wp-rte`
---

# Exercise 5: Formal Verification with Frama-C

This is a solution to **Exercise 5**, which focuses on verifying a simple C function using the ACSL specification language and **Frama-C**. The goal is to demonstrate **Hoare-style reasoning**, eliminate possible **integer overflows**, and ensure correctness via **Frama-C's WP (Weakest Precondition) plugin**.


## Problem Statement

Given the initial condition: `i * j + 2 * j + 3 * i == 0`

and the operations: `j := j + 3`; `i := i + 2`;
 
We are asked to verify that, after both assignments, the postcondition holds: `i * j == 6`

---
## Precondition Derivation

We start from the **postcondition** `i * j == 6` and use **backward analysis** to find the required precondition.

Backward analysis is a formal reasoning technique where we **work in reverse from a desired postcondition**, progressively substituting in expressions that reflect the earlier program state.

Since the operations change `i` and `j`, we express the postcondition in terms of their original values:

`(i + 2) * (j + 3) == 6`

Expanding the left-hand side:

`i * j + 3 * i + 2 * j + 6 == 6`

Subtracting 6 from both sides:

`i * j + 3 * i + 2 * j == 0`

This is the required **precondition** that guarantees the postcondition holds after the function executes.

---

##  Intermediate Assertion: `i * j == 6 - 2 * j`

To help guide the verification tool, we include an assertion **after `j` is incremented but before `i` is**. This is not arbitrary; it follows from applying backward reasoning to a partially updated state.

Let’s derive it step-by-step:

1. After `j = j + 3`, but before updating `i`, let’s call the updated `j` value `j1`.
2. Our goal postcondition is:
   `
   (i + 2) * j1 == 6
   `
3. Expanding it:
   `
   i * j1 + 2 * j1 == 6
   `
4. Rearranging:
   `
   i * j1 == 6 - 2 * j1
   `

This gives us the intermediate assertion:

`//@ assert i * j == 6 - 2 * j;`

This follows from **algebraic substitution** and the principle of **backward substitution of expressions**, often used in Hoare Logic and formal proof systems like ACSL.

---

## Code Explanation

The C function `prueba(int i, int j)` is annotated with ACSL specifications. It:

- Adds 3 to `j`
- Adds 2 to `i`

The **ACSL `requires` clause** ensures the precondition `i * j + 2 * j + 3 * i == 0` holds at entry.

The **`ensures` clause** guarantees that `i * j == 6` after execution, which is the desired postcondition.

Overflow safety is also verified using the `-wp-rte` plugin.

---

## Overflow Protection

Since adding 2 to `i` and 3 to `j` may cause integer overflow, we include the constraints:

` i <= INT_MAX - 2
j <= INT_MAX - 3 `

This ensures the operations remain within the `int` limits of 32-bit signed integers.

---






## Helpful Resources
1. Grassmann, Tremblay (1996)  
2. [YouTube Video](https://www.youtube.com/watch?v=R6B5c8Q93Fo)  
3. Notes from the class
