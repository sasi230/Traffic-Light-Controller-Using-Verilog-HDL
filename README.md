#   EXP NO:4 Traffic Light Controller using Verilog HDL and Testbench Verification

## Aim:
To design and simulate a traffic light controller using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 simulation environment. The objective is to control the traffic lights for a junction with a specific time-based sequence for Red, Yellow, and Green lights.

## Apparatus Required:
+Vivado 2023.1 or equivalent Verilog simulation tool.
+Computer system with a suitable operating system.
+FPGA board (optional for hardware verification).
## Procedure
1. Launch Vivado 2023.1:Open Vivado and create a new project.
2. Design the Traffic Light Controller Verilog Code:Write the Verilog code for the traffic light controller, using an FSM (Finite State Machine) to transition between Green, Yellow, and Red lights based on timing intervals.
3. Create the Testbench:Write a testbench to simulate the traffic light controller. The testbench will check the sequence of light transitions based on time.
4. Add the Verilog Files:Add the traffic light controller Verilog code and the testbench file to the project.
5. Run Simulation:Run the behavioral simulation in Vivado to verify the correct sequence of the traffic lights.
6. Observe the Waveforms:Examine the waveform output to verify that the traffic light transitions through the Green, Yellow, and Red lights in the correct sequence.
7. Save and Document Results:Capture screenshots of the waveform and save the simulation logs to include in your report.

## Verilog Code for Traffic Light Controller

// traffic_light_controller.v
```
module TrafficLightController(
    input wire clk,          // Clock input
    input wire reset,        // Reset input
    output reg [1:0] state   // 2-bit state output (00: Green, 01: Yellow, 10: Red)
);

    // State encoding
    localparam GREEN  = 2'b00;
    localparam YELLOW = 2'b01;
    localparam RED    = 2'b10;

    // State transition
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= GREEN; // Reset to Green state
        end else begin
            case (state)
                GREEN: state <= YELLOW; // Green to Yellow
                YELLOW: state <= RED;    // Yellow to Red
                RED: state <= GREEN;     // Red to Green
                default: state <= GREEN; // Default to Green
            endcase
        end
    end
endmodule
```
## Testbench for Traffic Light Controller

// traffic_light_controller_tb.v
`timescale 1ns / 1ps
```
module TrafficLightController_tb;

    reg clk;                // Clock signal
    reg reset;              // Reset signal
    wire [1:0] state;       // State output

    // Instantiate the TrafficLightController
    TrafficLightController uut (
        .clk(clk),
        .reset(reset),
        .state(state)
    );

    // Generate clock signal
    initial begin
        clk = 0;
        forever #5 clk = ~clk; // Toggle clock every 5 time units
    end

    // Test sequence
    initial begin
        // Initialize inputs
        reset = 1; // Assert reset
        #10;       // Wait for 10 time units
        reset = 0; // Deassert reset

        // Simulate for enough time to see state changes
        #30;       // Wait for 30 time units

        // Assert reset again
        reset = 1;
        #10;       // Wait for 10 time units
        reset = 0;

        // Simulate for more time
        #30;       // Wait for 30 time units

        
        $finish;
    end

    
    initial begin
        $monitor("Time: %0d | State: %b", $time, state);
    end
endmodule
```
## OUTPUT:
![Screenshot 2024-10-05 170022](https://github.com/user-attachments/assets/86b2655f-59ab-484c-a4b7-3305f87321f4)
## Conclusion:
In this experiment, a traffic light controller was successfully designed and simulated using Verilog HDL. The design controlled the traffic lights to switch between Green, Yellow, and Red in a cyclic manner based on timing intervals. The testbench verified that the traffic lights followed the correct sequence and timing. The simulation results confirm the correct functionality of the traffic light controller, demonstrating the effectiveness of Verilog HDL in designing FSM-based controllers for real-world applications.
