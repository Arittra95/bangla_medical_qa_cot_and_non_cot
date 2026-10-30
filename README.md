# Bangla Medical QA: CoT and Non-CoT

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Dataset on Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Dataset-Hugging%20Face-yellow)](https://huggingface.co/datasets/Arittra/bangla_medical_qa_cot_and_non_cot)

A Bengali (Bangla) medical question-answering dataset with two subsets. This repository contains the documentation and the scripts used to build it. **The data itself is hosted on Hugging Face:**

**https://huggingface.co/datasets/Arittra/bangla_medical_qa_cot_and_non_cot**

> **Not medical advice.** This dataset is for research and model development only. See [Limitations](#limitations-and-warnings).

| Subset | Rows | What it contains |
|---|---|---|
| `cot` | 6,276 | Clinical questions with a step-by-step **reasoning trace** and a final answer |
| `non_cot` | 3,994 | Questions with a direct answer (no reasoning trace) |

Total: **10,270** examples. Every example is classified into a medical specialty (`category`).

## সারসংক্ষেপ (বাংলায়)

এই ডেটাসেটটি বাংলা ভাষায় চিকিৎসা-বিষয়ক প্রশ্নোত্তরের একটি সংগ্রহ। এতে মোট **10,270টি** উদাহরণ আছে এবং এটি দুটি অংশে বিভক্ত:

- **`cot` (6,276টি):** ক্লিনিক্যাল প্রশ্ন, ধাপে ধাপে যুক্তি-বিশ্লেষণ (Chain-of-Thought) এবং চূড়ান্ত উত্তর।
- **`non_cot` (3,994টি):** প্রশ্ন ও সরাসরি উত্তর, কোনো যুক্তি-বিশ্লেষণ ছাড়া।

প্রতিটি উদাহরণ চিকিৎসার একটি শাখা (যেমন Cardiology, Paediatrics, Dermatology) অনুযায়ী শ্রেণিবদ্ধ।

**উৎস:** তথ্য নেওয়া হয়েছে HuatuoGPT-o1 (medical-o1-reasoning-SFT), MedReason এবং doctor_qa_bangla থেকে। মূল লেখকদের প্রতি কৃতজ্ঞতা; বিস্তারিত রেফারেন্স নিচের "Sources and attribution" অংশে আছে।

**কীভাবে তৈরি:** মূল ইংরেজি/চীনা লেখা **Gemma 4** দিয়ে বাংলায় অনুবাদ করা হয়েছে, তারপর শাখা অনুযায়ী শ্রেণিবদ্ধ করা হয়েছে। এরপর পরিষ্কার করার ধাপে অনুবাদের ত্রুটিযুক্ত সারি বাদ দেওয়া হয়েছে, যেমন বাংলা লেখার মাঝে রুশ, চীনা, থাই বা আরবি অক্ষর ঢুকে যাওয়া, কাটা প্রশ্ন, অতিরিক্ত দীর্ঘ লেখা ও হুবহু ডুপ্লিকেট। মোট 12,188টি সারির মধ্যে 1,918টি বাদ গেছে।

**সীমাবদ্ধতা ও সতর্কতা:**
- এটি **চিকিৎসা পরামর্শ নয়**। প্রকৃত রোগী-চিকিৎসার সিদ্ধান্তে চিকিৎসকের তত্ত্বাবধান ছাড়া ব্যবহার করবেন না।
- অনুবাদ মেশিন দিয়ে করা; চিকিৎসা-বিশেষজ্ঞরা সম্পূর্ণ যাচাই করেননি, তাই পরিভাষা, সংখ্যা বা এককে ভুল থাকতে পারে।
- `cot` অংশের যুক্তি-বিশ্লেষণ মূলত এআই-তৈরি, তাই এতে চিকিৎসাগত ভুল থাকতে পারে।
- বেশিরভাগ বিষয়বস্তু মার্কিন/চীনা পরীক্ষা বা কেস-ভিত্তিক প্রশ্ন থেকে এসেছে; বাংলাদেশের রোগের প্রকোপ, ওষুধ ও চিকিৎসা-নির্দেশিকার সঙ্গে সবসময় মিলবে না।

**ব্যবহার:** বাংলা মেডিকেল এনএলপি গবেষণা, ভাষা-মডেল ফাইন-টিউনিং ও মূল্যায়ন, এবং স্বল্প-সম্পদের ভাষা নিয়ে কাজের জন্য।

**লাইসেন্স:** Apache-2.0, মূল উৎসগুলোর লাইসেন্সের সঙ্গে সামঞ্জস্যপূর্ণ।

## Quick start

```bash
pip install datasets
```

```python
from datasets import load_dataset

cot = load_dataset("Arittra/bangla_medical_qa_cot_and_non_cot", "cot", split="train")
non_cot = load_dataset("Arittra/bangla_medical_qa_cot_and_non_cot", "non_cot", split="train")

print(cot[0]["question"])
print(cot[0]["reasoning"])
print(cot[0]["answer"])
```

## Repository contents

| Path | Purpose |
|---|---|
| `README.md` | This file |
| `LICENSE` | Apache License 2.0 |
| `scripts/` | Scripts used to clean, deduplicate and split the data (the data files themselves are **not** stored here; see the Hugging Face link above) |

[EDIT: list your actual script file names here, e.g. `scripts/clean_dataset.py`.]

## How it was made

1. Source material was collected from the datasets listed under [Sources and attribution](#sources-and-attribution).
2. English/Chinese-origin text was **machine-translated into Bengali with Gemma 4**.
3. Examples were **classified by specialty** (see categories below).
4. A cleaning pass removed rows with translation artifacts: stray Cyrillic, CJK, Thai, Arabic and other non-Bengali/non-Latin script mixed into Bengali text, truncated questions, extreme length outliers and exact duplicates. Of 12,188 candidate rows, 1,918 were removed (1,891 for foreign-script contamination).

> [EDIT: describe here, in your own words, exactly how you adapted content to the Bangladeshi context (e.g. local drug brands, ORS, dengue, EPI schedule, referral setting). Only claim localization that you actually applied.]

## Data fields

`cot`: `id`, `source`, `source_idx`, `category`, `question`, `reasoning`, `answer`

`non_cot`: `id`, `source`, `source_idx`, `category`, `question`, `answer`

- `source`: which upstream dataset the example came from.
- `source_idx`: row index in the upstream dataset, where available (null for `doctor_qa_bangla`).
- `reasoning`: the translated reasoning trace (the `<thought>` content, tags removed).

## Composition

| Subset | Source | Rows |
|---|---|---|
| cot | HuatuoGPT-o1 (medical-o1-reasoning-SFT) | 6,276 |
| non_cot | MedReason (question + final answer only) | 1,733 |
| non_cot | doctor_qa_bangla (curated) | 2,261 |

<details>
<summary><b>Rows per category</b></summary>

| Category | Rows |
|---|---|
| Gastroenterology & Hepatology | 1,343 |
| Respiratory Medicine | 1,098 |
| Dermatology | 852 |
| Cardiology & Vascular Medicine | 772 |
| Rheumatology & Musculoskeletal | 624 |
| Trauma & Emergency Medicine | 621 |
| Paediatrics & Neonatology | 577 |
| Endocrinology & Metabolism | 539 |
| Psychiatry & Mental Health | 526 |
| Other Basic Sciences | 510 |
| Ophthalmology | 504 |
| Obstetrics & Gynaecology | 434 |
| Infectious Diseases & Tropical Medicine | 425 |
| Nephrology & Urology | 207 |
| ENT & Oral Health | 204 |
| Other Clinical | 182 |
| Basic Paediatrics | 106 |
| Neurology | 82 |
| Allergy & Immunology | 76 |
| Basic Psychiatry | 60 |
| Forensic Medicine | 50 |
| Basic Obstetrics & Gynaecology | 49 |
| Basic Ophthalmology | 40 |
| Geriatrics & Palliative Care | 37 |
| Pharmacology | 37 |
| Oncology & Haematology | 35 |
| Epidemiology & Public Health | 34 |
| Basic ENT | 32 |
| Genetics & Molecular Biology | 27 |
| Microbiology & Immunology | 27 |
| Pathology | 22 |
| Basic Orthopaedics | 21 |
| Basic Neuroscience | 19 |
| Basic Surgery | 17 |
| Physiology | 17 |
| Radiology & Imaging | 17 |
| Anaesthesiology | 16 |
| Biochemistry | 16 |
| Anatomy | 15 |

</details>

## Sources and attribution

This dataset is a derivative work. Please cite the original authors in addition to this dataset.

- **HuatuoGPT-o1 / medical-o1-reasoning-SFT** (Apache-2.0). The reasoning traces were generated with GPT-4o and checked by a medical verifier in the original work.
  Chen, J., Cai, Z., Ji, K., Wang, X., Liu, W., Wang, R., Hou, J., Wang, B. *HuatuoGPT-o1, Towards Medical Complex Reasoning with LLMs.* arXiv:2412.18925, 2024. <https://huggingface.co/datasets/FreedomIntelligence/medical-o1-reasoning-SFT>
- **MedReason** (Apache-2.0). Wu, J. et al. *MedReason: Eliciting Factual Medical Reasoning Steps in LLMs via Knowledge Graphs.* arXiv:2504.00993, 2025. <https://huggingface.co/datasets/UCSC-VLAA/MedReason>
  MedReason itself aggregates questions from earlier medical QA benchmarks (e.g. MedQA, MedMCQA, PubMedQA); see its dataset card for their citations.
- **doctor_qa_bangla**. <https://huggingface.co/datasets/shetumohanto/doctor_qa_bangla>
  The upstream page is tagged Apache-2.0 but its dataset card is empty, so the original provenance of these doctor Q&A pairs could not be verified. [EDIT: add the original origin if you learn it.]
- **Translation model:** Gemma 4 (Google DeepMind), released under Apache-2.0.

Changes made: translation to Bengali, specialty classification, filtering, de-duplication, schema restructuring (CoT split into `reasoning` and `answer`).

## Intended use

Research and development on Bengali medical NLP: fine-tuning and evaluating language models, reasoning distillation, low-resource language studies.

## Limitations and warnings

- **Not medical advice.** Do not use this dataset or models trained on it for real clinical decisions without clinician oversight.
- **Machine translation.** Text was translated by an LLM and not fully reviewed by medical professionals. Terminology errors, mistranslated numbers or units, and awkward phrasing can remain. [EDIT: state how much, if any, was human-reviewed.]
- **Synthetic reasoning.** The `cot` traces were produced by LLMs in the original datasets and then translated; they may contain medical errors.
- **Origin bias.** Most content derives from US/Chinese exam-style or case-style questions; disease prevalence, drugs, guidelines and care settings may not match Bangladesh.
- **Format artifacts.** Some `non_cot` MedReason items are fill-in-the-blank style stems, and some answers contain the original explanation and textbook reference text.
- Some questions appear in both HuatuoGPT-o1 and MedReason-derived rows with different answers or formats.

## Citation

```bibtex
@misc{arittra2026banglamedicalqa,
  title        = {Bangla Medical QA: CoT and Non-CoT},
  author       = {Arittra},
  year         = {2026},
  howpublished = {\url{https://huggingface.co/datasets/Arittra/bangla_medical_qa_cot_and_non_cot}}
}
```

Please also cite HuatuoGPT-o1 and MedReason (see [Sources and attribution](#sources-and-attribution)).

## License

Released under **Apache-2.0**, consistent with the upstream licenses listed above. Verify the license and provenance of any component you add.
