---
title: Yet Another MOBA (In 13kb of JavaScript)
link: https://nicholas.carlini.com/writing/2021/javascript-moba-13k.html
source: nicholas-carlini-com
published: 2020-11-21T00:00:00Z
updated: 2020-11-21T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'For the third year in a row, I participated in JS13k 2021, where you''re tasked with making a game in 13kB of JavaScript. Each year I enter participate I try to learn something new I didn''t know how to do before. This year''s motivation: I wanted to make a multiplayer game with some nontrivial networking aspects. So below you''ll find a description of my experience building a MOBA in (barely) under 13kB of JavaScript.'
content: extracted
html: 2020-11-21-yet-another-moba-in-13kb-of-javascript.html
preview:
  file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.preview-e80dc844a34c.webp
  width: 256
  height: 160
  color: '#1b1a18'
images:
- source: https://nicholas.carlini.com/writing/2021/fig/game.png
  original:
    file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-3ffa5c8d3695.png
    width: 400
    height: 250
  color: '#080706'
- source: https://nicholas.carlini.com/writing/2021/fig/c1.png
  original:
    file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-2968c7845fa2.png
    width: 683
    height: 384
  variants:
  - file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-3ca33976e16d.webp
    width: 320
    height: 180
  - file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-cc18c476aaed.webp
    width: 683
    height: 384
  color: '#fdfdfd'
- source: https://nicholas.carlini.com/writing/2021/fig/c2.png
  original:
    file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-5f787f228b68.png
    width: 694
    height: 375
  variants:
  - file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-e53b94be86f2.webp
    width: 320
    height: 173
  - file: 2020-11-21-yet-another-moba-in-13kb-of-javascript.image-c38b063240a0.webp
    width: 694
    height: 375
  color: '#fdfdfd'
---

Clicking the video above (or [HERE](https://js13k2021-yet-another-moba.herokuapp.com/)) will take you to the game. Source code is [available here](https://github.com/carlini/js13k2021-yet-another-moba).

When writing up my experience the last two times building a game, I focused mainly on the gameplay and graphics aspects of the development (because that's what I was trying to learn how to do). This time I'll spend basically zero time on that. The core 3d-game engine is going to remain unchanged from the last two times. Instead I'll talk about the process of creating a networking game in just a few thousand bytes of code.

* * *

As I just said, my goal this year was to make a nontrivial multiplayer game, where human players interact with nonplayer characters in interesting ways. I desided to create a [MOBA](https://en.wikipedia.org/wiki/Multiplayer_online_battle_arena) (Mutiplayer Online Batle Arena): a type of game where two teams of players each move their own character around a map, and uses abilities to attack other players with the goal of destroying the enemy team's base.

The motivation to pick a MOBA is that it's a relatively simple type of multiplayer game, while still being sufficiently complicated to be interesting.

1. MOBAs have real multiplayer challenges. Yes, technically speaking chess is a multiplayer game. But that would be boring. A MOBA has realtime requirements, allow more than just two players at once, and nonplayer characters that have their own movement system. This makes things exciting, but not *too* exciting.
2. A bit of lag doesn't ruin everything, unlike [first person shooters](https://en.wikipedia.org/wiki/First-person_shooter) or [fighting games](https://en.wikipedia.org/wiki/Fighting_game) where a tiny amount of lag can be completely game breaking. MOBAs are more forgiving, and having a few hundred ms of lag, while not optimal, is still playable.
3. There aren't (too) many simultaneous players or objects. In something like an [MMO](https://en.wikipedia.org/wiki/Massively_multiplayer_online_role-playing_game) , there are often hundreds or thousands of units that are taking actions and that need to controled. That would be too hard to fit in 13kB.

So now let's talk about all the things that most multiplayer games do that I'm going to ignore completely. If you ever have to build a real multiplayer engine, this is where all of the truly difficult work comes in.

1. Unreliable transmission. The rules for JS13k entries specifies if you want to use networking, you have to the [socket.io](https://socket.io/) communication libary, which builds on [websockets](https://en.wikipedia.org/wiki/WebSocket), which themself operate over TCP. So we're going to get reliable, in-order data transmission for free. No worrying about retransmissions or out of order packet delivery.
2. Latency reduction. This game is going to be written in JavaScript. Communicating over websockets. Over TCP. There's going to be crazy lag, and there's nothing we're going to be able to do about it by optimizing the networking code.
3. Anticheat. One of the hardest part of most networking games is making sure that clients have to follow the game rules (and can't teleport to arbitrary locations, can't see the entire map all at once, can't deal damage to objects they can't actually reach, the list goes on). Fortunately, I have deployed a fullproof patent-pending [\[a\]](https://nicholas.carlini.com/writing/2021/javascript-moba-13k.html#) Just kidding. But honestly, it's more novel than half of all patents out there... state-of-the-art anti-cheat technique:

       Console.log("Please don't cheat.")

In principle making a networking game is straightforward: the server maintains an authoritative state of the world, the clients just hold shallow copies of what the server says is the truth, and then every so often (10 times a second, for example) the server sends the client the current game state to be rendered and shown on screen. Easy!

In code, this is really simple to implement. The server maintains a list of cients, each of which has a socket. The server game loop then looks something like this

class MovingBlock {

    constructor() {

 this.position = ZERO;

    }

    update(dt) {

 this.position = this.position.add(NewVector(dt,0,0));

    }

}

function server\_loop () {

    objects = objects.map(x=>x.update(FREQ));

    sockets.map(x=> {

        x.emit(CommandUpdate, objects);

    })

}

function server\_init () {

    FREQ = 100;

    setInterval(server\_loop, FREQ);

}

And then the client just receives the server's object list and renders it

function client\_init () {

    socket.on(CommandUpdate, server\_objects => {

        client\_objects = server\_objects;

    });

}

function client\_loop () {

    requestAnimationFrame(client\_loop);

    render(client\_objects);

}

Okay so let's try that. Below is a video of what it looks like when trying to move a cube around a grid using this very naive method.

Yeah, so that's pretty bad.

Why? The problem here comes down to the fact that even though the JavaScript renderer can run at 60 frames per second, we're only getting data from the server at something much slower (in this case, 5 ticks per second) and also because of client-server lag (artificially increased to 200ms, a not-so-bad-but-reasonable estimate of what can happen on the internet). This makes the game completely unplayable for something like a MOBA. (Would this work for a chess game? Certainly. But that's why we're not doing that.)

![](https://nicholas.carlini.com/writing/2021/fig/c1.png)

If we look at my wonderful networking diagram we can see why. [\[b\]](https://nicholas.carlini.com/writing/2021/javascript-moba-13k.html#) Please forgive my drawing skills... but I promised myself I wouldn't spend more than 5 hours on this post and I'm already at hour 6 and the figures aren't even done. So you, dear reader, get to admire my skills at MS Paint (or, someone's re-implementation in WebGL, but probably not one written in 13k of JavaScript). As soon as the player clicks to move, we have to wait for the server to receive the movement request, and then we have to wait for the server to tell the client that it's moved. And then it has to continue telling the client where it's at every single game step after this.

One potential way to “fix” this is to just send the updates from the server more often. And if you have a good connection, then this is actually a reasonable solution. But in our setting here when we're working with whatever network you end up with with our websockets connection, things will end up pretty bad.

Because TCP hepfully gauaratees strict packet ordering, if any of our messages from the server get delayed on the way to the client, we'll end up with the object just stuttering its movement followed by a burst of movement as all of the remaining packets arrive. So it's sometimes smooth, and sometimes just as bad as before.

Now every introduction to server programming starts this way. And in a typical article about networking, I might say that the way to fix the above issues is by doing some fancy interpolation: the server might tell the client where everything is *right now*, and the client also already knows where things *were*, a tenth of a second ago. And so instead of rendering exactly the current state of the world, the client would smoothly interpolate between where everything used to be and where it should eventually ends up. This is what most games do, and it generally works if you handle all the problems correctly.

Unfortunately, there are two problems with interpolation that make it unsuitable for our purposes here.

First, interpolation has really high networking bandwidth requirements. For each object that you need to move, at each game step, the server has to send the new location of *every* object that's moved. Since MOBAs tend to have roughly a hundred objects moving around at any point in time, simple interpolation would consume well over a hundred of kilobytes of data per second, per client. In any real dedicated game this is just fine! Just require that everyone has a good internet connection and run the game in an optimized language designed to handle efficient networking. But remember, (a) the server is running JavaScript, (b) the client is running JavaScript, (c) we're communicating over websockets over TCP, and (d) who knows what kind of connection people are playing with, it should still work.

There's another somewhat less important problem, but it still is good to understand: interpolation causes *even more* latency on top of our already-slow TCP/websocket networking engine. In order to actually interpolate, the client needs to know not only the current world state, but also the *next* world state. So the client, by definition, must be running at least one game frame behind the server. And in practice it should probably run at least two or three frames behind, because otherwise if you drop one packet you don't have anything to interpolate with. (And it's even worse with TCP, because if you drop a packet, now you're waiting for a retransmission...)

Alright, so what can we do if not interpolate? Well one convenient fact of MOBAs is everything, for the most part, just travels in straight lines. When the player clicks on the map, the object just linearly moves directly to that position in a line. So instead of every object that's at the server telling the client "I'm at (0,0). I'm at (1,2). I'm at (2, 4). I'm at (3,6)." the server tells the client "I'm currently at (0,0), and will be moving to (3,6) over the next four timesteps" then the *client* can now fill everything in perfectly, and knows exactly where every object is going to be at all times. In code, this means that what we're going to send from the server to the client is actually a function that computes the position as

function linear\_move\_withtime ( start\_pos, end\_pos, rate, start\_time) {

    return cur\_time =>

 start\_pos.lerp(end\_pos,

       clamp((cur\_time - start\_time)/duration,

                             0, 1))

}

Now when the client can render objects at 60fps, it can re-compute where the object is supposed to be going, and everything looks smooth.

![](https://nicholas.carlini.com/writing/2021/fig/c2.png)

This solves both of the previous problems at the same time. First, because we just send the function to calculate where we're going to be, we only need to send a new code block if we have to change direction. In MOBAs this happens infrequently (less than once a second, typically) and so we get a 10x reduction in communication costs right away. But also, because now we're computing where the object is *right now*, we don't have the latency problems from before.

Wonderful! Everything is smooth and it works like magic. The only real glitch here is the fact that for the remote player, there's a brief glitch where the object appears to just spin around in place instantly, but this is something I'm willing to tolerate for all of the practical gains.

This approach has a number of significant problems that make it unusable in practice for most real games. Among these are the problems that (1) People don't move in perfectly pre-defined paths in most real games. (2) What do you do if two objects collide? Things get really ugly here. Fortunately, I can just turn off collisions entirely! Problem solved. (3) Cheating: if I know exactly where someone is trying to move to, then I can use this information to play better. So we're relying on our amazing honor-code anti-cheat again.

Okay so that's the idea of how we're going to synchronize movement between the client and server. Now let me explain the details abuot how this is implemented in order to actually get something that has the object moving and implements our beautiful diagrams. The server is going to keep track of the list of every object in the game, and for each object, all of their attributes. It's also going to keep track of the *prior state* on the previous timestep. Then, at the end of each turn, the server will enumerate over all of the objects, over all of their attributes, and send to every client anything that's changed from the prior timestep.

function server\_loop () {

    // 1. Update the objects

    objects.map(x=>x.update(FREQ));

    var updates = objects.map(x=> {

        // 2a. For every object, compute the "current state" as a shallow copy

        var state = shallow\_copy(x);

        // 2b. And then compare to see what's different from last time

        var delta = Object.keys(state).reduce((diff, key) => {

            if (JSON.stringify(state\[key\]) === JSON.stringify(x.last\_state\[key\])) return diff

            return {

                ...diff,

                \[key\]: state\[key\]

            }

        }, {})

        delta.uid = state.uid; // keep uid for identification

 // 2c. And finally save this as the last state

        x.last\_state = state;

 // 2d. (But return the delta.)

        return delta;

    })

    // 2. Now send it to the client...

    sockets.map(x=> {

        x.emit(CommandUpdate, updates);

    })

}

"But wait!" I hear you say. Why is it okay to just store just one previous state of each object on the server? What if client A needs just one update, but client B has fallen behind and needs two updates? Well this would be a problem, if not for the fact that we're already running over TCP. So we are guaranteed (assuming we don't get exceptionally unlucky and have multiple cosmic rays strike our machine and break the TCP checksums) that if a client does fall behind, it will eventually receive the update for frame X before it gets frame X+1. So the server can just keep on sending updates and rely on the protocol to get everything there in order. Is this efficient? No way. But we're stuck with TCP anyway, so might as well put it to use.

The client now has to make use of the data. The basic idea is easy: either we make a new object (if we're receiving one that doesn't exist), or we update the object attributes (if this is an object we already have a copy of). The reason this update step is necessary is that we need to make sure that if object A current is following around object B, that if B moves, we actually update B's position---and not just make a new B object that now A won't be pointing to. Here's how we do it:

socket.on(CommandUpdate, object\_updates => {

    // For each object in the udpate ...

    object\_updates.map(update=> {

        var clientobj = object\_ht\[update.uid\];

        // If I already have a copy of this object

        if (clientobj) {

            // Loop through the {k:v} pairs ...

            Object.keys(update).map(k => {

                // And apply the update to my copy

                clientobj\[k\] = fix\_json(update\[k\]);

            })

        } else {

            // And if I dno't have a copy, then make one

            objects.push(fix\_json(update));

        }

    });

});

Now there's a bit of magic happening here that I'll need to explain. This all works perfectly fine for sending JSON objects back and forth. But our objects aren't just JSON objects, they're proper classes (as proper as JavaScript gives you at least) and functions with lexical closures. How are we supposed to serialize *those* over the network??

Well, like this. I lied a bit when I gave the move function definition. It actaully looks like this

function linear\_move\_withtime ( start\_pos, end\_pos, rate, start\_time) {

    return {

        \_function: "linear\_move\_withtime",

        args: \[... arguments\],

        act: curtime =>

            start\_pos.lerp(end\_pos,

                           clamp((cur\_time - start\_time)/duration,

                                 0, 1))

    }

This means that when the server serializes an function with JSON.stringify, it'll end up sending a blob that looks like this

{\_function: "linear\_move\_withtime", args: \[\[0, 0, 0\], \[5, 0, 0\], 3, 0\]}

(Notice the act attribute has just dropped. JavaScript's JSON.stringify removes any attributes whose values are functions. For fun I guess?)

Now we can just write a quick de-serializer for the client. This is going to look at the object type, and construct the right type of object. And so after all this, we have an efficient way to serialize communication between the server and client.

function fix\_json ( x) {

    if (x === null) return x;

    if (object\_ht\[x.uid\]) return object\_ht\[x.uid\];

    if (x.\_function) {

        return eval(x.\_function)(...(x.args.map(fix\_json)))

    } else if (x.\_constructor) {

        return new (eval(x.\_constructor))(...x.args.map(fix\_json));

    } else {

        return x;

    }

    return it;

}

Putting this all together we end up with some smooth animations and networking for making a game where you can attack someone else. Even though we're still running the server at just **five** game steps per second, there's only a tiny bit of teleportation on the remote side and for the most part everything looks great.

So everything I described above is how most of the game works. The problem is that there are a few situations where latency feels really bad for users. And one of these is when you try to take movement actions. If every time you made a movement request, the client had to go to the server and ask for your character to be moved, and then the server had to send this back to the client, the delay would be noticable. Especially for our TCP/websocket implementation, we're probably on the order of 100-200ms for any movement to happen, which is way too long.

So we cheat. Instead of the player telling the server "I would like to move to the left, please go do that and then tell me when it's done" we have the client just tell the server "I am moving to the left, I started already, deal with it and tell everyone else".

This has the effect of reducing the latency of this player to zero, but the problem now is that everyone *else* isn't going to see that movement started until a full roundtrip later. So I just change the game and make a player have to turn around in place before they can move, and this slows things down enough that the only thing the other players lose out on is a turn animation which is fine and much more forgiving.

And that's it! We have a full networking engine up and running that will perfectly synchronize state across multiple simultaneous connections, and transmits on average just 10kB/s when we have hundreds of objects and several players at the same time, even though we're sending JSON over the wire! Amazing.

What's missing from the above is the countless hours I spent debugging networking code that would cause just complely absurd things to happen for no good reason. A number of these issues were rather stupid and aren't worth talking about, but let me describe just a few of them that have maybe something like a generalizable lesson.

**Teleporting objects.** The way that I've put together the movement function, it takes the start and end points, and then current time to determine where it should be along this path. This all works just fine when you're running on one computer. But when I started playing the game on two physically different computers, objects were just teleporting to their end position right away and not smoothly moving from the start to the end.

Eventually I figured out the problem was that the two computers had (slightly) offset clocks. The other computer, in particular, was just a few seconds ahead. But these few seconds mean that when the first computer says "Move from A to B starting at time T and ening at time T+3" if the other computer is already at time T+3 then the object will teleport to B as soon as it is created. Not ideal.

Fixing this is fairly easy. We have a periodic ping-pong from server to client, and all clients synchronize themself with respect to the server. So we might be off initially, but we'll quickly get things into a coherent time. Who needs fancy time protocols.

**Circular object dependencies.** We send objects from the server to the client by just running JSON.serialize. This is really (JavaScript) space-efficient, but does have a few drabacks. One of these is that JSON doesn't support circular references. But circular references are common in game developemnt.

For example, consider two players A and B, each of whom are trying to attack the other. We might have something like A.target = b, and B.target = A. Now if you try to serialize A, you'll have

A={id: "A", target: {id= "B", target: {id: "A", target: ...}}}

B={id: "B", target: {id= "A", target: {id: "B", target: ...}}}

This is bad. To fix this, we only ever call JSON.stringify on *shallow* copies of objects, and for any property that itself is an object we only keep track of it's ID.

A={id: "A", target: {id= "B" }}

B={id: "B", target: {id= "A" }}

There's a little bit of weirness the first time that the client receives the server objects now, because it only receives a shallow copy of the objects but needs to create dense versions. So I do some hacking to make it work out. This is also why, earlier, I had to make sure to in-place update client objects instead of creating new copies.

**Unsynchronized movement.** Near the end of development I was something like 500 bytes over the 13kB space allowance and needed to find some way to cut down the code. When looking through the code I noticed that within my code I was writing repeating the following function format often

linear\_move\_withtime(source, destination, speed, get\_time())

where source/destination/speed are arguments that change each time, but I was alwyas passing the get\_time() function call for the final argument. I noticed I could save a lot of space by defining the helper function linear\_move\_withtime that dropped the last argument and compute it internal to the function, like this

function linear\_move\_withtime ( source, desination, speed) {

    var time = get\_time();

    // remaining code here

}

Because this is obviously the same, right? Except a little while later, I noticed that all of a sudden my lag reduction was completely broken and objects would be jumping all around without the smooth interpolations I'd worked so hard to create.

After far too long debugging things I realized what the problem was. Recall that the way we serialize functions to send from client to server is by passing a dictionary that looks like

{\_function: 'fn\_name', 'args': \[x, y z\]}

which will then be eval'd on the other end to construct the function. Well, what this means is that when the server calls linear\_move\_withtime instead of serializing all the argument (including the time it was constructed), we now send

{\_function: 'linear\_move\_withtime', 'args': \[src, dst speed\]}

And when the client runs eval, it's going to pass *it's current time*, and not the current time the server. Which completely removes all the benefits of designing this entire movement setup in the first place. So this was obvious in retrospect, but at the time was a huge pain to figure out.

**Teleporting objects, again!** At one time during the development process, I walked away from my computer and instead of closing the browser I left it open. And when I came back, a tower had just suddenly moved its position and was now floating somewhere in the sky. For no apparent reason. I double checked the tower code to be certain, and the towers never had any way to change their own position. So that couldn't be it. There was also no way for one object to move another object's position.

After a good amount of just being entirely confused, but eventually I looked back at how I was computing the object IDs and noticed I was constructing them by calling

this.uid = 0|Math.random()\*1e9

I implemented it this way because (a) the UIDs are sent on every packet, and so I wanted them to be small, (b) this code doesn't require synchronizing state from all the clients for generating UIDs, and (c) the code here is short (remember, at this point I'm 500 bytes over space).

Well, it turns out there's this thing called the [birthday paradox](https://en.wikipedia.org/wiki/Birthday_problem) which says (loosely) if you have N possile choices for an attribute, then you only need to see the square root of N items before you get the first collision. And the suqare root of a billion is ~thirty thousand, which means if I ever left the game running for an hour or two in the background, then all of a sudden you'll create a bullet or a minion that has the same ID another object, and then bad things happen.

Now any self-respecting security researcher will have taken advantage of this paradox dozens of times, but when I wrote this code I remember thinking to myself "Yeah no way this is going to actually become a problem". But I guess that's what you get for not being careful.

The fix here was actually rather simple. I just made the server be the only one who was allowed to create objects, and then IDs just became something that's incremented by one after every new object. So as long as we have fewer than 2^52 objects (when JavaScript's doubles won't have precision to allow increment to work correctly) we'll be good.

Now it's time to make a game. Unfortunately, if you calculate the cost of (3d engine + client/server networking) overhead, we only have about 6kB left to make something interesting happen. Or, to put a more positive spin on things: it takes *less than 7kB* to get a full 3d rendering multiplayer game engine! The problem though is that 6kB isn't much to make a game with. And I was also fairly busy in September doing actually productive things, so the code suffered a lot here. But nevertheless the show must go on. As promissed at the beginning, because it's now \*just\* a matter of making a game, which I've done twice now and is not the purpose of this project, let's take a whirlwind tour of the game.

Because last year I made a space game (and I was just really lucky that the theme this year was "SPACE") I started by replacing the boring grid with a space-themed map, and replaced the moving rectangle with a spaceship.

I stole the star backdrop from last time, and also stole the moving particles that help create a sense of movement. The bckground actually is layered three times, so there's a slight paralax where objects farther away move a bit slower than stars in the front which you don't really notice unless you're looking for it but I think helps make things feel better.

Each player will eventually have different actions they can take, but most of these involve shooting lazers at each other (yet again using my particle system from last time). Each object has a health bar that moves around with it, and when something gets damaged its health decreases. On death, the object explodes into pieces.

Now we just need something to shoot at, and for this I added the standard minion/creeps that MOBA games have. They have a really simple rule-based AI that goes something like this. At every turn, walk down the ordered list of instructions and take each action in turn.

1. If you've gone too far away from the path (that is, the major diagonal of the map), then make your way back. This rule takes precedence over all others so the minions never get too far away.
2. If you get damaged by anyone, they become your new target.
3. If you have an enemy target and it's been more than a second since the last time, shoot at them.
4. If youre moderately far away from your target, move closer. (If you're really far away, set the target to null.)
5. If you have no target, and an enemy in range, set that enemy as your target.
6. If you have no target and there's no enemy in range, just move up and to the right along the diagonal of the map until you get a target.

The one trick of minion movement is that you don't want minions to stack on top of each other. So whenever the logic says "move to some position" above, what it actually means is "find an open space near where I'm supposed to go". An open space here means two things: (1) there is *currently* nothing there, and (2) no other minion is *planning on being there* in the near future. These two rules are sufficient to make the movement entirely reasonable for the game.

And the final part of the game is for towers to do something. Towers are basically minions except they can't move, and so they have all the same rules as minions with respect to attacking but just don't follow the movement rules. (Also towers do more damage.)

And from here's it just a question of making different spaceships you can play as, and because at this point I had something like 1kB free and just a few days, it ended up with just enough to barely call everything a game. But it works! And because I'm now on hour 9 of what I promissed myself would be a 5 hour writeup, I'm just not going to even tell you about that. Go [play the game](https://js13k2021-yet-another-moba.herokuapp.com/) if you want to see it or [read the code](https://github.com/carlini/js13k2021-yet-another-moba) or something, but I'm done here.

* * *

There's also an [RSS Feed](https://nicholas.carlini.com/writing/feed.xml) if that's more of your thing.
