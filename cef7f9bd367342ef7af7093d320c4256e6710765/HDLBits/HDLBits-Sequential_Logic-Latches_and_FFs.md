# Asynchronous reset
```verilog
module top_module (
    input clk,
    input areset,   // active high asynchronous reset
    input [7:0] d,
    output reg [7:0] q
);
    always @(posedge clk or posedge areset) begin
        if (areset == 1) begin
            q [7:0] = 0; 
        end
        else begin
            q [7:0] = d [7:0];
        end
    end
endmodule
```



# DFF and gates
![](Pasted%20image%2020240603105053.png)
```verilog
module top_module (
    input clk,
    input x,
    output z
);
    reg q1, q2, q3 = 0;
    reg qb1, qb2, qb3 = 1;
    //wire qb1 = !q1, qb2 = !q2, qb3 = !q3;
    always @ (posedge clk) begin
        q1 = x ^ q1;
        qb1 = !q1;
        q2 = x & qb2;
        qb2 = !q2;
        q3 = x | qb3;
        qb3 = !q3;
    end
    assign z = !(q1 | q2 | q3);
    

endmodule
```


# Circuit from Truth Table
```verilog
module top_module (
    input clk,
    input j,
    input k,
    output Q);
    reg D, Qbar;
    always @ (posedge clk) begin
       Q <=  D;
       Qbar <= !D;
    end
    always @ (j or k or Q or Qbar) begin
        case ({j,k})
            2'b00: D <= Q;
            2'b01: D <= 0;
            2'b10: D <= 1;
            2'b11: D <= Qbar;
        endcase
    end

endmodule
```


# Posedge detector
```verilog
module top_module (
    input clk,
    input [7:0] in,
    output [7:0] pedge
);
    reg [7:0] old;
    always @ (posedge clk) begin
        for (int i = 0; i < 8; i++) begin
            if ((old[i] == 0) & (in[i] == 1)) begin
                pedge[i] = 1; 
            end
            else begin
                pedge[i] = 0;
            end
        end
        old = in;
    end
endmodule
```

# Edge detector
```verilog
module top_module (
    input clk,
    input [7:0] in,
    output [7:0] anyedge
);
    reg [7:0] old_in;
    always @ (posedge clk) begin
        for (int i = 0; i < 8; i++) begin
            if (!(in[i] == old_in[i])) begin
                anyedge[i] = 1;    
            end
            else begin
                anyedge[i] = 0;
            end
    	end
        old_in = in;
    end
endmodule
```

# Edge capture
```verilog
module top_module (
    input clk,
    input reset,
    input [31:0] in,
    output [31:0] out
);
    reg [31:0] old;
    always @ (posedge clk) begin
        for (int i = 0; i < 32; i++) begin
            if (reset == 1) 
                out[i] = 0;
            else if ((old[i] == 1) & (in[i] == 0)) begin
                out[i] = 1;
            end else
                out[i] = out[i];
        end
        old = in;
    end
endmodule
```

# Dual-edge FF
![](Pasted%20image%2020240603154956.png)
https://stackoverflow.com/questions/19605881/triggering-signal-on-both-edges-of-the-clock
```verilog
module top_module (
    input clk,
    input d,
    output reg q
);
    reg trig1, trig2;
    always @ (posedge clk) begin
       trig1 = d ^ trig2; 
    end
    always @ (negedge clk) begin
       trig2 = d ^ trig1; 
    end
    assign q = trig1 ^ trig2;
endmodule
```