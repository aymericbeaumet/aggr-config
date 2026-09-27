---
title: Yet Another Doom Clone (In 13kb of JavaScript)
link: https://nicholas.carlini.com/writing/2019/javascript-doom-clone-13k.html
source: nicholas-carlini-com
published: 2019-09-13T00:00:00Z
updated: 2019-09-13T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: This year I entered in JS13K 2019, which asks people to develop games in under 13K of JavaScript. I entered a Doom Clone called ... Yet Another Doom Clone.
content: extracted
html: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.html
preview:
  file: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.preview-2ec446346e7c.webp
  width: 256
  height: 160
  color: '#6c603f'
images:
- source: https://nicholas.carlini.com/writing/2019/doom.jpg
  original:
    file: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.image-9d24c8e38deb.png
    width: 400
    height: 250
  color: '#474747'
- source: https://nicholas.carlini.com/writing/2019/fig/object_lathe.png
  original:
    file: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.image-6843caa8a2ad.png
    width: 595
    height: 405
  color: '#262626'
- source: https://nicholas.carlini.com/writing/2019/fig/texture_brick.png
  original:
    file: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.image-5972f29e14a6.png
    width: 511
    height: 511
  color: '#e5e5e5'
- source: https://nicholas.carlini.com/writing/2019/fig/texture_tile.png
  original:
    file: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.image-ddecfde2aecc.png
    width: 508
    height: 512
  color: '#000000'
- source: https://nicholas.carlini.com/writing/2019/fig/texture_perlin.png
  original:
    file: 2019-09-13-yet-another-doom-clone-in-13kb-of-javascript.image-0dec946204a4.png
    width: 510
    height: 461
  color: '#878787'
---

Clicking the video above (or [HERE](https://nicholas.carlini.com/writing/2019/javascript-doom-clone-game.html)) will take you to the game. Source code is [available here](https://github.com/carlini/js13k2019-yet-another-doom-clone).

Why create a FPS in JavaScript in a total of 13k (compressed)? There are a few reasons. But probably the best answer is that the JS13K contest FAQ answers the question *“Can I use WebGL?”* with *“Yes, though it might be hard to fit it into 13 kilobytes if you plan on doing an FPS game.”*

But aside from that fact, I had just made a [3D renderer](https://nicholas.carlini.com/writing/2019/3d-renderer-javascript.html) and wanted to keep working on it more. Also, writing heavily compressed code is something I enjoy doing, (For example, many years ago I created a language and wrote a compiler for a new language designed specifically for use in [code golf](https://en.wikipedia.org/wiki/Code_golf).)

So that's why an FPS. What about the question “why doom?” The answer here is easier. If you're going to write an FPS, and want to keep it small, doom is about as minimalistic as you can get.

The reason doom is so simple (by today's standards) is clear: doom had to run on hardware five orders of magnitude slower than what we have today. For the same price today, it's entirely possible to build a machine that can do a hundred thousand times the work of the Pentium of 1994. And so, I thought it would be fun to try and re-create something like doom but instead of handling performance constraints, handling space constraints.

![](https://nicholas.carlini.com/writing/2019/doom.jpg)

I based the game engine on a [3D renderer I had been working on previously](https://nicholas.carlini.com/writing/2019/3d-renderer-javascript.html) with the intention of being as simple as possible. It turns out by being simple, it's also quite small: the core 3D rendering engine only came out to about 5KB of compressed JavaScript, so that left 8KB for the game itself. I figured this wasn't that bad of a state to be in, after all. As long as I didn't use any large game sprites or object files it should work out.

Going from a 3D renderer to 3D game engine basically just requires one change: player-controlable camera movement followed by re-rendering after every movement. To do this, I put the player on the inside of a box and worked for a while making movement feel right. Doom has a very distinctive style of movement that I tried to recreate.

The first step towards this is making the player accelerate in the direction they're facing up to some (fast) maximum speed. I had more acceleration than the original doom because that seemed more normal now. Most importantly to make things feel doom-like is adding the camera bobbing as the player moves. In the steady state this is just a sin wave, but there are some necessary details to make it feel right when going from stopped to moving.

If you don't do anything additional, then the player will *resume* the bobbing from wherever they were when they stopped. This looks very wrong. Fixing it is actually just some very small tweaks: The player maintains a variable of where the animation is in its cycle (modulo two pi), and when the player stops moving in any direction, the variable resets itself to zero. This really cleans up the motion and makes starting and stopping feel right.

One feature I added that doom didn't have was I added a small amount of camera [roll](https://en.wikipedia.org/wiki/Aircraft_principal_axes#Longitudinal_axis_\(roll\)) on every step. (Doom doesn't have this because it only allowed rotation along the z-axis, the player couldn't even look up.)

From there I added clipping. There's no reason to create complicated doom-esque mazes if the palyer can walk through walls. Now, the “correct” way to handle clipping is to make the player is a sphere, the walls plans, and then detect [sphere-plane intersections](https://en.wikipedia.org/wiki/Circle_of_a_sphere). I was worried this would take too much space for a 13k budget.

Instead, the game extends eight rays out of the current player's position, and detects if any of those rays intersect with a wall. If any does, then I back the player up so that it's at least five units away from the that wall. This has the effect of being a poor-mans sphere collision detection while also coming in at just a hundred bytes.

Getting this right was somewhat finicky, especially when the rays intersect with multiple walls simultaneously. It's still broken in a few situations, so when designing the levels I manually tested for glitches and just designed the levels around the collision detection bugs. (Hey, if it works...)

![](https://nicholas.carlini.com/writing/2019/fig/object_lathe.png)

In order to have any objects in the game, I was going to need to represent them somehow. One natural method would be to use the [.obj](https://en.wikipedia.org/wiki/Wavefront_.obj_file) file format. Unfortunately, this is quite large and cumbersome. (This is what I did for my prior 3D remderer to load the mandatory teapot.)

Instead, I decided to write a minimalistic [object lathe](https://en.wikipedia.org/wiki/Lathe_\(graphics\)) method: given a 2D path represented as a sequence of points, lathing rotates the points around in a circle to generate a 3D object. This lets you create some very nice objects with only a very small amount of space. For example, the jug pictured above comes in at around twenty bytes of code to represent.

For a long time through the development of the game I also created a heavily-minified 300-byte implementation of [surface subdivisioning](https://en.wikipedia.org/wiki/Subdivision_surface) (that I'd previously used for the 3d renderer). But at the end of the day, I ran out of space and had to cut it---I didn't feel too bad about this, because it really wasn't adding anything to the game.

Up until here everything has been fully static. Only the player camera moves through the world. Making a game, however, requires interactivity. So that's where the game loop comes in.

The game loop here is just the completely standard game loop. Initially it was actually the following code:

      function game\_step ( now) {

          if (game\_is\_over) return;

          frame = requestAnimationFrame(game\_step);

   player\_check\_dead();

   player\_move();

          player\_clip\_walls();

          player\_fall();

          player\_set\_position();

          lights.map(light => light.compute\_shadowmap());

          camera.draw\_scene();

          objects.map(object => object.update(dt));

          garbage\_collect();

      }

While this really is nice and pretty code, each of the functions there was only ever called once. So they all got inlined and the loop got ugly.

A first person shooter isn't much fun without something to shoot with, or shoot at.

I started with the “shooting with” problem. I wasn't sure if there would be enough space for more than one gun (narrator: there wasn't), and for a game designed around shooting, I figured starting with the doom chaingun was the natural choice.

The gun model is just a few cylinders made with the object lathe. Again the first step here was to make moving with the gun feel right. To do this I just re-use the existing tracking for the up-down movement when walking: the gun moves both up-and-down as well as side-to-side, depending on where the player is in the walking cycle.

The next necessary step was making shooting with the gun feel satisfying. That had three components. First, it has a small kick-back every time it shoots. Second, it spins. And finally, there's a small flash every time it shoots. All of these contribute nicely to the feel of the game. For a game that revolves around shooting, it has to.

Hit detection is implemented as a simple ray-cast hitscan (using the same code as the collision detection with walls for player movement) followed by creating some explosion particles and a small light flash.

I had to make a few changes to the ray-cast for hitscanning. First, because the initial implementation just checked horizontal collision, I had to check if the collision was the correct z-height when it arrived. This wasn't too hard: compte how far the object is away from the player, looking at the players pitch determine what the bullet height would be, and if that is within the objects height bounding-box, then it's a collision.

(One other minor detail was necessary to allow shooting the ceiling and floor. Otherwise when aiming too high there would be no explosion effects and it looked all wrong. All other collision detection only detects horizontal collision, but these are vertically-oriented and so needed a bit of special-purpose code. Doing this is actually not too difficult: record the starting z-height and pitch (angle off of the xy plane), and then for every polygon in the entire map, check how far the ray would have to extend to hit the floor and the ceiling of that polygon; then check if stepping that far actually ends up in the polygon at that location.)

After this, I sort all potential collisions and choose the closest one.

Shooting also has some screenshake, and because there is no ammo, slows down player movement. It's important to give the player a reason to not hold down the trigger the entire game.

Then I turned to the “shooting at” problem. I initially created a really basic block-model of a person. It's entirely based on cubes which makes it very easy to store: by saving just a single cube model, I can stretch it in arbitrary directions and get an enemy. The basic enemy then consists of eight boxes.

The death animation started out as something simple and the enemies would just fall over. This didn't look all that bad, but shooting felt weak and boring. So instead, I made a few tweaks so that the enemy would remember each of the constituent cubes that makes it up and then when it gets hit, explode into pieces. This felt much better.

Initially the blocks that made up the enemy would just fly through walls and off the screen. This was somewhat disconcerning, and so I added an effect to make them bounce off walls. The bouncing is completely physically implosible in order to be short. Collision is again implemented with the ray intersection. Then, if the nearest wall is completely on the xz plane, the objects reflects its y velocity. If it is completely on the yz plane, the x velocity is reflected. In all other cases, both the x and y velocities are reflected.

When the object position is lower than the current room's floor height, the z-velocity is negated and decreased by 20%. Every second the object slows down by 50% (as if by some hypotehtical friction).

For someone whose degree and career is ostensibly in (with lots of quotes) ““AI””, the enemies here are really rather unintenilgent. Enemies have a position and rotation. They pace back and forth forever, until being woken up, either (a) because the player is within line-of-sight, or (b) because an enemy within line-of-sight gets shot.

Detecting line-of-sight is again implemented with the raycast implementation which is re-used from detecting where the gun hit is supposed to be and from collision detection. In order to not burn cpu cycles with even more ray casts, the enemy only checks for enemies once per second.

Once awake, the enemy will move in a straight line directly towards the player. If something gets in the way, the enemy will pick a new random direction and move that way for a few steps, and then return to moving towards the player. Occasionally, the walking enemies will shoot at the player.

Also, whenever an enemy dies, it requests help from all other enemies that it can see (again through a ray cast). This makes it hard to just pick off enemies one by one while hiding behind a corner, which is a boring game mechanic that shouldn't be encouraged.

Until now everything was very dull being just one texture. I knew there was no way I would fit in any manually-drawn textures---even small ones---so instead I procedurally generated them. I opted to go for just three textures: a tile floor, a brick pattern for walls, and simple [Perlin](https://en.wikipedia.org/wiki/Perlin_noise) noise for lava and explosions.

![](https://nicholas.carlini.com/writing/2019/fig/texture_brick.png) ![](https://nicholas.carlini.com/writing/2019/fig/texture_tile.png) ![](https://nicholas.carlini.com/writing/2019/fig/texture_perlin.png)

The brick generation is somewhat trivial. Draw horizontal lines every N steps, and then vertical lines every N steps offset by N/2 when we're on even numbered rows. The code is just the following:

      make\_texture(cartesian\_product\_map(range(256),range(256),(y,x) => {

          if ((y%64) <= 2 || Math.abs(x-(((y/64)|0)%2)\*128) <= 2) {

              return \[0,0,0,1\];

          } else {

              var r = .9-perlin\_noise\[x\*256+y\]/20;

              return \[r,r,r,1\];

          }

      }).flat())

Tile generation is a little bit tricker. First I created a hexagonal lattice. Then, for each pixel in the tile, I compute the distance to the nearest two points on the lattice. If the distance between to the closer point is equal to double the distance of the second closest, then I draw a line. This gives the standard tile pattern.

[Perlin noise](https://en.wikipedia.org/wiki/Perlin_noise) is visually a much nicer type of noise than other noises. It looks more naturally-random than just pure white noise. Perlin noise generation is complicated enough I'll just point to the wikipedia article and say “I do that”. I also add a small amount of perlin noise to the brick pattern to make it less boring.

Having the enemies glide around the map wasn't very visually interesting, so next I added a walking animation. This took surprisingly little space, and just consisted of moving the limbs like pendulums.

Only having one enemy type wasn't going to work and was fairly boring, so from here I added a smaller flying enemy that moved faster and was harder to hit. The representation is again very minimalistic: is just two wing-looking trapezoids with a rectangle body. To make it look like it's flying, the wings flap a bit and it flutters up and down, again re-using the animation code from the walking enemy.

It seemed like the obvious choice for adding audio was [JSFXR](https://github.com/mneubrand/jsfxr). However at this point space was getting tight. After taking a quick look at the code I realized there was a bunch of room for optimizations.

I managed to shorten it by almost a factor of two through a series of small tweaks. The biggest saving came from removing the mapping from the array to playable WAV file and replacing it with a [WebAudio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) call which directly takes a float array. Next biggest was a bunch of case statements I replaced with array lookups. That is, I do the multiplexer thing and compute all cases inside of an array, and then just index out the one I want to keep. Slower, but shorter. After replacing the for loops with maps, and writing a clamp helper function, it was short enough to allow writinng a some nice sound effectsf.

Then I wanted to make the audio system respond to where the player was in the 3d space. I thought this part was going to take a bunch of effort but it turned out that it was quite easy, as there's now a [createStereoPanner](https://developer.mozilla.org/en-US/docs/Web/API/BaseAudioContext/createStereoPanner) function which does exactly this for you.

For background music, I transcribed the middle portion of [Toccata and Fugue](https://en.wikipedia.org/wiki/Toccata_and_Fugue_in_D_minor,_BWV_565). This just plays on repeat forever.

One of the defining characteristics of doom is the levels it puts you through. Each map feels more like a maze, and the player is running around looking for the way out. This was going to be a necessary component to get right.

However, storing the maps naively was going to take far too much space: even just storing some basic levels took over a kilobyte when represented naively (e.g., by storing the position of all walls). So instead, maps are represented by a collection of (potentially non-convex) polygons. Each polygon can have a floor height and ceiling height, and when two adjacent polygons share an edge the plaeyer can pass from one to the other (possibly going up or down accordingly).

The code that turns the polygons to actual three dimensional rooms requires a little bit of work. While it is easy to place walls around every polygon, determing when there should be *no* wall---because there are two adjacent edges---is in general difficult. To simplify this process, an opening is only created if two walls share both coordinates exactly.

Initially I just started creating the maps by hand-coding the polygon locations, but there were two problems with this: first, they were taking a bunch of space, but more importantly, it was taking forever to make them. If I wanted to be able to design enough maps I was going to need to do somthing better.

Figuring it may be possible to resolve both issues simultaneously, I implemented a tiny, tiny [turtle](https://en.wikipedia.org/wiki/Turtle_graphics) programming language to define the locations of the map regions. That is, instead of using the turtle to draw, I used it to create the polygons by replacing the drawing pen with a polygon-making function. Initially this language was very turtle-like: commands would move the turtle around by rotating it and having it move specific distances.

Att one point I even added functions to allow for loops and call/return so that I could, for example, create stairways and then re-use the “make-stairs” function every time I wanted stairs.

Unfortunately, this was still difficult to design make maps, as it required programming the turtle in an assembly language designed to be as dense as possible. With just a month to build the entire game, there was no way that I could afford to spend a few days just designing maps. Worse, it actually wasn't saving that much space. Because the 13k restriction is on the size of the zipped source code, duplicating the same function twice is perfectly fine: zip compression will save the space for me.

So the turtle code literally becomes this:

      function run\_turtle ( commands) {

          var regions = \[\]

          var turtle\_location = \[ZERO\];

          var floor\_height = 4;

          var ceil\_height = 40;

          var do\_later = \[\];

          for ( var i = 0; i < commands.length;) {

              var cmd = commands\[i++\];

              // Get the "opcode" and "argument" to the command

       var \[ low, high\] = \[cmd&31, cmd>>5\];

              // Opcode 0 is a "make polygon" command

              if (high <= 1) {

                  var coordinates = \[turtle\_location\[0\]\]

                  coordinates.push(...range(low).map(x=> {

                      var dydx = NewVector(((commands\[i\]>>4)-7)\*8,

                                           ((commands\[i++\]%16)-7)\*8, 0)

                      turtle\_location.unshift(turtle\_location\[0\].add(dydx));

                      return turtle\_location\[0\];

                  }));

                  regions.push( new MapPolygon (coordinates,

                                              floor\_height,

                                              ceil\_height))

                  // Opcode 1 is a "goto" command

                  if (high == 1) {

                      regions.pop();

                  }

              }

              // Opcodes 4: adjust ceiling

              floor\_height += 2\*(low-15)\*(high==4);

              // Opcodes 5: adjust floor

       ceil\_height += 4\*(low-15)\*(high==5);

              // Opcodes 6: backtrack

              turtle\_location.splice(0,low\*(high==6));

          }

          return \[regions, do\_later\];

      }

In order to make it easier to create maps, I scrapped most of the turtle language and made a much simpler language. I removed the loops, function calls, rotational movement, and implemented just two opcodes: make-polygon, and goto. The make-polygon opcode takes the number of vertices, and then has a sequence of bytes where the high 4 bits gives how far to move along the x axis and the low 4 bits how far to move along the y axis. After reacing the final vertex, the loop is closed and the turtle creates a polygon there. The move opcode just moves the turtle to a new location to place another polygon.

After making a few maps, I looked at what parts of my turtle language were taking the most space. I realized I was wasting a lot of space moving the turtle from polygon to polygon. How could this be made shorter? Well, notice that the polygons are typically not isolated: they are connected to other polygons by a vertex. Therefore, I added another function called backtrack. Every time the turtle moves, it adds its location to a stack. Backtrack pops the turtles' position off the stack some number of values ago. This was very effective: over 95% of turtle movement was replaced by backtrack commands.

In order to allow me to make maps efficiently, I quickly hacked together a WYSIWYG interface to help me design maps. This editor then compiles these maps to the turtle language, so they can be used in game.

Updating the map in the editor updates the rendered game in real-time to allow me to see exactly what was happening. The diagram above shows my general process for creating a new map.

Because this editor wasn't going to be bundled with the actual playable game, I didn't worry about code quality or usability of the editor. It's incredibly ugly and hackish, and the keybindings to use it are arcane (want to add a new polygon? Select an existing vertex and type shift-A to highlight the adjacent edge and then e (to extrude). want to add a vertex on an existing edge? shift-A s (to split). dete an existing polygon? shift-Q. they all make sense if you know what they do, but it's very unintuitive.)

Successfully creating a full game in 13k of compressed JavaScript requires constantly keeping code space in mind.

The main necessary objective to get everything working in 13k is to always be aware of code size. I updated the python script to monitor every time a source file was changed and to re-build the package, and show the number of free bytes remaining on the game itself so it would always be visible and I would ask myself if whatever change I just introduced was worth the complexity.

It was also helpful to write a bunch of helper functions that I used all throughout the program. Here are just a few of the ones that I found most useful:

      var pairs = (lst,fn) => lst.slice(0,-1).map((x,i)=>fn(x,lst\[i+1\],i))

      var transpose = (mat) => mat\[0\].map((x,i) => mat.map(x => x\[i\]))

      var range = (N,a) => Array(N).fill().map((\_,x)=>x+(a||0));

      var reshape = (A,m) =>

          range(A.length/m).map(x=>A.slice(x\*m,(x+1)\*m));

      var urandom = \_ => Math.random()\*2 - 1;

      var push = (x,y) => (x.push(y), x);

      var clamp = (x,low,high) => Math.min(Math.max(low, x), high)

      var sum = (x) => x.reduce((a,b)=>a+b)

I also defined my own matrix and vector math in order to make everything as minimalistic as possible. Rotations are all defined as 4x4 matrix products: [quaternions](https://en.wikipedia.org/wiki/Quaternions_and_spatial_rotation) would be much more efficient, but also take more space, so I didn't use those.

In some of the ugliest code I wrote, I got the 4x4 matrix multiplications to be both very efficient *and* really short, something that took some work:

    var mat\_product\_symbolic = B =>

        \`(a,b)=>\[${reshape(range(16),4).map(c=>B\[0\].map((\_,i)=> B.reduce((s,d,j)=>\` ${s}+b\[${d\[i\]}\]\*a\[${c\[j\]}\] \`,0))).flat()}\]\`;

    multiply = eval(mat\_product\_symbolic(reshape(range(16),4));

Constantly showing the current build size required an automated build process. The build script initailly just ran [uglifier](https://www.uglifyjs.net/) followed by standard zip to keep track of space. I soon realized that there were some optimizations that I could automate to help compress things more. I wrote a short script to identify all webgl variable names and rewrite the variable names to short one-letter names (since this isn't done by uglify). Then I modified this program to rewrite long WebGL function names like framebufferTexture2D with a short three-character code of e2m (where I take the 8th from last 2nd from last, and and 3rd characters respectively). Near the end of of development I came across [advzip](https://en.wikipedia.org/wiki/AdvanceCOMP) which helps more with better comopression.

* * *

There's also an [RSS Feed](https://nicholas.carlini.com/writing/feed.xml) if that's more of your thing.
