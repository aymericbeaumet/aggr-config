---
title: “They had no concept of a duty of care to their users.”
link: https://unsung.aresluna.org/they-had-no-concept-of-a-duty-of-care-to-their-users/
source: lobste-rs-top-1w
published: 2026-09-27T14:44:53Z
updated: 2026-09-27T14:44:53Z
first_seen: 2026-09-27T19:16:58.479681848Z
authors:
- unsung.aresluna.org via raymii
labels:
- vim
summary: Comments
content: extracted
html: 2026-09-27-they-had-no-concept-of-a-duty-of-care-to-their-users.html
preview:
  file: 2026-09-27-they-had-no-concept-of-a-duty-of-care-to-their-users.preview-e9e224e08910.webp
  width: 256
  height: 132
  color: '#eefbf3'
images:
- source: https://unsung.aresluna.org/_media/they-had-no-concept-of-a-duty-of-care-to-their-users/ogimage.jpg
  original:
    file: 2026-09-27-they-had-no-concept-of-a-duty-of-care-to-their-users.image-9eafed703aa9.jpg
    width: 1400
    height: 720
  color: '#f5fefb'
---

A [Mastodon post by computer scientist David Chisnall](https://mastodon.online/@david_chisnall@infosec.exchange/117171776561892821) has a very Unsung opener:

> I have used vim since around 2000. I have written five books, a PhD thesis, a few dozen papers and over 150 articles with it. At this point, my higher brain functions are not engaged at all when I use a bunch of common vim commands, they just happen. Documents I wrote with anything else have random :w in the middle.

Chisnall goes on to talk about one specific [vim](https://en.wikipedia.org/wiki/Vim_\(text_editor\)) feature:

> Persistent undo is one of my favourite features of vim. \[…\]
>
> I don’t often need the persistent undo. But on the few occasions when I have needed it, it’s been invaluable: ooops, I deleted something from this file, maybe last week and one reboot ago, what was it? Undo until I find it, copy it, paste it into the current version. Or, a bit more commonly: I had this working, then I tidied it up ready to commit, now it isn’t working, what did I do?
>
> Vim has kept this working across major version upgrades over a period of about 20 years. I don’t even think about it, it’s just part of Raskin’s First Law: A program may not harm a user’s data or, through inaction, allow a user’s data to come to harm. If vim or the computer crash, or if I close a file and come back to it six months later, my undo history is still there.

NeoVim is a fork of vim (in news [for other reasons](https://github.com/neovim/neovim.github.io/issues/501)):

> So I tried NeoVim when it was quite new. Vim that you are familiar with, but better? Great!
>
> The first thing I noticed in NeoVim was that undo didn’t work. I tried opening the file in vim and undo didn’t work there *either*.
>
> Neovim had changed the format of the undo files. It hadn’t upgraded the old one. It hadn’t used a different name for its undo files. It had just noticed the existence of a vim undo file, deleted it (losing all of the data in it) and replaced it with one that vim couldn’t read.
>
> I raised an issue about this and was told that the persistent undo format was unstable and users should not rely on data being preserved in a feature explicitly called *persistent* undo. It had changed once and would probably change again.
>
> And that ended my experience with NeoVim. The authors showed immediately that they absolutely could not be trusted with any of my data. Breaking persistent undo is something I could forgive as a bug, but the attitude that just because something is a persistent file on your filesystem that contains data that you might want is no reason for their program not to delete it meant they had no concept of a duty of care to their users.

I liked this post (which I quoted almost in its entirety), because it covers a few important things:

- I have never heard of the persistent undo like that, and it seems kind of amazing.
- People do remember when software loses their hard work or disrespects them.
- I can see how “It had changed once and would probably change again” can be such a powerful feeling.

I also loved it for the appearance of Raskin’s First Law. Jef Raskin, of Macintosh and Canon Cat fame, put together the three laws in his 2000 book [The Humane Interface](https://en.wikipedia.org/wiki/The_Humane_Interface), and they go as follows:

- A computer shall not harm your work or, through inaction, allow your work to come to harm.
- A computer shall not waste your time or require you to do more work than is strictly necessary.
- An interface is humane if it is responsive to human needs and considerate of human frailties.

It was a very important and formative book for me to encounter as a young designer. I have no idea how these laws haven’t made it to Unsung before today.
