---
title: "进程常见问题"

tags: "sv"

weight: 26
---

1. **fork_join、fork_join_any 和 fork_join_none 之间的区别**

|   **fork_join**  |          **fork_join_any**                         |                    **fok_join_none**                |                    
|:---------------|:------------------------------|:------------------------|
|在 fork_join 中，主（父）线程在 fork_join 中的所有线程（子线程）执行完毕后才会执行|在 fork_join_any 中，如果任何一个子线程执行，则主（父）线程执行| 在 fork_join_none 中，子线程和主（父）线程同时执行 |
| <img width="122" alt="fork-join  " src="https://user-images.githubusercontent.com/110509375/189809970-21ee4efe-78b7-4974-99ae-7471c4a56df4.png">|   <img width="116" alt="fork_join_any" src="https://user-images.githubusercontent.com/110509375/189810277-5c297e3c-97f7-406f-b7bf-115694df24cd.png">|<img width="130" alt="fork-join_none" src="https://user-images.githubusercontent.com/110509375/189810446-361a0b82-1f33-4f2c-bf91-f0da2b0893c3.png">|

2. **我们可以在 fork_join 中使用 wait_fork 吗？**

我们知道，在 fork_join 中，仅当 fork_join 中的所有线程执行完毕时，主线程才会执行，因此不需要使用 wait_fork。
我们可以在 fork_join_any 或 fork_join_none 语句后使用 wait fork，以等待 fork-join_any 或 fork_join_none 中的所有线程完成。
因此，在 fork_join 中不需要 wait_fork。

3. **阻塞和非阻塞赋值的区别**

|**阻塞** |**非阻塞** |
|:-------------|:----------------|
|在阻塞赋值中，一条语句执行完毕后，下一条语句将执行，即右侧表达式的第一个表达式被评估并立即分配给左侧变量|在非阻塞赋值中，对当前时间单位的所有右侧表达式进行评估，并在时间单位结束时分配给左侧变量|
|由 " *=* " 表示|由 " <= " 表示|
|它按顺序执行|它并行执行|
|阻塞用于组合逻辑|非阻塞用于时序逻辑|
|<img width="247" alt="bloc" src="https://user-images.githubusercontent.com/110509375/189839938-8dcace4c-74e6-4d85-8d49-59cdaf8c452d.png">|<img width="243" alt="non-b" src="https://user-images.githubusercontent.com/110509375/189839580-5c79c0e5-6f69-4a04-a201-d0fccc208142.png">|

4. **wait event 和 @ event 之间的区别**

如果我们在相同的延迟下触发 wait 和 @，那么 wait 语句会被执行，因为 wait 捕获速度比 @ 快。

5. **我们可以使用不同延迟执行 wait 和 @ 吗？**

```systemverilog
module tb;    
    event e;  
    initial begin  
    #20 ->e;  
   $display($time,"thread1");  
   end  
  initial   
    begin  
     #25 @e;  
     $display($time,"thread2");  
    end  
  initial   
   begin  
   #15 wait(e.triggered);  
   $display($time,"thread3");  
   end 
endmodule  
```

在上面的例子中，我们可以看到事件、wait 和 @ 的延迟是不同的。我们还可以看到这里 @ 的延迟大于事件的延迟，而 wait 的延迟小于事件的延迟，所以这里只有 wait 语句与事件的延迟一起执行。因此，在下面的图中，我们可以看到线程 1 和线程 3 使用相同的延迟（#20）执行。

![Untitled Diagram drawio (7)](https://user-images.githubusercontent.com/110509375/189864228-3e5d8427-8c2d-4a81-a422-2eda692f860e.png)