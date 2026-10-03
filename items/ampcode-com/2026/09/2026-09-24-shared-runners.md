---
title: Shared Runners
link: https://ampcode.com/news/shared-runners
source: ampcode-com
published: 2026-09-24T00:00:00Z
updated: 2026-09-24T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'You can now share a runner with your workspace. Start it with --share and everyone in your workspace can start threads on that machine from ampcode.com. If you have a machine with GPUs, a Mac that''s used to build and sign the iOS app, or a dev box in a specific network, you can now set it up and the whole team can spawn agents on it: $ amp --no-tui --runner-id macos-builder --share Everyone in your workspace now sees it in the picker under Shared Runners. With --amp-env, a shared runner gets the workspace and project Secrets & Env Vars, but never your personal ones. That applies to your own threads on it too. We need to offer a word of warning, though: everyone you share with runs code on your machine as you, with your files, your credentials, and your logins. Their threads can also work in the same directories at the same time. So only share a runner with people you''d trust with a shell on that machine. Even better, give the runner a machine of its own. (Better still: use orbs, so that every thread gets its own machine.) Workspace admins can turn off runner sharing in Member Settings. Read more about sharing a runner in the runner docs.'
content: extracted
html: 2026-09-24-shared-runners.html
preview:
  file: 2026-09-24-shared-runners.preview-d396d86921ef.webp
  width: 256
  height: 134
  color: '#676054'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Shared+Runners&date=September+24%2C+2026&tagline=Runners+can+now+be+shared+with+your+workspace.+Start+one+with+--share+and+everyone+in+your+workspace+can+start+threads+on+that+machine+from+ampcode.com.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fshared-runners-picker-dark.png%3Fv%3D2&sig=cf7169c0302b0d9fc179bd9ac417bdbfb60aa1c4fa9de7991bb6b1900e6c7848
  original:
    file: 2026-09-24-shared-runners.image-9df4206f0c59.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-24-shared-runners.image-54c850993bd6.webp
    width: 320
    height: 168
  - file: 2026-09-24-shared-runners.image-ad15e134817e.webp
    width: 640
    height: 336
  - file: 2026-09-24-shared-runners.image-9750b9300dad.webp
    width: 960
    height: 504
  - file: 2026-09-24-shared-runners.image-abde298cea42.webp
    width: 1200
    height: 630
  color: '#1b1714'
- source: https://static.ampcode.com/news/shared-runners-picker-dark.png?v=2
  original:
    file: 2026-09-24-shared-runners.image-5db9332c6a8a.png
    width: 1824
    height: 1185
  variants:
  - file: 2026-09-24-shared-runners.image-e0bf27bca76b.webp
    width: 320
    height: 208
  - file: 2026-09-24-shared-runners.image-12f9b60bac71.webp
    width: 640
    height: 416
  - file: 2026-09-24-shared-runners.image-5c037a1bdf6c.webp
    width: 960
    height: 624
  - file: 2026-09-24-shared-runners.image-f0f14eb7fabe.webp
    width: 1280
    height: 832
  - file: 2026-09-24-shared-runners.image-84ba20ac2816.webp
    width: 1600
    height: 1039
  - file: 2026-09-24-shared-runners.image-d7be79f8b0f3.webp
    width: 1824
    height: 1185
  color: '#0c1516'
---

You can now share a [runner](https://ampcode.com/docs/cli/runners) with your workspace. Start it with `--share` and everyone in your workspace can start threads on that machine from ampcode.com.

![The new thread composer on ampcode.com with the location picker open. Under Runners is your own devbox. Under Shared Runners are gpu-runner, shared by Allison, and macos-builder, shared by Monty, each with its owner's avatar and running-thread count.](https://static.ampcode.com/news/shared-runners-picker-dark.png?v=2)

If you have a machine with GPUs, a Mac that's used to build and sign the iOS app, or a dev box in a specific network, you can now set it up and the whole team can spawn agents on it:

```shell-session
$ amp --no-tui --runner-id macos-builder --share
```

Everyone in your workspace now sees it in the picker under **Shared Runners**.

With [`--amp-env`](https://ampcode.com/docs/cli/runners#secrets-env-vars), a shared runner gets the workspace and project Secrets & Env Vars, but never your personal ones. That applies to your own threads on it too.

We need to offer a word of warning, though: everyone you share with runs code on your machine as you, with your files, your credentials, and your logins. Their threads can also work in the same directories at the same time. So only share a runner with people you'd trust with a shell on that machine. Even better, give the runner a machine of its own. (Better still: use [orbs](https://ampcode.com/what-are-orbs), so that every thread gets its own machine.)

Workspace admins can [turn off runner sharing](https://ampcode.com/docs/cli/runners#turn-off-runner-sharing-for-a-workspace) in **Member Settings**.

Read more about [sharing a runner](https://ampcode.com/docs/cli/runners#share-a-runner-with-your-workspace) in the runner docs.
