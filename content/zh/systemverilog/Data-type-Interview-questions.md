---
title: "Data-type-Interview-questions"
categories: ["SystemVerilog"]
tags: "sv"

weight: 23
---

****1.Is there any difference between reg and logic in SystemVerilog?****

**Answer:**  
Before system verilog ,verilog is mainly used for synthesis and verification purposes. In verilog there are two main data type   
 1) reg  
 2)net   
Reg is mainly used when we want to store values in the variable .It is mainly used inside procedural block .mainly it is used for designing of sequential circuit .net variable are mainly continuously driven .In net type variable it is not possible to store value .

So ,there is lots of confusion for designer ,where to use reg and where to use net.This problem is solved in system verilog .System verilog introduced a new data type called as “logic”. This data type can be use at both the places. It can be use for net as well as reg.

**2.What is the difference between logic[7:0] and byte variable in System Verilog?**

**Answer:**
* The 'byte' is a signed variable which means it can only be used to count values till 127.
* A logic[7:0] variable can be used for an unsigned 8 bit variable that can count up to 255.  

**3.What are the difference between 2 state and 4 state?**

**Answer:**
* 4 state includes logic 0, logic 1, logic X where it is related to registers, in real world it is declared as 0 or 1 it is based on the simulators we're using and logic Z, which is related to wires.  
* 2 state includes of logic 1 and logic 0.

**4.Difference between packed and unpacked arrays**

**Answer:**
Refer the below link  
https://github.com/muneeb-mbytes/SystemVerilog_Course/wiki/02.Array#static-arrays-has-two-types

**5.Comparision between fixed, dynamic, associative array and queue.**

**Answer:**

Refer the below link 
https://github.com/muneeb-mbytes/SystemVerilog_Course/wiki/Choosing-an-array

**6.Given a dynamic array of size 100, how can the array be re-sized to hold 200 elements while the lower 100 elements are preserved as original?**

**Answer:**
A dynamic array needs memory allocation using new[] to hold elements. Here is an example with an integer array that grows from an initial size of 100 elements to 200 elements.
   
    // Declare the dynamic array.
    integer addr[]; 
    // Create a 100-element array.
     addr = new[100]; 
    ………
    // Double the array size, preserving previous values.
    addr = new[200](addr);

**7.Difference between packed and unpacked structure**

**Answer:**  
Refer the below link  
https://github.com/muneeb-mbytes/SystemVerilog_Course/wiki/03.Structure-and-Union#types-of-structure

**8.What happens if we give same value for different names in enumeration data type?**

**Answer:**   
code:
      
      module set_value_enum;    
      //set same value for MONDAY and TUESDAY.  
       enum {MONDAY=2, 
             TUESDAY=2, 
             WEDNESDAY=5, 
             THURSDAY, 
             FRIDAY=10, 
             SATURDAY, 
             SUNDAY }days;


output :       

      Error: Enum member 'TUESDAY' does not have unique value.



**9.What is the output if you declare bit as signed and assign value as 11xz11x1 ?**

**Answer:**   
code:   

      module data_type_bit;   
        // declare bit signed type 8 bit variable 
        bit signed [7:0] data = 8'b11xz11x1;   

        initial begin
           $display("\nValue of bit signed data in binary = %0b",data);
           $display("Value of bit signed data in decimal = %0d\n",data);
        end
      endmodule: data_type_bit


output :   

      Value of bit signed data in binary = 11001101
      Value of bit signed data in decimal = -51

In the above code, we declared bit as 11xz11x1, here bit is 2 state, so x and z are converted to 0 and it will become 11001101. Since it is signed bit it gives -51 in decimal format. 




**10.What is the output if you declare byte as unsigned and assign value as 11101001 ?**

**Answer:**   
code: 

      module data_type_byte;   
         // declare byte unsigned type 8 bit variable 
         byte unsigned data = 8'b11101001;

         initial begin
           $display("\nValue of byte unsigned data in binary = %0b",data);
           $display("Value of byte unsigned data in decimal = %0d\n",data);
         end
      endmodule: data_type_byte


output :   

      Value of byte unsigned data in binary = 11101001
      Value of byte unsigned data in decimal = 233


In the above code, we declared byte as 11101001. Since it is unsigned byte it gives 233 in decimal format.