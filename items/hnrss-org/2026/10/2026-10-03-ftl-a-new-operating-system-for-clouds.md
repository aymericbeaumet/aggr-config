---
title: 'FTL: A new operating system for clouds'
link: https://ftl-os.org/
source: hnrss-org
published: 2026-10-03T15:02:36Z
updated: 2026-10-03T15:02:36Z
first_seen: 2026-10-03T22:08:04.577533956Z
authors:
- romac
summary: 'https://github.com/nuta/ftl Comments URL: https://news.ycombinator.com/item?id=49944912 Points: 131 # Comments: 55'
content: extracted
html: 2026-10-03-ftl-a-new-operating-system-for-clouds.html
preview:
  file: 2026-10-03-ftl-a-new-operating-system-for-clouds.preview-7095ea679c5b.webp
  width: 256
  height: 134
  color: '#0a0a0a'
images:
- source: https://ftl-os.org/og.png
  original:
    file: 2026-10-03-ftl-a-new-operating-system-for-clouds.image-ddde0fbe56aa.png
    width: 1200
    height: 630
  color: '#000000'
---

## What's FTL?

- You can build your own OS as a library. This userspace OS design makes it easy to add features, debug, upgrade the OS safely, as if writing applications.
- FTL kernel isolates containers (userspace OS instances) better than existing monolithic kernels, with a hypervisor-like interface based on a lightweight hardware-based isolation (user mode). You don't need bare-metal machines.
- FTL is compatible with Linux binaries. For example, the Rust-based HTTP server serving this website is a Linux application running on FTL. You can also run Unikernel-like specialized applications without POSIX abstractions.

## How it works

Each container runs a userspace OS. It is a shared library which implements most of OS concepts such as Linux process, VFS, and TCP/IP. FTL kernel provides a minimal interface to implement Linux system calls in userspace, just like a hypervisor.

FTL combines the best of microkernels (flexible & secure) and monolithic kernels (performant & simple). Our goal is to make lightweight containers as secure as VMs, and unlock new OS-level abilities in applications, without sacrificing performance:

```
FTL                                   Linux
┌────────────────────────────────┐    ┌────────────────────────────────┐
│┏━━━━━━━━━━━━━┓  ┏━━━━━━━━━━━━━┓│    │┏━━━━━━━━━━━━━┓  ┏━━━━━━━━━━━━━┓│
│┃             ┃  ┃             ┃│    │┃             ┃  ┃             ┃│
│┃    Linux    ┃  ┃    Linux    ┃│    │┃    Linux    ┃  ┃    Linux    ┃│
│┃   Process   ┃  ┃   Process   ┃│    │┃   Process   ┃  ┃   Process   ┃│
│┃             ┃  ┃             ┃│    │┃             ┃  ┃             ┃│
│┃╌╌╌╌╌ Linux system calls ╌╌╌╌╌┃│    │┗━━━━━━━━━━━━━┛  ┗━━━━━━━━━━━━━┛│
│┃                              ┃│    └────────────────────────────────┘
│┃         Userspace OS         ┃│    ╌╌╌╌╌╌╌╌ Linux's interface ╌╌╌╌╌╌╌
│┃   (Process, VFS, TCP, ...)   ┃│    ╔════════════════════════════════╗
│┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛│    ║                                ║
└────────────────────────────────┘    ║          Linux Kernel          ║
╌╌╌╌╌╌╌ minimal interface ╌╌╌╌╌╌╌╌    ║                                ║
╔════════════════════════════════╗    ║   process, fork/exec, memory,  ║
║           FTL Kernel           ║    ║     signals, TCP/IP, /proc,    ║
║    vCPU, memory, drivers, ...  ║    ║       /dev, drivers ...        ║
╚════════════════════════════════╝    ╚════════════════════════════════╝
```

Userspace OS design also enables you to extend most of Linux kernel features without kernel/eBPF programming. You can add printfs, apply security updates, and add new features quickly and safely. In FTL, OS is just a library. [Read more](https://seiya.me/blog/introducing-ftl).

## Roadmap

- September 2026: Run a simple Linux HTTP server on FTL (released in [v0.0.1](https://seiya.me/blog/introducing-ftl) ✅)
- October 2026: Async Rust apps support - Linux threads, epoll, ... (released in [v0.1.0](https://seiya.me/blog/ftl-v0.1.0) ✅)
- November 2026: Filesystem
- December 2026: Node.js / Go support
- January 2027: SMP, container images, 64-bit Arm support

## Links

- [GitHub repository](https://github.com/nuta/ftl)
- [Introducing FTL: A new operating system for clouds (blog post)](https://seiya.me/blog/introducing-ftl)
