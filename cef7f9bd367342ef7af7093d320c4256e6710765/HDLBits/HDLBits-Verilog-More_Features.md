# Conditional ternary operator
```verilog
module top_module (
    input [7:0] a, b, c, d,
    output [7:0] min);//
    // assign intermediate_result1 = compare? true: false;
    wire [7:0] inter1, inter2;
    assign inter1 = (a > b) ? b : a;
    assign inter2 = (inter1 > c) ? c : inter1;
    assign min = (inter2 > d) ? d : inter2;

endmodule
```
![](Pasted%20image%2020240509211401.png)  

# Reduction Operators
```verilog
module top_module (
    input [7:0] in,
    output parity);
    assign parity = {^ in, in};

endmodule
```

# Combinational for-loop: Vector reversal 2
```verilog
module top_module( 
    input [99:0] in,
    output [99:0] out
);
    always @(*) begin
        integer i;
        for (i = 0; i < 100; i = i + 1) begin
            out[i] = in[99 - i];
        end
    end
endmodule
```


# Combinational for-loop: Counter
```verilog
module top_module( 
    input [254:0] in,
    output reg [7:0] out );
    always @(*) begin
        out = 8'd0;
        for (int i = 0; i < 255; i++) begin
        	if (in[i] == 1'b1) begin
               out = out + 8'd1;        
        	end
    	end
    end
endmodule
```

# Generate for-loop: 100-bit Binary Adder 2
Create a 100-bit binary ripple-carry adder by instantiating 100 [full adders](https://hdlbits.01xz.net/wiki/Fadd "Fadd"). The adder adds two 100-bit numbers and a carry-in to produce a 100-bit sum and carry out. To encourage you to actually instantiate full adders, also output the carry-out from _each_ full adder in the ripple-carry adder. cout[99] is the final carry-out from the last full adder, and is the carry-out you usually see.
```verilog
module top_module( 
    input [99:0] a, b,
    input cin,
    output [99:0] cout,
    output [99:0] sum );
    
    add1 add1v1 (.a(a[0]), .b(b[0]), .cin(cin), .cout(cout[0]), .sum(sum[0]));
    generate
        genvar i;
        for (i = 1; i < 100; i++) begin : blockname
            add1 add1v2_to_100 (.a(a[i]), .b(b[i]), .cin(cout[i - 1]), .cout(cout[i]), .sum(sum[i]));
        end
    endgenerate
endmodule

module add1 (
    input a, b, cin,
    output cout, sum
);
    assign {cout, sum} = a + b + cin;
endmodule
```

If you change the sequence of for-loop to ```for (i = 99; i > 0; i--)``` , the circuit still works because we are using continuous assignment.

# Generate for-loop: 100-digit BCD Adder
1 BCD digit = 4 bits
```verilog
module top_module( 
    input [399:0] a, b,
    input cin,
    output cout,
    output [399:0] sum );
    genvar i;
    reg [99:0] cout_arr;
    bcd_fadd bcd_fadd_v1 (.a(a[3:0]), .b(b[3:0]), .cin(cin), .cout(cout_arr[0]), .sum(sum[3:0]));
    generate
        for (i = 1; i < 100; i=i+1) begin : blockname
            bcd_fadd bcd_fadd_2_to_100 (.a(a[i + i*3 + 3: i + i*3]), .b(b[i + i*3 + 3: i + i*3]), .cin(cout_arr[i - 1]), .cout(cout_arr[i]), .sum(sum[i + i*3 + 3: i + i*3]));
        end
    endgenerate
    assign cout = cout_arr[99];
endmodule
```

