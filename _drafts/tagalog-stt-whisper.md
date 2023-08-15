---
layout: post
title: "Evaluating OpenAI Whisper's performannce on Tagalog speech-to-text"
date: 2023-08-15
category: notebook
comments: true
author: "ELYANAH ACO"
description: ""
---

I've been testing out [OpenAI Whisper](https://openai.com/research/whisper) to transcribe different English audio files for the past few days. I have to say that I'm impressed - it picks up changes of thoughts and stutters and translates numbers and units very well. It might not always spell out names and locations correctly (which is understandable), but it does pick up when those words are named entities most of the time. It also reasonably predicts when the current statement is an exclamation or a question and uses the appropriate ending punctuations.

I was curious how well Whisper would perform on Tagalog. It is a supported language and the model has a rather average [Word Error Rate (WER) on the FLEURS dataset](https://github.com/openai/whisper) compared to other languages. Plus, I wanted to write about my mother tongue this Buwan ng Wika (literally Month of Language). So, here's a little write-up on that!

## What is Whisper?

## Where a whisper is enough: results and findings

I picked four news Youtube videos that were each less than 5 minutes long and were mostly in Tagalog:
* [*36 tao, patay sa pananalasa ng wildfires sa Maui, Hawaii* ](https://www.youtube.com/watch?v=FfQ5qF3HlEY) from UNTV News
* [*DOTr: Konstruksyon ng Metro Manila subway nasa 33% na*](https://www.youtube.com/watch?v=5gL0NEqVoL0) from CNN Philippines
* [*Presyo ng palay tumaas, rice millers nababahala*](https://www.youtube.com/watch?v=qgeZzoWwMPw) from TV Patrol
* [*Tropical Storm na binabantayan sa Pacific Ocean, lumakas*](https://www.youtube.com/watch?v=wMubl9JiQCg) from GMA Integrated News

Note that none of these were *fully* in Tagalog, as code-switching (CS) is commonplace and sometimes even done in formal writing. Most of the CS observed was intrasentential, although there was also some intraword CS observed (eg. *mag-subscribe*).

I ran each video through Whisper, setting the language to `tl`, and produced model transcriptions. I also manually transcribed the videos, making sure to match mine with Whisper's on the same audio portions. I then measured how Whisper's transcriptions measured against my own by computing commonly used metrics for speech recognition performance, which are summarized below:


(METRICS HERE)

Qualitatively, here's where I think Whisper excels:

* **Spelling out syllables**: I suspect that Whisper failed to recognize many words from the audio, so it instead performed syllabic transcription. I honestly think it did a good job with this respect since I could recognize some words from their slightly broken and misspelled transcriptions. 

    Unsurprisingly, Whisper has a hard time distinguisihing between the letters "u" and "o", between "i" and "y", and between "d", "k" and "t".


And here's where I think Whisper falls short:
* **Combining syllables into words**: I mentioned earlier that Whisper does a passable job syllabicizing words. It struggles a bit more in understanding which syllables to combine or not. Many of the transcriptions either split up syllables that should be in one word (like *pang hukai* for *panghukay*), or combine syllables from different words (like *parau* which should be *pa raw*). I think the former happens when a word segment is itself a word, which would be an difficult hurdle to overcome given how affix-heavy Tagalog is.

* **Recognizing Filipino named entities**: I was expecting Whisper to misspell named entities. What I didn't expect was Whisper having a hard time figuring out if a noun or noun phrase is a named entity. Entities like *Metro Manila*, *Valenzuela* or *Agosto* were consistently transcribed as if they were common nouns. In contrast, Whisper was able to recognize English entities like *United Nations* and *Maui Consistently* more consistently, although it had a difficult time with some names like *Abi*.

    That being said, Whisper did get it right sometimes, like recognizing (and actually spelling them almost correctly!) *Luzon*, *Visayas* and *Mindanao*. Any entities that were verbally spelled out like *DOTR* were also recognized perfectly.

* **


