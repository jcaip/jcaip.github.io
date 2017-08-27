---
layout: post
published: True
title: Computer Systems Architecture

images:
    - url: https://upload.wikimedia.org/wikipedia/commons/e/ea/MIPS_Architecture_%28Pipelined%29.svg
      alt: Computer Architecture
      title: mips datapath 
---

## Measuring Performance 
The instruction count, or **IC**, is the number of instructions that a program must execute. 
**CPI** refers to cycles per instruction, and is usually fixed for different instructions. 

To get the CPI of a program, we take the weighted average

$$ \textbf{CPI} = \sum_{i=1}^n CPI_i \frac{ \text{Instuction Count}_i}{\text{Instruction Count}} $$

We can use how long a program will run as a measure of performance.
$$ T_{CPU} = IC \times CPI \times T_c $$


**Relative Performance** = $$\frac{1}{E_t}$$ where $$E_t$$ is the **execution time**. If programs run in less time, then the machine performs better

We've already hit the [power wall](https://www.technologyreview.com/s/421186/why-cpus-arent-getting-any-faster/), so we need to exploit parallelism to increase performance further.
Improving one aspect of a pipeline will not asymptotically increase performance for the entire pipeline.

**Amdahl's Law** - $$T_{imp} = \frac {T_{affected} }{\text{improvment factor}} + T_{unaffected} $$
![amdhals law](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/AmdahlsLaw.svg/400px-AmdahlsLaw.svg.png)
We cannot make this curve flat, which implies that linear improvement through parallelization is hard to achieve, since there are inherently parts that cannot be serialized.

#### What affects different performance metrics

|          | IC | CPI | T_c |
|Algorithm | Y  | Y   | N   |
|Language  | Y  | Y   | N   | 
|Compiler  | Y  | Y   | N   |
|ISA       | Y  | Y   | Y   |

## MIPS
MIPS is an instruction set with simple instructions, which enables pipelining and parallelism. It is a RISC architecture.

Each instruction in MIPS is 4 bytes (32 bits), or a **WORD** of memory. They are encoded in binary, with a small number of formats. Most instructions are highly regular. 

The processor cycle can be descripbed as a continuous loop between 5 stages:
  Instruction Fetch (IF)
  Instruction Decode (ID)
  Execute (EX)
  Acces Memory (MEM)
  Store Result (WB)

#### R-format Instuctions
Used for airthmetic/logical operands
```
| op   | rs  | rt  | rd  |shift| funct|
  6       5     5     5     5     6    
```

#### I-format Instructions
provides an immediate/constant, also used for load/store instructions
```
| op   | rs  | rt  |  constant/address |
  6       5     5          16
```

**Immediate Operations** - These operate on a register as well as data incoded in the instruction itself.
```asm
addi $s0, $s0, 4  //example addition, takes the value of $s0 and adds 4
addi $s0, $s9, -1 //for subtraction, just add a negative constant
```
This has the advantage of making common operations fast. The constant can only be 16 bits long - an appropriate trade off because small constants are the most common. 

**Logical Operations** - These are logical operations
```asm
sll //shift left logical (fills with 0)
srl //shift right logical (fills with 0)
and, andi // logical and, andi is for anding with an immediate
or, ori // logical or
nor //logical not
```

**Conditional Operations** - These are used to alter the PC
```asm
beq $rs $rt L1 // if rs==rt branch to L1
bne $rs $rt L1 // if rs!=rt bramch to L1
j L1   // jump to L1 unconditionally
```

**Arithmetic Operations** - These are used for airthmetic, usually in the format a gets b + c
```asm
addi
add
sub
subi
```

#### Register Operations
Arithmetic uses register operands (at least for MIPS) - you can't add two values stored in memory.
In MIPS, we use a 32x32bit register file. That is we have 32 registers, each of which is 32 bits on. This is directly from our R/I-format, each register is encoded by 5 bits, so in total 32 registers are able to be encoded.

#### Memory Operations
```
```
We need explicit instructions for memory mamagement because this is a RISC architecture.
We assume that 

## Arithmetic and the ALU
One-bit ALU, Full/Half Adder, Dealing with overflow

Operations: Add, subtract, SLT, 
Carry Lookahead adder / ripple-carry adder, generates and propogates

Multiplication, and Booth's algorithm. 

## Single Cycle Processor
CPU Overview
Parts: Instruction Fetch, Instruction Decode, Register, AlU, Data Memory

## Pipelining
In order to improve performance, we can overlap execution of instructions to reach a **steady state**, where CPI reaches it's asymptotic mininum.

If all stages are balanced, then $$T_{pipeline} = \frac{T_{sequential}}{n}$$ where $$n$$ is the number of stages.
Otherwise, $$T_{pipeline} = \max(T_i)$$ where $$T_i$$ is the time taken for each part of the pipeline.

MIPS is well suited for pipelining - all instructions are 32-bits, and are very regular. Furthermore, memory access only takes 1 cycle.

In order to pipeline properly, we must uphold several principles.
- Must have same stages in the same order
- All intermediate values must be latched
- We cannot use the same functional block twice in a single cycle

Pipelining also introduces **hazards** into the pipeline.
Data Hazards, structural hazards, and other hazard, data forwarding, load-use hazard

Branch prediction, dynamic branch prediction, 

## Instruction Level Parallelism
VLIW, Multiple issue, dynamic multiple issue, loop unrolling

## Memory and Caching
Memory Heirarchy, AMAT, Associative caches, Cache misses - write back, write-through, write-allocate. 
Multi-level cahces, Virtual memory and page faults (TLB)

Different types of misses, VIPT vs VIVT vs PIPT
Cache coherence, snooping and snarfing, 

