---
title: My Minimal MacBook Pro Setup Guide
link: https://eugeneyan.com//writing/mac-setup/
source: eugeneyan-com
published: 2024-11-17T00:00:00Z
updated: 2024-11-17T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- misc
summary: Setting up my new MacBook Pro from scratch
content: extracted
html: 2024-11-17-my-minimal-macbook-pro-setup-guide.html
preview:
  file: 2024-11-17-my-minimal-macbook-pro-setup-guide.preview-fc0b570dd3a5.webp
  width: 256
  height: 134
  color: '#414851'
images:
- source: https://eugeneyan.com/assets/og_image/mac.jpg
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-d1f2e6f15c95.jpg
    width: 1200
    height: 630
  color: '#161728'
- source: https://eugeneyan.com/assets/rectangle.webp
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-f8838b5956a1.webp
    width: 974
    height: 148
  variants:
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-199eda003222.webp
    width: 320
    height: 49
  color: '#eaeaea'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2024-11-17-my-minimal-macbook-pro-setup-guide.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

I just upgraded my personal laptop from a 2019 Intel MacBook Pro to an M4 MacBook Pro. Like all my new devices, instead of restoring from a backup, I try to Marie Kondo my digital life and start from a clean slate. This also lets me reexamine my existing tools and explore new options. Here’s my minimal Mac setup guide if you want to follow along.

- [MacOS settings](https://eugeneyan.com//writing/mac-setup/#macos-settings)
- [Basic developer tools](https://eugeneyan.com//writing/mac-setup/#basic-developer-tools)
- [Research, writing, development](https://eugeneyan.com//writing/mac-setup/#research-writing-development)
- [Productivity and quality of life](https://eugeneyan.com//writing/mac-setup/#productivity-and-quality-of-life)
- [Entertainment and communications](https://eugeneyan.com//writing/mac-setup/#entertainment-and-communications)

## MacOS settings

- **Apple ID**: Sign in
- **MacOS update**: Settings -> General -> Software Update
- **Keyboard**: Switch to Dvorak, key repeat = fast, delay until repeat = short
- **Trackpad**: Max tracking speed, tap to click, click = light, natural scroll = off
- **Displays**: Switch to Apple Display (p3-600) for slightly better battery life
- **Finder**: Show Library, show hidden files, show path to dir

```bash
# show Library folder
chflags nohidden ~/Library

# show hidden files
defaults write com.apple.finder AppleShowAllFiles YES

# add pathbar to title
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true

# restart finder
killall Finder;
```

- **Screenshots** (to clipboard): `CMD + SHIFT + F5` and change setting in “Option” menu. This lets me screenshot and paste into docs, chats, social media, etc directly (without saving a separate file). If I need to save it, open Preview and `CMD + N`.
- **Dock**: Hide and show dock, reduce size, remove most default apps

## Basic developer tools

- **Homebrew** (might take a while as it also installs Xcode)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# add brew to default shell path
echo >> /Users/eugeneyan/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/eugeneyan/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# check for updates
brew update
```

- **Terminal**: Trying Warp instead of my usual iTerm. (If you use [my referral code](https://app.warp.dev/referral/D9WZNJ) I can get some swag—thank you!)

```bash
brew install --cask warp
brew install --cask font-inconsolata-for-powerline
# Update warp font: Settings -> Appearance -> Terminal font
```

- **Shell**: Trying Fish instead of my usual Oh My Zsh

```bash
brew install fish

# make fish default shell
echo $(which fish) | sudo tee -a /etc/shells
chsh -s $(which fish)

# add brew to fish path
echo >> /Users/eugeneyan/.config/fish/config.fish
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/eugeneyan/.config/fish/config.fish
eval "$(/opt/homebrew/bin/brew shellenv)"
```

- **Htop**: A better top

```bash
brew install htop
```

- **New SSH keys**

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

eval "$(ssh-agent -s)"
touch ~/.ssh/config
open ~/.ssh/config

# add to config
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Also see the Github docs to [generate a new ssh key and add to ssh agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), and [add a new ssh key to Github account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

- **Git**

```bash
brew install git
git config --global init.defaultBranch main
git config --global user.name "eugeneyan"
git config --global user.email hi@eugeneyan.com
```

## Research, writing, development

- **Obsidian**: All my notes, writing, CRM, etc. live here

```bash
brew install --cask obsidian

# clone obsidian vault (i use obsidian-git for syncing)
git clone git@github.com:<github-username>/<obsidian-vault>.git
```

- **Zotero**: Papers and annotations. Zotero has a nice PDF reader and markup tools, and also has an iPad app that syncs seamlessly. (Previously Google Drive)

```bash
brew install --cask zotero
# enable zotero plugin in safari
```

- **Cursor**: Daily driver for prototyping and building. (Previously vscode)

```bash
brew install --cask cursor
# old machine: cmd+shift+p > export profile
# new machine: cmd+shift+p > import profile
```

- **Sublime**: Simple code and text edits

```bash
brew install --cask sublime-text
```

- **Chrome**: For web development

```bash
brew install --cask google-chrome
```

- **Postgres**: To prototype web apps that need persistent storage

```bash
brew install postgresql@16
```

- **Python, JS, and Ruby**

```bash
# very fast python package manager
brew install uv

# install the latest python
uv python install 3.12
```

```bash
brew install node
brew install nvm

# still deciding between pnpm and bun
brew install pnpm
brew install oven-sh/bun/bun
```

```bash
brew install chruby-fish ruby-install ruby-build
brew install rbenv

# workaround for fish shell
set --universal fish_user_paths $fish_user_paths ~/.rbenv/shims
rbenv global 3.3.5
rbenv rehash
```

- **Docker**

```bash
brew install --cask docker
```

- **Ollama + Open WebUI**: Running local models via a nice interface

```bash
brew install ollama
ollama serve

# in another terminal, pull some models to try
ollama pull llama3.2 nemotron  # 3B and 70B respectively
```

```bash
uv tool install open-webui
uv tool run open-webui serve
```

## Productivity and quality of life

- **Raycast**: A better spotlight

```bash
brew install --cask raycast
```

- **Rectangle**: Window management.

```bash
brew install --cask rectangle
```

While Raycast already has window management, Rectangle lets you hit the hotkey again (e.g., `CTRL + CMD + LEFT`) to resize windows from 1/3 to 1/2 to 2/3. Great for widescreens.

![Rectangle settings for resizing windows](https://eugeneyan.com/assets/rectangle.webp "Rectangle settings for resizing windows")

Rectangle settings for resizing windows

> Update: Turns out Raycast has this too ([h/t @mwheatfill](https://x.com/mwheatfill/status/1859094246101279085)), and also has presets for Rectangle hotkeys. I’ve since moved to Raycast window management.

- **Wispr Flow**: [Download](https://www.flowvoice.ai/download) and set output language = English. Speech-to-text. Returns accurate transcripts with decent punctuation and formatting. (If you use [my referral link](https://www.flowvoice.ai/?referral=EUGENE11) you earn good karma and I get $15 in credits! )

- **Google Drive**: Syncing documents, files, media, etc.

```bash
brew install --cask google-drive
```

- **Stats + Ice**: Adding system stats to menu bar and customizing the menu bar

```bash
brew install stats
brew install jordanbaird-ice
```

- **Logi Options+** [Download](https://www.logitech.com/en-us/software/logi-options-plus.html) and sign in to load saved settings for my [MX Ergo](https://www.logitech.com/en-us/products/mice/mx-ergo-wireless-trackball-mouse.html).

## Entertainment and communications

- **Media**

```bash
brew install --cask vlc
brew install --cask spotify
```

- **Chat**

```bash
brew install --cask telegram
brew install --cask discord
brew install --cask slack
```

## References

- [Tweet thread with lots of great suggestions](https://x.com/eugeneyan/status/1857234166800367812)
- [Swyx’s Mac setup](https://www.swyx.io/new-mac-setup)
- [Sourabh Bajaj’s Mac Setup](https://sourabhbajaj.com/mac-setup/)
- [Robin Wieruch’s Mac Setup](https://www.robinwieruch.de/mac-setup-web-development/)
- [Tania Rascia’s Mac Setup](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/)
- [Thorsten Ball’s Mac Setup](https://registerspill.thorstenball.com/p/new-year-new-job-new-machine)
- [Mimansa Jaiswal’s Mac Setup](https://mimansajaiswal.github.io/posts/mac-softwares/)

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Nov 2024). My Minimal MacBook Pro Setup Guide. eugeneyan.com. https://eugeneyan.com/writing/mac-setup/.

or

```
@article{yan2024macsetup,
  title   = {My Minimal MacBook Pro Setup Guide},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2024},
  month   = {Nov},
  url     = {https://eugeneyan.com/writing/mac-setup/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
