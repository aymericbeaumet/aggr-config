---
title: Self-Hosting on the Dark Web
link: https://david.alvarezrosa.com/posts/self-hosting-on-the-dark-web/
source: hnrss-org
published: 2026-09-27T20:03:36Z
updated: 2026-09-27T20:03:36Z
first_seen: 2026-09-28T06:49:16.850449770Z
authors:
- mooreds
labels:
- self-hosting
content: extracted
html: 2026-09-27-self-hosting-on-the-dark-web.html
preview:
  file: 2026-09-27-self-hosting-on-the-dark-web.preview-b8dca0a200b0.webp
  width: 237
  height: 256
  alt: Self-Hosting on the Dark Web | David Álvarez Rosa
  color: '#5f5f5f'
images:
- source: https://david.alvarezrosa.com/images/home-illustration.png
  original:
    file: 2026-09-27-self-hosting-on-the-dark-web.image-80c065e4ecff.png
    width: 537
    height: 581
  variants:
  - file: 2026-09-27-self-hosting-on-the-dark-web.image-374203d80334.webp
    width: 320
    height: 346
  - file: 2026-09-27-self-hosting-on-the-dark-web.image-399ae1438441.webp
    width: 537
    height: 581
  color: '#474747'
---

This site is now reachable over Tor as a hidden service, at a `.onion` address that resolves only inside the Tor network.1 1 Open it in the [Tor Browser](https://www.torproject.org/). There is no certificate authority, no DNS, and no exposed IP—the address is derived directly from a public key, and the connection is end-to-end encrypted by Tor itself.  [Tor](https://www.torproject.org/) relays and encrypts your traffic as it passes through thousands of volunteer-run servers, so that no single party can link who you are to what you are doing; a hidden service extends that anonymity to the server itself.

It’s built by the nonprofit [Tor Project](https://www.torproject.org/), which advances human rights and freedoms through free software and open networks, so that anyone can use the internet free from tracking, surveillance, and censorship. The network only works because people use it, so consider [supporting them](https://donate.torproject.org/) or running a relay—your contribution helps millions stay safe and private online every day.

Install Tor and point a hidden service at a local port. Edit `/etc/tor/torrc`

```cfg
HiddenServiceDir /var/lib/tor/blog/
HiddenServicePort 80 127.0.0.1:8080
```

The directory must be a dedicated, Tor-owned path—not your web root.2 2 Tor stores the service’s private key and `hostname` file here and insists on owning it (`chmod 700`, user `debian-tor`). Point it at your site files and Tor refuses to start.  Restart Tor and read the address it generates

```sh
$ sudo systemctl restart tor@default
$ sudo cat /var/lib/tor/blog/hostname
dhevt6e4rtgbtr3jh53xrpwmgtilkah6nyjujocsspssrsexc7omxhid.onion
```

## Serving the site

Tor forwards the onion’s port 80 to `127.0.0.1:8080`, so the web server just needs to listen there. Add an nginx server block for it—no TLS, no HTTP/2, no QUIC, since Tor speaks plain TCP and provides its own encryption.

```nginx
server {
  listen 127.0.0.1:8080;
  server_name dhevt6e4rtgbtr3jh53xrpwmgtilkah6nyjujocsspssrsexc7omxhid.onion;

  root /srv/tor.david.alvarezrosa.com;
  index index.html;
  error_page 404 /404/index.html;

  location / {
    try_files $uri $uri/ =404;
  }
}
```

Reload nginx and the site is live on Tor.

## Building for the onion

A static site bakes its base URL into absolute links, so a clearnet build would point visitors back to the clearnet domain even when served over Tor. The fix is to build a second copy with the onion as its base URL

```sh
$ hugo --minify --baseURL="http://dhevt6e4rtgbtr3jh53xrpwmgtilkah6nyjujocsspssrsexc7omxhid.onion/"
```

The deploy pipeline does this automatically: every push builds the site once per target—clearnet and Tor—and rsyncs each to its own web root, so the two stay in sync without any manual work.3 3 See [First Steps on a New Server](https://david.alvarezrosa.com/posts/first-steps-on-a-new-server/) for the underlying machine; the full configuration lives in my [homelab](https://github.com/david-alvarez-rosa/homelab) repository, and the [site’s own repository](https://github.com/david-alvarez-rosa/personal-website) holds the GitHub Actions workflow that builds and deploys the Tor copy.

That’s it. Read this site over Tor at `dhevt6e4rtgbtr3jh53xrpwmgtilkah6nyjujocsspssrsexc7omxhid.onion`.
