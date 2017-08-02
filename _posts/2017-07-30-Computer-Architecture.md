---
layout: post
published: True
title: Computer Systems Architecture

images:
    - url: https://upload.wikimedia.org/wikipedia/commons/e/ea/MIPS_Architecture_%28Pipelined%29.svg
      alt: Computer Architecture
      title: mips datapath 
---

## Current trends 

The instruction count, or **IC**,  of a program is determined by the ISA/compiler/program.

$$ \textbf{CPI} = \sum_{i=1}^n CPI_i \frac{Instuction Count_i}{Instruction Count} $$

Relative Performance = $$\frac{1}{E_t}$$ where $$E_t$$ is the **execution time**.

          | IC | CPI | T_c |
Algorithm | Y  | Y   | N   |
Language  | Y  | Y   | N   | 
Compiler  | Y  | Y   | N   |
ISA       | Y  | Y   | Y   |

$$ T_{CPU} = IC \times CPI \times T_c $$

## Instruction Set Architecture
Different computers have different instruction sets. 

**Amdahl's Law** - $$T_{imp} = \frac {T_{affected} }{improvment factor} + T_{unaffected} $$
We cannot make this curve flat, which implies that linear improvement through parallelization is hard to achieve, since there are inherently parts that cannot be serialized.

## MIPS
Each instruction is 4 bytes, or a WORD of memory.

#### Immediate Operands
These operate on a register as well as data incoded in the instruction itself.
```asm
addi $s0, $s0, 4  //example addition, takes the value of $s0 and adds 4
addi $s0, $s9, -1 //for subtraction, just add a negative constant
```
This has the advantage of making common operations fast. The constant can only be 16 bits long - an appropriate trade off because small constants are the most common. 

#### Logical Operations
sll
srl 
and, andi
or, ori
nor

#### Conditional Operations
beq rs rt L1
bne rs rt L1
j L1

#### R-format instructions
#### I-format instructions

## Arithmetic and the ALU
One-bit ALU, Full/Half Adder, Dealing with overflow

Operations: Add, subtract, SLT, 
Carry Lookahead adder / ripple-carry adder, generates and propogates

Multiplication, and Booth's algorithm. 

## Single Cycle Processor
CPU Overview
Parts: Instruction Fetch, Instruction Decode, Register, AlU, Data Memory

## Pipelining
sub  1 CPI for pipelined datapaths

Data Hazards, structural hazards, and other hazard, data forwarding, load-use hazard

Branch prediction, dynamic branch prediction, 

## Instruction Level Parallelism
VLIW, Multiple issue, dynamic multiple issue, loop unrolling

## Memory and Caching
Memory Heirarchy, AMAT, Associative caches, Cache misses - write back, write-through, write-allocate. 
Multi-level cahces, Virtual memory and page faults (TLB)

Different types of misses, VIPT vs VIVT vs PIPT
Cache coherence, snooping and snarfing, 

