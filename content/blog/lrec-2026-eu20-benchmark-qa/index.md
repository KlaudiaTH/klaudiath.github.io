---
title: "LREC 2026: Diagnosing translated benchmarks"
date: 2026-05-15
draft: false
summary: "A conference recap and reading guide on translated benchmarks, multilingual LLM evaluation, and automated QA."
tags: ["LREC", "multilingual evaluation", "benchmark QA", "machine translation"]
categories: ["conference", "paper"]
---

In May 2026, I presented our work **"Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite"** at LREC in Palma, Mallorca. What struck me most across the conference was a recurring theme: multilingual evaluation is becoming less about simply translating English benchmarks, and more about diagnosing whether our evaluation data is valid in the first place.

<img src="images/lrec_poster.png" alt="Presenting the paper at LREC" class="blog-image-medium">

This post collects the main idea of our paper, the most relevant work I saw at the conference, and a few personal takeaways for researchers working on multilingual LLM evaluation.

<!--more-->

## Our paper in one paragraph

EU20 takes five well-established English benchmarks — ARC, HellaSwag, MMLU, GSM8K, and TruthfulQA — and makes them available in 20 European languages via machine translation. This is an attractive setup: it scales, it keeps the benchmark parallel across languages, and it enables cross-lingual model comparisons. But translation also introduces noise, structural inconsistencies, and uneven quality across languages and benchmark types.

Our paper adds an automated QA layer to this setup. We combine three diagnostics: a structural audit for missing fields, mismatched splits, and incomplete language coverage; item-level quality profiling with COMET; and a span-level translation error analysis using an LLM-as-a-judge based on the MQM taxonomy. The signals converge: HellaSwag concentrates the largest share of MQM Accuracy errors, especially mistranslations, and receives the lowest COMET scores, while ARC is comparatively clean. Longer translated items also tend to receive lower quality scores.

The pragmatic takeaway is simple: automated QA does not replace expert human review, but it tells us where to spend the scarce human review budget first.

## Conference highlights

Across the conference I tried to follow the thread of *benchmark validity* — and the field is clearly converging on the realization that benchmark-as-artifact deserves the same scrutiny we usually reserve for the models that get evaluated on it. I grouped the most interesting work I saw into four clusters.

### 1. Translated benchmarks need quality assurance

The closest sibling of our work was [**Ingimundarson et al.'s "Who Benchmarks the Benchmarks?"**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.369.pdf), a hand-annotated error analysis of Icelandic LLM benchmarks. Their findings are a strong human-eye counterpart to our automated QA study: several machine-translated benchmarks contained severe validity issues, including errors consistent with MT artifacts. Where our paper diagnoses translation quality at scale across EU20, this work shows what such failures look like item by item.

[**Ojastu et al.'s Estonian WinoGrande study**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.549.pdf) makes the effect even more direct: they compare LLM performance on a human-translated, culturally adapted Estonian WinoGrande benchmark with a machine-translated version. The result supports the core motivation of our paper: translation quality is not a cosmetic detail, but can measurably affect benchmark outcomes.

A constructive counterpart came from [**Bayes et al.'s "Uhura"**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.115.pdf), a scientific QA and truthfulness benchmark for six low-resource African languages built via human translation. It highlights the cost and care required when translating technical benchmark content, and complements our work by showing what a more deliberate benchmark creation process can look like.

### 2. Evaluation metrics and scores are not neutral

Several papers also questioned the reliability of evaluation scores themselves. [**Kostić et al.'s "Same Meaning, Different Scores"**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.363.pdf) shows that even meaning-preserving lexical and syntactic perturbations can change LLM accuracy and leaderboard rankings. For translated benchmarks, this is an important warning: translation is a much stronger perturbation, and its effect may remain invisible unless explicitly audited.

[**Yim et al.'s MORQA**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.396.pdf) takes the meta-evaluation question one level further by benchmarking evaluation metrics for medical open-ended QA. Their comparison of traditional metrics and LLM-based evaluators against expert ratings is a useful reminder for our own work: any automated quality signal, including LLM-as-a-judge, needs validation.

I also found [**Schmidtová et al.'s "HotelCheckSpan"**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.782.pdf) relevant to our span-level error analysis. Their work on faithfulness errors in hotel summaries shows that example-level agreement can hide disagreement about the exact error span — a methodological issue that also matters when using MQM-style span annotations for translated benchmarks.

Finally, [**Vilar et al.'s work on benchmark contamination in underrepresented languages**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.374.pdf) broadened the validity discussion beyond translation. Translation artifacts are one threat to benchmark reliability; data contamination is another. Both are especially difficult to detect in lower-resource settings.

### 3. Translation changes meaning in subtle ways

Two MT-focused papers were especially relevant to our diagnosis of translation artifacts. [**Shafiabadi and Yvon's "Biases in Translation"**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.679.pdf) studies whether machine translation preserves stance. Their finding that multilingual MT systems can systematically distort expressed opinions is highly relevant for benchmarks such as TruthfulQA, where small meaning shifts can change what is being tested.

[**Marmonier et al.'s work on hindsight quality prediction**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.688.pdf) examines translation quality estimation in a multi-candidate MT setting with human post-edits. It complements our QA pipeline by showing that as LLMs become part of MT workflows, established QE signals may behave differently depending on the translation system and reference metric.

### 4. Native benchmarks as a counterpoint

The strongest counterpoint to translation-based evaluation was [**Lillepalu and Alumäe's "Estonian Native Large Language Model Benchmark"**](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.335.pdf). Instead of translating English benchmarks, they build a suite from native Estonian sources and validate it with human evaluation and LLM-as-a-judge. For me, this framed the broader trade-off well: translated benchmarks enable scalable cross-lingual comparison, but native benchmarks may better capture what language-specific competence actually means.


## Personal takeaways

A few things crystallized for me over the conference:

The "translation as preprocessing" framing is over. Whether we're talking about MMLU in twenty European languages or WinoGrande in Estonian, the act of translating a benchmark is a research artifact in its own right, and it needs to be documented, audited, and ideally published alongside the model results.

Automated QA scales, human QA validates. None of the automated methods I saw — ours included — claim to replace expert review. The question is which signals are reliable enough to *prioritize* where the scarce human attention goes. COMET, LLM-as-a-judge, and a structural audit gets you a surprisingly long way.

The community is converging without coordinating. The Icelandic, Estonian, African, and Brazilian Portuguese papers all reached very similar conclusions from very different starting points. That's a strong indicator that what we documented for EU20 is not a quirk of one benchmark suite but a structural feature of how multilingual evaluation is being practiced.

Taken together, these papers suggest that multilingual evaluation is entering a more diagnostic phase. The central question is no longer only which model scores highest, but whether the benchmark, metric, translation process, and validation protocol justify the score in the first place.


## Links

- **Paper:** [Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite](http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.710.pdf)
- **Cleaned EU20 datasets:** https://hf.co/eu20-cleaned/datasets
- **Structural cleaning code:** https://github.com/eu20-cleaned/lang-integrity
- **LLM-as-a-judge TQE setup:** https://github.com/eu20-cleaned/translation-quality-analysis
- Slides / poster: TODO

## Citation

```bibtex
@inproceedings{thellmann-etal-2026-diagnosing,
  title     = {Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite},
  author    = {Thellmann, Klaudia and Stadler, Bernhard and F{\"a}rber, Michael},
  booktitle = {Proceedings of the Fifteenth Language Resources and Evaluation Conference (LREC 2026)},
  year      = {2026},
  pages     = {9030--9043},
  address   = {Palma, Mallorca, Spain},
  publisher = {European Language Resources Association},
  doi       = {10.63317/46mkktmq3ytw},
  url       = {http://www.lrec-conf.org/proceedings/lrec2026/pdf/2026.lrec2026-1.710.pdf}
}
```









