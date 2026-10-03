---
title: One Runner, Many Worktrees
link: https://ampcode.com/news/one-runner-many-worktrees
source: ampcode-com
published: 2026-09-22T00:00:00Z
updated: 2026-09-22T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'A runner can now create Git worktrees. Pick a repository the runner serves, hit Tab, name the branch, and the thread starts in a fresh checkout. Your main checkout stays untouched. In the directory picker, every Git checkout on the runner gets a New Worktree option next to it. Choose it and you get a small form: The name you chose becomes the branch name and the folder name. The runner then creates the worktree as a sibling of the repository: ~/code/amp gets ~/code/amp-fix-flaky-login-test, checked out on a new branch fix-flaky-login-test off the current HEAD. Uncommitted changes stay where they are, in the original checkout. The new directory shows up in the picker right away, marked with a branch icon, and the thread starts in it. Puck can do this too: ask it to start a thread on your runner in a new worktree and it passes a worktree name to create_thread. Clean Up When You''re Done A thread that runs in a worktree the runner created gets a new action: Archive and Remove Worktree. It archives the thread, runs git worktree remove, and deletes the branch. New Folders and Projects Too The picker can also create things. Type the name of a directory that doesn''t exist yet and you get New Directory or New Project. Read more. Oh, They Understand Secrets Now Too Runners can also use the Secrets & Env Vars you configure on ampcode.com. Same variables that orbs get. It''s off by default; opt in with --amp-env: $ amp --no-tui --runner-id mac-mini --discover-dirs --amp-env Every time a thread starts, the runner fetches the variables that apply to it (personal, then project, then workspace) and adds them to the environment of the thread''s shell commands, MCP servers, and plugins. Change a variable on ampcode.com and the next thread gets the new value. No restart, no SSH. Both need a runner on the current Amp version. If yours has been running for a while, it has updated itself already. Read more about worktrees and Secrets & Env Vars in the runner docs.'
content: extracted
html: 2026-09-22-one-runner-many-worktrees.html
preview:
  file: 2026-09-22-one-runner-many-worktrees.preview-f05bb6b7ca57.webp
  width: 256
  height: 134
  color: '#5c554a'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=One+Runner%2C+Many+Worktrees&date=September+22%2C+2026&tagline=Runners+can+now+create+Git+worktrees%2C+directories%2C+and+projects+for+you%2C+straight+from+the+thread+composer.+And+they+finally+know+your+secrets.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fone-runner-many-worktrees-picker-dark.png&sig=2a34d6b1587c846374a555c2ce543885c498d05c703c37f1d3a58a905a95c6be
  original:
    file: 2026-09-22-one-runner-many-worktrees.image-668f0a7c841b.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-22-one-runner-many-worktrees.image-ff5ac4301ebd.webp
    width: 320
    height: 168
  - file: 2026-09-22-one-runner-many-worktrees.image-664ff4c1df56.webp
    width: 640
    height: 336
  - file: 2026-09-22-one-runner-many-worktrees.image-98cc697dbfd4.webp
    width: 960
    height: 504
  - file: 2026-09-22-one-runner-many-worktrees.image-010e2bebf619.webp
    width: 1200
    height: 630
  color: '#1b1714'
- source: https://static.ampcode.com/news/one-runner-many-worktrees-picker-dark.png
  original:
    file: 2026-09-22-one-runner-many-worktrees.image-ad7151186b42.png
    width: 1824
    height: 1312
  variants:
  - file: 2026-09-22-one-runner-many-worktrees.image-3b6195a13140.webp
    width: 320
    height: 230
  - file: 2026-09-22-one-runner-many-worktrees.image-8ab825120213.webp
    width: 640
    height: 460
  - file: 2026-09-22-one-runner-many-worktrees.image-c5ec7a7d7463.webp
    width: 960
    height: 691
  - file: 2026-09-22-one-runner-many-worktrees.image-aadd875b9e9a.webp
    width: 1280
    height: 921
  - file: 2026-09-22-one-runner-many-worktrees.image-5d2c7796521d.webp
    width: 1600
    height: 1151
  - file: 2026-09-22-one-runner-many-worktrees.image-261c392a845a.webp
    width: 1824
    height: 1312
  color: '#0c1516'
- source: https://static.ampcode.com/news/one-runner-many-worktrees-form-dark.png
  original:
    file: 2026-09-22-one-runner-many-worktrees.image-79a28b06c5ae.png
    width: 1824
    height: 1029
  variants:
  - file: 2026-09-22-one-runner-many-worktrees.image-fe2f158a2544.webp
    width: 320
    height: 181
  - file: 2026-09-22-one-runner-many-worktrees.image-f4bea0be800a.webp
    width: 640
    height: 361
  - file: 2026-09-22-one-runner-many-worktrees.image-80cb46a92ac8.webp
    width: 960
    height: 542
  - file: 2026-09-22-one-runner-many-worktrees.image-3393b6a2b90c.webp
    width: 1280
    height: 722
  - file: 2026-09-22-one-runner-many-worktrees.image-541404c36b60.webp
    width: 1600
    height: 903
  - file: 2026-09-22-one-runner-many-worktrees.image-d76ba3fd6c0b.webp
    width: 1824
    height: 1029
  color: '#0c1516'
- source: https://static.ampcode.com/news/one-runner-many-worktrees-menu-dark.png
  original:
    file: 2026-09-22-one-runner-many-worktrees.image-fc4713743844.png
    width: 736
    height: 834
  variants:
  - file: 2026-09-22-one-runner-many-worktrees.image-4adb7fff70b9.webp
    width: 320
    height: 363
  - file: 2026-09-22-one-runner-many-worktrees.image-ae08c2969762.webp
    width: 736
    height: 834
  color: '#0b0c0a'
- source: https://static.ampcode.com/news/one-runner-many-worktrees-create-dark.png
  original:
    file: 2026-09-22-one-runner-many-worktrees.image-6832fb42af1a.png
    width: 1824
    height: 856
  variants:
  - file: 2026-09-22-one-runner-many-worktrees.image-7bf9beed09a9.webp
    width: 320
    height: 150
  - file: 2026-09-22-one-runner-many-worktrees.image-d60c73476088.webp
    width: 640
    height: 300
  - file: 2026-09-22-one-runner-many-worktrees.image-1c6b2816a9f1.webp
    width: 960
    height: 451
  - file: 2026-09-22-one-runner-many-worktrees.image-30c1905d3b2a.webp
    width: 1280
    height: 601
  - file: 2026-09-22-one-runner-many-worktrees.image-814881ea0db6.webp
    width: 1824
    height: 856
  color: '#0c1516'
---

A [runner](https://ampcode.com/docs/cli/runners) can now create Git worktrees. Pick a repository the runner serves, hit Tab, name the branch, and the thread starts in a fresh checkout. Your main checkout stays untouched.

![The new thread composer on ampcode.com with the runner mac-mini selected and the directory picker open. The amp repository is highlighted and offers Current or New Worktree.](https://static.ampcode.com/news/one-runner-many-worktrees-picker-dark.png)

In the directory picker, every Git checkout on the runner gets a **New Worktree** option next to it. Choose it and you get a small form:

![The worktree form in the new thread composer: New worktree of amp, name fix-flaky-login-test, directory ~/code/amp-fix-flaky-login-test, branch fix-flaky-login-test, from current HEAD, and a Create Worktree button](https://static.ampcode.com/news/one-runner-many-worktrees-form-dark.png)

The name you chose becomes the branch name and the folder name. The runner then creates the worktree as a sibling of the repository: `~/code/amp` gets `~/code/amp-fix-flaky-login-test`, checked out on a new branch `fix-flaky-login-test` off the current `HEAD`. Uncommitted changes stay where they are, in the original checkout. The new directory shows up in the picker right away, marked with a branch icon, and the thread starts in it.

[Puck](https://ampcode.com/docs/puck) can do this too: ask it to start a thread on your runner in a new worktree and it passes a `worktree` name to `create_thread`.

## Clean Up When You're Done

A thread that runs in a worktree the runner created gets a new action: **Archive and Remove Worktree**. It archives the thread, runs `git worktree remove`, and deletes the branch.

![The thread actions menu on ampcode.com with a new entry, Archive and Remove Worktree, between Archive and Delete](https://static.ampcode.com/news/one-runner-many-worktrees-menu-dark.png)

## New Folders and Projects Too

The picker can also create things. Type the name of a directory that doesn't exist yet and you get **New Directory** or **New Project**. [Read more.](https://ampcode.com/docs/cli/runners#create-a-directory-or-project)

![The directory picker in the new thread composer with a name typed that doesn't exist yet, offering New Directory and New Project](https://static.ampcode.com/news/one-runner-many-worktrees-create-dark.png)

## Oh, They Understand Secrets Now Too

Runners can also use the [Secrets & Env Vars](https://ampcode.com/docs/orbs/handling-secrets) you configure on ampcode.com. Same variables that orbs get. It's off by default; opt in with `--amp-env`:

```shell-session
$ amp --no-tui --runner-id mac-mini --discover-dirs --amp-env
```

Every time a thread starts, the runner fetches the variables that apply to it (personal, then project, then workspace) and adds them to the environment of the thread's shell commands, MCP servers, and plugins. Change a variable on ampcode.com and the next thread gets the new value. No restart, no SSH.

Both need a runner on the current Amp version. If yours has been running for a while, it has [updated itself](https://ampcode.com/docs/cli/runners#keep-a-runner-updated) already.

Read more about [worktrees](https://ampcode.com/docs/cli/runners#create-a-worktree) and [Secrets & Env Vars](https://ampcode.com/docs/cli/runners#secrets-env-vars) in the runner docs.
