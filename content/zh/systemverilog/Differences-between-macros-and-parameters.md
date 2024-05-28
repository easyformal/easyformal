---
title: "宏与参数的区别"

tags: "sv"

weight: 24
---

|**宏**|**参数**|
|------|----|
|宏是可替换的|参数类似于变量|
|宏在预编译阶段工作|参数在展开阶段工作|
|***语法：*** **`define 宏名 值**  |***语法：*** **parameter 参数名=值**|
|我们可以在任何文件中使用 **`define**|我们可以在文件内使用 **parameter**|
|我们不能在宏中指定数据类型|我们可以在参数中使用和更改数据类型|
|宏可以有多行|多行在这里是不可能的，因为参数就像声明变量|
|我们可以在命令行中为宏赋值|参数值不能在命令行中更改|

## 宏和参数的执行阶段

![Untitled Diagram drawio (44)](https://user-images.githubusercontent.com/110509375/208600538-05875a31-1ca6-43ad-b0b5-7840cda420c7.png)

### 在宏中使用参数
```
parameter data = 5; // 在展开阶段，data 将被替换为值 5
`define  DATA data // 在预编译阶段，`DATA 将被替换为 data
module tb();
  int a,b;
  initial begin
   
    $display("DATA=%0d",`DATA);
    
     
     b= `DATA + 2;  

    $display("b=%0d",b);
  end 
endmodule
```   
在上面的代码中，参数 data 的值 5 被用在宏 `DATA 中    
### 在参数中使用宏  
```  
`define  DATA 25
parameter data = `DATA;

module tb();
  int a,b;
  initial begin
    $display("data=%0d",data); // 在预编译阶段，`DATA 将被替换为 25。
    $display("DATA=%0d",`DATA); // 在展开阶段，data 将被替换为值 25
    
     a = data +5;
     b= `DATA + 2;
    
    $display("a=%0d b=%0d",a,b);
  end 
endmodule  
``` 
在上面的代码中，宏 `DATA 25 的值被用在参数 data 中。

 **宏（`define）可以在命令行中使用**
 
`-timescale 1ns/1ns +define+name=20`

```
`define name 10

module hi;
int a;
initial begin
  $display("[%0t] a = %0d",$time,a);
  #1 a = `name;
  $display("[%0t] a = %0d",$time,a);
end
endmodule
```
我们可以使用编译选项 `-timescale 1ns/1ns +define+name=20` 在命令行接口中为宏提供输入。