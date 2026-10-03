---
title: No Mailmap Required
link: https://ampcode.com/news/no-mailmap-required
source: ampcode-com
published: 2026-08-28T00:00:00Z
updated: 2026-08-28T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Amp can now use a Git identity that is separate from the email on your Amp account. Pick the name and verified email that should appear on commits made in orbs. You can ask Puck to set it for you: If you have not added the identity yet, Puck sends a verification link to the email address. Once you click it, Puck gets notified and can make the identity your personal default if you asked it to. You can also manage identities under Signing Keys. Your personal default applies to personal projects unless you choose another identity in a project''s settings. Workspace admins choose one rule for the whole workspace: Amp uses the Amp identity. Amp Account uses each member''s Amp account. User Choice lets each member choose. When Amp starts an orb, it sets GIT_AUTHOR_* and GIT_COMMITTER_* from the selected identity. If commit signing is on, Amp only signs for an email you have verified. Your Amp login can stay tfc@ampcode.com while your commits use tim@culver.house.'
content: extracted
html: 2026-08-28-no-mailmap-required.html
preview:
  file: 2026-08-28-no-mailmap-required.preview-3f62fcf8861c.webp
  width: 256
  height: 134
  color: '#827b6f'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=No+Mailmap+Required&date=August+28%2C+2026&tagline=Choose+the+verified+name+and+email+Amp+uses+to+author+and+sign+your+commits.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fgit-identity-puck-20260826.png%3Fv%3D3&sig=04f9ef7f2b6285ac84065fd009871a0e2ab721c7a57cfbdbbecb7b59552f30b0
  original:
    file: 2026-08-28-no-mailmap-required.image-6dfd16bbdb80.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-28-no-mailmap-required.image-9d46bd90469a.webp
    width: 320
    height: 168
  - file: 2026-08-28-no-mailmap-required.image-4a9f560a5db7.webp
    width: 640
    height: 336
  - file: 2026-08-28-no-mailmap-required.image-f17fc20ab8ac.webp
    width: 960
    height: 504
  - file: 2026-08-28-no-mailmap-required.image-85ee8c6a595c.webp
    width: 1200
    height: 630
  color: '#66605a'
- source: https://static.ampcode.com/news/git-identity-puck-20260826.png?v=3
  original:
    file: 2026-08-28-no-mailmap-required.image-1c7b5c39c362.png
    width: 832
    height: 225
  color: '#f8fcf5'
---

Amp can now use a Git identity that is separate from the email on your Amp account. Pick the name and verified email that should appear on commits made in orbs.

You can ask Puck to set it for you:

![Puck setting Tim Culverhouse and tim@culver.house as the personal default Git identity](https://static.ampcode.com/news/git-identity-puck-20260826.png?v=3)

If you have not added the identity yet, Puck sends a verification link to the email address. Once you click it, Puck gets notified and can make the identity your personal default if you asked it to.

You can also manage identities under [Signing Keys](https://ampcode.com/settings/keys#git-identities). Your personal default applies to personal projects unless you choose another identity in a project's settings. Workspace admins choose one rule for the whole workspace:

- **Amp** uses the Amp identity.
- **Amp Account** uses each member's Amp account.
- **User Choice** lets each member choose.

When Amp starts an orb, it sets `GIT_AUTHOR_*` and `GIT_COMMITTER_*` from the selected identity. If commit signing is on, Amp only signs for an email you have verified.

Your Amp login can stay `tfc@ampcode.com` while your commits use `tim@culver.house`.
