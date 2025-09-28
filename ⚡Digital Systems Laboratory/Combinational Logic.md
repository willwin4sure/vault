Our FPGAs consist of around 8500 so called ==logic slices==, depicted below:

![[logic_slice.png|center|384]]

Each logic slice consists of four ==configurable logic blocks== (CLBs), which are highlighted above. These are the programmable aspects; you can load any 6-bit truth table onto it. If you want larger $n$-bit functions, you'll have to use multiple logic blocks and wire/mux them together.

## Variables in Verilog

We use the `logic` type for our basic variable. It can represent two different things:

* A `wire`, the literal routed output of some logic.
* A `reg`, a device that can hold a value over time (form of memory).

SV has other datatypes (e.g. `int`, `short`, etc.) but we'll ignore them (until we get to loops). `logic` variables can be any size, and are by default unsigned.

You can make multi-bit variables using inclusive slice indexing:

```verilog
logic [7:0] a;     // 8-bit value
logic [12:0] c,d;  // both 13-bit values!
```

You can also make "2D" arrays. There are two methods: packed/unpacked.

```verilog
logic [7:0] arr0;        // packed
logic [7:0] arr1 [2:0];  // unpacked of packed
logic [2:0][7:0] arr2;   // packed of packed
```

When packed (like `arr2`), the whole structure is continuous, like a subdivided larger array (you could do 1D indexing across elements!). Whereas unpacked means that they are separate (i.e. not continuous).

Some examples:

1. You may want to use an unpacked array to handle the output of three separate adders (their results don't belong in the same bitstream).
2. You may want to use a packed array for some sort of bitmap data structure, or a string object.

This is largely a programming construct, and may not determine how the synthesized circuit actually looks.

## Assignments

When you assign something large to something smaller, it will just take the lowest set of bits and cut off the top.

You can implicitly assign along with your declarations, but it is recommended to separate the *existence* of variables from *how they behave*.

Inside `always` blocks, the blocking statements are evaluated in sequence. For example,

```verilog
logic [3:0] a, b, c;
always_comb begin
	a = 4'b1010;
	a = a + b;
	a = a + c;
end
```

behaves the same as

```verilog
assign a = 4'b1010 + b + c;
```

You can also write case statements!

```verilog
logic [8:0] a;
logic [1:0] b;

always_comb begin
	case(a)
		8'hF0 : b = 2'b0;
		8'hA1 : b = 2'b1;
		8'h08 : b = 2'b10;
		default : b = 2'd3;
	endcase
end
```

There is no fall-through in Verilog, so also no need for break statements! This is better than a long-chained if-else chain since it doesn't encode any priority between the branches, which could lead to a large $t_{\text{pd}}$ if the toolchain struggles to synthesize it well.

## History of `always`

We stick with three types of always blocks:

1. `always_comb`
2. `always_ff`
3. `always_latch`

Why are they named this way? Well, historically, there was just a single `always` block and it would infer different types of logic based on its sensitivity list, and what happened in the block!

```verilog
always @(<sensitivity list>) begin
	// stuff here happens when anything
	// in the sensitivity list changes
end
```

To prevent typos in the sensitivity list, they added a "wildcard" in Verilog 2001, and writing `always @(*)` would just trigger the internal logic whenever anything in the block changes.

> [!problem] (Issue with `always @(*)` blocks)
> Say you want a combinatorial circuit that says `z = x + y` whenever `x == 3`. So you write down the following Verilog:
> 
> ```verilog
> always @(*) begin
> 	if (x == 3) begin
> 		z = x + y;
> 	end
> end
> ```

This doesn't actually do what you want. Why? Well, we didn't specify what happens when `x != 3`! And a natural default assumption would be that you don't want to change `z`.

However, this logic is no longer combinational, so Verilog will add a D-latch to your code!

A D-latch has two inputs: `D` and `E`, and a single output `Q`. When `E` is high, `D` will flow through to `Q` in a combinatorial sense. And when `E` is low, the latch will hold `Q` stable. So it is a *level-triggered* sample-and-hold device, as opposed to the flip-flop, which is *edge-triggered*.

So if you specify `always_comb`, Vivado will specify warnings when it tries to synthesize a latch for you.

> [!idea]
> Never use `always`! A lot of legacy code will contain it, but you shouldn't use it.

If you have competing assignments, one will be chosen, while the other ignored. It will not make a union or merge them!

## Parameters

These are different than variables. Their values can change, but only at the compile-stage. At run-time, they are just constants. They allow us to make flexible designs (e.g. an adder with shared code for 8 bits or 14 bits).

Please specify them in screaming snake case. There are two types of parameters: `localparam` and `parameter`. `localparam` is local to the module it exists in. `parameter` is local, but (depending on context) can be a configuration setting.

```verilog
localparam GOOD = 8'b1111_1111;
localparam STATE_SIZE = 8;
parameter BAD = 8'b1111_0000;

logic [STATE_SIZE-1:0] state;
logic [1:0] output;

always_comb begin
	case (state)
		GOOD : output = 2'b11;
		BAD  : output = 2'b00;
		default : output = 2'b10;
	endcase
end
```

Parameters can be based on other parameters, e.g.

```verilog
parameter NUM_CHICKENS = 167;
parameter CHICKEN_WIDTH = $clog2(NUM_CHICKENS);
```

Here `clog2` means to take the ceiling of the base-2 logarithm.

Here's how you make a parameterized module:

```verilog
module add_constant #(parameter TO_ADD = 12) (
		input wire [7:0] val_in,
		output logic [7:0] val_out);
	assign val_out = val_in + TO_ADD;
endmodule

module top();
	logic [7:0] a, b, c, d;
	assign a = 8'd11;
	assign c = 8'b100;
	
	add_constant ac_0 (.val_in(a), .val_out(b));
	add_constant #(.TO_ADD(5)) ac_1 (.val_in(c), .val_out(d));
endmodule
```

> [!warning]
> Operator precedence is generally derived from C, but there are some that are out-of-order, e.g. `+` has precedence over `>>`, so `100 + 8 >> 2` is `27`, not `102`.

## `for` loop

These aren't *in time*, they are *in space*, to avoid code duplication. It's mainly just in compile time to conveniently lay out hardware.

There are two types:

* ==Generate== for loops (for loops in a `generate` block),
* ==Regular== for loops.

Here's a regular for loop in an `always` block:

```verilog
logic [7:0] b [63:0];
logic [7:0] c [63:0];
logic [63:0] a;

always_comb begin
	for (integer i = 0; i < 64; i = i + 1) begin
		a[i] = b[i] > c[63 - i];
	end
end
```


If you need to create multiple assign statements, multiple `always`blocks, multiple instances of a module, or create many logics, you must use a generate for loop. Your iterating variable must be a `genvar` rather than an `integer`.

```verilog
generate
	genvar i;
	for (i = 0; i < 5; i = i + 1) begin: myloop
		logic[31:0] hi;
		assign hi = 32'hAAAAAAAA ^ i;
	end
endgenerate
```

So this is making a ton of variables `hi`, which can be accessed with `myloop[2].hi`, for example.

## `logic` v.s. `wire` v.s. `reg`

Don't use `reg` in this class! `logic` is nicer than `wire` in some cases since it prevents multi-driven nets, i.e. if one variable is assigned to by two things (for `wire`, it will just make a choice).

So why do we even use `wire` in module definitions at all? Here's the problem with `logic`:

```verilog
module thing(
		input logic [3:0] a,
		input logic [3:0] b,
		output logic [3:0] c
	);
	assign d = 4;
	assign c = a ^ b ^ d;
endmodule
```

The Verilog will synthesize this happily, and since `d` is undeclared, it will just assume that `d` is a one-bit wire.

To protect ourselves, we will use this directive at the top of our file:

```verilog
`default_nettype none
```

But now... if you have input/output `logic`s, their values are unspecified so they default to `none`, and an error is thrown!

So now you have to make the inputs `wire`s. But actually a lot of internal Vivado source code relies on the default nettype being `wire`! So at the bottom of your file, we tack on

```verilog
`default_nettype wire
```

