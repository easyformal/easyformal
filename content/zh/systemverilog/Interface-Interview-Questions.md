---
title: "Interface-Interview-Questions"

tags: "sv"

weight: 25
---

**1. How does the clocking block handles synchronous reset?**

Solution:  

Synchronous reset  is sampled  only with respect to clock event. When reset is enabled, it will be effective only after next active clock edge.    

     clockingblock  TB_CB @(posedge clk)   
     default input #1step output #0;   
     input rst;
     output a,b;
     input y;

**Note:** `The clocking block is only designed to handle synchronous reset` - it should  happen only be based on clock event. Reset in clocking block  should be handled elsewhere. If the clocking block outputs are variables, you can procedurally assign them outside of a clocking block on a reset.   

---   

**2. How does the modport handle asynchronous reset?**

Solution:  

Asynchronous Reset  

In asynchronous reset sampling is  done by independent of clk. When reset is enabled ,it will be effective immediately within the same the clock edges.  

![image](https://user-images.githubusercontent.com/110484152/192695050-b55d72c7-d71c-423e-8cca-aab9433be1f7.png)  

                                             Fig 1: Design example   

Here, in this example there are three signals a,b and c. 'a' is continuous signal and asynchronous. 'b' and 'c' are synchronous signals. Using modport we can change the asynchronous signal 'a' to synchronous signal.  
`modport TB_MP(TB_CB , output a);` 

**Note:** `In Modport, the design can handled with asynchronous reset.` The asynchronous reset will be happen at any time.  

---

**3.What is  synchronous and asynchronous  reset?**      

Solution: 

Synchronous reset means reset is sampled with respect to clock. In other words, when reset is enabled, it will not be effective till the next active clock edge.  

module synchronous_reset_test (input logic clk, reset, in1, output logic out1)  
always @(posedge clk)  
if(reset) out1 <= 1'b0;  
else out1 <= in1;  
endmodule  

The out1 will be changed only with the posedge of clk. To get the effect of reset, reset should be wide enough to be captured by the next posedge of clk.  

![image](https://user-images.githubusercontent.com/110484152/192996653-a09a4755-a5c5-4b47-aeda-97d2156185a1.png)  

                                                   Fig 2: Synchronous Reset  

Asynchronous Reset  

In asynchronous reset, reset is sampled independent of clk. That means, when reset is enabled it will be effective immediately and will not check or wait for the clock edges.  

module asynchronous_reset (input logic clk, reset, in1, output logic out1);  
always @(posedge clk or posedge reset)  
if(reset) out1 <= 1'b0;  
else out1 <= in1;  
endmodule  

![image](https://user-images.githubusercontent.com/110484152/192997263-68f7de6a-da15-4deb-a429-86597ebc3e43.png)  

                                                      Fig 3: Asynchronous Reset     

---

**4.What is the usage of blocking and non blocking assignments in sampling and driving signals?**   

Solution: 

The blocking assignment (=) used for sampling signals in active region.  
The non blocking assignment (<=)used for driving signals in active-NBA region.  
So we can avoid the race condition in verilog.  

---

**5. Access restriction of signals using modport**

Solution:

**Code Snippet:**

interface.sv file:

    // interface defination for and gate
      interface and_intr;
      //input and output signals declaration for design
        logic p,q;
        logic r;
      // modport declaration for design file
        modport DUT_MP(input q,output r);
      // modport declaration for testbench file
        modport TB_MP(output p,output q,input r);
     endinterface : and_intr

tb.sv file:

   // testbench file for and gate design
   // module defination for testbench with interface instanciation
      module tb(and_intr inf);
  
       initial
        begin
         $display("// and gate output using modports\n");
         repeat(5)
           begin
             inf.TB_MP.p = $random;
             #1;
             inf.TB_MP.q = $random;
             #1;
             $display("input_p=%b\t input_q=%b\t output_r=%b",inf.TB_MP.p,inf.TB_MP.q,inf.TB_MP.r);
           end
        end
      endmodule : tb

design.sv file:

    // and gate design file
    // module defination for and gate with interface instanciation
       module and_gate(and_intr inf);
    // assign the output using continuous assignment
          assign inf.DUT_MP.r = (inf.DUT_MP.p) & (inf.DUT_MP.q);
       endmodule : and_gate

**Output:**

when the input signal 'p' is not there in the DUT_MP(design modport) but that signal is called in testbench to generate the output at that time it will give an error which is shown in below snapshot.

![access_restricting_modports](https://user-images.githubusercontent.com/110448056/193022636-f48a0246-1d66-4741-b3a6-130f6e02eac0.png)

**Lab link:-** https://github.com/muneeb-mbytes/SystemVerilog_Course/tree/production/interface/interface_error/modport_error   

---

**6. What happens if we assign value to output in test module, while doing interface?**

solution: We get a warning as one of the assignment is implicit , but we'll get proper output.

![inter_error](https://user-images.githubusercontent.com/110443214/193393454-5b2a0429-3c8f-434c-8777-86e183924d50.png)


**Lab link:** https://github.com/muneeb-mbytes/SystemVerilog_Course/tree/production/interface/interface_error/interface_example





