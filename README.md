# HinglishCodeSwitch-Syntax

## A High-Fidelity Hinglish Code-Switching Conversational Dataset

**Schema Version:** v1.4.2-MLF
**Records:** 1,000,000
**Features:** 45 Columns
**Format:** Apache Parquet
**License:** CC BY 4.0

---

# Overview

HinglishCodeSwitch-Syntax is a large-scale conversational dataset designed for research and development in:

* Code-Switching NLP
* Multilingual Language Modeling
* Conversational AI
* Dialogue Systems
* Sociolinguistics
* Sentiment Analysis
* Text Classification
* Code-Mixed Language Understanding

The dataset captures conversational patterns where Hindi and English are naturally mixed within the same utterance, following the principles of the Matrix Language Frame (MLF) model.

The corpus contains rich linguistic, conversational, behavioral, and contextual features suitable for training and evaluating modern multilingual AI systems.

---

# Dataset Statistics

| Property       | Value                                   |
| -------------- | --------------------------------------- |
| Total Records  | 1,000,000                               |
| Total Features | 45                                      |
| Format         | Apache Parquet                          |
| Compression    | Snappy                                  |
| Schema Version | v1.4.2-MLF                              |
| Language Type  | Hinglish (Hindi-English Code Switching) |
| Data Type      | Conversational                          |
| Storage Format | Columnar                                |
| License        | CC BY 4.0                               |

---

# Project Structure

```text
HinglishCodeSwitch-Syntax/
│
├── data/
│   ├── README.md
│
├── docs/
│   ├── data_dictionary.md
│   ├── dataset_card.md
│   ├── methodology.md
│
├── notebooks/
│   ├── generate_dataset.ipynb
│   ├── generate_dataset.py
│   ├── validation.ipynb
│   └── validation.py
│
├── plots/
│   ├── language_distribution.png
│   ├── sentiment_distribution.png
│   ├── code_switch_ratio.png
│   └── feature_correlations.png
│
├── LICENSE
└── README.md
```

---

# Dataset Access

The complete dataset is hosted on Hugging Face because the full Parquet files exceed GitHub's recommended repository size limits.

## Hugging Face Dataset

Replace the link below with your final dataset URL:

```text
https://huggingface.co/datasets/dhatri-02/HCSS-v1.4.2-MLF
```

Dataset files can be downloaded directly from Hugging Face.

---

# Schema Overview

The dataset contains 45 structured features grouped into the following categories:

### Conversational Metadata

* record_id
* session_id
* timestamp_utc
* turn_number
* turn_role
* conversation_state

### User & Context Information

* age_group
* geo_state
* device_type
* platform_channel
* domain_context
* social_register

### Language Features

* lang_dominance
* script_modality
* cs_ratio_hindi
* cs_ratio_english
* insertion_rate
* alternation_rate
* morpheme_binding_score

### Linguistic Metrics

* lexical_density
* syntax_fluency_index
* pragmatic_coherence
* token_count
* oov_token_rate

### Sentiment & Emotion

* emotional_tone
* sentiment_valence
* sentiment_arousal

### Behavioral Features

* response_latency_ms
* typing_speed_wpm
* edit_distance_ratio

### Language Model Metrics

* perplexity_score
* language_model_perplexity

### Dialogue Payload

* dialogue_json

A complete column-by-column description is available in:

```text
docs/data_dictionary.md
```

---

# Methodology

The dataset generation process includes:

1. Conversational template generation
2. Hinglish code-switching synthesis
3. Feature generation
4. Dialogue construction
5. Statistical validation
6. Schema verification
7. Export to Apache Parquet

Detailed documentation:

```text
docs/methodology.md
```

---

# Example Record

```json
{
  "record_id": "HCSS_00000001",
  "lang_dominance": "hindi_dominant",
  "social_register": "casual_peer",
  "emotional_tone": "friendly",
  "cs_ratio_hindi": 0.64,
  "cs_ratio_english": 0.36,
  "dialogue_json": {
    "turns": [
      {
        "turn": 1,
        "role": "USER",
        "utterance": "Yaar meeting kab start hogi?"
      },
      {
        "turn": 2,
        "role": "AGENT",
        "utterance": "Meeting 10 minutes mein start hogi."
      }
    ]
  }
}
```

---

# Validation

Validation notebooks are provided for:

* Schema validation
* Data quality checks
* Missing value detection
* Distribution verification
* Statistical consistency checks

---

# Visualizations

The repository includes exploratory visualizations for:

* Language dominance distribution
* Sentiment distribution
* Code-switching ratio analysis
* Feature correlation analysis

Located in:

```text
plots/
```

---

# Installation

Clone the repository:

```bash
git clone https://github.com/Dhatri024/HinglishCodeSwitch-Syntax.git

cd HinglishCodeSwitch-Syntax
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# Loading the Dataset

```python
import pandas as pd

df = pd.read_parquet(
    "HinglishCodeSwitch_Syntax.parquet"
)

print(df.head())
```

---

# Research Applications

This dataset can be used for:

* Code-Switching Detection
* Multilingual NLP
* Conversational AI
* Dialogue State Modeling
* Sentiment Analysis
* Language Identification
* Sociolinguistic Research
* LLM Fine-Tuning
* Retrieval-Augmented Generation (RAG)
* Benchmark Creation

---

# Documentation

| File                    | Description                |
| ----------------------- | -------------------------- |
| docs/data_dictionary.md | Complete schema reference  |
| docs/dataset_card.md    | Dataset card               |
| docs/methodology.md     | Generation methodology     |
| data/README.md          | Dataset access information |

---

# License

This project is released under the CC BY 4.0 License.

See:

```text
LICENSE
```

for full details.

---

# Citation

```bibtex
@dataset{hinglish_codeswitch_syntax,
  title={HinglishCodeSwitch-Syntax},
  author={Your Name},
  year={2026},
  version={v1.4.2-MLF},
  publisher={Hugging Face},
  url={https://huggingface.co/datasets/<username>/HinglishCodeSwitch-Syntax}
}
```

---

## Acknowledgements

This project was created to support research and development in multilingual conversational AI and code-switching language technologies.
