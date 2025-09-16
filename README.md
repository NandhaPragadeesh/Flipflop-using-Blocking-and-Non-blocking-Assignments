# Flipflop-using-Blocking-and-Non-blocking-Assignments
Exp 3 -Write and simulate D, SR, JK, T Flipflops using Blocking and Non blocking Assignments
   Aim: To design and simulate D, SR, JK, T Flipflops using Blocking and Non blocking Assignments in Verilog HDL and verify its functionality through a testbench using the Vivado 2023.1 simulation environment.
   
  Apparatus Required:
  Vivado 2023.1
Procedure:

Launch Vivado Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu. 
Create a New Project Click on "Create Project" from the Vivado Quick Start window. In the New Project Wizard: Project Name: Enter a name for the project (e.g., Mux4_to_1). 
Project Location: Select the folder where the project will be saved. Click Next. 
Project Type: Select RTL Project, then click Next. Add Sources: Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.). 
Make sure to check the box "Copy sources into project" to avoid any external file dependencies. 
Click Next.
Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation). 
Default Part Selection: You can choose a part based on the FPGA board you are using (if any). 
If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7). 
Click Next, then Finish.
Add Verilog Source Files In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier.
Add the Verilog files and the testbench. 
Check Syntax Expand the "Flow Navigator" on the left side of the Vivado interface. 
Simulate the Design In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation". 
Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench. 
View and Analyze Simulation Results Adjust Simulation Time To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings". Under "Simulation", modify the Run Time (e.g., set to 1000ns).
Generate Simulation Report Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results".
Save the report for reference in your lab records. Save and Document Results Save your project by clicking File → Save Project.
Take screenshots of the waveform window and include them in your lab report to document your results. 
You can include the timing diagram from the simulation window showing the correct functionality of the Seven Segment across different select inputs and data inputs. 
Close the Simulation Once done, by going to Simulation → "Close Simulation

Input/Output Signal Diagram:

D FF

<img width="495" height="246" alt="image" src="https://github.com/user-attachments/assets/29e9d962-eeeb-4fb5-af1b-261005c66ded" />

SR FF

<img width="485" height="290" alt="image" src="https://github.com/user-attachments/assets/80bc78a5-f209-4215-b6c3-9afb1b8f1a00" />

T FF

<img width="483" height="270" alt="image" src="https://github.com/user-attachments/assets/34936d59-6113-44fa-9971-b19a573c34d0" />

JK FF

<img width="453" height="283" alt="image" src="https://github.com/user-attachments/assets/8a9cdf56-ce49-4c00-89ce-02102ea404e2" />

RTL Code:

D FF:
```
module dff_block(clk,rst,d,dout);
    input clk,rst,d;
    output reg dout;
    always@ (posedge clk)
    begin
        if(rst)
            dout = 1'b0;
        else
            dout = d;
     end
endmodule
```
SR FF:
```
module sr_ff (input clk,input S,input R,output reg Q);
always @(posedge clk)
 begin
    case ({S,R})
      2'b00: Q <= Q;    
      2'b01: Q <= 0;    
      2'b10: Q <= 1;    
      2'b11: Q <= 1'bx; 
 endcase
 end
endmodule
```
T FF:
```
module tff_block(clk,rst,Tout,T);
    input clk,rst,T;
    output reg Tout;
    always@ (posedge clk)
     begin
     if(rst)
        Tout = 1'b0;
     else if(T)
        Tout = ~Tout;
     else
        Tout = Tout;
     end
endmodule
```
JK FF:
```
module jk_ff(input clk,J,K, output reg Q);
always @(posedge clk) begin
case({J,K})
2'b00: Q<=Q;
2'b01: Q<=0;
2'b10: Q<=1;
2'b11: Q<=~Q;
endcase
end
endmodule
```
TestBench:
D FF:
```
module dff_block_tb;
    reg clk_t,rst_t,d_t;
    wire dout_t;
    dff_block dut(.clk(clk_t),.rst(rst_t),.d(d_t),.dout(dout_t));
    initial
      begin
        clk_t = 1'b0;
        rst_t = 1'b1;
     #20
        rst_t = 1'b0;
        d_t   = 1'b0;
     #20
        d_t = 1'b1;
     end
     always 
        #10 clk_t = ~clk_t;
endmodule
```
SR FF:
```
module sr_ff_tb;
  reg clk, S, R;
  wire Q;
  sr_ff uut (.clk(clk),.S(S),.R(R),.Q(Q));
  initial begin
   clk = 0;
  forever #10 clk = ~clk; 
  end
  initial begin
    S = 0; R = 0;
    #100 S = 1; R = 0;   
    #100 S = 0; R = 0;   
    #100 S = 0; R = 1;   
    #100 S = 1; R = 1;  
    #100 S = 0; R = 0;
 end
endmodule
```
T FF:
```
module tff_block_tb;
    reg clk_t,rst_t,T_t;
    wire Tout_t;
    
    tff_block dut(.clk(clk_t),.rst(rst_t),.T(T_t),.Tout(Tout_t));
    
    initial
     begin
        clk_t = 1'b0;
        rst_t = 1'b1;
     #20
        rst_t = 1'b0;
        T_t = 1'b0;
     #20
        T_t = 1'b1;
     end
     
     always 
        #10 clk_t = ~clk_t;
endmodule
```
JK FF:
```
module tb_jk_ff;
  reg clk;
  reg J, K;
  wire Q;
  jk_ff uut (.clk(clk),.J(J),.K(K),.Q(Q));
initial begin
clk=0;
forever #20 clk=~clk;
end
initial begin
 J = 0; K = 0;
    #100 J=0; K=0;  
    #100 J=0; K=1; 
    #100 J=1; K=0;  
    #100 J=1; K=1;  
    #100 J=0; K=1; 
    #100 J=1; K=0;  
    #100 J=1; K=1;  
end
endmodule
```
Output waveform:
D FF:
<img width="1919" height="1199" alt="Screenshot 2025-09-16 155110" src="https://github.com/user-attachments/assets/f8ef5386-ac4f-4546-8e78-0a3a33e17571" />
SR FF:
<img width="1635" height="1097" alt="Screenshot 2025-09-16 201028" src="https://github.com/user-attachments/assets/a8fa769d-6ce0-4f85-a47f-b285365fd3b9" />
T FF:
<img width="1626" height="1099" alt="Screenshot 2025-09-16 161709" src="https://github.com/user-attachments/assets/933a8d25-a0ac-4ae8-89fb-07a459d6e418" />
JK FF:
<img width="1633" height="1098" alt="Screenshot 2025-09-16 201417" src="https://github.com/user-attachments/assets/7ce07292-fb38-4c32-a76c-e6a8b51b5de4" />

Conclusion:

We successfully implemented and verified D, SR, T, and JK flip-flops using Verilog HDL in Xilinx Vivado.
Design Implementation:
Each flip-flop (D, SR, T, JK) was written as a synthesizable Verilog module, following standard sequential logic behavior.
Testbench Development:
Separate testbenches were created for each design (*_tb.v) to apply clock signals, reset conditions, and all possible input combinations systematically.
Simulation & Verification:
Behavioral simulations were run in Vivado, and the output waveforms were analyzed. The results confirmed that all flip-flops behaved as expected:
D Flip-Flop: Output Q follows input D on the rising edge of the clock.
SR Flip-Flop: Proper set, reset, and hold operations observed; invalid condition (S=R=1) resulted in X (undefined state) as expected.
T Flip-Flop: Output toggled on each clock pulse when T=1, and held state when T=0.
JK Flip-Flop: Verified all four combinations of J and K, including toggling when J=K=1.

This exercise helped in understanding sequential circuit behavior, clocked logic design, and testbench-driven verification.
