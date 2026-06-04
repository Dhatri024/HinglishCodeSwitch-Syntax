# DATASET_CARD
# HinglishCodeSwitch-Syntax

### Large-Scale Hinglish Code-Switching Dataset for Multilingual NLP and Conversational Language Analysis

![Version](https://img.shields.io/badge/Version-1.0.0-blue?style=for-the-badge)
![Rows](https://img.shields.io/badge/Rows-1M-orange?style=for-the-badge)
![Format](https://img.shields.io/badge/Format-Parquet-yellow?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-Hinglish-green?style=for-the-badge)

---

## Dataset Identity

| Attribute          | Value                                   |
| ------------------ | --------------------------------------- |
| Dataset Name       | HinglishCodeSwitch-Syntax               |
| Version            | 1.0.0                                   |
| Dataset Type       | Synthetic Conversational NLP Dataset    |
| Domain             | Multilingual Conversational Language    |
| Primary Language   | Hinglish (Hindi-English Code Switching) |
| Storage Format     | Apache Parquet                          |
| Schema Format      | JSON                                    |
| Total Records      | 1,000,000                               |
| Dataset Splits     | Train / Validation / Test               |
| Character Encoding | UTF-8                                   |

---

## Dataset Description

HinglishCodeSwitch-Syntax is a large-scale synthetic dataset designed to model conversational language where Hindi and English are mixed within the same utterance. The dataset captures a broad range of linguistic, conversational, emotional, and behavioral attributes commonly observed in code-switched communication.

The primary objective of the dataset is to support research and experimentation in multilingual natural language processing, conversational AI systems, language understanding, code-switching analysis, and feature-driven machine learning workflows.

Each record represents a conversational observation enriched with structured metadata and linguistic measurements. The dataset combines categorical, numerical, temporal, and language-related features to provide a rich environment for experimentation and benchmarking.

The dataset has been released together with predefined train, validation, and test partitions, schema definitions, exploratory visualizations, and detailed documentation to facilitate reproducible research workflows.

---

## What the Dataset Contains

The dataset includes information related to:

### Language Characteristics

* Hindi language usage ratio
* English language usage ratio
* Language dominance indicators
* Code-switching intensity measurements
* Morpheme binding metrics
* Mixed-language conversational patterns

### Conversational Features

* Conversation states
* Turn-level interaction information
* Dialogue-related metadata
* Contextual conversation attributes
* Session-level identifiers

### Linguistic Quality Features

* Syntax fluency indicators
* Lexical measurements
* Token statistics
* Language complexity indicators
* Structural language properties

### Emotional and Behavioral Signals

* Emotional tone categories
* Sentiment-related attributes
* Response behavior metrics
* Interaction dynamics
* Communication style indicators

### Metadata Attributes

* Temporal information
* Geographic attributes
* Domain-specific categories
* Platform-related metadata
* Dataset versioning information

---

## Feature Overview

The dataset contains a diverse collection of feature types:

| Feature Category        | Description                                                   |
| ----------------------- | ------------------------------------------------------------- |
| Identifiers             | Unique record and session identifiers                         |
| Temporal Features       | Time-related attributes and conversation timing information   |
| Language Features       | Hindi-English usage statistics and language dominance metrics |
| Conversational Features | Dialogue state and interaction-level information              |
| Linguistic Features     | Syntax, lexical, and structural language measurements         |
| Sentiment Features      | Emotional and affect-related attributes                       |
| Behavioral Features     | Response and interaction characteristics                      |
| Metadata Features       | Contextual and categorical information                        |

---

## Dataset Files

### Raw Dataset

| File                                       |
| ------------------------------------------ |
| `HinglishCodeSwitch_Syntax_v1_raw.parquet` |

Contains the complete dataset before train-validation-test partitioning.

### Dataset Splits

| Split      | File                                   |
| ---------- | -------------------------------------- |
| Train      | `hinglish_codeswitch_v1_train.parquet` |
| Validation | `hinglish_codeswitch_v1_val.parquet`   |
| Test       | `hinglish_codeswitch_v1_test.parquet`  |

These files provide ready-to-use partitions for machine learning experiments and model evaluation.

---

## Included Documentation

The repository includes comprehensive supporting documentation:

| File                                 | Purpose                                |
| ------------------------------------ | -------------------------------------- |
| `dataset_card.md`                    | Dataset overview and usage information |
| `data_dictionary.md`                 | Detailed feature descriptions          |
| `methodology.md`                     | Dataset creation workflow              |
| `limitations.md`                     | Known limitations and considerations   |
| `hinglish_codeswitch_v1_schema.json` | Dataset schema specification           |

---

## Exploratory Data Analysis Resources

The repository includes visualization assets generated during dataset analysis.

Available visualizations include:

* Language distribution
* Domain distribution
* Emotional tone distribution
* Geographic distribution
* Conversation state distribution
* Device distribution
* Morpheme binding analysis
* Syntax fluency analysis
* Token distribution analysis
* Response behavior analysis
* Feature correlation matrix
* Additional descriptive statistics plots

These visualizations provide an overview of dataset characteristics and support exploratory data analysis.

---

## Repository Structure

```text
HinglishCodeSwitch-Syntax/
│
├── Data/
│   ├── Raw/
│   │   └── HinglishCodeSwitch_Syntax_v1_raw.parquet
│   │
│   └── Splits/
│       ├── hinglish_codeswitch_v1_train.parquet
│       ├── hinglish_codeswitch_v1_val.parquet
│       └── hinglish_codeswitch_v1_test.parquet
│
├── Docs/
│   ├── data_dictionary.md
│   ├── dataset_card.md
│   ├── limitations.md
│   └── methodology.md
│
├── Plots/
│   ├── conversation_distribution.png
│   ├── device_distribution.png
│   ├── domain_distribution.png
│   ├── emotional_distribution.png
│   ├── feature_correlation_matrix.png
│   ├── hindi_code_switch.png
│   ├── language_distribution.png
│   ├── morpheme_binding.png
│   ├── response_distribution.png
│   ├── syntax_fluency.png
│   ├── token_distribution.png
│   └── top_geo_states.png
│
├── Schema/
│   └── hinglish_codeswitch_v1_schema.json
│
├── notebooks/
│   ├── Hinglish.ipynb
│   └── hinglish.py
│
├── LICENSE
├── README.md
├── .gitattributes
└── .gitignore
```

---

## Data Generation Summary

The dataset was generated through a structured synthetic data generation pipeline designed to create diverse Hinglish conversational records.

The workflow included:

1. Dataset schema design
2. Feature generation and validation
3. Conversational attribute generation
4. Language distribution modeling
5. Feature consistency verification
6. Train-validation-test partitioning
7. Exploratory data analysis
8. Documentation and schema creation

The resulting dataset provides a large-scale resource suitable for experimentation and benchmarking across a variety of NLP and machine learning tasks.

---

## Potential Research Applications

The dataset may be used for:

### Natural Language Processing

* Code-switching analysis
* Language identification
* Text classification
* Feature engineering
* Conversational language modeling

### Conversational AI

* Dialogue understanding
* Conversation state prediction
* Chatbot experimentation
* Response behavior analysis

### Machine Learning

* Supervised learning
* Classification tasks
* Regression tasks
* Feature importance analysis
* Model benchmarking

### Data Science

* Exploratory data analysis
* Statistical modeling
* Correlation analysis
* Distribution analysis
* Synthetic data research

---

## Limitations

* The dataset is synthetic and does not represent real user conversations.
* The generated patterns are designed for experimentation and benchmarking purposes.
* Real-world language behavior may exhibit characteristics not captured within the dataset.
* Models trained exclusively on this dataset should be evaluated on additional datasets before deployment in production environments.

---

## License

Refer to the repository license file for usage rights and distribution terms.

---

## Citation

```bibtex
@dataset{hinglishcodeswitchsyntax,
  title={HinglishCodeSwitch-Syntax},
  author={Mididuddi, Dhatri},
  year={2026},
  version={1.0.0}
}
```

---

## Contact

For questions, bug reports, or contributions, please use the GitHub repository issue tracker.
