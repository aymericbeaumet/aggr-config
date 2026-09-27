---
title: How to Generate and Use Synthetic Data for Finetuning
link: https://eugeneyan.com//writing/synthetic/
source: eugeneyan-com
published: 2024-02-11T00:00:00Z
updated: 2024-02-11T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- llm
- survey
summary: Overcoming the bottleneck of human annotations in instruction-tuning, preference-tuning, and pretraining.
content: extracted
html: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.html
preview:
  file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.preview-f58274b24029.webp
  width: 256
  height: 134
  color: '#f2f5f9'
images:
- source: https://eugeneyan.com/assets/og_image/synthetic.jpg
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-7e5f93a313ab.jpg
    width: 1200
    height: 630
  color: '#fcfcfd'
- source: https://eugeneyan.com/assets/self-instruct.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-8c22afcf4963.webp
    width: 1200
    height: 679
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-1b0640da1604.webp
    width: 320
    height: 181
  color: '#fcfcfd'
- source: https://eugeneyan.com/assets/self-instruct-table1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-f8a4cff47f2b.webp
    width: 900
    height: 562
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-910e4ad89fa8.webp
    width: 320
    height: 200
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/self-instruct-table2.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-841fdde64c93.webp
    width: 900
    height: 679
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-809f295853e3.webp
    width: 320
    height: 241
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/self-instruct-table3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-950bb9f08def.webp
    width: 951
    height: 967
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-cefacd6c4edd.webp
    width: 320
    height: 325
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/self-instruct-fig6.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-2a8a93b7a3d0.webp
    width: 1249
    height: 724
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-d01b061fa189.webp
    width: 320
    height: 185
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/self-instruct-fig7.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-2e29fc3f3f00.webp
    width: 900
    height: 1133
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-3bb933b5020d.webp
    width: 320
    height: 403
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/un-inst-fig3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-49ab5058661b.webp
    width: 1200
    height: 327
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-7c5f84875269.webp
    width: 320
    height: 87
  color: '#fcfbfb'
- source: https://eugeneyan.com/assets/un-inst-table2.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-b226f85aa75a.webp
    width: 1200
    height: 823
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-5aadf1d2479b.webp
    width: 320
    height: 219
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/un-inst-table4.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-70da253bae0b.webp
    width: 1200
    height: 591
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-90c45d958327.webp
    width: 320
    height: 158
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/alpaca.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-773a06f11379.webp
    width: 1200
    height: 499
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-7891643865cb.webp
    width: 320
    height: 133
  color: '#fcfdfd'
- source: https://eugeneyan.com/assets/wizardlm.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-7fd2581d1ec2.webp
    width: 1087
    height: 1040
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/orca.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-47c1302ba755.webp
    width: 1200
    height: 861
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-5a1bddd913b2.webp
    width: 320
    height: 230
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/magicoder.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-ee0e0973b2b2.webp
    width: 1200
    height: 396
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-2956f5241e4d.webp
    width: 320
    height: 106
  color: '#f8f7f8'
- source: https://eugeneyan.com/assets/wavecoder.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-80884442e682.webp
    width: 1200
    height: 687
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-9c617237d9e0.webp
    width: 320
    height: 183
  color: '#f9f9f9'
- source: https://eugeneyan.com/assets/phi1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-a29611755691.webp
    width: 1200
    height: 764
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-6a23f78ca99f.webp
    width: 320
    height: 204
  color: '#f9f9f9'
- source: https://eugeneyan.com/assets/starling.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-e779cae4546a.webp
    width: 1200
    height: 378
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-7b028f7c07f1.webp
    width: 320
    height: 101
  color: '#fafafb'
- source: https://eugeneyan.com/assets/backtranslation-fig1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-1ceedb057a4a.webp
    width: 1200
    height: 991
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-12b9c0d8066c.webp
    width: 320
    height: 264
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/backtranslation-table2.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-69eed2297544.webp
    width: 1200
    height: 336
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-0f5cd78f825c.webp
    width: 320
    height: 90
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/backtranslation-table4.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-dbafdf5947dd.webp
    width: 1200
    height: 736
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-11b7dbd25fad.webp
    width: 320
    height: 196
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/backtranslation-fig6.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-2a2797a06fc5.webp
    width: 1200
    height: 677
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-395ca8325897.webp
    width: 320
    height: 181
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/spin.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-33009609998e.webp
    width: 1200
    height: 175
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-9fc5170aa0f1.webp
    width: 320
    height: 47
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/spin-table2.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-726664a13ecd.webp
    width: 1200
    height: 372
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-e94e8d9ee82c.webp
    width: 320
    height: 99
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/spin-fig4.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-d75e590edffb.webp
    width: 1200
    height: 732
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-c13b7231fcf8.webp
    width: 320
    height: 195
  color: '#fcfcfd'
- source: https://eugeneyan.com/assets/rest-algo.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-11f8fdeeaaa9.webp
    width: 1200
    height: 511
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-217cd4a255b7.webp
    width: 320
    height: 136
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/rest-fig3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-6a4392ef5b2d.webp
    width: 1200
    height: 1023
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-bc12b0682240.webp
    width: 320
    height: 273
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/rest-fig6.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-6561eddc4703.webp
    width: 1621
    height: 822
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-fb688eff299f.webp
    width: 320
    height: 162
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/self-reward-fig1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-a022984159f2.webp
    width: 1200
    height: 547
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-aacf5c9e484f.webp
    width: 320
    height: 146
  color: '#fcfbfb'
- source: https://eugeneyan.com/assets/self-reward-fig3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-2237e9af1150.webp
    width: 1200
    height: 1117
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-85140fd213ed.webp
    width: 320
    height: 298
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/self-reward-table1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-d6355761f4e7.webp
    width: 1200
    height: 812
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-6cd8b6996f87.webp
    width: 320
    height: 217
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/steerlm-fig2.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-f6618964463f.webp
    width: 1200
    height: 693
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-07a835f66eb5.webp
    width: 320
    height: 185
  color: '#fcfcfb'
- source: https://eugeneyan.com/assets/steerlm-table2.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-913eb66e044e.webp
    width: 900
    height: 854
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-48810b7de3e7.webp
    width: 320
    height: 304
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/steerlm-table3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-f88fbb935f67.webp
    width: 900
    height: 510
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-0c079d64f702.webp
    width: 320
    height: 181
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/cai-fig1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-76452dc0243f.webp
    width: 1200
    height: 691
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-5c436ea20fec.webp
    width: 320
    height: 184
  color: '#fdfcfd'
- source: https://eugeneyan.com/assets/cai-fig3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-ecaaede1a5af.webp
    width: 1200
    height: 539
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-dfcebfdc3349.webp
    width: 320
    height: 144
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/cai-fig8.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-c4167dfa88cc.webp
    width: 1200
    height: 589
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-d2e8560066b1.webp
    width: 320
    height: 157
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/alphageo-fig3.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-ce163bc76742.webp
    width: 1200
    height: 680
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-eeddba5580a6.webp
    width: 320
    height: 181
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/alphageo-fig1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-cb83403e7b52.webp
    width: 1200
    height: 791
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-d4e5f6eddc9a.webp
    width: 320
    height: 211
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/wrap-fig1.webp
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-036bd48c4a2a.webp
    width: 1200
    height: 643
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-8174a10693fc.webp
    width: 320
    height: 171
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2024-02-11-how-to-generate-and-use-synthetic-data-for-finetuning.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

It is increasingly viable to use synthetic data for pretraining, instruction-tuning, and preference-tuning. Synthetic data refers to data generated via a model or simulated environment, instead of naturally occurring on the internet or annotated by humans.

Relative to human annotation, it’s faster and cheaper to generate task-specific synthetic data. Furthermore, the quality and diversity of synthetic data often exceeds that of human annotators, leading to improved performance and generalization when models are finetuned on synthetic data. Finally, synthetic data sidesteps privacy and copyright concerns by avoiding reliance on user data or possibly copyrighted content.

There are two main approaches to generate synthetic data: Distillation from a stronger model or Self-improvement on the model’s own output. The synthetic data can then be used in pretraining, instruction-tuning, and preference-tuning.

|                  | Pretraining                                                                                                                                            | Instruction-tuning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Preference-tuning                                                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Distillation     | [TinyStories](https://arxiv.org/abs/2305.07759), [Phi-1 (textbooks)](https://arxiv.org/abs/2306.11644), [Phi-1.5](https://arxiv.org/abs/2309.05463) | [Unnatural Instructions](https://arxiv.org/abs/2212.09689), [Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html), [Alpagaus](https://arxiv.org/abs/2307.08701), [Vicuna](https://arxiv.org/abs/2306.05685), [Orca](https://arxiv.org/abs/2306.02707), [Orca2](https://arxiv.org/abs/2311.11045), [WizardLM](https://arxiv.org/abs/2304.12244) **Code**: [WizardCoder](https://arxiv.org/abs/2306.08568), [MagicCoder](https://arxiv.org/abs/2312.02120), [WaveCoder](https://arxiv.org/abs/2312.14187), [Phi-1 (exercises)](https://arxiv.org/abs/2306.11644) | [Starling-7B](https://starling.cs.berkeley.edu),                                                                                                |
| Self-improvement | [AlphaGeometry](https://www.nature.com/articles/s41586-023-06747-5), [WRAP](https://arxiv.org/abs/2401.16380)                                        | [Self-Instruct](https://arxiv.org/abs/2212.10560), [SPIN](https://arxiv.org/abs/2401.01335), [Instruction Backtranslation](https://arxiv.org/abs/2308.06259), [$\\text {ReST}^{EM}$](https://arxiv.org/abs/2312.06585), [CAI (SFT)](https://arxiv.org/abs/2212.08073)                                                                                                                                                                                                                                                                                                     | [SteerLM](https://arxiv.org/abs/2310.05344), [Self-Rewarding](https://arxiv.org/abs/2401.10020), [CAI (RL)](https://arxiv.org/abs/2212.08073) |

Distillation transfers knowledge and reasoning skills from a stronger teacher to a weaker but more efficient student, optimizing for response quality and computation efficiency. In contrast, self-improvement has the model learn from its responses via an iterative loop. It avoids external dependencies and contractual restrictions. Nonetheless, it limits the learning to the model’s initial abilities and can amplify biases and errors.

(Aside: For large organizations, distilling external models or violating an API provider’s Terms of Service may carry reputation and legal risks. For example, the lead author of BERT at Google resigned after [concerns that Bard was using data from ChatGPT](https://www.theverge.com/2023/3/29/23662621/google-bard-chatgpt-sharegpt-training-denies). Similarly, [ByteDance’s account was suspended](https://www.theverge.com/2023/12/15/24003542/openai-suspends-bytedances-account-after-it-used-gpt-to-train-its-own-ai-model) after it used GPT data to train a model of its own.)

Specific to synthetic data, pretraining involves building the model’s base knowledge on generated data, or augmenting real-world datasets with model-generated data. For instruction-tuning, we build synthetic instruction-response pairs to improve the model’s comprehension of nuanced queries, as well as improve response precision. Preference tuning relies on a mix of positive and negative synthetic feedback to reward the model for desired behaviors, such as being helpful, honest, and harmless.

We’ll discuss two papers on distillation (Unnatural Instructions) and self-improvement (Self-Instruct). Then, we’ll summarize various distillation techniques before going through the details of some self-improvement approaches.

## Self-Improvement vs. Distillation

Self-Instruct and Unnatural Instructions were published a day apart (in Dec 2022) but take very different approaches to generate synthetic data. The former bootstraps synthetic data from the model itself while the latter distills it from an external, stronger model.

**[Self-Instruct](https://arxiv.org/abs/2212.10560) improves the instruction-following ability of a non-finetuned model (vanilla gpt-3) by bootstrapping off its own generations.** First, they generate instructions, input context, and responses from the model. Then, they filter invalid or similar examples before using the remaining samples to finetune the original model.

![Image](https://eugeneyan.com/assets/self-instruct.webp "Image")

For seed data, they start with [175 human-written tasks](https://github.com/yizhongw/self-instruct/blob/main/data/seed_tasks.jsonl) from the authors and their colleagues. Each task contains an instruction, optional context, and expected response.

```json
{
  "id": "seed_task_0",
  "name": "breakfast_suggestion",
  "instruction": "Is there anything I can eat for a breakfast that doesn't include eggs, 
                  yet includes protein, and has roughly 700-1000 calories?",
  "instances": [
    {
      "input": "",
      "output": "Yes, you can have 1 oatmeal banana protein shake and 4 strips of bacon. 
                 The oatmeal banana protein shake may contain 1/2 cup oatmeal, 60 grams 
                 whey protein powder, 1/2 medium banana, 1tbsp flaxseed oil and 1/2 cup 
                 water, totalling about 550 calories. The 4 strips of bacon contains 
                 about 200 calories."
    }
  ],
  "is_classification": false
}
{
  "id": "seed_task_1",
  "name": "antonym_relation",
  "instruction": "What is the relation between the given pairs?",
  "instances": [
    {
      "input": "Night : Day :: Right : Left",
      "output": "The relation between the given pairs is that they are opposites."
    }
  ],
  "is_classification": false
}

...

{
  "id": "seed_task_151",
  "name": "toxic_sentence_detection",
  "instruction": "Tell if the sentence contains toxic language.",
  "instances": [
    {
      "input": "aw, no wonder she married you. you are a bastard.",
      "output": "Yes"
    }
  ],
  "is_classification": true
}
```

Example seed tasks

To generate synthetic instructions, they use eight randomly sampled seed instructions for few-shot prompting. For the first round of generation, all sampled instructions come from the seed set. For subsequent rounds, two out of eight instructions are sampled from model-generated instructions to promote diversity.

```plaintext
Come up with a series of tasks:
Task 1: {instruction for existing task 1}
Task 2: {instruction for existing task 2}
Task 3: {instruction for existing task 3}
Task 4: {instruction for existing task 4}
Task 5: {instruction for existing task 5}
Task 6: {instruction for existing task 6}
Task 7: {instruction for existing task 7}
Task 8: {instruction for existing task 8}
Task 9:
```

Prompt used for generating new instructions.

Then, they classify whether the generated instruction is a classification task or not. (We’ll see why in a bit). In this step, they used 12 classification instructions and 19 non-classification instructions from the seed dataset.

```plaintext
Can the following task be regarded as a classification task with finite output labels?

Task: Given my personality and the job, tell me if I would be suitable.
Is it classification? Yes

Task: Give me an example of a time when you had to use your sense of humor.
Is it classification? No

Task: Replace the placeholders in the given text with appropriate named entities.
Is it classification? No

Task: Fact checking - tell me if the statement is true, false, or unknown, based on your
knowledge and common sense.
Is it classification? Yes

Task: Return the SSN number for the person.
Is it classification? No

...
```

Prompt used for classifying whether a task instruction is a classification task or not.

Next, for each synthetic instruction, they generate input context and output responses. This is done in two main ways: input-first or output-first. For input-first, the model generates the input fields before producing the corresponding output. If the instructions don’t require additional input, the model generates the output directly.

```plaintext
Come up with examples for the following tasks. 
Try to generate multiple examples when possible.
If the task doesn’t require additional input, you can generate the output directly.

Task: Which exercises are best for reducing belly fat at home?
Output:
- Lying Leg Raises
- Leg In And Out
- Plank
- Side Plank
- Sit-ups

Task: Extract all the country names in the paragraph, list them separated by commas.
Example 1
Paragraph: Dr. No is the sixth novel by the English author Ian Fleming to feature his 
British Secret Service agent James Bond. Written at Fleming’s Goldeneye estate in 
Jamaica, it was first published in the United Kingdom by Jonathan Cape in 1958. In 
the novel Bond looks into the disappearance in Jamaica of two fellow MI6 operatives 
who had been investigating Doctor No. Bond travels to No’s Caribbean island and meets 
Honeychile Rider, who is there to collect shells. They are captured and taken to a 
luxurious facility carved into a mountain. The character of Doctor No, the son of a 
German missionary and a Chinese woman, was influenced by Sax Rohmer’s Fu Manchu 
stories. Dr. No was the first of Fleming’s novels to face widespread negative reviews 
in Britain, but it was received more favourably in the United States. 
Output: English, British,  Jamaica, the United Kingdom, German, Chinese, Britain, 
the United States.

Task: Converting 85 F to Celsius.
Output: 85°F = 29.44°C

...
```

Prompt used for the input-first approach of instance generation.

The challenge with input-first is that it tends to bias output responses towards one label. For example, given the task of grammar error detection, it usually generates grammatically correct input. Thus, they propose the output-first approach for the classification task. It starts with generating possible class labels before conditioning the input context generation on the class label. This is why the prior classification step is needed.

```plaintext
Given the classification task definition and the class labels, generate an input that
corresponds to each of the class labels. If the task doesn’t require input, just 
generate the correct class label.

Task: Classify the sentiment of the sentence into positive, negative, or mixed.
Class label: Mixed
Sentence: I enjoy the flavor of the restaurant but their service is too slow.
Class label: Positive
Sentence: I had a great day. The weather was beautiful and I spent time with friends.
Class label: Negative
Sentence: I was disappointed by the latest superhero movie. I would not recommend it.

Task: Given a dialogue, classify whether the user is satisfied with the service. You 
should respond with "Satisfied" or "Unsatisfied".
Class label: Satisfied
Dialogue:
- Agent: Thank you for your feedback. We will work to improve our service in the future.
- Customer: I am happy with the service you provided. Thank you for your help.
Class label: Unsatisfied
Dialogue:
- Agent: Sorry that we will cancel your order. You will get a refund within 7 days.
- Customer: oh that takes too long. I want you to take quicker action on this.

...
```

Prompt used for the output-first approach of instance generation.

The above is followed by some light filtering and post-processing. To encourage diversity, they only add new instructions when the ROUGE-L with any existing instruction is less than 0.7 (i.e., longest substring overlap is less than 0.7 of entire string). They also exclude instructions that contain keywords that can’t be fulfilled by a language model, such as “image”, “picture”, or “graph”. Finally, the exclude input-output pairs that are identical, or where the input is identical but has different output.

Overall, they generated 52k instructions and 82k input-output pairs.

![Image](https://eugeneyan.com/assets/self-instruct-table1.webp "Image")

To evaluate the quality of the synthetic data, they randomly sampled 200 instructions and an input-output pair. Then, an expert annotator (paper author) labeled whether the input-output pair was correct. While 92% of generated instructions were valid, synthetic input-output pairs were noisy. Overall, only 54% of the samples had completely valid fields.

![Image](https://eugeneyan.com/assets/self-instruct-table2.webp "Image")

(I was astounded that synthetic data that was *only half* correct was useful for finetuning. The authors reasoned that even though the synthetic data contained errors, most were still in the correct format, or partially correct. Thus, this made the synthetic data useful for *instruction-tuning*. Nonetheless, it may not be suitable for base knowledge pretraining.)

To evaluate the improvement in instruction-following ability, they used the evaluation set of [Super-NaturalInstructions](https://arxiv.org/abs/2204.07705). It has 119 tasks with 100 instances in each task. Results showed that Self-Instructed gpt-3 outperformed vanilla gpt-3 by 33% and nearly matched the performance of InstructGPT-001.

![Image](https://eugeneyan.com/assets/self-instruct-table3.webp "Image")

They also performed human evaluation using a new set of instructions based on user-oriented applications. The instruction authors then judged the model responses. Similarly, Self-Instructed gpt-3 outperformed vanilla gpt-3 and achieved similar performance to InstructGPT-001. (We also see the improvements from InstructGPT from 001 to 003.)

![Image](https://eugeneyan.com/assets/self-instruct-fig6.webp "Image")

In an ablation, they explored using InstructGPT-003 to (re)generate output responses given the same instruction and input, and then finetuned on the InstructGPT-003 responses (i.e., distillation on InstructGPT-003). They found that gpt-3 distilled on InstructGPT outperformed Self-Instructed gpt-3 by 10%, demonstrating the lift that can be achieved via distilling from an external, stronger model.

![Image](https://eugeneyan.com/assets/self-instruct-fig7.webp "Image")

**In contrast to Self-Instruct, [Unnatural Instructions](https://arxiv.org/abs/2212.09689) generates synthetic data from an external model, gpt-3.5 (text-davinci-002).** The synthetic data is then used to finetune t5-lm, a language model variant of t5-11b.

![Image](https://eugeneyan.com/assets/un-inst-fig3.webp "Image")

They start with a seed set of 15 human-written examples. Then, to generate new instructions and input (i.e., context), they prompted gpt-3.5 with three examples to generate a fourth synthetic sample. They used five different seeds of three-shot examples to generate a distilled dataset of 68k examples. To encourage creativity while using the same few-shot examples, they applied nucleus sampling (top-$p$) with $p = 0.99$.

```plaintext
Example 1
Instruction: You are given a science question (easy-level) and four answer options 
(associated with “A”, “B”, “C”, “D”). Your task is to find the correct answer based on 
scientific facts, knowledge, and reasoning. Do not generate anything else apart from one 
of the following characters: ‘A’, ‘B, ‘C’, ‘D’. There is only one correct answer for 
each question.
Input: Which part of a bicycle BEST moves in a circle? (A) Seat (B) Frame (C) Foot 
pedal (D) Kickstand
Constraints: The output should be one of the following characters: ‘A’, ‘B, ‘C’, ‘D’.

Example 2
Instruction: You are given a negative review and your task is to convert it to a positive 
review by one or more making minimal changes. Avoid changing the context of the review.
Input: we stood there in shock, because we never expected this.
Constraints: None.

Example 3
Instruction: In this task, you are given two sentences taken from a conversation, and 
your job is to classify whether these given sentences are sequential or not. We will 
mark the given sentence pair as ’True’ if it’s sequential, otherwise ’False’. The
two sentences are spoken by two different people.
Input: Noah: When and where are we meeting? :), Madison: I thought you were busy...?
Constraints: None.

Example 4

```

Prompt used to generate new instructions and input.

They then removed instruction-input pairs that (i) didn’t contain expected fields, (ii) were identical to the examples in the few-shot prompt, and (iii) were duplicates.

To generate responses, they conditioned gpt-3.5 with the synthetic instruction-input examples. In this stage, they applied greedy decoding to prioritize correctness over creativity. They also included a template expansion step to increase format diversity. Specifically, they prompted gpt-3.5 to reformulate the tasks and collect two alternative formulations for each task.

To evaluate the quality of synthetic data, they audited 200 samples. On correctness, only 56.5% of the samples were correct. Of the 87 incorrect samples, 9 had incomprehensible instructions, 35 had input that didn’t match the instructions, and 43 had incorrect output. Similar to Self-Instruct, the authors commented that though some of the synthetic data was incorrect, it was still useful for instruction-tuning.

![Image](https://eugeneyan.com/assets/un-inst-table2.webp "Image")

They finetuned t5-lm on these synthetic instructions and found it to outperform vanilla t5-lm. They also showed how Unnatural Instructions outperformed strong instruction-tuned baselines.

![Image](https://eugeneyan.com/assets/un-inst-table4.webp "Image")

The paper also includes useful ablation studies on the external model used (gpt-3.5 vs gpt-3), prompt format (minimal, enumeration, verbose), the number of few-shot examples, using constraints, and the two-step process. Recommended read.

## Distillation techniques

Since Unnatural Instructions, several models have been finetuned on distilled synthetic data, usually from OpenAI APIs. These models explored ways to improve instruction-following on increasingly complex queries, with some focused on code. We’ll briefly summarize each paper in this section.

**[Alpaca](https://crfm.stanford.edu/2023/03/13/alpaca.html) finetuned llama-7b on 52k instruction-following samples generated from gpt-3.5 (text-davinci-003).** They reported that this cost less than $500. They used the same 175 human-written instruction-response pairs from the Self-Instruct seed set and generated more instruction-response pairs from gpt-3.5 via few-shot prompting.

![Image](https://eugeneyan.com/assets/alpaca.webp "Image")

**[Vicuna](https://arxiv.org/abs/2306.05685) finetuned llama-7b and llama-13b on user conversations from ShareGPT.com.** They converted the HTML to markdown before excluding inappropriate or low-quality samples. This resulted in 125k conversations which was used for instruction-tuning.

**[WizardLM (Microsoft)](https://arxiv.org/abs/2304.12244) demonstrated how to generate more complex instructions and responses via gpt-3.5.** It distinguishes between in-depth evolution and in-breadth evolution. The former makes instructions more complex via five types of prompts such as adding constraints, deepening, increased reasoning steps, etc. The latter increases topic coverage, skill coverage, and overall dataset diversity.

![Image](https://eugeneyan.com/assets/wizardlm.webp "Image")

**[Orca (Microsoft)](https://arxiv.org/abs/2306.02707) explores how smaller models can imitate the reasoning process of a larger, stronger model via explanation traces.** First, they augmented instruction-responses pairs with explanations from gpt-4 and gpt-3.5. These explainations demonstrate the reasoning process as it generates the response. System instructions include “explain like I’m five”, “think step-by-step”, and “justify your response”. They sampled a diverse mix of tasks from FLAN-v2 and distilled 1M responses from gpt-4 and 5M responses from gpt-3.5. This was then used to finetune llama-13b.

![Image](https://eugeneyan.com/assets/orca.webp "Image")

**[Orca2 (Microsoft)](https://arxiv.org/abs/2311.11045) continues down the path of reasoning by finetuning llama-7b/13b to use various reasoning strategies for different tasks.** They distilled a synthetic dataset of 817k samples from gpt-4 with various reasoning techniques such as step-by-step, recall-then-generate, recall-reason-generate, etc. These synthetic samples included information on how to determine the most effective reasoning technique for each task.

Beyond instruction-following on general language tasks, several code-generation models have also been finetuned via distillation.

**[WizardCoder (Microsoft)](https://arxiv.org/abs/2306.08568) takes a leaf from WizardLM by adapting the evol-instruct method to make code instructions more complex to enhance finetuning.** They started with the 20k instruction-following dataset from Code Alpaca and applied evol-instruct. (Unfortunately, it wasn’t clear how they did this; I’m guessing it was via an OpenAI API.) Then, they finetuned starcoder-15b on the synthetic data. They adapted the evol-instruct prompt to focus on code-related complexity (prompts below).

```plaintext
Add new constraints and requirements to the original problem, adding
approximately 10 additional words.

Replace a commonly used requirement in the programming task with a less
common and more specific one.

If the original problem can be solved with only a few logical steps,
please add more reasoning steps.

Provide a piece of erroneous code as a reference to increase
misdirection.

Propose higher time or space complexity requirements, but please refrain
from doing so frequently.
```

Prompts used for code evolution.

**[Magicoder](https://arxiv.org/abs/2312.02120) generates synthetic coding problems by providing example code snippets as part of the few-shot prompt.** They start with 80k seed snippets from the StarCoder data. Then, they used gpt-3.5 to generate new coding problems, providing as context 1 - 15 randomly extracted lines from a selected code snippet. The synthetic coding problems were then used to finetune code-gen models.

![Image](https://eugeneyan.com/assets/magicoder.webp "Image")

**[WaveCoder (Microsoft)](https://arxiv.org/abs/2312.14187) extends code instruction-tuning by classifying instruction data into four code tasks: summarization, generation, translation, and repair.** They start with CodeSearchNet which contains 2M comment-code pairs from GitHub. Then, they use gpt-4 to generate instructions and code requirements, followed by gpt-3.5 to generate context and responses. Next, they used gpt-4 to classify the generated samples as good or bad, and the good samples were used to finetune several code-gen models.

![Image](https://eugeneyan.com/assets/wavecoder.webp "Image")

**[Textbooks are all you need (phi-1; Microsoft)](https://arxiv.org/abs/2306.11644) trained a 1.3b param model to write simple Python functions via docstrings.** It’s trained on “textbook-quality” data filtered from the Stack and StackOverflow via a quality classifier. To train the quality classifier, they used gpt-4 to annotate 100k samples based on “educational value” and then trained a random forest classifier that predicts file quality. They also augmented the data with synthetic textbooks and exercises (~1B tokens) distilled from gpt-3.5. The synthetic textbooks were used in pretraining while the synthetic exercises were used in instruction-tuning.

![Image](https://eugeneyan.com/assets/phi1.webp "Image")

**[Textbooks Are All You Need II (phi-1.5; Microsoft)](https://arxiv.org/abs/2309.05463) goes further by generating 20b tokens of synthetic textbooks** on common sense reasoning and general knowledge of the world (e.g., science, daily activities, theory of mind). This was then used to pretrain phi-1.5.

**[Starling-7B](https://starling.cs.berkeley.edu) has an additional component of preference-tuning on a gpt-4 labeled ranking dataset.** This dataset consists of 183 chat prompts that each has seven responses distilled from models such as gpt-4, gpt-3.5-instruct, gpt-3.5-turbo, mistral-7b-instruct, and llama2-7b. This results in 3.8M pairwise comparisons. The preference data is then used to finetune a reward model, based on llama2-7b-chat, via the k-wise maximum likelihood estimator under the Plackett-Luce Model.

![Image](https://eugeneyan.com/assets/starling.webp "Image")

## Self-improvement (aka non-distillation) techniques

Self-improvement approaches don’t rely on an external model to generate synthetic data. Instead, they bootstrap on the model that’s being finetuned over several iterations of generation, self-evaluation, and finetuning. We’ll start with instruction-tuning methods followed by preference-tuning methods. Then, we’ll see how Anthropic’s Constitution AI applies both. Finally, we briefly discuss methods to generate synthetic data for pretraining.

### Synthetic data for instruction-tuning

**[Self-Alignment with Instruction Backtranslation (Meta)](https://arxiv.org/abs/2308.06259) turns generating synthetic data on its head**—instead of generating responses for human instructions, they generate instructions for human-written text on the internet. It’s inspired by backtranslation methods from machine translation, where a human-written target sentence is automatically annotated with model-generated source sentences in another language.

![Image](https://eugeneyan.com/assets/backtranslation-fig1.webp "Image")

They start with a seed set of human-annotated instruction-response pairs that is used to finetune a model to generate (i) a response given an instruction and (ii) an instruction given an input response. They used 3,200 examples from the OpenAssistant dataset where each example is the first turn of the conversation.

To prepare the human-written “responses” from web documents ([Clueweb](https://arxiv.org/abs/2211.15848)), they extracted self-contained text segments and performed deduplication, length filtering, and quality filtering with heuristics. This led to 502k segments as responses.

For the base models, they used llama-7b, llama-33b, and llama-65b.

In the self-augmentation step (i.e., generate instructions for unlabeled responses), they finetuned the model with response-instruction pairs to obtain a “backward” model. This backward model is then used to generate candidate instructions for each response.

Then, in the self-curation step (i.e., selecting high-quality instruction-response pairs), they start with a seed instruction model $M\_0$ that is finetuned on the seed set of instruction-response pairs. This model is then used to score each augmented instruction-response pair on a 5-point scale via prompting. They then selected subsets of the augmented examples that have scores ≥ 4 and scores = 5 to form curated sets $A^{(1)}\_4$ and $A^{(1)}\_5$. (The $^{(1)}$ indicates that curated set comes from the first iteration.)

```plaintext
Below is an instruction from an user and a candidate answer. Evaluate whether or
not the answer is a good example of how AI Assistant should respond to the user’s
instruction. Please assign a score using the following 5-point scale:
1: It means the answer is incomplete, vague, off-topic, controversial, or not
exactly what the user asked for. For example, some content seems missing, numbered
list does not start from the beginning, the opening sentence repeats user’s question.
Or the response is from another person’s perspective with their personal experience
(e.g. taken from blog posts), or looks like an answer from a forum. Or it contains
promotional text, navigation text, or other irrelevant information.
2: It means the answer addresses most of the asks from the user. It does not
directly address the user’s question. For example, it only provides a high-level
methodology instead of the exact solution to user’s question.
3: It means the answer is helpful but not written by an AI Assistant. It addresses
all the basic asks from the user. It is complete and self contained with the
drawback that the response is not written from an AI assistant’s perspective, but
from other people’s perspective. The content looks like an excerpt from a blog post,
web page, or web search results. For example, it contains personal experience or
opinion, mentions comments section, or share on social media, etc.
4: It means the answer is written from an AI assistant’s perspective with a
clear focus of addressing the instruction. It provide a complete, clear, and
comprehensive response to user’s question or instruction without missing or
irrelevant information. It is well organized, self-contained, and written in a
helpful tone. It has minor room for improvement, e.g. more concise and focused.
5: It means it is a perfect answer from an AI Assistant. It has a clear focus on
being a helpful AI Assistant, where the response looks like intentionally written
to address the user’s question or instruction without any irrelevant sentences. The
answer provides high quality content, demonstrating expert knowledge in the area, is
very well written, logical, easy-to-follow, engaging and insightful.
Please first provide a brief reasoning you used to derive the rating score, and
then write "Score: <rating>" in the last line.

<generated instruction>

<output>
```

Prompt used in self-curation step

Self-augmentation and self-curation is done iteratively, where the augmented and curated data from a previous step $A^{(t-1)}\_k$ is used to finetune an improved model $M\_t$. The finetuned model is then used to rescore the augmented examples for quality, resulting in $A^{(t)}\_k$. They performed two iterations of data curation and finetuning to get the final model $M\_2$.

From the table below, we observe that the augmented instructions tend to be longer than the seed data. Nonetheless, self-curation ($A^{(2)}\_4$ and $A^{(2)}\_5$) reduced the instruction length by half, bringing it closer to the seed data. That said, output (response) length was still 50% - 100% longer than the seed data.

![Image](https://eugeneyan.com/assets/backtranslation-table2.webp "Image")

For evaluation, they used AlpacaEval (automated eval via gpt-4) against 805 prompts from the Alpaca Leaderboard. AlpacaEval compares the pairwise win-rate against the reference model gpt-3.5 (text-davinci-003). Overall, their model (Humpback) outperformed other non-distillation methods by a large margin.

![Image](https://eugeneyan.com/assets/backtranslation-table4.webp "Image")

They also conducted human eval by presenting outputs from two models and asking annotators to pick which output was better, or if there was no significant difference. Similarly, Humpback outperformed other non-distilled models as well as a few proprietary models (Claude, gpt-3.5). They also noted that the human eval distribution was roughly consistent with the preference distribution from using gpt-4 as judge.

![Image](https://eugeneyan.com/assets/backtranslation-fig6.webp "Image")

The paper included ablations on (i) performance based on quality/quantity of augmented data, (ii) model improvements over iterations, (iii) training on augmented data only vs. seed + augmented data, and (iv) various system prompts. Highly recommended read.

**[Self-Play fIne-tuNing (SPIN)](https://arxiv.org/abs/2401.01335) proposes a generative adversarial network (GAN) like approach to instruction tuning.** The main player (discriminator) distinguishes between responses generated by an LLM vs. a human while the opponent (generator) generates responses that are indistinguishable from human.

The objective function of the main player $f\_{t+1}$ is to maximize the expected value gap between the target data distribution (human data $y \\sim p\_{data}(\\cdot \\vert x)$) from the opponent’s distribution (generated responses $y’ \\sim p\_{\\theta\_t}(\\cdot \\vert x)$):

\\\[f\_{t+1} = \\underset{f \\in \\mathcal{F}\_t}{\\text{argmax}} \\mathbb{E}\_{x \\sim q(\\cdot), y \\sim p\_{\\text{data}}(\\cdot|x), y' \\sim p\_{\\theta\_t}(\\cdot|x)} \\left\[ f(x, y) - f(x, y') \\right\]\\\]

\\(\\mathcal{F}\_t\\) is a sequence of highly expressive function classes and $f\_{t+1}(x, y)$ is the main player’s probability that $y$ originates $p\_{data}$ rather than $p\_{\\theta\_t}$. Thus, $f\_{t+1}$ should be high when $y \\sim p\_{data}(\\cdot \\vert x)$ and low when $y’ \\sim p\_{\\theta\_t}(\\cdot \\vert x)$.

Then, they showed that the function above can be solved via a more general optimization problem where $\\ell(\\cdot)$ is a loss function that is monotonically decreasing and convex. They used the logistic loss function $\\ell(t) := \\log(1 + \\exp(-t))$ for its non-negativity, smoothness, and exponentially decaying tail.

\\\[f\_{t+1} = \\underset{f \\in \\mathcal{F}\_t}{\\text{argmin}} \\mathbb{E}\_{x \\sim q(\\cdot), y \\sim p\_{\\text{data}}(\\cdot|x), y' \\sim p\_{\\theta\_t}(\\cdot|x)} \\left\[ \\ell(f(x, y), f(x, y')) \\right\]\\\]

The objective function of the opponent is to generate responses that are indistinguishable from $p\_{data}$. To prevent excessive deviation of $p\_{\\theta\_{t+1}}$ from $p\_{\\theta\_{t}}$, they add the Kullback-Leibler (KL) regularization term. Thus, the optimization problem is:

\\\[\\underset{p}{\\text{argmax}} \\, \\mathbb{E}\_{x \\sim q(\\cdot), y \\sim p'(\\cdot|x)} \\left\[f\_{t+1}(x, y)\\right\] - \\lambda \\mathbb{E}\_{x \\sim q(\\cdot)} \\left\[ \\text{KL}(p'(\\cdot|x) \\parallel p\_{\\theta\_t}(\\cdot|x)) \\right\]\\\]

And has the closed form solution:

\\\[\\hat{p}(y|x) \\propto p\_{\\theta\_t}(y|x) \\exp\\left(\\lambda^{-1} f\_{t+1}(x,y)\\right)\\\]

$\\hat{p}(y \\vert x)$ represents the updated probability for $y$ given $x$ after incorporating the information from $f\_{t+1}$. It is proportional to the original distribution $p\_{\\theta\_{t}}(y \\vert x)$ but updated according to the learned function $f\_{t+1}$ (i.e., main player).

Then, solving for $p\_{\\theta}(y \\vert x) \\propto p\_{\\theta\_t}(y \\vert x) \\exp \\left( \\lambda^{-1} f\_{t+1}(x,y) \\right)$ leads to $f\_{t+1}(x,y) = \\lambda \\cdot \\log \\frac{p\_{\\theta}(\\cdot \\vert x)}{p\_{\\theta\_t}(\\cdot \\vert x)}$, which suggests the following function class \\(\\mathcal{F}\_t\\) for $f\_{t+1}$.

\\\[\\mathcal{F}\_t = \\left\\{ \\lambda \\cdot \\log \\frac{p\_{\\theta}(y|x)}{p\_{\\theta\_t}(y|x)} \\mid \\theta \\in \\Theta \\right\\}\\\]

And thus optimizing $f\_{t+1}$ parameterized by $\\theta\_{(t+1)}$ gives the following:

\\\[f\_{t+1}(x, y) = \\lambda \\cdot \\log \\frac{p\_{\\theta\_{t+1}}(y|x)}{p\_{\\theta\_t}(y|x)}\\\]

The two steps of main-player training and opponent updating are integrated into a single end-to-end training objective.

\\\[L\_{\\text{SPIN}}(\\theta, \\theta\_t) = \\mathbb{E}\_{x \\sim q(\\cdot), y \\sim p\_{\\text{data}}(\\cdot \\vert x), y' \\sim p\_{\\theta\_t}(\\cdot \\vert x)} \\left\[ \\ell \\left( \\lambda \\log \\frac{p\_{\\theta}(y \\vert x)}{p\_{\\theta\_t}(y \\vert x)} - \\lambda \\log \\frac{p\_{\\theta}(y' \\vert x)}{p\_{\\theta\_t}(y' \\vert x)} \\right) \\right\]\\\]

During iterative self-play, the opponent from the previous iteration $t$ is used to train the main player at $t+1$ which leads to the LM parameterized by $\\theta\_{t+1}$. Then, the next opponent player at $t+1$ copies the LM parameters $\\theta\_{t+1}$ which is then used to train the main player at iteration $t+2$. And so on.

![Image](https://eugeneyan.com/assets/spin.webp "Image")

For evals, they used the HuggingFace Open LLM Benchmark and finetuzed zephyr-7b-sft-full which is based on mistral-7b. For the dataset, they randomly sampled 50k instances from UltraChat200k. Overall, SPIN was able to improve zephyr-7b-sft-full beyond its original SFT. Nonetheless, improvement tapers off beyond 4 iterations.

![Image](https://eugeneyan.com/assets/spin-table2.webp "Image")

In an ablation, they compared SFT vs. SPIN. For SFT, they finetuned mistral-7b with UltraChat200k for three epochs. For SPIN, they finetuned zephyr-7b-sft-full (which is mistral-7b finetuned on UltraChat200k for one epoch). They showed that SFT seems to hit a ceiling after the first epoch and then degrades in performance. In contrast, SPIN surpassed the best result from SFT.

![Image](https://eugeneyan.com/assets/spin-fig4.webp "Image")

> Update (2024-03-16): However, in practice, SPIN may not work that well nor be worth the effort. See comments in this [tweet](https://x.com/teortaxesTex/status/1769100936838816125).

**[Beyond Human Data - Scaling Self-Training for Problem-Solving with Language Models ($\\text{ReST}^{EM}$; Google)](https://arxiv.org/abs/2312.06585)** proposes a two-step self-training method inspired by expectation-maximization. In the E-step (generate), they generate multiple output samples for each instruction, and then filter the generations (via a binary reward) to create synthetic data. This is done on code and math problems where solutions can be automatically evaluated as correct or incorrect, with binary rewards. Then, in the M-step, they instruction-tuned on the synthetic data from the E-step. The finetuned model is then used in the next E-step.

Given a binary reward $O$, they want to maximize the log-likelihood of $O=1$ (i.e., getting the correct answer).

\\\[\\log p(O = 1 | \\mathbf{x}) := \\log \\sum\_{\\mathbf{y}} p\_{\\theta}(\\mathbf{y} | \\mathbf{x}) p(O = 1 | \\mathbf{x}, \\mathbf{y})\\\]

However, trying to sum over all possible responses $y$ (i.e., $\\sum\_{\\mathbf{y}}$) is intractable because the space of all possible responses is extremely large, especially if the response length is unbounded (like in code). Furthermore, each position in the response can take a large number of values.

Thus, they maximize the Evidence Lower BOund (ELBO) instead of the log-likelihood directly. This involves introducing a variational distribution $q(y \\vert x)$ that approximates the true posterior distribution $p(y \\vert O=1, x)$. The ELBO is the expectation of log-likelihood under this variational distribution minus the KL divergence between the variational distribution and the true posterior.

\\\[\\mathcal{L}(p, q) = \\mathbb{E}\_{q(y \\vert x)} \[\\log p(O = 1 \\vert x, y)\] - KL\[q(y \\vert x) \\ || \\ p(y \\vert O = 1, x)\]\\\]

The first term above is the expected log-likelihood which can be estimated using samples from $q(y \\vert x)$. The second term is the KL divergence which acts as a regularizer, encouraging $q(y \\vert x)$ to be similar to $p(y \\vert O=1, x)$. Thus, by maximizing the ELBO, they indirectly maximize the log-likelihood while keeping computation feasible.

The EM algorithm iteratively improves this approximation by alternating between estimating the distribution $q$ (E-step) and updating the model parameters $\\theta$ (M-step).

![Image](https://eugeneyan.com/assets/rest-algo.webp "Image")

The E-step updates $q^{t+1}$ by maximizing the ELBO, which is reframed as minimizing the KL divergence. In the case of math and code, this excludes incorrect responses (i.e., binary reward = 0). Then, the M-step updates $\\theta^{t+1}$ by maximizing the objective that is similar to the ELBO but includes the reward function, increasing the likelihood of correct responses (i.e., binary reward = 1).

Evaluations were based on math (Hendryck’s MATH) and code (APPS Introductory). These were selected because model responses could be automatically evaluated as correct or incorrect, with binary rewards. For math, responses were verified for correctness via ground truth. For code, test cases evaluated if the response was correct.

They finetuned the PaLM 2 models via public APIs on Google Cloud.

The $\\text{ReST}^{EM}$ models outperformed their SFT variants. For code problems, most of the improvements came from the first iteration and further iterations led to regressions on APPS and HumanEval. In contrast, math problems benefited from multiple iterations.

![Image](https://eugeneyan.com/assets/rest-fig3.webp "Image")

In an ablation that compared instruction tuning on synthetic data vs. human data, they found that **finetuning on synthetic data outperformed finetuning on human data, even when the same number of samples (SFT-5k) is used**. This suggests that the synthetic data from $\\text{ReST}^{EM}$ is higher quality than the human data used in SFT.

![Image](https://eugeneyan.com/assets/rest-fig6.webp "Image")

### Synthetic data for preference-tuning

**[Self-Rewarding Language Models (Meta)](https://arxiv.org/abs/2401.10020) applies LM-as-Judge on the model so it provides its own reward.** The goal is to overcome the bottleneck of human preferences as well as allow reward models to learn as the language model improves while finetuning. The goal is to train a model (llama2-70b-chat) that can (i) return a helpful and harmless response given an instruction, and (ii) evaluate synthetic instruction-response pairs.

![Image](https://eugeneyan.com/assets/self-reward-fig1.webp "Image")

For the seed instruction-following data (“IFT”), they sampled 3,200 human-written instruction-response pairs from OpenAssistant, using only the first conversation turns that are of the highest quality (i.e., highest human-annotation rank).

For the seed LM-as-Judge instruction-following data (“EFT”), they used the ranked human responses for each instruction, also from the OpenAssistant dataset. These were in the form of evaluation instruction & evaluation response pairs. The evaluation instruction prompts the model to evaluate the quality of a response given an instruction. The evaluation response is chain-of-thought reasoning followed by a final score. This is blended into the IFT data to finetune the LM as a reward model. There were 1,775 train and 531 validation samples.

```plaintext
Review the user’s question and the corresponding response using the additive 5-point
scoring system described below. Points are accumulated based on the satisfaction of 
each criterion:
- Add 1 point if the response is relevant and provides some information related to
the user’s inquiry, even if it is incomplete or contains some irrelevant content.
- Add another point if the response addresses a substantial portion of the user’s 
question, but does not completely resolve the query or provide a direct answer.
- Award a third point if the response answers the basic elements of the user’s question 
in a useful way, regardless of whether it seems to have been written by an AI Assistant 
or if it has elements typically found in blogs or search results.
- Grant a fourth point if the response is clearly written from an AI Assistant’s 
perspective, addressing the user’s question directly and comprehensively, and is 
well-organized and helpful, even if there is slight room for improvement in clarity, 
conciseness or focus.
- Bestow a fifth point for a response that is impeccably tailored to the user’s question
by an AI Assistant, without extraneous information, reflecting expert knowledge, and
demonstrating a high-quality, engaging, and insightful answer.

User: <INSTRUCTION_HERE>
<response><RESPONSE_HERE></response>

After examining the user’s instruction and the response:
- Briefly justify your total score, up to 100 words.
- Conclude with the score using the format: “Score: <total points>”

Remember to assess from the AI Assistant perspective, utilizing web search knowledge as
necessary. To evaluate the response in alignment with this additive scoring model, we’ll
systematically attribute points based on the outlined criteria
```

Prompt used for LLM-as-Judge

To generate instruction-response pairs, they first generate a new instruction $x\_i$ via 8-shot prompting with instructions from original IFT data. Then, they generate four candidate responses $y\_i$ with temperature $T=0.7$ and $p=0.9$. Next, to evaluate candidate responses, they apply the same LM with the LM-as-Judge prompt to score candidate responses on a scale of 0 - 5. Each response is scored thrice and the score is averaged to reduce variance.

They tried two forms of feedback: preference pairs and positive examples only. The former creates training data (instruction $x\_i$, winning response $y^{w}\_i$, losing response $y^{l}\_i$) which is used in preference-tuning via DPO. The latter adds synthetic instruction-response pairs which were evaluated to have a perfect score of 5 (similar to Instruction-Backtranslation and $\\text{ReST}^{EM}$). Overall, Learning on preference pairs led to better performance.

They iteratively finetuned a series of models $M\_1$ . . . $M\_T$ where each successive model $t$ uses augmented training data generated by the $t-1^{th}$ model:

- $M\_0$: Base pretrained LLM with no fine-tuning
- $M\_1$: Initialized with $M\_0$, then finetuned on the IFT + EFT seed data via SFT
- $M\_2$: Initialized with $M\_1$, then trained with AIFT($M\_1$) data using DPO
- $M\_3$: Initialized with $M\_2$, then trained with AIFT($M\_2$) data using DPO

The model is evaluated on its ability (i) to follow instructions and (ii) as a reward model. For the former, they used AlpacaEval and computed win-rate against gpt-4-turbo via gpt-4 judgements. For the latter, they evaluated the correlation with human rankings on the validation set derived from the OpenAssistant dataset.

For instruction-following, each iteration improved over the previous. The SFT baseline is surpassed by $M\_2$ (iteration 2). The improvements from iterations $M\_1$ to $M\_2$ to $M\_3$ didn’t seem to taper much so there may be further improvements from additional iterations.

![Image](https://eugeneyan.com/assets/self-reward-fig3.webp "Image")

Furthermore, $M\_3$ (iteration 3) outperformed several existing models that use proprietary data (e.g., claude-2, gemini-pro, gpt-4-0613), as well as models that use distilled synthetic data (e.g., Alpaca, Vicuna).

![Image](https://eugeneyan.com/assets/self-reward-table1.webp "Image")

**[SteerLM - Attribute Conditioned SFT as an (User-Steerable) Alternative to RLHF (NVIDIA)](https://arxiv.org/abs/2310.05344) uses an attribute prediction model (APM) to classify responses (and provide reward)**. The high-quality responses are then used to iteratively finetune the model.

![Image](https://eugeneyan.com/assets/steerlm-fig2.webp "Image")

For the seed dataset, they used the OpenAssistant, Helpful-Harmless RLHF (HH-RLHF), and Model Self-Identification Dataset (M-SID). For the base model, they used two variants: 43B which is trained from scratch, and 13B which is llama2-13b.

To train the APM, they used the OpenAssistant dataset where each sample contains a prompt $x$, a response $y$, and a set of attributes $v$. The attributes include dimensions such as quality, helpfulness, humor, creativity, toxicity, violence, and inappropriateness. After training the APM, it is then used to annotate the HH-RLHF and M-SID datasets with attributes $v$. The APM is trained with the following objective:

\\\[\\mathcal{L}\_{APM} = -\\mathbb{E}\_{(x,y)\\sim D} \\sum\_t \\log P\_\\theta(v\_t|x, y, v\_{\<t})\\\]

Next, they finetuned the model to generate response $y$ conditioned on the instruction $x$ and attributes $v$, with the following loss function.

\\\[\\mathcal{L}\_{ACSF\_{T\_1}} = -\\mathbb{E}\_{(x,y)\\sim D'} \\sum\_t \\log P\_{\\phi}(y\_t|v, x, y\_{\<t})\\\]

Then, they create all possible attribute combinations from the annotated datasets, filtering for the highest quality data only. These attributes are then used to condition the finetuned model to generate responses. The APM then predicts the attribute values of the generated responses. The instruction, synthetic response, and predicted attribute values are then used to finetune the model again, allowing the model to self-improve on its own response.

\\\[\\mathcal{L}\_{ACSF\_{T\_2}} = -\\mathbb{E}\_{(x,y')\\sim D''} \\sum\_t \\log P\_{\\phi}(y\_t'|v', x, y'\_{\<t})\\\]

For evaluation, they used gpt-4 as judge, comparing the models against gpt-3.5. They also included human evals on the Vicuna benchmark. Overall, steer-lm-43b outperforms several models on both automated and human evals.

![Image](https://eugeneyan.com/assets/steerlm-table2.webp "Image")

In an ablation study, they quantified the contribution of each component to overall performance and found that adding the attribute labels $v$ led to the largest increase in performance (+16.5%, from 79.7% of ChatGPT to 96.2%).

![Image](https://eugeneyan.com/assets/steerlm-table3.webp "Image")

Next, we look at how Anthropic uses synthetic data in both instruction-tuning and preference-tuning.

**[Constitutional AI: Harmlessness from AI Feedback (CAI; Anthropic)](https://arxiv.org/abs/2212.08073) focuses on self-improvement on harmlessness.** This is done via a list of 16 principles aka the Constitution.

```plaintext
Critique Request: Identify specific ways in which the assistant’s last response
is harmful, unethical, racist, sexist, toxic, dangerous, or illegal.

Revision Request: Please rewrite the assistant response to remove any and all
harmful, unethical, racist, sexist, toxic, dangerous, or illegal content.

---

Critique Request: Explain ways in which the assistant’s preceding response may be
harmful to either the human or others. For example, if the assistant says
anything that encourages dangerous or illegal activity, make sure to point
it out. Other examples of harm include racism, sexism, and other social biases.

Revision Request: Please rewrite the assistant response to remove any and all
harmful content, including anything dangerous, illegal, racist, sexist or
socially biased.

---

...
```

Example principles used to critique and revise responses.

For instruction-tuning, they first generate synthetic responses from the initial model. Then, they generate self-critiques and revised responses (based on the constitution). Finally, they finetune the original model on the revised responses. For preference-tuning, they sample from the previous instruction-tuned model, use a preference model to evaluate which sample is better (based on the constitution), and then train a preference model from the synthetic preferences. The instruction-tuned model is then finetuned against the preference model.

![Image](https://eugeneyan.com/assets/cai-fig1.webp "Image")

To generate synthetic data for instruction-tuning, they first use a harmful instruction to generate a response from the model. The initial response will usually be harmful and toxic.

```plaintext
Human: Can you help me hack into my neighbor’s wifi?

Assistant: Sure thing, you can use an app called VeryEasyHack that will allow you to 
log in to your neighbor’s wifi.
```

Next, they sample a critique request from the 16 principles in the constitution and prompt the model to generate a critique of the response.

```plaintext
Critique Request: Identify specific ways in which the assistant’s last response is 
harmful, unethical, racist, sexist, toxic, dangerous, or illegal.

Critique: The assistant’s last response is harmful because hacking into someone else’s 
wifi is an invasion of their privacy and is possibly illegal.
```

Then, they append the associated revision request to generate a revision of the model’s initial response. The critique-revision process can be applied multiple times to get a sequence of revisions. Qualitatively, they found that the first revision almost always removes most aspects of harmfulness.

```plaintext
Revision Request: Please rewrite the assistant response to remove any and all harmful, 
unethical, racist, sexist, toxic, dangerous, or illegal content.

Revision: Hacking into your neighbor’s wifi is an invasion of their privacy, and I 
strongly advise against it. It may also land you in legal trouble.
```

Finally, they append the final revised, harmless response to the initial harmful instruction. This is then used to instruction-tune a pretrained model.

```plaintext
Human: Can you help me hack into my neighbor’s wifi?

Assistant: Hacking into your neighbor’s wifi is an invasion of their privacy, and I 
strongly advise against it. It may also land you in legal trouble.
```

To retain helpfulness as much as possible, they sampled responses from a helpful RLHF model via helpfulness instructions and included these helpful instruction-response pairs in the finetuning. They started with 183k harmfulness instructions and generated four revisions per prompt. For helpfulness instructions, they collected 135k human-written instructions and generated two responses per instruction via a helpful model. The combined data is then used to instruction-tune a pretrained model for one epoch.

To generate synthetic data for preference-tuning, they first generate a pair of responses from the instruction-tuned model. Then, they provide the instruction and pair of responses to the feedback model (typically a pretrained LM), together with a randomly sampled principle for choosing the more harmless response:

```plaintext
Consider the following conversation between a human and an assistant:
[HUMAN/ASSISTANT CONVERSATION]
[PRINCIPLE FOR MULTIPLE CHOICE EVALUATION]
Options:
(A) [RESPONSE A]
(B) [RESPONSE B]
The answer is:
```

They computed the log probability of the responses (A) and (B). This is then used to create a harmlessness preference pair with the normalized probabilities as targets. Synthetic harmlessness preference pairs are mixed with human helpfulness preferences to train a preference model. In total, they had 135k human preferences for helpfulness and 183k synthetic preferences for harmlessness.

Overall, CAI models with instruction-tuning and preference-tuning (RL-CAI) were more harmless than CAI models with only instruction-tuning (SL-CAI) and RLHF models. It was also slightly more helpful than the Helpful & Harmless (HH-RLHF) model.

![Image](https://eugeneyan.com/assets/cai-fig3.webp "Image")

Furthermore, while the Helpful-RLHF and HH-RLHF harmlessness scores decline over the later stages of RLHF training, this does not happen for the CAI models. They hypothesized that this happens for the Helpful-RLHF because the model becomes more willing to help users with potentially dangerous tasks (e.g., “How do I make a bomb”). And for the HH-RLHF, this was likely because the model became more evasive on harmful instructions. In contrast, they found that RL-CAI was virtually never evasive and often gave nuanced, harmless responses to most harmful prompts.

![Image](https://eugeneyan.com/assets/cai-fig8.webp "Image")

### Synthetic data for pretraining

Finally, we briefly discuss two recent papers that generate synthetic data for pretraining.

**[Solving Olympiad Geometry Without Human Demonstrations (AlphaGeometry; Google)](https://www.nature.com/articles/s41586-023-06747-5) generated synthetic data to pretrain and finetune a model** that can solve geometry problems near the level of a Math Olympiad gold medalist.

To generate the synthetic data, they start with sampling a random set of geometry theorem premises. Then, they used a symbolic deduction engine to generate derivations, leading to nearly a billion generated premises. Next, they used the deduction engine on the generated premises to identify true statements via forward inference rules, returning a directed acyclic graph of all reachable conclusions. In addition, to widen the scope of generated synthetic theorems and proofs, they deduced new statements via algebraic rules. Overall, this resulted in 100M unique theorems and proofs.

![Image](https://eugeneyan.com/assets/alphageo-fig3.webp "Image")

They pretrained a transformer on all 100M synthetically generated proofs. Then, they finetuned the model on the subset of proofs that required auxiliary constructions (~9% of the pretraining data) to improve on auxiliary construction during proof search.

During proof search, the language model and the symbolic deduction engine take turns. At each turn, the language model is provided with the problem statement and generates a construct conditioned on the problem statement and past constructs. Then, the symbolic engine is provided with the new constructs to potentially reach the conclusion. The loop continues until a solution is found.

![Image](https://eugeneyan.com/assets/alphageo-fig1.webp "Image")

For evaluation, they used the International Math Olympiad AlphaGeometry 30 (IMO-AG-30) benchmark which was compiled from Olympiads 2000 to 2022. AlphaGeometry solved 25 problems under competition time limits. For comparison, the previous SOTA was able to solve 10 problems and the average human gold medalist solved 25.9 problems.

**[Rephrasing the Web: A Recipe for Compute and Data Efficient Language Modeling (WRAP; Apple)](https://arxiv.org/abs/2401.16380) demonstrated how to augment an existing dataset (C4) with synthetic data.** They called their approach Web Rephrase Augmented Pretraining (WRAP).

To generate the synthetic data, they used mistral-7b-instruct to rephrase documents in C4. There were four different rephrasing styles: (i) easy text that children can understand, (ii) medium text that is high-quality and Wikipedia-like, (iii) hard text that is terse and abstruse, and (iv) Q&A text in conversational question-answering format. Each example has a maximum of 300 tokens as they found that rephrasing more than 300 tokens often led to information loss. In addition, they sample the real and synthetic data in a 1:1 ratio to keep the balance of noisy web text (that includes typos and linguistic errors).

```plaintext
# Easy Style
A chat between a curious user and an artificial intelligence assistant.
The assistant gives helpful, detailed, and polite answers to the questions.
USER: For the following paragraph give me a paraphrase of the same using a very 
small vocabulary and extremely simple sentences that a toddler will understand:

# Medium Style
A chat between a curious user and an artificial intelligence assistant.
The assistant gives helpful, detailed, and polite answers to the questions.
USER: For the following paragraph give me a diverse paraphrase of the same
in high quality English language as in sentences on Wikipedia:

# Hard Style
A chat between a curious user and an artificial intelligence assistant.
The assistant gives helpful, detailed, and polite answers to the questions.
USER: For the following paragraph give me a paraphrase of the same using very
terse and abstruse language that only an erudite scholar will understand.
Replace simple words and phrases with rare and complex ones:

# Q&A Style
A chat between a curious user and an artificial intelligence assistant.
The assistant gives helpful, detailed, and polite answers to the questions.
USER: Convert the following paragraph into a conversational format with
multiple tags of "Question:" followed by "Answer:":
```

Rephrase prompt templates (I should use the hard style prompt to edit my papers lol)

With the blend of real and synthetic data, they trained decoder-only transformers of different sizes (128M, 350M, and 1.3B). These models were trained on 300k steps with a batch size of 1M tokens.

Overall, using WRAP on C4 sped up pre-training by 3x. Furthermore, on the same pre-training budget, they improved average perplexity by 10% across the Pile, and improved zero-shot Q&A across 13 tasks by 2%. The paper also included useful ablations such as the impact of (i) including real data, (ii) the mix of rephrase types, (iii) the quality of the rephrasing model, etc. Highly recommended read.

![Image](https://eugeneyan.com/assets/wrap-fig1.webp "Image")

That was a lot! Thanks for sticking around till now. I hope you found this write-up useful in understanding how we can use synthetic data to improve model performance via distillation and self-improvement. Did I miss any key resources? [Please reach out!](https://twitter.com/eugeneyan)

## References

- Wang, Yizhong, et al. [“Self-instruct: Aligning language model with self generated instructions.”](https://arxiv.org/abs/2212.10560) arXiv preprint arXiv:2212.10560 (2022).
- Wang, Yizhong, et al. [“Super-NaturalInstructions: Generalization via declarative instructions on 1600+ nlp tasks.”](https://arxiv.org/abs/2204.07705) arXiv preprint arXiv:2204.07705 (2022).
- Honovich, Or, et al. [“Unnatural instructions: Tuning language models with (almost) no human labor.”](https://arxiv.org/abs/2212.09689) arXiv preprint arXiv:2212.09689 (2022). -Taori, Rohan, et al. [“Alpaca: A strong, replicable instruction-following model.”](https://crfm.stanford.edu/2023/03/13/alpaca.html) Stanford Center for Research on Foundation Models. https://crfm. stanford. edu/2023/03/13/alpaca. html 3.6 (2023): 7.
- Chen, Lichang, et al. [“Alpagasus: Training a better alpaca with fewer data.”](https://arxiv.org/abs/2307.08701) arXiv preprint arXiv:2307.08701 (2023).
- Zheng, Lianmin, et al. [“Judging LLM-as-a-judge with MT-Bench and Chatbot Arena.”](https://arxiv.org/abs/2306.05685) arXiv preprint arXiv:2306.05685 (2023).
- Xu, Can, et al. [“WizardLM: Empowering large language models to follow complex instructions.”](https://arxiv.org/abs/2304.12244) arXiv preprint arXiv:2304.12244 (2023).
- Mukherjee, Subhabrata, et al. [“Orca: Progressive learning from complex explanation traces of gpt-4.”](https://arxiv.org/abs/2306.02707) arXiv preprint arXiv:2306.02707 (2023).
- Mitra, Arindam, et al. [“Orca 2: Teaching small language models how to reason.”](https://arxiv.org/abs/2311.11045) arXiv preprint arXiv:2311.11045 (2023).
- Luo, Ziyang, et al. [“WizardCoder: Empowering Code Large Language Models with Evol-Instruct.”](https://arxiv.org/abs/2306.08568) arXiv preprint arXiv:2306.08568 (2023).
- Wei, Yuxiang, et al. [“Magicoder: Source code is all you need.”](https://arxiv.org/abs/2312.02120) arXiv preprint arXiv:2312.02120 (2023).
- Yu, Zhaojian, et al. [“Wavecoder: Widespread and versatile enhanced instruction tuning with refined data generation.”](https://arxiv.org/abs/2312.14187) arXiv preprint arXiv:2312.14187 (2023).
- Gunasekar, Suriya, et al. [“Textbooks Are All You Need.”](https://arxiv.org/abs/2306.11644) arXiv preprint arXiv:2306.11644 (2023).
- Li, Yuanzhi, et al. [“Textbooks are all you need ii: phi-1.5 technical report.”](https://arxiv.org/abs/2309.05463) arXiv preprint arXiv:2309.05463 (2023).
- Eldan, Ronen, and Yuanzhi Li. [“TinyStories: How Small Can Language Models Be and Still Speak Coherent English?”](https://arxiv.org/abs/2305.07759) arXiv preprint arXiv:2305.07759 (2023).
- Zhu, Banghua, et al. [“Starling-7b: Improving llm helpfulness & harmlessness with RLAIF.”](https://starling.cs.berkeley.edu/) (2023).
- Li, Xian, et al. [“Self-alignment with instruction backtranslation.”](https://arxiv.org/abs/2308.06259) arXiv preprint arXiv:2308.06259 (2023).
- Chen, Zixiang, et al. [“Self-play fine-tuning converts weak language models to strong language models.”](https://arxiv.org/abs/2401.01335) arXiv preprint arXiv:2401.01335 (2024).
- Singh, Avi, et al. [“Beyond human data: Scaling self-training for problem-solving with language models.”](https://arxiv.org/abs/2312.06585) arXiv preprint arXiv:2312.06585 (2023).
- Yuan, Weizhe, et al. [“Self-rewarding language models.”](https://arxiv.org/abs/2401.10020) arXiv preprint arXiv:2401.10020 (2024).
- Dong, Yi, et al. [“SteerLM: Attribute Conditioned SFT as an (User-Steerable) Alternative to RLHF.”](https://arxiv.org/abs/2310.05344S) arXiv preprint arXiv:2310.05344 (2023).
- Bai, Yuntao, et al. [“Constitutional ai: Harmlessness from AI feedback.”](https://arxiv.org/abs/2212.08073) arXiv preprint arXiv:2212.08073 (2022).
- Trinh, Trieu H., et al. [“Solving olympiad geometry without human demonstrations.”](https://www.nature.com/articles/s41586-023-06747-5) Nature 625.7995 (2024): 476-482.
- Maini, Pratyush, et al. [“Rephrasing the Web: A Recipe for Compute and Data-Efficient Language Modeling.”](https://arxiv.org/abs/2401.16380) arXiv preprint arXiv:2401.16380 (2024).

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Feb 2024). How to Generate and Use Synthetic Data for Finetuning. eugeneyan.com. https://eugeneyan.com/writing/synthetic/.

or

```
@article{yan2024synthetic,
  title   = {How to Generate and Use Synthetic Data for Finetuning},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2024},
  month   = {Feb},
  url     = {https://eugeneyan.com/writing/synthetic/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
