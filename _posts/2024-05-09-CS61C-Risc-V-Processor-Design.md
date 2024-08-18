---
title: CS61C-Risc-V Processor Design
author: Sillycheese
date: 2024-05-09 20:00:00 +0800
categories:
  - CS61C
tags:
  - Architecture
---

友情链接：[MIPS指令结构](https://cosmosning.github.io/2019/11/12/mips-zhi-ling-xi-tong-jian-yi-ru-men/)

## CPU

- **Processor**: active part of the computer that does all the work(data manipulation and decision-making)
- **Datapath**: portion of the Processor that contains hardware necessary to perform operations required by the Processor
- **Control**: portion of the Processor(also in hardware) that tells the Datapath what needs to be done

## Building a Risc-V Processor

可以类比成状态机的设计。

将执行指令的过程拆解为多个小状态，方便设计。

- Stage 1: 指令的抓取（posedge）
- Stage 2: 指令的解码/寄存器的读取
- Stage 3: 执行-ALU
- Stage 4: 访问内存
- Stage 5: 写回到寄存器中（posedge）

### How to Build a Datapath

![Desktop View](/img/image-20240509191531569.png){: .normal}

- 寄存器
- 写使能：为1时在posedge的时候更新为In

![Desktop View](/img/image-20240509192020112.png){: .normal}

- 寄存器组（包含32个寄存器）
- RA用来选择放在busA的寄存器，RB同理
- RW用来选择要被执行写操作的寄存器（当写使能为1时且写入的数据由busW传递）
- 当执行读取操作时，寄存器组被视作一个组合逻辑电路（在输出前的延迟时间为乘坐”access time“

![Desktop View](/img/image-20240509192646974.png){: .normal}

- 内存
- 对于读取：地址选择应该被放在Data Out的数据
- 对于写入：地址选择应该被Data In写入的内存（当写使能为1时）
- 读取操作与寄存器相同

#### Datapath for add/sub

![Desktop View](/img/image-20240509195932535.png){: .normal}

#### Datapath With Immediates

在I形指令中，立即数需要做扩展。而扩展的方式分为零扩展与符号扩展（根据编码的不同，扩展的方式也不相同）

![Desktop View](/img/image-20240509202029451.png){: .normal}

#### Datapath for Stores

![Desktop View](/img/image-20240509210142003.png){: .normal}

#### Implementing Branches

改变PC来实现分支：

 - PC=PC+4(非分支)
 - PC=PC+immediate(分支)

![Desktop View](/img/image-20240509212452539.png){: .normal}

#### Adding JALR to Datapath

JALR是I形指令（Jump-And-Link-Register)：

 - JALR rd,rs,immediate
 - 将PC+4写入rd
 - 设置PC=rs1+immediate

![Desktop View](/img/image-20240509214341818.png){: .normal}

#### Adding JAL

J形式指令（与上面的JALR有一些类似）。不同的地方在于，PC=PC+offset（PC-relative jump)

![Desktop View](/img/image-20240509215137258.png){: .normal}

#### Adding U-Types

~~终于要结束了~~

我们来看最后一种类型的指令——U-Types.

- lui：load upper immediate
- auipc：add upper immediate to PC

![Desktop View](/img/image-20240509220202591.png){: .normal}

#### Recap Datapath!

到此，Datapath设计完成！

![Desktop View](/img/image-20240509221019335.png){: .normal}

 