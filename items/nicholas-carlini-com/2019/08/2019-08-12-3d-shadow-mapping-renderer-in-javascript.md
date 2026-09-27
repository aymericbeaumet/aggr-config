---
title: 3D Shadow Mapping Renderer in JavaScript
link: https://nicholas.carlini.com/writing/2019/3d-renderer-javascript.html
source: nicholas-carlini-com
published: 2019-08-12T00:00:00Z
updated: 2019-08-12T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Late last year I decided it would be fun to build a 3D renderer in JavaScript. Recently it got into some sort of finished state and decided to put it here. This isn't so much of a tutorial on how to get there, but rather more of a here's a fun thing to do with nice pictures. But it was interesting to do. So here's that.
content: extracted
html: 2019-08-12-3d-shadow-mapping-renderer-in-javascript.html
extra:
  content:interactive: true
---

Late last year I decided it would be fun to build a 3D renderer in JavaScript. Recently it got into some sort of “finished” state and decided to put it here. This isn't so much of a tutorial on how to get there, but rather more of a “here's a fun thing to do with nice pictures”. But it was interesting to do. So here's that.

START

To start out I really wanted to do this from first principles and didn't want to “cheat” by using any kind of libraries. Initially, that meant writing the rendering engine entirely with the \<canvas> 2d graphics library, doing all of the 3d math by hand.

Technically there's nothing terribly interesting going on with this initial renderer. It's just over 250 lines long and does the naive thing: for each triangle in the three dimensional space, the renderer projects it onto the two dimensional screen, drawing from the back to the front.

 Smooth surface:

START

Once I actually got that working doing all of the math by hand in JavaScript I decided that using [WebGL](https://en.wikipedia.org/wiki/WebGL) would probably be worth it, and actually probably wasn't cheating all that much. WebGL exposes access to the GPU-enabled rendering engine through JavaScript. While it does abstract away some of the rendering, it's less than I thought---it just supports the ability to do the necessary math efficiently---so I decided this wouldn't be cheating. And fortunately, it didn't take long to reproduce the initial renderer, but this time supporting much better (and more efficient) graphics.

 Shadow map resolution: 256

START

Once the base renderer was ready, I started adding lighting with shadows. To do this I used [shadow mapping](https://en.wikipedia.org/wiki/Shadow_mapping): one of the most common approaches for generating real-time shadows. At a high level, shadow mapping works by rendering the scene twice every frame. First, from the perspective of the light source, we render the scene and record how far the closest object is to the light, for each pixel in the scene. This gives us the “shadow map” Then, we render the scene again, this time from the actual camera. In order to determine if any given pixel is in light or is in the shadow, we project it back onto the (pre-rendered) shadow map. If this pixel is the closest one to the light, then it's in light; if not, it's in the shadow.

Unfortunately, the light is fairly blocky.

Getting a high-quality shadow on my old laptop without a dedicated GPU wasn't possible: I had to increase the dimensions of the shadow camera to 2048 by 2048, and I was no longer getting a smooth 60 frames a second. Also, those hard edges on the shadows really don't look that realistic.

 Shadow map resolution: 256

START

In order to get nicer-looking shadows efficiently, I decided to move to a slightly fancier method: [variance shadow maps](http://student.agh.edu.pl/~mradzisz/EGA/vsm_paper.pdf). At their core they do the same thing as standard shadow maps: first, render from the light, recording the distance to the light for each pixel; then, render from the camera. However, variance shadow maps introduce one key difference: on the first rendering pass, instead of just recording the distance from the object to the camera, also record the distance squared.

But first, let's back up---one of the main reason that standard shadow maps look bad is because of their hard edges. It would be possible to blur the edges slightly by taking a large number of random samples when rendering from the camera, but this doesn't look great and is very inefficient.

Variance shadow maps allow using [Chebyshev's inequality](https://en.wikipedia.org/wiki/Chebyshev%27s_inequality) to get a nice smooth blur by estimating the fraction of pixels that are occluded by comparing the squared expected distance to the expected distance squared. The math is somewhat complicated here, but when you do it right, you get a pretty picture.

At the same time, I also added some [light bloom](https://en.wikipedia.org/wiki/Bloom_\(shader_effect\)), the effect that extra-bright lights cause a fuzzy blur around the boundary. Given that I had already written a blur method for computing the shadow maps, adding a filter to the screen was straightforward.

And that's all there is.
