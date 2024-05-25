---
title: "Control-Flow-Interview-questions"
categories: ["SystemVerilog"]
tags: "sv"

weight: 22
---


**1. How to call the task inside a function? giving example with code.**

**Answer**: Normally a function can be called inside the task.    
but calling the task inside the function is illegal because task has delay elements and the function does not have any delay elements but there is an exceptional case for it, by using the fork and join_none  inside the function to call the task is possible.  

Please refer to the below code for a better understanding.  

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

 The below figure shows the output of calling a task inside a function.    
![Untitled Diagram drawio (9)](https://user-images.githubusercontent.com/106074838/188474636-7609f629-5713-46ff-bfbd-c9753fd06b99.png)  

                      Figure.1. output of calling a function inside a task. 

In the above screenshot calling a task inside a function. In the initial module, function is called and printed I'm in function and called task inside fork join_none and in task printed I'm in task after 5 ns and returned back to function and then to initial block.  

***
     
**2. What is the output of the following snippet?** 

         int x=2;
         repeat(x);
         begin
         x++;
         $display("The value of x is %0d", x);
         end


**Answer**: Repeat loop is used for repeating statements or operations for a fixed number of times.    
Here initially they give the value of x is 2 and later x value changed inside repeat, In repeat once x is taken and doesn't bother if x value changes i.e., initially x value evaluates and and doesn't evaluate again.       
So Statements will be repeated 2 times        

Please refer to the below code for a better understanding.  

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

The below figure shows the output of the Repeat loop.  
![Untitled Diagram drawio (4)](https://user-images.githubusercontent.com/106074838/188260448-466387c0-54b6-4767-b421-9bb4b83d0a9e.png)   

                          Figure.2. the output of the Repeat loop. 
***


**3. What is a dead code in System Verilog?**    
**Answer**: Dead code is a piece of code that doesn't run at all & which doesn't affect your simulation or synthesis.  
The code which has code coverage of 0% i.e., the final product chip there will be some hardware generated in the chip which has never been used in the lifetime.  
Examples of dead code are signals that are never used or conditions that are never triggered in code.  


***

**4. Can we use a local variable of the foreach loop outside of the loop as shown in following snippet ? why?**  

                        int array[5]  
                        foreach(array[i])  
                        begin  
                        array[i]=i;  
                        $display("\tarray[%0d]=%0d",i,array[i]);  
                        end  
                        $display("value of i out of loop: %0d",i);
                        $display(" out of loop ");  

**Answer** : No    

In the foreach loop we can use some random variable let's say i as a variable and i variable can't be used outside the inside module because i refers to only foreach block itself (using the scope resolution operator is also not possible). It will show the error as shown in the following fig.  
  
![for error1](https://user-images.githubusercontent.com/110412468/189056740-5458aa77-8fc5-4fa7-bdcd-7c34d21143db.png)

***


**5.How to use the forever loop using for loop ?**   
Normally As the name says forever loop will execute the statements inside the loop forever.  
It can be said as infinite iterations. so if we use that simulator will hang.  
But there are 2 ways to stop the forever loop.  
1. using if conditions and break statements inside the for a loop.  
2. $finish.

* **using if conditions and break statements to terminate the loop**    
 
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

In the above code we use the for loop syntax to do the forever loop and inside the for loop **i=0;1;i++** The condition is 1 which means true all the time. so for loop never terminate means for is acting as forever loop. so to stop it we are using the if condition and break statements in if condition when i=5 then break so it will work from 0 to 5 iterations and then exits the loop.  
  
* **using $finish to terminate the forever loop**  

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

Here also the same example as we used in the previous example but here we are using a $finish statement to terminate the loop.   

The below figure shows the output of the forever loop   
![Untitled Diagram drawio (6)](https://user-images.githubusercontent.com/106074838/188388444-a9060788-1faa-4cf9-b4b8-796d41bb6c70.png)  
   
                          Figure.3. the output of the forever loop.  

***
  
 
**6.What is difference between pre-increment(++i) and post-increment(i++) in SV** ?  
**Answer** :   
let's say we are assigning a variable j with the use of i variable incrementing by post and pre-increment methods.

In pre-increment(++i) will increment the value of i and then the assignment will happen to the LHS i.e., to j.  

In post-increment(i++) will increment the value of i and after the assignment to LHS. 


for a better understanding look at the below code   
* pre-increment code
 
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
 
In the above code, i value is initially 1 and we assign i value to j with pre-incrementing (++i).   

* post increment code  

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
In the above code, i value is initially 1 and we assign i value to j with post-incrementing (i++); 
 
The below figure shows the output of both pre-increment and post-increment 
  
![post pre](https://user-images.githubusercontent.com/110412468/189060002-f1516383-89bf-4ff9-ba14-39e71d11e2b4.png)

                     Figure.4. the output of both pre-increment and post-increment.  
  

*** 

**7. How will the break keyword work inside the repeat loop?**  

**Answer** :  
  we use the break keyword inside the repeat loop it will terminate the loop. and based on certain conditions.   

for a better understanding look at the below code  

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


In the above code is repeat loop with a break keyword, we are declaring the repeat loop it will be executed 5 times but using the break keyword to terminate the loop in a 3rd iteration and comes out of the repeat loop completely.
       
The below figure shows the output of the repeat loop with the break keyword.   
![Untitled Diagram drawio (8)](https://user-images.githubusercontent.com/106074838/188438380-83f8c068-78f3-491d-9dec-3c1c5febf12c.png)   

                                Figure.5. the output of the repeat loop with the break keyword. 


**8.what is the difference between If and unique if statements**?    
Answer :  
|SL.No|condition | if |unique if |
|-----|----------|----|----------|
|1.|only one conditional expression is true|execute the set of statements inside the true conditional block|execute the set of statements inside the true conditional block|  
|2.|more than one conditional expression is true|executes the first true conditional block statements|executes the first true conditional block statements and also displays a warning that multiple conditions were true which leads to a dead code|
|3.|none of the conditional expressions is true without else|comes out of the if-else block|comes out of the unique if block and shows a warning that none of the conditions is true|  



***

**9. what is the difference between If and priority if statements**?  
Answer :  
|SL.No|condition | if |priority if |
|-----|----------|----|----------|
|1.|only one conditional expression is true|execute the set of statements inside the true conditional block|execute the set of statements inside the true conditional block|  
|2.|more than one conditional expression is true|executes the first true conditional block statements, no warning display|executes the first true conditional block statements, no warning display|
|3.|none of the conditional expressions is true without else|execute the statements out from the conditional block, no warning display|execute the statements out from the conditional block, a warning display|  
 

   



