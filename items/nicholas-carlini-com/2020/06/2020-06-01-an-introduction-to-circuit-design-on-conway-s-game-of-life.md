---
title: An Introduction to Circuit Design on Conway's Game of Life - Part 2
link: https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html
source: nicholas-carlini-com
published: 2020-06-01T00:00:00Z
updated: 2020-06-01T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Abstract: Using AND/OR/NOT gates built on top of Conway''s Game of Life, this post walks through how to construct a actual circuits, for example a 7-segment display.'
content: extracted
html: 2020-06-01-an-introduction-to-circuit-design-on-conway-s-game-of-life.html
preview:
  file: 2020-06-01-an-introduction-to-circuit-design-on-conway-s-game-of-life.preview-dbcdb63dffa7.webp
  width: 256
  height: 197
  color: '#a3a3a3'
images:
- source: https://nicholas.carlini.com/writing/2020/gameoflife/horizontal_grid_bad.gif
  original:
    file: 2020-06-01-an-introduction-to-circuit-design-on-conway-s-game-of-life.image-a2b8635e2fd1.gif
    width: 877
    height: 675
  color: '#fbfbfb'
extra:
  content:interactive: true
---

This is the second in a series of posts ([\[1\]](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html), [\[3\]](https://nicholas.carlini.com/writing/2021/improved-logic-gates-game-of-life.html) [\[4\]](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html), [\[5\]](https://nicholas.carlini.com/writing/2022/multiplexing-circuits-game-of-life.html)) implementing digital logic gates on top of Conway's Game of Life, with the final goal of designing an Intel 4004 and using it to simulate the game of life.

[Last time](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html) I walked through how to create AND, OR, and NOT gates on top of the game of life. (If you haven't read it yet, you might want to (at least skim) that one first.)

This time, I'm going to use those components to create a 7-segment display that counts up from 0 to 9. In principle, these techniques are sufficient to extend and make a full CPU. But there are a number of small inefficiencies I'll talk about at the end that should be fixed first.

* * *

Okay I know I said that you should go read the prior post for this one to make sense, but because I know people are busy and many won't have time for that, here's what you need to know. (If you have done your homework and read the last post, then just skip down to the [next section](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html#nextsection).)

Conway's Game of Life is an (infinite) grid where all cells at any given timestep are either alive or dead. Each cell is either alive or dead. If a cell is alive at one timestep, it remains alive if it has two or three living neighbors. Conversely, dead cells becomes alive at the next timestep if three of its neighbors are alive.

These rules allow the construction of the *glider gun*: a repeating pattern that continuously emits *gliders* that move to the south east of the grid.

Glider streams are the fundamental primitive that represent the flow of “electrons” in this circuit.

* * *

When streams of gliders collide, they can have various interactions. This allows us to build logic gates, for example, shown to the left is an AND gate. Because there is a glider stream coming from the lower left and upper right, the right interactions take place that allow for the stream to exit down the lower right.

(To use the simulator, click on it to play/pause the circuit, and use the buttons above to increase and decrease the speed, or restart the simulation.)

Last time I used these glider collisions to build up AND, OR, and NOT gates. Also, unlike traditional circuits, I needed to make “gates” in order to rotate the glider stream, duplicate a glider stream, and to allow two streams to cross without an unintended explosion.

* * *

With that short review out of the way, let's build our first useful circuit: a clock. The purpose of a clock is to produce a regular oscillating signal that can be used as a basic primitive in almost all digital logic.

The other reason to start with a clock is that it's also very easy to build, and gives some good introduction to the types of things we can do that would be completely meaningless with actual electronics.

Our clock works by connecting a NOT gate to itself (which we do by rotating the circuit 90 degrees three times, plus the rotation from the NOT gate itself). There's no need for external power supply here, and on a traditional circuit this may seem insane (what does it even mean to say the output is the negation of the input?) but this actually works on the game of life.

And the reason why is that everything is completely deterministic and predictable. Initially there will be nothing feeding in to the NOT gate. So it will produce output, which will then get routed around back to the input. Once this happens, the output stops. The lack of gliders propagates through the loop, and then the cycle repeats.

This produces a perfect periodic cycle of gliders, alternating between present and absent, until the end of time---or you click pause.

* * *

I briefly described last time the process I go through to actually lay out the circuits. Let me go into that in a bit more detail here.

[Logisim](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html) is a logic simulator that's mainly designed for teaching digital logic. [\[a\]](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html#) I first used it in my undergraduate course on compute architecture. Using the GUI you can place components, wire them together, and then simulate the result of the circuit. Compared to the real tools (e.g., Verilog and VDHL) that are actually used for real circuits, Logisim is a toy. It's very slow to do anything useful, requiring lots of manual repetitive work to perform many basic tasks.

That's not always a bad thing. Once a circuit is designed, typically circuit layout is its own complicated process. In Logisim the design and layout of the circuit are one and the same. For small tasks, (I have found) this actually makes it easier to use this simple tool.

Converting a Logisim circuit to something I can run on top of the game of life is actually slightly nontrivial, and requires a bit of care. The basic process is simple: read and parse the file, and then for each gate, place the corresponding game of life components, and save it. There's some difficulty in the details here though.

The most important detail is in constantly tracking the direction of flow of the “electricity” (glider stream, here). Normal wires don't have a direction: electrons can flow in one direction just as well as in the other. However, in our circuits, gliders can only go one direction, and not the other.

So when we see a wire rotate 90 degrees (say, heading north and west) we need to know if the gliders are coming along the west and then rotate to the north, or if the gliders are coming along the north wire and then rotate to the west. The details here aren't very entertaining and I didn't really want to spend much time on this, so I hacked together a short ugly python program in order to get it to work. I'll (probably) eventually get around to releasing that program. But that's enough of this for now.

Alright, so now that we've got our simple clock, let's build a first stateful component. Here's where things actually start getting interesting, and where our designs start to (significantly) deviate from how things are done in the real world.

AND\
(clear)

OR\
(set)

A [latch](https://en.wikipedia.org/wiki/Flip-flop_\(electronics\)#Simple_set-reset_latches) is a basic primitive in digital logic that holds a particular value. It has two inputs and one output. Sending a (short) pulse along the first input causes the output to emit a 1, until whenever there happens to be a (short) pulse on the second input to clear the state and stops the latch from emitting anything.

The basic idea behind our latch is that we can again form a cycle, but this time instead of always negating the flow as in the clock, we're going to control whether or not there's anything going on in the cycle. We'll do this by having in-line an AND gate (letting us clear the state) and an OR gate (letting us set the state). [\[b\]](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html#) Most latches are implemented with NAND or NOR gates because that's what efficient to create in actual hardware. For us, AND and OR are what's efficient.

Initially, the state is initialized with no flow along the cycle, and so the latch does not emit any signal. After a brief period of time, a pulse comes in along the first input. This pulse of gliders gets directed into the OR gate input, which then causes them to be permenently looped. Now, and this is what makes this a latch, even after the pulse of gliders stops and the OR gate input is deactivated, we see that the gliders continue flowing through this circuit in a loop.

From here we can clear the stream by sending gliders along the second input, the NOT-to-AND input. Just as how the prior pulse put the gliders into the loop, this one takes them out by making the second input to the AND gate a zero, and so the cycle ends. As before, we only need the input pulse to be applied for a short period of time for the circuit to “remember” this fact.

We can repeat this, sending pulses on either input for as long as we would like.

Latches are convenient and easy to implement, but they're somewhat difficult to use in actual circuits, because they're not directly tied to a clock. The [D Flip-Flop](https://en.wikipedia.org/wiki/Flip-flop_\(electronics\)#D_flip-flop) fixes that. The D flip-flop has two inputs, clock and data. Whenever the clock goes from low to high (or high to low, depending on how the circuit was designed) we hold onto the the value of whatever was on the data line at that instant, until whenever the clock again goes from low to high.

Getting the D flip-flop from this latch is fairly straightforward. We just add a little bit of logic surrounding it.

The first component we need to introduce is an edge detector. We're going to start with a rising edge detector, because that turns out to be easier. In a rising edge detector, when the glider stream goes from absent to present, we should generate a short pulse to send out. Otherwise, it remains inactive. Again, because we're on the game of life, we can implement this in the simplest method possible: take the input, split it, connect one of these to an AND, and the other to a NOT and then the same AND. Critically, the path from the split to the AND is shorter than the path from the split to the NOT to the AND.

This works because when the input is steady-state 0, we have 0 AND 1 = 0, and when the input is steady-state 1 we have 1 and 0 = 0. But when we're in the middle of flipping from 0 to 1, the delay in the circuit (because the path is physically longer to go through the NOT and AND gates) means we end up computing 1 AND 1 = 1 for a very short period of time. When we transition from 0 to 1 though, the delay works against us and we end up computing 0 AND 0 = 0 and again get no signal on the output.

--clock-->

--data-->

--out-->

edge detector

latch

So now we have our mechanism for generating a short pulse. From here, all we need is to selectively send a pulse to either the set or clear inputs of the latch we made earlier, depending on whether or not the data line is active.

And that's really all there is to it. You can see an example of a d-flip-flop running to the left. When the clock line goes from high to low, whatever signal was flowing along the data line at that instant gets captured and held.

* * *

Alright, and that's all we need to create our 7-segment display. Now what might be the simplest way to build it would be to do the traditional thing: Have four D flip-flops that hold a counter base 16, make some increment logic, feed the output of the increment back into the flip-flops, hook this all up to the clock, and then decode that to a 7-segment display.

While that would do the job, it's a lot of work. More work than we need to do. Instead, we're again going to abuse things and rely on the fact that everything here is deterministic. We'll still have four flip-flops, but take the output of each flip flop, negate it, and then feed that back as its input. The first flip-flop's clock is driven by the actual clock. The second flip-lop clock is driven by the output of the prior flip flop. This makes it have double the period of the actual clock, because our flip-flop only updates on the rising edge. Similarly, the third flip-flop is clocked by the second, and the fourth by the third.

At the end of this, we get four flip-flops each of which is has a period twice the prior. This gives us a perfect 4-bit counter from 0 to 16, exactly as we want, without having to waste time doing things “right”.

From here we just have to build the 7-segment display logic, to make it all look nice, and we're done.

* * *

Alright, confession time. Everything you've seen up until now is something I'd worked on from 2011-2013. (Don't believe me? Look take a look at the source code. Notice the conspicuous mixture of plain old JavaScript and then suddenly *bam!*, ES6 arrow functions I added when fixing the writeup.) And I made a lot of mistakes in the design. So to end this, I'll describe most of what I did wrong. And [next time](https://nicholas.carlini.com/writing/2021/improved-digital-logic-game-of-life.html) will be dedicated to cleaning it all up, and completely re-building the same seven segment display, but better.

### A horizontal-and-vertical grid wastes half of the space.

I designed the gates so that each fit nicely within a 270-by-270 grid, placed horizontally. However, these gliders move diagonally.

This means that half of the space of the grid is completely wasted! Look. Let me take an image of the seven segment display and black black out every other grid cell. Either I'm extraordinarily unlucky, or components can only be placed every other grid cell.

![](https://nicholas.carlini.com/writing/2020/gameoflife/horizontal_grid_bad.gif)

(Proof: Assume some component is at index (i,j). Because gliders move diagonally, I can either go to (i+1,j+1) or (i-1,j+1) or similarly. In all cases, the invariant (i+j) mod 2, both before and after. Therefore if we start at an entry 1 mod 2, it will stay at 1 mod 2 (as is the case here). All the squares 0 mod 2 are dead space.)

### Standard gliders travel at c/4.

The standard glider that I introduced earlier travels at speed c/4, meaning that for ever four ticks of the game of life, the glider translates over by one square. It turns out there's a better glider: the [light weight space ship](https://www.conwaylife.com/wiki/Lightweight_spaceship) that travels at speed c/2, twice as fast. This is better.

### Overly complicated primitives.

I designed my AND/OR/NOT primitives just to work. Not to be efficient or anything. It was a proof of concept. It turns out that you (meaning, future me) can do much better if given a bunch of free time to waste on designing efficient circuits. Or, as the case may be, not much free time, but a complete disregard for “real work” when there are important problems to be solved.

To give a brief teaser of what'll happen next time when I resolve all of these issues, compare the following two AND gates. On the left is the AND gate I show above, and have been using for all my circuits. On the right is the new AND gate, in the new framework. It's an order of magnitude more efficient: it has four times smaller area, it tiles perfectly so there's no wasted space, and the gliders themselves are twice as fast.

### Diagonal circuits aren't pretty.

Clearly the most important issue of the bunch, because gliders travel at 45 degree angles, you have to tilt your head to view everything. This is not as pretty as being able to just look at the vertical/horizontal circuit, as it is actually laid out in the diagram tool.

## Next time: Fixing this!

In the [next post](https://nicholas.carlini.com/writing/2021/improved-logic-gates-game-of-life.html) I'll fix all these issues described here, and make things much nicer.
