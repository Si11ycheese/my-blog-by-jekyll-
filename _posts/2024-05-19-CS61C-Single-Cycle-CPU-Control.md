---
title: CS61C-Single Cycle CPU Control
author: Sillycheese
date: 2024/5/19 19:49:50 +0800
categories:
  - CS61C
tags:
  - Architecture
---
## Control and Status Registers

- CSRs 在 register file 中被分配（x0-x31）
  - 被用来monitor the status and performance
  - 上限可达4096 CSRs
- 不是ISA的一部分

### System Instructions

- ecall
- ebreak
- fence

## Datapath Control

## Instruction Timing

IF、ID、ALU、MEM、WB五个阶段（后续流水线也是这个过程）

## Control Logic Design

ROM(ReadOnly Memory)

## Pipelining Hazards

### Structural Hazards

Two or more instructions in the pipeline compete for access to a single physical resource

**Solutions**：

- Instructions take it in turns to use resource, some instructions have to stall
- Add more hardware to machine，and can always solve a structural hazard by adding more hardware

![Desktop View](/common/e856c7a2d8f8b72d699771da35c4bde.png){: .normal}

![Desktop View](/common/ed0c3090bb50b425e478906657fe2bf.png){: .normal}

##### Summary

![Desktop View](/common/d0554de548d26a4bcf323910fdf1c64.png){: .normal}

### Data Hazards

**Solutions**:

- Stall
- Forwarding（aka Bypassing）

### Load Data Hazard

Slot after a load is called a load delay slot

- If that instruction uses the result of the load, then the hardware will stall for one cycle
- Equivalent to inserting an explicit nop in the slot
- Performance loss

Idea:

- Put unrelated instruction into load delay slot
- No performance loss!

We can reorder code to avoid use of load result in the next instr!

### Control Hazard

#### Observation

- If branch not taken, then instructions fetched sequentially after branch are correct
- If branch or jump taken, then need to flush incorrect instructions from pipeline by converting to NOPs

#### Reducing Branch Penalties

- Every taken branch in simple pipeline costs 2 dead cycles
- To improve performance, use “branch prediction” to guess which way branch will go earlier in pipeline
- Only flush pipeline if branch prediction was incorrect
