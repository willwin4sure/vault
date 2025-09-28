## WTFPGA

An ==FPGA== is a giant array of very primitive logic blocks (i.e. "gates"), memory, and other specialized hardware, each of which are individually programmable. The giant array of logic exists in a huge sea of programmable interconnects. It can be repeatedly reprogrammed even once in a device, hence programmable "in the field".

Our FPGA in 6.205:

* Has roughly 33,000 programmable blocks.
* 150 18kb memory units
* 5 clock management tiles
* 120 dedicated multipliers
* Vast array of I/O devices

FPGAs are important when speed/latency and efficiency are important. They are essentially flexible hardware. It can be reprogrammed to have the application-specific benefits of an ASIC. It is much cheaper than designing a chip from scratch (and is often used in prototyping new chips), though it will be less performant.

## Digital Functions

We are in the digital world (as opposed to analog). The world is 1s and 0s, and a bit of noise is fine (recall that input thresholds are looser than output thresholds in gates).

There are two broad types of digital logic: functions (stateless), and storage (stateful). These correspond to combinational and sequential.

We want to be able to build functions $f:\{0, 1\}^{n}\to\{0,1\}$ in circuits. We can represent these functions with truth tables. Another way is to build them from basic operations, i.e.

* $\overline{x}$ for NOT $x$,
* $x|y$ for $x$ OR $y$,
* $xy$ for $x$ AND $y$.

These can be composed into more complicated expressions.

There are four one-bit functions: the ==buffer==, the ==inverter==, the one gate, and the zero gate. There are sixteen two-bit functions (familiar faces), and the number blows up astronomically.

Two special two-bit functions are NAND and NOR, since they are each computationally complete (e.g. you can make any function by wiring together NOR gates).

## Verilog/SystemVerilog

Theoretically, you can decompose every function into disjunctive normal form (i.e. sum of products), but this is really unwieldy. Nowadays, we use ==hardware description languages==, like SystemVerilog.

In this class, we'll use Verilog to synthesize onto a device, and then Python (`cocotb`) for simulating (Verilog could do simulation too).

For variables, we use `logic`, e.g.

```verilog
logic x,y,z;         // one-bit variables
logic [3:0] b;       // four-bit variable
logic [11:0] c,d,e;  // 12-bit variables
```

It's usage will determine what gets generated from it.

For values, we use the format `S'Txxxx_xxxx`, where `S` is the size in bits, `'` is a marker, `T` is the numerical base (`b`, `d`, or `h` for binary, decimal, or hex), and the rest is the value (`_` is ignored, just for spacing). If you don't use a size, it defaults to 32-bit (don't do this!).

Each bit can actually have four values:

* Logical 1 or 0.
* X, i.e. undefined.
* Z, i.e. high impedance.

The latter two actually *are* 0s or 1s in real life, they're just unspecified.

There are two ways to create pure digital functions:

```verilog
// first method
logic a, b, c;
assign c = a || b;

// second method
logic a, b, c;
always_comb begin
	c = a || b;
end
```

There are tons of way to write functions, e.g. the majority function of three bits:

```verilog
// solution 1, sum of products
logic x, y, z, out;
assign out = (!x && y && z) ||
             (x && !y && z) ||
             (x && y && !z) ||
             (x && y && z);
             

// solution 2, above but using concat 
logic x, y, z, out;
assign out = ({x,y,z} == 'b011) ||
             ({x,y,z} == 'b101) ||
             ({x,y,z} == 'b110) ||
             ({x,y,z} == 'b111);
             
// solution 3, chained ternary operator
logic x, y, z, out;
assign out = ({x,y,z} >= 5) ? 1 : ({x,y,z} == 3) ? 1 : 0;

// solution 4, above but more verbose
logic x, y, z, out;
always_comb begin
	if ({x,y,z} >= 5) begin
		out = 1'b1;
	end else if ({x,y,z} == 3'b101) begin
		out = 1'b1;
	end else begin
		out = 0;
	end
end

// solution 5, overriden assignment inside. this may be confusing...
logic x, y, z, out;
always_comb begin
	out = 0
	if ({x,y,z} >= 5) begin
		out = 1'b1;
	end else if ({x,y,z} == 3'b101) begin
		out = 1'b1;
	end
end
```

Generally, in Verilog, you should think of all your assigns as coexisting (i.e. they will run at the same time). But within `always_comb` blocks, the individual instructions generally can be thought of as running sequentially.

There are two delays you need to worry about for combinational circuits:

1. $t_{\text{cd}}$, the ==contamination delay==, i.e. the minimum time it takes for a change in the inputs to affect the output of the function.
2. $t_{\text{pd}}$, the ==propagation delay==, i.e. the maximum time it takes for a change in the inputs to affect the output of the function. The input must be held stable!

This is important, e.g., for sequential circuits with clocks. At each clock rising edge, you don't want to contaminate your outputs while saving to register, but you also need the computation to finish before the next clock hit.

Verilog also lets us compartmentalize our code with modules, e.g.

```verilog
// declare a module
module f1( input wire x,
		   input wire y,
		   input wire z,
		   output logic out);
		   
	assign out = ...
endmodule

// instantiate it!
logic q,r,t,cat;
f1 my_f1(.x(q), .y(r), .z(t), .out(cat));
```

This let's you duplicate things and attach them. Wiring modules sequentially adds their propagation delays, which lowers throughput. All this stuff can be solved with flip flops, driven with clocks (i.e. the idea pipelining)!

Remember a flip flop has two inputs (`D` and `clk`) and a single output (`Q`). At any point in time, it has a state characterized by holding a `0` or a `1` constant at its output `Q`. You can load in a "new value" at `D`, and on the next rising edge of `clk`, it will get swapped in for `Q`, updating the state.

```verilog
// pure combinational version
module h (input wire [7:0] y, input wire [7:0] z, output logic b);
	always_comb begin
		b = y > z;
	end
endmodule

// sequential version with a flip flop before output b
module h (input wire clk, input wire [7:0] y, input wire [7:0] z, output logic b);
	logic b_i;
	always_comb begin
		b_i = y > z;
	end
	
	always_ff @(posedge clk) begin
		b <= b_i;
	end
endmodule

// terser version (less old-school); harder to separate combinational and sequential
module h (input wire clk, input wire [7:0] y, input wire [7:0] z, output logic b);	
	always_ff @(posedge clk) begin
		b <= y > z;
	end
endmodule
```

Note that `=` is a ==blocking statement==, meaning that the line of code will only be evaluated after the previous line has been evaluated. Meanwhile, `<=` is a ==nonblocking statement==, meaning all the assignments in the block will be implemented in parallel once the end of the block is reached.

Here's an illustrative example:

```verilog
always_ff @(posedge clk) begin
	x = y || 1;
	z = x ^ u;
end
```

synthesizes to:

![[blocking.png|center|512]]

while

```verilog
always_ff @(posedge clk) begin
	x <= y || 1;
	z <= x ^ u;
end
```

synthesizes to:

![[nonblocking.png|center|512]]

One simple rule-of-thumb to avoid confusion is to only use `=` inside `always_comb` blocks and only use `<=` in `always_ff` blocks.

Note that a variable can refer to both its old and new versions (and the old version can influence the new version!). This demonstrates the need for the `<=` assignment:

```verilog
module counter(
		input wire clk,
		output logic [15:0] count
	);
	always_ff @(posedge clk) begin
		count <= count + 1;
	end
endmodule
```

This is a basic counter that increments on every clock rising edge.