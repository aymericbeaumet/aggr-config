---
title: Yet Another Space Game (In 13kb of JavaScript)
link: https://nicholas.carlini.com/writing/2020/javascript-space-game-13k.html
source: nicholas-carlini-com
published: 2020-12-19T00:00:00Z
updated: 2020-12-19T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: This year I entered in JS13K 2020, a game jam for JavaScript games in under 13KB (total size). I wrote a 3rd-person space shooter game, building on top of game engine I built last year for a doom clone.
content: extracted
html: 2020-12-19-yet-another-space-game-in-13kb-of-javascript.html
preview:
  file: 2020-12-19-yet-another-space-game-in-13kb-of-javascript.preview-caab8a6c48ac.webp
  width: 256
  height: 160
  color: '#2e3533'
images:
- source: https://nicholas.carlini.com/writing/2020/fig/shooter_teaser.png
  original:
    file: 2020-12-19-yet-another-space-game-in-13kb-of-javascript.image-e1150753bc54.png
    width: 400
    height: 250
  color: '#030407'
---

Clicking the video above (or [HERE](https://nicholas.carlini.com/writing/2020/javascript-space-game-play.html)) will take you to the game. Source code is [available here](https://github.com/carlini/js13k2020-yet-another-space-shooter).

I don't really care (much) about making games, or even about making them particular fun (maybe that's why I never actually do well during the judging...) but I do like small time-constrained projects with tools I normaly don't use.

And making games pointless in JavaScript is just about as far as you can get from academic computer science research. Last year's entry was focused on building the game engine itself, and just making everything run at all. So, this year I decided it would be a good opportunity to build something “more”. So this means that I tried to just pack in as much game-like-stuff as possbile. The remainder of this post describes most of these.

![](https://nicholas.carlini.com/writing/2020/fig/shooter_teaser.png)

They say the number one mistake of trying to make a game is building a game engine. Well, I'd already done that last year. So this year the first thing that I did was take last year's code and gutted everything out of it except for what was absolutely necessary for basic rendering. Anything that I wasn't absolutely sure I needed got removed, because I knew that I could always go and pull code back in if I wanted it back.

This gave me a relatively clean slate game engine to start with. I wasn't sure where this game was going at first, but I knew that I wanted to blow things up in space. My initial vision was as a cross between [Descent](https://en.wikipedia.org/wiki/Descent_\(1995_video_game\)) and [Asteroids](https://en.wikipedia.org/wiki/Asteroids_\(video_game\)), and it ended up somewhere kind of like that.

This gave me an initial plan. First, I need to be able to fly around. Then, I need to be able to shoot at the asteroids. And then I'd see where things stood from there. I started off by putting the camera in a [skybox](https://en.wikipedia.org/wiki/Skybox_\(video_games\)), that has stars rendered out at infinity. Then the game making started.

I spent at least a few days playing around with variations of player movement. Initially it was truly Asteroids-like where player momentum was always preserved, but in three dimensions that was really rather disconcerning, and just not very fun having to manage accelerating the “proper” direction in order to move in a different direction. So I eventually settled on just fairly standard movement, where the player automatically slows down and stops even though there's no physical reason this would happen in space.

The camera moves by following around the player. Instead of directly following exactly behind the player at all times, it lags behind ever so slightly. This is achived by making the camera have a specific target in mind (always in the correct position right behind the player), but on every frame only moving (say) 20% of the way there. This gives a kind of exponential moving average of the player's past positions, and gives a nice appearance of movement instead of the much more rigid option of following the player directly.

The problem with this simple movement is that even though the player is moving around a lot, and moving quickly, it doesn't feel that way. Especially when there are no other objects in the scene, it's literally impossible to know the difference between moving quickly and standing still: the skys in the backdrop never move, and so there's no frame of reference. The camera lagging behind the player adds some feel of movement, but it just doesn't look right.

So what's the solution here? I briefly tried experimenting with putting a bunch of asteroids in the space so no matter where you looked there was something to see you moving by. But this cluttered everything up and was just generally very confusing. There was already too much happening even without a game going on.

So the solution I decided upon was to add little bits of just glittery-things that represent a sort of [aether](https://en.wikipedia.org/wiki/Aether_theories). It's always present, and the player moves though it. Now in order to make the game actually playable, it's not possible to actually fill the aether and render every particle in an infinite space. So what I do is generate a single 10x10x10 aether cube. Then, I tile the entire space with this single cube repeating over and over. (So if a player managed to move exactly 10 meters in one direction, they would see exactly the same configuration of particles. I figured this is unlikely to happen when there are three degrees of freedom.)

Now of course I can't *actually* tile the entire space with this cube, and this doesn't directly solve the problem. But I can pretend I have. Given wherever the player is, I redner only the cube for the 8x8x8 box centered (approximately) around the player. Doing this is actually fairly trivial with the following code:

var base = NewVectorFromList(camera\_position.\_xyz().map(x=>(x>>5)<<5));

range(8\*8\*8).map(z => {

    var v = NewVector((z&7)-3,((z>>=3)&7)-3,(z>>3)-3);

    if (v.vector\_length() < 3) {

        glitter.position = base.add(v.scalar\_multiply(32));

        glitter.render();

    }

});

The one detail here that important is that the aether cube should be centered not around the actual ship (which would be the physically realistic thing to do) but around the camera. The reason here is subtle: it gives a much more dramatic feeling of movement. In fact, by adjusting the density of the particles in the space it is possible to make movement feel much faster or slower by adjusting the rate at which the camera passes them by. Compare the two images above. Both travel at exactly the same speed (notice the objects in the background move at the same rate) but the the image on the left feels much faster.

The next thing I was worried about was how to make sure the player couldn't easily get lost in space. For this, I wanted to have a nice big star that was always visible and could easily be located to fly around. And this means particle systems.

A [particle system](https://en.wikipedia.org/wiki/Particle_system) is one of the core building blocks of most games: every time there are sparks, or smoke, or fire (or anything procedural of this nature) there's a particle system that's backing it. At the core, a particle system is just a way of creating an effect by rendering a bunch of independent particles that each follow an identical set of (randomized) rules.

Particle systems are rendered with the same two components as any objects in a game engine. The first is the shaders that actually draw the particles. My particles were going to be really simple and have just a few attributes. Particles are circles with a given position, velocity, color, size, and opacity. That's it. They can't accelerate, be affected by gravity, change shape, or anything else.

This lets me render particles with a really simple vertex shader that just places individual gl.POINTS in the correct position of the correct size

gl\_Position = projection\_matrix \* object\_position;

transparency \*= smoothstep(1., 0., (u\_time-a\_settings.x)/(a\_settings.y-a\_settings.x));

gl\_PointSize = (2000./gl\_Position.w) \* (2.+transparency) \* size;

v\_normal = a\_normal;

And then in the fragment shader makes them the circles of the correct color

vec2 from\_center = 2.\*gl\_PointCoord.xy-vec2(1,1);

out\_color.rgb = v\_color.rgb;

out\_color.w = smoothstep(1.,0.,length(from\_center)) \* transparency;

The second component of a particle system is how to decide where the particles are actually supposed to be drawn. This started with the simplest---but least efficient---implementation possible. Every frame, I manually (in JavaScript) computed the new location of each particle and updated the particle system's buffer with the new position and color buffers with the updated values. This is clearly very slow, but it does work, and served as a reasonable baseline. I could render something like 5,000 particles before things got slow. (Modern computers are amazing. This is 300,000 particle computatinos per second in JavaScript copying over to the GPU.)

This wasn't quite enough. As an improvement on top of this, I read a little bit into webgl's [Transform Feedback](https://open.gl/feedback) mechanism, which is the “correct” way to be doing this: it basically lets the GPU compute the new location of each next particle on each frame. This can get really fast---millions of particles per second---but is a bit overkill. The shortest implementation I could get for this was far too much space to fit in a 13k game.

Eventually I had an important realization. Everything I was doing in JavaScript to manually set the particle's new position, and new opacity, was a completely linear function of the initial position, initial velocity, and opacity. So all I have to do is store these initial values, and then instead of updating the position every frame, I just need a single scalar recording the age of every particle. I can pass this age to the shader, and then have the shader itself automatically compute where it should be, and what the opacity and size should be, given just the age.

Except I can do one better. Instead of having to update the age of every particle on every frame (this would still be a lot of work copying to the GPU every frame) I record, once, at what time each particle was created. Then, each frame, I only need to pass in a single float: the current time. Each particle can then compute how long it's been alive, and therefore what its attributes should be. The JavaScript side of this code looks like this:

update(dt, other\_max) {

    for ( var i = 0; i < this.ages.length; i++) {

        if (( this.ages\[i\] -= dt) < 0) {

            var \[ pos, vel\] = this.particle\_start(dt, max);

            this.ages\[i\] = this.maxage\*1000\*( this.use\_random||Math.random());

            this.setup\_particle(current\_time(), i, pos, vel)

        }

    }

    this.rebuffer()

}

The JavaScript particle system code keeps track of just one float per particle that is updated on every frame: how much longer it is allowed to live for. This age is decremented every frame, and if it's ever less than zero, then I create a new particle in it's place. The shader can now do all of the work, and compute where it's supposed to be, the opacity, and color---all given just the current time.

And now it's time for something completely uselss but really fun. Let's add some lens flare. Why? Because I want to.

I implemented the lens flare by mashing together a bunch of different ideas I found online. Briefly, all that's done is drawing a bunch of nearly-transparent circles at different poitns on the screen (if the player faces towards the star that generates the lens flare). In order ot make things somewhat nicer, there's a slight [chromatic aberration](https://en.wikipedia.org/wiki/Chromatic_aberration) that shifts the positions of the colors slightly based on the angle from the light.

It's time to start making this a space *shooter*. We've done the space part, let's add the shooting.

The idea is that while the player's ship rotates fairly slowly, the shooting will always target exactly where the cursor is pointing at any point in time. This makes it much less disorienting to shoot, and makes everything generally easier. Implementing it takes a bit of work, though.

Given the current (2d) mouse coordinates in screen-space, I now have to figure out what this corresponds to in world-space. Now this is easy without space constraints: take the world projection matrix (that converts from world space to screen space), compute it's inverse, and---bam!---you have the screen space to world space matrix. It's now just a single multiplication to solve our problem.

The problem with this is that computing the inverse of a matrix takes a lot of code. Several hundred bytes of code, a reasonable fraction of the *total* space constraints on just a single function is a hard pill to swallow.

Except here's the thing. There's a builtin inverse function in webgl. So instead of creating the correct view projection matrix in JavaScript land, I'm only ever going to create the correct *inverse* view matrix. Then, when I need to know how to take a 2d world position and project it into 3d position, I can multiply by this inverse view matrix.

And now, the first line of the vertex shader just computes the inverse and goes on with life. Is this ugly? You bet. But it's really short!

var mouse\_position\_3d = mat\_vector\_product(proj\_mat, NewVector(mouse\_dx\*2e5, mouse\_dy\*2e5, 1e5, 1e5)).add(camera\_position);

Shooting now is just a matter of making the right particle effect. This is mostly a matter of just playing around with the parameters that I have available (how long the particles live for, their size, how quickly they fade, and the color). Eventually I decided on the above animation that I thought looks quite nice.

Okay. Now we have a game where you can fly around a star, and shoot at ... nothing. Let's fix that.

There's not really much to this piece. I added a few enemies that fly around and shoot at you. I made each of the enemies in [blender](https://www.blender.org/). The fact that the ships have to be low poly helps here, because I am truly awful at anything design.

I'll talk about making the enemies do something interesting later on in the section on the Enemy AI. For now, they just sit and look (not so) pretty as a target.

So now there are enemies ... but we can't actually shoot them because there's no way to detect if we hit them or miss. The easy and cheap thing to do is to just compute the distance between the point of the enemy and the ray of where you shot, and if it's close enough, declare it a hit. The problem with this is that while people may not notice small errors when shooting at enemies, they certainly aren't going to be forgiving about *being shot*.

If your spaceship isn't a perfect sphere, and an enemie misses you but you take damage, people will bitterly complain. So I'm going to have to detect precise collisions eventually. It turns out that this is, again, a time where we can just say computers are fast, just brute force it. Every lazer, every frame, will check its collision with every possible object that can be shot at. To do this, I literally check every single ray-triangle intersection in the object. Tens of thousands of checks every frame! But it works!

function does\_hit ( position, direction, filter) {

    objects.map(other => {

        other.sprites.map((sprite, sid) => {

            sprite.b\_positions.map((tri,idx) => {

                tri = tri.map(x=>mat\_vector\_product(sprite.rotation, x).add(sprite.position))

                var distance = ray\_triangle\_intersect(position, direction, ...tri);

                if (distance > 0) {

                    return true;

                }

            })

        })

    })

    return false;

}

The actual code is a little more efficient, and has a few early-stop checks that skip collision checks for objects that are too far away to ever possibly collide. It also doens't just report if there's a hit or miss, but returns which object and at what position the hit occurred. But this is just multiplying the distance by the direction, so I've left it out too.

So once contact occurs, it's time to make a show of it. The first few times something gets hit, it just does a particle hit-effect and shows that something happened. Doing this naively is fairly simple, but doesn't loook great.

It's now time to make the enemies blow up. To do this I basically stole the code I used last time in my doom clone: every object breaks apart into the components that constructed it, and flies out towards infinity. This gives a fairly satisfying result.

It's really easy to get disoriented in three dimensions. To help give the player some sense of direction, every enemy will have a HUD tracker that shows its current position if it's on the screen, and if it's off the screen will give an arrow pointing to where that enemy is.

Making the projection work in this way actually is fairly trivial. I render it as if it's any other object in the scene, take the target's position in three-dimensional space, and project that on to the two dimensional screen with the same view projection matrix that does any of the work. However, in order to ensure the HUD always shows up, disable the depth buffer test and always draw the tracker on the top of the screen. Doing this literally just requires a few lines of code changed to the camera

Showing an arrow to where the object is if it's off-screen takes a tiny amount more work, but not much.

if (v\_off\_screen > 0.) {

    float a = -atan(uv.y, uv.x);

    vec2 x\_dir = vec2(cos(a),sin(a));

    vec2 y\_dir = vec2(-sin(a),cos(a));

    out\_color.w = dot(from\_center,x\_dir) > -1. && dot(from\_center,x\_dir)+abs(dot(from\_center,y\_dir))\*3. < -.3 ? 1. : 0.;

} else {

    out\_color.w = min(abs(from\_center.x),abs(from\_center.y)) < .25 ? 1. : 0.;

}

In order to put objects into the game there needs to be some way to specify how they look. Last year I used the [object lathe](https://en.wikipedia.org/wiki/Lathe_\(graphics\)) which lets you build up objects from simple primitives. However, it was really rather difficult to build up better looking objects.

So this year I decided to build a .obj file format compressor. Naively, an object is a list of triangles, each polygon requiring an XYZ triple. Now if you export a standard cube as a .obj file, just about the smallest file you can get is 136 bytes. Given that a single spaceship is going to be tens of objects, and I'll want five of them, that would almost completely eat up my entire space budget in ojbects alone.

Now this wouldn't at all work. So I decided to do something to shrink the objects fairly considerably. I managed to get something like a 100x compression ratio when compared to the basic .obj files. How did I do that? Well, this is already really long, so I think I've decided that I'll write up a completely separate thing on how to do 3d object compression. I think I'll be able to do better, and so want to spend some time getting it right.

Alright now I have everything needed to make a game. The final piece left is to actually ... make it a game. This is where I spent the least time, and does it ever show!

I eventually decided a survival game where the goal is just to live as long as possible. Enemies keep coming, and with increasing frequency, until eventually you get overwhelmed and die. This means that the actual game component is fairly minimal, and doesn't require much work on top of what's already built.

Enemies in this game are just about as dumb as they come. In fact, the AI is basically designed to make entertaining targets to shoot, and does basically nothing intelligent to attack the player.

At any point in time, enemies have a target in mind. They will move towards the target, and if it's something that can be shot at (and they're facing towards it) will shoot at it. Enemies can only turn at a fixed rotation per second, and this gives the nice image as if they were really flying around with some momentum.

By default, enemies will target the base and attack it, and will completely ignore the player. This changes when they get shot at: if the player shoot (near) an enemy, then they'll switch target to the player.

If the enemy gets within a small distance of the player, the enemy will enter a new RUN AWAY state. It sets a target at a random offset away from the player, and moves that way until it gets far enough that it can go back to attacking. If the player turns to chase the enemy, then the enemy will keep going in a similar direction more or less forever until it gets destroyed.

The one piece of intelligence I added was some aim-prediction. Instead of shooting directly at wherever you are *right now*, the enemies will shoot so that they will hit wherever you *will be* once the bullet arrives. There's probably some kind of smart way to actually compute this, but I got really lazy and just iteratively refine where I expect the player to be ten times and call it good enough.

var predicted\_location = me.position;

var vel = lazerspeed( this.damage);

range(10).map(x=> {

    var time\_to\_hit = predicted\_location.distance\_to(start\_pos)/vel;

    predicted\_location = me.position.add(me.velocity.scalar\_multiply(time\_to\_hit))

});

Given that the goal of the game is to stay alive as long as possible, I wanted to have some nice effects for when you lose and die. So when the ship gets sufficiently damaged, the screen slowly pans back as time slows down and shows you get blown up.

There's probably more to making this game than I remember now. But given that it's already three months since I made this and I'm still procrastinating the writing, this is all you'll get.

I guess the obvious question is what comes next. I don't have any desire to do this again, because now that I've mostly explored the space of what needs to go into a game it's less interesting than it was when I didn't know how to do it. Maybe next year I'll think of something new that would be surprising to be able to fit in 13k.
