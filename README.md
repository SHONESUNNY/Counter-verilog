h
A 4-bit Verilog counter is a digital device that counts from 0 to 15 in binary. It starts from 0000 and increments until it reaches 1111. Once it hits 1111, the counter rolls over to 0000 and continues counting. The counter works as long as the clock is running and the reset signal stays high



RTL SCHEMATIC : 
![image](https://github.com/user-attachments/assets/58edb96f-780b-468f-8db5-25ccb631d7d1)

CODE :
```verilog
module counter(
    input clk,              
    input rstn,            
    output reg [3:0] out  
);
  
  // Always block triggered at the rising edge of the clock
  always @ (posedge clk) begin
    if (!rstn)              // If reset is active (low), set counter to zero
      out <= 0;
    else                    // Else, increment the counter
      out <= out + 1;
  end
endmodule
```
Test bench :
![Screenshot 2025-03-16 235104](https://github.com/user-attachments/assets/0107e10d-35c9-4bcd-8dfd-db12b6ff5819)
```verilog
module tb_counter;
  reg clk;                // Internal clock variable for testbench
  reg rstn;               // Internal reset variable for testbench
  wire [3:0] out;         // Wire to connect to the counter's output

  // Instantiate the counter module and connect to testbench variables
  counter c0 ( .clk(clk), .rstn(rstn), .out(out));

  // Clock generation: toggles every 5 ns (100 MHz clock)
  always #5 clk = ~clk;

  // Stimulus generation in initial block
  initial begin
    clk = 0;              // Initialize clock to 0
    rstn = 0;             // Initialize reset to 0

    #20 rstn = 1;         // Assert reset after 20ns
    #80 rstn = 0;         // Deassert reset after 80ns
    #50 rstn = 1;         // Assert reset again after 50ns


    #200 $finish;          // End simulation after 200ns
  end
endmodule                      


