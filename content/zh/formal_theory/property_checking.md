---
weight: 2
title: "使用 SystemVerilog 断言进行属性检查"
description: "使用 SystemVerilog 断言进行属性检查"
draft: true
---

### 1. 什么是断言

根据P1800规范，断言用于指定系统的行为。这个术语容易引起混淆，因为它与“属性”的定义相似。实际上，断言和属性指的是同一件事。这种不一致主要是因为在断言驱动验证（ABV）中使用了“断言”这个术语，而在形式化属性验证（FPV）中使用了“属性”。ABV的应用更为广泛，因此“断言”这个术语被以一种更“传统”的方式使用。

断言有两种类型：即时断言和并发断言。下显示了并发断言的示例。这是形式属性验证（FPV）中常用的断言类型。

![sva](https://yosyshq.readthedocs.io/projects/ap109/en/latest/_images/assertion_struct.png)

如上图 1 所示，属性具有一个包含不同功能的验证层，这些功能包括assert、assume、cover和restrict，它们后面会进行描述。

### 2. 断言的种类

#### 2.1 即时断言

即时断言是纯组合逻辑，不允许有时间域事件或序列。即时断言具有以下特性：

- 非时间性：它们在同一时间进行评估和报告（不能等待任何时间）。
- 立即评估：在激活断言条件变量时对值进行采样。
- 更简单的评估语义：时钟控制的即时断言不具有并发断言的语义。
- 只能在过程块中指定。

#### 2.2 并发断言

并发断言捕获跨越时间的事件序列，即它们具有在系统的每个时钟周期或时间步长评估的时间域。

下图2 显示了并发断言定义的示例。这种断言可以定义在：

- `initial` 或 `always` 块  
- 在 `module` 或 `interface` 里面  
- 对于仿真，可以在 `program` 里面  

![concurrent](https://yosyshq.readthedocs.io/projects/ap109/en/latest/_images/concurrent0.png) 

### 3. 时钟或时间步

并发断言与定义采样时钟或评估断言的时间点的时钟相关联。此构造还有助于显式定义采样值函数的事件，这将在下一节中讨论。并发属性的默认时钟事件可以使用关键字default Clocking 进行定义，并充当所有并发属性的主导时钟。图 4.1 显示了默认时钟定义的示例。

### 4. disable 条件

禁用条件

同样，一些属性可能需要在某些事件期间禁用，因为无论如何它们的结果都无效，例如，在复位状态期间。可以使用默认的`disable iff (event)`关键字来定义不打算检查并发断言结果的时间。图4.1显示了一个默认复位定义的示例。

```
// 并发属性在每个 *posedge* PCLK 时进行检查。
default clocking
   formal_clock @(posedge PCLK);
endclocking

// And disabled if the *PRSTn* reset is deasserted 如果 *PRSTn* reset 无效，则会 disable
default disable iff (!PRSTn);

/* 该属性不需要显式定义 PCLK 作为主时钟和 !PRSTn 作为禁用事件，因为它们已在默认时钟和禁用块中定义。*/
property_a: assert property (RxStatus == 3'b011 |-> ##1
			     Receiver_detected);
```