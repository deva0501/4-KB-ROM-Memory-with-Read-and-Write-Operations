# 4 KB-ROM-Memory-with-Read-and-Write-Operations
Aim:
   To design and simulate a 4KB ROM memory with read and write operations using Verilog HDL and verify the functionality through a testbench in the Vivado 2023.1 simulation environment.

Apparatus Required:
     Vivado 2023.1 or equivalent Verilog simulation tool.
     
Procedure:
     Launch Vivado 2023.1:

1.Open Vivado and create a new project:
     Design the Verilog Code for ROM:
     Write the Verilog code for a 4KB ROM memory with read and write capabilities.
2.Create the Testbench:
     Write a testbench to simulate both the read and write operations, verifying that the data is correctly written to and read from the memory.
3.Add the Verilog Files:
     Add the ROM Verilog module and the testbench file to the project.
4.Run Simulation:
     Run the behavioral simulation in Vivado and check the memory's read and write operations.
5.Observe the Waveforms:
     Analyze the waveform to verify that the memory read and write operations work as expected.
6.Save and Document Results:
     Capture the waveform and include the simulation results in the final report.
Verilog Code for 4KB ROM Memory with Read and Write Operations
In this design, we will implement a 4KB ROM. Since ROM is typically read-only, we will simulate the behavior as if it's writable, but in actual hardware, ROM is typically pre-programmed.

4KB = 4096 Bytes = 4096 x 8 bits The address width for 4KB memory is 12 bits (2^12 = 4096).


Verilog Code rom memory:

// rom_memory.v
module rom_memory (
    input wire clk,
    input wire write_enable,   // Signal to enable write operation
    input wire [11:0] address, // 12-bit address for 4KB memory
    input wire [7:0] data_in,  // Data to write into ROM
    output reg [7:0] data_out  // Data read from ROM
);

    // Declare ROM with 4096 memory locations (each 8 bits wide)
    reg [7:0] rom[0:4095];

    always @(posedge clk) begin
        if (write_enable) begin
            // Write operation: Write data into the ROM at the given address
            rom[address] <= data_in;
        end
        // Read operation: Read data from the ROM at the given address
        data_out <= rom[address];
    end
endmodule

Simulated Output:

![WhatsApp Image 2025-04-26 at 13 22 39_5f96103e](https://github.com/user-attachments/assets/3bbdd3f1-44b9-4622-95d7-889d9d1d8caa)

 Verilog Code for the Testbench for 4KB ROM Memory:

 // rom_memory_tb.v
`timescale 1ns / 1ps

module rom_memory_tb;

    // Inputs
    reg clk;
    reg write_enable;
    reg [11:0] address;
    reg [7:0] data_in;

    // Outputs
    wire [7:0] data_out;

    // Instantiate the ROM module
    rom_memory uut (
        .clk(clk),
        .write_enable(write_enable),
        .address(address),
        .data_in(data_in),
        .data_out(data_out)
    );

    // Clock generation
    always #5 clk = ~clk;  // Toggle clock every 5 ns

    // Test procedure
    initial begin
        // Initialize inputs
        clk = 0;
        write_enable = 0;
        address = 0;
        data_in = 0;

        // Write data into memory
        #10 write_enable = 1; address = 12'd0; data_in = 8'hA5;  // Write 0xA5 at address 0
        #10 write_enable = 1; address = 12'd1; data_in = 8'h5A;  // Write 0x5A at address 1
        #10 write_enable = 1; address = 12'd2; data_in = 8'hFF;  // Write 0xFF at address 2
        #10 write_enable = 1; address = 12'd3; data_in = 8'h00;  // Write 0x00 at address 3

        // Disable write and start reading from memory
        #10 write_enable = 0; address = 12'd0;
        #10 address = 12'd1;
        #10 address = 12'd2;
        #10 address = 12'd3;

        // Stop the simulation
        #10 $stop;
    end

    // Monitor the values for verification
    initial begin
        $monitor("Time = %0t | Write Enable = %b | Address = %h | Data In = %h | Data Out = %h", 
                 $time, write_enable, address, data_in, data_out);
    end

endmodule

Simulated Output:

![WhatsApp Image 2025-04-26 at 13 23 53_542434bc](https://github.com/user-attachments/assets/9ce80e31-4535-47e2-949b-9e4186247267)


Conclusion:
          In this experiment, a 4KB ROM memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes for ROM.
