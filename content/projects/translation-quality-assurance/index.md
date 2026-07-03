---
title: "Translation Quality Assurance for Multilingual LLM Evaluation"
date: 2026-07-03
draft: false
hiddenInHomeList: true
summary: "Diagnosing translated benchmarks, and quantifying how translation errors affect multilingual LLM evaluation."
tags: ["multilingual evaluation", "translation quality", "benchmark QA", "EU20", "LREC", "ACL"]
aliases:
  - /projects/eu20-benchmark-qa/
  - /projects/tqe-translation-errors/
---

## Overview

This project brings together two closely connected papers on translation quality assurance for multilingual LLM evaluation. The first paper diagnoses quality risks in translated benchmarks and releases cleaned EU20 benchmark resources. The second paper asks how much annotated translation errors actually affect multilingual LLM benchmark accuracy.

Together, the two studies follow one research line: translated benchmarks are useful for scalable multilingual evaluation, but they need validation, documentation, and quality-aware analysis.

## Research story

### 1. Diagnosing and cleaning translated benchmarks

In **"Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite"**, we study five established benchmarks translated into 20 European languages: ARC, GSM8K, HellaSwag, MMLU, and TruthfulQA.

The work combines three quality-assurance steps:

1. **Structural audit** for missing fields, answer alignment, split consistency, and cross-language coverage.
2. **Neural quality profiling** with xCOMET-style quality estimates to identify benchmark- and language-level quality patterns.
3. **Span-level error analysis** with LLM-as-a-Judge annotation to inspect error type, severity, and location.

The outcome is **EU20-Cleaned**, a cleaned and documented version of the EU20 benchmark resources, together with scripts and documentation for the QA workflow.

### 2. Building reference resources for TQE meta-evaluation

A central part of the follow-up work is the release of reference data for evaluating automatic translation quality/error annotation methods:

- **EU20-MQMRef**: 225 benchmark items across 9 languages with MQM-style span-level human annotations.
- **Span-ACESRef**: approximately 1.4k revised items across 20 languages for span-level translation error evaluation.

These resources make it possible to ask whether automatic TQE methods and LLM-based judges can identify translation error spans in benchmark translations with enough reliability to support downstream analysis.

### 3. Quantifying downstream impact

In **"Quantifying the Impact of Translation Errors on Multilingual LLM Evaluation"**, we use annotated translation errors to estimate their effect on multilingual benchmark accuracy.

The main question is simple but important: when a model performs worse on a translated benchmark item, is the model failing, or is the benchmark translation failing?

The analysis shows that target-side translation errors are consistently associated with measurable accuracy drops, even after controlling for English correctness and source-side issues. The results suggest that translation errors can bias absolute multilingual scores downward, even when model rankings remain relatively stable.

## Papers

- **Diagnosing Translated Benchmarks: An Automated Quality Assurance Study of the EU20 Benchmark Suite**  
  Klaudia-Doris Thellmann, Bernhard Stadler, Michael Färber — **LREC 2026**.  
  arXiv: <https://arxiv.org/abs/2604.01957>

- **Quantifying the Impact of Translation Errors on Multilingual LLM Evaluation**  
  Klaudia-Doris Thellmann, Bernhard Stadler, Michael Färber, Jens Lehmann — **ACL 2026**.  
  arXiv: <https://arxiv.org/abs/2605.24904>

## Code and data

- **EU20-Cleaned / LREC artefacts:** <https://github.com/eu20-cleaned/translation-quality-analysis>
- **TQE / ACL artefacts:** <https://github.com/btqe/trans_qa>

## Related blog posts

- [LREC 2026: Diagnosing translated benchmarks](/blog/lrec-2026-eu20-benchmark-qa/)
- [ACL 2026 preview: Translation errors and multilingual LLM evaluation](/blog/acl-2026-tqe-paper/)
