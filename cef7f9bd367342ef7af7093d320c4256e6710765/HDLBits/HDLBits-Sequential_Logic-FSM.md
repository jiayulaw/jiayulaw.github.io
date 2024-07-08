# FSM1 - Asynchronous reset
```verilog
module top_module(
    input clk,
    input areset,    // Asynchronous reset to state B
    input in,
    output out);//  

    parameter A=0, B=1; 
    reg state, next_state;
    always @(*) begin    // This is a combinational always block
        // State transition logic
        case (state)
            A: next_state = in ? A:B;
            B: next_state = in ? B:A;
        endcase
    end
    always @(posedge clk, posedge areset) begin    // This is a sequential always block
        // State flip-flops with asynchronous reset
        if (areset == 1) state = B;
        else state = next_state;
    end
    // Output logic
    assign out = (state == B);
endmodule
```

# FSM1 - Synchronous reset
```verilog
// Note the Verilog-1995 module declaration syntax here:
module top_module(clk, reset, in, out);
    input clk;
    input reset;    // Synchronous reset to state B
    input in;
    output out;//  
    reg out;
    // Fill in state name declarations
	parameter A=0, B=1; 
    reg present_state, next_state;
    always @(posedge clk) begin
        if (reset) begin  
            // Fill in reset logic
            present_state = B;
            out = 1'b1;
        end else begin
            case (present_state)
                // Fill in state transition logic
                A: next_state = in ? A: B;
                B: next_state = in ? B : A;  
            endcase
            // State flip-flops
            present_state = next_state;   
            case (present_state)
                // Fill in output logic
                A: out = 1'b0;
                B: out = 1'b1;
            endcase
        end
    end
endmodule
```


# FSM2 - Asynchronous reset
```verilog
module top_module(
    input clk,
    input areset,    // Asynchronous reset to OFF
    input j,
    input k,
    output out); //  

    parameter OFF=0, ON=1; 
    reg state, next_state;

    always @(*) begin
        // State transition logic
        case (state)
            ON: next_state = k ? OFF:ON;
            OFF: next_state = j ? ON:OFF;
        endcase
    end

    always @(posedge clk, posedge areset) begin
        // State flip-flops with asynchronous reset
        if (areset == 1) state = OFF;
        else state = next_state;
    end

    // Output logic
    assign out = (state == ON);

endmodule
```


# FSM2 - Synchronous reset
```verilog
module top_module(
    input clk,
    input reset,    // Synchronous reset to OFF
    input j,
    input k,
    output out); //  

    parameter OFF=0, ON=1; 
    reg state, next_state;

    always @(*) begin
        // State transition logic
        case (state)
            ON: next_state = k ? OFF:ON;
            OFF: next_state = j ? ON:OFF;
        endcase
    end

    always @(posedge clk) begin
        // State flip-flops with synchronous reset
        if (reset ==1) state = OFF;
        else state = next_state;
    end

    // Output logic
    assign out = (state == ON);

endmodule
```


# Simple state transitions 3
```verilog
module top_module(
    input in,
    input [1:0] state,
    output [1:0] next_state,
    output out); //
    parameter A=0, B=1, C=2, D=3;

    // State transition logic: next_state = f(state, in)
    always @ (*) begin
        case (state)
            A: next_state = in ? B:A;
            B: next_state = in ? B:C;
            C: next_state = in ? D:A;
            D: next_state = in ? B:C;
        endcase
    end
    // Output logic:  out = f(state) for a Moore state machine
    assign out = (state == D);
endmodule
```

# Simple one-hot state transitions 3
```verilog
module top_module(
    input in,
    input [3:0] state,
    output [3:0] next_state,
    output out); //

    parameter A=0, B=1, C=2, D=3;

    // State transition logic: Derive an equation for each state flip-flop.
    assign next_state[A] = state[A] & ~in | state[C] & ~in;
    assign next_state[B] = state[A] & in | state[B] & in | state[D] & in;
    assign next_state[C] = state[B] & ~in | state[D] & ~in;
    assign next_state[D] = state[C] & in;

    // Output logic: 
    assign out = (state[D] == 1);

endmodule
```

# FSM3 - Asynchronous reset
> I think the website solution has issue.

```verilog
module top_module(
    input clk,
    input in,
    input areset,
    output reg out); //
    parameter A = 0, B = 1, C = 2, D = 3;
    reg state, next_state;

    // State transition logic
    always @ (*) begin
        case (state)
            A: next_state = in ? B:A;
            B: next_state = in ? B:C;
            C: next_state = in ? D:A;
            D: next_state = in ? B:C;
        endcase
    end

    // State flip-flops with asynchronous reset
    always @ (posedge clk, posedge areset) begin
        if (areset == 1) state = A;
        else state = next_state;
    end

    // Output logic
    assign out = (state == D);

endmodule
```


# Design a Moore FSM
Using one-hot FSM encoding produces simpler logic.
```verilog
module top_module (
    input clk,
    input reset,
    input [3:1] s,
    output fr3,
    output fr2,
    output fr1,
    output dfr
); 
    parameter A, B, C, D, E, F;
    reg state, next_state;
    always @ (*) begin
        case (state)
            // Below S1, lower than previous level
            A: 
            // Between S1 and S2, lower than previous level
            B:
            // Between S1 and S2, not lower than previous level
            // Between S2 and S3, lower than previous level
            // Between S2 and S3, not lower than previous level
            // Above S3, not lower than previous level
            
        endcase
        
    end
    
    always @ (posedge clk) begin
        if (reset == 1) 
        else
    end

endmodule
```