---
title:CS61C-Single-Cycle CPU Control
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

IF、ID、ALU、MEM、WB五个阶段

## Control Logic Design

ROM(ReadOnly Memory)

