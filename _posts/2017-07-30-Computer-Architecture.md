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

The instruction count of a program is determined by the ISA/compiler/program.

$$ \textbf{CPI} = \sum_{i=1}^n CPI_i \frac{Instuction Count_i}{Instruction Count} $$

## Instruction Set Architecture
Different computers have different instruction sets. 

## MIPS
details about MIPS assembly

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

