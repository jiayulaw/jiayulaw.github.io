# Count10
```verilog
module top_module (
    input clk,
    input reset,        // Synchronous active-high reset
    output [3:0] q);
    always @ (posedge clk) begin
        if ((reset == 1) | (q == 4'd9)) begin
           q = 0; 
        end
        else begin
            q = q + 4'd1;
        end
    end
endmodule
```

# Count1to10
```verilog
module top_module (
    input clk,
    input reset,
    output [3:0] q = 4'd1);
    always @ (posedge clk) begin
        if ((reset == 1)|(q == 4'd10)) begin
            q = 4'd1;
        end
        else begin
            q = q + 1;
            
        end
    end

endmodule
```


# Counter 12 with checker (uncompleted)
Solution by chatgpt. 
Output 'c_enable' has no mismatches. 
Output 'c_d' has no mismatches.
```verilog
module top_module (
    input clk,
    input reset,         // Synchronous active-high reset
    input enable,        // Counter enable
    output [3:0] Q,      // 4-bit counter output
    output c_enable,     // Control signal to count4
    output c_load,       // Control signal to count4
    output [3:0] c_d     // Control signal to count4
);

    // Internal signals
    reg [3:0] counter_value;
    wire load;
    wire [3:0] next_value;

    // Instantiate the 4-bit counter
    count4 internal_counter (
        .clk(clk),
        .enable(c_enable),
        .load(c_load),
        .d(c_d),
        .Q(Q)
    );

    // Determine the next value and load signal
    assign next_value = (counter_value == 4'd12) ? 4'd1 : counter_value + 4'd1;
    assign load = reset || (counter_value == 4'd12);
    assign c_enable = enable && !load;
    assign c_load = load;
    assign c_d = reset ? 4'd1 : next_value;

    // Synchronous block to update counter_value
    always @(posedge clk) begin
        if (reset) begin
            counter_value <= 4'd1;
        end else if (enable) begin
            if (load) begin
                counter_value <= reset ? 4'd1 : next_value;
            end else begin
                counter_value <= next_value;
            end
        end
    end
endmodule
```

