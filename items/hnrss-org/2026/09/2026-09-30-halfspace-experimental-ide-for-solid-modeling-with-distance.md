---
title: Halfspace experimental IDE for solid modeling with distance fields
link: https://www.mattkeeter.com/projects/halfspace/
source: hnrss-org
published: 2026-09-30T19:44:37Z
updated: 2026-09-30T19:44:37Z
first_seen: 2026-10-01T06:33:51.242437607Z
authors:
- luu
content: extracted
html: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.html
preview:
  file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.preview-eb51e354352f.webp
  width: 256
  height: 151
  color: '#423e3a'
images:
- source: https://mattkeeter.com/projects/halfspace/hero@2x.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-4de164982244.png
    width: 1200
    height: 707
  color: '#282828'
- source: https://www.mattkeeter.com/projects/halfspace/hero.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-8aa72b99fa58.png
    width: 600
    height: 354
  color: '#272727'
- source: https://www.mattkeeter.com/projects/halfspace/overview.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-f5039bfecd5c.png
    width: 282
    height: 455
  variants:
  - file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-381bd786eb09.webp
    width: 282
    height: 455
  color: '#fde9ec'
- source: https://www.mattkeeter.com/projects/halfspace/shapes.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-1b7ec4f697d2.png
    width: 514
    height: 494
  color: '#fcfcfc'
- source: https://www.mattkeeter.com/projects/halfspace/sponge.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-792327c6f6db.png
    width: 600
    height: 567
  color: '#363536'
- source: https://www.mattkeeter.com/projects/halfspace/print.jpg
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-0095c534c50b.jpg
    width: 512
    height: 384
  color: '#e9e9e7'
- source: https://www.mattkeeter.com/projects/halfspace/bad_sawtooth.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-38c123b59458.png
    width: 400
    height: 254
  color: '#d6f2fe'
- source: https://www.mattkeeter.com/projects/halfspace/good_sawtooth.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-29c0092c1768.png
    width: 400
    height: 254
  color: '#875a2d'
- source: https://www.mattkeeter.com/projects/halfspace/cabin_bad.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-e751c7090623.png
    width: 400
    height: 388
  color: '#b7b7b7'
- source: https://www.mattkeeter.com/projects/halfspace/better_sawtooth.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-5ae54b3e95e7.png
    width: 400
    height: 254
  color: '#885a2d'
- source: https://www.mattkeeter.com/projects/halfspace/wgpu.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-238d6740c786.png
    width: 437
    height: 404
  variants:
  - file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-eb639ddc15c4.webp
    width: 320
    height: 296
  - file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-8c1769b61401.webp
    width: 437
    height: 404
  color: '#eaeaea'
- source: https://www.mattkeeter.com/projects/halfspace/flag.png
  original:
    file: 2026-09-30-halfspace-experimental-ide-for-solid-modeling-with-distance.image-1bfb444264c8.png
    width: 600
    height: 538
  color: '#363536'
---

## Introduction

[Halfspace](https://github.com/mkeeter/halfspace) is an experimental IDE for solid modeling with distance fields.

[![Picture of an IDE showing a model for a cabin](https://www.mattkeeter.com/projects/halfspace/hero.png)](https://www.mattkeeter.com/projects/halfspace/demo/?example=cabin.half)

[Try the demo](https://www.mattkeeter.com/projects/halfspace/demo/?example=cabin.half)

(The demo is best experienced on a computer; mobile Safari has some WebGPU issues, and pan / tilt / zoom interactions are not yet designed for multitouch)

Halfspace is a showcase app for the [Fidget kernel](https://www.mattkeeter.com/projects/fidget), which is used for rasterization and meshing. Within the GUI, images are rasterized in real(ish)-time; models can be exported as either images or triangle meshes:

![Diagram showing Halfspace and Fidget integration](https://www.mattkeeter.com/projects/halfspace/overview.png)

Since it's 2026, let me note at the outset that this is **not vibe-coded**. I've been working on it since [April 2025](https://github.com/mkeeter/halfspace/commit/317ddc435dcdef74bcfdb03a42c414182268005d#diff-b1a35a68f14e696205874893c07fd24fdb88882b47c23cc0e0c80a30c7d53759) and am writing the code using my human brain, for [various reasons](https://github.com/mkeeter/halfspace#llm-usage) (and Fidget dates back to [2022](https://github.com/mkeeter/fidget/commit/f44c0489af34b0d8052c9a11e3f593b731c4784f)).

Now, the rest of this writeup assumes *some* knowledge of implicit surfaces; please [see](https://www.mattkeeter.com/projects/libfive) [many](https://www.mattkeeter.com/projects/fidget) [previous](https://www.mattkeeter.com/projects/ao) [writeups](https://www.mattkeeter.com/projects/antimony) [for](https://www.mattkeeter.com/projects/kokopelli) [details](https://www.mattkeeter.com/projects/solver) for more background info (or just keep reading, you'll be fine).

### Why?

I've spent a bunch of time writing implicit kernels, slightly less time writing GUIs on those kernels, and even less time actually modeling with those tools.

In practice, I don't actually need to do much solid modeling in my daily life, so most of the stuff that I create is a demo or an example of how to use a particular kernel.

Still, I've noticed a particular tension when working with implicit surfaces. Working with low-level implicit surfaces is a bit like writing assembly: it's low-level, powerful, and annoying. If all you're given is `x`, `y`, `z` variables – and it's your responsibility to combine them into all the shapes of your dreams – that can be a painful experience.

When faced with the pain of writing assembly, most people build abstractions on top of it: high-level languages and libraries that compile down to a low-level representation. The equivalent here is a standard library of shapes and transformations: `sphere`, `box`, `translate`, `scale`, etc.

![Screenshot of a help box listing shapes](https://www.mattkeeter.com/projects/halfspace/shapes.png)

There's also a less common approach to the pain of assembly: *making assembly itself less painful to write*.

My favorite project along those lines is [Kartik Agaram's Mu](https://akkartik.name/akkartik-convivial-20200315.pdf), which wraps emulation, tracing, and time-travel debugging around a subset of x86 assembly language.

Halfspace takes both paths. It includes a (small but growing) standard library, but also makes it easy to build up models **incrementally**: a complex model can be split into smaller pieces, which can be parameterized and visualized individually.

[![Model of a Menger sponge](https://www.mattkeeter.com/projects/halfspace/sponge.png)](https://www.mattkeeter.com/projects/halfspace/demo/?example=sponge.half)

Given that justification, let's unpack the description a bit farther.

## Solid modeling

First off, "solid modeling" means that we're focusing on objects with a definitive inside and outside; you should be able to pick any point in space and say whether it's inside or outside the model.

This seems obvious, but there's plenty of modeling that doesn't care about that property: pull up any [video game model viewer](https://noclip.website/#mkwii/rainbow_course;ShareData=Au:Um=fYwiWaO1\)=rkL%5BgL) and you'll see plenty of infinitely-thin textured walls, built from a single fan of triangles. Since my background is in [CAD/CAM software](https://en.wikipedia.org/wiki/CAD/CAM) (with an emphasis on 3D printing), I want models that can be physically realized.

![Photograph of a 3D printed Menger sponge](https://www.mattkeeter.com/projects/halfspace/print.jpg)

There are a bunch of ways to do solid modeling. In most CAD software, a [boundary-representation](https://en.wikipedia.org/wiki/Boundary_representation) [geometry kernel](https://en.wikipedia.org/wiki/Geometric_modeling_kernel) is responsible for stitching a bunch of individual surfaces together into a solid body. This is a tremendously hard problem – for example, the intersection of two [NURBS](https://en.wikipedia.org/wiki/Non-uniform_rational_B-spline) surfaces may not have a closed-form solution!

Dating back to my Master's thesis, I've been working on geometry kernels based on implicit surfaces. These have the advantage that they can conceivably be written and fully understood by a single person or small team, so they're a good fit for personal-scale fabrication software.

This continues in Halfspace: it's a GUI wrapped around the [Fidget](https://www.mattkeeter.com/projects/fidget) geometry kernel. Models can be designed using some combination of pre-defined primitives and hand-written scripts, and exported as either images or triangle meshes.

## An IDE for distance fields

We could use the Fidget kernel *purely* at the constructive solid geometry (CSG) layer, building shapes (spheres, cubes, cylinders, etc) and combining them with logical operations (union, intersection, difference). Halfspace instead make the decision to put the underlying distance fields in the foreground.

Let me give you an example of why this matters. Here are two distance fields for a sawtooth wave, which have the same signs at every point in space, but different values:

![Sawtooth distance field with discontinuities](https://www.mattkeeter.com/projects/halfspace/bad_sawtooth.png) ![Sawtooth distance field with no discontinuities](https://www.mattkeeter.com/projects/halfspace/good_sawtooth.png)

In this visualization, the sign (which defines *inside* versus *outside*) is shown by color (blue versus orange), with the boundary of the shape shown in white (corresponding to a value of 0). The field **values** are shown by the fainter lines, which are spaced at regular intervals (like a topographic map).

Despite having identical signs everywhere, the first field is very poorly behaved. Look at the vertical edge of the sawtooth: there's a transition from inside (blue) to outside (orange) without a crossing through zero.

This is a C0 or ["jump" discontinuity](https://en.wikipedia.org/wiki/Classification_of_discontinuities#Jump_discontinuity), and it's bad news! Fidget uses automatic differentiation to compute normals, so the normals across this boundary don't point in the correct direction (compare the field lines between top and bottom images). In 3D, where we use normals for shading, this produces incorrect shading in the cabin's shingles:

![Rendering of a cabin with incorrectly shaded shingles](https://www.mattkeeter.com/projects/halfspace/cabin_bad.png)

(If this looks familiar, it's because it's extracted from [an earlier blog post](https://www.mattkeeter.com/blog/2025-04-12-continuity/))

Putting distance fields front-and-center makes it easy to diagnose these kind of issues. In fact, it suggests a further improvement on the sawtooth field: we can tweak the gradient so that it's 1 everywhere, instead of being bunched up on the diagonals. Here's a before / after comparison:

![Sawtooth distance field with no discontinuities](https://www.mattkeeter.com/projects/halfspace/good_sawtooth.png) ![Sawtooth distance field with no discontinuities and constant gradient](https://www.mattkeeter.com/projects/halfspace/better_sawtooth.png)

Having uniform gradients makes various algorithms better-behaved; Fidget doesn't require it for correctness, but it may (for example) improve mesh quality.

## Halfspace is experimental and cross-platform

Right now, it would be a **very bold** decision to use Halfspace in any load-bearing capacity. In the [Fidget writeup](https://www.mattkeeter.com/projects/fidget), here's one of the project goals:

> Finding the "right" APIs for implicit kernels, with the possibility of making substantial compatibility breaks

Halfspace is similar; it doesn't present APIs to end-users, but I'm flexing my software architecture skills by building a substantial cross-platform application, and I'm willing to aggresively iterate and break things as we go.

Speaking of cross-platform, I'm making my life harder by targeting both the web and native platforms. In the era of supply-chain attacks, being able to share a web link – instead of asking someone to compile and run your code – is great for onboarding and casual usage.

To that end, I've been collecting the "Halfspace stack": a set of libraries and patterns which let me ship a combined native + web application with a minimum of pain. Right now, here are the core pieces:

- Rust for the application (and all dependencies)
  - [Manually calling `wasm-bindgen` *et cetera* to build for the web](https://github.com/mkeeter/halfspace/blob/main/justfile)
- [`egui`](https://github.com/emilk/egui) for the GUI
  - [`egui_dock`](https://docs.rs/egui_dock/latest/egui_dock/) for the core window-and-tab abstraction
- [`wgpu`](https://wgpu.rs/) for both rendering the UI **and** GPU compute (!)
- [Rhai](https://rhai.rs) for scripting
- [Rayon](https://docs.rs/rayon/) and [`wasm-bindgen-rayon`](https://docs.rs/wasm-bindgen-rayon/latest/wasm_bindgen_rayon/), used for two purposes:
  - Speeding up parallel algorithms by distributing work over many workers; this is a "typical" usage of the library
  - A thread pool for short-lived background tasks; this is a more unusual usage, but we can't spawn threads on the web.
- ...and a long tail of other libraries and shenanigans
  - [`web-time`](https://docs.rs/web-time/latest/web_time/) for cross-platform time support
  - A [homebrew worker pool](https://github.com/mkeeter/halfspace/blob/3fe871ed94e8e46645af3ed363439e0eeea51a74/src/platform/web.rs#L500-L563) for off-thread (`async`) GPU rendering

This all deserves a dedicated writeup, and I have complaints about every single layer of the stack, but overall, it's incredible that everything Just Works™.

## Driving Fidget improvements

Another goal of Halfspace is to drive improvements in the Fidget kernel, by using it in a non-trivial application.

The biggest victory on this front has been ongoing work on [`fidget-wgpu`](https://docs.rs/fidget-wgpu/latest/fidget_wgpu/). This was motivated by performance on the web: the native build was pleasantly fast, but doing rasterization on the CPU was awfully slow ("non-interactive speeds") when running through a layer of WebAssembly.

After a lot of work on the Fidget side, both rasterization **and** post-processing (e.g. shading) can be run purely on the GPU, without any roundtrips to the CPU.

![WGPU pipeline](https://www.mattkeeter.com/projects/halfspace/wgpu.png)

This was both a performance and architectural win:

- We now have native rendering speed on both native and web targets
- Rendering logic is no longer spread between Fidget (on the CPU) and Halfspace (with a mix of CPU and GPU code):
  - Fidget implements the canonical rendering logic
  - Halfspace has thin shaders which draw an RGBA texture

(The 2D rendering pipeline is also now fully GPU-accelerated, although it has slightly fancier shaders in Halfspace, for *reasons*)

## Wrapping up

Halfspace is a thing that exists!

[![Picture of an IDE showing a Utopian Flag from the Terra Ignota series](https://www.mattkeeter.com/projects/halfspace/flag.png)](https://www.mattkeeter.com/projects/halfspace/demo/?example=utopian.half)

You should try it out, and should probably not use it for critical applications! If you encounter problems, please file an issue or open a discussion on Github.

Like most of my work, I plan to keep working on it until I have run out of things to learn from the project. Also like most of my work, it's [open-source](https://github.com/mkeeter/halfspace/) under [the MPLv2 license](https://github.com/mkeeter/halfspace/blob/main/LICENSE.txt).
