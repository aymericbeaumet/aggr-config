---
title: Earning taste and judgment
link: https://addyosmani.com/blog/earning-judgment/
source: addyosmani-com
published: 2026-07-14T00:00:00Z
updated: 2026-07-14T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Taste used to be a byproduct of the reps. Agents took the reps. So if you're junior you now have to go get the taste and judgment on purpose. A builder's look at the weakening entry level, the two debts we now owe, and concrete ways to build judgment when the machine writes the code.
content: extracted
html: 2026-07-14-earning-taste-and-judgment.html
preview:
  file: 2026-07-14-earning-taste-and-judgment.preview-1cbb2562d5d6.webp
  width: 256
  height: 134
  color: '#252525'
images:
- source: https://addyosmani.com/assets/images/earning-judgment.jpg
  original:
    file: 2026-07-14-earning-taste-and-judgment.image-839ca61d6ed6.jpg
    width: 2400
    height: 1256
  color: '#000000'
---

**Taste used to be a byproduct of the reps. Agents took the reps. So if you’re junior you now have to go get the taste (and judgment) on purpose.**

As I was building my career, I learned to think about code in fundamentally different ways. **The first chunk of my learning came from thousands of reps of boilerplate, bug fixes, and solving problems**. **Almost all of the taste and judgment I have came from those reps, not from talent.** Over time, I learned to handle increasingly complex abstractions and tradeoffs. That’s how a junior developer matures into a senior one.

But agents will automate all of these reps. This will fundamentally change the path from junior to senior.

**Spoiler: The thing that stays durable and ungradeable is not getting better at solving problems that already have a known answer; it’s choosing what to build and judging whether it’s any good.**

**The entry-level is weakening.** As of March 2026, unemployment among recent college graduates sat at a staggering 5.6%, with underemployment at 41.5% - and recent graduates are now *more* likely to be unemployed than the workforce as a whole, an inversion of the historical norm ([Federal Reserve Bank of New York](https://www.newyorkfed.org/research/college-labor-market)). [Forbes](https://www.forbes.com/sites/michaeltnietzel/2026/02/23/unemployment-and-underemployment-rates-among-recent-college-graduates/), citing the St. Louis Federal Reserve, notes recent computer-engineering graduates had an unemployment rate of 7.5% and computer-science graduates 6.1%, both elevated above many non-technical majors.

The demand side reshaped underneath them: the [Indeed Hiring Lab](https://www.hiringlab.org/2025/07/30/experience-requirements-have-tightened-amid-the-tech-hiring-freeze/) found junior and standard tech titles down 34% since early 2020 against just 19% for senior and manager roles, while the share of tech postings demanding five or more years of experience climbed from 37% to 42% - even as the number of computer-science graduates kept growing. The most AI-exposed early-career workers (ages 22–25) showed a 13% relative employment decline in the original Stanford Digital Economy Lab study; a [February 2026 follow-up](https://digitaleconomy.stanford.edu/news/canaries-interest-rates-and-timinga-more-on-recent-drivers-of-employment-changes-for-young-workers/) revised that to 16% through October 2025 ([Brynjolfsson, Chandar & Chen](https://digitaleconomy.stanford.edu/publication/canaries-in-the-coal-mine-six-facts-about-the-recent-employment-effects-of-artificial-intelligence/)) - and in many of these occupations that age group had accounted for roughly half of all hiring.

Should we panic? Well, **entry-level software-developer jobs are more than employment; they’re a training system**.

Microsoft’s [Mark Russinovich and Scott Hanselman](https://www.infoq.com/news/2026/04/junior-developer-pipeline-crisis/) have warned that, **far from taking the bread of all software developers, agents actually help senior developers while robbing juniors of theirs and narrowing the pyramid**. Hanselman’s telling example: inserting a sleep() to paper over a race condition. The senior developer spots the error and helps you learn from it. The junior developer skips the code review, pushes it to production, and the race later crashes the site - then does it again. Their real point is that the agents aren’t doing this on their own; it’s less an AI story than a story about how people choose to use the products.

Worse, a 2026 [*MIT Technology Review*](https://www.technologyreview.com/2026/05/26/1137865/its-time-to-address-the-looming-crisis-in-entry-level-work/) piece warned that automating the learning stage buys output now but leaves society gradually less capable later, and the [World Economic Forum](https://www.weforum.org/publications/artificial-intelligence-and-the-future-of-entry-level-work-a-framework-for-safeguarding-and-reinventing-early-career-pathways/) warned in mid-2026 that cutting entry-level recruitment could weaken the talent pipeline going forward. A word of caution, though: this isn’t purely an AI story. We’re partly correcting a hiring binge that started in 2022 and ran into 2025. And even the Stanford authors were careful early on - the early signal was muddy, and only after 2024 does the data get clean enough to isolate an AI-attributable decline. Too many vendor “synthetic labor” announcements are marketing prose, not prophecy.

And of course, all of this sits right next to a genuinely optimistic story from the rest of the market. **The [World Economic Forum’s Future of Jobs 2025](https://www.weforum.org/press/2025/01/future-of-jobs-report-2025-78-million-new-job-opportunities-by-2030-but-urgent-upskilling-needed-to-prepare-workforces/), published in January 2025, projects that by 2030 some 170 million new roles will emerge against just 92 million displaced - a net gain of 78 million.** Apollo Global Management’s chief economist Torsten Sløk [caused a stir](https://fortune.com/2026/06/01/apollo-chief-economist-torsten-slok-zero-evidence-ai-killing-jobs-says-its-creating-them/) with the claim that there is “zero evidence of job losses because of AI” - arguing that, if anything, the spending is creating work for people with AI skills.

**So how can there be contradictory reports in the same year?**

Because the net and the entry level are two different questions. The optimists aren’t wrong. **Senior roles requiring judgment and critical thinking will grow substantially while junior roles will likely shrink**. Indeed’s numbers say the same. And [Goldman Sachs](https://fortune.com/2026/04/06/ai-tech-displacement-effect-gen-z-16000-jobs-per-month/), looking at 2026 payrolls, estimated a net loss of about 16,000 US jobs a month with Gen Z and entry-level roles hit hardest - the empirical answer to “zero evidence.”

Aggregate growth and a vanishing first rung are both true at once - and if you’re the one trying to climb onto that rung, the net number is cold comfort.

So if junior developers are at risk in this phase, the real question is how we should proceed? If the AI layer gets good at anything, it will be anything that has an answer key. School used to be answer keys all the way down, all about getting the right answer.

**So I propose we look less at the availability of jobs and focus more on developing taste: compressed experience in the form of earned pattern-matching**. [Kent Beck](https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent) notes that it’s unlikely AI agents will ever possess taste, by which he means judgment, so we’ll be left to supply it in these intersections.

**We now owe two debts: cognitive surrender and comprehension.** When you first learn to code, a wise teacher told you not to copy and paste from Stack Overflow. Unfortunately, I think we’ve all violated that advice at some point, especially mid-career. The problem gets worse when we offload the copy-paste decision to an AI agent. [Shaw and Nave](https://knowledge.wharton.upenn.edu/podcast/ripple-effect/how-ai-is-reshaping-human-intuition-and-reasoning-gideon-nave-and-steven-shaw/) surveyed 1,372 participants over ~10,000 trials and found that they tended to accept incorrect AI output nearly 80% of the time. Their accuracy dropped ~15 points below the no-AI baseline, while confidence rose ~12%.

The agent runs a loop of activities (investigate, implement, test, report).

**You, the human, own the outer loop of deciding whether the result is worth your attention; verifying that the result is worthy of approval (diffs, test results, logs, and a short why); approving or blocking; carrying the consequence. The boundary is evidence (my psalm).** The gap between the loops is growing rapidly, and you must manage it.

When agents write code, it’s easy to think they’re doing all the hard work. It’s hard to see what a human must do to maintain quality when agents are getting it mostly right. **Let me now turn from managing agents to building your taste and judgment, starting with seven concrete things you can do.**

- **Read far more code than you generate**. Hunt for logic errors, security holes, simple or subtle edge cases. On a regular basis, ask yourself about the code you read: Did I consider the right things?
- **Keep a wrong log**. Every mistake an agent makes gets one sentence. After thirty days you get a sense of patterns.
- **Do a few things the hard way on purpose**. Build a parser manually, or a CRM, or something meaningful from scratch. Protect your collateral learning. Karpathy emphasizes fundamentals like memory, views (how the world appears to the system), and storage that agents get wrong.
- **Go deep on one system end to end.** Push it all the way to failure. Then learn what real depth feels like.
- **Learn to specify and verify separately.** Spec-writing is clear thinking. Verification is evidence. Specification quality is the biggest lever.
- **Build an eval, a test framework around a rubric of correctness, maintainability, efficiency, security, style.** Run it on fifty real PRs generated by AI agents. Note surprising test failures and fixes. Calibrate to make your internal quality function explicit.
- **Calibrate autonomy per task.** Turn it way up on cheap, reversible tasks. Down on expensive failures. Learning to calibrate is a senior developer instinct worth exercising daily.

What do people who are building these agents tell juniors?

- **“People are still going to need to know the craft underlying software engineering, including languages, compilers, runtimes, and system design”** [Boris Cherny](https://rogerwong.me/2026/02/what-happens-after-coding-is-solved-boris-cherny) of Claude Code says. His ‘coding is largely solved’ statement is about the workflow he personally finds productive, not about forgoing your fundamentals. As an engineer, you really need to know what’s going on under the hood.
- **Beware the paradox of supervision.** [A report from Anthropic](https://www.anthropic.com/research/how-ai-is-transforming-work-at-anthropic) says they only use AI to help when they already know the answer, and warn that supervising an agent requires exactly the skills that atrophy when you over-rely on one. For a junior developer, growing into that state of mind deliberately, rather than accepting answers blindly, takes effort. Use the tool carefully while deliberately building deeper understanding of the systems you work on.
- **Think carefully about which muscles you are willing to let atrophy.** [Gergely Orosz](https://newsletter.pragmaticengineer.com/p/the-pragmatic-engineer-ama) uses AI for coding but zero AI for writing. Be mindful of trade-offs and decide which ones you are choosing to keep sharp.

**Where does durable value concentrate? Start by optimizing for scarce resources.**

Capital is abundant. Time is abundant. Real relationships, and especially a track record of doing good work, are still scarce: I can raise money in a couple of weeks, but I can’t raise a reputation. When vibe-coding makes earning a quick buck trivial, that quick buck is worth very little; the scarce move is choosing something worth shipping. With that lens, four principles.

1. **Finish the last mile.** Automation covers the easy 80-90% of software engineering. The last mile - edge cases, architecture, taste - is the whole game. As first drafts become free, the finish is the product, and it’s where people distinguish themselves.
2. **Solve the hard version.** Richard Sutton’s famed [bitter lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) of the last three decades is not just career advice: The easy version is already solved; durable value comes from solving the hard one.
3. **Build in public near hard problems.** The same constraint that drove junior developers to abandon open source - a well-conceived project takes one person a year, easy to space-out or clone in bits - is less important than the scarcity of an open-source track record of really good work. Think of it like expected goals in soccer: your reputation and public work decide how many chances you get in front of goal, and judgment is whether you convert them. You can’t script which chances arrive, only whether you’re standing where they land; almost every real opportunity I’ve had came from work I did in public, never from a job I applied for. So get a foot in the door wherever there is a hard problem you want to solve. That means taking an internship at a small or midsize company, getting on a software engineering track somewhere, or building side projects that matter with production code. Those who try to cut corners will find that it’s really hard.
4. **Be a T-shaped generalist.** Developers with the deepest knowledge in one or two areas while maintaining broad literacy tend to deliver the best results. A good dose of AI assistance means developers will be able to accomplish more as single contributors working on fewer different areas than they could before. Teams that lean into small cross-functional teams and end-to-end engineers will thrive.

**To put it plainly: the world isn’t short on opportunity; it’s short on people who can find the right problem, tell whether the machine solved it, and finish past where the machine stopped.**

We talk about the “last mile” as the biggest piece of the puzzle, but in the world of agents the last few feet are effectively infinite - agents scale output infinitely; you don’t. Your attention is your most precious asset, and it doesn’t refill, so protect it. Anything gradeable by someone else is getting automated.

The career is the ungradeable part: choosing what matters, judging honestly when you’ve got it, and answering for it. Do that. In public. Near the hard problems. The rest tends to follow.

*Pangram* [scored](https://www.pangram.com/history/97a8a162-1c10-4d30-b185-3a6ab8b68a0e) *this article as 100% human authored*
