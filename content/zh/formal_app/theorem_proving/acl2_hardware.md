+++
categories = ["Theorem Proving"]
tags = "acl2"
title = "使用 ACL2 进行硬件验证"
weight = 7
+++

## 1. Vl

VL Verilog 工具包是一个大型 ACL2 库，用于处理SystemVerilog外部链接（以及 常规 Verilog外部链接）源代码，它作为许多 Verilog 工具的前端。它包含以下部分：

- Verilog 语法的内部表示（Vl-design、Vl-module、Vl-port、Vl-always 等）
- 用于将 Verilog 源代码解析为这种表示形式的加载器
- 可以简化这些设计的各种变换（Annotate、Elaborate等过程）
- 打印和其他报告生成功能

## 2. Sv

SV 是一个硬件验证库，具有基于向量的 表达式表示（svex）、与 gl 集成的高效符号模拟并支持许多 SystemVerilog 功能。它通常作为vl的后端。

- SV 的核心是基于一种符号表达式语言，该语言表示四值“bits”无限宽度向量
