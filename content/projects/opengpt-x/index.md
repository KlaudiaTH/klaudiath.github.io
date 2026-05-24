---
title: "Teuken-7B and multilingual evaluation"
date: 2026-01-01
draft: false
hiddenInHomeList: true
summary: "Open, multilingual LLMs for Europe — and the tokenizer, instruction-tuning, and evaluation work behind them. Four publications from the OpenGPT-X project (2022–2025)."
tags: ["OpenGPT-X", "Teuken-7B", "European LLMs", "multilingual evaluation", "EU20", "EACL", "EMNLP", "NAACL"]
aliases:
  - /projects/teuken-european-llms/
  - /projects/multilingual-llm-evaluation/
---

## Overview

[OpenGPT-X](https://opengpt-x.de/) (2022–2025) was a BMWK-funded German consortium led by Fraunhofer IAIS to build open, multilingual large language models for Europe. I joined the project as a Research Engineer at TU Dresden's ZIH (Center for Information Services and High Performance Computing), where I focused on the **evaluation side** of the work: the pipelines, benchmarks, and comparative studies that made large multilingual LLMs comparable, reproducible, and meaningful for European languages.

The four papers below tell one continuous story — from how to tokenize multilingual text, to how to instruction-tune it, to the **Teuken-7B** model itself, and finally to the open multilingual evaluation infrastructure used to compare it with the rest of the LLM landscape.

## The story behind the papers

### 1. Getting the basics right — multilingual tokenizers

**Tokenizer Choice For LLM Training: Negligible or Crucial?** — *Findings of NAACL 2024*

Across 24 mono- and multilingual 2.6B-parameter models, we asked a basic question that turns out to matter a lot: how much does tokenizer design cost you in multilingual training? An English-centric tokenizer can inflate training cost by **up to 68%** on multilingual data. Tokenizers trained on a balanced language mix produce shorter sequences (lower fertility), keep multilingual training within budget, and yield better non-English downstream performance.

*My contribution:* built and scaled the evaluation pipelines comparing Byte-Pair Encoding vs. Unigram tokenizers, adapted EleutherAI's *LM Evaluation Harness* for tokenizer-aware testing, and ran the fertility and downstream-task analyses (reasoning, world knowledge, truthfulness).

### 2. Teaching the model to follow multilingual instructions

**Investigating Multilingual Instruction-Tuning: Do Polyglot Models Demand for Multilingual Instructions?** — *EMNLP 2024*

We built a 1,030-instruction multilingual dataset across five languages and asked whether multilingual models really need multilingual instructions. They do: multilingual instruction-tuning improves low-resource accuracy by **up to 10%**, and human-curated data substantially outperforms synthetic. Diversity and dataset size matter most.

*My contribution:* extended the *FastChat* instruction-tuning framework, ran and managed the fine-tuning experiments on HPC, and shaped dataset selection and preparation.

### 3. Teuken-7B — the open European LLM

**Teuken-7B-Base & Teuken-7B-Instruct: Towards European LLMs** — *EACL 2025*

The flagship deliverable: a 7B-parameter LLM trained from scratch on all **24 EU official languages**, with a strong focus on non-English European language data, plus an instruction-tuned variant. The tokenizer and instruction-tuning design follow directly from the two studies above. The full pipeline — data curation, training, instruction-tuning, evaluation — is openly documented, and the models are released for research and commercial use on Hugging Face.

*My contribution:* designed and ran the evaluation suite used throughout development — comparing every Teuken development snapshot against state-of-the-art baselines so that decisions on model and data choices were always grounded in empirical comparisons.

### 4. A trustworthy evaluation infrastructure for European LLMs

**Towards Multilingual LLM Evaluation for European Languages** — *arXiv 2025*

This work turned the in-house evaluation infrastructure into a public resource. We translated five widely used benchmarks into 20 EU languages — the **EU20** benchmark suite — and evaluated **40+ state-of-the-art LLMs** on reasoning, knowledge, and truthfulness. The resulting **European LLM Leaderboard** on Hugging Face exposes a >20% accuracy gap between high- and mid-resource languages. We also validated the automated benchmarks against human preferences from the Chatbot Arena and found strong agreement — supporting EU20 as a reliable multilingual evaluation instrument.

*My contribution:* led the planning and execution of the multilingual evaluation, the release of the European LLM Leaderboard, and the open publication of the EU20 benchmarks.

## Papers

- **Tokenizer Choice For LLM Training: Negligible or Crucial?**  
  Mehdi Ali, Michael Fromm, Klaudia-Doris Thellmann, *et al.* — *Findings of NAACL 2024*.  
  DOI: <https://doi.org/10.18653/v1/2024.findings-naacl.247>

- **Investigating Multilingual Instruction-Tuning: Do Polyglot Models Demand for Multilingual Instructions?**  
  Alexander Arno Weber, Klaudia-Doris Thellmann, Jan Ebert, *et al.* — *EMNLP 2024*.  
  DOI: <https://doi.org/10.18653/v1/2024.emnlp-main.1159>

- **Teuken-7B-Base & Teuken-7B-Instruct: Towards European LLMs.**  
  Mehdi Ali, Michael Fromm, Klaudia-Doris Thellmann, *et al.* — *EACL 2025*.  
  arXiv: <https://arxiv.org/abs/2410.03730>

- **Towards Multilingual LLM Evaluation for European Languages.**  
  Klaudia-Doris Thellmann, Bernhard Stadler, Michael Fromm, *et al.* — *arXiv 2025*.  
  arXiv: <https://arxiv.org/abs/2410.08928>

## Code & models

- **Teuken-7B models on Hugging Face:**
  - [Teuken-7B-base-v0.6](https://huggingface.co/openGPT-X/Teuken-7B-base-v0.6)
  - [Teuken-7B-instruct-v0.6](https://huggingface.co/openGPT-X/Teuken-7B-instruct-v0.6)
  - [Teuken-7B-instruct-research-v0.4](https://huggingface.co/openGPT-X/Teuken-7B-instruct-research-v0.4) · [Teuken-7B-instruct-commercial-v0.4](https://huggingface.co/openGPT-X/Teuken-7B-instruct-commercial-v0.4)
- **European LLM Leaderboard:** [Hugging Face Space](https://huggingface.co/spaces/openGPT-X/european-llm-leaderboard)
- **Leaderboard data:** [openGPT-X/leaderboard_data](https://huggingface.co/datasets/openGPT-X/leaderboard_data) · [leaderboard_data_ogx](https://huggingface.co/datasets/openGPT-X/leaderboard_data_ogx)
- **Code on GitHub:** [OpenGPTX organization](https://github.com/OpenGPTX) — including OpenGPTX forks of [lm-evaluation-harness](https://github.com/OpenGPTX/lm-evaluation-harness) (used and extended for multilingual evaluation) and [Megatron-LM](https://github.com/OpenGPTX/Megatron-LM) (used for training)
- **Hugging Face organization:** [openGPT-X](https://huggingface.co/openGPT-X)

## Press & project links

- [Teuken-7B project page](https://opengpt-x.de/en/models/teuken-7b/) — official project site
- [Fraunhofer IAIS — Teuken-7B release (Nov 2024, DE)](https://www.iais.fraunhofer.de/de/presse/presseinformationen/presseinformationen-2024/presseinformation-241126.html)
- [FZ Jülich — Multilingual and open-source: OpenGPT-X releases a large AI language model (DE)](https://www.fz-juelich.de/de/aktuelles/news/meldungen/2024/multilingual-und-open-source-opengpt-x-veroeffentlicht-grosses-ki-sprachmodell)
- [TU Dresden — Teuken-7B release news (DE)](https://tu-dresden.de/tu-dresden/newsportal/news/mehrsprachig-und-open-source-forschungsprojekt-opengpt-x-veroeffentlicht-grosses-ki-sprachmodell)
- [TU Dresden — European LLM Leaderboard release news](https://tu-dresden.de/ing/informatik/die-fakultaet/news/european-llm-leaderboard-of-opengptx)
- [Fraunhofer IAIS — OpenGPT-X topic page (DE)](https://www.iais.fraunhofer.de/de/branchen-themen/themen/generative-ki/opengpt-x.html)
