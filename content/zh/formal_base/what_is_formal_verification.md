---
weight: 1
title: "什么是形式验证"
description: "什么是形式验证"
---

## 一、 一句话描述

形式验证是一种利用数学逻辑和算法全面分析系统设计，以证明或反驳其是否符合特定性质或规格的静态验证技术。

## 二、形式验证概念

形式验证（Formal Verification）是一种静态验证技术，是解决复杂芯片验证问题的重要手段。与仿真验证不同，形式验证不依赖于测试用例，而是通过数学逻辑和算法对设计进行全面的分析。

## 三、形式验证的基础技术

### 1. 模型检查（Model Checking）

模型检验是一种自动化的方法，用于验证有限状态系统的性质。它通过形式化的技术来验证 SystemVerilog 断言 (SVA) 属性，并使用算法遍历所有可能的状态，检查设计是否满足给定的性质。

通常来说，模型检查和属性验证（FPV）是同一个概念，常见的 FPV 工具有 Synopsys 公司的 VC Formal FPV App，和 Cadence 公司的 Jasper FPV App。

模型检查是一个基础技术，除了 FPV 应用外，还衍生出覆盖率检查工具，如 Synopsys 公司的 VC Formal FCA App 和 Cadence 公司的 Jasper UNR App，连接性检查工具，如 Synopsys 公司的 VC Formal CC App 和 Cadence 公司的 Jasper CONN App 等等。

### 2. 等价性检查（Equivalence Checking）

等价性检查用于比较两个设计的功能是否完全相同，通常用于验证设计变更后的正确性。它通过对比两个设计的行为，确保优化或修改后的设计没有引入新的错误。

等价性检查是非常好用的验证手段，它可以作用在设计的不同阶段，如 C 到 RTL（通常是高阶综合前后的验证），RTL 到 Netlist （通常是逻辑综合前后的验证），Netlist 到 Netlist （布局布线前后的验证、或者 ECO 前后的验证等）。

另外，等价性验证可以进一步分为逻辑等价性检查（LEC）和时序等价性检查（SEC）。

- LEC：指两个设计在逻辑功能上完全一致，即给定相同的输入，两个设计产生相同的输出。逻辑等价性验证主要关注设计的功能正确性，不考虑时序行为。LEC 因为它的简单、高效受到广泛应用，如 Synopsys 公司的 Formality 和 Cadence 公司的 Conformal 。

- SEC：不仅要求两个设计在逻辑功能上等价，还要求它们在时序行为上保持一致。这意味着两个设计的延迟和时序约束也必须相同。时序等价性验证不仅确保功能正确性，还确保设计在预期的时序范围内工作。虽然 SEC 全面考虑了时序问题，但是也因此带来了复杂性，面对复杂设计时，消耗庞大的计算资源和时间，应用范围较小，在这方面做得比较好的有西门子的 OneSpin。

### 3. 定理证明（Theorem Proving）

定理证明依赖于数学逻辑和推理技术，通过构建逻辑推理链来证明系统设计的正确性。

定理证明是一个非常强大的形式验证方法，它拥有严格的数学证明，比模型检查具有更高严格的正确性保证，但也因此更具复杂性、资源消耗更庞大。

它特别适用于对设计的可靠性、安全性和功能正确性有极高要求的领域和项目。常见的应用场景是乘法器验证。

ACL2 是一个著名的定理证明器。Intel 和 AMD 都曾拿它来验证他们的浮点数乘法器。

## 四、相关术语

- FV：形式验证（Formal Verification）
- SVA：SystemVerilog 断言（SystemVerilog Assertions）
- FPV：形式化属性验证（Formal Property Verification）
- UNR/FCA：UNR 覆盖率不可达（Coverage Unreachability）；FCA 覆盖率分析器 （Formal Coverage Analyzer）
- CC/CONN：连接性检查（Connectivity Checking）
- LEC：逻辑等价性检查（Logic Equivalence Check）
- SEC：时序等价性检查（Sequential Equivalence Checking）

## 五、相关链接

