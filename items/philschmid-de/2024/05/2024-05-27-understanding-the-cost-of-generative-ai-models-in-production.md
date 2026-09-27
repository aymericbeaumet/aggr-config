---
title: Understanding the Cost of Generative AI Models in Production
link: https://www.philschmid.de/cost-generative-ai
source: philschmid-de
published: 2024-05-27T00:00:00Z
updated: 2024-05-27T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The cost of deploying Generative AI models is very shallow, many people are fixated on raw compute pricing. But in reality, the cost is much more complex.
content: extracted
html: 2024-05-27-understanding-the-cost-of-generative-ai-models-in-production.html
preview:
  file: 2024-05-27-understanding-the-cost-of-generative-ai-models-in-production.preview-5c575e8678e2.webp
  width: 256
  height: 128
  alt: Understanding the Cost of Generative AI Models in Production
  color: '#5b4f4e'
images:
- source: https://www.philschmid.de/static/blog/tco-generative-ai/thumbnail.jpg
  original:
    file: 2024-05-27-understanding-the-cost-of-generative-ai-models-in-production.image-452c156536d7.jpg
    width: 1200
    height: 600
  color: '#161716'
---

The cost of deploying Generative AI models is very shallow, many people are fixated on raw compute pricing. Statements like "we're cheaper" often dominate discussions and decision making. But in reality, the cost of deploying Generative AI models in production is much more complex and involves various factors beyond just compute resources.

This post aims to provide a more comprehensive understanding the total cost of ownership (TCO) for Generative AI Models. Deploying a Generative AI model requires more than a VM with a GPU. It normally includes:

- **Container Service**: Most often Kubernetes to run LLM Serving solutions like Hugging Face Text Generation Inference or vLLM.
- **Compute Resources**: GPUs for running models, CPUs for management services
- **Networking and DNS**: Routing traffic to the appropriate service, Ingress & Egress.
- **Load Balancing**: Distributes incoming traffic needed for scaling
- **Autoscaling**: Adjusts the number of models based on current demand to optimize resource utilization and cost.
- **Logging & Telemetry**: Collects and analyzes data on system performance and troubleshoot issues.
- **CI**: Continuous integration systems that allow for seamless updates and the ability to revert to previous versions if needed.

### **Hidden Costs of Deploying Generative AI Models**

Hidden costs can impact the total cost of ownership (TCO) when deploying Generative AI models. These hidden costs are often unrecognized at the beginning and come gradually. Self-built solutions may appear cheaper initially, particularly in terms of raw compute costs.

However development time, and maintenance can offset these savings. Hiring skilled data scientists, machine learning engineers, and DevOps professionals can be expensive and time consuming. Using available resources for “reimplementing” solutions hinder innovation and lead to a lack of focus. Since You not longer work on improving your model or product.

Unless building and maintaining such infrastructure is the USP of your company, opting for managed services that offer integrated support and faster time to market can be more cost-effective in the long run.

To illustrate this let's consider a hypothetical scenario of deploying LLM on a single A100 80GB using a managed service versus building it yourself with a cheaper compute service.

*Note: For compute i used [1.89$/h](https://www.runpod.io/). For other cost i went with [AWS pricing](https://calculator.aws/#/). Some compute provider might not offer services for Networking or Logging our even Kubernetes, meaning you would need to roll them yourself leading to more development resources and time needed.*

**Infrastructure cost**

| Cost                          | Managed Service | Self-Built Solution |
| ----------------------------- | --------------- | ------------------- |
| Compute (1x A100 80GB)        | -               | $1.89/hr            |
| Storage (500GB)               | -               | $0.09/hr            |
| Domain / IP (1x)              | -               | $0.01/hr            |
| Networking (100GB)            | -               | $0.05/hr            |
| Load Balancer (1x)            | -               | $0.03/h             |
| Container Services (k8s)      | -               | $0.28/hr            |
| Logging and Monitoring (25GB) | -               | $0.05/hr            |
| Total                         | $4/h            | $2.40/h             |
| Total + 1 Engineer\*          | $4/h            | $70.9/h             |

*[\* based on Machine learning engineer salary in United States from indeed](https://www.indeed.com/career/machine-learning-engineer/salaries)*

Including all required services the total cost of the self-built solution is increased 27% over the raw compute prices. It still appears to be cheaper in costs, but if you include development time and maintenance for just one additional Engineer its offset. This 1 engineer would now need to manage ~43 models to amortize its cost.

### Conclusion

Understanding the cost for deploying Generative AI Models in production is important for making informed decisions. Raw compute pricing is an important factor, but it's just one piece of the puzzle. Infrastructure costs, development time, and operational overheads play a role similar to non-monetary factors, like stability of providers. Startups with aggressive prices could be VC subsidized, disappear in the future or have thin margins for less flexibility in long term negotiations.

Whether you are an individual, a startup or a company, don't let you fool by raw compute prices unless you want raw ssh access! Value your time!

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
