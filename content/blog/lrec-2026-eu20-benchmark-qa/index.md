---
title: "LREC 2026: Diagnosing translated benchmarks"
date: 2026-05-15
draft: false
hiddenInHomeList: true
summary: "A short conference note on translated benchmarks, EU20-Cleaned, and automated quality assurance for multilingual LLM evaluation."
tags: ["LREC", "multilingual evaluation", "benchmark QA", "machine translation"]
categories: ["conference", "paper"]
---

In May 2026, I presented our work **"Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite"** at LREC in Palma, Mallorca. The main theme I took away from the conference was that multilingual evaluation is moving beyond simply translating English benchmarks. We also need to ask whether the translated evaluation data is structurally sound, semantically reliable, and documented well enough to support fair model comparisons.

<img src="images/lrec_poster.png" alt="Presenting the paper at LREC" class="blog-image-medium">

This post briefly summarizes our paper and my main personal takeaways from the conference.

<!--more-->

## Our paper in one paragraph

EU20 makes five established English benchmarks — ARC, GSM8K, HellaSwag, MMLU, and TruthfulQA — available in 20 European languages. This makes multilingual LLM evaluation scalable, but it also introduces a risk: translation can break structure, change meaning, or create uneven quality across datasets and languages. Our paper adds an automated quality-assurance layer to this setup. We combine a structural audit, neural quality profiling with xCOMET-style scores, and span-level error analysis with LLM-as-a-Judge annotation. The results show that quality risks are not evenly distributed: HellaSwag is particularly difficult, while ARC is comparatively clean. The main takeaway is practical: automated QA does not replace expert human review, but it helps identify where human review is needed most.

## Personal takeaways

For me, the strongest message from LREC was that benchmark validity is becoming a central research topic in multilingual NLP. Several papers approached this from different angles. Work on Icelandic benchmark quality by [Ingimundarson et al.](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.369.pdf) and Estonian WinoGrande by [Ojastu et al.](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.549.pdf) reinforced the same point as our study: translated benchmarks can contain errors that affect what is being measured. At the same time, work such as [Uhura](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.115.pdf) showed how carefully designed human translation can support benchmark construction for low-resource languages.

A second takeaway was that evaluation scores themselves need more scrutiny. Papers such as [Same Meaning, Different Scores](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.363.pdf) and [MORQA](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.396.pdf) made clear that model scores depend not only on model ability, but also on benchmark wording, evaluation metrics, and validation protocols. This connects directly to our work: translation quality is one part of a larger reliability problem in multilingual evaluation.

Finally, I left the conference with a stronger sense that translated and native benchmarks should be seen as complementary. Translated benchmarks enable controlled cross-lingual comparison, while native benchmarks can better capture language-specific and culture-specific knowledge. A robust multilingual evaluation ecosystem will likely need both.

## Links

- **Paper:** [arXiv:2604.01957](https://arxiv.org/abs/2604.01957)
- **Code and EU20-Cleaned data:** [eu20-cleaned/translation-quality-analysis](https://github.com/eu20-cleaned/translation-quality-analysis)
- **Project page:** [Translation Quality Assurance for Multilingual LLM Evaluation](/projects/translation-quality-assurance/)

## Citation

```bibtex
@misc{thellmann2026diagnosing,
  title         = {Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite},
  author        = {Thellmann, Klaudia-Doris and Stadler, Bernhard and F{"a}rber, Michael},
  year          = {2026},
  eprint        = {2604.01957},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CL},
  url           = {https://arxiv.org/abs/2604.01957}
}
```
