---
title: "接口常见问题"

tags: "sv"

weight: 25
---
1. 时钟块如何处理同步复位？

解决方案：

同步复位仅在时钟事件下采样。当复位被使能时，它只在下一个活动时钟边沿之后生效。

```systemverilog
clocking block TB_CB @(posedge clk);
default input #1 output #0;
input rst;
output a, b;
input y;
```

**注意：** 时钟块仅设计用于处理同步复位 - 它应该仅基于时钟事件发生。时钟块中的复位应该在其他地方处理。如果时钟块的输出是变量，则可以在复位时在时钟块外部对其进行过程赋值。

---

2. modport 如何处理异步复位？

解决方案：

异步复位

在异步复位中，采样独立于时钟。当启用复位时，它会立即生效，在同一时钟边沿内生效。

在这个例子中有三个信号a、b和c。'a'是连续信号且是异步的。'b'和'c'是同步信号。使用modport，我们可以将异步信号'a'改为同步信号。

```systemverilog
modport TB_MP(TB_CB, output a);
```

**注意：** 在 modport 中，设计可以处理异步复位。异步复位将在任何时候发生。

---

3. 什么是同步复位和异步复位？

解决方案：

同步复位意味着复位是相对于时钟进行采样的。换句话说，当启用复位时，它将在下一个活动时钟边沿之后才生效。

异步复位

在异步复位中，复位是独立于时钟进行采样的。这意味着，当启用复位时，它会立即生效，并且不会检查或等待时钟边沿。

---

4. 在采样和驱动信号中，阻塞和非阻塞赋值的用法是什么？

解决方案：

阻塞赋值（=）用于在活动区域中采样信号。
非阻塞赋值（<=）用于在活动非阻塞区域中驱动信号。
因此，我们可以避免 Verilog 中的竞争条件。

---

5. 使用 modport 限制信号的访问方式

解决方案：

接口文件（interface.sv）：

```systemverilog
interface and_intr;
  logic p, q;
  logic r;
  modport DUT_MP(input q, output r);
  modport TB_MP(output p, output q, input r);
endinterface : and_intr
```

测试文件（tb.sv）：

```systemverilog
module tb(and_intr inf);
  initial begin
    $display("// and gate output using modports\n");
    repeat(5) begin
      inf.TB_MP.p = $random;
      #1;
      inf.TB_MP.q = $random;
      #1;
      $display("input_p=%b\t input_q=%b\t output_r=%b", inf.TB_MP.p, inf.TB_MP.q, inf.TB_MP.r);
    end
  end
endmodule : tb
```

设计文件（design.sv）：

```systemverilog
module and_gate(and_intr inf);
  assign inf.DUT_MP.r = (inf.DUT_MP.p) & (inf.DUT_MP.q);
endmodule : and_gate
```

输出：

当输入信号 'p' 不在 DUT_MP（设计 modport）中但该信号在测试中被调用以生成输出时，会出现错误，如下图所示：

![access_restricting_modports](https://user-images.githubusercontent.com/110448056/193022636-f48a0246-1d66-4741-b3a6-130f6e02eac0.png)

---

6. 如果在接口时为输出赋值，会发生什么？

解决方案：我们会收到一个警告，因为其中一个赋值是隐式的，但我们会得到正确的输出。

![inter_error](https://user-images.githubusercontent.com/110443214/193393454-5b2a0429-3c8f-434c-8777-86e183924d50.png)

