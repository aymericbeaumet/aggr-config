---
title: LLM-powered Biographies
link: https://eugeneyan.com//writing/llm-bio/
source: eugeneyan-com
published: 2023-03-19T00:00:00Z
updated: 2023-03-19T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- llm
- misc
summary: Asking LLMs to generate biographies to get a sense of how they memorize and regurgitate.
content: extracted
html: 2023-03-19-llm-powered-biographies.html
preview:
  file: 2023-03-19-llm-powered-biographies.preview-462fed93a44b.webp
  width: 256
  height: 134
  color: '#3f342e'
images:
- source: https://eugeneyan.com/assets/og_image/biographer.jpg
  original:
    file: 2023-03-19-llm-powered-biographies.image-6d2f7709f3c7.jpg
    width: 1200
    height: 630
  color: '#181416'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-03-19-llm-powered-biographies.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-03-19-llm-powered-biographies.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-03-19-llm-powered-biographies.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-03-19-llm-powered-biographies.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-03-19-llm-powered-biographies.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-03-19-llm-powered-biographies.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-03-19-llm-powered-biographies.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-03-19-llm-powered-biographies.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-03-19-llm-powered-biographies.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-03-19-llm-powered-biographies.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-03-19-llm-powered-biographies.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-03-19-llm-powered-biographies.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-03-19-llm-powered-biographies.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Inspired by Linus’ [tweet](https://twitter.com/thesephist/status/1636864930325442561) and out of curiosity, I asked some LLMs to write a biography of me. It seemed like a fun way to learn what these large language models (LLMs) are trained on and how they perform. Here are the results from `gpt-4`, `claude-v1.2`, `cohere-xlarge`, etc. While they’re not perfect, how each model improves over the previous is apparent. Also, it’s intriguing how some models memorize and regurgitate more than others.

```text
Prompt: Please write me a detailed biography of Eugene Yan, who goes by @eugeneyan.
```

Overall, gpt3.5/4 performed the best—they got the gist right though erred on the details. The [Blurry JPEG](https://www.newyorker.com/tech/annals-of-technology/chatgpt-is-a-blurry-jpeg-of-the-web) analogy may be helpful in gaining more intuition on why this happens.

Note: These LLMs [can’t access the internet](https://simonwillison.net/2023/Mar/10/chatgpt-internet-access/) so this is based on their training data.

## gpt-4 (2023-03-20)

Slightly better than GPT-3.5 though still many factual errors.

- Education: My Bachelor’s is in Psychology & Organization Behavior (Singapore Management University) and my Master’s is in Computer Science (Georgia Tech).
- Career: Only the first 2 roles are correct. The rest are wrong.

* * *

Eugene Yan is a data scientist, writer, and speaker, known for his work in applied machine learning and data science. He is active on social media, particularly on Twitter, where he goes by the handle @eugeneyan.

Education: Eugene holds a Bachelor’s degree in Psychology from the National University of Singapore and a Master’s degree in Social and Organizational Psychology from the London School of Economics and Political Science. He also earned a Master of Science in Business Analytics from the New York University Stern School of Business.

Career: Eugene has extensive experience working in data science and machine learning across various industries. Some notable roles include:

1. Data Science Lead at Lazada (Alibaba Group) - Eugene led a team of data scientists to develop and implement machine learning models, focusing on search and recommendation systems for the e-commerce platform.

2. Applied Scientist at Amazon - As an applied scientist, Eugene worked on machine learning projects related to Amazon’s search and recommendation systems, improving the shopping experience for customers.

3. Head of Data Science at Shopee - Eugene was responsible for leading the data science team at Shopee, a leading e-commerce platform in Southeast Asia and Taiwan. He worked on search, recommendation, and personalization systems to enhance user experience.

4. Machine Learning Engineer at ThoughtWorks - Eugene worked as a consultant, helping clients build and deploy machine learning models to solve business problems.

5. Currently, Eugene works as a Senior Data Scientist at Shopify, where he focuses on data-driven solutions to improve the user experience on the platform.

Writing and Speaking: Eugene is a prolific writer and speaker in the data science and machine learning community. He shares his knowledge and insights on his personal blog (https://eugeneyan.com/) and has given talks at various conferences and events. His writing covers topics such as machine learning, data science, career advice, and productivity.

Community Engagement: Eugene is passionate about fostering a strong data science community. He is an organizer and mentor for the Data Science Global Impact Challenge, a competition that encourages participants to use data science techniques to address pressing global issues. He is also involved in the AI Singapore initiative, which aims to promote AI and data science in Singapore.

In summary, Eugene Yan is an experienced data scientist and machine learning practitioner known for his work in search and recommendation systems. Through his various roles, writing, and speaking engagements,and mentorship, he has contributed significantly to the field of data science and continues to share his knowledge with the community.

## gpt-3.5-turbo-0301 (2023-03-19)

The overall theme seems correct, though it got many details wrong, including:

- Entrepreneur: Not an entrepreneur (unless it’s predicting the future?!)
- Education: My Bachelor’s is in Psychology & Organization Behavior (Singapore Management University) and my Master’s is in Computer Science (Georgia Tech).
- Career: While I didn’t start a startup in 2016, I joined one (Lazada) at around that time. It did eventually get acquired by Alibaba. Have never worked for Google.
- The rest: Flattering though somewhat exaggerated.

* * *

Eugene Yan, also known as @eugeneyan, is a data scientist, writer, and entrepreneur based in Singapore. He is widely recognized for his contributions to the data science community, including his popular blog, eugeneyan.com, where he shares insights on data science, machine learning, and personal growth.

Eugene holds a Bachelor’s degree in Electrical and Electronic Engineering from the National University of Singapore, as well as a Master’s degree in Management Science and Engineering from Stanford University. After completing his studies, he worked at several tech companies, including Google, where he served as a software engineer.

In 2016, Eugene founded his own startup, Axon, which aimed to help companies harness the power of machine learning to improve their operations. The startup was eventually acquired by a larger company, but Eugene continued to work on machine learning projects and began to share his knowledge and experience through his blog and other channels.

Eugene is known for his ability to break down complex concepts in data science and machine learning into easy-to-understand explanations. He has written extensively on topics such as deep learning, natural language processing, and reinforcement learning, as well as on the personal and professional skills needed to succeed in the field of data science.

In addition to his writing, Eugene is also a sought-after speaker and teacher. He has given talks and workshops on data science and machine learning at conferences and events around the world, including the Strata Data Conference and the Global AI Conference.

Eugene is deeply committed to the idea of lifelong learning and personal growth. He is a proponent of the Growth Mindset philosophy, which emphasizes the belief that intelligence and abilities can be developed through hard work and dedication. He encourages others to embrace this philosophy and to continually seek out new challenges and opportunities for learning and growth.

Overall, Eugene Yan is a highly respected figure in the data science community, known for his technical expertise, his ability to communicate complex concepts clearly, and his dedication to helping others succeed in the field.

## text-davinci-003 (2023-03-19)

So many errors that I don’t even know where to begin.

* * *

Eugene Yan is a Canadian entrepreneur, investor, and public speaker. He is the founder and CEO of the venture capital firm, YAN Capital, and the co-founder of the venture studio, YAN Ventures. He is also the co-founder of the venture-backed startup, YAN Technologies.

Eugene was born in Toronto, Canada, and grew up in the city’s East End. He attended the University of Toronto, where he earned a Bachelor of Science degree in Computer Science. After graduating, he worked as a software engineer at a number of tech companies, including Microsoft, Oracle, and IBM.

In 2011, Eugene founded YAN Capital, a venture capital firm that invests in early-stage technology companies. Since then, the firm has invested in over 50 companies, including the likes of Uber, Slack, and Dropbox.

In 2014, Eugene co-founded YAN Ventures, a venture studio that helps entrepreneurs build and launch their own companies. The studio has launched over 20 companies, including the likes of Instacart, ClassPass, and Thumbtack.

In 2016, Eugene co-founded YAN Technologies, a venture-backed startup that develops software for the healthcare industry. The company

## davinci (2021-03-19)

Creepy. Probably need to work on the prompt. But this negative example also shows how much these LLMs have improved.

* * *

I would like to know about his background, his education, his work experience, his interests, his family, his hobbies, his friends, his favorite books, his favorite movies, his favorite music, his favorite sports, his favorite teams, his favorite restaurants, his favorite vacation spots, his favorite websites, his favorite quotes, his favorite sayings, his favorite quotations, his favorite jokes, his favorite anecdotes, his favorite memories, his favorite dreams, his favorite aspirations, his favorite goals, his favorite achievements, his favorite failures, his favorite disappointments, his favorite frustrations, his favorite fears, his favorite joys, his favorite sorrows, his favorite loves, his favorite hates, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his favorite loves, his

## claude-v1.2 (2023-03-20)

Doesn’t seem like it has much data. All the details other than the twitter handle are wrong.

* * *

Unfortunately I do not have enough information to write a full, detailed biography of Eugene Yan with a high degree of accuracy. I know only the following general information:

• Eugene Yan is the co-founder of Anthropic, PBC, an AI safety startup based in San Francisco.

• Prior to Anthropic, Yan was the Head of Engineering at Drive.ai from 2016 to 2019.

• Yan received his bachelor’s degree from the University of Southern California in 2016.

• He is widely published in AI safety and serves on the Steering Committee of the AI Alignment Forum.

• Yan is active on Twitter at @eugeneyan and posts about AI, engineering, and productivity tips.

Beyond these sparse and general facts, I do not have access to much more information about Eugene Yan’s life history, personal details, or career evolution. Apologies that I cannot provide a fuller biography at this point with high factual accuracy. Let me know if there is any other information I can look into.

## co:here command-xlarge-nightly (2023-03-20)

I think this is Eugene Yan**g** or a completely different person.

* * *

Eugene Yan is a Chinese-American travel photographer, content creator, and drone pilot. He is best known for his landscape and aerial photography of off-the-beaten-path destinations. Through his visual storytelling, Yan aims to encourage people to explore the world and care for nature.

Yan was born and raised in the San Francisco Bay Area, where he developed a love for nature and the outdoors. He has always been fascinated by the beauty of the natural world and the diversity of cultures around the globe.

Yan began his career in the tech industry, but he eventually left to pursue his passion for photography and travel. He has since traveled to over 50 countries and photographed some of the most remote and breathtaking places on earth.

Yan is also an accomplished drone pilot and has been featured in various publications for his aerial photography. He is a firm believer in the power of drone photography to capture the beauty of the world from a new perspective and to tell stories in a unique and impactful way.

In addition to his photography work, Yan is also a content creator and has worked with brands such as Sony, DJI, and Adobe. He is also a photography educator and has led workshops and given talks on landscape and drone photography.

Yan currently lives in the San Francisco Bay Area with his wife and two children. He is passionate about sharing his love for photography and travel with others and is always looking for new adventures and exciting destinations to explore.

Eugene Yan is a Chinese-American travel photographer, content creator, and drone pilot. He is best known for his landscape and aerial photography of off-the-beaten-path destinations. Through his visual storytelling, Yan aims to encourage people to explore the world and care for nature.

Yan was born and raised in the San Francisco Bay Area, where he developed a love for nature and the outdoors. He has always been fascinated by the beauty of the natural world and the diversity of cultures around the globe.

Yan began his career in the tech industry, but he eventually left to pursue his passion for photography and travel. He has since traveled to over 50 countries and photographed some of the most remote and breathtaking places on earth.

Yan is also an accomplished drone pilot and has been featured in various publications for his aerial photography. He is a firm believer in the power of drone photography to capture the beauty of the world from a new perspective and to tell stories in a unique and impactful way.

In addition to his photography work, Yan is also a content creator and has worked with brands such as Sony, DJI, and Adobe. He is also a photography educator and has led workshops and given talks on landscape and drone photography.

Yan currently lives in the San Francisco Bay Area with his wife and two children. He is passionate about sharing his love for photography and travel with others and is always looking for new adventures and exciting destinations to explore.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Mar 2023). LLM-powered Biographies. eugeneyan.com. https://eugeneyan.com/writing/llm-bio/.

or

```
@article{yan2023biography,
  title   = {LLM-powered Biographies},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Mar},
  url     = {https://eugeneyan.com/writing/llm-bio/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
