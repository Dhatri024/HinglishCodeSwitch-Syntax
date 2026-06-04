# METHODOLOGY

# HinglishCodeSwitch-Syntax (HCSS) v1.4.2-MLF

### Methodology and Dataset Generation Process

---

# 1. Introduction

The HinglishCodeSwitch-Syntax (HCSS) dataset is a large-scale synthetic multilingual conversational corpus designed to support research in:

* Code-switching detection
* Multilingual NLP
* Conversational AI
* Dialogue systems
* Sentiment analysis
* Sociolinguistics
* Language modeling
* Behavioral language analytics

The dataset contains **1,000,000 conversational records** representing realistic Hinglish (Hindi-English code-switched) interactions across multiple domains, social registers, emotional tones, and conversational states.

The generation process was designed to simulate natural language variation while maintaining statistical consistency and schema integrity.

---

# 2. Design Principles

The dataset was built according to five primary principles:

## Realistic Linguistic Variation

Conversations emulate common Hinglish communication patterns observed in digital messaging environments.

## Controlled Statistical Distributions

Feature distributions were generated using probabilistic models to maintain consistency across the corpus.

## Multidomain Coverage

Records span multiple conversational domains to improve generalization across NLP tasks.

## Zero-Null Architecture

Every record satisfies strict schema validation requirements and contains complete information.

## Reproducibility

The generation process follows deterministic schema rules and quality-control procedures to ensure reproducibility.

---

# 3. Dataset Generation Pipeline

The HCSS generation workflow consists of eight stages.

```text
Schema Design
      ↓
Metadata Generation
      ↓
Conversation Context Generation
      ↓
Dialogue State Simulation
      ↓
Code-Switch Construction
      ↓
Feature Computation
      ↓
Payload Assembly
      ↓
Quality Validation
```

---

# 4. Metadata Generation

Each record begins with metadata synthesis.

Generated attributes include:

* Record identifier
* Session identifier
* Timestamp
* Geographic region
* Age group
* Device type
* Platform channel

Category frequencies were sampled according to predefined probability distributions to emulate realistic population diversity.

Example:

| Attribute        | Example            |
| ---------------- | ------------------ |
| geo_state        | Telangana          |
| age_group        | 25-34              |
| device_type      | android_smartphone |
| platform_channel | WhatsApp           |

---

# 5. Conversation Context Generation

A conversational environment is generated for each session.

The following contextual attributes are sampled:

* Domain context
* Social register
* Emotional tone
* Language dominance
* Script modality

These variables influence subsequent linguistic and behavioral features.

Example:

```text
Domain: software_engineering
Register: semi_formal
Tone: urgent
Language Dominance: balanced_mixed
Script: Roman_transliterated
```

---

# 6. Dialogue State Modeling

Conversation progression is modeled using a finite-state conversational framework.

Possible states include:

* INIT_GREETING
* PROBLEM_STATEMENT
* CLARIFICATION_REQUEST
* INFORMATION_EXCHANGE
* NEGOTIATION
* TASK_DELEGATION
* CONFIRMATION_SEEK
* AFFIRMATIVE_CLOSE
* NEGATIVE_ESCALATION
* TERMINAL_RESOLUTION

State transitions are conditioned by:

* Emotional tone
* Stress index
* Turn depth
* Domain context

This enables realistic dialogue progression across multiple turns.

---

# 7. Code-Switching Generation

The dataset follows the Matrix Language Frame (MLF) model.

Three language dominance configurations are supported:

| Class            | Description                                 |
| ---------------- | ------------------------------------------- |
| hindi_dominant   | Hindi supplies most grammatical structure   |
| balanced_mixed   | Both languages contribute equally           |
| english_dominant | English supplies most grammatical structure |

Generated utterances may contain:

* Intra-sentential switching
* Inter-sentential switching
* Lexical insertions
* Embedded language islands
* Congruent lexicalization

Example:

```text
Kal deployment complete karna hai before client review.
```

---

# 8. Conversational Payload Construction

Each record contains a structured multi-turn conversation serialized into JSON format.

Example:

```json
{
  "turns": [
    {
      "turn": 1,
      "role": "USER",
      "utterance": "PR review kar diya kya?"
    },
    {
      "turn": 2,
      "role": "AGENT",
      "utterance": "Abhi start kar raha hoon."
    }
  ]
}
```

The payload serves as the primary source for:

* Dialogue modeling
* Sequence learning
* Conversation reconstruction
* Language modeling

---

# 9. Feature Generation

Linguistic, sentiment, and behavioral variables are generated after dialogue construction.

## Linguistic Features

Examples:

* cs_ratio_hindi
* cs_ratio_english
* insertion_rate
* alternation_rate
* morpheme_binding_score

## Lexical Features

Examples:

* lexical_density
* syntax_fluency_index
* token_count
* oov_token_rate

## Sentiment Features

Examples:

* sentiment_valence
* sentiment_arousal
* emotional_tone
* politeness_score

## Behavioral Features

Examples:

* response_latency_ms
* typing_speed_wpm
* edit_distance_ratio

Feature values are sampled from predefined statistical distributions and validated against schema constraints.

---

# 10. Quality Assurance Pipeline

Every generated record passes four validation stages.

## Stage 1 — Schema Validation

Checks:

* Required fields present
* Correct data types
* Enum compliance

## Stage 2 — Range Validation

Checks:

* Numerical bounds
* Category membership
* Timestamp validity

## Stage 3 — Linguistic Validation

Checks:

* Language ratio consistency
* Morpheme count validity
* Dialogue structure integrity

## Stage 4 — JSON Validation

Checks:

* Valid JSON structure
* Turn ordering
* Required payload fields

Records failing validation are regenerated.

---

# 11. Dataset Integrity Guarantees

The final release guarantees:

* 1,000,000 valid records
* 44 documented columns
* UTF-8 encoding
* Zero-null policy
* Structured dialogue payloads
* Consistent categorical distributions
* Schema-compliant records

---

# 12. Intended Research Applications

The dataset is designed for:

* Code-switching analysis
* Matrix language identification
* Dialogue state classification
* Conversational AI training
* Sentiment analysis
* Emotion recognition
* Language modeling
* Behavioral analytics
* Reinforcement learning for dialogue systems
* Sociolinguistic research

---

# 13. Ethical Considerations

HCSS is a synthetic dataset.

No real user conversations are included.

No personally identifiable information (PII) is present.

All conversational content, metadata, and behavioral attributes are algorithmically generated for research and educational purposes.

---

# 14. Reproducibility

The generation pipeline follows deterministic schema definitions and validation rules.

Researchers may reproduce similar datasets by implementing:

1. Metadata synthesis
2. Context generation
3. Dialogue state simulation
4. Code-switch construction
5. Feature generation
6. Validation procedures

under the schema specifications documented in this repository.

---

# Conclusion

HCSS v1.4.2-MLF provides a large-scale synthetic Hinglish conversational corpus designed to support modern multilingual NLP research. The methodology emphasizes realism, diversity, reproducibility, and schema integrity while enabling experimentation across a broad range of conversational AI and linguistic analysis tasks.
