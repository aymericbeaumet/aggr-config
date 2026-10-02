---
title: Git 3.0's upcoming SHA-256 default will be a costly mistake
link: https://blog.gitbutler.com/git-3-sha-256
source: hnrss-org
published: 2026-10-01T16:57:03Z
updated: 2026-10-01T16:57:03Z
first_seen: 2026-10-02T02:35:29.351097324Z
authors:
- chmaynard
labels:
- git
content: extracted
html: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.html
preview:
  file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.preview-547cef64dfbd.webp
  width: 256
  height: 183
  alt: Git 3.0's upcoming SHA-256 default will be a costly mistake
  color: '#c2bcb4'
images:
- source: https://gitbutler-docs-images-public.s3.us-east-1.amazonaws.com/git-3-sha-256.webp
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-5cf44c9959ee.webp
    width: 2040
    height: 1455
  color: '#b4e4e1'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F9ed26406-8fd8-40f6-a179-3bd86bada713.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-5f68ed7995e4.png
    width: 1536
    height: 1024
  color: '#fcfcfa'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F0f648e45-755d-4aad-bbff-a9f6a426fbf2.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-a993ed577cb2.png
    width: 1536
    height: 1024
  color: '#fcfaf6'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F37babe9e-9039-4329-a06a-18e5d1c0a7cd.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-3cef3589c1d5.png
    width: 1536
    height: 1024
  color: '#f9f9f4'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2Fgithub-sha-256.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-1cf521376dc0.png
    width: 1272
    height: 1236
  color: '#fcfcfd'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2FCleanShot%25202026-09-30%2520at%252010.53.22%2520AM%25402x.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-176224d9800c.png
    width: 1450
    height: 1094
  color: '#fdfdfd'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2Fe6868753-835d-43bc-b34e-2f922cbfdbb3.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-1340d4803b31.png
    width: 1536
    height: 1024
  color: '#f8f7f4'
- source: https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F56429e24-13e4-4f99-977a-2ed48ab4202b.png&w=3840&q=75
  original:
    file: 2026-10-01-git-3-0-s-upcoming-sha-256-default-will-be-a-costly-mistake.image-88755d815e94.png
    width: 1536
    height: 1024
  color: '#faf9f7'
---

Where to begin?

I've been sitting on this for a few years, mostly because there are smarter people who have been concentrating on this and I don't love being a back seat driver. However, I think that the Git 3.0 release is about to cost everyone a lot of time and angst for little benefit, and virtually nobody knows what's coming.

So grab some popcorn and let me tell you a tale of how one of the new upcoming Git 3.0 [breaking changes](https://git-scm.com/docs/BreakingChanges#_changes) is about to be a huge, costly, global train wreck of a change for almost no practical value.

I'll keep this short as many of you probably know this at a basic level.

Git is what's known as a content addressable database. This means that if you want to store and transmit data in it, Git will calculate a hash of the contents and use that in a key/value database as the key (the value being the content). The same content always gets the same hash, globally.

![Git's use of SHA-1 hashes as keys in it's key-value object database](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F9ed26406-8fd8-40f6-a179-3bd86bada713.png&w=3840&q=75)

Git's use of SHA-1 hashes as keys in it's key-value object database

This is nice, because it means that the same file content is never stored twice. There is also a cool property where commits encode the hash of the commit that came before it, which means that this integrity essentially propagates - you can't change the hash of anything without changing the hash of everything that comes after it. This gives it "cryptographic integrity", meaning that hashing the latest commit essentially also hashes potentially millions of file contents, trees and commits that came before it.

In Git, this hash function has always been SHA-1. This was what Linus picked in 2005 when Git was started and it's worked pretty well for 20 years - it's relatively fast and impossible in a practical sense for two different files to accidentally hash to the same value.

In fact, as far as I'm aware, this has never happened in the history of every file, tree and commit ever made in Git in every repository ever created - billions and billions of them.

Mathematically, for SHA-1’s 160-bit output, the birthday bound means that you would need about 1.4 septillion random files (1.4 quadrillion billion files - 1,400,000,000,000,000 billion - it's impossible to effectively describe) in a *single project* to have file hashes accidentally collide.

There is a problem though, which is that mathematically, SHA-1 is now considered semi-"broken" because there have been published collision attacks ([SHAttered](https://eprint.iacr.org/2017/190) in 2017, [SHA-1 is a Shambles](https://sha-mbles.github.io/) in 2020) - not really practical to exploit in any demonstrated way, but now theoretically possible.

So in response to this, after a huge amount of work by very smart people, the upcoming Git 3.0 release is planning to change it's default hashing algorithm from the semi-"broken" SHA-1 to the stronger SHA-256 algorithm.

But first, let's pause. Before we dig into this, what does "broken" mean?

This is important to understand, because it's probably not what normal people would think "broken" means. From a cryptographic hashing standpoint, broken in this sense essentially means that finding collisions *isn't impossible*.

In other words, if you throw enough money and GPUs at the problem, you can, for some content shapes, come up with two different things that hash to the exact same value, on purpose.

![What can you do with a broken SHA-lor? Earl-y in the morrrrn...](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F0f648e45-755d-4aad-bbff-a9f6a426fbf2.png&w=3840&q=75)

What can you do with a broken SHA-lor? Earl-y in the morrrrn...

This means that while it's still nearly impossible to *accidentally* have two reasonable files with different contents, it's not technically impossible to *manufacture* two different files that hash to the same thing.

This means that there theoretically exist attack vectors where someone could replace one file's contents with a malicious version and Git can't tell the difference because the hash is mathematically identical.

These papers showed that SHA-1 has a property that in theory can be exploited by modern GPU farms to produce purposeful collisions on the order of a few tens of thousands of dollars today that SHA-256 does not have ([google "linear message schedule sha-1 vs sha-256"](https://www.google.com/search?q=linear+message+schedule+sha-1+vs+sha-256) if you super-duper care...).

Sounds really scary and concerning, right? *Won't somebody please think of the children!?!*

Well, not really.

But let's take one minute to step back and talk about these collisions.

There are two main problems with hash functions when you're considering attacks on them (assuming I had to massively, stupidly, simplify things). One is a "collision attack" and the other is a "second-preimage attack".

A "collision attack" is when you know you're going to be an attacker but pretend to be a good guy until you gain trust. You generate two files on purpose with the same hash - one is benign and the other is malicious. You give people the benign one until you're trusted and then switch it with the malicious one because the hash matches and you know Git can't tell the difference. You can even get signed tags or commits on trees that have the benign file and make it look like the bad file was signed.

A "second-preimage attack" is when you see a file you want to replace and then create a second file that does something malicious that also *happens* to match that hash so you can get people to unknowingly pull it down instead. Importantly, this does *not* mean that the author of the original file needs to also be the attacker.

![Collision versus second-preimage](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F37babe9e-9039-4329-a06a-18e5d1c0a7cd.png&w=3840&q=75)

Collision versus second-preimage

So, the most important thing that I would like to emphasize before this rant begins is that while second-preimage attacks have a more concerning aspect to them, nearly no widely used hash function ever used is susceptible to it.

Git could be using MD5 (considered an incredibly broken hash function) and still be effectively immune from a second-preimage attack. Again, MD5 is what is considered a *completely "broken"* hash function, which SHA-1 is not - it's much stronger.

What do I mean by "effectively immune"? If every one of the roughly 3 billion GPUs on Earth were magically replaced with an RTX 5090, and every one spent 100% of its time doing nothing but MD5, the expected time to brute-force a particular preimage would still be about 16 billion years (~11 billion median), or roughly the age of the universe.

3.4× 1038 checks 3× 109 GPUs × 2.2× 1011 hashes/sec ≈ 5.2× 1017 seconds ≈ 16 billion years

So, any realistic interesting attack vector therefore relies on a collision attack, meaning the person who introduces the original file has also pre-computed the malicious version and intends to inject it after it's accepted.

However, let's be massively, unrealistically conservative and even assume for argument's sake that a *preimage* is *easy*. Let's say that you found a way to create a second preimage matching the hash of any known file on your laptop in an hour.

Now you can target any file you want to replace and come up with a different malicious file that matches the hash effortlessly. Congratulations! You can now pwn any codebase on earth!

Egh. Hm, one tiny problem. How do you:

1. get that file to be fetched by people who don't know you and have never pulled a previous version of this content
2. get them to run it in a way that is useful to you

Every argument and problem set after this depends on the answers to these questions, yet the answers to these questions are generally not part of the conversation around this problem.

Even if we assume that SHA-1 was trivial to break, even if we assume that second-preimage production was possible (or even cheap), it does not make the attack vectors that are available actually very easy to exploit.

The reason why is that hashing is not *really* the mechanism of trust in the SCM world. It certainly provides an element of cryptographic integrity, but it is not what *trust* is fundamentally based on.

Linus literally [argues this](https://lore.kernel.org/git/Pine.LNX.4.58.0504291221250.18901@ppc970.osdl.org/) at the birth of Git.

> I really think people should not consider the sha1 the "security". The real security is in distribution.

[Linus Torvalds, 2005](https://lore.kernel.org/git/Pine.LNX.4.58.0504291221250.18901@ppc970.osdl.org/)

Trust is based on "*where do you pull from*?" and it always has been.

For one minute, let's consider what an *actual attack* looks like in the world of getting untrusted content into codebases. This is, of course, the worst case scenario of what we're looking at here. Some source code file (or more likely in a hash attack case, binary file) is inserted maliciously without you knowing.

It turns out, this actually happens a fair amount.

Not because people are spending hundreds of thousands of dollars on GPUs to generate random entropy to throw in after a null byte, so that some binary file can happen to match a SHA-1 checksum.

No, it currently happens in the real world because someone socially engineers an exploit to get write access to some npm package used by millions of projects. Now it's not one binary that's difficult to detect, it's every single file in a dependency that is blindly pulled in with no previous checksumming match by every project that has this project in its `package.json` file.

This is maybe a *billion* times simpler, cheaper and more likely to succeed than trying to brute force a hash collision and manipulate an untrusted fetch.

If I wanted to get untrusted code into Android, it's *so much* simpler to bribe or convince the maintainer of a popular downstream project, take it over and inject difficult to detect code into an already trusted source URL than try to engineer some *easy to detect* hash collision and put it into a URL that *nobody would ever pull from*.

Do you think there is no tired open source maintainer who wouldn't give over maintenance of a highly used, unloved project for a $40k lump sum payment? Voila, now you don't need to rent GPUs, it takes one day and you can replace any file you want with any content you wish.

In other words, hash collision attacks are maybe the dumbest possible way to get untrusted code on a system when unpaid open source maintainers and low-trust package management forges exist.

To go back to Linus's argument, I don't pull code from [https://github.com/rust-lang/rust](https://github.com/rust-lang/rust) because I trust the GPG signature that signed the latest commit SHA and just assume that any random source is fine.

I pull it from there because I trust that GitHub has its authentication game together enough that it's unlikely that anyone malicious pushed something there without the maintainer's knowledge. Which is also why I don't pull from [https://randomhash.onion/hAAAxx0r/rust](https://randomhash.onion/hAAAxx0r/rust) because some email told me to.

I don't really care what the hashing algorithm is and how many fewer billions of years of compute it might have taken to generate a hash collision on a large binary file that Rust clearly would not have committed to the repository in the first place.

It's just honestly a ridiculous premise.

Every attack scenario I'm aware of in defense of the incredible and expensive SHA-256 migration effort that the entire industry is about to be forced to undertake is unrealistic to me. They are all easily and cheaply mitigated by many other stronger, simpler protections of sources and content rather than slightly stronger hash algorithms.

If we consider SHA-1 to simply be a good enough, fast enough method to generate unique keys for content in a trusted repository, and accept that the hashes are [not intrinsically to be used for trust itself](https://lore.kernel.org/git/Pine.LNX.4.58.0504291221250.18901@ppc970.osdl.org/), then there is no real reason to replace it. We could be using MD5 and it would honestly probably be just fine.

Accidental content collisions are mathematically nearly impossible in any reasonable codebase and the rest can be handled by signed content verification, external authentication and social trust mechanisms.

If we don't accept this, then we will be stuck in a loop of someone finding a theoretical collision exploit and again needing to migrate the entire Git ecosystem every time a new paper is published. We can go through years of this SHA-1 to SHA-256 migration and then quantum computers break 256 and we're back in the same stupid boat again.

Simply because we're conflating cryptographic integrity with trust.

Now hold on a minute Scott, you said that this is going to be a horrible train wreck. Surely that's hyperbole.

Probably.

But no matter how you calculate the cost, it won't be cheap or easy. Emily Shaffer just gave [a talk](https://www.youtube.com/watch?v=eJJp0RE7cd4) about how Google is preparing to deal with this problem and it's not a pretty picture.

Let's break down what is going to happen when Git 3.0 ships with this new default.

The first thing is that all new repositories created with `git init` will be created with the SHA-256 content hashing algorithm. You can try this yourself today by running `git init --object-format=sha256`.

```
❯ git init --object-format=sha256 /tmp/twofiddy
Initialized empty Git repository in /private/tmp/twofiddy/.git/

❯ cd /tmp/twofiddy

❯ echo 'sha 256' > README.md

❯ git add README.md; git commit -am 'first'
[main (root-commit) 11043f6] first
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

❯ git log
commit 11043f6a3be7d21e999dc84550886306bee65f4faf4fc9226979108fd1a0b1af (HEAD -> main)
Author: Scott Chacon <schacon@gmail.com>
Date:   Wed Sep 30 10:36:13 2026 +0200

    first
```

The first thing that you'll notice (other than the much longer hash value) is that you can't push this code to GitHub, though that will almost certainly be fixed by the time Git 3.0 is released. In fact, that's probably the main thing currently delaying 3.0 entirely.

But when you do want to push it to GitHub (or any host), you will need to tell them when creating the repository on the server that this is a sha256 project. Every project will now be in one bucket or the other and they cannot be mixed.

This is immediately going to frustrate people because now they need to *know* what version of git they ran `git init` with and make sure when they go to GitHub to create the server repository, they choose the right one.

![New repo creation on forges will need to look something like this, but how do you know which you need? Also, what normal person would know what this means or that it's _insanely_ important which you choose?](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2Fgithub-sha-256.png&w=3840&q=75)

New repo creation on forges will need to look something like this, but how do you know which you need? Also, what normal person would know what this means or that it's \_insanely\_ important which you choose?

We are about to see a *lot* of this:

```
❯ git push origin main
fatal: the receiving end does not support this repository's hash algorithm
```

For a couple of different reasons. Because depending on how hosts label this, it's very difficult to know which format you need the upstream repo initialized as. So you could do 256 upstream and try to push one initialized locally with a pre-3.0 binary. Or clearly vice versa.

### Libraries, Links, and Tools, Oh My!

Furthermore, for libraries, this is *really* problematic, because submodules can only be used with projects of the same type, so they will need to have two versions in order to be used both by existing projects and new ones.

It's possible that forges will be able to handle both formats by keeping mirrors of the other format, but this will increase load on sites like GitHub for nearly every operation *and* also further confuse the trust problem.

For *existing* projects that do decide to convert from SHA-1 to SHA-256, they will need to convert every object in the project to the new format, which will break all existing signatures. Also everyone working on or using that project will need to switch over at the same time to avoid split head issues. Or, again, have the mirror setup and cut over write access from one to the other. Which still does not solve the object replacement problem for other mirrors that do not have the 256 version.

If they *do* convert, then all existing URLs or links in Slack or emails or anything that ever had a SHA-1 hash in it will no longer work and will need to be redirected (assuming that's possible - you haven't switched hosts or something to a place that doesn't have a mapping).

All internal or other tooling globally that expects 40 characters for the hash will break or need to be updated to guess or detect the hash format.

Also, while core Git has this functionality to work with repositories of either format, not all libraries do and nearly none of them have full support.

Since Git was developed as a project with a non-reentrant, GPL licensed, unlinkable library, most projects in the wider Git ecosystem use from-scratch reimplementations. Since those libraries all have zero or partial support, all scripts and tooling that are not fork-exec'ing out to the Git binary will have some amount of breakage on these new repositories.

![If you have any tool that depends on any of these libraries, you may be out of luck for some operations](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2FCleanShot%25202026-09-30%2520at%252010.53.22%2520AM%25402x.png&w=3840&q=75)

If you have any tool that depends on any of these libraries, you may be out of luck for some operations

I can go on, but this is a huge, wide-ranging and still largely unsolved problem and there is no consensus or great solution that anyone has come up with to make this easy. Emily even noted that Google may just set internal system-wide overrides to make sure all new projects in Google are still SHA-1, attempting to make sure everything reverts the new 3.0 default for as long as possible.

Personally, I think this is an unnecessary solution looking for a theoretical problem and even that theoretical, impractical problem can be solved in a much simpler and straightforward way for the small number of projects that might actually care.

**Don't use the hash to trust content.**

You know, like Linus said 20 years ago.

If you assume that your repository sources are trusted, then none of this matters. At all. And 99% of us only work with repository sources that are trusted. For nearly everyone using Git, fancy backdoor hash collision attacks are irrelevant because if you're only working on one repository and if write access to that is compromised, anyone can put anything on the main branch and probably nobody will notice. You don't need fancy object replacement tricks to hoodwink people.

If you don't fully trust your sources (the other 1%) then perhaps there are other, simpler, better approaches we should try before our global Hashmageddon.

What if we approach this very differently? Perhaps we do something simple like independently rehash the tree contents with a different algorithm, then inject that header into the objects we sign so that we sign *both* hashes? One (SHA-1) to be used for content retrieval and the other (SHA-256 let's say) to be used for independent content verification.

I want to dig into this a bit, because I feel that an approach like this can solve nearly all of the *even theoretical* problems without having to bifurcate the entire Git ecosystem and frustrate everyone.

Let's say that you want to rely on an external library or vendor and want to mitigate possible object collision attacks. You can't trust the SSH/GPG signature on a commit or tag because what it's signing is the SHA-1 hash we use to store the content and that is shown to be replaceable without detection or changing the thing you're signing.

So separate them.

Independently calculate the SHA-256 (or BLAKE3 or whatever) hash of all of the content in the tree when you sign the commit or tag and inject that as a new header in that object that is then signed. Now the signature hashes *both* the SHA-1-based content and history and *additionally* the hash of the tree contents independently calculated at signing time.

![independent tree content hash in the signed fields of Git objects](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2Fe6868753-835d-43bc-b34e-2f922cbfdbb3.png&w=3840&q=75)

independent tree content hash in the signed fields of Git objects

Now when you pull that into another project, you have the option of verifying the signature via a public key and making sure that the content you check out matches both signatures.

This isn't a new idea. Colin Walters' [git-evtag](https://github.com/cgwalters/git-evtag) has done almost exactly this since 2015: it's a drop-in replacement for `git tag -s` that adds a `Git-EVTag-v0-SHA512` checksum over the commit, tree and every blob (recursing into submodules) to the tag before it's signed that can be verified independently from the SHA-1 of the same tree.

If SHA-256 is ever compromised, we can add support for a new hash function and projects that care can start requiring that (a `tree-blake3` header or whatever). We can even do this with multiple hashes and verify none, any or all of them for content trust. Every time trust in an algorithm is compromised, all we have to do is add a new hashing and verification method, not migrate every project in existence.

The downsides would be that we may not be able to trust *every* commit, only the objects that have the header in them and are signed, but for nearly every project that cares, this is probably fine. Trust in a signature in this case does not propogate down the entire history now, but how big of a problem is that, really? Projects of this nature are almost certainly pegging to tagged releases anyhow.

It would be slightly more expensive to create this independent content signature, but only when you want to sign something. I implemented a [proof of concept](https://github.com/schacon/tree-sha256) for this and ran it against the absolute worst case I can imagine - Chromium with all submodules recursively checksummed.

In this case, we're dealing with a 35GB working tree of 2.1M files. My tool generated a checksum (all main tree files, all submodule files) in 5 seconds (M5 Mac multithreaded). This is pretty much the absolute worst case scenario.

The Linux tree takes 257ms for its 1.5GB tree. The Git project takes 17ms. You could even put this in every commit for most projects. It's also backfillable. You could go back in time and throw signed, checksummed tags on commits in the past for any project, pretty easily.

![It's not very expensive to verify tree content independently](https://blog.gitbutler.com/_next/image?url=https%3A%2F%2Fd2m1ukvwmu7gz4.cloudfront.net%2F56429e24-13e4-4f99-977a-2ed48ab4202b.png&w=3840&q=75)

It's not very expensive to verify tree content independently

Every tree that anyone cares about could be independently checksummed and signed with no change in the core hashing model. If anything, it would technically be more secure than SHA-256 alone, since now you would need to get content that collides in *both* hash algorithms to be a problem.

We can stop using SHA-1 to trust content without throwing away SHA-1 and breaking the entire ecosystem. We can add another content trust vector without causing an immense amount of chaos and confusion for a tiny minority of projects with this trust issue.

Thank you for coming to my TED Talk.

This second-signature approach also arguably addresses the NIST [compliance](https://lore.kernel.org/git/YJm23HESQb1Z6h8y@camp.crustytoothpaste.net/) [issue](https://lore.kernel.org/git/011f01d88571$755df480$6019dd80$@nexbridge.com/). [NIST's](https://lore.kernel.org/git/d0ec38a25ab4fbdcb8ffa726931ead3fc68662b4.1718345026.git.ps@pks.im/) 2030 SHA-1 deadline is about using SHA-1 ["for applying cryptographic protection"](https://csrc.nist.gov/news/2022/nist-transitioning-away-from-sha-1-for-all-apps), not about SHA-1 existing anywhere in your stack. If every signature also covers a SHA-256 content hash, then SHA-1 isn't protecting anything anymore. It's just a content-addressed key. FIPS-mode systems already handle this: OpenSSL 3 lets an application ask for a non-FIPS implementation of a hash it isn't using for security (that's what Python's `usedforsecurity=False` does), and Git doesn't even go through OpenSSL for object IDs. It uses its own built-in SHA-1 code, which sits outside any FIPS crypto module entirely.

I could also take this arguemnt even further and entirely remove the sha1dc ("collision detection") computational tax that we all pay to try to avoid collisions of these specific types. We could speed up clones and pushes in many cases if we assume that this is not where this kind of check needs to go.
