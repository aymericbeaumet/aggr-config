---
title: Multiplexing Circuits on the Game of Life - Part 5
link: https://nicholas.carlini.com/writing/2022/multiplexing-circuits-game-of-life.html
source: nicholas-carlini-com
published: 2022-02-27T00:00:00Z
updated: 2022-02-27T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Abstract: Improving digital logic gates on Conway''s game of life by allowing 8-bit logic gates instead of boolean logic gates.'
content: extracted
html: 2022-02-27-multiplexing-circuits-on-the-game-of-life-part-5.html
preview:
  file: 2022-02-27-multiplexing-circuits-on-the-game-of-life-part-5.preview-dd145fc3c441.webp
  width: 256
  height: 178
  color: '#e4e4e2'
images:
- source: https://nicholas.carlini.com/writing/2022/gameoflife/explain_fib.png
  original:
    file: 2022-02-27-multiplexing-circuits-on-the-game-of-life-part-5.image-74061968726d.png
    width: 2270
    height: 1578
  color: '#f0f0f0'
- source: https://nicholas.carlini.com/writing/2022/gameoflife/old_adder.png
  original:
    file: 2022-02-27-multiplexing-circuits-on-the-game-of-life-part-5.image-6ddef0ad8429.png
    width: 1680
    height: 1530
  color: '#313131'
- source: https://nicholas.carlini.com/writing/2022/gameoflife/new_fib.png
  original:
    file: 2022-02-27-multiplexing-circuits-on-the-game-of-life-part-5.image-ba6039afa18b.png
    width: 1680
    height: 1530
  color: '#303030'
extra:
  content:interactive: true
---

This is the fifth in a series of articles ([\[1\]](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html), [\[2\]](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html), [\[3\]](https://nicholas.carlini.com/writing/2020/improved-logic-gates-game-of-life.html), [\[4\]](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html)) discussing implementing digital logic on Conway's game of life. This will also be the third time that I redesign everything from the ground up, completely re-thinking what a logic gate actually does.

By the end of this post, I'll describe how to construct circuits that are somewhere beteween 8 and 50 times more efficient than the implementation from the last rewrite in the [third post](https://nicholas.carlini.com/writing/2020/improved-logic-gates-game-of-life.html), which itself is at least another order of magnitude more efficient than the first initial construction. For example, the entire Fibonacci counter circuit shown below is *smaller* than just the four-bit adder from the first post!

* * *

So this is part five in this series. Why am I re-designing everything again? Well, the previous approaches, even as efficient as I could make them, still were to inefficient for my taste. I want better.

In each of the past four articles there was a direct one-to-one correspondence between a wire (in a physical electronic circuit) and a glider stream (on the game of life). If a glider stream is present, then the wire currently holds the value 1; and if there is no glider stream present then the wire holds the value 0.

To see why this setup is fairly ineffecient, consider for example how we might compute the AND of two 8-bit integers with our previous construction. We'd need eight wires coming from the left, eight wires coming from the bottom, and eight AND gates. A circuit that actually performs this computation is drawn at right, and has a cost of 64 “tiles” in an 8x8 grid. And only **eight** of these are actually filled with gates---the rest of the empty tiles are just used for moving the data from one location to another.

Now this design basically makes sense if you're trying to build things out of physical transistors. (Or vaccuum tubes, if you're into that.) Two-state binary logic is really easy for design purposes and just makes everything easier.

But we're not dealing with the physical world. So can we do better?

Our new construction is going to have no basis in physical reality, and would be impossible directly to implement with standard digital logic gates in hardware. But we don't care about the physical world---for some time now we've been designing circuits that would have been physically impossible to build in real hardware.

(It's not anything new that our circuits aren't physically realizable. Our clock, for example, has always just been a NOT gate connected into itself---which in the game of life generates a perfect clock signal but would just be all kinds of wrong if tried it in reality.)

In our new construction, instead of a wire representing a single value boolean of either true (1) or false (0), our wires are now going to carry eight bit integers. And so any wire at a given point can hold one of 256 distinct different values.

(For the purpose of this post, for the sake of clarity, I'm going to pretend that we're just working with 4-bit integers. It'll make everything simpler.)

The wire shown at right is carrying the value 3 in our new construction. The way that you can tell that it's holding this value is that when you look at the position of the gliders when the simulation is at time step zero mod 4, the presence or absence of gliders in the indicated location allows you to read off the binary value 0011, which decodes to the decimal value 3.

Noticed though that it's (very!) important we read off the value when the current time is 0 mod 4. If we had gotten this wrong and instead and read off the data when the current time was different we might have instead read the glider configuration 0110, which converts to 6, or we might have read 1100, which converts 12, or maybe even 1001, which converts to 9.

And this is actually going to be the main difficutly with multiplexing logic gates: we're going to have to keep track of the *phase* at every moment in time so we know how to read off the correct value from each wire.

How do we actually generate a signal's multiplexed? Well for this we need a gate that will generate a glider stream where only 1 out of every 4 (or 8, in practice, but 4 for this post) possible glider locations is filled in.

The example drawn at right does exactly this. All we have here is a LWSS gun that creates the output stream, and then we hit this with a quarter-period glider stream. I've timed the collision just right so that when they hit each other a little box is left over that will clear out the next LWSS and so we end up with a period of 4.

Performing digital logic on wires carrying eight bits is (almost) identical to performing digital logic on wires carrying Boolean values. That makes this new modification rather appealing, because we can actually (for the most part) use the same logic we built [last time](https://nicholas.carlini.com/writing/2020/improved-logic-gates-game-of-life.html).

For example right we see the exact same AND gate which we previously used to compute the AND of two Boolean values, but this time we're using it to compute the AND of two 4-bit values.

The input coming from the left is the decimal value 3 (0011), and the input coming from the bottom is the decimal value 6 (0110). And so the output we expect should be the decimal value 2, because 3 AND 6 = 0011 AND 0110 = 0010 = 2. And indeed we can see that it is by reading off the binary value 0010 when the phase is 0 mod 4.

The reason that this works is because the glider that represents the least significant bit coming from the left only ever interacts with the glider that represents the least significant bit on the wire coming from the bottom. Similarly, the gliders that represents the second-to-least significant bits only (potentially) hit each other, and so on for all eight glider pairs.

And so what we've basically done here is used the one logic gate to actually perform eight different ANDs at the exact same time. This is why I call this approach *multiplexing*, because we're using the same glider stream to perform different calculations at different time setps.

(“Now wait a second”, you say. “Why do we only have wires cary eight bits? Why not 10? Or 32?” That's actually a good question! There's no fundamental reason we couldn't carry either 10 (or 32) bits on the wire. We would just need to remind ourself that whenever we're reading off the result, we should be reading off when the current time step is 0 mod 10 or 0 mod 32.)

I've glossed over one very important problem when showing the above example. Let's look at what happens if we *naively* compute the AND of the value 0001 (coming in from the left) and the value 0001 (coming in from the bottom). We would expect the answer to again be 0001, because that's the right answer. Except it turns out that we get zero! (It only worked above because I cheated and used a fix we're about to develop.)

Why did this happen? Well when you look at the circuit it's actually obvious. The gliders aren't hitting at the right time to actually have the bits overlap in the right way, and so the glider on the left isn't actually allowed to pass through.

Now one thing we could try to do is declare the AND gate to be broken, and require that it makes gliders from the bottom interact exactly with the corresponding glider coming from the left. There are two reasons, though, that this won't work.

1. Most importantly, it complicates every single gate construction significantly. It was already hard enough to cram an OR gate into 150x150 game of life cells. Doing that while *also* keeping the phase aligned is probably impossible.
2. But also, this would bake into the construction of the OR gate that we wanted wires to hold 8 bits. For now I do. But what if in the future I wanted them to hold 27 bits? We'd have to completely re-design every gate just to allow this change.

Instead of fixing the problem “in hardware”, and modifying the construction of the gates, let's see if we can fix it “in software”, and modify the compiler that transforms circuits into the game of life.

To do this, we'll start by creating a new phase-fixing gate that doesn't actally change the value that's being carried along any wire, but instead just adds a little bit of latency which adjusts the phase of the wire. So for example, if we add these proper phase adjusting gates to the OR gate above then we get this here and everything works out perfectly.

Because it will be useful later, let's define the *latency* of a gate as the number of time steps it takes for a glider to exit the gate after entering it. So for example, if you're really patient, you could calculate, for example, that the latency of the AND gate is 10 gliders long from the left input to the output, or 11 gliders long from the bottom input to the output.

Does this work? Definitely. It does. But is it efficient? Not at all! In fact, it's going to quadruple the number of gates we must use in any circuit. Requiring four gates where one was required previously basically completely eliminates any gains from having a multi-phased wire setup.

But wait, it gets worse. This simple greedy approach for adding new gates works when you have acyclic circuits, but what happens when the circuits are actually cyclic?

Let's imagine that I was trying to build a circuit that has an OR gate connecting back onto itself, with the idea that I can use this as a kind of memory cell that can only be written to once.

This circuit might look like something at right. We send in the value 0001 from the left. And for a little while, everything is okay. But quckly we run into a problem! When the data from the OR gate comes back around the second time, the phase is wrong! And so we end up ORing the value 0001 from the left (as before) with the value 0010 (which is wrong!). This gives a new value of 0011 (or 3) going around the circuit.

And it only gets worse from here. Now we have the value 3 going around the circuit, which causes the resulting output to get ever further off, because next time we OR this incorrectly to get a 7 around the circuit, until eventually we have a full 15 traveling around the circuit.

What's going wrong here? Again this is a problem of phase. If we pause the circuit at 0 mod 4 and look at the output of the OR gate, we find that actually, the value has been shifted, and it's ORing 0001 from the left, but 0010 from the bottom. We're all out of phase!

Somehow, after going through the OR gate, we shouldn't be trying to decode things by 0 mod 4, we should be reading off the resulting value 1 mod .

The fix here can't be just *local* looking at the input and output of each gate in isolation. We also have to somehow track of what's going on at a *global* scale so that when we add a delay gate to one part of the circuit, we don't end up also breaking another part of the circuit. For example, the fix on the above circuit is to add a gate that delays the loop by 3 time steps longer than previously. This will make the the gliders require exactly 3 + 1 = 0 mod 4 time steps to travel around the loop, and so everything will run smoothly.

In general, how should we calculate were to add delay gates?

Let's start out by defining the variables and constraints that we're going to be working with. Starting with variables: every gate is going to have an input phase (for each of their inputs), and an output phase (for each of their outputs). And then we have a constraint: the phase at the output of a gate should be exactly equal to the phase at the input plus the latency from the input to the output.

Now these variables are not “free” variables that we can modify arbitrarily. If the input phase to a NOT gate is 0 mod 4, then the output has to be 14 mod 4 = 2 mod 4 because that's how long it takes the glider to travel through the gate. We can't do anything about that. It's just a fact about the gate.

We can express this with a linear equation not\_input\_phase + 14 ≡ not\_output\_phase (mod 4).

Now we can actually go through and do this for every single gate, for example the rotate gates have a latency of 13 mod 4 each and the duplicate gate has a latency of 10 from left-to-right and 11 from left-to-down.

Okay but that's not all the constraints we have. There is a wire that goes from the *output* of the NOT gate to the *input* of the first rotation gate. So we need to add a new constraint that these match: not\_output\_phase + 10 ≡ first\_rotate\_input\_phase (mod 4). (Where did this 10 come from? That's the phase-offset from just traveling through empty space.)

But now here's where we actuall have some slack. Because this is a wire, we have the option of adding some extra latecy with a delay gate. So in reality the equatino that we should have is more like not\_output\_phase + 10 + optional\_latency ≡ first\_rotate\_input\_phase (mod 4)

```
      0mod4_out_0 + 50 + latency_for_wire_12 = or_in_0
      or_in_1 + 21  = or_out_0
      or_in_0 + 14  = or_out_0
      or_out_0 + 10 + latency_for_wire_13 = dup_270_in_0
      dup_270_in_0 + 10  = dup_270_out_0
      dup_270_in_0 + 11  = dup_270_out_1
      dup_270_out_1 + 10 + latency_for_wire_14 = Ar270_in_0
      Ar270_in_0 + 13  = Ar270_out_0
      Ar270_out_0 + 10 + latency_for_wire_15 = Br270_in_0
      Br270_in_0 + 13  = Br270_out_0
      Br270_out_0 + 10 + latency_for_wire_16 = or_in_1
      		
```

And now this is where the magic happens. These constraints form a system of linear equations modulo 8, which we can rewrite as a matrix

```
                                              [ 0mod4_out_0         ]
      [1 7 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]     [ or_in_0             ]       [0]
      [0 1 7 0 0 0 0 0 0 0 0 0 6 0 0 0 0]     [ or_in_1             ]       [6]
      [0 0 0 1 7 0 0 0 0 0 0 0 0 0 0 0 0]     [ or_out_0            ]       [3]
      [0 0 1 0 7 0 0 0 0 0 0 0 0 0 0 0 0]     [ dup_270_in_0        ]       [2]
      [0 0 0 0 1 7 0 0 0 0 0 0 0 6 0 0 0]     [ dup_270_out_0       ]       [6]
      [0 0 0 0 0 1 7 0 0 0 0 0 0 0 0 0 0]     [ dup_270_out_1       ]       [6]
      [0 0 0 0 0 1 0 7 0 0 0 0 0 0 0 0 0]     [ r270_in_0           ]       [5]
      [0 0 0 0 0 0 0 1 7 0 0 0 0 0 6 0 0]  *  [ r270_out_0          ]   =   [6]
      [0 0 0 0 0 0 0 0 1 7 0 0 0 0 0 0 0]     [ r270_in_0           ]       [3]
      [0 0 0 0 0 0 0 0 0 1 7 0 0 0 0 6 0]     [ r270_out_0          ]       [6]
      [0 0 0 0 0 0 0 0 0 0 1 7 0 0 0 0 0]     [ latency_for_wire_12 ]       [3]
      [0 0 0 7 0 0 0 0 0 0 0 1 0 0 0 0 6]     [ latency_for_wire_13 ]       [6]
      [1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]     [ latency_for_wire_14 ]       [0]
                                              [ latency_for_wire_15 ]  
                                              [ latency_for_wire_16 ]    
      
```

And now we just need to write a fairly simple [RREF](https://en.wikipedia.org/wiki/Row_echelon_form) operation that works with integers in [GF(8)](https://en.wikipedia.org/wiki/Finite_field) and we're done.

```
      1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0   |  0
      0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0   |  0
      0 0 1 0 0 0 0 0 0 0 0 0 2 0 0 0 0   |  2
      0 0 0 1 0 0 0 0 0 0 0 0 2 0 0 0 0   |  3
      0 0 0 0 1 0 0 0 0 0 0 0 2 0 0 0 0   |  0
      0 0 0 0 0 1 0 0 0 0 0 0 2 0 6 6 6   |  0
      0 0 0 0 0 0 1 0 0 0 0 0 2 0 6 6 6   |  2
      0 0 0 0 0 0 0 1 0 0 0 0 2 0 6 6 6   |  3
      0 0 0 0 0 0 0 0 1 0 0 0 2 0 0 6 6   |  5
      0 0 0 0 0 0 0 0 0 1 0 0 2 0 0 6 6   |  2
      0 0 0 0 0 0 0 0 0 0 1 0 2 0 0 0 6   |  4
      0 0 0 0 0 0 0 0 0 0 0 1 2 0 0 0 6   |  1
      0 0 0 0 0 0 0 0 0 0 0 0 0 2 2 2 2   |  2
      
```

The first 12 rows of this tell us the phase of each of the nodes in the graph (so for example this tells us the phase of the “0mod”gate that emits gliders at time steps 0 mod 4 is ... 0 mod 4. Very good. It also tells us the phase of the or gate inputs are 0 and 2, etc reading down. The 13th row is the one that's interesting. If we decoded this off, what it's actually saying is that any solution is valid as long as

```
      2*(latency_13 + latency_14 + latency_15 + latency_16) = 2
      		
```

If you think about it for a minute this makes sense. Adding a delay gate anywhere along the loop gives the same effect as far as the total overall latecy is concerned. So we just need to pick one of these (my code arbitrarily chooses the first) and set it's additional delay to 1, and we're good!

At right you can see the final result of doing this, and what do you know, it's perfect. Now this all may seem very complicated, and I'm not going to lie, it actually is. I was stuck on this issue of phase for over a year. Now obviously I wasn't spending every waking minute of my time thinking about it, and once you see the solution it really just seems obvious, but it's one of the most annoying problems I've had to solve.

![](https://nicholas.carlini.com/writing/2022/gameoflife/explain_fib.png)

We're now ready to put it all together. We can use our program to fix the phase of any circuit we want to design.

Okay so how does that Fibonacci circuit at the top work? If you've read the prior post, there's not really any new ideas here, and it would have technically been possible to do previously---I'm just using the fact that we have 8-bit wires for this to make it more efficient.

The clock on the left drives the circuit. This clock feeds into six 4-bit registers that each hold a single decimal digit (encoded with [binary-coded decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal)). These registers come in pairs of two, holding the current counter state at the previous two timesteps.

The 4-bit adders above this add together the prior two states, and then above that is the BCD conversion logic that takes values greater than 10 and subtracts 10 from these (so that, for example, 11 maps back to 1). In practice this is implemented with another 4-bit adder that's adding 6 (because ~6 = 10 in 4-bit math). This new data gets sent both down to the shift registers, and also up to the display.

The first step of the seven-segment display is to convert the decimal value into the on/off values for each segment of the display. This is done with a small ROM lookup table (I'll describe this magic next time when I start getting into the full CPU) and the decoded values from here then go into the display itself.

To get a sense of scale for just how much better things have gotten over the last few posts, let's just look at the Fibonacci counter at the top and compare it to the 4-bit adder I built the first time around. Note that these are *to scale* snapshots!

![](https://nicholas.carlini.com/writing/2022/gameoflife/old_adder.png) ![](https://nicholas.carlini.com/writing/2022/gameoflife/new_fib.png)

Despite containing three 4-bit binary-coded decimal adders (so, three 4-bit adders, plus 3 conditional subtraction circuits), plus six 4-bit counters, plus three BCD to seven segment display decoder, plus three seven segment displays ... it's still smaller!

Now, with this final (I hope!) rewrite out of the way, it's time to go and actually build a real CPU using all these fancy gates we've just developed. So look forward to that next time.
