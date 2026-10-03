---
title: One Runner Is Now Enough
link: https://ampcode.com/news/one-runner-is-now-enough
source: ampcode-com
published: 2026-09-17T00:00:00Z
updated: 2026-09-17T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'A runner can now serve many directories, not just the one it was started in. You can point it at specific projects and directories or let it find your Git repositories. Start a runner and tell it which directories to serve with --dir. Repeat the flag for each one: $ amp --no-tui --runner-id mac-mini --dir ~/code/amp --dir ~/code/sandcastle Or start it in the directory that holds your checkouts and let it find them: $ cd ~/code $ amp --no-tui --runner-id mac-mini --discover-dirs --discover-dirs serves every Git checkout up to two levels beneath the current directory and picks up new ones as you clone them. Checkouts somewhere else, or deeper than two levels? Give --discover-dirs a path, repeat it for more, and set --discover-depth to look further down: $ amp --no-tui --discover-dirs=~/work --discover-dirs=~/code --discover-depth 4 Add or remove directories while the runner is running, without restarting it: $ amp runner dirs add ~/code/new-repo $ amp runner dirs list $ amp runner dirs remove ~/code/dotfiles The runner remembers the directories you add this way and serves them again the next time you start it from the same directory. Oh, They Can Update Themselves Too Runners now update themselves too. If you leave an amp --no-tui runner running, it keeps checking for new releases about once an hour and installs them. Once no thread is running on it, it restarts into the new version, at most once every 12 hours. It keeps its runner ID, its directories, and the rest of its flags. Turn it off with amp.runner.autoUpdate.enabled: false in your settings. Read more about serving multiple directories and runner updates in the runner docs.'
content: extracted
html: 2026-09-17-one-runner-is-now-enough.html
preview:
  file: 2026-09-17-one-runner-is-now-enough.preview-c90bbe2c0117.webp
  width: 256
  height: 134
  color: '#675a4a'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=One+Runner+Is+Now+Enough&date=September+17%2C+2026&tagline=A+runner+can+now+serve+many+directories%2C+not+just+the+one+it+was+started+in.+You+can+point+it+at+specific+projects+and+directories+or+let+it+find+your+Git+repositories.&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fone-runner-is-now-enough-background.avif&sig=cca78c400cf387dd4be21205bf476471aa17aec27f1d2f19f611bbc88a3011a2
  original:
    file: 2026-09-17-one-runner-is-now-enough.image-bc23fe116fe3.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-17-one-runner-is-now-enough.image-d1b2be42c981.webp
    width: 320
    height: 168
  - file: 2026-09-17-one-runner-is-now-enough.image-8bb972d0f020.webp
    width: 640
    height: 336
  - file: 2026-09-17-one-runner-is-now-enough.image-c99dc6765be6.webp
    width: 960
    height: 504
  - file: 2026-09-17-one-runner-is-now-enough.image-817bb0ec150d.webp
    width: 1200
    height: 630
  color: '#1b1511'
- source: https://static.ampcode.com/news/one-runner-is-now-enough-picker-dark.png
  original:
    file: 2026-09-17-one-runner-is-now-enough.image-53f1322c6280.png
    width: 1824
    height: 1183
  variants:
  - file: 2026-09-17-one-runner-is-now-enough.image-406bed339088.webp
    width: 320
    height: 208
  - file: 2026-09-17-one-runner-is-now-enough.image-dc5faa07030a.webp
    width: 640
    height: 415
  - file: 2026-09-17-one-runner-is-now-enough.image-e4b6cf3db977.webp
    width: 960
    height: 623
  - file: 2026-09-17-one-runner-is-now-enough.image-bab23cdc553e.webp
    width: 1280
    height: 830
  - file: 2026-09-17-one-runner-is-now-enough.image-ab60c9059b4f.webp
    width: 1600
    height: 1038
  - file: 2026-09-17-one-runner-is-now-enough.image-f09b1246e118.webp
    width: 1824
    height: 1183
  color: '#0c1516'
---

A [runner](https://ampcode.com/docs/cli/runners) can now serve many directories, not just the one it was started in. You can point it at specific projects and directories or let it find your Git repositories.

![The new thread composer on ampcode.com with the runner mac-mini selected and a picker listing its projects and folders](https://static.ampcode.com/news/one-runner-is-now-enough-picker-dark.png)

Start a runner and tell it which directories to serve with `--dir`. Repeat the flag for each one:

```shell-session
$ amp --no-tui --runner-id mac-mini --dir ~/code/amp --dir ~/code/sandcastle
```

Or start it in the directory that holds your checkouts and let it find them:

```shell-session
$ cd ~/code
$ amp --no-tui --runner-id mac-mini --discover-dirs
```

`--discover-dirs` serves every Git checkout up to two levels beneath the current directory and picks up new ones as you clone them.

Checkouts somewhere else, or deeper than two levels? Give `--discover-dirs` a path, repeat it for more, and set `--discover-depth` to look further down:

```shell-session
$ amp --no-tui --discover-dirs=~/work --discover-dirs=~/code --discover-depth 4
```

Add or remove directories while the runner is running, without restarting it:

```shell-session
$ amp runner dirs add ~/code/new-repo
$ amp runner dirs list
$ amp runner dirs remove ~/code/dotfiles
```

The runner remembers the directories you add this way and serves them again the next time you start it from the same directory.

## Oh, They Can Update Themselves Too

Runners now update themselves too. If you leave an `amp --no-tui` runner running, it keeps checking for new releases about once an hour and installs them. Once no thread is running on it, it restarts into the new version, at most once every 12 hours. It keeps its runner ID, its directories, and the rest of its flags.

Turn it off with `amp.runner.autoUpdate.enabled: false` in your settings.

Read more about [serving multiple directories](https://ampcode.com/docs/cli/runners#serve-multiple-directories) and [runner updates](https://ampcode.com/docs/cli/runners#keep-a-runner-updated) in the runner docs.
