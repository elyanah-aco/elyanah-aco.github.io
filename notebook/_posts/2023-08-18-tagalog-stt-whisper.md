---
layout: post
title: "Evaluating OpenAI Whisper's performance on Tagalog speech-to-text"
date: 2023-08-18
category: notebook
comments: true
author: "ELYANAH ACO"
description: "A quick look at Whisper's Tagalog transcriptions"
---

I've been testing out [OpenAI's Whisper](https://openai.com/research/whisper) to transcribe different English audio files for the past few days. I have to say that I'm impressed - it picks up false starts and stutters, and translates numbers and units very well. It might not always spell out names and locations correctly (which is understandable), but it does pick up when those words are named entities most of the time. It also reasonably predicts when the current statement is an exclamation or a question and uses the appropriate ending punctuations.

I was curious how well Whisper would perform on Tagalog. It is one of 99 supported languages and the model has a rather average [Word Error Rate (WER) on the FLEURS dataset](https://github.com/openai/whisper) for Tagalog when also accounting for the other languages. Plus, I wanted to write about my mother tongue this Buwan ng Wika (literally Month of Language). So, here's a little write-up on that!

## What's in a whisper: Model motivation and background
Whisper is an automatic speech recognition (ASR) model trained in a weakly-supervised manner on up to 680,000 hours of noisy labeled audio data. Whisper follows recent works in computer vision which found that moving beyond gold-standard, crowdsourced datasets to much larger but lower-quality datasets helped improved the robustness and generalization of models to different downstream tasks. 

Whisper itself is not finetuned to any particular dataset. As such, it does not outperform models finetuned to specific datasets, such as those trained on the LibriSpeech dataset (which is just around 960 hours of data). However, it generalizes quite well to different domains and datasets. Moreover, Whisper was trained on about 117,000 hours of non-English audio, allowing for multilingual transcription and translation.

As for model architecture, Whisper is a sequence-to-sequence encoder-decoder transformer. Audio is split into 30-second chunks and converted into a log-Mel spectogram before being passed and processed in the encoder. This transformer is also trained multitude of tasks such as language identification, voice activity detection, timestamp detection, etc. which are jointly represented as a sequence of special tokens passed into and to be predicted by the decoder. The decoder also uses previous transcribed texts to help resolve ambiguous audio and to provide context in longer-range transcriptions.

![](/assets/png/tagalog-whisper-stt/whisper_architecture.PNG){:width="700px"}
{: style="text-align: center;"}
*Image taken from Radford et al (2022)*


## Where a whisper is enough: Results and findings

I picked four news Youtube videos, each less than 5 minutes long and were mostly in Tagalog:
* [*36 tao, patay sa pananalasa ng wildfires sa Maui, Hawaii* ](https://www.youtube.com/watch?v=FfQ5qF3HlEY) from UNTV News
* [*DOTr: Konstruksyon ng Metro Manila subway nasa 33% na*](https://www.youtube.com/watch?v=5gL0NEqVoL0) from CNN Philippines
* [*Presyo ng palay tumaas, rice millers nababahala*](https://www.youtube.com/watch?v=qgeZzoWwMPw) from TV Patrol
* [*Tropical Storm na binabantayan sa Pacific Ocean, lumakas*](https://www.youtube.com/watch?v=wMubl9JiQCg) from GMA Integrated News

Note that none of these were fully in Tagalog, as code-switching (CS) is commonplace and sometimes even done in formal writing. Most of the CS observed was intrasentential, although there was also some intraword CS observed (eg. *mag-subscribe*).

I ran each video through Whisper, setting the language to `tl`, and produced model transcriptions. I also manually transcribed the videos, making sure to follow Whisper's transcription format (using symbols instead of spelling out units, adding punctuation marks) and to match my work with Whisper's on the same audio portions. 

To see how Whisper's transcriptions measured against my own, I computing commonly used metrics for speech recognition performance. All evaluations were made using the `jiwer` package [here](https://jitsi.github.io/jiwer/). A value closer to 0 means better accuracy in recognizing speech for all metrics.

<table>
<thead>
  <tr>
    <th colspan="2"></th>
    <th colspan="4">Average value per video<br></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Metric</td>
    <td>Description</td>
    <td>Hawaii</td>
    <td>MM Subway</td>
    <td>Wheat price</td>
    <td>Tropical storm</td>
  </tr>
  <tr>
    <td>Word Error Rate (WER)</td>
    <td>The minimum edit distance between changes to the hypothesis (model) transcription, expressed as the number of substitutions, deletions and insertions of words, and the number of words in the reference (human) transcription.</td>
    <td>0.6616</td>
    <td>0.5145</td>
    <td>0.5571</td>
    <td>0.5289</td>
  </tr>
  <tr>
    <td>Match Error Rate (MER)</td>
    <td>The proportion of matches among all input/output (I/O) word matches between transcriptions that is incorrect.</td>
    <td>0.6173</td>
    <td>0.4637</td>
    <td>0.5352</td>
    <td>0.5008</td>
  </tr>
  <tr>
    <td>Character Error Rate (CER)</td>
    <td>Is similar to WER, except that it measures changes on a character-level.</td>
    <td>0.2746</td>
    <td>0.1785</td>
    <td>0.1677</td>
    <td>0.1699</td>
  </tr>
</tbody>
</table>

Qualitatively, here's where I think Whisper works well:
* **Transcribing English words**: I suppose this isn't a big surprise, but it's interesting to see how often it correctly transcribed English words amidst a sea of slightly wrong Tagalog ones. Take for example this snippet from the Hawaii video:

    | Whisper transcription | Human transcription |
    | ---------------------- | -------------------- |
    | kasama sa marin sa mga napin sala yung historic landmark na banyan tree. | kasama sa- kasama rin sa mga napinsala yung historic landmark na Banyan Tree. Mary Jo |

    It also does a good job of transcribing English numbers, directions and units. Segments purely in English were transcribed perfectly even up to recognizing when sentences started and ended.

* **Spelling out syllables**: I suspect that Whisper failed to recognize many words from the audio, so it instead performed syllabic transcription. Some letters are commonly interchanged, like "b" and "p", "u" and "o", "i" and "y", and "d", "k" and "t". Some words ending in vowels would also have random consonants added to the end of them. But overall, I honestly think it did a good job with this respect to the point that I could sometimes recognize some words from their slightly broken and misspelled transcriptions. The relatively low CERs also shows that this syllabic transcription works quite well.

And here's where I think Whisper falls short:
* **Combining syllables into words**: I mentioned earlier that Whisper does a good job syllabicizing the audio. It struggles a bit more in understanding which syllables to combine or not. Many of the transcriptions either split up syllables that should be in one word (like *pang hukai* for *panghukay*), or combine syllables from different words (like *parau* which should be *pa raw*). I think this point is why the CERs are relatively much lower compared to the WERs or MERs. Looking at Whisper's transcriptions, I theorize that the former happens when a word segment is itself a word. This would be a difficult hurdle to overcome given that Tagalog has many commonly used short words. 

* **Recognizing Filipino named entities**: I was expecting Whisper to misspell named entities. What I didn't expect was Whisper having a hard time figuring out if a noun or noun phrase is a named entity. Entities like *Metro Manila*, *Valenzuela* or *Agosto* were consistently transcribed as if they were common nouns. In contrast, Whisper was able to recognize English entities like *United Nations* and *U.S. President Joe Biden* fairly accurately, although it still made a few minor mistakes here and there.

    That being said, Whisper did get it right sometimes, like recognizing (and actually spelling them almost correctly!) *Luzon*, *Visayas* and *Mindanao*. Some enities that were verbally spelled out like *DOTR* were also recognized, but some like *GMA* weren't.

* **Missing the mark entirely**: Sometimes Whisper just bugs out and produces completely wrong transcriptions. Take this snippet from the Hawaii video:

    | Whisper transcription | Human transcription |
    | ---------------------- | -------------------- |
    | sa mga napin salak, masang sa mga napin salak, masang sa mga napinsalak | kamusta naman yung mga ulat ng mga nawawala Abi? Meron pa rin bang mga nawawala? |

    The Whisper transcription seems to be working from the phrase "sa mga napinsala" repeatedly, which was in the previous audio segment. However, this phrase was at the middle of the segment, and words after that phrase were transcribed fairly well. Other users have also experienced this specific hallucination [here](https://huggingface.co/spaces/sanchit-gandhi/whisper-jax/discussions/2).

    For some reason, Whisper can also translate segments to English (and quite poorly if I may add) without prompting it to do so:

    | Whisper transcription | Human transcription |
    | ---------------------- | -------------------- |
    | And the standard speed is high. So even if we are not able to see it, it is just a little bit longer. | yun naman na yung standard speed. So kahit pa gusto nating bilisan, hanggang doon na lang naman siya.
    |

    Finally, Whisper can also completely leave out specific segments, like in this snippet from the Tropical Storm video:

    | Whisper transcription | Human transcription |
    | ---------------------- | -------------------- |
    | Apektado ni to ang lobby waiting aisle, ward at emergency room. | Apektado nito ang lobby, waiting aisle, ward at emergency room. Kahit maulan, marami pa rin daw ang nagpupunta sa ospital. Bangka na rin nga ang ginagamit ng paghatid ng mga pasyente.
    |

    Interestingly, this wasn't the last segment of the video. None of the following segments had this issue as well. It begs the question on how these bugs happen in the first place!

## Is Tagalog Whisper good enough?

Do I think that Whisper does a good enough job at transcribing Tagalog audio?

I recommend using Whisper as a first pass and if you'll be doing a lot of manual cleaning to fix these issues. I still think that a native Tagalog speaker could figure out a few words from the syllabicized transcriptions (maybe after a lot of squinting and thinking, but still). I imagine this alone is already a huge timesaver for any transcribers looking to save some time. 

It's definitely not advisable to use Whisper as-is for production though. I also doubt that someone just learning Tagalog would be able to recognize what the transcriptions are originally supposed to be. There are many errors with the transcriptions, many of which I think are too complicated to fix algorithmically. Whisper also hallucinates by repeating phrases multiple times, translating audio to English, missing entire audio segments and [even adding words not present in the original audio](https://community.openai.com/t/how-to-avoid-hallucinations-in-whisper-transcriptions/125300). OpenAI hypothesizes that these hallucinations occur when the model tries predicting the next word in audio and transcribing the audio itself at the same time.

Of course, one can finetune Whisper to work better on Tagalog data. I probably won't delve deeper into this in the future, but I recommend checking out this [HuggingFace repo](https://github.com/huggingface/community-events/tree/main/whisper-fine-tuning-event#set-up-an-environment) that contains eveything that you need to know to do this.

## References

* Andrew C. Morris, Viktoria Maier, and Phil Green. 2004. From WER and RIL to MER and WIL: improved evaluation measures for connected speech recognition. In *Proceedings of the 8th International Conference on Spoken Language Processing*, Jeju Island, South Korea.

* Alec Radford, Jong Wook Kim, Tao Xu, Greg Brockman, Christine McLeavey, and Ilya Sutskever. 2022. "Robust Speech Recognition via Large-Scale Weak Supervision". *arXiv preprint arXiv:2212.04356*.

* Vered Silber Varod, Ingo Siegert, Oliver Jokisch, Yamini Sinha, and Nitza Geri. 2021. A cross-language study of speech recognition 
systems for English, German, and Hebrew. *Online Journal of Applied Knowledge Management 9*(1).



