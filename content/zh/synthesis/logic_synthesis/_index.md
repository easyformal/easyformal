---
weight: 1
title: "逻辑综合"
description: "HDL design."


---

### 什么是逻辑综合？

![img](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/easyformal/0_0_synthesis.png)

### 逻辑综合流程

1. 翻译：将 RTL 在约束下转换成通用门级电路（如 GTECH），该通用门级电路无时序和载荷信息
2. 逻辑优化：对通用门级电路进行优化
3. 门级映射：将优化过的通用门级电路，用工艺厂商的工艺库把电路映射出来，得到网表文件、时序约束信息、延时信息等。

---

{{< treeview
  display="tree"
/>}}
