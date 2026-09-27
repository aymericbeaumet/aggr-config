---
title: The Cathedral, the Bazaar, and the Winchester Mystery House
link: https://www.dbreunig.com/2026/03/26/winchester-mystery-house.html
source: dbreunig-com
published: 2026-03-27T00:13:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- dev tools
- software engineering
summary: The internet made coordination cheap and gave us the Bazaar. Coding agents make code cheap and are giving us sprawling, idiosyncratic software.
content: extracted
html: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.html
preview:
  file: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.preview-018e5cb5433b.webp
  width: 256
  height: 209
  color: '#8f8c7f'
images:
- source: https://www.dbreunig.com/img/winchester.jpg
  original:
    file: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.image-8beca4e765c2.jpg
    width: 1760
    height: 1434
  color: '#cde5f2'
- source: https://www.dbreunig.com/img/winchester_02.jpg
  original:
    file: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.image-61e88bcdbe45.jpg
    width: 2050
    height: 1150
  color: '#76bbeb'
- source: https://www.dbreunig.com/img/winchester_03.jpg
  original:
    file: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.image-c05d24b18d0a.jpg
    width: 1567
    height: 799
  color: '#87a7e3'
- source: https://www.dbreunig.com/img/crowded_bazaar.jpg
  original:
    file: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.image-971c6c83445e.jpg
    width: 1200
    height: 549
  color: '#756758'
- source: https://www.dbreunig.com/img/resized/agent_sharing.jpg
  original:
    file: 2026-03-27-the-cathedral-the-bazaar-and-the-winchester-mystery-house.image-a4526aadf27c.jpg
    width: 1600
    height: 846
  color: '#f3eee8'
extra:
  thumbnail: https://www.dbreunig.com/img/winchester.jpg
---

### Our era of sprawling, idiosyncratic tooling

![](https://www.dbreunig.com/img/winchester.jpg)

In 1998, Eric S. Raymond published the founding text of open source software development, “[The Cathedral and the Bazaar](http://www.catb.org/~esr/writings/cathedral-bazaar/)”. In it, he detailed two methods of building software:

- *The Cathedral* model is carefully planned, closed-source, and managed by an exclusive team of developers.
- *The Bazaar* model is open, transparent, and community-driven.

The Bazaar model was enabled by the internet, which allowed for distributed coordination and distribution. More people could contribute code and share feedback, yielding better, more secure software. “Given enough eyeballs, all bugs are shallow,” Raymond wrote, coining [Linus’ Law](https://en.wikipedia.org/wiki/Linus%27s_law).

The ideas crystallized in “The Cathedral and the Bazaar” helped kick off a quarter-century of open source innovation and dominance.

But just as the internet made communication cheap and birthed the Bazaar, AI is making code cheap and kicking off a new era filled with idiosyncratic, sprawling, cobbled-together software.

Meet the third model: *the Winchester Mystery House*.

* * *

![https://www.flickr.com/photos/harshlight/3669393933](https://www.dbreunig.com/img/winchester_02.jpg)

### The Winchester Mystery House

Located less than 10 miles southeast from the [Computer History Museum](https://computerhistory.org), the [Winchester Mystery House](https://en.wikipedia.org/wiki/Winchester_Mystery_House) is an architectural oddity.

Following the death of her husband and mother-in-law, Sarah Winchester controlled a fortune. Her shares in the [Winchester Repeating Arms Company](https://en.wikipedia.org/wiki/Winchester_Repeating_Arms_Company), and the dividends they threw off, made it so Sarah could not only live in comfort but pursue whatever passion she desired. That passion was architecture.

Sarah didn’t build her mansion to house ghosts[^1], [she built her mansion because she liked architecture](https://amzn.to/4rZK1C8). With no license, no formal training, in an era when women (even very rich women) didn’t have a path to practicing architecture, Sarah focused on her own home. She made up for her lack of license with passion and effectively unlimited funds.

Sarah built what she wanted. “[At its largest the house had ~500 rooms](https://en.wikipedia.org/wiki/Winchester_Mystery_House).” Today it has roughly 160 rooms, 2,000 doors, 10,000 windows, 47 stairways, 47 fireplaces, 13 bathrooms, and 6 kitchens. Carved wood drapes the walls and ceilings. Stained glass is everywhere. Projects were planned, completed, abandoned, torn down, and rebuilt.

It was anything but aimless. And practical innovations ran throughout, including push-button gas lighting, an early intercom system, steam heating, and indoor gardens. The oddities that amuse today’s visitors were mostly practical accommodations for Sarah’s health (stairways with very small steps), functional designs no longer used (trap doors in greenhouses to route excess water), or quick fixes to damage from the 1906 earthquake.

Winchester passed in 1922. Nine months later, the house became a tourist attraction.

Today, many programmers are Sarah Winchester.

* * *

Claude Code's Public GitHub Activity

 Lines Added Lines Deleted

### What Happens When Code is Cheap

We aren’t as rich as Sarah Winchester, but when code is this cheap, we don’t need to be.

Jodan Alberts illustrated this recently, [collecting and visualizing data detailing public Github commits attributed to Claude Code](https://www.claudescode.dev). That’s his data in the chart above, with Claude seeming to only accelerate through March[^2].

It’s hard to get a handle on individual usage, though, so I went searching for a proxy and landed on the chart below:

Average Net Lines Added Per Commit in Claude Code

 7-Day Average

After Opus 4.5 and recent work enabling Agent Teams, the average net lines added by Claude per commit is now smooth and steady at *1,000 lines of code per commit*[^3].

**1,000 lines of code per commit is ~2 magnitudes higher than what a human programmer writes *per day*.**

If you search for human benchmarks, you’ll find many citing Fred Brooks’ *[The Mythical Man Month](https://web.eecs.umich.edu/~weimerw/2018-481/readings/mythical-man-month.pdf)* while claiming a good engineer might write *10 cumulative lines of code per day*[^4]. If you further explore, you’ll find numbers higher than 10 cited, but generally less than 100.

Here’s a good anecdote from [antirez](https://antirez.com/latest/0) on a [Hacker News](https://news.ycombinator.com/item?id=22305934) thread discussing the Brooks “quote”:

> I did some trivial math. Redis is composed of 100k lines of code, I wrote at least 70k of that in 10 years. I never work more than 5 days per week and I take 1 month of vacations every year, so assuming I work 22 days every month for 11 months:
>
> *70000/(22 x 11 x 10) = ~29 LOC / day*
>
> Which is not too far from 10. There are days where I write 300-500 LOC, but I guess that a lot of work went into rewriting stuff and fixing bugs, so I rewrote the same lines again and again over the course of years, but yet I think that this should be taken into account, so the Mythical Man Month book is indeed quite accurate.

6 years after this comment, Claude is pushing *1,000* lines of code *per commit*.

* * *

So what do we do with all this cheap code?

Unfortunately, everything else remains roughly the same cost and roughly the same speed. Feedback hasn’t gotten cheaper; the “[eyeballs](https://en.wikipedia.org/wiki/Linus%27s_law)” that guided the software developed by the bazaar haven’t caught up to AI.

There is only one source of feedback that moves at the speed of AI-generated code: yourself. You’re there to prompt, you’re there to review. You don’t need to recruit testers, run surveys, or manage design partners. You just build what you want, and use what you build.

And that’s what many developers are doing with cheap code: building idiosyncratic tools for ourselves, guided by our passions, taste, and needs.

Sound familiar?

* * *

![https://commons.wikimedia.org/wiki/File:Winchester_Mystery_House_2023-07-17_02.jpg](https://www.dbreunig.com/img/winchester_03.jpg)

### Welcome to the Mystery House

Steve Yegge’s [Gastown](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) is a Winchester Mystery House. It’s *incredibly* idiosyncratic and sprawling, rich with metaphors and hacks. It’s the perfect tool for Steve.

Jeffrey Emanuel’s [Agent Flywheel](https://agent-flywheel.com/) is a Winchester Mystery House. A significant subset of [tokenmaxxers](https://www.nytimes.com/2026/03/20/technology/tokenmaxxing-ai-agents.html) decide they need to rebuild their dependencies in Rust; Jeff is one such example. His “[FrankenSuite](https://github.com/Dicklesworthstone#the-frankensuite)” includes Rust rewrites of SQLite, Node, btrfs, Redis, Pandas, NumPy, JAX, and Torch.

Philip Zeyliger noted the pattern last week, writing, “[Everyone is building a software factory](https://blog.exe.dev/bones-of-the-software-factory).” But it goes beyond software. Gary Tan’s personal AI committee [gstack](https://github.com/garrytan/gstack) is a Winchester Mystery House constructed mostly from Markdown.

Everywhere you look, there are Winchester Mystery Houses.

Each Winchester Mystery House is **idiosyncratic**. They are highly personalized. The tightly coupled feedback loop between the coding agent and the user yields software that reflects the developer’s desires. They usually lack documentation. To outsiders, they’re inscrutable.

Winchester Mystery Houses are **sprawling**. Guided by the needs of the developer, these tools tend to spread out, constantly annexing territory in the form of new functions and new repositories. Work is almost always additive. Code is added when it’s needed, bugs are patched in place, and countless appendages remain. There’s little incentive to prune when code is free.

And building a Winchester Mystery House should be **fun**. Coding agents turn everything into a sidequest, and we eagerly join in. Building the perfect workflow is a passion for many devs, so we keep pushing.

Winchester Mystery Houses are idiosyncratic, sprawling, and fun. But does this mean we’re abandoning the bazaar?

* * *

![https://www.flickr.com/photos/ifpri/4860343116](https://www.dbreunig.com/img/crowded_bazaar.jpg)

### What Happens to the Bazaar?

What happens when we all tend to our Mystery Houses? When our free time is spent building tools just for ourselves, will we stop working on shared projects? Will we abandon the bazaar?

Probably not. The bazaar is *packed* right now, but not in a good way.

Code is cheap, so people are slamming open source repositories with agent-written contributions, in an attempt to pad their resumes or manifest their pet features. Daniel Stenberg [ended bug bounties for curl](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/) after a deluge of poor submissions sapped reviewer bandwidth. It’s gotten so bad, [Github recently added a feature to disable pull request contributions](https://github.blog/changelog/2026-02-13-new-repository-settings-for-configuring-pull-request-access/).

Anecdotally, I’m seeing good contributions pick up as well. They’re just drowned out by the slop. For what it’s worth, [curl commits are dramatically *up* in the agentic era](https://github.com/curl/curl/graphs/contributors). And people *are* sharing what they build. A [recent analysis by Dumky](https://www.dumky.net/posts/youre-right-to-be-anxious-about-ai-this-is-how-much-we-are-building/) shows more packages and repos rising in the last quarter.

![](https://www.dbreunig.com/img/resized/agent_sharing.jpg)

There’s plenty of budget for both Mystery Houses and the bazaar when code is *this* cheap. The new challenge is developing systems and processes for managing the deluge. We don’t need [eyeballs](https://en.wikipedia.org/wiki/Linus%27s_law) to find bugs *in* the software, we need eyeballs to find bugs before they *reach* the software.

In many ways this is the inverse of the bazaar model era. The internet made feedback and communal coordination faster, easier, and cheaper. The bazaar model has a high throughput of feedback (many eyeballs) but relatively high latency for modifications (file an issue, discuss, submit a PR, wait for review, etc.)

Coding agents, on the other hand, make implementation faster while feedback and coordination are unchanged. The Winchester Mystery House model sidesteps this by collapsing the feedback loop into one person: latency is near zero, but throughput is just you. The bazaar, defined by communal work, can’t adopt this hack. Coding agents in the bazaar create a mess: implementation at machine speed hitting coordination infrastructure built for human speed. Which is why maintainers feel like they’re drowning.

We need new tools, skills, and conventions.

* * *

### Lessons from the Mystery House

Coding agents have dropped the cost of code so dramatically, we’re entering a new era of software development, the first change of this magnitude since the internet kicked off open source software. Change arrived quickly, and it’s not slowing down. But in reviewing the Winchester Mystery House framework, I think we can take away a few lessons.

**Lesson 1: The bazaar and Winchester Mystery Houses can coexist.**

When listing example Winchester Mystery Houses, I didn’t mention [OpenClaw](https://github.com/openclaw/openclaw), even though it is *the* defining example. I saved it for here because it nicely illustrates how Winchester Mystery Houses and the bazaar can coexist.

OpenClaw is incredibly modular and places few limitations on the user. It integrates 25 different chat and notification systems, plugs into most inference end points, and is built on the exceptionally flexible [pi](https://github.com/badlogic/pi-mono) agent toolkit. This eager flexibility was embraced early – security and data protections be damned – but since its exponential adoption Peter Steinberger and the community have been steadily pushing improvements and fixes.

And like other breakout open source projects of yore, the ecosystem is adopting the best ideas and mitigating the worst aspects of OpenClaw. Countless alternate “claw” projects have emerged (there’s NanoClaw, NullClaw, ZeroClaw, and more!) Companies have launched services to make claws easy or safer. Cloudflare launched Moltworker to make deploy easy, Nvidia shipped NemoClaw with a security focus, and Claude keeps adding claw-like features to its desktop app.

**Lesson 2: Don’t sell the fun stuff.**

One reason OpenClaw works so well in the bazaar is that it is a *foundation for personal tools.* Out of the box, a claw just sits there. It’s up to the user to determine what it does and how it does it, leveraging the connections and infrastructure OpenClaw provides. OpenClaw lets less experienced developers spin up their own Winchester Mystery Houses, while experienced devs get to leverage much of the common integrations and systems OpenClaw provides. Peter and team have done a great job drawing a line between the common core (what the bazaar works on) and what they leave up to the user: the boring, critical stuff is the job of the commons.

Thinking back to Sarah Winchester and her idiosyncratic, sprawling mansion, we see the same pattern. Sarah hired vendors! She used off-the-shelf parts! Her bathtubs, toilets, faucets, and plumbing weren’t crafted on site.

The boring stuff, the hard bits, or the things that have *disastrous* failure modes are the things we should collaborate on or employ specialists to handle. (Come to think, plumbing checks all three boxes). This is the opportunity for open source software, dev tools, and software companies.

Don’t try to sell developers the stuff that’s fun, the stuff they *want* to build. Sell them the stuff they avoid or don’t want to take responsibility for. Sarah Winchester didn’t hire metalworkers to craft the pipes for her plumbing, but she *did* hire craftspeople to create hundreds of stained-glass windows to her specs.

**Lesson 3: The limits of code are communication.**

OpenClaw shows the bazaar remains relevant, but also highlights the problems facing open source in the agentic era. Right now, there are 1,173 open pull requests and 1,884 new issues on the [OpenClaw repo](https://github.com/openclaw/openclaw/pulse).

There is more code and more projects than we could ever review. The challenge now, for open source maintainers and users, is sifting through it all. How do we find the novel ideas that *everyone* should adopt and borrow?

OpenClaw is one of the successes, something we *all* noticed. And for it, the problem is processing the feedback. For the projects we’ll never find, the ones lost in the deluge, their problem is lack of feedback. You either find attention and drown in contributions or drown in the ocean of repos and never hear a thing.

The internet made coordination cheap and gave us the bazaar. Coding agents made implementation cheap and gave us the Winchester Mystery House. What we’re missing are the tools and conventions that make attention cheap, that let maintainers absorb contributions at machine speed and let good ideas surface among the noise. Until we figure this out, the bazaar will keep getting louder without getting smarter, and the best ideas in our Mystery Houses will be forgotten once we stop maintaining them.

[^1]: The lore that Winchester built her mansion to house ghosts killed by Winchester rifles is likely just gossip and marketing. There’s little evidence to support these claims. ([99% Invisible has a good episode exploring Winchester, her house, and this lore](https://99percentinvisible.org/episode/mystery-house/).)

[^2]: While editing this piece, [Dumky published another analysis illustrating the production of coding agents](https://www.dumky.net/posts/youre-right-to-be-anxious-about-ai-this-is-how-much-we-are-building/). In it he shows a 280% increase in “Show HN” posts, a 93% increase in new Github repos, and a *dramatic* uptick in packages published to Crates.io.

[^3]: Anthropic’s ability to stabilize this line is rather impressive. Claude code is getting better at planning, better at chunking out work, enabling more effective sub-agent delegation.

[^4]: Though this is likely an updated tweak of Brooks’ statement that an “industrial team” might write 1,000 “statements” per *year*.
