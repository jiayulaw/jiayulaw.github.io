
# 4-digit BCD Adder
```verilog
module top_module ( 
    input [15:0] a, b,
    input cin,
    output cout,
    output [15:0] sum 
);
    reg [3:0] cout_arr;
    bcd_fadd bcd_faddv1 (.a(a[3:0]), .b(b[3:0]), .cin(cin), .cout(cout_arr[0]), .sum(sum[3:0]));
    bcd_fadd bcd_faddv2 (.a(a[7:4]), .b(b[7:4]), .cin(cout_arr[0]), .cout(cout_arr[1]), .sum(sum[7:4]));
    bcd_fadd bcd_faddv3 (.a(a[11:8]), .b(b[11:8]), .cin(cout_arr[1]), .cout(cout_arr[2]), .sum(sum[11:8]));
    bcd_fadd bcd_faddv4 (.a(a[15:12]), .b(b[15:12]), .cin(cout_arr[2]), .cout(cout_arr[3]), .sum(sum[15:12]));
    assign cout = cout_arr[3];
endmodule
```