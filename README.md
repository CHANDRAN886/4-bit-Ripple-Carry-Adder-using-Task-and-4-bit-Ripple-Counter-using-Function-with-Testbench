# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.
```
 `timescale 1ns / 1ps
module ripple_4bit_adder (
    output reg [3:0] s,
    output reg cout,
    input [3:0] a,
    input [3:0] b,
    input cin
);
    reg c1, c2, c3;
    reg temp_sum, temp_carry;
    always @(*) begin
FA(temp_sum, temp_carry, a[0], b[0], cin);
s[0] = temp_sum;
c1 = temp_carry;
FA(temp_sum, temp_carry, a[1], b[1], c1);
s[1] = temp_sum;
c2 = temp_carry;
FA(temp_sum, temp_carry, a[2], b[2], c2);
s[2] = temp_sum;
c3 = temp_carry;
FA(temp_sum, temp_carry, a[3], b[3], c3);
s[3] = temp_sum;
cout = temp_carry;
end
task FA;
output sum, carry;
input A, B, C;
begin
sum = A ^ B ^ C;
carry = (A & B) | (B & C) | (C & A);
end
endtask
endmodule
```

![435371853-c1d969e1-aa87-48ce-9683-71eb73105349](https://github.com/user-attachments/assets/3a506ea0-ac28-48f2-8048-c0ce99b54d30)


// Test bench for Ripple carry adder

module ripple_carry_adder_4bit_tb;

    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the ripple carry adder
    ripple_carry_adder_4bit uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    initial begin
        // Test cases
        A = 4'b0001; B = 4'b0010; Cin = 0;
        #10;
        
        A = 4'b0110; B = 4'b0101; Cin = 0;
        #10;
        
        A = 4'b1111; B = 4'b0001; Cin = 0;
        #10;
        
        A = 4'b1010; B = 4'b1101; Cin = 1;
        #10;
        
        A = 4'b1111; B = 4'b1111; Cin = 1;
        #10;

        $stop;
    end

    initial begin
        $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b", $time, A, B, Cin, Sum, Cout);
    end

endmodule

![image](https://github.com/user-attachments/assets/8c237c2b-f21b-4f48-9d1f-6404380ee1a5)


// Verilog Code ripple counter
````
module ripple_counter(
input clk,
input reset,
output reg [3:0] Q);
function [3:0] next_state;
input [3:0] curr_state;
begin
next_state = curr_state + 1;
end
endfunction
always @(posedge clk or posedge reset) begin
if (reset)
Q <= 4'b0000;
else
Q <= next_state(Q);
end
endmodule
````
![image](https://github.com/user-attachments/assets/0eabb954-ee45-4c3d-87e3-69c894cd9a80)

// TestBench

module ripple_counter_4bit_tb;

    reg clk;
    reg reset;
    wire [3:0] Q;

    // Instantiate the ripple counter
    ripple_counter_4bit uut (
        .clk(clk),
        .reset(reset),
        .Q(Q)
    );

    // Clock generation (10ns period)
    always #5 clk = ~clk;

    initial begin
        // Initialize inputs
        clk = 0;
        reset = 1;

        // Hold reset for 20ns
        #20 reset = 0;

        // Run simulation for 200ns
        #200 $stop;
    end

    initial begin
        $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q);
    end

endmodule
![image](https://github.com/user-attachments/assets/85e98e00-6aec-4c77-8bbf-ea07abfadb69)

Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.

