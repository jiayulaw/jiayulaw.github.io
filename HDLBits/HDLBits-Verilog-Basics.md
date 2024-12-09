https://hdlbits.01xz.net/wiki/Notgate

# Inverter / Notgate
![](Pasted%20image%2020240505234534.png)
```verilog
module top_module( input in, output out );
assign out = !in;
endmodule
```


# Norgate
![](Pasted%20image%2020240505231748.png)
```verilog
module top_module( 
    input a, 
    input b, 
    output out );
    assign out = !(a || b);
endmodule
```

# Xnorgate
```verilog
module top_module( 
    input a, 
    input b, 
    output out );
    assign out = !(a ^ b);
endmodule
```

