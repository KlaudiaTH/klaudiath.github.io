---
title: "ACL 2026 preview: Translation errors and multilingual LLM evaluation"
date: 2026-07-03
draft: false
hiddenInHomeList: true
summary: "A short preview of our ACL paper on translation errors, TQE reference data, and their impact on multilingual LLM evaluation."
tags: ["ACL", "translation quality estimation", "multilingual evaluation", "LLMs"]
categories: ["conference", "paper"]
---

The ACL main conference starts soon, so this post is still a short preview. I will add conference impressions and personal takeaways after the presentation. For now, this page collects the core idea of our paper, the public artefact repository, and the citation.

<!--more-->

## Our paper in one paragraph

Machine-translated benchmarks are widely used to evaluate multilingual LLMs, but translation errors can make benchmark results harder to interpret. In this paper, we ask two questions. First, can automatic translation quality/error annotation methods identify MQM-style error spans reliably enough to support benchmark analysis? To study this, we release two reference resources: **EU20-MQMRef**, with 225 benchmark items across 9 languages, and **Span-ACESRef**, with approximately 1.4k revised items across 20 languages. Second, we ask how much target-side translation errors affect multilingual LLM benchmark accuracy. Across translated and annotated benchmark items, we find that translation errors are associated with measurable accuracy drops even after controlling for English correctness and source-side issues. The broader takeaway is that translated benchmark scores can be biased downward by translation quality problems, even when model rankings remain relatively stable.

## Conference notes

To be added after ACL 2026.

## Links

- **Paper:** [arXiv:2605.24904](https://arxiv.org/abs/2605.24904)
- **Code, prompts, reference data, and analysis artefacts:** [btqe/trans_qa](https://github.com/btqe/trans_qa)
- **Project page:** [Translation Quality Assurance for Multilingual LLM Evaluation](/projects/translation-quality-assurance/)

## Citation

```bibtex
@misc{thellmann2026quantifying,
  title         = {Quantifying the Impact of Translation Errors on Multilingual LLM Evaluation},
  author        = {Thellmann, Klaudia-Doris and Stadler, Bernhard and F{"a}rber, Michael and Lehmann, Jens},
  year          = {2026},
  eprint        = {2605.24904},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CL},
  url           = {https://arxiv.org/abs/2605.24904}
}
```
