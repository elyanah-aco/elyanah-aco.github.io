---
layout: post
title: "Analyzing respresentations of disability in media through tropes"
date: 2017-01-18
category: notebook
comments: true
author: "ELYANAH ACO"
description: ""
---

I consider [TvTropes.org](https://tvtropes.org/) one of the best time-wasters in the Internet if (a) you prefer something low-bandwidth, which is perfect when you're stuck at traffic, or (b) you still want to feel like you're learning something. I've spent many hours looking up the latest game I've played or the current series I'm watching and just reading what tropes were used in its characters, story and themes.

A **trope** as defined in the website is a "storytelling device or convention, a shortcut for describing situations the storyteller can reasonably assume the audience will recognize". For example, if a story contains a group of protagonists, they might be a [Power Trio](https://tvtropes.org/pmwiki/pmwiki.php/Main/PowerTrio) where two people with completely different personalities are balanced by the third member, or a [Five-Man Band](https://tvtropes.org/pmwiki/pmwiki.php/Main/FiveManBand) with each member serving specific roles, such as being the leader, the smart guy, or the muscle. 

TvTropes clarifies that tropes are not the same as cliches or stereotypes, and are are simply tools for storytelling. However, by the earlier definition of a trope, a stereotypical depiction becomes a trope when used in enough works of media. And since tropes are narrative shortcuts understood by audiences, creators may feel inclined to use them to get their point across more quickly and efficiently. 

(TROPES AND STEREOTYPES)

This is the motivation behind [Gala et al (2020)](https://aclanthology.org/2020.nlpcss-1.23.pdf) which investigated gender bias by looking into gendered tropes in film, television and literature. Using TvTropes descriptions, they found out that genre impacts genderedness and that male-leaning tropes exhibit more topical diversity, among other findings.

Inspired by this paper, I plan to do a similar analysis on tropes about disabilities, both physical and mental. Even back in the 1990s, critical disabiility theorists have criticized stereotypical media portrayals of people with disabilities (PWDs). Three such steoreotypes according to [Nelson (2000)](https://www.tandfonline.com/doi/abs/10.1207/S15327728JMME1503-4?journalCode=hmme20) portray the disabled as such:

* As a **victim**: Characters are portrayed as pitiful, helpless, and *in need of being saved* from their disabilities. Disability is shown as something that *limits a person's chance to live normal or happy lives*. As such, some narratives frame overcoming disability as the resolution these characters need and/or achieve throughout the story. This stereotype also involves comedic portrayals of PWDs if the disability itself is the source of comedy.

* As a **supercrip**: Characters perform extraordinary willpower or achievements not in spite of, but *because* of their disabilities. [Grue (2015)](https://www.taylorfrancis.com/chapters/edit/10.4324/9781315796574-17/problem-supercrip-grue-jan) criticizes this portrayal as problematic when it not only attributes these feats to the disability, but when it also shows that these feats *would not have been possible if it were not for the disability*. Disability is thus framed and legitimized as positive and even necessary.

* As a **threat**: Characters serve as *antagonistic or villainous forces* to the protagonists, and are designed to evoke fear in- and out-of-universe. Many times, disability is shown through physical or mental deformities, contrasting villains from heroes who tend to have more normative bodies and act more neurotypically. Some narratives *use disability as a reason of villainy*, as a source of resentment and anger used to explain character actions and motivations. 

I seek to answer the following questions in this short project:

1. What are the topics present in trope descriptions and examples per media genre and per trope category (victim, supercrip, threat)?
2. How have trends in tropes changed over time? Which tropes or trope categories became more popular in recent years, and which fell off?


## Constructing the TvTropes dataset
I follow the same methodology as Gala et al and crawl trope descriptions and their examples in film, video games and anime from 1990 onwards. Different from their paper where they seem to have crawled all tropes and examples, I focus only on [Disability Tropes](https://tvtropes.org/pmwiki/pmwiki.php/Main/DisabilityTropes) and their examples. I also obtained the year when each work was released, and removed duplicate examples (which happens when entries of the same franchise link to the same page).

In the end, I obtained 101 tropes and 2,593 unique examples of these tropes in the same three genres of media. Trope descriptions are fairly longer than examples, with the former having a median length of 189 words vs. the latter's 26.

(PICTURE HERE)

I also manually labeled each trope based on which stereotype they perpetuate, if any (victim, supercrip, threat, or neutral). This is easy for some tropes (see [Depraved Dwarf](https://tvtropes.org/pmwiki/pmwiki.php/Main/DepravedDwarf) or [Blind Black Guy](https://tvtropes.org/pmwiki/pmwiki.php/Main/BlindBlackGuy), which overlaps with other supercrip tropes) but not for others (losing an [An Arm and a Leg](https://tvtropes.org/pmwiki/pmwiki.php/Main/AnArmAndALeg) doesn't necessarily make the character more pitied or admired in-universe) so I used the "neutral" label for tropes that are general enough based on their names and descriptions to not primarily lean towards one of the three stereotypes.


## Topics within disability tropes
I used and compared three well-known topic modeling algorithms (LDA, NMF, and GDSMM). A summary of what each algorithm does is as follows:

(TABLE HERE)

Since this section is independent from the other questions I wanted to answer, my goal is just to test out different conditions and hyperparameters, and compare the semantic coherence of each model subjectively. I don't aim to draw any conclusions on which model performs best! My findings are as follows:

1. **Keeping nouns only improves coherence**: To my knowledge, this preprocessing technique before topic modeling was first proposed in [Martin and Johnson](2015).
