---
title: Setup Without a Commit
link: https://ampcode.com/news/setup-without-a-commit
source: ampcode-com
published: 2026-08-25T00:00:00Z
updated: 2026-08-25T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'You can now store scripts to set up orbs outside of your repository. Amp can store pre-clone and pre-setup scripts in your project settings and access them when spawning an orb. The pre-clone setup script gives Amp anything it needs before it clones the repository: Install Git extensions that fetch files during checkout. Configure certificates for an internal Git server. Configure a network proxy needed to reach the Git server. Connect the orb to a private network with Tailscale (known issue: use TAILSCALE_API_KEY, OIDC is not yet working with pre-clone scripts). Install a credential helper required by the Git server. The pre-setup script lets you work with orbs when you aren''t ready to commit setup files to the repository. Amp agents and Puck can access the scripts and set them for you. Give Puck this prompt, or start a thread for the project with this prompt: Amp will then inspect the repository and decide which work belongs before or after the clone. It will write and test the scripts, then save them in the project settings. The scripts are also available on the project settings page, where you can review or edit them by hand.'
content: extracted
html: 2026-08-25-setup-without-a-commit.html
preview:
  file: 2026-08-25-setup-without-a-commit.preview-1ff71a379fb7.webp
  width: 256
  height: 134
  color: '#817b6e'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Setup+Without+a+Commit&date=August+25%2C+2026&tagline=Set+up+orbs+before+and+after+cloning+without+committing+Amp-specific+files.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fpre-clone-puck-working-final-cropped-20260825.png&sig=79d1a199444f9d85af5c72d316b3f02c6e5153532786a8536e1ea18bdeab4b11
  original:
    file: 2026-08-25-setup-without-a-commit.image-663ff2a47703.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-25-setup-without-a-commit.image-6434ffceda30.webp
    width: 320
    height: 168
  - file: 2026-08-25-setup-without-a-commit.image-8c88e1b279c5.webp
    width: 640
    height: 336
  - file: 2026-08-25-setup-without-a-commit.image-8780d77427db.webp
    width: 960
    height: 504
  - file: 2026-08-25-setup-without-a-commit.image-6bde56ebe4eb.webp
    width: 1200
    height: 630
  color: '#66615a'
- source: https://static.ampcode.com/news/pre-clone-puck-working-final-cropped-20260825.png
  original:
    file: 2026-08-25-setup-without-a-commit.image-87ef9186f9bc.png
    width: 808
    height: 185
  color: '#f8fcf5'
- source: https://static.ampcode.com/news/pre-setup-amp-thread-20260826.png
  original:
    file: 2026-08-25-setup-without-a-commit.image-3575de6496a9.png
    width: 864
    height: 208
  color: '#f8fcf5'
---

You can now store scripts to set up orbs outside of your repository. Amp can store pre-clone and pre-setup scripts in your project settings and access them when spawning an orb.

The pre-clone setup script gives Amp anything it needs before it clones the repository:

- Install Git extensions that fetch files during checkout.
- Configure certificates for an internal Git server.
- Configure a network proxy needed to reach the Git server.
- Connect the orb to a private network with Tailscale (known issue: use `TAILSCALE_API_KEY`, OIDC is not yet working with pre-clone scripts).
- Install a credential helper required by the Git server.

![Puck working on a request to set up a pre-clone script that installs Git LFS](https://static.ampcode.com/news/pre-clone-puck-working-final-cropped-20260825.png)

The pre-setup script lets you work with orbs when you aren't ready to commit setup files to the repository. Amp agents and Puck can access the scripts and set them for you. Give Puck this prompt, or start a thread for the project with this prompt:

![An Amp thread starting setup without committing files to the repository](https://static.ampcode.com/news/pre-setup-amp-thread-20260826.png)

Amp will then inspect the repository and decide which work belongs before or after the clone. It will write and test the scripts, then save them in the project settings.

The scripts are also available on the project settings page, where you can review or edit them by hand.
