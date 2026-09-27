---
title: Improved Logic Gates on Conway's Game of Life - Part 3
link: https://nicholas.carlini.com/writing/2021/improved-logic-gates-game-of-life.html
source: nicholas-carlini-com
published: 2021-03-23T00:00:00Z
updated: 2021-03-23T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Abstract: This post describes improvemnets made to my prior digital logic gate constructions (e.g., AND/OR/NOT) built on top of Conway''s Game of Life, resulting in 100x faster simulations.'
content: extracted
html: 2021-03-23-improved-logic-gates-on-conway-s-game-of-life-part-3.html
extra:
  content:interactive: true
---

This is the third in a series of posts ([\[1\]](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html), [\[2\]](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html), [\[4\]](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html), [\[5\]](https://nicholas.carlini.com/writing/2022/multiplexing-circuits-game-of-life.html)) implementing digital logic gates on top of Conway's Game of Life, with the final goal of designing a fully functional CPU.

[The first post](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html) used the simplest possible techniques to build AND, OR, and NOT gates on top of the game of life. Then, the [second post](https://nicholas.carlini.com/writing/2020/intro-to-circuits-game-of-life.html) used these to design some interesting circuits (like a flip-flop counter)

This time I'm going to start over from scratch. Everything I did previously worked, but was rather inefficient. If we're eventually going to be able to get any real and interesting circuit running on the game of life, then speed is a priority. So this post will be dedicated to rebuilding everything I did the last two times. And as a result of being more careful, everything will be 100 times faster.

As a result, the same 7-segment display that last time was contained in a 50,000-square grid this time fits in a 7,000-by-11,000 cells, more than 20 times smaller area and runs even faster than that. If you'd like to download the [Macrocell](https://www.conwaylife.com/wiki/Macrocell) file to run in [Golly](https://www.conwaylife.com/wiki/Golly), you can get it [here: 7seg.mc](https://nicholas.carlini.com/writing/2021/7seg_new.mc).

(If this is interesting, [next time](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html), I promise we'll get something new in a first real CPU: the [Unlimited Register Machine](http://www.cs.cornell.edu/courses/cs6110/2015sp/docs/computability%20an%20intro%20to%20recursive%20function%20theory.pdf) from [Shepherdson and Sturgis](https://dl.acm.org/doi/10.1145/321160.321170).)

* * *

If you're not already familiar with [Conway's](https://en.wikipedia.org/wiki/John_Horton_Conway) [Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life), go back and read [the first post](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html) and then skip the rest of this section. (I'll wait.)

To briefly summarize, Conway's Game of Life is an (infinite) grid where all cells at any given timestep are either alive or dead. Each cell is either alive or dead. If a cell is alive at one timestep, it remains alive if it has two or three living neighbors. Conversely, dead cells becomes alive at the next timestep if three of its neighbors are alive.

It turns out that these rules allow the creation of a [glider gun](https://nicholas.carlini.com/writing/2020/digital-logic-game-of-life.html) (shown at right) that spawned a series of [gliders](http://www.conwaylife.com/wiki/Gosper_glider_gun), and showed how when these glider streams collide it can cause various effects.

By putting together different configurations of glider guns, it's possible to construct all of the digital logic gates that you might want to use.

* * *

All of my prior constructions used the relatively simple standard glider as representing the “flow” of current. This time we're going to change this basic building block, and use something else: the [light weight space ship](https://www.conwaylife.com/wiki/Lightweight_spaceship) (or, LWSS for short). A LWSS is exactly like the glider in that it's a type of [spaceship](https://www.conwaylife.com/wiki/Spaceship) that self-propagates after a small number of iterations. At left is a comparison of the movement of a LWSS versus a glider. Observe there are two key differences.

First: The LWSS moves twice as fast as the glider. Whereas the glider takes four turns to translate its position by one cell down and to the right, the LWSS can do this in *two*. This means that by using the LWSS to propagate information any circuits are already going to be twice as fast.

Second: The LWSS moves horizontally (or vertically), and not diagonally. This doesn't actually make anything better as such, but makes everything much prettier to look at because you don't have to tilt your head 45 degrees to match a circuit to a game of life grid.

**The LWSS Gun —** Like the glider gun, there is a LWSS gun that spawns off a series of light weight space ships every fixed number of iterations. It turns out though, that the LWSS gun is actually just three glider guns that cause a particular collision in order to create a glider. Shown at right is how this process happens. Three guns fire gliders that collide in exactly the right configuration in order to create a LWSS that then continues moving.

The LWSS gun shown at right has one additional feature. The standard gun emits an LWSS every 30 turns. But, as before, it's better to have have a slightly longer period. So there's one little oscillator that's added here that removes every other LWSS, to double the frequency to 60. This allows two LWSS streams to pass through each other without causing any damage.

**LWSS Collisions —** Just as we could use glider collisions to cause specific interactions between streams, we can do the same thing with LWSS collisions. I'm not going to show all of them that are possible here, because there are are many (many) options, but I'll walk through a few common patterns for how LWSS streams can collide.

The first of these interactions is the simplest collision where two LWSS streams collide and both die off. I use this frequently to construct circuits.

**LWSS Collisions 2 —** Again, we can also cause two glider streams to collide but as a result of the collision allow one of the two streams to continue even though the other dies off.

This is helpful in all kinds of constructions where we want one stream to serve two purposes: (1) to make one action against the first glider stream, but then (2) continue on and make a further action with a later stream. This is vital for constructing “gates” that split one glider stream into two different streams, and without a construction like this I don't think it would be possible to split glider streams.

**LWSS Collisions 3 —** The final collision I'll show (again, there are a lot more we can do) is one where a LWSS and a glider stream interact. Just as we can make two gliders collide to cause various effects, we can make a glider and LWSS cause almost any effect we want.

In this collision, we do something really quite fun. We take a standard glider stream coming from the upper right, collide it with a LWSS stream from the left, and somehow (magically!) we end up with a glider stream that has “bounced” off the LWSS stream and goes to the upper left.

* * *

With the primitive operations out of the way, it's time to go ahead and build our new logic gates. This is the next point where we improve on the the prior design. Instead of circuits being 270 cells square as we had previously, they're now going to be just 150 cells square. The smaller circuits are possible for two reasons. First: tooling. I built the prior circuits by hand-placing each glider gun---and nearly a decade ago as a way to procrastinate from taking exams. This time around I build some tools to help me automagically create circuits with some hand-design, but a lot of automation. Second: the LWSS goes horizontal to the grid lines, making fitting everything in a square (much) easier. When the gliders travel at 45 degree angles it can be difficult to make full use of the available grid space.

I made one important design-level change that was quite trivial, but significantly cleaned up one of the main problems from the last setup: LWSS streams going north/south have a different phase than those going east/west. Recall from above that I've set things up so that all LWSS streams have a frequency of 60 ticks. However, gliders traveling horizontally have a “phase” of 0, and the LWSS streams traveling vertically have a phase of 30. As a result, two glider streams can pass right through each other without causing any damage. (Last time, because they both traveled at a phase of 0, they would just explode if we didn't include a special pass-through gate. That made things ugly.)

But with that one overall design change, at the core we're going to do the same things as last time and build gates one by one to do all of the standard logical primitives.

* * *

So let's go ahead and show what's possible with these new primitives, and build an AND gate. For comparison, shown at the left is the AND gate I built previously which has as seven different glider guns that result in ten possible collision points in order to compute the function. The new AND gate has just one.

Because it's so simple, explaining how it works also is much easier. If there's a LWSS stream just coming from the left, then it will collide with the diagonal glider stream, and nothing exits on the right. If there's a LWSS stream from the bottom, then similarly, it collides, and nothing happens.

However, if we have LWSS streams coming from both the bottom and left, then the glider stream will only collide with the first one and allow the second to continue unobstructed.

Note here that it's the ability to have a single diagonal glider stream that can collide with both a vertical and horizontal LWSS stream that makes this AND gate so simple. If it was only possible to interact with one direction at a time, we'd have to have a lot more complicated machinery.

* * *

The NOT gate is slightly more complicated, but again, is much simpler than the prior construction using just standard gliders. Notice that here again, it's critical that we can collide a normal glider with the LWSS.

Here, if there's a LWSS stream coming in from the left, then it will collide with the proper glider stream. As a result, the glider stream going up to the north east will be able to continue forward and prevent anything from leaving the gate.

In contrast, if there's not a LWSS stream coming in from the left, then the first glider stream completely stops the second glider stream, and this leaves the LWSS gun in the middle to send an output.

* * *

Standard circuits can rotate traces without any kind of gates, but here in order to cause the LWSS stream to rotate we need a gate. This again just requires a simple interaction. The first LWSS stream coming from the left will pass *through* the LWSS stream heading north without interaction. (This again is something that's new: glider streams that go up and down run a different “phase” and so they can pass through each other without collisions. This makes lots of things nicer.) Then, it will run into the glider stream going north east which as a result allows the LWSS gun to send gliders north.

If the input stream was absent, then we'll just have the glider gun hit the LWSS gun and nothing interesting happens.

* * *

Now let's go on to a “gate” that doesn't exist for real circuits, but has to exist in the game of life. Sometimes you want to take a single output and feed it into two other inputs. This is trivial on real circuits: just split the trace or have another wire. On the game of life it requires a proper gate to duplicate the glider stream.

The way this works is through almost exactly Collision 2 shown at the top. There's a glider stream that normally will prevent the north-facing LWSS gun from doing anything (after being bounced off of a reflector). But if a LWSS stream comes in from the west, then the glider stream will hit it and the glider stream will die while allowing both LWSS streams to continue on out.

Again, notice how critical this diagonal-traveling stream is because it can interact with both a vertical and horizontal LWSS stream.

* * *

Definitely the most complicated of the three “primitive” gates is OR. It's just nasty, and there's not much we can do about it.

When there's no inputs, a LWSS stream goes through a long convoluted path where it first gets converted to a glider, then gets reflected (twice), and finally collides with the output preventing anything from happening.

However, if there is a glider stream present from either the west or the south, then we'll hit the LWSS stream that's been generated and remove it---and as a result, it won't get bounced around and won't eventually prevent anything from leaving.

The magic piece of this construction is this conversion object that accepts a LWSS and converts it into a glider. I've used it twice here: once to turn the LWSS stream from the west into a glider heading south, and once to turn the LWSS stream that's being generated on the bottom and converting it into a glider stream.

* * *

I ended up building a lot of other gates that I'll use later. But just to show what kinds of things can help, here I'll make an XOR gate. Normally this would require a lot of space to implement (five proper gates, several rotation gates, contained in a 5 by 5 grid). By making a few more special purpose gates (like the XOR gate) it'll be much easier to build good circuits.

This one is actually really similar to the OR gate above. Normally the glider gun gets reflected twice and prevents the LWSS in the center from doing anything.

If a LWSS stream comes from the south, then it gets first converted into a glider stream, and then gets converted back into a LWSS stream (but rotated by 90 degrees). It then collides with the main glider gun that will prevent it from causing a collision with the final LWSS stream and so we get an output.

* * *

When both the south and west LWSS streams are active, then we see why the construction had to be complicated.

The LWSS stream from the south gets converted into a glider stream, and then it actually collides with the LWSS stream from the west. This both (1) prevents it from getting rotated and hitting the glider stream from the top, but also (2) prevents the other stream from the west from doing the same. As a result, we end up in the same place as if there was no input and the glider stream gets rotated twice an then hits the output preventing anything from happening.

The ability to build one-off gates quickly is something that will be useful next time, when we realize that we'll need a few more special purpose gates to save space. But let's not get ahead of ourself.

* * *

Now that we have a more efficient set of primitives, and a way to create them more easily, let's build something with them. As we did last time, I'll start with a D flip-flop for all the same reasons: it's simple, but also quite useful.

My last last flip-flop that fit in a 25-block by 25-block grid---and 270 cells wide this takes 4860 cells on each axis which means we need **23 million** cells for just this one basic operation.

This flip flop fits in a 4-block by 4-block grid, with the grid size now at just 150 cells per gate, which makes the total flip flop fit in just 360,000 cell---**63** times smaller. And remember---the gliders even travel twice as fast, so we're at roughly 120 times more efficient. (Now, it's important to note that because we simulate the game of life with [hashlife](https://en.wikipedia.org/wiki/Hashlife), our runtime isn't directly proportional to the grid size, because we can rely on caching to speed things up.)

Rising Edge Detector

S-R Latch

-- Output -->

Clock-->

Data-->

The flip flop works as before. The clock signal comes from the top; the first thing that happens is we run it through a rising edge detector. From here, we run this through some logic to trigger the latch SET if the data signal is currently 1 during the rising edge, and the latch RESET if the data signal is currently 0 during the rising edge. The signal is then maintained forever until the next time a set or reset comes through.

The rising edge detector computes clock and (not clock)---you might think this always returns false, but because there is a slight delay in computing the not what actually happens is we compute clock\_now and (not clock\_before which is true exactly when the clock transitions from 1 to 0.

What makes building circuits on the game of life really easy (compared to building actual physically realizable circuits) is that the game of life is completely predictable. It would be insane to detect the rising edge this way normally: how would we know how long the pulse generated would be? What if it's too long? Too short? But in the game of life, it's going to be exactly the same: 23 light weight space ships will go through the gate each time we get a pulse. Exactly 23. Every time.

The SR Latch works by having a loop of data with an AND gate and an OR gate; sending a 1 into the OR gate causes the data loop to forever have the value 1, and sending a 0 into the AND gate clears anything going on in the data loop.

Again, this is not physically realizable. We couldn't just expect that the pulse sent along the SET input would be able to spin forever in a loop and also maintain some kind of output. The charge would (quickly) dissipate. But this is a simulation, and we don't have to respect the laws of physics.

The reason this is so much more efficient than last time is threefold. First, because gliders now travel horizontally and vertically we now no longer have “dead space” in a checkerboard pattern like we had when using standard gliders. Second, because of how my backend circuit constructor works (which I promise we'll get to next time) we're able to more densely pack gates next to each other. Previously there was a limitation in my circuit -> game of life converter that required at least one wire in between any pair of gates. A better implementation has removed this limitation. Finally, I have a lot more gates available than just the basic AND/OR/NOT. For example, this circuit actually uses a one-off gate I needed that just computes A AND (NOT B), saving two gates in the space of one.

* * *

Alright, so we're now done with rebuilding what we did the first two times around. It's been quite a long. [Next time](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html) I'll go ahead and actually do something with it---and build a Unlimited Register Machine (URM) from [Shepherdson and Sturgis](https://dl.acm.org/doi/10.1145/321160.321170). The URM is one of the early models of computation, but unlike the Turing machine is actually something that can be reasonably programmed because it basically looks like a modern assembly language.
