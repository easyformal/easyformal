---
title: "Processes-InterviewQuestions"

tags: "sv"

weight: 26
---

**1.Difference between fork_join,fork_join_any and fork_join_none ?**
|   **fork_join**  |          **fork_join_any**                         |                    **fok_join_none**                |                    
|:---------------|:------------------------------|:------------------------|
|In fork_join the main(parent) threads will execute after all the threads(child threads) in the fork_join is executed|In fork_join_any the main(parent) threads executes if any one of the child threads excecutes | In fork_join_none child threads and main(parent) threads are executed parallely |
| <img width="122" alt="fork-join  " src="https://user-images.githubusercontent.com/110509375/189809970-21ee4efe-78b7-4974-99ae-7471c4a56df4.png">|   <img width="116" alt="fork_join_any" src="https://user-images.githubusercontent.com/110509375/189810277-5c297e3c-97f7-406f-b7bf-115694df24cd.png">|<img width="130" alt="fork-join_none" src="https://user-images.githubusercontent.com/110509375/189810446-361a0b82-1f33-4f2c-bf91-f0da2b0893c3.png">|

**2.Can we use wait_fork in fork_join ?**

We know that in fork_join  the main thread is executed only when all the threads in fork_join is executed so here no need of using wait_fork.
We can use wait fork after the fork_join_any or fork_join_none  statement to wait for all the threads in the fork-join_any or fork_join_none to complete.
So here in fork_join wait_fork is not required.

**3.Difference between Blocking and Non-Blocking Assignments ?**
|**blocking** |**Non-Blocking** |
|:-------------|:----------------|
|In Blocking Assignment one statement is executed then the next statement will executed i.e first expression of right hand side is evaluated and immediately assigned to the left hand side variable|In Non-Blocking assignment evaluated all the right hand side expression for the current time unit and assigned to left-hand side variable at the end of the time unit|
|Represented by  " *=* "|Represented by " <= "|
|It executes sequentially |It executes parallely|
|Blocking is used in combinational|Non-Blocking is used in sequential|
|<img width="247" alt="bloc" src="https://user-images.githubusercontent.com/110509375/189839938-8dcace4c-74e6-4d85-8d49-59cdaf8c452d.png">|<img width="243" alt="non-b" src="https://user-images.githubusercontent.com/110509375/189839580-5c79c0e5-6f69-4a04-a201-d0fccc208142.png">|

**4.Difference between wait event and @ event ?**

If we triggered wait and @ in the same delay then wait statement is executed because the wait catches fast then @ event.

**Example:**

    module tb;  
      event e;  
     initial begin  
            #20 ->e;  
            $display($time,"thread1");  
            end  
      initial   
        begin  
         #20 @e;  
         $display($time,"thread2");  
        end  
      initial   
       begin  
       #20 wait(e.triggered);  
       $display($time,"thread3");  
       end 
    endmodule   

In the above example we can see that we have same delay for the event,wait and @ but after executing the event wait is executed eventhough we declared '@' in between, its because wait catches the fast then the @ so as a output we can only see thread1 and thread3.

![Untitled Diagram drawio (6)](https://user-images.githubusercontent.com/110509375/189859603-70f3ae62-381a-42ad-945b-682dfd577522.png)

**5.Can we execute wait and @ with different delays ?**
     
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

In the above example we can see that delays are different for event ,wait and @.We can also see that here @ delay is more than the event delay and wait delay is less than the event delay so here it executes only the wait statement with the event delay.So in the below figure we can see that thread 1 and thread 3 is executed with same delay(#20)

![Untitled Diagram drawio (7)](https://user-images.githubusercontent.com/110509375/189864228-3e5d8427-8c2d-4a81-a422-2eda692f860e.png)



 

