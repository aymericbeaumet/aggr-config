---
title: The systems that no one will test
link: https://blog.christianperone.com/2026/09/the-systems-that-no-one-will-test/
source: hnrss-org
published: 2026-09-28T10:51:46Z
updated: 2026-09-28T10:51:46Z
first_seen: 2026-09-29T14:21:22.166029648Z
authors:
- perone
labels:
- cybersec
- machine learning
content: extracted
html: 2026-09-28-the-systems-that-no-one-will-test.html
preview:
  file: 2026-09-28-the-systems-that-no-one-will-test.preview-be9e6ba1aaf2.webp
  width: 64
  height: 64
  color: '#474141'
images:
- source: https://secure.gravatar.com/avatar/6516dcaae08a9b3f2aaed86f666562b387b5afec8f96782238cc0e44fdaac90a?s=64&d=robohash&r=g
  original:
    file: 2026-09-28-the-systems-that-no-one-will-test.image-5c8f215dd851.png
    width: 64
    height: 64
  variants:
  - file: 2026-09-28-the-systems-that-no-one-will-test.image-be9e6ba1aaf2.webp
    width: 64
    height: 64
  color: '#f4f5f6'
---

This happened to me in 2020, and it has been on my mind again lately. During the worst of the pandemic, I found a vulnerability in a system that gave me access to the Brazilian federal system, and with that access I was able to retrieve information on any Brazilian (think of 200+ million people data). These records had essentially each person’s entire set of documents (ID, CPF, passport, place of birth, parents’ names, driver’s licence, home addresses, mobile and landline numbers, whether the person was in a witness protection programme, etc.), so you can get a sense of how serious this was and how much someone could do with this data. Honestly, I didn’t quite know how to react and I immediately phoned the agency responsible for the system (it wasn’t easy to find the right contact, you obviously don’t want to disclose that to the wrong person).

When the person picked up my call, I identified myself and told him what I had found and what access I had. He was in disbelief at first, thinking it didn’t make much sense (and I understand his confusion, imagine someone calling you out of the blue and bringing a weird topic), and asked whether I was a civilian. I said yes, and a few hours later they got back in touch and asked me to explain the issue. By then I think they were a bit worried but still not believing much on it, and once I explained the problem, they probably realised very quickly that this was a major breach. They acknowledged the issue and fixed it very fast. I honestly don’t blame them: these same people were working really hard during the pandemic, and software is software. I was glad to have helped in some small way (at least I like to think that way). I never exfiltrated any data from this system, and I avoided talking about it later because it makes you look like a “hacker”, which is something stupid to say.

So why am I talking about this ? Between 2020 and 2026, a lot changed, and ML began advancing at a crazy pace (and it still is), at perhaps the worst possible geopolitical moment. Fast-forward to 2026, and we’re seeing reports of cybersecurity incidents involving many different models, including the one OpenAI described in its [technical report](https://cdn.openai.com/pdf/67869394-cb91-4c12-888c-5cbd85c7814c/OpenAI-Hugging-Face%20Incident-Technical-Report.pdf). Although I’m not sure the narrative that the model “*escaped its safeguards*” makes much sense when OpenAI deliberately disabled classifiers and reduced safeguards (something many people weren’t aware of), these models are now clearly a big problem for nation-states. Even though safeguards are the labs’ choice, we need to acknowledge that the capability itself is real, and clearly not everyone will run evals (or, even worse, training) with proper safeguards enabled. I think training is even more complicated, because there it is also easier for the agent to figure out how to bypass/exploit safeguards.

Following recent advances in mid-training, RLVR and long-horizon tasks, it is clear that over the past year (or more) many labs have been **aggressively** scaling RL environments, usind third-party companies, using models to build them, to synthesise them and provide their rewards, in a “self-improvement” loop (in quotes because the term is quite overloaded nowadays) that accelerates as fast as your rollout potential. That’s why I keep returning to the episode I described earlier: today it would be straightforward for me to build an RL environment that not only replicates the setup in which I found the vulnerability (which is not that different from the one in OpenAI’s report, where an agent leveraged SSRF through OpenAI’s internal Artifactory) but also to develop a curriculum for it. So I wonder: how many environments like this are frontier labs now building with support from cybersecurity companies ? What will happen to the many government systems **that will never get the chance to be AI-pentested** ? The flaw I found didn’t require any extraordinary expertise, just attention to detail, and that is exactly what worries me. What will happen from now on, with agents that are far more capable, faster and easily scalable ?

Just like the climate problem, ML is now a planetary problem, and as Yuk Hui mentioned in his book *Machine and Sovereignty: For a Planetary Thinking* (which I recommend to anyone interested), **we are still not thinking planetarily, but we must learn to, even if that takes considerable time**. I think the only way forward is to first get past the polarisation between technophobia and technophilia (I wrote a bit about it [here](https://blog.christianperone.com/2025/01/notes-on-gilbert-simondons-on-the-mode-of-existence-of-technical-objects-and-artificial-intelligence/)) and start working on solutions. Obviously many people in frontier labs are already doing this work. But it seems to me an impossible task to find a solution while our thinking is guided by geopolitical interests. Unfortunately, emerging economies and developing countries will be the most vulnerable, as if it weren’t already enough that they seldom have access to compute. That is a very sad thing to think about. I also can’t help imagining that, somewhere, there is a very similar environment that reproduces what I found in 2020: almost the same system, almost the same flaw. Multiple agents enter it thousands of times a second, generating trajectories until one of them finds the flaw. The difference is that when this is put into use, in none of the attempts does anyone pick up the phone.
