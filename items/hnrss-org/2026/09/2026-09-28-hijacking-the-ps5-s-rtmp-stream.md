---
title: Hijacking the PS5's RTMP stream
link: https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/
source: hnrss-org
published: 2026-09-28T15:35:23Z
updated: 2026-09-28T15:35:23Z
first_seen: 2026-09-28T21:24:47.177888442Z
authors:
- ibobev
content: extracted
html: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.html
preview:
  file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.preview-657ca7240ada.webp
  width: 256
  height: 128
  color: '#e5e3e0'
images:
- source: https://yashgarg.dev/_astro/cover.CH2lYHLC_OFBUo.webp
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-534393b90482.webp
    width: 1200
    height: 600
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-074a6a9f5b92.webp
    width: 320
    height: 160
  color: '#fbf9f5'
- source: https://yashgarg.dev/_astro/diagram-1-light.DQtKQAgY.svg
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-7f45232796f7.png
    width: 362
    height: 181
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-0e406dd48df0.webp
    width: 362
    height: 181
  color: '#eee9e6'
- source: https://yashgarg.dev/_astro/diagram-1-dark.DHxfKYSh.svg
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-3a11a9ec833f.png
    width: 362
    height: 181
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-b953da7db389.webp
    width: 362
    height: 181
  color: '#2b231d'
- source: https://yashgarg.dev/_astro/diagram-2-light.DTxc9dxk.svg
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-44e71d7463e2.png
    width: 564
    height: 181
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-084f213b1e63.webp
    width: 320
    height: 103
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-79bb17a51c5c.webp
    width: 564
    height: 181
  color: '#eee9e6'
- source: https://yashgarg.dev/_astro/diagram-2-dark.DPzf_7uv.svg
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-59f8e0ff2bf7.png
    width: 564
    height: 181
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-7d9ae91b4c5f.webp
    width: 564
    height: 181
  color: '#2b221c'
- source: https://yashgarg.dev/_astro/diagram-3-light.Dd-yXgWU.svg
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-fa9f6374b8a0.png
    width: 637
    height: 70
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-cc5b1d9b81ce.webp
    width: 637
    height: 70
  color: '#eee9e6'
- source: https://yashgarg.dev/_astro/diagram-3-dark.lPIBSoSB.svg
  original:
    file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-aa25efb64872.png
    width: 637
    height: 70
  variants:
  - file: 2026-09-28-hijacking-the-ps5-s-rtmp-stream.image-e0a9dfa9e557.webp
    width: 637
    height: 70
  color: '#2b231c'
---

The long way around to screen sharing.

Contents

- [1.The Problem](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#the-problem)
- [2.Remote Play](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#remote-play)
- [3.How PS5 Streaming Works](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#how-ps5-streaming-works)
- [4.Finding the Right Hostname](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#finding-the-right-hostname)
- [5.DNS Trick](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#dns-trick)
- [6.Receiving the Stream](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#receiving-the-stream)
- [7.Watching It](https://yashgarg.dev/posts/hijacking-ps5-rtmp-stream/#watching-it)

Sony has progressively locked down what you can do with the PS5’s hardware. Streaming is a good example: the console gives you a nice, convenient **“Broadcast”** button, but the moment you want to do anything outside the handful of services Sony supports, it gets annoying very fast.

Third-party Bluetooth devices are the same story! Sony locks the wireless stack to their own peripherals, so your headphones or controllers from other brands simply won’t pair :/

## The Problem

I often stream games with friends on [Discord](https://discord.com) who watch me play, but the PS5 doesn’t support screen sharing to Discord. The obvious fix is a capture card — plug the HDMI output into a [capture card](https://www.elgato.com/us/en/explorer/products/capture/what-is-a-capture-card/), feed it into [OBS](https://obsproject.com/) on your Mac, stream from there. But decent ones aren’t cheap, and I didn’t want to spend upwards of $100 just for this.

## Remote Play

[Remote Play](https://www.playstation.com/en-in/remote-play/) somewhat worked for me. I could connect the PS5 to my MacBook, share the Mac’s screen to Discord and play from there.

The problem is that you need to connect everything to the Remote Play device: controller, earphones, etc. I also occasionally ran into input lag, and the stream quality is entirely controlled by the PS5. You can’t really configure anything.

I didn’t want to change my physical setup every time I wanted to stream.

## How PS5 Streaming Works

The PS5 supports streaming to [YouTube](https://youtube.com) and [Twitch](https://twitch.tv) by default if you’re signed into those accounts. The protocol used for this is [RTMP](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol), or Real-Time Messaging Protocol, which is commonly used for live audio/video streaming.

So when you start a broadcast, the PS5 roughly does this:

![](https://yashgarg.dev/_astro/diagram-1-light.DQtKQAgY.svg) ![](https://yashgarg.dev/_astro/diagram-1-dark.DHxfKYSh.svg)

What if we could make our own device act as Twitch and receive that RTMP stream instead?

![](https://yashgarg.dev/_astro/diagram-2-light.DTxc9dxk.svg) ![](https://yashgarg.dev/_astro/diagram-2-dark.DPzf_7uv.svg)

That’s the idea. The PS5 doesn’t hardcode Twitch’s IP, it looks it up via DNS every time. If we control what DNS returns, we control where the stream goes.

## Finding the Right Hostname

The obvious first attempt was to spoof `ingest.twitch.tv` directly. That’s the hostname the PS5 resolves when you hit broadcast, so pointing it at the Mac should work, right?

Not quite. `ingest.twitch.tv:443` is actually a **discovery endpoint**, not the RTMP server itself. PS5 makes an HTTPS call to it asking “which regional ingest server should I use?” and Twitch responds with something like `ap-southeast-1.prod.fi.contribute.live-video.net`. Then the PS5 pushes the actual stream there.

Spoofing that hostname ran into a different problem: the actual Twitch ingest uses **RTMPS** (RTMP over TLS on port 443), and PS5 validates the certificate against trusted [CAs](https://en.wikipedia.org/wiki/Certificate_authority). A self-signed cert doesn’t work, and there’s no way to install custom CAs on a PS5.

I then tried YouTube as a workaround. Its RTMP ingest uses plain RTMP on port 1935, so there was no TLS certificate to deal with. The PS5 happily sent the stream to my Mac, so I knew the basic approach worked.

The problem was that PS5 periodically checks YouTube’s API to make sure the stream is actually live. Since YouTube never received the stream, that check failed and it stopped broadcasting after about 60 seconds.

That meant I needed to find a Twitch endpoint that used plain RTMP. The answer came from watching DNS logs while broadcasting:

```sh
sudo tail -f /tmp/dnsmasq.log
# Sep 22 23:20:28 dnsmasq: query[A] ingest.global-contribute.live-video.net from 192.168.8.171
# Sep 22 23:20:28 dnsmasq: reply aps30.contribute.live-video.net is 35.55.13.0
```

The PS5 was resolving `ingest.global-contribute.live-video.net`, which chains down to `aps30.contribute.live-video.net`. That’s the real RTMP server. Spoofing `contribute.live-video.net` covers all subdomains and redirects the actual stream to the Mac without any certificate issues.

## DNS Trick

The setup has two main parts: [`dnsmasq`](https://dnsmasq.org) and [`nginx-rtmp`](https://github.com/arut/nginx-rtmp-module). I built a small macOS menu bar app that bundles both and manages them.

I run `dnsmasq` on my Mac and configure it to resolve Twitch’s ingest domains to my Mac’s LAN address:

```sh
server=1.1.1.1
server=8.8.8.8

# Redirect Twitch ingest traffic to the Mac
address=/contribute.live-video.net/192.168.8.175
address=/ingest.global-contribute.live-video.net/192.168.8.175
address=/live.twitch.tv/192.168.8.175
address=/live-sin.twitch.tv/192.168.8.175
address=/live-nrt.twitch.tv/192.168.8.175
address=/live-syd.twitch.tv/192.168.8.175
address=/live-fra.twitch.tv/192.168.8.175
address=/live-ams.twitch.tv/192.168.8.175
address=/live-lhr.twitch.tv/192.168.8.175
address=/live-jfk.twitch.tv/192.168.8.175
address=/live-lax.twitch.tv/192.168.8.175
address=/live-sea.twitch.tv/192.168.8.175

log-queries
log-facility=/tmp/dnsmasq.log

no-hosts
listen-address=0.0.0.0
```

`192.168.8.175` is my Mac’s IP. When the PS5 asks DNS for one of these Twitch endpoints, `dnsmasq` returns my Mac’s IP instead. The PS5 connects to my Mac thinking it’s Twitch.

The last piece is pointing the PS5 at this DNS server. I have a [GL.iNet router](https://www.gl-inet.com/) running [OpenWRT](https://openwrt.org/), so I configured it to hand my Mac’s IP as the DNS server specifically for the PS5’s [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) lease.

```sh
# SSH into the router and run:
uci add_list dhcp.lan.dhcp_option="tag:PS5,6,192.168.8.175"
uci commit dhcp
/etc/init.d/dnsmasq restart
```

The `tag:PS5` part works because the PS5’s static lease already has that tag set in `/etc/config/dhcp`. Option `6` is the DHCP option for DNS server. The PS5 picks this up on its next DHCP renewal, no manual DNS configuration is required on the console!

## Receiving the Stream

For that, I’m using `nginx-rtmp`:

```sh
worker_processes 1;

error_log /tmp/nginx-error.log warn;
pid /tmp/nginx.pid;

events {
    worker_connections 512;
}

rtmp {
    server {
        listen 1935;
        chunk_size 4096;
        application ps5 {
            live on;
            record off;
            sync 10ms;
            # Notify our app when a stream starts
            on_publish http://127.0.0.1:9988/on_publish;
        }
    }
}

http {
    server {
        listen 8080;
        location /stat {
            rtmp_stat all;
        }
    }
}
```

The `on_publish` callback is how the menu bar app detects when the PS5 starts broadcasting. nginx fires a POST to `localhost:9988` with the stream name, and the app surfaces the full RTMP URL ready to copy.

At this point, the PS5 is pushing its stream (1080p60, H.264, AAC stereo) directly to my Mac instead of Twitch.

![](https://yashgarg.dev/_astro/diagram-3-light.Dd-yXgWU.svg) ![](https://yashgarg.dev/_astro/diagram-3-dark.lPIBSoSB.svg)

From here I can pull the stream into anything: OBS to re-stream it, record it locally, or just play it directly.

## Watching It

Instead of going through OBS, I used [`mpv`](https://mpv.io/) to pull the stream and shared the window to Discord. The low-latency profile keeps the delay less than a second:

```sh
mpv --profile=low-latency --audio-buffer=0.3 rtmp://127.0.0.1/ps5/stream-key
```

This has been quite reliable surprisingly. I’ve been using it for a few weeks now and haven’t had any issues. You can find the complete source code [here](https://github.com/yash-garg/PS5Streamer).

*Until next time! 👋*
