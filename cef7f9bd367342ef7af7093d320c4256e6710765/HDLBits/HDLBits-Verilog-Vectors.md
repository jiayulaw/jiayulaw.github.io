# Vector2
```verilog
module top_module (
	input [31:0] in,
	output [31:0] out
);

	assign out[31:24] = in[ 7: 0];	
	assign out[23:16] = in[15: 8];	
	assign out[15: 8] = in[23:16];	
	assign out[ 7: 0] = in[31:24];	
	
endmodule
```
# Bitwise operators
```verilog
module top_module( 
    input [2:0] a,
    input [2:0] b,
    output [2:0] out_or_bitwise,
    output out_or_logical,
    output [5:0] out_not
);
    assign out_or_bitwise = a | b;
    assign out_or_logical = a || b;
    assign out_not[5:3] = ~b;
    assign out_not[2:0] = ~a;
endmodule
```

# Four-input gates
Build a combinational circuit with four inputs, in[3:0].

There are 3 outputs:

- out_and: output of a 4-input AND gate.
- out_or: output of a 4-input OR gate.
- out_xor: output of a 4-input XOR gate.

To review the AND, OR, and XOR operators, see [andgate](https://hdlbits.01xz.net/wiki/andgate "andgate"), [norgate](https://hdlbits.01xz.net/wiki/norgate "norgate"), and [xnorgate](https://hdlbits.01xz.net/wiki/xnorgate "xnorgate").

See also: [Even wider gates](https://hdlbits.01xz.net/wiki/gates100 "gates100").

```verilog
module top_module( 
    input [3:0] in,
    output out_and,
    output out_or,
    output out_xor
);
    assign out_and = in[0] && in[1] && in[2] && in[3];
    assign out_or = in[0] || in[1] || in[2] || in[3];
    assign out_xor = in[0] ^ in[1] ^ in[2] ^ in[3];

endmodule
```

![](Pasted%20image%2020240509203117.png)
# Replication operator
One common place to see a replication operator is when sign-extending a smaller number to a larger one, while preserving its signed value. This is done by replicating the sign bit (the most significant bit) of the smaller number to the left. For example, sign-extending 4'b**0**101 (5) to 8 bits results in 8'b**0000**0101 (5), while sign-extending 4'b**1**101 (-3) to 8 bits results in 8'b**1111**1101 (-3).

Build a circuit that sign-extends an 8-bit number to 32 bits. This requires a concatenation of 24 copies of the sign bit (i.e., replicate bit[7] 24 times) followed by the 8-bit number itself.
```verilog
module top_module (
    input [7:0] in,
    output [31:0] out );
    assign out = { {24{in[7]}} , in };
endmodule
```

# More replication
Given five 1-bit signals (a, b, c, d, and e), compute all 25 pairwise one-bit comparisons in the 25-bit output vector. The output should be 1 if the two bits being compared are equal.

out[24] = ~a ^ a;   // a == a, so out[24] is always 1.
out[23] = ~a ^ b;
out[22] = ~a ^ c;
...
out[ 1] = ~e ^ d;
out[ 0] = ~e ^ e;

[![Vector5.png](https://hdlbits.01xz.net/mw/images/a/ac/Vector5.png)](https://hdlbits.01xz.net/wiki/File:Vector5.png)

As the diagram shows, this can be done more easily using the [replication](https://hdlbits.01xz.net/wiki/Vector4 "Vector4") and concatenation operators.
- The top vector is a concatenation of 5 repeats of each input
- The bottom vector is 5 repeats of a concatenation of the 5 inputs
```verilog
module top_module (
    input a, b, c, d, e,
    output [24:0] out );//

    // The output is XNOR of two vectors created by 
    // concatenating and replicating the five inputs.
    // assign out = ~{ ... } ^ { ... };
    assign out = ~{ {5{a}},{5{b}},{5{c}},{5{d}},{5{e}} } ^ { {5{a,b,c,d,e}} }; 
endmodule
```