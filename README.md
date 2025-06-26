# From Moments to Meanings: Egocentric Temporal Localization and Answer Generation with VSLNet and Video-LLaVA on Ego4D

This repository implements a modular pipeline for **egocentric temporal localization** and **vision-language answer generation** using the **Ego4D NLQ dataset**. We benchmark span-based models like **VSLNet/VSLBase** for query-based video segment retrieval, and extend this with **Video-LLaVA-7B** for free-form answer generation based on the localized segments.

---

## 🧠 Project Highlights

- 📍 Benchmarks 8 span-localization configurations:
  - Models: `VSLNet` vs `VSLBase`
  - Text encoders: `BERT` vs `GloVe`
  - Video encoders: `EgoVLP` vs `Omnivore`
- 📦 Generates natural language answers from top localized segments using:
  - `Video-LLaVA-7B`
  - Commercial comparisons: `Gemini 1.5/2.5`, `GPT-4o`
- 📊 Evaluation metrics:
  - Temporal: `Rank@1@0.3/0.5`, `mIoU`
  - Language: `BLEU`, `ROUGE`, `BERTScore`

---
---

## 📦 Datasets and Features

This project uses the [Ego4D Natural Language Queries](https://ego4d-data.org/tasks/natural-language-queries/) (NLQ) benchmark:

- 💡 Video features:
  - `Omnivore` (general-purpose vision encoder)
  - `EgoVLP` (egocentric-specific pretrained encoder)
- ✍️ Text embeddings:
  - `GloVe 840B.300d`
  - `BERT-base-uncased`
- 🗃️ Segment annotations: from official validation set
- 📦 Frame extraction: 8 evenly spaced frames per retrieved segment
