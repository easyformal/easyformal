---
title: "Differences-between-macros-and-parameters"
categories: ["SystemVerilog"]
tags: "sv"

weight: 24
---


|**Macros**|**Parameters**|
|------|----|
|macros are replaceable|parameters are like variables |
|macros works in pre-compilation state | parameters works in Elaboration state|
|***syntax :*** **`define macro_name value**  |***syntax :*** **parameter parameter_name=value**|
|We can use **`define** in any file|we can use **parameter** within the file|
|we can't give datatypes in macros|we can use and change datatype in parameters|
|Macros can have multiple lines| multiple lines cannot be possible here because parameter is just like declaring variables |
|We can give value for a macro in command line | parameters value can't be changed in command line|

## Execution stage of macros and parameters

![Untitled Diagram drawio (44)](https://user-images.githubusercontent.com/110509375/208600538-05875a31-1ca6-43ad-b0b5-7840cda420c7.png) 



  
### Parameters used in macros     
```
parameter data = 5; // data will be replaced by value 5 in Elaboration
`define  DATA data // in pre-compilation `DATA will be replaced with data
module tb();
  int a,b;
  initial begin
   
    $display("DATA=%0d",`DATA);
    
     
     b= `DATA + 2;  

    $display(" b=%0d",b);
  end 
endmodule
```   
In the above code the parameter value of data =5 is used in the macro `DATA    
### Macros used in parameters  
```  
`define  DATA 25
parameter data = `DATA;

module tb();
  int a,b;
  initial begin
    $display("data=%0d",data); // in pre-compilation `DATA will be replaced with 25.
    $display("DATA=%0d",`DATA); // data will be replaced by value 25 in Elaboration 
    
     a = data +5;
     b= `DATA + 2;
    
    $display("a=%0d b=%0d",a,b);
  end 
endmodule  
``` 
In the above code the macro value of `DATA 25 is used in the parameter data.


 **Macros(`define) can be used  in command line**
 
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
We  can give inputs to macros in command line interface using `-timescale 1ns/1ns +define+name=20` in compile options .





