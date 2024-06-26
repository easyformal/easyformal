
---
weight: 2
title: "VCFormal FPV 使用教程"
description: "Synopsys VC Formal FPV 使用教程"
draft: true
---

### 简介

FPV app 用于通过验证属性来验证DUT。这些属性可以由用户创建，或由商业AIP提供，用于接口协议或功能模块。

FPV 应用程序使用一系列验证引擎来彻底证实或证伪一个属性。如果断言失败（证伪），FPV 应用程序将生成一个反例。然而，有些情况下可能无法明确证实或证伪一个属性。在这些情况下，该应用程序将提供 **有界证明** （Bounded Proof）结果，表明在初始状态的**特定深度**内无法找到反例。对于一组断言的证实结果意味着在给定约束下，这些属性不可能为假。

![fpv1](https://cdn.jsdelivr.net/gh/easyformal/easyformal-site@master/content/zh/formal_app/fpv/image/fpv1.png)

FPV 应用程序需要以下几个输入：

- Verilog/SystemVerilog/VHDL 格式的 RTL 设计文件
- SVA 文件中编写的 assertion、assumption 和 cover 属性，或 SystemVerilog 设计文件中的内联 SVA
- TCL 脚本文件

然后，FPV 应用程序将输出属性状态，如 assertion 状态、assumption 状态和 cover 状态。assertion 状态可能为 proven, falsified, vacuous,
covered, uncovered, 或者 inconclusive。

| 状态 | 解释 |
| ---- | ---- |
proven | assertion pass 证实，即穷尽状态空间也找不到任何一个反例。|
falsified | assertion fail 证伪，即出现不满足断言的反例。|
inconclusive | 不确定，即在指定的 N 个 cycle depth 内，没有找到反例，属于有界证明。|
vacuous | 空成功，对于包含蕴含操作符|-> |=>的property，如果 antecedent（先行算子）一直未被触发，因此一定不会出现反例，此时为 vacuous success。 |
covered | 覆盖 |
uncovered | 未覆盖 |

### 设计文件

第一种方法是在 SVA 文件夹中创建 .sva 文件，创建一个模块，并在其中编写 SVA 断言。然后需要创建另一个 binder 文件，将 SV 设计和 SVA 断言模块绑定在一起。

第二种方法是在 SV 代码中内联编写 SVA 断言。这通常更快，也可能更容易，尽管在编写大量断言时不太实用。

为了简化操作，这里将介绍如何在 SV 设计中内联编写断言。性。在这些情况下，该应用程序将提供 **有界证明** 性。在这些情况下，该应用程序将提供 **有界证明** （Bounded Proof）结果，表明在初始状态的**特定深度**内无法找到反例。对于一组断言的证实结果意味着在给定约束下，这些属性不可能为假。

![fpv1](https://cdn.jsdelivr.net/gh/easyformal/easyformal-site@master/content/zh/formal_app/fpv/image/fpv1.png)
（Bounded Proof）结果，表明在初始状态的**特定深度**内无法找到反例。对于一组断言的证实结果意味着在给定约束下，这些属性不可能为假。

![fpv1](https://cdn.jsdelivr.net/gh/easyformal/easyformal-site@master/content/zh/formal_app/fpv/image/fpv1.png)


第二种方法是在 SV 代码中内联编写 SVA 断言。这通常更快，也可能更容易，尽管在编写大量断言时不太实用。

为了简化操作，这里将介绍如何在 SV 设计中内联编写断言。性。在这些情况下，该应用程序将提供 **有界证明** 性。在这些情况下，该应用程序将提供 **有界证明** （Bounded Proof）结果，表明在初始状态的**特定深度**内无法找到反例。对于一组断言的证实结果意味着在给定约束下，这些属性不可能为假。

![fpv1](https://cdn.jsdelivr.net/gh/easyformal/easyformal-site@master/content/zh/formal_app/fpv/image/fpv1.png)
（Bounded Proof）结果，表明在初始状态的**特定深度**内无法找到反例。对于一组断言的证实结果意味着在给定约束下，这些属性不可能为假。

![fpv1](https://cdn.jsdelivr.net/gh/easyformal/easyformal-site@master/content/zh/formal_app/fpv/image/fpv1.png)


### TCL 文件
性。在这些情况下，该应用程序将提供 **有界证明** （Bounded Proof）结果，表明在初始状态的**特定深度**内无法找到反例。对于一组断言的证实结果意味着在给定约束下，这些属性不可能为假。

![fpv1](https://cdn.jsdelivr.net/gh/easyformal/easyformal-site@master/content/zh/formal_app/fpv/image/fpv1.png)

接下来，我们将设置 TCL ⽂件。下⾯是 TCL ⽂件（图 5），您可以将其⽤作 VC Formal 上功能检查的模板。

<!-- ![alt text](Aspose.Words.92948dad-5a71-4124-9ea8-5b069112310c.006-1.jpeg) -->


1. 指令：在 VC Formal 中设置应用模式为 FPV。
2. 设计文件中建立的主模块名称。
3. 需要指定设计文件的位置，以便 VC Formal 能够找到它。
4. 使用 “+define+INLINE_SVA” 让 VC Formal 知道我们将在设计中使用内联 SVA。
5. 时钟信号的名称。你可以指定想要使用的周期（可以保持为 100）。
6. 设计中复位信号的名称，以及它是高电平有效还是低电平有效。

### 编写断言

现在，为了使用 FPV 应用程序，我们需要在设计中编写要测试的断言。断言使用 SVA（SystemVerilog Assertions）编写。在本教程中，我们将编写内联 SVA 断言。

在你的 SV 设计文件中，向下滚动到 `endmodule` 之前编写断言。你需要在编写断言之前使用“`ifdef INLINE_SVA”，在编写断言结束后使用“`endif”。下面是一个在我的 `fpv_example.sv` 文件底部、紧接着 “endmodule” 之前编写的 4 个内联断言的示例：

### 应用程序设置

有多种方法可以调用 VC Formal GUI 并加载 TCL 脚本。

- 调⽤VC Formal GUI，然后在应⽤程序中⼿动加载TCL 脚本：`$vcf-gui`
- 通过⼀个命令调⽤VC Formal GUI 和TCL 脚本：`$vcf -f run.tcl -gui` 或者 `$vcf -f run.tcl -verdi`

“run.tcl” 是我们正在使用的 TCL 文件的名称。如果你的文件名与此不同，你需要在这个命令中相应地进行更改。

“-gui” 开关在 GUI 中打开 VC Formal，相当于开关 “-verdi”。

### 运行

在成功调用 VC Formal GUI 并加载上述方法中的 TCL 脚本后，你的屏幕应该在 VCF:GoalList 选项卡中显示内容，看起来类似于这样：

<!-- ![alt text](Aspose.Words.92948dad-5a71-4124-9ea8-5b069112310c.021-1.jpeg) -->

在这里你应该看到列出了所有的输出。现在，继续点击 VCF:GoalList 选项卡左上角的运行图标来运行验证分析。

### 结果分析

<!-- ![alt text](Aspose.Words.92948dad-5a71-4124-9ea8-5b069112310c.023-1.jpeg) -->

在上图中，我们看到两个 √ 图标，表示两个断言都已经被证明。在我们的设计中，“check_out_S0”属性意味着S0的输出确实是2'b00，“check_state_S0”断言意味着确实当我们处于S0状态时，输入a为高电平时，下一个状态将会是我们在内联断言中指定的S1。

我们还有两个 × 图标，表示两个断言被证伪。在我们的设计中，“onehot_out”属性意味着信号“out”具有单热编码，即我们每次只有一个位为高电平“1”，但显然这在我们的设计中是错误的，因为我们正在使用二进制编码，而信号“out”可以是2'b11和2'b00。 VC Formal 证明了这个属性是错误的，正如预期的那样。

“check_out_S3”属性表示状态S3的输出信号“out”是2'b01，这显然是不正确的，因为在我们的设计中我们将该输出设置为2'b11，因此该属性被证伪。


### Debug 断言失败的属性

要查看是什么导致了被证伪的属性被证伪，双击图13中的第一个（第2行）。然后，你应该会看到一个生成的波形，类似于下面显示的波形

<!-- ![alt text](Aspose.Words.92948dad-5a71-4124-9ea8-5b069112310c.030-1.jpeg) -->

如果你双击 out[1:0] 信号，你会看到一些实例，其中 out[1] 和 out[0] 同时为 0 或 1，这不是正确的单热编码。

<!-- ![alt text](Aspose.Words.92948dad-5a71-4124-9ea8-5b069112310c.031-1.jpeg) -->