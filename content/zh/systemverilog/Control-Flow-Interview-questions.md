---
title: "常见控制流相关问题"

tags: "sv"

weight: 22
---


**1. 如何在函数内部调用任务？给出带有代码示例的例子。**

**答案**：通常情况下，可以在任务内部调用函数。但是在函数内部调用任务是非法的，因为任务具有延迟元素，而函数没有任何延迟元素，但有一个特殊情况，可以在函数内部使用 fork 和 join_none 调用任务。

请参考下面的代码以获得更好的理解。 

``` 
module test;
initial begin
  void'(function_name);
end

function function_name;
  $display($time, "\t I'm in function");
  fork
    $display("\t calling task inside a function");
    task_name;
  join_none
endfunction

task task_name;
  #5 $display($time,"\t Now I'm in task");
endtask

endmodule

 ```

下图显示了在函数内部调用任务的输出。

在上述截图中，调用了一个函数内部的任务。在初始模块中，调用了函数并打印了"I'm in function"，然后在 fork join_none 中调用了任务，在任务中打印了"I'm in task after 5 ns"，然后返回到函数，最后返回到初始块。

***
     
**2. 以下片段的输出是什么？**

         int x=2;
         repeat(x);
         begin
         x++;
         $display("The value of x is %0d", x);
         end

**答案**：重复循环用于重复语句或操作固定次数。  

这里最初给出了 x 的值为 2，后来在重复循环内部更改了 x 的值。在重复循环中，x 的值只被获取一次，并且不关心 x 的值是否改变，即最初会评估 x 的值，然后不会再次评估。因此，语句将重复执行 2 次。

请参考下面的代码以获得更好的理解。

                    module test;    
                    int x=2;      
                    initial begin      
                    $display("output of repeat(x=2) is ");     
                    repeat(x)     
                    begin    
                    $display("The value of x is %0d", x);   
                    x++;  
                   end    
                   end  
 
                   endmodule   

下图显示了重复循环的输出。

![Untitled Diagram drawio (4)](https://user-images.githubusercontent.com/106074838/188260448-466387c0-54b6-4767-b421-9bb4b83d0a9e.png)   

***


**3. 在 System Verilog 中，什么是死代码？**  
**答案**：死代码是一段根本不运行的代码，它不会影响您的仿真或综合。代码覆盖率为 0% 的代码，即在最终的产品芯片中，芯片中生成了一些从未在其生命周期中使用过的硬件。  
死代码的示例包括从未使用过的信号或代码中从未触发的条件。


***

**4. 我们可以像下面的代码片段中所示在 foreach 循环外部使用循环的局部变量吗？为什么？**
                        int array[5]  
                        foreach(array[i])  
                        begin  
                        array[i]=i;  
                        $display("\tarray[%0d]=%0d",i,array[i]);  
                        end  
                        $display("value of i out of loop: %0d",i);
                        $display(" out of loop ");  

**答案** : 不可以    

在 foreach 循环中，我们可以使用一些随机变量，比如 i 作为变量，但是 i 变量不能在内部模块之外使用，因为 i 只引用于 foreach 块本身（使用作用域解析运算符也不可能）。如下图所示，将显示错误。

![for error1](https://user-images.githubusercontent.com/110412468/189056740-5458aa77-8fc5-4fa7-bdcd-7c34d21143db.png)

***


**5. 如何使用 for 循环实现 forever 循环？**  
通常，正如其名称所示，forever 循环将永远执行循环内的语句。可以说是无限次迭代。因此，如果使用它，模拟器将会挂起。但是有两种方法可以停止 forever 循环。  
1. 使用 for 循环内的 if 条件和 break 语句。  
2. $finish。

* **使用 if 条件和 break 语句终止循环**   
 
                        module forever_for;  
                        initial begin  
                        $display("-----forever loop output ---");  
                        for (int i=0;1;i++)  
                        begin  
                        #4  $display("\t value of i = %0d", i);  
                        if(i==5)  
                        break;  
                        end 
                        end    
                        endmodule    

在上面的代码中，我们使用 for 循环的语法来实现 forever 循环，在 for 循环内部，条件为 **i=0;1;i++**，条件始终为 true。因此，for 循环永远不会终止，也就是说 for 循环充当了 forever 循环。为了停止它，我们在 if 条件中使用 if 条件和 break 语句，当 i=5 时，就会执行 break，这样循环将从 0 迭代到 5 迭代，然后退出循环。

* **使用 $finish 终止 forever 循环** 

                  module forever_for;`  
                 initial begin  
                 $display("-----forever loop output ---");  
                 for (int i=0;1;i++)    
                 begin  
                 #4  $display("\t value of i = %0d", i);    
                 end   
                 end  
                 initial begin   
                 #21 $finish;  
                 end    
                 endmodule 

这里也是与之前示例相同的示例，但在这里我们使用了 $finish 语句来终止循环。

下图显示了 forever 循环的输出。
![Untitled Diagram drawio (6)](https://user-images.githubusercontent.com/106074838/188388444-a9060788-1faa-4cf9-b4b8-796d41bb6c70.png)  
   

***
  
 
**6. 在 SystemVerilog 中，前增量（++i）和后增量（i++）有什么区别？**  
**答案**：
假设我们正在使用 i 变量通过后置递增和前置递增方法来给变量 j 赋值。

在前置递增(++i)中，会先递增 i 的值，然后才会将值赋给左侧的变量 j。

在后置递增(i++)中，会先将值赋给左侧的变量 j，然后再递增 i 的值。

为了更好地理解，请看下面的代码示例：  
* 前置递增代码
 
                module pre_increment; 
                int i=1;  
                int j;  
                initial begin  
                $display("\t the value of i is %0d",i);  
                $display("\t the value of j is %0d",j);  
                $display("\t assigning i value to j");  
                j=++i;  
                $display("\t after pre-increment the value j is %0d",j);  
                end  
                endmodule     
 
在上面的代码中，i 的初始值为 1，我们使用前置递增 (++i) 将 i 的值赋给了 j。

* 后置递增代码 

                module post_pre;  
                int i=1;  
                int j;  
                initial begin  
                $display("\t the value of i is %0d",i);  
                $display("\t the value of j is %0d",j);  
                $display("\t assigning i value to j");  
                j=i++;  
                $display("\t after post-increment the value j is %0d",j);  
                end    
                endmodule  
在上面的代码中，i 的初始值为 1，我们使用后置递增 (i++) 将 i 的值赋给了 j。

下面的图显示了前置递增和后置递增的输出结果：

![post pre](https://user-images.githubusercontent.com/110412468/189060002-f1516383-89bf-4ff9-ba14-39e71d11e2b4.png)

---

**7. repeat 循环中的 break 关键字如何工作？**

**答案**：我们在 repeat 循环中使用 break 关键字时，它会终止循环，具体取决于某些条件。

为了更好地理解，请看下面的代码：

          module repeat_break;  
          int b;  
          initial begin  
          repeat(5) begin   
          $display("\tVlaue of b=%0d",b);  
          b++;  
          if(b==3)  
          break;  
          end 
          end    
          endmodule 


在上面的代码中，使用了带有 break 关键字的 repeat 循环。我们声明了 repeat 循环将执行 5 次，但在第 3 次迭代中使用了 break 关键字来终止循环，并完全退出 repeat 循环。

下面的图显示了带有 break 关键字的 repeat 循环的输出结果。
![Untitled Diagram drawio (8)](https://user-images.githubusercontent.com/106074838/188438380-83f8c068-78f3-491d-9dec-3c1c5febf12c.png)

**8. if 语句和 unique if 语句之间有什么区别？**  
答案：  
| 序号 | 条件 | if 语句 | unique if 语句 |
|-----|----------|----|----------|
| 1 | 只有一个条件表达式为真 | 执行真条件块中的语句集 | 执行真条件块中的语句集 |
| 2 | 多个条件表达式为真 | 执行第一个真条件块中的语句 | 执行第一个真条件块中的语句，并显示警告，指示多个条件为真，导致死代码 |
| 3 | 没有条件表达式为真且没有 else | 退出 if-else 块 | 退出 unique if 块并显示警告，指示没有条件为真 |

**9. if 语句和 priority if 语句之间有什么区别？**  
答案：  
| 序号 | 条件 | if 语句 | priority if 语句 |
|-----|----------|----|----------|
| 1 | 只有一个条件表达式为真 | 执行真条件块中的语句集 | 执行真条件块中的语句集 |
| 2 | 多个条件表达式为真 | 执行第一个真条件块中的语句，不显示警告 | 执行第一个真条件块中的语句，不显示警告 |
| 3 | 没有条件表达式为真且没有 else | 执行条件块外的语句，不显示警告 | 执行条件块外的语句，显示警告 |

   



