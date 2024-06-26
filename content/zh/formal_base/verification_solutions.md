---
weight: 2
title: "形式验证解决方案：从入门到签核（Signoff）"
description: "形式验证解决方案：从入门到签核（Signoff）"

---
<!--

https://www.systemverilog.io/verification/blueprint-for-formal-verification/

https://blog.csdn.net/Holden_Liu/article/details/124387333


https://singularitykchen.github.io/blog/2021/01/29/Glean-Formal-Signoff/

## 蓝图

对于任何验证方法来说，成功的关键在于制定可靠的策略。为此，这里提供一个形式化验证的解决方案。这是一个模板或验证计划，您可以在执行形式化验证时可以借鉴参考。

该解决方案简单分为 3 个步骤：

- 熟悉工具和 DUT

对形式验证来说，花一些时间来熟悉工具和待测设计（DUT）很重要，这一步视为热身。

- 属性验证（FPV）

写 SVA 不是一件容易的事，这里将是耗费大量时间的地方，测试计划在此步骤中创建和实施。

- 签核（Sign-off）

Sign-off 的最终目的是确保“你所写的内容”确实涵盖了你预期的所有场景。

## 详细策略

### 步骤 1：熟悉该工具和 DUT

1. 创建正式的测试台外壳

2. 使用该工具自动检测组合循环、算术溢出和数组超出范围索引

3. 使用工具自动检测无法访问的死代码

### 步骤 2：熟悉该工具和 DUT




是否有足够的属性来检查并覆盖所有的设计？
是否存在由于不正确的约束而导致的无效证明？
该设计是否已经经过足够深度的序列分析？
设计的哪些部分真正得到了验证？
断言可以捕获设计中所有可能的错误吗？

-->