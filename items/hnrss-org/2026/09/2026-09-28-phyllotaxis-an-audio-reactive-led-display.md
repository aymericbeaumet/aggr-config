---
title: 'Phyllotaxis: An audio-reactive LED display'
link: https://jagi.studio/posts/phyllotaxis/
source: hnrss-org
published: 2026-09-28T16:18:57Z
updated: 2026-09-28T16:18:57Z
first_seen: 2026-09-29T07:30:54.171276122Z
authors:
- evakhoury
content: extracted
html: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.html
preview:
  file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.preview-5d51b14be306.webp
  width: 256
  height: 134
  color: '#351022'
images:
- source: https://jagi.studio/images/art/phyllotaxis_1_hu_7ca781e5e00bef34.jpeg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-f550ed95fd7a.jpg
    width: 1200
    height: 630
  color: '#0c020b'
- source: https://jagi.studio/images/art/phyllotaxis_1.jpeg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-42bd1e82ed42.jpg
    width: 800
    height: 801
  color: '#0c020a'
- source: https://jagi.studio/posts/phyllotaxis/basic_pattern_hu_fb66faeaafe5b16.webp
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-e7bd687236a0.webp
    width: 1600
    height: 1534
  variants:
  - file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-a2f9ec0ad841.webp
    width: 320
    height: 307
  - file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-daf63d162229.webp
    width: 640
    height: 614
  color: '#000000'
- source: https://jagi.studio/posts/phyllotaxis/tessellated_phyllo_hu_54b839433a7583d0.webp
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-835527f1719d.webp
    width: 1600
    height: 1492
  variants:
  - file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-28f44297b23d.webp
    width: 320
    height: 298
  color: '#020202'
- source: https://jagi.studio/posts/phyllotaxis/cq_model.jpeg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-495ca582b5b0.jpg
    width: 1395
    height: 1613
  color: '#a4a4a4'
- source: https://jagi.studio/posts/phyllotaxis/freecad_faceplate.jpeg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-55e089deb821.jpg
    width: 1224
    height: 1278
  color: '#3a3a3a'
- source: https://jagi.studio/posts/phyllotaxis/paper_installation_hu_e354091c0b52f54b.webp
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-6b212215d2d1.webp
    width: 1600
    height: 1661
  variants:
  - file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-6695a2562748.webp
    width: 320
    height: 332
  color: '#171718'
- source: https://jagi.studio/posts/phyllotaxis/breadboard_i2s.jpg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-c79e9218fc04.jpg
    width: 1600
    height: 823
  color: '#a8a6a6'
- source: https://jagi.studio/posts/phyllotaxis/bamboo_backplate.jpg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-a3245cf25459.jpg
    width: 1600
    height: 1472
  color: '#453838'
- source: https://jagi.studio/posts/phyllotaxis/enclosure.jpg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-c3b1e8645638.jpg
    width: 1588
    height: 1600
  color: '#070405'
- source: https://jagi.studio/posts/phyllotaxis/control_pcb.jpg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-b8c4dcf17049.jpg
    width: 681
    height: 1600
  color: '#544748'
- source: https://jagi.studio/posts/phyllotaxis/pcb_arrangement.jpg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-7bafa9e2a216.jpg
    width: 1600
    height: 1549
  color: '#d1a37a'
- source: https://jagi.studio/posts/phyllotaxis/rc_its_alive.jpg
  original:
    file: 2026-09-28-phyllotaxis-an-audio-reactive-led-display.image-95f316d2147f.jpg
    width: 1463
    height: 1600
  color: '#473b34'
---

![Phyllotaxis LED sculpture](https://jagi.studio/images/art/phyllotaxis_1.jpeg)

I’ve been fascinated by understanding patterns that appear in nature, through code. Communing with the inherent emergent patterns that exist in the universe.

The phyllotaxis is one such pattern, the expanding double-spiral shape that appears in the center of sunflowers, succulents, other plants. It’s surprisingly simple code:

```java
// Inner and outer are throw-away,
// just for spacing the cells that are used
float[][] points = new float[numCells + numOuter + numInner][2];
float outerRadius = width / 2;
final int total = numCells + numInner + numOuter;
for (int i = 0; i < total; i++)
{
	float f = i / float(total);
	float a = i * 1.6180339887;
	
	float distance = f * outerRadius;
	
	float x = -cos(a * TWO_PI) * distance;
	float y = sin(a * TWO_PI) * distance;
	
	points[i][0] = x;
	points[i][1] = y;
}
```

Take a number of linear points on a radial line of a circle, and rotate each one by an increasing multiple of the golden ratio.

You get a neat point cloud out of this that already looks very much like a sunflower. Voronoi tessellating it looks quite interesting and seed-pod-like:

![The shape emerges when plotting the points](https://jagi.studio/posts/phyllotaxis/basic_pattern_hu_fb66faeaafe5b16.webp)\
The shape emerges when plotting the points

![Voronoi tessellation result](https://jagi.studio/posts/phyllotaxis/tessellated_phyllo_hu_54b839433a7583d0.webp)\
Voronoi tessellation result

After experimenting with this for a while, I had the idea: what if I made it a physical object and put an RGB addressable LED in each cell? A kind of cellular LED matrix. I had built a rectangular neopixel matrix in high school, but I’d been waiting to find a more interesting shape for the next one.

## From digital to physical

I exported the cell edge data from my processing sketch, and imported it into a python sketch using the CadQuery library.

Wrote some code to build physical geometry out of the data:

```python
# Create the negative space of a single cell
# shrunk with a negative offset-2d
cell_shape = (cq.Workplane("XY")
	.polyline(vertices)
	.close()
	.offset2D(-wall_thickness/2, kind='intersection')
	.extrude(total_height)
	.translate((0, 0, base_height)))

# Cut it out of the total to create a shell with walls
total_shape = total_shape.cut(cell_shape)
# Cut the shape of the hole to fit the LED in the center of the cell
total_shape = total_shape.cut(
	led_hole.rotate((0, 0, 0), (0, 0, 1), math.degrees(angle))
			.translate((centerX, centerY, 0)))
```

This snippet, after constructing the total volume of all merged voronoi cells, subtracts the shrunk volume of each individual cell from the total, and then carves an LED-sized hole in each. Doing this for all cells yields the yellow shape below. I also split the geometry into four roughly equal quadrants at this point, each of which would fit on my 3D printer bed.

I exported the STEP files and did some post processing in FreeCAD, adding screw holes, building a thin version of it that acted as a ‘faceplate’ with paper under it. I cut and glued paper to the bottom of the thin version, and used the screw holes to screw it into the cell walls.

![The CadQuery models for each quadrant](https://jagi.studio/posts/phyllotaxis/cq_model.jpeg)\
The CadQuery models for each quadrant

![Faceplate model, transformed in FreeCAD from the CadQuery output](https://jagi.studio/posts/phyllotaxis/freecad_faceplate.jpeg)\
Faceplate model, transformed in FreeCAD from the CadQuery output

I 3D printed the shapes, and found some translucent paper that diffuses light well and looks organic and playful with light shining through it. I love the look of this mulberry paper, with such beautiful organic fibers.

![Installing paper on the board](https://jagi.studio/posts/phyllotaxis/paper_installation_hu_e354091c0b52f54b.webp)

Soldering 89 LEDs is kinda tedious. But I found my way into the flow state and it became relaxing. I hooked it up to a spare STM32 blackpill board I had lying around, and wrote a little neopixel driver using SPI. It was already cool with some basic test patterns.

I built a simple sketch framework, letting me write processing-style ‘shader’ code for the LEDs. Using a look-up table with the floating point position for every LED as if it was on the unit circle, I could easily write code like this, which feels like a fragment shader:

```c
void radial_spirals(LEDBuffer leds) {
	for (int i = 0; i < NUM_LEDS; i++) { // iterate every cell
		// Get LED position from LUT
		float x = led_positions[i][0];
		float y = led_positions[i][1];
		
		// Polar coordinates
		float r = sqrtf(x*x + y*y);
		float theta = atan2f(y, x);  // Angle in radians [-π, π]
		
		// Spiral pattern: combine angle and radius
		float spiral = sinf(theta * 2.0f + r * 5.0f - seconds * 2.0f);
		
		float brightness = (spiral + 1.0f) * 0.5f;
		uint8_t level = (uint8_t)(brightness * 255.0f);
		
		leds[i] = rgb(level, 0, 0);
	}
}
```

Pretty nifty.

I very quickly discovered this radial pattern that I enjoyed the most, a pulsating sine wave modulating brightness outward from the center. This ended up being what I used for the primary feature of the audio-reactive sketch I did later.

## Adding audio

Wouldn’t it be cool if it could react dynamically to the sounds in the room, rather than just displaying predefined patterns? I wanted it to feel alive and dynamic.

The STM32 board driving it had floating point support and ARM DSP instructions, so I could run the fourier transform and do audio analysis. I added a digital I2S mic, the INMP441, to my breadboard circuit driving the thing.

![Breadboard circuit with the I2S mic](https://jagi.studio/posts/phyllotaxis/breadboard_i2s.jpg)

Since it outputs a digital signal, it’s easy to wire up with no risk of analog noise. The ARM CMSIS library came in handy to process the audio signal and do some analysis. With some auto-gain control and multiband splitting to analyze energy at different frequencies, I had a good foundation to write some code that reacted to audio in dynamic ways.

A lot of tuning and a very chaotic sketch with a lot of global timers and messy state produced this audio-reactive program, which is still the basis of what runs on the board to this day. It came alive listening to Jon Hopkins’ Neon Pattern Drum.

## Tidying up

Just a few details left. It needs a sturdier surface to mount it to. I found a bamboo cutting board and routed it into a circle; the natural bamboo wood grain complements the organic paper so well.

![The bamboo backplate assembled, and the sculpture mounted on my wall](https://jagi.studio/posts/phyllotaxis/bamboo_backplate.jpg)

I had assembled the controller MCU and mic into a perfboard, and stuffed it into a 3D printed box with a guitar pedal switch on it. But the electronics were janky and prone to noise that would cause the LEDs to flicker when the box moved. I decided to learn how to build PCBs, and switched to a proper PCB mounting the blackpill board and mic and a button, and a proper spot for a logic-level shifter to get a good 5V signal out of the microcontroller. It worked well, and making PCBs was a bit easier than I expected, at least for this level of complexity.

![The initial enclosure with a very messy perfboard hidden inside](https://jagi.studio/posts/phyllotaxis/enclosure.jpg)\
The initial enclosure with a very messy perfboard hidden inside

![The blank PCB I designed, before I soldered all the components onto it](https://jagi.studio/posts/phyllotaxis/control_pcb.jpg)\
The blank PCB I designed, before I soldered all the components onto it

## Building the second version

I showed it off at a New Year’s party ringing in 2026, and people seemed to like it; some friends even told me that they wanted one. I got inspired to build a second version, which would hopefully be easier to assemble and would be more sturdy. Since I had just learnt PCB design, I decided to try and switch from a 3D printed plate to mount the LEDs on, to using PCBs as a backplate. I tweaked the shape a bit and added 5-fold symmetry, since PCBs are ordered in batches of 5; rather than four unique pieces that I’d 3D print, I’d order five of the same PCB, and assemble them into a ring.

![The second version of the project, five radially tiling PCBs](https://jagi.studio/posts/phyllotaxis/pcb_arrangement.jpg)

It turns out that hand soldering neopixel LEDs with a soldering iron is even more of a nightmare, as the pads for individual neopixels sit underneath them; they’re intended to be soldered using a method that heats up the PCB rather than an iron. The project went on hiatus for a bit.

I brought the project parts with me when I attended a programming retreat at [Recurse Center](https://recurse.com/) in the summer of 2026, and a friend I met there was much more handy with a hot air soldering station than I am. We managed to finish all the soldering, and I designed and 3D printed new cell walls to clip onto the PCBs and screw them together.

![Bringing up the PCB mounted lights after hours of soldering](https://jagi.studio/posts/phyllotaxis/rc_its_alive.jpg)

After finishing the new version during my Recurse batch, I decided to leave it at Recurse Center as an installation. I switched to an ESP32, so it can be on the wifi network there, and rewrote the firmware in Rust. The controller serves a local website where RC attendees can upload sketches and games to it using a [tiny webassembly backend](https://github.com/jagnat/esp32_phyllotaxis) and [minimal API](https://github.com/jagnat/phyllotaxis-sketch-sdk).

## What’s next?

I’m still not fully satisfied with this project.

Enough people have expressed enjoyment over it that I’d love to be able to make more of them, at the very least to gift to friends if not to sell more broadly. But the process has not been ideal; soldering has been tedious both times for different reasons. The process of attaching the paper is tricky, and the paper itself is fragile and difficult to repair if it breaks. I have some ideas for improvements that merit experimenting with: different types of LEDs which are easier to hand solder, using PCB assembly, testing out a layer of clear acrylic to protect the paper with, attaching the paper face plates with magnets.. So many things to test out in the future.

For now, the project deserves a break, but when the time is right it’ll be onto version 3!
