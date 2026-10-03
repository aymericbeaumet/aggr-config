---
title: npm Package Changes
link: https://ampcode.com/news/npm-package-changes
source: ampcode-com
published: 2026-05-14T00:00:00Z
updated: 2026-05-14T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'We''re now shipping the Amp CLI as a single-file executable (compiled by Bun) instead of as a JavaScript source package. This makes Amp faster and more compatible across platforms and runtimes, and it''s necessary to support Amp plugins. If you''re using the recommended direct installation, nothing changes for you. You''ve been using this single-file executable for several months. You can stop reading here. If you''ve installed Amp via npm, you should switch to direct installation: npm uninstall -g @sourcegraph/amp curl -fsSL https://ampcode.com/install.sh | bash (See all installation methods.) If you need to keep using npm to install Amp, usually because your company has an internal npm mirror/archive, be aware of some changes: The CLI''s npm package will now contain the executable instead of sources. We''re renaming 2 npm packages: The Amp CLI is now @ampcode/cli (was @sourcegraph/amp) The Amp TypeScript SDK is now @ampcode/sdk (was @sourcegraph/amp-sdk) The old package names are aliases but will be removed on June 15, 2026.'
content: extracted
html: 2026-05-14-npm-package-changes.html
preview:
  file: 2026-05-14-npm-package-changes.preview-8fe1051942fc.webp
  width: 256
  height: 134
  color: '#645d52'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=npm+Package+Changes&date=May+14%2C+2026&tagline=The+npm+package+for+the+Amp+CLI+is+now+%40ampcode%2Fcli+%28was+%40sourcegraph%2Famp%29.&sig=4268c2a9f6e02c6704ee832ef243563a0852c920566d3e83649a82c857faba09
  original:
    file: 2026-05-14-npm-package-changes.image-9fff5134b7c5.png
    width: 1200
    height: 630
  variants:
  - file: 2026-05-14-npm-package-changes.image-ec97b660c790.webp
    width: 320
    height: 168
  - file: 2026-05-14-npm-package-changes.image-483911bb94c1.webp
    width: 640
    height: 336
  - file: 2026-05-14-npm-package-changes.image-fe5cc5cfd460.webp
    width: 960
    height: 504
  - file: 2026-05-14-npm-package-changes.image-420c72e3281e.webp
    width: 1200
    height: 630
  color: '#221b16'
---

We're now shipping the Amp CLI as a single-file executable ([compiled by Bun](https://bun.com/docs/bundler/executables)) instead of as a JavaScript source package. This makes Amp faster and more compatible across platforms and runtimes, and it's necessary to support [Amp plugins](https://ampcode.com/manual#plugins).

If you're using the recommended [direct installation](https://ampcode.com/manual#get-started), nothing changes for you. You've been using this single-file executable for several months. You can stop reading here.

If you've installed Amp via npm, you should switch to direct installation:

```bash
npm uninstall -g @sourcegraph/amp
curl -fsSL https://ampcode.com/install.sh | bash
```

(See [all installation methods](https://ampcode.com/manual#get-started).)

If you need to keep using npm to install Amp, usually because your company has an internal npm mirror/archive, be aware of some changes:

- The CLI's npm package will now contain the executable instead of sources.
- We're renaming 2 npm packages:
  - The Amp CLI is now [`@ampcode/cli`](https://www.npmjs.com/package/@ampcode/cli) (was [`@sourcegraph/amp`](https://www.npmjs.com/package/@sourcegraph/amp))
  - The Amp TypeScript SDK is now [`@ampcode/sdk`](https://www.npmjs.com/package/@ampcode/sdk) (was [`@sourcegraph/amp-sdk`](https://www.npmjs.com/package/@sourcegraph/amp-sdk))

The old package names are aliases but will be removed on June 15, 2026.
