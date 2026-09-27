---
title: Reading Data off an Apple ProFile Hard Drive with an Arduino
link: https://nicholas.carlini.com/writing/2023/reading-an-apple-profile-hard-drive.html
source: nicholas-carlini-com
published: 2023-12-03T00:00:00Z
updated: 2023-12-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: So let's suppose you had a 1980s Apple ProFile Hard Drive, and you wanted to recover the data.
content: extracted
html: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.html
preview:
  file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.preview-8a0301037a87.webp
  width: 256
  height: 256
  color: '#454545'
images:
- source: https://nicholas.carlini.com/writing/2023/lisa-data.jpg
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-1c2aac77485d.jpg
    width: 2048
    height: 2048
  color: '#030303'
- source: https://nicholas.carlini.com/writing/2023/profile-protocol.png
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-40d710f3604f.png
    width: 1486
    height: 814
  color: '#fdfdfd'
- source: https://nicholas.carlini.com/writing/2023/actual-header.png
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-8a5d432cc907.png
    width: 6000
    height: 1800
  variants:
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-736320b3d93f.webp
    width: 320
    height: 96
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-290160c913f4.webp
    width: 640
    height: 192
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-84d28e731cdd.webp
    width: 960
    height: 288
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-156ecf070341.webp
    width: 1280
    height: 384
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-d093ede9f36b.webp
    width: 1600
    height: 480
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-06f163d12b72.webp
    width: 6000
    height: 1800
  color: '#fcfcfc'
- source: https://nicholas.carlini.com/writing/2023/actual-writeloc.png
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-5ae272761792.png
    width: 6000
    height: 1800
  variants:
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-75d26c6ff03b.webp
    width: 320
    height: 96
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-d71ddb816ba9.webp
    width: 640
    height: 192
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-7f8d5ad9ae5b.webp
    width: 960
    height: 288
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-dd8e9638b49d.webp
    width: 1280
    height: 384
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-55bcdacde429.webp
    width: 1600
    height: 480
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-8360a9e7f826.webp
    width: 6000
    height: 1800
  color: '#fcfcfc'
- source: https://nicholas.carlini.com/writing/2023/actual-finalize.png
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-921070f8a752.png
    width: 6000
    height: 1800
  variants:
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-eeea3e31b36e.webp
    width: 320
    height: 96
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-52d646eda1a8.webp
    width: 640
    height: 192
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-8cd57e5dd749.webp
    width: 960
    height: 288
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-e35c6f7f734a.webp
    width: 1280
    height: 384
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-0e81d11b68f5.webp
    width: 1600
    height: 480
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-7cb02e00bf07.webp
    width: 6000
    height: 1800
  color: '#fcfcfc'
- source: https://nicholas.carlini.com/writing/2023/actual-data.png
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-baa7933f64f1.png
    width: 6000
    height: 1800
  variants:
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-449ca0c5b504.webp
    width: 320
    height: 96
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-b1dc466dca71.webp
    width: 640
    height: 192
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-0d4874e7162a.webp
    width: 960
    height: 288
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-1affc4cc6cf5.webp
    width: 1280
    height: 384
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-bdbd7d898103.webp
    width: 1600
    height: 480
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-1d529f70dad0.webp
    width: 6000
    height: 1800
  color: '#fcfdfc'
- source: https://nicholas.carlini.com/writing/2023/actual-readwrite.png
  original:
    file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-33eb76c3f41d.png
    width: 6000
    height: 1800
  variants:
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-b7900f50fd01.webp
    width: 320
    height: 96
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-f3f10e0bb9b7.webp
    width: 640
    height: 192
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-6b5e052d3058.webp
    width: 960
    height: 288
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-faff011b10ed.webp
    width: 1280
    height: 384
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-a4e016a5173a.webp
    width: 1600
    height: 480
  - file: 2023-12-03-reading-data-off-an-apple-profile-hard-drive-with-an-arduino.image-8dc11d8aa213.webp
    width: 6000
    height: 1800
  color: '#fbfcfb'
---

So let's suppose you had a 1980s [Apple ProFile Hard Drive](https://en.wikipedia.org/wiki/Apple_ProFile), and you wanted to recover the data.

But the drive is older than you are, and it doesn't even power on. Oh, and the [Lisa](https://en.wikipedia.org/wiki/Apple_Lisa) it was connected to is completely corroded to death. So you can't use the Lisa. (yet.)

But you *do* have an Arduino, which runs 3x faster than the original Lisa did (for ~5000x cheaper too!), so you have that going for you...

What do you do? Well, apparently this.

## What am I even talking about

The Apple Profile Hard Drive was an external hard drive designed for use with the Apple III and Apple Lisa. It's an early 1980s drive, with a capacity of 5MB and was initially sold for a price of $3,500---just under ~1 dollar per kilobyte of storage. (For comparison: today you can easily get 50 gigabytes---50 million times more---for the same dollar.)

The ProFile is actually more than just a hard drive. In fact, it's two separate devices bundled together:

- A 5 MB spinning hard drive. The hard drive has a parallel port that receives commands and sends data in reply. (What kind of port? Honestly, no idea. I never needed to find out so I didn't bother to look. And I promised myself I'd spend under 2 hours writing this all up so I will not be looking it up now either.)
- A controller board with a Z8 microprocessor and 2KB of RAM. This controller acts as an interface between the hard drive and the Lisa, and is what converts the Lisa's special protocol into commands the hard drive understands.

## The ProFile Read Protocol

The controller connects to the Lisa through an Apple proprietary protocol over a standard [DB-25](https://en.wikipedia.org/wiki/D-subminiature) cable. The protocol is fairly simple, and is described in several manuals I managed to find online. Here's a logic timing diagram of how to read from the drive:

![](https://nicholas.carlini.com/writing/2023/profile-protocol.png)

- **CMD** is the command pin. When the Lisa wants to send a command to the ProFile, it pulls this pin low.
- **BUSY** is the busy pin. When the ProFile is busy, it pulls this pin low, and when it's free, it pulls it high. This pin is directly connected to the LED on the front of the ProFile to indicate when the drive is busy.
- **RRW** is the read/write pin. When the Lisa wants to read data from the data bus, it pulls this pin high. When it wants to write data to the data bus, it pulls this pin low.
- **The data pins** are eight bidirectional pins that carry the data either from the Lisa to the ProFile or from the ProFile to the Lisa.

That is, in words the protocol works like this:

1.  The Lisa raises CMD, in order to say “I want to send you a command”.
2.  The ProFile puts 0x01 on the data bus, indicating that it's ready to receive data, and then raises BUSY. The Lisa waits for BUSY to go high and then...
3.  The Lisa first lowers RRW to indicate that it wants to write data to write to the data bus. Then, the Lisa puts 0x55 on the data bus by pulling the appropriate pins high or low, and then lowers CMD. This tells the ProFile that the Lisa acknowledges that the ProFile is ready to receive data.
4.  The ProFile lowers BUSY, indicating that it's ready to receive data.
5.  The Lisa writes the 6 bytes of data to the bus. I'll say what these are later, and exactly how the writes happen.
6.  The Lisa then raises CMD and RRW, saying that it's done sending the command.
7.  The ProFile then puts 0x02 on the data bus, which acknowledges that it's been told to read data off the disk, and raises BUSY.
8.  The Lisa acknowledges that it's ready to receive data by putting 0x55 on the data bus again, and then lowering CMD.
9.  Finally, the ProFile actually goes and reads the data off of the disk. This takes a while (tens to hundreds of milliseconds). Once data is available, the ProFile lowers BUSY indicating that it has the data ready to transfer.
10. The Lisa then reads 4 status bytes off of the data bus, followed by a 532 byte block of data off the hard drive.

In order to actually indicate that the Lisa has read or written off of the data bus, there's a fourth pin, **PSTRB**, which the Lisa pulses high for a few microseconds to say either “I've read the data, you can put new data on the bus” or “I've written the data, you can take it off the bus now”. The ProFile can read or write data to the bus at a rate of 1MHz.

## Getting the ProFile to power on

Now let me comment briefly on what I needed to do to get the hard drive turned on. I'm not going to linger here much because this isn't something I'm good at and so don't actually feel qualified to talk about it much. Which also means, a word of caution:

THIS IS NOT AN INSTRUCTION MANUAL. I DO NOT KNOW WHAT I AM DOING. IF YOU FOLLOW THESE INSTRUCTIONS YOU WILL PROBABLY BREAK YOUR PROFILE. AND MAYBE GET ELECTROCUTED TOO.

When applying power to the ProFile, I put a volt meter on what's supposed to be the 5 volt rail, I wasn't getting any voltage within a few seconds. Given that this drive is 40 years old, I figured that the capacitors in the power supply were probably dead. Capacitors are known in particular to fail catastrophically, and I didn't want to cause any serious damage, so I decided to just replace them all. This involved opening up the power supply which is kind of scary because it connects to the mains voltage. But I was pretty careful. Buying the replacements wasn't so expensive (~$20) and I didn't do too much damage to the board resoldering them.

After this, the power supply actually gave the correct voltage readings on the 5V and 12V rails. Which was a good sign. But when I plugged it in to the ProFile the drive didn't spin.

So here's where I did another thing that you probably shouldn't do. Either the drive motor was dead (in which case I'd have to replace it), or maybe it wasn't dead but just was a little bit stuck. In either case, I'd need to open up the drive and try to see what's going on. So I got it open, and then just manually spun the motor around a few times to hopefully losen any stuck bearings.

After reassembling the drive, I plugged it in again, and it still didn't work, but I did hear the motor trying to spin. So I opened it back up, spun it around a few more times, and this time before turning the drive on, put it under a heat lamp for a few minutes. (Not that it's *cold* in california, but the drive was sitting at around 50 farenheit / 10 celsius, and figured maybe this would help warm up any stuck bearings.)

And then it worked! I don't know what actually ended up fixing it, but the drive now spins. And even better: when the ProFile starts up, it runs through a self test of a bunch of sectors on the drive. And mine passed! (I think. The red light on the front comes on, which I think means it passed. Again: I don't really know what I'm doing here.)

Now that the ProFile is powered on, let's try to read some data off of it. We'll walk our way through the protocol step by step writing the code to directly follow the protocol outlined above.

**Steps 1-4:** as described abvve, we should raise CMD, and then the ProFile raises BUSY, we read the 0x01 off the bus (and pulse RSTB), we lower RRW, write 0x55, lower CMD, and then the ProFile lowers BUSY. Except there's a catch... the ProFile decided that, for some reason, the pins that it uses for PSTRB, CMD, and BSY are inverted. So you should flip everything that I just said upside down (except for RRW which stays the same.)

// step 1: lower cmd

digitalWrite(CMD, 0);

setup\_read\_bus();

delay(A\_SHORT\_WHILE);

// step 2: wait for BSY to go low

while (digitalRead(BSY));

delay(A\_SHORT\_WHILE);

// step 2.5: read the 0x01 on the bus

int is\_ok = read\_byte();

// assert is\_ok is true

// step 3: write a 0x55 on the bus

setup\_write\_bus();

write\_bus(0x55);

delay(A\_SHORT\_WHILE);

// step 3.5: tell the profile we wrote it

digitalWrite(CMD, 1);

delay(A\_SHORT\_WHILE);

// step 4: wait for the drive to say it's read it

while (!digitalRead(BSY));

Here I've made use of a few helper functions to read and write from the bus. I'll show just the read function here, if you want to see the write too, you can find the whole implementation on [my github project](https://github.com/carlini/arduino-apple-profile-reader).

int read\_byte () {

  int o = 0;

  digitalWrite(PSTRB, 0);

  for ( int i = 0; i < 8; i++)  {

    o |= digitalRead(i+2)<\<i;

  }

  int parity = digitalRead(PARITY);

  delayMicroseconds(A\_VERY\_SHORT\_WHILE);

  digitalWrite(PSTRB, 1);

  delayMicroseconds(A\_VERY\_SHORT\_WHILE);

  return (parity << 8) + o;

}

And if you run this, then you actually get to see data on the on the parallel port that makes it looks like it's understanding what we're sending! The first byte we read is indeed 0x01 and that's a very good sign.

Let me show you what's actually happening over the port. (I don't have a digital logic analyzer, but I did buy an oscilloscope for this project. So I can show you four channels of data. I've picked the four control channels, you'll have to trust me that the data channels are correct.)

Here you can see that even though the chip is a 5 volt logic chip, BSY actually only being reported at slightly over 3 volts. I don't know if this is something wrong with my Lisa, expected behavior because BSY is tied to the LED on the front panel, or something else. (Maybe there's an internal 3.3v logic somewhere? Who knows.) I also found it interesting to actaully see the processing delay between setting CMD low and the ProFile responding with BSY, and then high to high again.

![](https://nicholas.carlini.com/writing/2023/actual-header.png)

**Step 5-6:** Now we need to run the next steps, and tell the ProFile which sector on the drive we want to read. We do that with this code here

// step 5: write command bytes

write\_cmd(0x00); // read

write\_cmd(0x00); // location 00

write\_cmd(sector1); // location high

write\_cmd(sector2); // location low

write\_cmd(0x64); // retry 100

write\_cmd(0x14); // no idea but set it to 0x14

delay(A\_SHORT\_WHILE);

// step 6: say that we're done

digitalWrite(PSTRB, 1);

delay(A\_SHORT\_WHILE);

setup\_read\_bus();

delay(A\_SHORT\_WHILE);

digitalWrite(CMD, 0);

Where the implementation of write\_cmd is given here:

void write\_bus ( int byte) {

  int xorb = 1;

  for ( int i = 0; i < 8; i++) {

    digitalWrite(i+2, (byte>>i)&1);

    xorb ^= (byte>>i)&1;

  }

  digitalWrite(PARITY, xorb);

}

void write\_cmd ( int byte) {

  digitalWrite(PSTRB, 1);

  delayMicroseconds(A\_SHORT\_WHILE);

  write\_bus(byte);

  delayMicroseconds(A\_SHORT\_WHILE);

  digitalWrite(PSTRB, 0);

  delay(A\_SHORT\_WHILE);

}

Again we can see the diagram of this below. Sorry that I can't show you the data bits being written. Just be happy you get anything at all.

![](https://nicholas.carlini.com/writing/2023/actual-writeloc.png)

**Step 7-8:** Finally, we do a lot of cleanup to make sure everything went through correctly.

// step 7: wait for read

while (digitalRead(BSY));

// step 7.5: and read the byte on the bus

int read\_ack = read\_byte();

// assert that read\_ack is 2

// step 8: write a 0x55 on the bus

setup\_write\_bus();

write\_bus(0x55);

// step 8.5: tell the profile we wrote it

digitalWrite(CMD, 1);

I found it rather interesting that BSY starts to have a large amount of noise on the line right before it goes low. Maybe it's doing something internally that's drawing power or something? No idea. I'm pretty sure this isn't just noise on the oscilloscope because I can replicate the effect consistently.

![](https://nicholas.carlini.com/writing/2023/actual-finalize.png)

**Step 9:** And then we wait ... for quite a long time, until the disk seeks and we reach the data we asked for.

![](https://nicholas.carlini.com/writing/2023/actual-data.png)

**Step 10:** Finally we read the data off the bus and dump it to the serial port of the Arduino.

int log \[4+256+256+20\];

for ( int i = 0; i < 2\*(4 + 256 + 256 + 20);) {

  // read each of the bytes on the bus

  int n = read\_byte();

  log\[i++\] = (n & 0xF) + 0x40;

  log\[i++\] = (n >> 4) + 0x40;

 }

// and when we're done dump it to the serial port

Serial.print( "<");

Serial.write(log, 2\*(256+256+4+20));

Serial.println( ">");

In the plot below, I've moved the oscilloscope probe to probe just PSTRB (which is the pin for the arduino to ask the ProFile for more data) and two of the data pins. The other 6 pins behave similarly but then the plot gets messy. (And also my oscilloscope only has 4 channels.)

You can see here how the response bits aren't as perfectly clean as you might think, where sometimes they overshoot by a pretty good amount. Also, I see the same slightly-over-3v response as from the BSY pin, so maybe it's actually expected that this should happen? Or maybe there's some 3.3v logic inside this thing after all? I don't know, but it works!

![](https://nicholas.carlini.com/writing/2023/actual-readwrite.png)

Could I read the data faster if I really wanted to put in the work? Yeah, probably. The ProFile is capable of feeding these 532 bytes at 1MHz and I'm only reading it out at like one twientieth of that. But this was good enough for me to read the entire contents of my ProFile off this 40 year old disk.

I've made my code available on GitHub at [this repository](https://github.com/carlini/arduino-apple-profile-reader) if you want to use it for yourself. I'm pretty sure it won't destroy your hard drive---it didn't destroy mine---but I'm not making any promises.

## It worked!

After running the above code for every sector on the disk, I got a full 5MB disk image, and it looks like it was a success! I'll have to do more work (later...) to figure out how to actually mount this disk image on a modern computer, but if I run strings over the image I can read a few nontrivial pieces of information, e.g., ":ESCAPE cannot be used to exit this menu; use Quit instead.", "!No device drivers have been read.", "%You must use the 'Read' option first.".

Here's an image representing the entire disk, with one pixel for each byte, each page represented as one small grid. It looks like there's a bunch of code there (that's what I'm guessing the random looking data is) along with some things that have patterns (encoded images?) and then a bunch of empty blocks.

![](https://nicholas.carlini.com/writing/2023/lisa-data.jpg)

As for what any of this data actually means, that's a problem for another day. (You'll notice I never said I actually needed any of this data. That's because I don't, and I was told this drive was probably factory fresh. So this entire project really didn't have any point except for the fun of it.)
