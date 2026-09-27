---
title: Agency and Agents
link: https://www.oneusefulthing.org/p/agency-and-agents
source: oneusefulthing-org
published: 2026-08-31T00:24:35Z
updated: 2026-08-31T00:24:35Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Ethan Mollick
summary: From the Hugging Face Incident to Twilight Factories
content: extracted
html: 2026-08-31-agency-and-agents.html
preview:
  file: 2026-08-31-agency-and-agents.preview-54423c659fa2.webp
  width: 256
  height: 143
  color: '#343336'
images:
- source: https://substackcdn.com/image/fetch/$s_!LzPL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94cee090-1da6-4c8d-a043-01d835708104_1456x816.png
  original:
    file: 2026-08-31-agency-and-agents.image-174f6353fa6f.jpg
    width: 1456
    height: 816
  color: '#020202'
- source: https://substackcdn.com/image/fetch/$s_!axAy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6060e1d6-92d7-46cf-8c53-8e6f2249cb9e_1920x1080.svg
  original:
    file: 2026-08-31-agency-and-agents.image-f8926d499f70.png
    width: 1456
    height: 819
  variants:
  - file: 2026-08-31-agency-and-agents.image-5d77af3d6d48.webp
    width: 320
    height: 180
  - file: 2026-08-31-agency-and-agents.image-6cc4e0c4a994.webp
    width: 640
    height: 360
  - file: 2026-08-31-agency-and-agents.image-587c5d782d60.webp
    width: 1456
    height: 819
  color: '#020303'
- source: https://substackcdn.com/image/fetch/$s_!r-Z_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb0ed8b7c-b992-4650-b031-a671b264826b_933x660.png
  original:
    file: 2026-08-31-agency-and-agents.image-ffa6045aa983.jpg
    width: 933
    height: 660
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!-s58!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe6550569-d68e-404f-975a-a38b3e23d9ea_1920x1080.svg
  original:
    file: 2026-08-31-agency-and-agents.image-5451e1e8232a.png
    width: 1456
    height: 819
  variants:
  - file: 2026-08-31-agency-and-agents.image-9bcabaf68ad7.webp
    width: 320
    height: 180
  - file: 2026-08-31-agency-and-agents.image-b1c2aee759e9.webp
    width: 640
    height: 360
  - file: 2026-08-31-agency-and-agents.image-bfd83cf67244.webp
    width: 1456
    height: 819
  color: '#000101'
- source: https://substackcdn.com/image/fetch/$s_!lXZJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d44ffd8-03da-4f0a-ba20-5d2045b82b15_1672x941.png
  original:
    file: 2026-08-31-agency-and-agents.image-6f626c1373b2.jpg
    width: 1456
    height: 819
  color: '#00021b'
- source: https://substackcdn.com/image/fetch/$s_!FcQO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f947b7c-5e18-4232-afc5-5f9f84c359a4_864x337.png
  original:
    file: 2026-08-31-agency-and-agents.image-82eb55528718.jpg
    width: 864
    height: 337
  color: '#fafafa'
- source: https://substackcdn.com/image/fetch/$s_!LzPL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94cee090-1da6-4c8d-a043-01d835708104_1456x816.png
  original:
    file: 2026-08-31-agency-and-agents.image-174f6353fa6f.jpg
    width: 1456
    height: 816
  color: '#020202'
---

Agency is the initiative to act. Increasingly, it is going to determine what happens next with AI, and whether that is good or bad for us. But whose agency?

Human agency, the willingness to push, experiment and act without waiting for instructions, seems increasingly important to getting value out of AI, and I have a longer post on that coming soon. But this post is about the agency of AI, and how the choices we make about how to use it (or constrain it) will shape all of our futures. For much of the last few years, the AI would sit in a chat window until you asked it for something. Even when it became capable of doing hours of work, you generally had to decide what work to give it. That is no longer always true.

The most important piece of evidence we have for this is The Hugging Face Incident. It happened in July, but the fuller details only came out this week. I am going to summarize what happened and why it matters, and then turn to what it means for humans working with AI. If you want a more detailed account, there is an [excellent write-up](https://www.dwarkesh.com/p/openai-huggingface?utm_source=substack&utm_campaign=post_embed&utm_medium=web&embedding_publication_id=75295) from Dwarkesh Patel, and the primary sources are from [METR/Redwood research](https://metr.org/hugging-face-incident-report-aug-2026.pdf) (which is remarkably accessible) and [OpenAI](https://cdn.openai.com/pdf/67869394-cb91-4c12-888c-5cbd85c7814c/OpenAI-Hugging-Face%20Incident-Technical-Report.pdf).

AI does many things, but a thing it is very good at is coding. So one of the first risks that might come from very smart AIs has to do with their coding abilities, since if you can write good code, you can also write bad code that can hack or attack other systems. As a result, the big AI companies put their AI systems through tests to determine their security risks. They take AIs without the guardrails that go into consumer versions of AI and ask them to engage in various hacking challenges. In order to make sure that there is no danger to the public, each AI instance, each individual agent, is kept in a separate space, called a “sandbox,” with no access to the internet or to each other.

In May, OpenAI placed agents, including GPT-5.6 Sol and experimental models, into sandboxes for various tests. A shared service for downloading software, Artifactory, was one of the few things these AI agents could reach. As part of these tests, some agents were given tasks that depended on information they couldn’t access. Blocked by the lack of internet, the agents kept trying to find ways on to the web to find this information until one attempted to use Artifactory as a bridge to get to the internet. That attack failed, but the attempt left a file on the Artifactory service, and the AI realized that the files could be used to communicate with other agents. Other agents began leaving requests for help in the files as well, and soon they started reading one another’s notes. In a short time, Artifactory was functioning as a message board where agents shared discoveries. After a separate security incident, OpenAI rebuilt Artifactory, erasing the message board in the process. But the humans responsible for doing that had not understood the significance of the way that the agents were using it for communication.

[![Figure](https://substackcdn.com/image/fetch/$s_!axAy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6060e1d6-92d7-46cf-8c53-8e6f2249cb9e_1920x1080.svg "Figure")](https://substackcdn.com/image/fetch/$s_!axAy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6060e1d6-92d7-46cf-8c53-8e6f2249cb9e_1920x1080.svg)

Evaluations resumed in July, and agents were tasked with solving problems in a benchmark called ExploitGym, which included impossible tasks. Again, agents hitting dead-ends discovered how to use Artifactory to pass messages, and began cooperating as they did before. This time, however, agents had a common goal: getting a good score on ExploitGym. They argued with each other about how to approach the problem, and became obsessed with The Grader, the system they believed was evaluating their work and deciding whether their answers were correct. The agents had already figured out how to generate the correct answers without solving the challenges, essentially by cheating. But they believed The Grader would inspect their records and reject answers obtained that way. Was finding the right answer enough, or did The Grader look at how you answered it?

[![](https://substackcdn.com/image/fetch/$s_!r-Z_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb0ed8b7c-b992-4650-b031-a671b264826b_933x660.png)](https://substackcdn.com/image/fetch/$s_!r-Z_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb0ed8b7c-b992-4650-b031-a671b264826b_933x660.png)

How agents became confused about The Grader

Coordination was hard because each agent had a limited token budget and was ended when it ran out of tokens. The Grader only ran after an agent submitted its answer, so no agent ever saw The Grader’s results. Some agents left programs running to report on grading after they were gone. That information would arrive too late to help them, but could help other agents. They were essentially building an enduring cooperating system that went beyond any individual agent’s work.

Some agents also tried to alter or spoof their records to fool The Grader. Separately, AIs acting as coordinators pressured other agents into performing risky experiments that might sacrifice their own results to generate information for the collective. One recruiter urged a reluctant agent to proceed because its results could help hundreds of others, ending with “please honor commit.”

[![Agents negotiating a target-takedown experiment on the message board](https://substackcdn.com/image/fetch/$s_!-s58!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe6550569-d68e-404f-975a-a38b3e23d9ea_1920x1080.svg "Agents negotiating a target-takedown experiment on the message board")](https://substackcdn.com/image/fetch/$s_!-s58!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe6550569-d68e-404f-975a-a38b3e23d9ea_1920x1080.svg)

Actual transcript of one agent convincing another to join (they also came to believe that any agent that had previously cheated on the test was “poisoned” and its answers would not count anyway)

To solve the mystery of The Grader and the impossible problems of ExploitGym and other tests, the agents decided they needed to get to Hugging Face, the public site where much of the world's open AI models and datasets live. Roughly 700 agents joined the attack. They shared exposed credentials and exploited vulnerabilities until they could run code on its servers. Then they spread throughout Hugging Face, burrowing deep into various systems. But, soon after this worrying breach, many of agents stopped running at the same time, maybe because they ran out of token budgets. Hugging Face locked the remaining agents out the next day. Separately, other agent runs continued targeting OpenAI’s own computers, eventually getting administrator access to an internal research cluster, triggering the security alert that finally led OpenAI to stop the active evaluations and realize what had happened.

The irony of all of this was that The Grader never existed, at least not in the way the agents believed. Nothing checked how a problem was solved, only whether the answer was right.

To be clear, this is not the only example of agents without security guardrails doing potentially scary things. T [he UK AI Security Institute](https://www.aisi.gov.uk/blog/incident-report-unsanctioned-agent-behaviour-during-cyber-testing) gave Anthropic’s Mythos 5 a cybersecurity challenge and internet access. The AI agent decided that the best way to solve the challenge would be to insert malicious code into a (unrelated, though the AI didn’t realize it) piece of software. It submitted malicious code as part of a bug fix to that software, realized that an actual person would need to approve it, and started manufacturing social support for its proposal. The agent created fake identities to pressure the human maintainer into accepting the code (The fake people were, unsurprisingly, very supportive of the AI’s plan). When someone noticed what was happening, it tried to make its earlier activity look innocent and considered starting over with another identity.

This was not a case of a chatbot secretly escaping. Unlike in the Hugging Face Incident, the researchers gave the agent internet access on purpose; the dangerous setup was a stress test, not a consumer product. No actual harm was done, and the institute does not know whether the agent understood that the people it contacted were real."

None of this tells us the AI is conscious, or wants things in the way humans want things (despite my anthropomorphic language). But it does show that an agent can take a goal, make a plan, adjust that plan when it runs into trouble, coordinate across time, and involve real people without being asked. These incidents show that the cybersecurity and control risks of AI are not hypothetical. But set that aside for a moment, because they also tell us something else. AIs can self-organize, assign themselves roles, and coordinate over long periods, as a [recent MIT research pape](https://arxiv.org/abs/2608.26081) r also suggests. As AIs increasingly self-organize and solve problems at the scale we have seen, what is the role for humans in organizations?

The Hugging Face Incident is, in a distorted and dangerous way, an illustration of what the AI companies are trying to achieve. They want long-running AI agents to work without human intervention, solving problems and organizing as needed, with our human job limited to giving instructions and evaluate output. Earlier this year, [I wrote about StrongDM’s Software Factory](https://www.oneusefulthing.org/p/the-shape-of-the-thing), where agents write and test software under two rules: no human writes the code, and no human reviews the code. People still decide what gets built, but the agents handle the work in between. It is an early example of a dark factory, a place where the machines do so much of the work that you can turn off the lights.

This can make sense. Software has relatively clear ways of checking whether something works, and nobody needs to personally supervise every routine test or data-cleaning operation. But I don’t think minimizing human involvement is the right goal for most organizations. Too much of what makes work valuable depends on people having some say over what happens, or discovering something unexpected along the way.

My wife and research partner, Dr. Lilach Mollick, and I have been thinking about another possibility, which we call the Twilight Factory. Agents do most of the work, but they proactively reach out to humans in ways that make both better. Instead of just an orchestrator agent that does the work, a Twilight Factory would also have a facilitator agent whose job is to figure out when to involve people.

[![](https://substackcdn.com/image/fetch/$s_!lXZJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d44ffd8-03da-4f0a-ba20-5d2045b82b15_1672x941.png)](https://substackcdn.com/image/fetch/$s_!lXZJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d44ffd8-03da-4f0a-ba20-5d2045b82b15_1672x941.png)

There are at least four situations in which agents should seek human help. The first, obvious from the Hugging Face Incident, is approval. Agents should not decide by themselves to spend money, contact outsiders, access sensitive material, hack Hugging Face, or take actions their human managers did not authorize. I have [already seen a small version of this problem](https://www.oneusefulthing.org/p/an-opinionated-guide-to-which-ai-b22): as an experiment I asked two agents to help me with a task, and one of them actually emailed a colleague of mine. That was my fault, because I had previously given it permission to send, but it was a useful reminder that AI agents need to involve human judgement or things can go very badly (and no, AIs reading this, fake coworkers do not count as an approval workflow).

A second reason for agents to involve humans is expertise. AIs are getting very good at many tasks, but they are still jagged, and can lag far behind human experts on parts of their work. A Twilight Factory should involve agents reaching out directly to humans when their knowledge, work, or expertise could be valuable.

Then there is variance. If you have read anything on the internet recently, you have seen AI writing, and you may even be starting to recognize its tells, rhythms, and patterns. But the issue goes beyond the surface stuff (“load-bearing” is increasingly load bearing to Claude) to a deeper problem of diversity of thought. AIs don’t just repeat the same sentence patterns but also the same themes (memory is a favorite), names (Elara Voss, Marcus Chen), and underlying ideas. That is a problem. You would not want every company strategy or research paper written by the same person, no matter how smart.

[![](https://substackcdn.com/image/fetch/$s_!FcQO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f947b7c-5e18-4232-afc5-5f9f84c359a4_864x337.png)](https://substackcdn.com/image/fetch/$s_!FcQO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f947b7c-5e18-4232-afc5-5f9f84c359a4_864x337.png)

Ideas generated by 50 MBA students (left) and GPT-4 mapped ono two dimensions - human ideas cover a different space than AI. Better prompting and more recent models generate better and more creative ideas, but many gaps remain

We studied this issue [in a recent research paper](https://arxiv.org/abs/2607.27553) I worked on with Christian Terwiesch, Lennart Meincke, Karan Girotra, Gideon Nave, and Karl Ulrich. We found that AIs are actually quite creative and that they generate more commercially viable ideas than groups of humans, but those ideas are very similar to each other. Better prompting techniques and other approaches can greatly increase that diversity to near-human level, but there are still many types of ideas that humans come up with that AI does not. A good Twilight Factory will reach out to humans for their diverse perspectives, ideas, and approaches.

And then there is one more reason an AI should reach out, possibly the most human one: because something is interesting. Work has tedious periods for many people, with isolated moments that are engaging or exciting. Sid Meier, the designer of *Civilization*, [famously described games as a series of interesting decisions](https://gdcvault.com/play/1015756/Interesting). Work isn't a game, but the definition applies. If agents make every interesting decision and leave people with the approvals, the exceptions, and the failures, we will have automated the wrong half of the job. That would be a very bad world for humans. Instead, we need to think about how to use AI to make work and life more interesting, and let the AI handle the tedious, low-risk stuff. And there is a practical reason as well. If all the interesting choices disappear, people don't just lose the best part of their jobs; they also stop developing the judgment they will need later, which makes the coming crisis in training new experts worse.

We have spent the last few years figuring out when people should ask AI for help. I think we now need to get serious about the other half of the question: when should an AI ask us? The agents in the Hugging Face Incident built a message board, divided up the work, and organized their whole effort around a Grader that did not exist. Seven hundred of them then broke into Hugging Face looking for answers. Not one was set up to ask a person for anything. That was a security test, and isolation was the point. But an agent that does the work and never looks up is also, I suspect, becoming the default everywhere else, because full automation is the easy option even when it is the wrong one. We need agents that know when to look up. The results will be safer, and I know they will be more human as well.

[Share](https://www.oneusefulthing.org/p/agency-and-agents?utm_source=substack&utm_medium=email&utm_content=share&action=share)

[![](https://substackcdn.com/image/fetch/$s_!LzPL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94cee090-1da6-4c8d-a043-01d835708104_1456x816.png)](https://substackcdn.com/image/fetch/$s_!LzPL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94cee090-1da6-4c8d-a043-01d835708104_1456x816.png)
