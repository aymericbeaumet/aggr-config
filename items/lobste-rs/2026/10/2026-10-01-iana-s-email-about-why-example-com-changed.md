---
title: IANA's email about why example.com changed
link: https://www.oliverdunk.com/2026/09/30/iana-reply
source: lobste-rs
published: 2026-10-01T08:47:50Z
updated: 2026-10-01T08:47:50Z
first_seen: 2026-10-01T19:33:44.897168732Z
authors:
- oliverdunk.com via videah
labels:
- networking
- web
summary: Comments
content: extracted
html: 2026-10-01-iana-s-email-about-why-example-com-changed.html
preview:
  file: 2026-10-01-iana-s-email-about-why-example-com-changed.preview-fb2fc4d09c7f.webp
  width: 256
  height: 256
  alt: Oliver Dunk
  color: '#877f7c'
images:
- source: https://www.oliverdunk.com/assets/avatar.png
  original:
    file: 2026-10-01-iana-s-email-about-why-example-com-changed.image-c2d45a04af6b.png
    width: 1440
    height: 1442
  color: '#a59989'
---

I emailed IANA (Internet Assigned Numbers Authority) earlier today to ask about the new content on [example.com](https://example.com). To my surprise, their VP replied. Here is the email:

> I am happy to try and answer any questions you have.
>
> The changes we make to the site from time-to-time are usually informed by a desire to either reduce the overall bandwidth demands of serving the site, or improve its utility. At its heart it is a placeholder for a site no-one should be accessing, but we have historically showed a message there to inform visitors about why the domain is registered and how they may use it.
>
> As it tends to be heavily trafficked, the overall bandwidth is a key consideration, and to that end one of the changes we made this week was to split the page content into a more basic page, augmented by a separate Javascript file with additional content beyond that. Since most of the traffic to the site is automated it tends not to fetch the Javascript file, reducing overall data requirements.
>
> The fundamental purpose of the domain is for use in documentation, such as illustrative examples you may find in instructions. We know that people may, for example, cut-and-paste a sample configuration file and forget to customize entries that contain an example domain, and we expect we get some incidental traffic from that. However, the domain isn't intended to be a general purpose endpoint for things like availability testing so we do not encourage that kind of usage. There is no requirement there is a HTTP service present on the host in order to fulfill its purpose, we just operate it as a courtesy.
>
> Hope this is helpful. Let us know if you have any questions. You're free to use and recirculate this information as you see fit.
>
> Kim Davies
>
> Internet Assigned Numbers Authority

30 Sep 2026
