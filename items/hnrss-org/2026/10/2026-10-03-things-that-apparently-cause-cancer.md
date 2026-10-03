---
title: Things that apparently cause cancer
link: https://www.breakthroughjournal.org/p/things-that-apparently-cause-cancer
source: hnrss-org
published: 2026-10-03T00:23:11Z
updated: 2026-10-03T00:23:11Z
first_seen: 2026-10-03T05:41:51.909771426Z
authors:
- timpera
content: extracted
html: 2026-10-03-things-that-apparently-cause-cancer.html
preview:
  file: 2026-10-03-things-that-apparently-cause-cancer.preview-d97befcc4126.webp
  width: 256
  height: 144
  color: '#90755d'
images:
- source: https://substackcdn.com/image/fetch/$s_!dwtP!,w_1200,h_675,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F486cc33a-3601-45d2-b915-5e1f2ab16c3f_1400x771.jpeg
  original:
    file: 2026-10-03-things-that-apparently-cause-cancer.image-c5f9e5d1e4d6.jpg
    width: 1200
    height: 675
  color: '#a74627'
- source: https://substackcdn.com/image/fetch/$s_!dwtP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F486cc33a-3601-45d2-b915-5e1f2ab16c3f_1400x771.jpeg
  original:
    file: 2026-10-03-things-that-apparently-cause-cancer.image-45a055d6a725.jpg
    width: 1400
    height: 771
  color: '#b7c6d8'
---

[![](https://substackcdn.com/image/fetch/$s_!dwtP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F486cc33a-3601-45d2-b915-5e1f2ab16c3f_1400x771.jpeg)](https://substackcdn.com/image/fetch/$s_!dwtP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F486cc33a-3601-45d2-b915-5e1f2ab16c3f_1400x771.jpeg)

It would shock you to know that Costco, according to a new Harvard School of Public Health methodology, caused 120,687 cases of cancer mortality nationally every year, simply due to living in close proximity to their warehouses. Private colleges accounted for 83,782 more. Harvard itself is responsible for 5,842 cancer deaths. Living near a Major League Baseball field causes almost 19 times as many cancer deaths as living near a Major League Soccer pitch. These conclusions are obviously absurd, but they come from the exact same methodology that researchers at Harvard use to “prove” that nuclear power plants cause cancer in nearby communities.

Earlier this year, Adam Stein and I called out a series of Harvard studies showing that nuclear power plants were associated with higher rates of cancer incidence and cancer mortality as nonsense. Now that we’ve spent the last few months replicating the analysis and expanding it, we are able to show that no matter what landmark you use, their methodology will show an increased cancer risk and mortality. The studies simply do not prove anything about cancer incidence.

Let’s recap: In December 2025, researchers led by Yazan Alwadi at Harvard’s T.H. Chan School of Public Health published a [paper](https://link.springer.com/article/10.1186/s12940-025-01248-6) in *Environmental Health* that claimed to find that cancer incidence increased for people living closer to nuclear power plants in Massachusetts. In March, the same researchers published an expanded nationwide [study](https://www.nature.com/articles/s41467-026-69285-4) claiming a similar result—this time looking at cancer mortality rates, rather than incidence—in *Nature Communications*. This was followed by a [paper](https://www.nature.com/articles/s41370-026-00922-2) in the *Journal of Exposure Science & Environmental Epidemiology* that looked at associations of lung, breast, and colon cancers. Most recently, a [study](https://link.springer.com/article/10.1007/s10654-026-01442-x) of total mortality, not just cancer, was published in the European journal *Environmental Epidemiology*.

The papers construct a “proximity score” based on distance from nuclear plants, up to 120 km in Massachusetts and 200 km in the national studies (about 75 and 125 miles). Every ZIP code or county inside those radii is treated as exposed, with closer locations receiving higher weights. If a ZIP code or county is within range of multiple sites, then the effect is cumulative. That means that a county that is in close proximity to one nuclear power plant could have a smaller score than another county that is further away from any one power plant but within range of many.

From this proximity score, the authors run regressions testing the outcome (cancer incidence, cancer mortality, or total mortality) on the proximity of the county and a collection of covariates. From this, they use the statistical coefficients, construct the Relative Risk for each county, age, and sex group, and compute the Attributable Fraction, from which they derive the number of attributed deaths from the nuclear power plant from 2000 to 2018. The authors state that their methodology and results provide the scientific basis and public health justification for an expanded research program.

But proximity is not exposure. We have methods of measuring exposure for nuclear power plant workers. While the radiation exposure of nuclear workers will always be greater than or equal to that received by the surrounding public, most of the closely monitored US nuclear workforce [receive no measurable annual dose](https://inl.gov/content/uploads/2023/07/INLRPT-25-85463_Reevaluation-of-Radiation-Protection-Standards-R0-Final.pdf). When workers are exposed to radiation, the average dose received is only 2 percent of the occupational limit. If operators and workers who are on-site at nuclear power plants receive an annual dose between zero and one-fiftieth of the occupational limit, how is it possible that residents 5, 10, 25, 50, 120, or 200 kilometers away would receive any measurable dose from the same plant?

In talks, the authors hedge that their papers are merely ecological studies that show association, but never prove causality. Ecological studies are used to understand the relationship between outcome and exposure at a population level. This leads us to ask: what would the mechanism of exposure be? Well, according to the authors, we can just ignore the broader literature and physics, and instead make up exposure pathways and mechanisms. The use of an “ecological study” allows a lot of leeway in terms of explaining the broader world.

Over the last several months, we have replicated the results of these papers. The authors supplied us with eight lines of code and answered a couple of questions about the covariates, which did not replicate the results. Most of our replication was done through first principles combined with trial and error. Once we were reasonably close to the results of the first national study on cancer mortality, we took the methodology and applied it to numerous other landmarks.

[Share](https://www.breakthroughjournal.org/p/things-that-apparently-cause-cancer?utm_source=substack&utm_medium=email&utm_content=share&action=share)

Everything causes cancer. Sounds cliché; maybe those California warning tags were right all along. But thanks to the methodology created by Alwadi et al., we can now prove that anything and everything causes cancer. Oh, sorry, that is too strong of an assertion. To use the authors’ words, since they have claimed their papers don’t prove causality, we can create an association between any physical landmark and cancer and, from that association, figure out how much cancer is attributable to that thing. Which is totally not the same as saying “that thing causes cancer.”

For instance, living near a private four-year university is associated with a 15-fold increase in cancer mortality when compared to living near a nuclear power plant.

Costco has the largest effect of all the locations we have tested. Over 2.2 million cancer deaths can be attributed to Costco; that’s more than 20% of all cancer deaths between 2000 and 2018. Hot dogs, bulk spices, and reasonably priced clothes come with a cost. But it goes to show that the authors’ choice of nuclear power plants wasn’t serious. Private 4-year colleges, including Harvard, are associated with the deaths of 1.59 million people, public 4-year colleges 626,212, and Superfund sites 895,258. Once you include the attributable deaths from state capitals at 132,389, we can account for 5.54 million cancer deaths, over one-half of all cancer deaths, between 2000 and 2018.

If we look at professional sports venues, the spread looks even more bizarre. Living near an NFL stadium was associated with 418,520 deaths, NBA arenas 471,794, MLB fields 944,769, NHL rinks 126,751, and MLS pitches 50,889. None of these sports have any kind of emissions that leave the playing field, except for the occasional home run or foul ball. The difference across the sports might be the kinds of concessions being sold. Baseball is right up there with Costco, selling copious amounts of hot dogs. Perhaps most surprising of all in the realm of professional sports is NASCAR, which accounted for 79,278 cancer deaths. One would expect the emissions, noise, smoking, and copious amounts of light beer to be more impactful than the emission-free ball and puck sports, but moving closer to a speedway might just save your life. With sports added to the mix, over 7.85 million cancer deaths can be attributed across a variety of landmarks; that’s 3/4s of all cancer deaths between 2000 and 2018.

The supposed 115,586 attributable deaths from nuclear power plants look like chump change to the real perpetrators. Harvard, when singled out, accounts for 111,003 deaths. That’s just one institution threatening every community within 200 km. Once again, we are left asking what the mechanism could be. Plus, our study only goes from 2000 to 2018; the school has been around since 1636, so the actual death toll could be much higher.

According to a [webinar presentation](https://youtu.be/Cy_Dh-vOjiM?si=KncY9emCjLRBQ8gq) given by Dr. Koutrakis, one of the coauthors, the supposed release of radioactive effluence from nuclear power plants comes from the occasional refueling of the reactors, when operators open the reactor core, and radioactive dust escapes from the fuel assemblies, past the containment vessel, through the outer shielding, and out into the world without ever being detected by the myriad number of dosimeters and radiological sensors throughout the facility. If we really are so terrible at measuring radionuclides (we are actually very good at it), then it is entirely plausible that Harvard could have unlawful access to special nuclear materials and the brains of Cambridge could have built a nuclear reactor under the squash courts without the proper licensure.

Cancer is a scourge across the human race. You have a ~40% chance of getting cancer sometime in your life. Which is why we should take cancer studies very seriously. If one is showing a surprising result, we want to get to the bottom of it. All kinds of claims can be made with statistics, but if we want to make good policies based on evidence, it is worth spending time making sure the evidence upholds its claims.

In our original rebuttal, we remarked that the studies can’t prove their assertions because they lacked proper control. It seems they also neglected to do any placebo testing. They chose nuclear power plants because a story could be built around that framework. When the researchers got positive results across our nation’s nuclear power plants, they didn’t check what their shiny new methodology would do using other landmarks. This is their pitfall: by taking the easy way out—getting results and making up a story around those results without double-checking their method—the authors could have no idea that what they were actually capturing was the methodology itself. In our replication and expansion, we have shown that choosing a number of sites and landmarks from capitals to warehouse stores can all yield a positive result without any reasonable pathway for American citizens to become exposed to radiation, develop cancer, and then die. The variations are random noise, all sloping in a positive direction. Even when using randomized outcomes, the methodology outputs positive results.

The papers by Alwadi et al. don’t show a novel mechanism by which nuclear power plants meaningfully contribute to cancer mortality; instead, they show an association of data points across 400 km-wide circles. 48,519 square miles, about the size of Mississippi, is a massive area to claim any kind of exposure. Most studies measuring distance-based exposures look at much smaller distances, such as under 10 km (an area of 113 square miles). The methodology is blind, which can be a feature in research areas needing to avoid bias, but in this case it is a bug; it doesn’t understand radiation exposure or dose. All it understands are its inputs: coordinates, proximity, covariates, and cancer deaths. From these, it can give you a number and even a positive result, but it cannot explain why that result exists. It is still an ecological study, one with a sophisticated statistical technique, but not a very useful one.

Sophisticated statistics can produce extremely precise nonsense when the design fails to identify the causal mechanism. Bad science like this is especially dangerous when applied to something culturally plausible that gets a positive result—it is easy to twist a result into a compelling narrative, especially if it confirms one’s biases. Nuclear power plants create energy through radiation, and radiation causes cancer; therefore, nuclear power plants cause cancer.

It is especially telling that no matter what landmark we applied to the methodology, we have yet to get a negative result. There is, in fact, a real chance that you simply cannot get a negative result from this method.

The attributable number of deaths from this methodology is probably zero, but the attributable number of bad papers is at least four.

[Share The Ecomodernist](https://www.breakthroughjournal.org/?utm_source=substack&utm_medium=email&utm_content=share&action=share)

No posts
