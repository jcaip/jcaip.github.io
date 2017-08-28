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
Linear improvement through parallelization is impossible to achieve, since there are inherently parts that cannot be parallelized. 

![amdhals law](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/AmdahlsLaw.svg/400px-AmdahlsLaw.svg.png)

This can be seen in this graph - as you add more processors, the speedup factor stops improving.

### What affects different performance metrics

|          | IC | CPI | T_c |
|----------|----|-----|-----|
|Algorithm | Y  | Y   | N   |
|Language  | Y  | Y   | N   | 
|Compiler  | Y  | Y   | N   |
|ISA       | Y  | Y   | Y   |

## MIPS
In order to fully understand computer architecture, we must first understand MIPS.
MIPS is an instruction set with simple instructions, which enables pipelining and parallelism. It is a RISC architecture.

Each instruction in MIPS is 4 bytes (32 bits), or a **WORD** of memory. They are encoded in binary, with a small number of formats. Most instructions are highly regular. 

The processor cycle can be descripbed as a continuous loop between 5 stages:
+ Instruction Fetch (IF)
+ Instruction Decode (ID)
+ Execute (EX)
+ Acces Memory (MEM)
+ Store Result (WB)

### MIPS Instriction Formats
**R-Format Instructions** - Used for airthmetic/logical operands
```
| op   | rs  | rt  | rd  |shift| funct|
  6       5     5     5     5     6    
```

**I-Format Instructions** - provides an immediate/constant, also used for load/store instructions
```
| op   | rs  | rt  |  constant/address |
  6       5     5          16
```

**J-Foramt Instructions** - provides a larger constant, used for jump instructions
```
| op   |              constant/address |
  6               26
```

### MIPS Operations
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
add $rs $rt $rd //add with register operands
addi $rs $rd 0x32 //immediate add
sub $rs $rt $rd //sub with register operands
subi $rs $rd 0x32 //immediate subtract
```

**Register Operations** - In MIPS arithmetic uses register operands as you can't add two values stored in memory.
In MIPS, we use a 32x32bit register file. That is we have 32 registers, each of which is 32 bits on. This is directly from our R/I-format, each register is encoded by 5 bits, so in total 32 registers are able to be encoded.

Smaller register files tend to be faster, so increasing the register file size creates a tradeoff.

**Memory Operations** - We need explicit instructions for memory mamagement because this is a RISC architecture.
```asm
lw $t0, 32($s3) // loads a value into memory
sw
```
We assume that memory is a giant array of byte-addressable words and words are aligned in memory.

**Branch Instructions** - Used to alter the PC to control execution flow.
One thing to note is that less than is slower than equivalence
Create combinations of branches/subtractions as a compromise

```asm
blt 
bge
slt, slti
slta, sltai
```

### Other
**Procedure Calls** - How procedure calls are handled in MIPS.
1. Place parameters in registers
2. Transfer control to procedure (Change the PC)
3. Acquire storage for the procedure
4. Run Procedure
5. Place result into a register
6. Return to place of call and continue execution

```asm
jal LabelFuncStart //this changes the PC, saves the previous PC in $ra
jr $ra //jumps the PC back to where it would have been
```

We can see by altering `$ra` we alter the location of the PC, which lets us do calculated jumps as you would see in a case/switch statement.

**Large Constants** - Since the immediate values must fit within 16 bits, when we deal with large constants we have to store them into a register.

```asm
lui $s0 61 //load the upper 16 bits of the register
ori $s0 $s0 2304 //load the lower 16 bits
```

### Addressing
Since memory is byte-addressable, we can omit the last 2 bits when storing an address (It'll always be 00).

**Immediate** - 
**Register** - Store address inside a register file and access it that way
**PC-relative** - PC + offsetx4, where the x4 comes from efficient storing of bits
**Base** - 
**Pseudo-direct** - used for J-type instructions. Takes the upper 4 bits from the current PC and concatenates the next 28 bits 


## Arithmetic and the ALU
Input: ALU op, a, b
Output: Zero, Result, Overflow, Carry Out

### ALU Operations
## Multiplication, and Booth's algorithm. 

### ALU Design
**One Bit Full Adder** - the layout of a full adder for 1-bit operands.
![1-bit adder](/images/comp_arch/1ba.png)

**Ripple Carry Adder** - To get a 32 bit adder, we can chain together 32 one-bit adders, connecting the output of one adder to the carry in of the one below it.
![ripple carry adder](/images/comp_arch/rca.png)
A single NOR gate over the results provides us with the zero output.
Overflow is detected by xoring $$C_{31}$$ and $$C_{32}$$. When the two carry bits are different, an overflow has occured.
Here we have a $$2N$$ gate delay where $$N$$ is the number of bits as there are 2 gates per carry out. (1 OR and 1 AND as seen from the 1-bit full adder aboce) 

**Carry Lookahead Adder** - In order to reduce the gate delay, we can use a carry lookahead adder.
![carry lookahead adder](/images/comp_arch/cla.png)
| A | B | Carry Out | signal  |
|---|---|-----------|---------|
| 0 | 0 |0          | kill    |
| 0 | 1 |Carry In   |propagate|
| 1 | 0 |Carry In   |propagate|
| 1 | 1 |1          |generate |

We can compute the carries as a function propogates and generates

$$C_1 = G_0 + C_0P_0$$
$$C_2 = G_1 + G_0P_1 + C_0P_0P_1$$
$$C_3 = G_1 + G_0P_1 + C_0P_0P_1$$
$$C_4 = G_1 + G_0P_1 + C_0P_0P_1$$

This creates a larger but faster adder.


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

