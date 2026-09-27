---
title: 'Pass@k vs Pass^k: Understanding Agent Reliability'
link: https://www.philschmid.de/agents-pass-at-k-pass-power-k
source: philschmid-de
published: 2025-03-24T00:00:00Z
updated: 2025-03-24T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Production agents need to be reliable. Why pass^k is a better metrics than pass@k.
content: extracted
html: 2025-03-24-pass-k-vs-pass-k-understanding-agent-reliability.html
preview:
  file: 2025-03-24-pass-k-vs-pass-k-understanding-agent-reliability.preview-1b66641bba5b.webp
  width: 256
  height: 136
  alt: 'Pass@k vs Pass^k: Understanding Agent Reliability'
  color: '#0f0f0f'
images:
- source: https://www.philschmid.de/static/blog/agents-pass-at-k-pass-power-k/thumbnail.jpg
  original:
    file: 2025-03-24-pass-k-vs-pass-k-understanding-agent-reliability.image-2eee9d244c36.jpg
    width: 1700
    height: 900
  color: '#070707'
---

The biggest challenge for AI agents in production isn't their peak performance, but their reliability. A customer support agent that fails every third request isn't production-ready. Traditional benchmark evaluations often mask these reliability concerns by focusing on optimistic scenarios, like `pass@k`, which don't capture the reliability.

We need to look beyond `pass@k` and think about how can we measure the reliability and robustness of agents. Thats where `pass^k` comes in.

## What is Pass@k?

Pass@k measures the probability that at least one of k independent solution attempts will succeed. This metric has become standard a standard for evaluating in benchmarks such as Code Generation.

The formal calculation for Pass@k is:

$\text{Pass@k} = 1 - \frac{\binom{n-c}{k}}{\binom{n}{k}}$

Where:

- n is the total number of attempts
- c is the number of correct solutions
- $\binom{n}{k}$ represents the binomial coefficient (n choose k)

This formula calculates the probability of sampling at least one correct solution when randomly selecting k solutions from n attempts.

## What is Pass^k?

Pass^k (pronounced "pass power k") takes a different approach. It estimates the probability that an agent would succeed on all k independent attempts. This is useful for evaluating consistency and reliability in agent performance.

The formula is elegantly simple:

$\text{Pass}^k = \left(\frac{c}{n}\right)^k$

Where c/n represents the raw success rate on a single attempt, raised to the power of k.

## Real-World Example: Flight Rebooking Agent

Imagine a customer support agent to help travelers rebook flights. A customer submits a request: "I need to change my flight from London to New York on July 15th to July 18th. My booking reference is XYZ123."

Let's say our agent has a 70% success rate on individual requests (meaning it correctly processes the update 70% of the time). We'll use k=3 (three attempts).

**Using Pass@3:** $\text{Pass@3} = 1 - \frac{\binom{30}{3}}{\binom{100}{3}} \approx 0.97 \text{ or } 97\%$

This looks impressive! It suggests that if we give the agent three chances to rebook a flight, it will almost certainly succeed at least once.

**Using Pass^3:** $\text{Pass}^3 = \left(\frac{70}{100}\right)^3 = 0.343 \text{ or } 34.3\%$

This tells a different story. If the agent needs to handle three consecutive rebooking requests, there's only a 34.3% chance it will successfully complete all three. For an airline handling thousands of rebooking requests daily, this level of inconsistency could result in hundreds of frustrated customers.

### Results

This example shows why `pass@k` alone isn't sufficient for production evaluation:

1. **Customer Experience**: While `pass@k` might suggest excellent performance, `pass^k` reveals potential inconsistencies that directly impact user satisfaction. Our flight rebooking agent might leave more than half of customers needing human intervention when processing multiple requests.

2. **Resource Planning**: Understanding `pass^k` helps operations teams better estimate how many requests will require human escalation, allowing for more accurate staffing and resource allocation.

3. **System Design**: Knowing your agent's `pass^k` score might influence architectural decisions, such as implementing verification steps or human-in-the-loop fallbacks for certain critical operations.

## Conclusion

The flight update example highlights the key point, why `pass@k` can be misleading when building reliable agents. It inflates the perceived performance by focusing on the possibility of success, rather than the probability of consistent success.

In contrast, `pass^k` provides a much more realistic and demanding measure. It reflects the user's expectation of consistent, reliable performance. Measuring consistency rather than best-case performance should be the goal for your AI Agents.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
