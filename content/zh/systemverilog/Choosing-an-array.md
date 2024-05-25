---
title: "Choosing-an-array"

tags: "sv"

weight: 21
---

## Fixed Arrays/Static Arrays:

**1) Fixed Arrays Over All Arrays:**

* Fixed Arrays execute faster than all other types of arrays (i.e Associative arrays, dynamic arrays, queues) because it will store in uninitialized data segment (bss segment) of memory, so it will consume less simulation time to execute. And the heap is the segment where dynamic, associative and queue  memory allocation takes place.  
The below figure shows the memory layout.

![memory_segments](https://user-images.githubusercontent.com/110448056/188258335-c478ebd9-4785-415d-9873-cbf6269eecd4.png)

                                                Figure.1. Memory Layout

* Size is known previously then we can choose fixed arrays over all other types of arrays.

## Dynamic Arrays:

**1) Dynamic Arrays Over Fixed Arrays:**

* Memories allocated at run time, so memory will not be wasted but in fixed array memories would be wasted when we don't know the actual size.
* We can easily add elements using new() method but in fixed arrays we can't add elements after the declaration.
* We can delete entire arrays after allocating the memories to it but in fixed arrays we can't delete the memories after declaring array.

**Note:** In fixed arrays, there is no methods like new(),delete().

**2) Dynamic Arrays Over Associative Arrays:**

* Indexing is in continues manner but in associative arrays it's non continuous. it's show in below figure.

![indexing](https://user-images.githubusercontent.com/110448056/188263690-151e9f91-36a8-4b2e-baf6-42505a1f7fc1.png)

                                 Figure.2. continuous and non continuous indexing

* We can find relation between indexing so travelling through arrays with ease using loops but in associative arrays keys are required.

**3) Dynamic Arrays Over Queues:**

* According to footprint, dynamic array executes faster and it will required less simulation time but queues will required more time to execute.

![dynamic-1](https://user-images.githubusercontent.com/110448056/188267346-99ee256d-34b8-4946-92b6-4ecf42d368d4.png)

                             Figure.3. dynamic array execution time

![queue-1](https://user-images.githubusercontent.com/110448056/188267352-d9bba19f-cf55-41fa-8646-2b907fabcb67.png)

                             Figure.4. queue execution time

* Dynamic array used to trace the each and every element in the array using the loops but in queue with pop and push operation. we can't trace the all element of array using fooreach loop. for pop operation the size of the array is decrement and i is increment and for push operation the size of the array is increment and i is also increment it result in Killing process. 
 
code:

      module dynamic;
      int dyn [];
      initial
      begin
      dyn = new[8];
      dyn = '{2,7,0,6,1,9,9,7};
      foreach(dyn[i])begin
        $display("verifying the values of each index, dyn[%0d] = %0d", i, dyn[i]);
      end
      end
      endmodule 

OUTPUT:  

     run -all
     verifying the values of each index, dyn[0] = 2  
     verifying the values of each index, dyn[1] = 7  
     verifying the values of each index, dyn[2] = 0  
     verifying the values of each index, dyn[3] = 6  
     verifying the values of each index, dyn[4] = 1  
     verifying the values of each index, dyn[5] = 9  
     verifying the values of each index, dyn[6] = 9  
     verifying the values of each index, dyn[7] = 7  
     exit  

  
code: using pop operation

     `module queue;
     int que[$];
     initial
     begin
      que = '{2,7,0,6,1,9,9,7};
      foreach(que[i])begin
      $display("verifying the values of each index, que[%0d] = %0d", i, que[i]);
      que.pop_back();
      end
      end 
   endmodule

OUTPUT:  

     run -all  
     verifying the values of each index, que[0] = 2  
     verifying the values of each index, que[1] = 7  
     verifying the values of each index, que[2] = 0  
     verifying the values of each index, que[3] = 6  
     exit  

code: using push operation in queue

     module queue;
     int que[$];
     initial
     begin
     que = '{2,7,0,6,1,9,9,7};
     foreach(que[i])begin
     $display("verifying the values of each index, que[%0d] = %0d", i, que[i]);
         que.push_back(6);
     end
     end
     endmodule 

OUTPUT:

    `run -all
     verifying the values of each index, que[0] = 2
     verifying the values of each index, que[1] = 7
     verifying the values of each index, que[2] = 0
     verifying the values of each index, que[3] = 6
     verifying the values of each index, que[4] = 1
     verifying the values of each index, que[5] = 9
     verifying the values of each index, que[6] = 9
     verifying the values of each index, que[7] = 7
     verifying the values of each index, que[8] = 6
     verifying the values of each index, que[9] = 6
     verifying the values of each index, que[10] = 6
     verifying the values of each index, que[11] = 6
     verifying the values of each index, que[12] = 6
     Result reached the maximum of 5000 lines. Killing process.
     Execution interrupted or reached maximum runtime.

---

## Associative Arrays:

**1) Advantages of Associative array over all arrays:**

* The ****function exists(input index);**** method is used in the associative array to find the index element where this method is not used in associative and dynamic array.

code:  

    module associative;           
    int id[int];    
    int name[string];    
    int value[int];     
    initial    
    begin    
      id = '{4275:25, 4278:12};    
      name = '{"Dilip":12};    
      value = '{50000:25};  
      //check whether the name exist  
      name.exists("Dilip");  
      $display("the element exist= %0p", name);  
    end  
    endmodule 

OUTPUT:   
 
     run -all    
     the element exist= {"Dilip":12}   
     exit 

* In Dynamic array & queue  there is no such method like **function exists(input index)** to find the array element if we use  **function exists(input index);** in dynamic array the following error is occur

Dynamic array:    

code:  

      module dynamic;
      int dyn [];
      initial
       begin
      dyn = new[8];
      dyn = '{2,7,0,6,1,9,9,7};
      dyn.exists(0);
      $display("the element of the dyn=%0p", dyn);
      end
    endmodule 

OUTPUT:  

       -- Compiling module dynamic
        ** Error (suppressible): (vlog-13276) testbench.sv(7): Could not find field/method name (exists) in 'dyn' of 'dyn.exists'.  
        ** Error (suppressible): (vlog-13276) testbench.sv(7): Could not find field/method name (exists) in 'dyn' of 'dyn.exists.$0'.  
       End time: 02:35:57 on Sep 03,2022, Elapsed time: 0:00:00  
       Errors: 2, Warnings: 0  


Queue :

code:

    module queue;
     int que[$];
     initial
    begin
      que = '{2,7,0,6,1,9,9,7};
      que.exists(0);
      $display("the element of the que=%0p", que);
    end
  endmodule

OUTPUT:  

    -- Compiling module queue    
     ** Error (suppressible): (vlog-13276) testbench.sv(6): Could not find field/method name (exists) in 'que' of 'que.exists'.  
     ** Error (suppressible): (vlog-13276) testbench.sv(6): Could not find field/method name (exists) in 'que' of 'que.exists.$0'.  
     End time: 02:43:56 on Sep 03,2022, Elapsed time: 0:00:00
     Errors: 2, Warnings: 0

---

**2) Associative array over dynamic array and queue:**

* In Associative array the **index type is of any data_type** can used but while in dynamic and queue **we can't use different data types for indexing.**

code:

    module associative;
    int id[int];
    int name[string];
    int value[int];
    initial
     begin
      id = '{4275:25, 4278:12};
      name = '{"Dilip":12};
      value = '{50000:25};
      $display("The id is =  {%0p}", id);
      $display("The name is =  %0p", name);
      $display("The value is =  %0p", value);
    end 
   endmodule

OUTPUT:  

     run -all  
     The id is =  {{4275:25} {4278:12} }  
     The name is =  {"Dilip":12}   
     The value is =  {50000:25}   
     exit  

---

**3) Associative array over dynamic array and fixed array:**

* We can delete the particular index element in Associative array by using **function void delete(input index);** but in dynamic and fixed type we can't use **function void delete(input index);** to delete the particular index.  

code:

    module associative;
    int id[int];
    int name[string];
    int value[int];
    initial
    begin
      
      id = '{4275:25, 4278:12};
      name = '{"Dilip":12};
      value = '{50000:25};
      $display("The id is =  {%0p}", id);
      $display("The name is =  %0p", name);
      $display("The value is =  %0p", value);
      $display("The size of the id is=%0d", id.size());
      id.delete(4278);
      $display("The size of 'id' after deleting = %0d", id.size());
      $display("The id is = {%0p}", id);
     end
    endmodule 

OUTPUT:   

     run -all  
     The id is =  {{4275:25} {4278:12} }  
     The name is =  {"Dilip":12}   
     The value is =  {50000:25}   
     The size of the id is=2  
     The size of 'id' after deleting = 1  
     The id is = {{4275:25} }  
     exit  

* The Associative array is memory friendly we can store the value of the array in any memory location.

---

## Queues:

**1) Advantages of Queue over all arrays**

* In queue the main advantage is push & pop operation.  
* The push used to insert the element in to queue.  
* The push as two methods **function push_back(input element_t); & function push_front(input element_t);**
* The pop as two methods **function pop_front(); & function pop_back();**

code:   

    `module queue_data_type;
     string queue1[$];
    initial 
     begin
     queue1 = {"manipal", "banglaore", "udupi"};
      $display("\nRemove the array element at first index position of queue1:%p",queue1.pop_front());
      $display("\n After removing the 'manipal' from queue1 is :%p", queue1);
      $display("\nRemove the array element at last index position of queue1: %p", queue1.pop_back());
      $display("\n After removing the 'udupi' from queue1 is :%p", queue1);
      queue1.push_front("Yelahanka");
      $display("\ninsert the array element at first index position of queue1:output");
      $display("\n queue1: %p", queue1);
      queue1.push_back("udupi");
       $display("\ninsert the array element at last index position of queue1: ouptut");
       $display("\n queue1 : %p", queue1);
    end 
  endmodule
  

OUTPUT:

    `run -all  
     // Remove the array element at first index position of queue1:"manipal"  

     // After removing the 'manipal' from queue1 is :'{"banglaore", "udupi"}  
 
     // Remove the array element at last index position of queue1: "udupi"  
  
    // After removing the 'udupi' from queue1 is :'{"banglaore"}  
 
    // insert the array element at first index position of queue1:output  
 
    // queue1: '{"Yelahanka", "banglaore"}  
 
    // insert the array element at last index position of queue1: ouptut  

    // queue1 : '{"Yelahanka", "banglaore", "udupi"}  
    exit

------
  
**2) Advantages of Queue over Dynamic & Associative Array:**

* In queue we can insert the element at any index position using the method **function insert(index , queue_element);**. So we can say inserting is easy. Thus the queue length increases and shrinks even if we remove or add elements from the middle, but in dynamic and associative array we can't insert the element in the array.  

code:

     `module queue_data_type;
      string queue1[$];
      int queue2[$];
     initial 
     begin
     queue1 = {"banglaore", "udupi"};
       queue1.insert(0, "manipal");
      $display("Insert the array element at 0 index position of queue1: {%0p}", queue1);
     end 
    endmodule

OUTPUT:

      `run -all      
      // Insert the array element at 0 index position of queue1: {{manipal} {banglaore} {udupi}}  
      exit  
---
