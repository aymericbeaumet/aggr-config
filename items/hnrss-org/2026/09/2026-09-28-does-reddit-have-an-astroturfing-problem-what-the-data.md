---
title: Does Reddit have an astroturfing problem? What the data suggests
link: https://www.petervijeh.com/projects/reddit-astroturf
source: hnrss-org
published: 2026-09-28T13:30:54Z
updated: 2026-09-28T13:30:54Z
first_seen: 2026-09-29T01:16:22.439760281Z
authors:
- p-s-v
content: extracted
html: 2026-09-28-does-reddit-have-an-astroturfing-problem-what-the-data.html
preview:
  file: 2026-09-28-does-reddit-have-an-astroturfing-problem-what-the-data.preview-191d2ab291f2.webp
  width: 256
  height: 134
  alt: Does Reddit have an astroturfing problem? What the data suggests
  color: '#f0f2f4'
images:
- source: https://petervijeh.com/projects/reddit-astroturf/opengraph-image?ea42a52299b9e000
  original:
    file: 2026-09-28-does-reddit-have-an-astroturfing-problem-what-the-data.image-4a8b18702e81.png
    width: 1200
    height: 630
  color: '#fefefe'
- source: https://www.petervijeh.com/reddit-astroturf/redcmts-pricing.jpg
  original:
    file: 2026-09-28-does-reddit-have-an-astroturfing-problem-what-the-data.image-5f3306de2ad8.jpg
    width: 1440
    height: 900
  color: '#f9f9f8'
- source: https://www.petervijeh.com/reddit-astroturf/soar-warmed-accounts.jpg
  original:
    file: 2026-09-28-does-reddit-have-an-astroturfing-problem-what-the-data.image-bf4a0b05d771.jpg
    width: 1440
    height: 900
  color: '#fdf9f9'
---

One chef's-knife brand gets 31% of its "what should I buy" mentions from 5% of the accounts, four times what chance predicts. So I pulled those accounts' full Reddit histories.

I like to cook, and cooking turned into an obsession with high-end Japanese chef's knives. When I want to buy a knife, or anything else, I type the product name into Google and add the word "reddit". A lot of people do this. Mike Riggs wrote it up in Reason in 2022 as "the Reddit hack", after Dmitri Brereton's essay on Google search made the same point: a query with "reddit" on the end returns humans instead of affiliate pages. The bet behind the habit is that a stranger in r/chefknives has no reason to lie to you about a knife.

That bet has an obvious weak point. If Reddit is where buyers go for unpaid opinions, Reddit is where a brand would want to plant paid ones. I wanted to know whether the knife subreddits I read, and scrape for New Knife Day, show any sign of that. Not a hunch about one suspicious comment, but something I could count and someone else could recount.

## Why the question is testable at all

Last year I fine-tuned a small named-entity model, GLiNER, to pull brands, models and steels out of knife comments. From "picked up a Mazaki in white #2, way better than my old Fibrox" it returns Mazaki as a brand, Fibrox as a model and white #2 as a steel. That model runs over every comment the New Knife Day scraper collects from six subreddits: r/knives, r/knifeclub, r/chefknives, r/japaneseknives, r/FixedBladeEdc and r/KnifeSteels. [The write-up on that model is here.](https://www.petervijeh.com/projects/reddit-ner)

So for every comment I already have who wrote it, which brands it names, and whether the thread it sits in is someone asking what to buy. That is enough to ask a narrow question: in the threads where a recommendation changes a purchase, who is doing the recommending?

New Knife Day is my site for knife collectors. It catalogs knives and steels and tracks which knives people on Reddit are buying and arguing about, so I have a stake in knife Reddit being worth reading. [New Knife Day is at new.knife.day.](https://new.knife.day)

## What astroturfing would look like in the data

Nobody publishes their shill accounts, so I had to decide in advance what paid posting would leave behind. The market for it is not hidden. REDCmts sells one Reddit comment for $9.99 and 100 for $699.99, from what it calls "real, aged accounts", and shows a gallery of brand mentions it says it delivered. Soar says its accounts are "aged and manually warmed" for weeks before a single brand mention. Bazzly advertises automated replies to every post that looks like someone shopping.

![REDCmts pricing page: one comment $9.99, ten $89.99, one hundred $699.99](https://www.petervijeh.com/reddit-astroturf/redcmts-pricing.jpg)\
REDCmts price list, September 2026. This shows the service exists and what it costs. Nothing in this article connects it, or any vendor, to any account in the knife subreddits.

![Soar marketing page describing accounts aged and manually warmed before any brand mention](https://www.petervijeh.com/reddit-astroturf/soar-warmed-accounts.jpg)\
Soar describes the account preparation a buyer is paying for. Same caveat as above.

Taking those sales pages as the description of the product, a paid campaign for one brand, delivered through a handful of prepared accounts, should show up as:

- A small tail of accounts writing a disproportionate share of the brand mentions in "what should I buy" threads.
- Those accounts naming one brand almost every time they name any.
- Thin accounts: few comments, low scores, no real standing in the subreddit.
- Young accounts, or accounts with histories that are hidden or wiped.
- Accounts that post mostly in knife subreddits, since the knife comments are what is being paid for.
- Links to a store or an affiliate page.

Every one of those is also what a devoted fan, a maker's employee posting on their own time, or a brand's own subreddit regulars wandering into a buying thread would produce. Public Reddit data can show that recommendations are concentrated, and cannot show why. Everything below is about the first half.

## The corpus, and the refresh that changed the answer

The scraper stores each new post and its comments shortly after posting. That turned out to be the wrong moment for this question: a buying thread collects its recommendations over the following day or two, and r/knives posts had 1.5 stored comments each. A refresh pass went back to 3,607 posts older than 48 hours and refetched their comment trees, which took the corpus from 21,673 comments to 51,129. Before the refresh the test below found nothing, because about 800 buying-thread mentions were too few to tell 7.6% from the 7.1% chance gives.

|                                                      | Count  |
| ---------------------------------------------------- | ------ |
| Posts                                                | 6,675  |
| Comments after refresh                               | 51,129 |
| Authors with 10 or more comments                     | 987    |
| Brand mentions in buying threads, from those authors | 1,471  |

Three definitions do the work. A buying thread is a post whose title or body matches phrases like "should I buy", "recommend", "under $" or "best knife"; it is a regular expression, not a classifier. An author's brand-heaviness is the share of their comments that name a brand, weighted toward naming the same brand each time. The tail is the top 5% of the 987 authors with 10 or more comments on that score, 49 accounts. I score it this way because an account that keeps bringing up the same brand is the product the vendors above are selling.

The question is what share of buying-thread mentions the tail would write by chance, given how much everyone posts. To get that number I keep every brand mention where it is, in its thread and naming its brand, and reassign the author names at random across those mentions, like shuffling name tags. A prolific account still gets many mentions and a quiet one gets few, but which threads an account appears in no longer depends on who it is. I do this 1,000 times and record the tail's share each time. If the real share sits inside the range those 1,000 reassignments produce, the tail is no more concentrated in buying threads than its comment count explains. If it sits above the range, the tail accounts turn up in buying threads more often than their posting volume explains. Authors are salted hashes throughout; no usernames or comment text leave the database.

## What the data supports

If the tail shows up in buying threads only because it posts a lot, random reassignment says it should write about 7.9% of the brand mentions there, and almost always between 6.3% and 10.1%. It wrote 11.3%. Only 2 of the 1,000 random reassignments reached that. In plain counts, about one buying-thread recommendation in nine comes from these 49 accounts, where chance says one in thirteen, which is about 50 extra recommendations out of 1,471.

Where the extra share lands matters more than its size. If knife Reddit as a whole were being gamed, every subreddit would sit above chance. Two do, r/chefknives and r/knifeclub. r/knives, the largest, is within half a point of chance. A paid campaign is bought by one brand, so it should also show up brand by brand, and it does: three brands get far more of their buying advice from the tail than chance gives, one large brand gets slightly less, and three get none. That is the shape a few targeted campaigns would leave, and also the shape a few loud fan bases would leave.

| Buying-thread brand mentions                     | Share from the tail | Share by chance |
| ------------------------------------------------ | ------------------- | --------------- |
| r/chefknives                                     | 14.9%               | 7.7%            |
| r/knifeclub                                      | 12.4%               | 7.3%            |
| r/knives                                         | 6.7%                | 6.4%            |
| r/japaneseknives                                 | 15.2%               | 14.3%           |
| r/FixedBladeEdc                                  | 8.5%                | 8.7%            |
| Brand B003, a chef's-knife brand                 | 31.2%               | 8.0%            |
| Brand B004                                       | 26.1%               | 8.2%            |
| Brand B001, the most-mentioned brand in r/knives | 20.4%               | 11.8%           |
| Brand B002                                       | 8.1%                | 9.5%            |

The brands are coded because a concentration statistic is not evidence that any brand paid for anything, and the brand with the strongest signal also has a large, loud fan base.

On corpus data, the tail also matches two more of the predictions above. The accounts are thin: a median of 12 comments in these six subreddits, and a median score of 1, so nobody is upvoting them into prominence. They are loyal to one brand: two thirds of a tail account's brand mentions go to the same brand. That is three of the six predictions, all the corpus can test. Age, knife focus and store links need each account's whole Reddit history.

## Their full Reddit histories look ordinary

I took the 23 tail accounts behind the three brands with a signal and fetched everything Reddit would return for each: up to about 2,000 comments and their submissions, account creation date and karma. For comparison, 23 other 10-plus-comment authors from the same corpus, drawn at random from the tail's comment-count range. Eight tail histories and seven comparison histories were hidden, suspended or deleted; the rest were readable.

A prepared paid account should be young, post mostly in the subreddits it is paid to post in, and push one brand. The tail accounts are 4.5 years old at the median, the same as the comparison accounts. Only 3% of their comments are in the six knife subs, against 10% for the comparison group, and they spread over 66 subreddits to the comparison group's 47. Across their whole history they name seven knife brands, not one. Store links are rare in both groups.

I compared about two dozen features in all. With 15 and 16 readable accounts, none of the differences is larger than splitting 31 people into two random groups would often produce, and the ones that lean anywhere lean toward the tail being less knife-focused.

That is not what a warmed, single-purpose account looks like. It is what a person who is on Reddit a lot, and who has strong feelings about one knife maker, looks like. It is also what a well-run paid account would look like, which is why the vendors sell aged accounts instead of fresh ones. The eight hidden tail histories could hold the whole story and I cannot read them.

## Shortcomings

- The brand detector is the stock GLiNER model, not the knife-tuned one from the earlier write-up, because that repo shipped without weights. It misses some brands and over-tags some model names. No hand-labeled check of its output on this corpus exists yet.
- "Buying thread" is a regex over titles and bodies. It has not been checked against 100 hand-labeled threads either. Both checks are cheap and I have not done them; until then every percentage above is a percentage of machine labels.
- The tail is 49 accounts and the per-brand results rest on 15 to 20 of them. One brand-model false positive in the NER could move a per-brand row.
- I tested eight brands and six subreddits, and with that many tests one or two will look unusual by luck. The overall 11.3% result is a single test and does not have that problem.
- The comparison accounts were drawn from the tail's comment-count range, not paired account by account. They have a median of 23 comments in the corpus to the tail's 13, which should make them look more knife-focused, not less.
- Histories are capped at what Reddit's listing returns, around 2,000 comments. For accounts over the cap, the age at first knife comment is unknown and left blank.
- Reddit's listings stop near 1,000 posts per subreddit, so the kitchen subs cover about a year and r/knives and r/knifeclub only the weeks before the refresh. This is six subreddits, not Reddit.
- New Knife Day, which I run, tracks the same brands this corpus counts. I have no relationship with any brand in the data and no way to prove that to you.

## What I take from it

Adding "reddit" to a knife search still gets you humans, mostly. For a couple of brands, a quarter to a third of the buying advice comes from accounts that mostly recommend that brand. Whether those are fans or paid, I do not know, and after reading their histories I lean toward fans and hold the lean loosely.

The practical check is the one the data endorses: when a knife recommendation comes from an account you do not recognize, click through and see whether it has ever named a different brand. In this corpus that one question separates the tail from everyone else better than karma does. It will not catch the paid comment from a real aged account, and I do not think anything a reader can do will.

Code, anonymized data and charts are in the public repository linked below. Usernames, comment text, account histories and the brand-code key are not published and will not be.

## At a glance

Question

Do the knife subreddits' "what should I buy" threads show the concentration paid posting would leave behind

Approach

GLiNER brand tags over 51,129 comments from six subreddits, 1,000 random reassignments of authors to estimate chance, then full Reddit histories for 23 tail accounts and 23 comparison accounts

Result

Tail accounts write 11.3% of buying-thread brand mentions against 7.9% expected; their full histories are 4.5 years old and no more knife-focused than the comparison group's

Not shown

Payment, coordination, or which brand is B003
