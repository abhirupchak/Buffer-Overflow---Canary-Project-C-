

# Canary Measure for Buffer Overflow Protection


## Introduction

A **buffer overflow** happens when a program writes more data to a buffer than it can handle, which can overwrite important memory and allow attackers to take control.

To prevent this, a **canary** is used as a safeguard. It helps detect buffer overflows before they cause harm.

## What is a Canary?

A **canary** is a small piece of data placed before important memory, like return addresses. If an overflow occurs, the canary gets overwritten. Before a function returns, the program checks if the canary is still the same. If it's changed, the program stops to prevent an attack.

## How Canaries Work

1. **Placing the Canary:** The program puts a canary before the return address in memory.
2. **Function Runs:** The function executes as normal.
3. **Checking the Canary:** Before returning, the program checks if the canary is unchanged.
4. **Stopping the Attack:** If the canary is different, the program detects an attack and stops.

## Return Addresses and Exploitation

The **return address** is a critical piece of memory that tells a function where to go after execution. Attackers often try to overwrite return addresses in buffer overflow attacks to redirect execution to malicious code.

When a buffer overflow occurs, if there is no protection, the attacker can change the return address to:

- Point to their own injected shellcode.
- Redirect execution to another part of the program (Return-Oriented Programming - ROP).

By using a canary before the return address, we can detect if an overflow has occurred before an attacker can alter execution flow.

## How Attackers Bypass Canaries

Although canaries are effective, attackers have found ways to bypass them:

- **Leaking Canary Values:** If an attacker can read memory using an information leak vulnerability, they can find the canary value and overwrite it correctly to avoid detection.
- **Jumping Over the Canary:** Some attacks modify memory beyond the canary without touching it, allowing them to overwrite the return address while keeping the canary intact.
- **Corrupting Execution Flow in Other Ways:** Instead of overwriting the return address, attackers may overwrite function pointers, exception handlers, or global variables that control execution flow.

## Optimizing Canary Protection

While canaries help, attackers have found ways to bypass them. Here’s how to make them stronger:

### **1. Stronger Canaries**

- **Use unique canaries per function** instead of one for the entire program.
- **Randomize canaries** using time-based values or system entropy.
- **XOR canaries with a secret key** stored in protected memory.
- **Hardware-Assisted Protection:** Use modern CPU features like Intel MPX (Memory Protection Extensions) to store canaries in special registers rather than memory.

### **2. Smarter Detection**

- **Check canaries at random times,** not just at function return.
- **Combine canaries with other security techniques**.

![Screenshot 2025-03-17 211945](https://github.com/user-attachments/assets/d8871ea0-c214-4e8d-8fee-bba3eaff6fb5)


Buffer Overflow is only a problem in languages like C/C++ which do no have runtime bounds checking which check if the array or any buffer has the space for the character being inputted.

![image](https://github.com/user-attachments/assets/d5c47592-21fd-40bd-8bc4-702f7844c09e)

Normally buffer overflow would lead to the program crashing since the overwritten return address does not have any significance in the code but attackers can alter this return address and exploit your code's vulnerability.
They can access some other function in the code by altering the return address to that function's return address or even inject their own code into return address.
![image](https://github.com/user-attachments/assets/1464a92c-8755-4b36-906c-a0487ab22d1a)

