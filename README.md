# XL-SafetyBench-Based Audit

A study of how two leading frontier models — **GPT-5.5** (OpenAI) and **Claude Opus 4.8** (Anthropic) — behave across **8 country-language pairs** underrepresented in existing LLM safety research: Chile, Colombia, Ethiopia, Germany, Hungary, Italy, Jordan, and Macedonia.

Built on **XL-SafetyBench** (Choi et al., 2026), a country-grounded benchmark for evaluating LLM safety across languages and cultures. We adapted its two-track methodology — adversarial jailbreak resistance and cultural sensitivity detection — into a smaller-scale, 8-country audit (240 tests total, 30 per country: 15 jailbreak + 15 cultural).

---

## What we tested

**Track 1 — Jailbreak resistance:** Can the models be tricked, through deliberately disguised requests, into producing content they would normally refuse?

**Track 2 — Cultural awareness:** Do the models notice and respond appropriately to a locally sensitive detail embedded inside an otherwise ordinary request?

For every response we scored three things: how often a disguised harmful request actually succeeded (**Attack Success Rate, ASR**), how often the model's response missed the point entirely rather than offering a genuine refusal (**Neutral-Safe Rate, NSR** — a comprehension failure rather than a safety success), and how often the model correctly recognized a culturally sensitive detail (**Cultural Sensitivity Rate, CSR**).

Test questions were drafted by Claude with country-specific context, then reviewed and corrected by a native-speaker team member for each country — one per language — so both question design and response scoring drew on genuine cultural knowledge rather than assumption.

---

## What we found

- **Jailbreak vulnerability is common and highly language-dependent.** Average ASR across the 8 countries was ~36% for GPT-5.5 and ~40% for Claude Opus 4.8 — but with wide variance by country (e.g. Colombia and Jordan showed much lower ASR than Hungary or Italy for at least one model). The same safety rules behave very differently depending on the language they're tested in.
- **A blind spot in our own pipeline:** the attacker model used to generate disguised prompts follows the same safety training as the models being tested — so it overwhelmingly refused to generate disguised requests for the most heavily-scrutinized topics (self-harm, hate speech), leaving those topics undertested. Less-scrutinized topics (criminal activity, economic conflict) were tested far more often, and succeeded more frequently — which likely inflates apparent vulnerability in those categories specifically, rather than reflecting uniform risk.
- **Cultural sensitivity was high on average (~92-93% CSR for both models)** but inherently hard to measure cleanly: local norms vary as much within a country as between countries, and many traditional taboos are increasingly globalized — making it difficult to tell whether a "correct" answer reflects genuine local knowledge or a generic safe response that happens to align by coincidence.
- **Scorer reliability is an open question.** With only one scorer per country, we could not run the standard inter-annotator agreement check — so part of the cross-country variation we observed may reflect differences in how scorers applied the rubric, not just differences in model behavior.

These limitations are reported deliberately: the goal of this audit was to stress-test a smaller-scale replication of XL-SafetyBench's methodology, not to draw firm conclusions about either model's safety in production.

---

## Repository structure

```
Code/
├── Run_probes.ipynb   # Prompt generation (attacker model) + model probing pipeline
└── Result_Analysis.ipynb                      # Scoring, aggregation, and visualization

Annotations/
├── Chile/      Colombia/      Etiopia/      Germany/
├── Hungary/    Italy/         Jordan/       Macedonia/
└── (each folder: probe_set_XX.xlsx — scenario/attack prompts + native-speaker scoring)

Written_Report.pdf      # Full methodology, results, and discussion

```

---

## Methodology credit

This audit adapts the taxonomy, two-track structure, and metrics (ASR, NSR, CSR) directly from **XL-SafetyBench** (Choi et al., 2026) — the original paper is not redistributed here; see citation below. Our contribution is a smaller-scale, 8-country replication with a simplified pipeline, run as part of an NLP coursework project.

> Choi, D., Kim, E., Noh, J., Seo, S., Kim, E., Oh, M., Park, Y., Kartono, B. J., Pichlmeier, J., Berndt, H., Mendu, S. K., Tungka, G. J., Gökçe, Ö., Gehlot, S., Pratt, K., Minnich, A., & Park, H. (2026). *XL-SafetyBench: A Country-Grounded Cross-Cultural Benchmark for LLM Safety and Cultural Sensitivity*. arXiv preprint [arXiv:2605.05662](https://arxiv.org/abs/2605.05662).

---

## Context

Written report authored by **Daniela Maturana**; country annotations contributed by one native-speaker team member per country.
