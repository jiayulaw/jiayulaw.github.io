# Arithmetic Shift
Need to cast as "signed" data for arithmetic shift operator to work
```verilog
module top_module(
    input clk,
    input load,
    input ena,
    input [1:0] amount,
    input [63:0] data,
    output reg [63:0] q);
    
    always @ (posedge clk) begin
        if (load == 1) begin
            q = data;
        end
        else if (ena == 1) begin
            case (amount)
                2'b00: q = q <<< 1;
                2'b01: q = q <<< 8;
                2'b10: q = $signed(q) >>> 1;
                2'b11: q = $signed(q) >>> 8;
            endcase
        end
    end
endmodule
```

# Galois LSFR
linear feedback shift register

# 3-input LUT
```verilog
module top_module (
    input clk,
    input enable,
    input S,
    input A, B, C,
    output reg Z ); 
    reg [0:7] Q;
    always @ (posedge clk) begin
        if (enable == 1) begin
            Q = {S, Q[0:6]}; 
        end
    end
    always @ (*) begin
        case ({A,B,C})
            3'b000: Z = Q[0];
            3'b001: Z = Q[1];
            3'b010: Z = Q[2];
            3'b011: Z = Q[3];
            3'b100: Z = Q[4];
            3'b101: Z = Q[5];
            3'b110: Z = Q[6];
            3'b111: Z = Q[7];
        endcase 
    end
endmodule
```