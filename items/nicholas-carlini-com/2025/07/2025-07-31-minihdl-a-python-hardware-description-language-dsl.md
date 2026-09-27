---
title: 'miniHDL: A Python Hardware Description Language DSL'
link: https://nicholas.carlini.com/writing/2025/minihdl-python-hardware-description-dsl.html
source: nicholas-carlini-com
published: 2025-07-31T00:00:00Z
updated: 2025-07-31T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A small hardware description language implemented as a DSL on Python, with a small 170 LoC 32-bit RISC CPU.
content: extracted
html: 2025-07-31-minihdl-a-python-hardware-description-language-dsl.html
---

[Hardware description languages](https://en.wikipedia.org/wiki/Hardware_description_language) (like [Verilog](https://en.wikipedia.org/wiki/Verilog) or [VHDL](https://en.wikipedia.org/wiki/VHDL)) have always confused me. This is despite the fact that they're just another programming language, albeit one that describes how to connect logic gates together instead of how to perform some dynamic computation. Probably this is just a skill issue on my part. I decided a while ago that I wanted to learn this dark art, and since the best way to learn anything is to build it from scratch, in this article I want to walk through the design of [miniHDL](https://github.com/carlini/miniHDL), a python domain-specific language I wrote. In order to be as simple as possible, this HDL is meant to generate circuits that work in a determinsitic simulator; lets me write much simpler idealized circuits than you can write in practice. [\[a\]](https://nicholas.carlini.com/writing/2025/minihdl-python-hardware-description-dsl.html#footnote1) For example, my clock will be some not gates connected toether, the HDL it doesn't care about clock domain crossing, can't represent unknown X or Z states, etc etc. In fact, if you know what those words mean, you probably are going to have a bad time reading this post.

This is the first in a two-part series of articles. I mainly wrote this article because the next part I'm preparing to upload just crossed ten thousand words... and so I decided to pull this HDL piece out of it to make the second part more readable. So look forward to that coming soon. For now it's top secret though.

I'll spend the first half of this post introducing the DSL, and then using it to build a simple 32-bit CPU in ~a hundred lines of code. Below I've added a visual representation of the circuit running a short program that computes successive Fibonacci numbers in registers R1 and R2. Each cell in the grid below represents one logic gate. I've clustered the logic gates according to the function that they're performing, and view what logic gates it's receiving data from, and what logic gates it's pushing data out to. (Below that, I've pulled out some named values that are particularly interesting, like the values of the registers, the currently executing instruction, and ALU inputs and outputs.

Loading circuit...

### miniHDL Architecture

The entirety miniHDL is, at the moment, [212 lines of Python](https://github.com/carlini/miniHDL/blob/main/minihdl.py). It defines two classes: `Bit`, and `Bit`s. The `Bit`s class is just a `List[Bit]` and so there's not much to say about it for now. A `Bit` is the fundamental primitive, and corresponds to the value held at a single location on a physical electronic circuit. Each `Bit` keeps track of how its value is computed, as a function of other `Bit`s.

class Bit ():

    signals = \[\]

    def \_\_init\_\_ ( self, how):

        self. how = how

        self.signals.append( self)

        self. uid = len ( self.signals)

    def \_\_and\_\_ (a, b):

        return Bit(( '&', a, b))

    def \_\_xor\_\_ (a, b):

        return Bit(( '^', a, b))

    def connect ( self, other):

        self. how = other.uid

For example, if I have `a = Bit()` and `b = Bit()` then I can write `c = a & b` to calculate the bitwise AND of these two Bits. (And similarly for OR, or XOR, or NOT operations.) The resulting value `c` then remembers that its two inputs are `a` and `b`.

The final piece of the program is then the “compiler” that takes in a bunch of `Bit`s, and converts this to a text file that describes which gates should connect to which other gates. This text file can then be processed by the simulator, which maintains a large array of the state of each gate, and updates the state of each gate depending on the state of all others.

def export\_gates (f, signals):

    for i, signal in enumerate (signals):

        if signal.how\[0\] == 'VAR':

            f.write(f "out {i} = out {signal.how\[1\]} \\n ")

        elif signal.how\[0\] == '~':

            f.write(f "out {i} = ~out {signal.how\[1\].uid} \\n ")

        elif signal.how\[0\] == '&':

            f.write(f "out {i} = out {signal.how\[1\].uid} & out {signal.how\[2\].uid} \\n ")

        elif signal.how\[0\] == '|':

            f.write(f "out {i} = out {signal.how\[1\].uid} | out {signal.how\[2\].uid} \\n ")

        elif signal.how\[0\] == '^':

            f.write(f "out {i} = out {signal.how\[1\].uid} ^ out {signal.how\[2\].uid} \\n ")

Taken together, this lets you write nice Python code that corresponds to circuits you might want to use. Here, for example, is the implementation of a simple ripple-carry full-adder on the left. And when the HDL compiler lowers this when adding two four-bit numbers, you end up with the following circuit (assuming that the arguments are passed in v\[0..3\], v\[4..7\], and v\[8\].

```
v[9] = v[8]
v[10] = v[0] ^ v[4]
v[11] = v[10] ^ v[9]
v[12] = v[0] & v[4]
v[13] = v[10] & v[9]
v[14] = v[12] | v[13]
v[15] = v[1] ^ v[5]
v[16] = v[15] ^ v[14]
v[17] = v[1] & v[5]
v[18] = v[15] & v[14]
v[19] = v[17] | v[18]
v[20] = v[2] ^ v[6]
v[21] = v[20] ^ v[19]
v[22] = v[2] & v[6]
v[23] = v[20] & v[19]
v[24] = v[22] | v[23]
v[25] = v[3] ^ v[7]
v[26] = v[25] ^ v[24]
v[27] = v[3] & v[7]
v[28] = v[25] & v[24]
v[29] = v[27] | v[28]
```

def full\_add (in1, in2, in3):

    low1 = in1^in2

    low2 = low1^in3

    high = (in1 & in2) | (low1 & in3)

    return low2, high

def add (in1, in2, carry):

    out = \[\]

    for a,b in zip (in1, in2):

        c, carry = full\_add(a, b, carry)

        out.append(c)

    return Bits(out)

The above full adder is fairly standard compared to what you might see in any other HDL language, but I find it a lot easier to understand when presented in this way. Things become a tiny bit more complicated when you need to introduce cyclic dependencies in the computation graph. For this, you need to explicitly construct a Bit, and then set its value after having defined it. Let me give you an example for this:

def dff\_half (inp, clock):

    q = Bit()

    out = mux(clock, iftrue=inp, iffalse=q)

    q.connect(out)

    return q

def dff (inp, clock):

    q = dff\_half(inp, ~clock)

    q = dff\_half(q, clock)

    return q

Before we start implementing things, let me briefly explain what a D flip-flop is and why it's important. A [D-flip-flop](https://en.wikipedia.org/wiki/Flip-flop_\(electronics\)) is a fundamental building block in digital circuits that can store exactly one bit of information. Think: a single cell of memory that can remember either a 0 or 1 until you give it new data: whatever data you present to its input will be stored when the clock signal triggers it.

Without flip-flops, digital circuits would be purely linear---data would flow through logic gates from inputs to outputs and quickly settle into a steady state. There would be no way to set a variable, and then reference it in the future. Flip-flops enable temporal logic by introducing memory into the circuit, and this is what lets us to counters, state machines, and ultimately, the registers and program counters that make CPUs possible. At the most basic level, all a flip-flop does is multiplex to set the *next* state either as the currently held state, or whatever new input is being passed in. A full D flip-flop actually has two back-to-back flip-flops to ensure that we only grab the value of the input exactly as it was on the rising edge, and then hold that value until the next clock tick. If we didn't have them back to back in this way, it wouldn't be much of a flip-flop, because it would only hold the value when the clock was 0: when the clock was 1, it would constantly be updating the output to whatever the input currently was set to.

Notice, here, how the output `q` is first defined as a `Bit` with no value. We then have the variable output, which is either equal to the input value (when the clock is high), or the as-of-yet-undefined value `q` (when the clock is low). (The `mux` function is a multiplexor, defined just as `(iftrue & clock) | (iffalse & ~clock)`.) Then, we actually assign the value of `q` to the output of the multiplexor.

For this flip-flop to work, we're going to need a clock. Unlike a real chip that has to use some physical crystal or something to get a reliable clock signal, I can just create a long chain of `Bit`s, each of which is connected to the next, and then put a NOT gate at the end. That is, this looks basically like an inverting ring oscillator but it's much simpler. That looks something like this:

def clock (latency):

    inv = ~out

    delays = \[Bit() for \_ in range (latency)\]

    for a,b in zip (delays,delays\[1:\] + \[delays\[0\]):

        b.connect(a)

    return delays\[-1\]

The final component of miniHDL is a class `Bits`. All that this class does is directly pass through any function to the inner list of `Bit` objects inside of it.

class Bits:

    def \_\_init\_\_ ( self, bits):

        self.bits = bits

    def \_\_and\_\_ (a, b):

        if isinstance (b, Bit):

            return Bits(\[x & b for x in a.bits\])

        return Bits(\[x & y for x,y in zip (a.bits, b.bits)\])

    def \_\_or\_\_ (a, b):

        if isinstance (b, Bit):

            return Bits(\[x | b for x in a.bits\])

        return Bits(\[x | y for x,y in zip (a.bits, b.bits)\])

    def connect ( self, other, reuse= False):

        \[x.connect(y, reuse) for x,y in zip ( self.bits, other.bits)\]

    def clone ( self):

        out = Bits(\[Bit() for \_ in range ( len ( self.bits))\])

        out.connect( self)

        return out

This means that you can do things like create `dff(Bits(...), clock)` without having to think about the fact that you're operating on a whole block of `Bit`s at the same time.

### From Basic Gates to a Complete CPU

So far, we've built up a collection of basic components: logic gates for computation, flip-flops for memory, and a clock to make everything tick at the same time. At a very high level, we turn these simple building blocks into something as complex as a CPU by making the observation that a CPU is fundamentally just a state machine that repeatedly executes a simple cycle: fetch an instruction from memory, decode what operation it represents, execute that operation, and store the result. Each of these steps can be built from our basic components.

A RISC CPU makes each of these steps extremely explicit, and so I'll design here a simple but complete 32-bit CPU to do just that. We'll need several key components: a register file to store data, an ALU (Arithmetic Logic Unit) to perform operations, a program counter to track which instruction we're executing, and instruction memory to store our program. By connecting these components together with the right control logic, we'll have a working processor.

### CPU Architecture Overview

Let me define the architecture of our CPU. This is a RISC-style processor with 32-bit words: all registers and data paths are 32 bits wide. It has 16 general-purpose registers numbered R0-R15, and each instruction can access any register. The instruction format uses fixed 32-bit instructions with three formats: Register-Register operations like `Rd = Rs OP Rt` (e.g., R3 = R1 + R2), Register-Immediate operations like `Rd = Rs OP constant` (e.g., R1 = R1 + 5), and Branch operations that jump if a register is non-zero. The ALU supports operations including ADD, SUB, AND, OR, XOR, NOT, and bit shifts. For memory, we have separate instruction memory (ROM) and the register file, but there's no data memory in this CPU, so your program had better use only 16 words of RAM!

The instruction encoding uses the first byte for control: bits 0-3 specify the ALU operation, bit 4 selects between register-register and register-immediate modes, and bit 5 indicates if this is a branch instruction. The remaining 24 bits encode the operands.

As an example, here's a simple program for our CPU (the program that's running at the top of this page).

  LD 1, 0  # set r1 to 0 (0th Fib number)

  LD 2, 1  # set r2 to 1 (1st Fib number)

loop:

  ADD 3, 1, 2  # Fib update: c=a+b

  MOV 1, 2  # move fib\_{i} = fib\_{i+1}

  MOV 2, 3  # move fib\_{i+1} = fib\_{i+2}

  JMP loop  # repeat

### Building the CPU Components

Let's get started designing the circuit for this. To start we'll define a n-way multiplexer, where instead of just looking at a single bit `b` and deciding to either take the left or right piece, we'll take a n-bit condition, and use that to select from a corresponding element of a list.

def muxn (cond, choices):

    if len (choices) == 1: return choices\[0\]

    return muxn(Bits(cond\[1:\]),

                \[mux(cond\[0\], b, a) for a,b in \\

                 zip (choices\[::2\], choices\[1::2\])\])

Notice, here how we just write natural python to operate on all the bits at once. This code probably looks horrific to hardware engineers, but to someone who's used to reading Python every day, I actually prefer code like this to something like VHDL that feels very arcane.

With this, we can build our program ROM, which is nothing more than a big giant lookup table that takes as the argument the current program counter, and picks which ROM value it wants to return as the output. We do this by just bit-slicing the input program ROM into 32 individual bits, and then putting a big multiplexor that, given the current program counter, retrieves the corresponding 32-bit instruction that is located at that address.

def rom (addr, arr, depth):

    if len (arr) < 2\*\* len (addr):

        arr = arr + \[0\] \* (2\*\* len (addr) - len (arr))

    zero = const(0)

    one = ~zero

    myconst = \[zero, one\]

    arr = \[Bits(\[myconst\[(x>>i)&1\] for i in range (depth)\]) \\

           for x in arr\]

    low\_byte = muxn(addr, arr)

    return low\_byte

Now let's build our first interesting component: the register file. This will be a function that holds the 16 registers in our CPU, and lets us read from two at a time, and write to one at a time. By making use of the components we've built so far, this actually is fairly straightforward, here's the entire program to do that:

def regfile (clock, read1, read2, write3, data, enable\_write):

    registers = \[\]

    for idx in range (16):

        ok = const(1)

        for bit in range ( int (math.log(16, 2))):

            if (idx>>bit)&1:

                ok &= write3\[bit\]

            else:

                ok &= ~write3\[bit\]

        update = clock & enable\_write & ok

        registers.append(dff(data, update, 32))

    out1 = muxn(read1, registers)

    out2 = muxn(read2, registers)

    return out1, out2, registers

We loop for each of the 16 registers in our CPU, and create 16 (32-bit) D Flip-Flops for each. The input to the flip flop is whatever value the current instruction wants to be writing into our register file, and we selectively write to that flip-flop if (1) `enable_write` is set, e.g., we're not executing a jump instruction, and (2) we're at the correct register index as determined by the `write3` argument. We put these 16 registers into a list, and then with two n-way multiplexors return the two output registers that we want to read as output.

And then the final component of our circuit is the ALU, which actually calculates the operation we want our CPU to perform. The ALU is nothing more than a big n-way multiplexor again, where we switch between the different values we might have wanted to compute: addition, subtraction, some boolean operations, and some bit shifts. The input to the ALU is always one register, and then either another register, or a constant, as determined by the fifth bit of the instruction opcode.

def alu (opcode, left, right, const\_bits, use\_const\_not\_right):

    right = mux(use\_const\_not\_right,

                const\_bits.upcast(32), right)

    alu\_out = muxn(opcode,

                   \[add(left, right, const(0)),

                    add(left, ~right, const(1)),

                    left & right,

                    left | right,

                    left ^ right,

                    \~left,

                    const\_bits.upcast(32),

                    left\[1:\].upcast(32),

                    Bits(\[const(0)\]+ left\[:-1\].value),

                    Bits(\[any\_set(left)\]\*32),

                    \])

The final thing we have to do is connect the output of the ALU back to the data input on the register (if it's a register write instruction), and look at the output of the ALU to decide what the next instruction should be (if it's a jump instruction).

data.connect(alu\_out)

next\_insn = mux(alu\_out\[-1\] & is\_jump,

                const\_bits.upcast(32), ip\_inc)

ip\_in.connect(next\_insn)

All in, [this CPU is about 170 lines of code](https://github.com/carlini/miniHDL/blob/main/examples/risc32/cpu.py), so really there's not much to it. But it's sufficient to do fairly interesting things, like calculate Fibonacci numbers or even do basic arithmetic like (multiply, divide, and calculate square roots). Notably absent are instructions to read or write to a larger memory (but this wouldn't be that hard, you'd just have to define a bigger chunk of memory), instructions that allow for anything indirect (e.g., indirect branches), or instructions that do more complex arithmetic operations directly (e.g., multiplication).

### Efficient Circuit Simulation

The provided simulator in [simulator.py](https://github.com/carlini/miniHDL/blob/main/simulator.py) (and shown at the top of this page) efficiently simulates the gates in any provided circuit. Naively this just means running through each of the gates one by one, and performing the corresponding operation, and then saving the result.

And while this would work quite well for small circuits, it would get rather slow on large circuits. (Like the ones I will build in the next post.) As has been said many times before, you can't make computers perform the same amount of work faster. You can only make them do less. And so I've implemented a caching mechanism so that on each iteration of the loop it only updates the gates need to be re-calculated because at least one of their inputs has changed. (If all inputs to the gate have remained the same, its output is also going to be the same.)

Instead of re-calculating every gate on every iteration of the loop, it keeps track of which inputs have changed last time, and then only re-calculates the gates whose input changed during the prior round. Doing this efficiently is rather easy: I just maintain a [heap](https://en.wikipedia.org/wiki/Heap_\(data_structure\)) that stores which gates need to be updated next, and then push and pop from the heap on each pass through the circuit. On reasonably large circuits, this gives 100x-500x performance boost on average.

### Future Work: VLSI layout/gate placement

The current project only supports a way to design circuits, and the output of this design is just a text file that says what gates should be connected to which other gates. This is enough for what I'm going to do it in the next post. (Something that's top secret for a bit longer.)

But in the future, I'd like to extend miniHDL to also have some code to actually physically lay out the gates onto a proper circuit board. My hope, maybe in a future part 3 (but no promises!), is that it'll be able to actually automatically convert a miniHDL program to a [Gerber](https://en.wikipedia.org/wiki/Gerber_format) file, that I could then get manufactured in order to make my own discrete transistor CPU. This might take some time, but it seems like something that would be fun to try.

### Coming Soon: Part 2

I don't want to spoil the surprise, but in Part 2 I'll have a very exciting use of all of this code I just built. So stay tuned. (Or, as the kids say, “like comment subscribe”..? just kidding this is the real Internet and not one of those walled gardens; we don't have those features here.) But, more seriously, I'd encourage you to try and go design some circuits with whatever HDL you find fits your style---I've had a lot of fun playing with this since I built it.
