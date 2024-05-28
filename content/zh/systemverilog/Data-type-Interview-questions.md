---
title: "常见数据类型相关问题"

tags: "sv"

weight: 23
---

**1. SystemVerilog 中 reg 和 logic 有什么区别吗？**

**答案：**  
在 SystemVerilog 之前，Verilog 主要用于综合和验证目的。在 Verilog 中有两种主要的数据类型：
 1) reg  
 2) net  
reg 主要在需要将值存储在变量中的时候使用，通常在过程块内部使用，主要用于设计时序电路。而 net 变量主要是连续驱动的，在 net 类型变量中无法存储值。

因此，设计者在选择使用 reg 还是 net 时常常会感到困惑。这个问题在 SystemVerilog 中得到了改善。SystemVerilog 引入了一种新的数据类型，称为 “logic”。这种数据类型可以在两种情况下使用，既可以用作 net 也可以用作 reg。

**2. SystemVerilog 中 logic[7:0] 和 byte 变量有什么区别？**

**答案：**
* 'byte' 是一个有符号变量，这意味着它只能用于计数到 127。
* logic[7:0] 变量可以用作无符号的 8 位变量，计数范围可以达到 255。
**3.两态和四态的区别是什么？**

**答案：**
* 四态包括逻辑0、逻辑1、逻辑X（与寄存器相关，在现实世界中它被声明为0或1，具体取决于我们使用的模拟器）和逻辑Z（与线相关）。
* 两态仅包括逻辑1和逻辑0。

**4.打包数组和非打包数组的区别**

**答案：**
请参阅以下链接  
https://github.com/muneeb-mbytes/SystemVerilog_Course/wiki/02.Array#static-arrays-has-two-types

**5.固定数组、动态数组、关联数组和队列的比较**

**答案：**
请参阅以下链接  
https://github.com/muneeb-mbytes/SystemVerilog_Course/wiki/Choosing-an-array

**6.给定一个大小为100的动态数组，如何在保留前100个元素的情况下，将数组重新调整为200个元素的大小？**

**答案：**
一个动态数组需要使用new[]进行内存分配以容纳元素。以下是一个将初始大小为100的整数数组扩展到200个元素的示例。
   
    // 声明动态数组。
    integer addr[]; 
    // 创建一个100元素的数组。
    addr = new[100]; 
    ………
    // 在保留先前值的情况下，将数组大小加倍。
    addr = new[200](addr);

**7.打包结构和非打包结构的区别**
**答案：**  
请参阅以下链接  
https://github.com/muneeb-mbytes/SystemVerilog_Course/wiki/03.Structure-and-Union#types-of-structure

**8.如果在枚举数据类型中为不同的名称赋予相同的值，会发生什么？**

**答案：**   
代码：
      
      module set_value_enum;    
      // 为 MONDAY 和 TUESDAY 赋相同的值
       enum {MONDAY=2, 
             TUESDAY=2, 
             WEDNESDAY=5, 
             THURSDAY, 
             FRIDAY=10, 
             SATURDAY, 
             SUNDAY }days;


输出：       

      Error: Enum member 'TUESDAY' does not have unique value.

**9.如果将 bit 声明为有符号并赋值为 11xz11x1，会输出什么？**

**答案：**   
代码：   

      module data_type_bit;   
        // 声明 bit 为 8 位有符号类型
        bit signed [7:0] data = 8'b11xz11x1;   

        initial begin
           $display("\nValue of bit signed data in binary = %0b",data);
           $display("Value of bit signed data in decimal = %0d\n",data);
        end
      endmodule: data_type_bit


输出：   

      Value of bit signed data in binary = 11001101
      Value of bit signed data in decimal = -51

在上述代码中，我们将 bit 声明为 11xz11x1，这里 bit 是两态的，因此 x 和 z 被转换为 0，结果为 11001101。由于它是有符号的，十进制表示为 -51。

**10.如果将 byte 声明为无符号并赋值为 11101001，会输出什么？**

**答案：**   
代码： 

      module data_type_byte;   
         // 声明 byte 为 8 位无符号类型
         byte unsigned data = 8'b11101001;

         initial begin
           $display("\nValue of byte unsigned data in binary = %0b",data);
           $display("Value of byte unsigned data in decimal = %0d\n",data);
         end
      endmodule: data_type_byte


输出：   

      Value of byte unsigned data in binary = 11101001
      Value of byte unsigned data in decimal = 233

在上述代码中，我们将 byte 声明为 11101001。由于它是无符号的，十进制表示为 233。