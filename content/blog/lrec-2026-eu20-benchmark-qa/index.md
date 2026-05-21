---
title: "LREC 2026: Diagnosing translated benchmarks in EU20"
date: 2026-05-15
draft: false
summary: "A conference recap and reading guide on translated benchmarks, multilingual LLM evaluation, and automated QA."
tags: ["LREC", "multilingual evaluation", "benchmark QA", "machine translation"]
categories: ["conference", "paper"]
---

In May 2026, I presented our work **"Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite"** at LREC in Palma, Mallorca. What struck me most across the conference was a recurring theme: multilingual evaluation is becoming less about simply translating English benchmarks, and more about diagnosing whether our evaluation data is valid in the first place.

This post collects the main idea of our paper, the most relevant work I saw at the conference, and a few personal takeaways for researchers working on multilingual LLM evaluation.

<!--more-->

## Our paper in one paragraph

EU20 takes five well-established English benchmarks — ARC, HellaSwag, MMLU, GSM8K, and TruthfulQA — and makes them available in 20 European languages via machine translation. This is an attractive setup: it scales, it keeps the benchmark parallel across languages, and it enables cross-lingual model comparisons. But translation also introduces noise, structural inconsistencies, and uneven quality across languages and benchmark types.

Our paper adds an automated QA layer to this setup. We combine three diagnostics: a structural audit for missing fields, mismatched splits, and incomplete language coverage; item-level quality profiling with COMET; and a span-level translation error analysis using an LLM-as-a-judge based on the MQM taxonomy. The signals converge: HellaSwag concentrates the largest share of MQM Accuracy errors, especially mistranslations, and receives the lowest COMET scores, while ARC is comparatively clean. Longer translated items also tend to receive lower quality scores.

The pragmatic takeaway is simple: automated QA does not replace expert human review, but it tells us where to spend the scarce human review budget first.

## Conference highlights

Across the conference I tried to follow the thread of *benchmark validity* — and the field is clearly converging on the realization that benchmark-as-artifact deserves the same scrutiny we usually reserve for the models that get evaluated on it. I grouped the most interesting work I saw into four clusters.

### 1. Translated benchmarks need quality assurance

The closest sibling of our paper was **Ingimundarson et al.'s "Who Benchmarks the Benchmarks?"** — a case study on LLM evaluation in Icelandic. Where we ask the question quantitatively across 20 languages, they zoom into a single low/medium-resource language and conduct a hand-annotated error analysis of twelve commonly used Icelandic benchmarks. The result is striking: HellaSwag-IS was so flawed that not a single sampled item was rated valid by any annotator, machine-translated ARC variants had only 20–55% valid items, and even Belebele — which the original authors claim was built without MT — turned out to contain errors that are consistent with MT artifacts (e.g. *kalkúnn*, the Icelandic word for the bird turkey, used as if it meant the country Turkey). The paper makes the case that machine-translated benchmarks are strongly bounded by the MT quality available at the time of translation, unless they undergo substantial human review. It pairs nicely with our findings: where we measure the problem at scale with COMET and LLM judges, Ingimundarson et al. show the human-eye view of what those low scores actually look like.

**Ojastu et al.** approached the same question from yet another angle in **"Estonian WinoGrande Dataset: Comparative Analysis of LLM Performance on Human and Machine Translation."** They commissioned a human, culturally adapted Estonian translation of WinoGrande by professional translators, and then evaluated LLMs on both the human and a carefully prompt-engineered machine translation. The headline result is exactly what you would predict but rarely measured this cleanly: model performance on the human-translated benchmark is only slightly below performance on the English original, while performance on machine-translated Estonian is notably worse — and even careful prompt engineering offered only limited improvement. It's the kind of controlled experiment that turns "translation quality matters for benchmarks" from a methodological hunch into a quantifiable effect.

A third paper in this cluster came from a different geography but the same logic: **Bayes et al.'s "Uhura"** introduces a benchmark for scientific QA and truthfulness in six typologically diverse African languages, created via *human* translation of ARC-Easy and TruthfulQA. The paper documents the practical challenges of translating highly technical content for low-resource languages and finds significant performance gaps between proprietary and open-source models, with all models performing better in English than in any African language. Uhura is essentially the constructive counterpart to our diagnostic work: rather than auditing existing machine translations, the authors invest the human labor upfront and document what that takes.

### 2. Evaluation metrics and scores are not neutral

A second strand of work I followed asked a more uncomfortable question: even when our benchmark items are clean, can we trust the metrics we use to score them?

**Kostić et al.'s "Same Meaning, Different Scores"** is a sobering piece of work for anyone (including us) who treats benchmark numbers as ground truth. The authors apply two linguistically principled, meaning-preserving perturbation pipelines — synonym substitution at the lexical level and dependency-parsing-based transformations at the syntactic level — to MMLU, SQuAD, and AMEGA, and run 23 contemporary LLMs through the variants. Lexical perturbations alone induce substantial, statistically significant accuracy drops across nearly all models, and both perturbation types destabilize the relative ranking of models on the leaderboards. Worse, robustness does not scale reliably with model size. If something as innocuous as synonym substitution can move models on the leaderboard, then translation — which is essentially a much more aggressive form of perturbation — is doing the same thing, only invisibly. This is a paper I will be citing in every future translated-benchmark discussion.

**Yim et al.'s MORQA** ("Benchmarking Evaluation Metrics for Medical Open-Ended Question Answering") goes one level deeper and benchmarks not models, but *the metrics themselves*. The setup is well thought out: for three medical visual and text-based QA datasets in English and Chinese, the authors collected multiple gold-standard answers per item from medical professionals, plus expert ratings. Then they tested how well traditional metrics (BLEU, ROUGE, BERTScore) and LLM-based evaluators (GPT-4, Gemini) correlate with the expert ratings. LLM-based evaluators correlate best with expert ratings, often substantially. The paper is a good empirical reminder that whatever metric we choose to summarize translation quality or answer faithfulness, that metric itself sits on assumptions that need validating against humans. It's also the kind of meta-evaluation work that our own LLM-as-a-judge component in EU20 quietly depends on.

**Schmidtová, Dušek and Mahamood's "HotelCheckSpan"** is a methodological cousin to our span-level translation error landscape, but in a different application: the faithfulness of LLM-generated hotel summaries. They build the first span-level faithfulness dataset for the hotel domain, with three error types (Incorrect, Misleading, Not Checkable), and crucially compare human annotations to LLM-produced span judgments. The finding I found most useful for our own work: example-level agreement can mask substantial span-level disagreement — annotators (and LLMs) often agree that something is wrong in a paragraph but disagree on *where exactly* the error sits. That's an effect we observed in our MQM annotations as well, and HotelCheckSpan gives the field a clean benchmark for studying it.

**Vilar et al.’s "Benchmark Data Contamination in Underrepresented Languages"** broadens the validity discussion beyond translation quality. Instead of asking whether benchmark items are linguistically valid, the paper asks whether evaluation data for underrepresented languages may already be present in model training data, making benchmark scores less trustworthy. For me, it complemented the translated-benchmark papers well: translation artifacts are one threat to validity, but contamination is another, and both are harder to detect in lower-resource settings where benchmark provenance is often less transparent.

### 3. Translation changes meaning in subtle ways

If the previous cluster questioned our metrics, this one questions whether translation preserves what we think it preserves.

**Shafiabadi and Yvon's "Biases in Translation"** is the paper I would hand to anyone who still believes that "high BLEU/COMET implies preserved meaning." The authors formalize stance preservation as an invariance problem and adapt classical statistical tests (McNemar, two-proportion Z-test) to detect systematic shifts in the opinion expressed by a translated text. In controlled experiments with synthetically distorted translations, the tests pick up graded levels of stance manipulation. When applied to twelve multilingual models, *none* of them reliably preserved stance across all tested language directions. For a benchmark suite that translates, say, TruthfulQA — which is full of socially and politically loaded items — that's a quietly alarming finding.

**Marmonier, Sagot and Bawden's "Hindsight Quality Prediction Experiments in Multi-Candidate Human-Post-Edited Machine Translation"** approaches translation quality from the production side. The authors assembled a real MT post-editing dataset of 6,000+ English source segments with nine translation hypotheses each (a mix of traditional NMT systems and LLMs), all evaluated against a single human-post-edited reference. They then ran source-side difficulty metrics and candidate-side QE models against gold-standard TER and COMET scores. Three findings stand out: source-side difficulty metrics behave very differently depending on whether you use TER or COMET as the reference; modern QE metrics align significantly better with traditional NMT output than with LLM output; and the positional bias that used to plague document-level MT is becoming negligible with LLM translators. The take-home is that as LLMs move deeper into MT pipelines, the reliability of established QE methods is shifting under our feet — which complicates any QA pipeline (including ours) that leans on those methods.

### 4. Maybe the long-term answer is native benchmark creation

The most interesting *counter-position* I encountered came from **Lillepalu and Alumäe's "Estonian Native Large Language Model Benchmark."** Where our paper, the Icelandic case study, and the Estonian WinoGrande work all try to make translation-based evaluation more trustworthy, this paper takes the opposite stance: skip machine translation entirely, build benchmarks from native Estonian sources, and validate with human evaluation plus LLM-as-a-judge. The result is a seven-task suite that assesses general and domain-specific knowledge, grammar, vocabulary, summarization, and contextual comprehension in Estonian, tested across 6 base models and 26 instruction-tuned models. The human evaluation showed moderate-to-high correlation with the benchmark scores. It's a healthy tension to hold in mind: even if we get translated benchmarks right, native benchmarks measure something fundamentally different — and arguably more authentic — about a model's capabilities in a target language. The two approaches aren't mutually exclusive; they answer different questions.

## Personal takeaways

A few things crystallized for me over the conference:

The "translation as preprocessing" framing is over. Whether we're talking about MMLU in twenty European languages or WinoGrande in Estonian, the act of translating a benchmark is a research artifact in its own right, and it needs to be documented, audited, and ideally published alongside the model results.

Automated QA scales, human QA validates. None of the automated methods I saw — ours included — claim to replace expert review. The question is which signals are reliable enough to *prioritize* where the scarce human attention goes. COMET, LLM-as-a-judge, and a structural audit gets you a surprisingly long way.

The community is converging without coordinating. The Icelandic, Estonian, African, and Brazilian Portuguese papers all reached very similar conclusions from very different starting points. That's a strong indicator that what we documented for EU20 is not a quirk of one benchmark suite but a structural feature of how multilingual evaluation is being practiced.

Taken together, these papers suggest that multilingual evaluation is entering a more diagnostic phase. The central question is no longer only which model scores highest, but whether the benchmark, metric, translation process, and validation protocol justify the score in the first place.

## Photos

<img src="images/lrec_poster.png" alt="Presenting the paper at LREC" class="blog-image-medium">

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









