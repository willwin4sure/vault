## Verilog Values

In Verilog there are four possible values:

* A ==logical 0== or a ==logical 1==.
* X: ==undefined==, meaning it can't be determined by simulation.
* Z: ==high-impedance==, meaning it is driven by something else.

In real life, all these values will actually realize as either a 0 or a 1. This is one useful thing about simulation: you will see Xs appear instead, telling you something is likely wrong!

Equality checks will pad 0s (or 1s) as needed. There are two types: `==` only compares 1's to 0's, while `===` compares all four types of values (so two undefined values will be equal).

## Latches and Flip-Flops

A naive way to try to hold state is to just put a loop in your circuit. However, this doesn't work, because it will just degrade back into an analog circuit.

The ==D latch== is a level-triggered sample-and-hold device, while the ==D flip-flop== is a edge-triggered sample-and-hold device.

A latch by itself is basically never good: when you open the door, the circuit degrades into analog chaos again. So what you actually need is two doors, so it's never left transparent. This is the construction of a D flip-flop.

Pulling back the curtains, here's the internals of a D-latch, which just consists of NAND gates with loops.

![[dlatch.png|center|256]]

Meanwhile, a D flip-flop consists of two of these latches with opposing E signals:

![[dflipflop.png|center|384]]

## Debouncer

Switches are mechanical devices. When they open/close, they actually bounce, if you measure the signal on an oscilloscope.

We need to fix this problem so that when a human presses a button, it doesn't actually count 9 presses.

The idea is relatively simple: you have a value that you are propagating. Then, you have a counter; if the value switches, you'll start the counter and if it hits a certain number you'll actually propagate the change in the value. If the value keeps switching, you'll restart the counter each time, so the propagation doesn't happen until the end.

Here's the module:

```verilog
module debouncer #(
	parameter CLK_PERIOD_NS = 10,
	parameter DEBOUNCE_TIME_MS = 5
) (
		input wire clk,
		input wire rst,
		input wire dirty,
		output logic clean
	);
	
	// explicit cast to int since Vivado $ceil keeps as float
	localparam DEBOUNCE_CYCLES = int'($ceil(DEBOUNCE_TIME_MS * 1_000_000 / CLK_PERIOD_NS));
	localparam COUNTER_SIZE = $clog2(DEBOUNCE_CYCLES);
	
	logic [COUNTER_SIZE-1:0] counter;
	logic old_dirty;
	
	always_ff @(posedge clk) begin
		old_dirty <= dirty;
		if (rst) begin
			counter <= 0;
			clean <= dirty;
		end else begin
			if (dirty != old_dirty) begin
				counter <= 0;
			end else if (counter < DEBOUNCE_CYCLES) begin
				counter <= counter + 1;
			end else begin
				clean <= dirty;
			end
		end
	end
	
endmodule
```

## Data Deserializer

You build something like this when writing SPI in Lab 2.

![[data_deserializer.png|center|512]]

This takes in data sequentially on a data clock and then spits it out in parallel once it's all assembled.

We all wrote code using an indexable array, and then Joe got mad at us during lab checkoffs. 

> [!warning]
> Think like a hardware engineer!

A more natural "data structure" to use is a ==shift buffer==! We don't need the "random" access property of the indexable array for this situation.

## Combinational Glitches

Arise when outputs transition through unintended outputs in response to transitioning inputs.

You can get intermediate values that are not the starting or ending values, or even values that are outside of the static truth table of the circuit!

## Digital Device Contracts

You learned about contamination and propagation delays in 6.191. These are part of the contracts between circuit components to help us ensure consistency on our digital devices.

Once we introduce registers (flip-flops) into the mix, there are also the setup and hold times. The setup time is how long each input has to be hold stable before the rising edge of the clock, while the hold time is how long the input must remain stable after the rising edge of the clock.

Suppose there is a `reg1` wired into sequential `logic` wired into `reg2`. Then recall the inequalities

$$
t_{\text{CLK}}\geq t_{\text{PD, reg1}}+t_{\text{PD, logic}}+t_{\text{SETUP, reg2}}
$$

and

$$
t_{\text{CD, reg1}} + t_{\text{CD, logic}}\geq t_{\text{HOLD, reg2}}.
$$

The first inequality is usually the relevant one. That's why we want to minimize the length of the critical path between registers in processors; that allows us to clock them faster. This is also why we pipeline certain operations, so cut their propagation delay down the middle.

Here's a Verilog module and its instantiation:

```verilog
module simple_counter(
		input wire clk,
		input wire rst,
		input wire evt,
		output logic [15:0] count
	);
	always_ff @(posedge clk) begin
		if (rst) begin
			count <= 16'b0;
		end else if (evt) begin
			count <= count + 1;
		end
	end
endmodule

simple_counter msc(
	.clk(clk),
	.rst(counter_reset),
	.evt(clk),
	.count(count)
);
```

See what's wrong with it?

The problem is you're trying to count the `clk` event while your flip flops trigger on the `clk` itself. So you're not satisfying the digital contract. In particular, you're not honoring the setup time of the flip-flop: the `clk` is not stable when you try to count it as an `evt`.

Another issue though, is that you can have asynchronous inputs in sequential systems, e.g. button presses. After all, if you didn't have I/O, Vivado would optimize your entire circuit away! When these cause a setup/hold violation (i.e. you change something right on a rising clock edge), all sorts of things could happen:

1. Transition is missed on the front clock cycle, but caught on the next clock cycle.
2. Transition is caught on first clock cycle.
3. Output is ==metastable== for an indeterminate amount of time.

The way to fix this is to just chain a bunch of flip flops in a row: the hope is that the metastability will resolve itself by the next clock cycle (for the next flip flop to resolve), etc.

