---
title: 'Show HN: NSL – WSL for Linux'
link: https://frostyard.github.io/nsl/
source: hnrss-org
published: 2026-09-29T14:51:36Z
updated: 2026-09-29T14:51:36Z
first_seen: 2026-09-30T09:01:05.864532337Z
authors:
- bketelsen
summary: 'One of the things that Windows really got right is WSL2. I drive an atomic Linux distro for daily use, but wanted a way to develop with multiple different distros with that same WSL UX. NSL is my answer. It is a faithful reproduction of the developer experience, powered by a single VM that hosts one or more systemd-nspawn containers with your development instances. Host file edits and port sharing come along for the ride, just like WSL. Take a look and tell me what you think... It''s yet another step in my long journey to keep my host installation free from all the changing and breaking dev dependencies that force a reinstall every few months. Comments URL: https://news.ycombinator.com/item?id=49894351 Points: 129 # Comments: 79'
content: extracted
html: 2026-09-29-show-hn-nsl-wsl-for-linux.html
preview:
  file: 2026-09-29-show-hn-nsl-wsl-for-linux.preview-98f600cab909.webp
  width: 256
  height: 256
  alt: NSL
  color: '#1f4358'
images:
- source: https://frostyard.github.io/nsl/assets/nsl.svg
  original:
    file: 2026-09-29-show-hn-nsl-wsl-for-linux.image-0af520d93445.png
    width: 512
    height: 512
  variants:
  - file: 2026-09-29-show-hn-nsl-wsl-for-linux.image-af76531ee028.webp
    width: 320
    height: 320
  - file: 2026-09-29-show-hn-nsl-wsl-for-linux.image-55c35df1621b.webp
    width: 512
    height: 512
  color: '#081726'
---

## Linux development on an atomic host

Install your development tools in a Debian, Fedora or other Linux machine and leave the host alone. nsl works much like WSL: each machine keeps its packages, services and files between sessions. The machines run as systemd-nspawn containers inside a shared VM, start when you need them, and can work in your host files.

[Install nsl →](https://frostyard.github.io/nsl/getting-started/install/) [How it works](https://frostyard.github.io/nsl/concepts/how-it-works/)

![NSL](https://frostyard.github.io/nsl/assets/nsl.svg)

```sh
nsl create debian --distro debian:13   # verify the signed images; the first machine is the default
nsl                                    # a login shell in the machine, in this directory
nsl run make test                      # one command, with its exit status
```

## What a machine gives you

- **A shell where you need it**

  * * *

  Run `nsl` from your project directory to open a shell in the default machine, working in the same files. Use `nsl run` for a single command; its exit status comes back to the host.

- **Your files and your account**

  * * *

  Your `$HOME`, `/run/media/USER` and `/mnt` are available under `/mnt/host`. The machine uses your username, UID and GID, so files you create there still belong to you. You also get passwordless `sudo` inside the machine.

- **Ports and windows on the host**

  * * *

  Run a development server in the machine and reach its forwarded port on host `127.0.0.1`. Wayland applications can open windows on your desktop through Waypipe.

- **Seven signed distros**

  * * *

  Choose Debian, Ubuntu, Fedora, CentOS Stream, Arch, openSUSE Tumbleweed or Leap. The images are rebuilt weekly. nsl verifies that they came from the signed Frostyard publishing workflow before using them.

- **Isolation when you need it**

  * * *

  Use `--isolated` for software you don't trust. It gets its own VM, without access to your host files, desktop or host actions.

- **Runs as your user**

  * * *

  nsl runs as your user. You'll need the host prerequisites installed first; nsl doesn't install packages or change device permissions, groups or sudoers.

## Start here

- 01 **[Get started](https://frostyard.github.io/nsl/getting-started/install/)**

  Check the host, install nsl and create your first machine.

- 02 **[Architecture](https://frostyard.github.io/nsl/concepts/how-it-works/)**

  How the VM runs your machines and connects them to the host.

- 03 **[Command reference](https://frostyard.github.io/nsl/reference/cli/)**

  Look up commands, settings in `nsl.conf` and published images.

Pre-release

nsl has no stable release yet. v0.4.0 is the first release of this design; v0.3.0 and earlier are a retired prototype. The tested host is Snow Linux 13 on x86-64 with systemd 261.2, QEMU 10.0.13, virtiofsd 1.13.2 and GNOME Wayland.
