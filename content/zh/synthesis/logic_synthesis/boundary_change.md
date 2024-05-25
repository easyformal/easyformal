---
weight: 2
title: "综合优化技术：边界优化"
description: "HDL design."

---


边界优化是综合器跨层次边界进行优化的一种策略。 不同类型的边界优化如下：

1. 跨层级传播常量
2. 跨层级传播相同或相反的信号
3. 跨层级传播未连接的端口信息
4. 推动反向器跨越层级

在子模块输入有很多常量（VCC 和 GND）的情况下，传播可以减少面积。如下图：

![img](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/easyformal/boundary_opt.png)
