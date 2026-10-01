---
title: Various Projects Find Hidden SDR Capabilities in ESP32 Microcontrollers
link: https://www.rtl-sdr.com/various-projects-independently-find-hidden-sdr-capabilities-in-esp32-microcontrollers/
source: hnrss-org
published: 2026-10-01T15:07:42Z
updated: 2026-10-01T15:07:42Z
first_seen: 2026-10-01T23:33:46.642706577Z
authors:
- nkw
labels:
- esp32
- news
- other
content: extracted
html: 2026-10-01-various-projects-find-hidden-sdr-capabilities-in-esp32.html
preview:
  file: 2026-10-01-various-projects-find-hidden-sdr-capabilities-in-esp32.preview-4b1c23673f81.webp
  width: 256
  height: 160
  alt: eSpDR ESP32 80 MHz output to SDR++
  color: '#131548'
images:
- source: https://www.rtl-sdr.com/wp-content/uploads/2026/10/web-receiver-fft.png
  original:
    file: 2026-10-01-various-projects-find-hidden-sdr-capabilities-in-esp32.image-bd5df168e58d.png
    width: 1920
    height: 1200
  color: '#090d11'
---

Back in 2025, [we posted](https://www.rtl-sdr.com/espargos-an-esp32-phased-array-for-seeing-wifi/) about ESPARGOS, a phased array of many patch antennas, each connected to an ESP32 WiFi microcontroller. ESPARGOS could be used to determine the direction of arrival for WiFi signals and create a live augmented reality heatmap.

Recently, the ESPARGOS team has [made an exciting discovery](https://espargos.net/espsdr/). They found that several ESP32 chips have an undocumented feature that lets the firmware bypass the fixed WiFi and Bluetooth functionality and instead capture raw IQ baseband samples. As a result, several ESP32 models can now be used as an internal SDR covering 2.2–2.7 GHz, plus 4.8–6.0 GHz on the ESP32-C5, with up to 80 MS/s sample rate and roughly 13–54 MHz of analog bandwidth, depending on the chip.

However, for use as a general-purpose PC-connected SDR, the output bandwidth is insufficient, so only snapshots of data can be exported to a PC. This means that the SDR will only work as a spectrum analyzer, and demodulating or decoding continuous radio data is not possible with just an ESP32. The exception is the new ESP32-S31, which can stream continuously at up to 16 MS/s over its Gigabit Ethernet interface, with a SoapySDR driver for GNU Radio and gqrx coming soon. If you want to try it yourself, the ESP-WebSDR page lets you flash the firmware to most ESP32 dev boards directly from your browser and view a live spectrum and waterfall.

Furthermore, ESPARGOS notes that phase-coherent IQ sample capture is now possible with their hardware. This means ESPARGOS is no longer limited to WiFi and Bluetooth signals; it can now perform direction finding on any arbitrary signal in the 2.4 GHz band. The team also says phase-coherent transmissions would be possible, but they aren't implementing them right now because they could be misused.

It also seems that, independently, at least two other projects discovered the same or similar features around the same time, but to solve different problems.

Over on Reddit, [user /u/h0m3us3r discovered the same feature](https://www.reddit.com/r/esp32/comments/1wq37xz/got_raw_iq_streaming_out_of_an_esp32s3_at_80_mss/) and uploaded it to GitHub on Sept 26, and recorded a video showing an ESP32-S3 working as an SDR with an FPGA used as a USB3 front end. Unlike ESPARGOS, h0m3us3r's project streams the raw IQ data to a PC continuously via the FPGA rather than in snapshots, so full demodulation/decoding on PC should be possible. However, the current prototype uses the FPGA to clock the ESP32, which results in poor phase noise. So if the clocking issues can be resolved, an ESP32 combined with an FPGA could make a standard general-purpose SDR, like the RTL-SDR, but with a 2.2–2.8 GHz frequency range and up to 80 MHz of bandwidth.

Another project that seems to be using a somewhat similar finding is [C5VRX](https://github.com/Twotoz/C5VRX), which was first uploaded to GitHub on August 13. C5VRX uses an ESP32-C5 as a 5.8 GHz real-time FPV video receiver. Although this project appears to use a different mechanism, the result is similar: it uses an undocumented IQ data stream on the ESP32 to sample FPV signals and demodulate them onboard, outputting analog composite video through a simple resistor DAC. The project is still a work in progress and doesn't yet work reliably at range.

[![eSpDR ESP32 80 MHz output to SDR++](https://www.rtl-sdr.com/wp-content/uploads/2026/10/web-receiver-fft.png)](https://www.rtl-sdr.com/wp-content/uploads/2026/10/web-receiver-fft.png)\
eSpDR ESP32 80 MHz output to SDR++
