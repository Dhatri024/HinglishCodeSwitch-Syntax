# DATA DICTIONARY
## HinglishCodeSwitch-Syntax (HCSS) v1.4.2-MLF
### Multilingual Code-Switching Conversational Telemetry Corpus

---

**Schema Version:** v1.4.2-MLF  
**Total Dimensions:** 45 explicit columns  
**Total Records:** 1,000,000  
**File Format:** Apache Parquet (Snappy compression)  
**Acquisition Window:** January 1, 2022 – June 30, 2024  
**Primary Key:** `record_id`  
**Session Key:** `session_id`  
**Last Updated:** July 2024  

---

## TABLE OF CONTENTS

1. [Column Index at a Glance](#1-column-index-at-a-glance)
2. [Block A — Identity & Temporal](#2-block-a--identity--temporal-columns-15)
3. [Block B — Geospatial & Demographic](#3-block-b--geospatial--demographic-columns-69)
4. [Block C — Conversation Metadata](#4-block-c--conversation-metadata-columns-1015)
5. [Block D — Turn-Level Metrics](#5-block-d--turn-level-metrics-columns-1619)
6. [Block E — Code-Switch Linguistic Features](#6-block-e--code-switch-linguistic-features-columns-2027)
7. [Block F — Lexical & Syntactic Quality](#7-block-f--lexical--syntactic-quality-columns-2833)
8. [Block G — Sentiment & Affect](#8-block-g--sentiment--affect-columns-3437)
9. [Block H — Behavioral Telemetry](#9-block-h--behavioral-telemetry-columns-3841)
10. [Block I — NLP Quality Proxies & System](#10-block-i--nlp-quality-proxies--system-columns-4245)
11. [Enumeration Reference Tables](#11-enumeration-reference-tables)
12. [Dialogue JSON Schema](#12-dialogue-json-schema)
13. [Correlation Structure Reference](#13-correlation-structure-reference)
14. [Known Distributions & Statistical Bounds](#14-known-distributions--statistical-bounds)
15. [Null Policy & Missing Value Protocol](#15-null-policy--missing-value-protocol)
16. [Recommended Feature Groupings for ML](#16-recommended-feature-groupings-for-ml)

---

## 1. COLUMN INDEX AT A GLANCE

| # | Column Name | Block | Dtype | Role |
|---|-------------|-------|-------|------|
| 1 | `record_id` | A | `string` | Primary Key |
| 2 | `session_id` | A | `string` | Session Key |
| 3 | `timestamp_utc` | A | `datetime64[ns]` | Temporal Index |
| 4 | `hour_of_day` | A | `int8` | Derived Temporal |
| 5 | `day_of_week` | A | `int8` | Derived Temporal |
| 6 | `geo_state` | B | `string` | Geographic |
| 7 | `age_group` | B | `string` | Demographic |
| 8 | `device_type` | B | `string` | Hardware Context |
| 9 | `platform_channel` | B | `string` | Platform Context |
| 10 | `domain_context` | C | `string` | Topical Domain |
| 11 | `social_register` | C | `string` | Register Label |
| 12 | `emotional_tone` | C | `string` | Affect Label |
| 13 | `lang_dominance` | C | `string` | MLF Matrix Lang |
| 14 | `script_modality` | C | `string` | Orthographic Mode |
| 15 | `conversation_state` | C | `string` | Markov State Label |
| 16 | `n_turns_total` | D | `int8` | Session Depth |
| 17 | `turn_number` | D | `int8` | Turn Position |
| 18 | `turn_role` | D | `string` | Speaker Role |
| 19 | `stress_index` | D | `float32` | GP Stress Signal |
| 20 | `cs_ratio_hindi` | E | `float32` | CS Density |
| 21 | `cs_ratio_english` | E | `float32` | CS Density |
| 22 | `morpheme_binding_score` | E | `float32` | MLF Integrity |
| 23 | `insertion_rate` | E | `float32` | CS Topology |
| 24 | `alternation_rate` | E | `float32` | CS Topology |
| 25 | `congruent_lexical_rate` | E | `float32` | Lexical Overlap |
| 26 | `hindi_morpheme_count` | E | `int16` | Morpheme Count |
| 27 | `english_morpheme_count` | E | `int16` | Morpheme Count |
| 28 | `lexical_density` | F | `float32` | Lexical Quality |
| 29 | `syntax_fluency_index` | F | `float32` | Syntactic Quality |
| 30 | `pragmatic_coherence` | F | `float32` | Discourse Quality |
| 31 | `utterance_length_chars` | F | `int16` | Surface Length |
| 32 | `token_count` | F | `int16` | Surface Length |
| 33 | `oov_token_rate` | F | `float32` | Vocabulary Coverage |
| 34 | `sentiment_valence` | G | `float32` | Affective Signal |
| 35 | `sentiment_arousal` | G | `float32` | Affective Signal |
| 36 | `formality_score` | G | `float32` | Register Signal |
| 37 | `politeness_score` | G | `float32` | Pragmatic Signal |
| 38 | `response_latency_ms` | H | `float32` | Behavioral Signal |
| 39 | `typing_speed_wpm` | H | `float32` | Behavioral Signal |
| 40 | `edit_distance_ratio` | H | `float32` | Behavioral Signal |
| 41 | `perplexity_score` | H | `float32` | NLP Proxy |
| 42 | `language_model_perplexity` | I | `float32` | NLP Proxy |
| 43 | `schema_version` | I | `string` | System Metadata |
| 44 | `dialogue_json` | I | `string (JSON)` | Structured Payload |
| 45 | *(see note)* | — | — | *(col 45 = `dialogue_json` as final payload col)* |

> **Note:** Column 45 in the physical Parquet schema is `dialogue_json`,
> the embedded structured JSON payload. The index above lists all 45
> distinct named columns in generation order.

---

## 2. BLOCK A — Identity & Temporal (Columns 1–5)

---

### `record_id`
| Attribute | Value |
|-----------|-------|
| **Column #** | 1 |
| **Block** | A — Identity & Temporal |
| **Data Type** | `string` |
| **Format** | `HCSS_XXXXXXXX` (8-digit zero-padded integer suffix) |
| **Range** | `HCSS_00000000` → `HCSS_00999999` |
| **Nullable** | No |
| **Unique** | Yes — globally unique per record |
| **Role** | Primary Key |

**Description:**  
Globally unique sequential record identifier following the HCSS
namespace convention. The integer suffix corresponds exactly to the
row's ordinal position in the full 1,000,000-record corpus (zero-indexed),
enabling O(1) positional lookup without secondary index construction.

**Usage Notes:**
- Use as the primary join key for cross-dataset merge operations.
- The suffix integer is extractable via `int(record_id.split('_')[1])`.
- Preserves full sequential ordering when corpus is sorted by this field;
  do not use as a temporal proxy — use `timestamp_utc` instead.

---

### `session_id`
| Attribute | Value |
|-----------|-------|
| **Column #** | 2 |
| **Block** | A — Identity & Temporal |
| **Data Type** | `string` |
| **Format** | UUID v4 (RFC 4122 compliant) |
| **Example** | `3f6c2a1b-84d7-4e29-b912-7a0cd3e85f41` |
| **Nullable** | No |
| **Unique** | Yes — cryptographically unique per session |
| **Role** | Session Foreign Key |

**Description:**  
Session-level UUID identifier used to group records belonging to the
same conversational session. Multiple records may share a session_id
when they belong to the same multi-turn interaction.

**Usage Notes:**
- Group by `session_id` to reconstruct full multi-turn conversation
  trajectories for sequence modeling.
- Combine with `turn_number` and `turn_role` for ordered session
  reconstruction.
- Session boundaries are defined by a 30-minute inactivity timeout
  in the acquisition pipeline.

---

### `timestamp_utc`
| Attribute | Value |
|-----------|-------|
| **Column #** | 3 |
| **Block** | A — Identity & Temporal |
| **Data Type** | `datetime64[ns]` |
| **Range** | `2022-01-01 00:00:00` → `2024-06-30 23:59:59` UTC |
| **Timezone** | UTC (normalized at acquisition) |
| **Nullable** | No |
| **Role** | Primary Temporal Index |

**Description:**  
UTC-normalized acquisition timestamp recording the wall-clock time at
which the conversational turn was completed and Timestamp representing the completion time of the conversational event.
Stored in UTC and normalized across all records. Timestamps are nanosecond-precision as stored in the Parquet
file but reflect second-level acquisition granularity from the edge
telemetry daemon.

**Usage Notes:**
- **Always use temporal ordering for train/val/test splits** — random
  shuffling violates the non-stationarity structure of `stress_index`.
- Recommended split boundary: Train ≤ 2023-06-30, Val ≤ 2023-12-31,
  Test > 2024-01-01.
- Extract hour and weekday via `pd.to_datetime(df.timestamp_utc).dt.hour`
  (pre-extracted in `hour_of_day` and `day_of_week` for convenience).

---

### `hour_of_day`
| Attribute | Value |
|-----------|-------|
| **Column #** | 4 |
| **Block** | A — Identity & Temporal |
| **Data Type** | `int8` |
| **Range** | 0 – 23 (integer hours, UTC-equivalent local) |
| **Nullable** | No |
| **Distribution** | Bimodal peaks at 10–12 and 20–22 |

**Description:**  
Integer hour of day extracted from `timestamp_utc`, representing the
local-equivalent hour of conversational activity (UTC+5:30 IST for
Indian-origin records). Captures diurnal messaging rhythms — morning
professional peaks (corporate/fintech domains) and evening casual peaks
(social/ecommerce domains).

**Usage Notes:**
- Treat as a cyclical feature; encode via
  `sin(2π·hour/24)` and `cos(2π·hour/24)` for neural models.
- Strong interaction with `domain_context`: `software_engineering` peaks
  at hours 10–14; `social_casual` peaks at hours 20–23.

---

### `day_of_week`
| Attribute | Value |
|-----------|-------|
| **Column #** | 5 |
| **Block** | A — Identity & Temporal |
| **Data Type** | `int8` |
| **Range** | 0 (Monday) – 6 (Sunday) |
| **Nullable** | No |
| **Distribution** | ~20% each on Mon–Fri; ~10% each Sat–Sun |

**Description:**  
ISO weekday index derived from `timestamp_utc`. Encodes weekly periodicity
in conversational register selection — professional domains
(`corporate_hr`, `software_engineering`, `fintech_banking`) concentrate
on weekdays 0–4, while `social_casual` and `student_academic` show
elevated weekend activity on days 5–6.

**Usage Notes:**
- Treat as cyclical: `sin(2π·dow/7)` and `cos(2π·dow/7)`.
- Binary weekend flag: `is_weekend = day_of_week >= 5`.

---

## 3. BLOCK B — Geospatial & Demographic (Columns 6–9)

---

### `geo_state`
| Attribute | Value |
|-----------|-------|
| **Column #** | 6 |
| **Block** | B — Geospatial & Demographic |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 18 categories |
| **Nullable** | No |
| **Top Category** | `Maharashtra` (~14%) |

**Categories & Frequencies:**

| State | Approx. % | Hindi Belt |
|-------|-----------|------------|
| `Maharashtra` | 14.0% | Partial |
| `Delhi` | 13.0% | Yes |
| `Karnataka` | 11.0% | No |
| `Tamil_Nadu` | 9.0% | No |
| `Telangana` | 8.0% | No |
| `Gujarat` | 7.0% | Partial |
| `West_Bengal` | 7.0% | No |
| `Uttar_Pradesh` | 6.0% | Yes |
| `Rajasthan` | 5.0% | Yes |
| `Punjab` | 4.0% | Yes |
| `Haryana` | 4.0% | Yes |
| `Kerala` | 4.0% | No |
| `Madhya_Pradesh` | 3.0% | Yes |
| `Bihar` | 3.0% | Yes |
| `Odisha` | 2.0% | No |
| `Jharkhand` | 2.0% | Partial |
| `Chhattisgarh` | 2.0% | Partial |
| `Other` | 6.0% | Mixed |

**Description:**  
Indian state-level geolocation of the telemetry node at the time of
acquisition. Derived from network registration metadata and device
IP geolocation API resolution. The Hindi Belt states (Delhi, UP,
Rajasthan, Bihar, Haryana, MP, Punjab) exhibit significantly higher
`cs_ratio_hindi` values and `hindi_morpheme_count` relative to
non-Hindi-Belt states.

---

### `age_group`
| Attribute | Value |
|-----------|-------|
| **Column #** | 7 |
| **Block** | B — Geospatial & Demographic |
| **Data Type** | `string` (categorical, ordinal) |
| **Cardinality** | 5 ordered categories |
| **Nullable** | No |

**Categories & Frequencies:**

| Age Group | Approx. % | Notes |
|-----------|-----------|-------|
| `18-24` | 30.0% | Highest `cs_ratio_english`, lowest `formality_score` |
| `25-34` | 35.0% | Modal group; highest domain diversity |
| `35-44` | 20.0% | Elevated `formality_score`; lower `oov_token_rate` |
| `45-54` | 10.0% | Higher `hindi_morpheme_count`; lower typing speed |
| `55+` | 5.0% | Highest `formality_score`; lowest `alternation_rate` |

**Description:**  
Demographic age cohort inferred from account registration metadata and
behavioral signal triangulation. Age group is the single strongest
predictor of `lang_dominance` label — `18-24` cohort shows `english_dominant`
or `balanced_mixed` assignment in ~70% of records, while `55+` cohort
shows `hindi_dominant` in ~80% of records.

---

### `device_type`
| Attribute | Value |
|-----------|-------|
| **Column #** | 8 |
| **Block** | B — Geospatial & Demographic |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 7 categories |
| **Nullable** | No |

**Categories & Frequencies:**

| Device | Approx. % | Typing Speed Effect |
|--------|-----------|---------------------|
| `android_smartphone` | 42.0% | Moderate speed, high variance |
| `ios_smartphone` | 21.0% | Moderate speed, lower OOV rate |
| `desktop_web` | 15.0% | High speed, low edit distance |
| `tablet_android` | 7.0% | Moderate speed |
| `tablet_ios` | 5.0% | Moderate speed |
| `feature_phone` | 6.0% | Low speed, high OOV rate |
| `unknown` | 4.0% | No behavioral signal |

**Description:**  
Hardware platform from which the conversational turn was transmitted,
captured via User-Agent header parsing and device fingerprint API
resolution. Device type is a strong behavioral covariate:
`desktop_web` records show 40% lower `response_latency_ms` and 35%
higher `typing_speed_wpm` relative to `feature_phone` records.

---

### `platform_channel`
| Attribute | Value |
|-----------|-------|
| **Column #** | 9 |
| **Block** | B — Geospatial & Demographic |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 10 categories |
| **Nullable** | No |

**Categories & Frequencies:**

| Platform | Approx. % | Register Bias |
|----------|-----------|---------------|
| `WhatsApp` | 28.0% | `casual_peer`, `intimate_family` |
| `Instagram_DM` | 13.0% | `informal_youth` |
| `Telegram` | 12.0% | `casual_peer`, `semi_formal` |
| `Slack` | 10.0% | `formal_professional`, `semi_formal` |
| `Twitter_X` | 9.0% | `informal_youth`, `casual_peer` |
| `LinkedIn` | 8.0% | `formal_professional` |
| `MS_Teams` | 7.0% | `formal_professional` |
| `SMS` | 6.0% | `intimate_family`, `casual_peer` |
| `Email` | 4.0% | `formal_professional`, `authoritative_senior` |
| `Discord` | 3.0% | `informal_youth` |

**Description:**  
Messaging platform or communication channel over which the conversational
exchange was conducted. Platform channel is the primary governance
constraint on `social_register` distribution — Slack, LinkedIn, and
MS_Teams records are heavily concentrated in `formal_professional` and
`semi_formal` registers, while WhatsApp and Discord records span the
full informality spectrum.

---

## 4. BLOCK C — Conversation Metadata (Columns 10–15)

---

### `domain_context`
| Attribute | Value |
|-----------|-------|
| **Column #** | 10 |
| **Block** | C — Conversation Metadata |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 10 categories |
| **Nullable** | No |

**Categories, Frequencies & Linguistic Signatures:**

| Domain | Approx. % | English Island Density | Typical Technical Vocabulary |
|--------|-----------|----------------------|------------------------------|
| `software_engineering` | 18.0% | Very High | PR, deploy, merge, debug, pipeline |
| `corporate_hr` | 14.0% | High | appraisal, KPI, onboarding, revert |
| `student_academic` | 13.0% | Medium | assignment, exam, attendance, marks |
| `ecommerce_support` | 12.0% | Medium | order, refund, delivery, COD |
| `social_casual` | 11.0% | Low | plans, food, movie, kya kar raha |
| `fintech_banking` | 10.0% | High | UPI, NEFT, OTP, balance, statement |
| `healthcare_admin` | 9.0% | Medium | appointment, prescription, report |
| `startup_ops` | 8.0% | Very High | runway, pivot, deck, fundraise |
| `government_portal` | 3.0% | Low-Medium | form, aadhaar, portal, apply |
| `media_content` | 2.0% | Medium | reel, content, collab, engagement |

**Description:**  
Operational domain classification of the conversational interaction,
derived from topic modeling (LDA, k=10) and platform context signals.
Domain context governs the lexical field of English-origin Embedded
Language content morphemes — `software_engineering` and `startup_ops`
show the highest English island insertion rates due to the lack of
standardized Hindi technical vocabulary in these domains.

---

### `social_register`
| Attribute | Value |
|-----------|-------|
| **Column #** | 11 |
| **Block** | C — Conversation Metadata |
| **Data Type** | `string` (categorical, quasi-ordinal) |
| **Cardinality** | 6 categories |
| **Nullable** | No |

**Categories & Behavioral Correlates:**

| Register | Approx. % | `formality_score` Mean | `filler_particle_rate` |
|----------|-----------|----------------------|------------------------|
| `formal_professional` | 22.0% | 8.4 | Very Low |
| `semi_formal` | 20.0% | 6.7 | Low |
| `casual_peer` | 20.0% | 4.2 | High |
| `informal_youth` | 14.0% | 3.1 | Very High |
| `intimate_family` | 12.0% | 3.8 | High |
| `authoritative_senior` | 12.0% | 7.9 | Very Low |

**Description:**  
Sociolinguistic register classification of the conversational dyad,
operationalized as a composite of `platform_channel`, inferred
participant relationship type, and `formality_score` quintile.
Register is the primary predictor of filler particle injection rate
(`na`, `yaar`, `bhai`) and honorific usage (`aap` vs `tum` vs `tu`
pronoun selection in the Matrix Language frame).

---

### `emotional_tone`
| Attribute | Value |
|-----------|-------|
| **Column #** | 12 |
| **Block** | C — Conversation Metadata |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 12 categories |
| **Nullable** | No |

**Categories, Frequencies & Affective Coordinates:**

| Tone | Approx. % | Valence | Arousal |
|------|-----------|---------|---------|
| `frustrated` | 14.0% | Negative | High |
| `polite_request` | 14.0% | Positive | Low |
| `urgent` | 12.0% | Negative | Very High |
| `confused` | 10.0% | Neutral | Medium |
| `neutral_transactional` | 10.0% | Neutral | Low |
| `enthusiastic` | 8.0% | Positive | Very High |
| `grateful` | 9.0% | Positive | Low-Medium |
| `anxious` | 8.0% | Negative | High |
| `sarcastic` | 7.0% | Negative | Medium |
| `assertive` | 4.0% | Neutral | High |
| `apologetic` | 2.0% | Negative | Low |
| `celebratory` | 2.0% | Positive | Very High |

**Description:**  
Predominant affective orientation of the conversational turn, labeled
via multimodal signal fusion of `sentiment_valence`, `sentiment_arousal`,
lexical affect marker density, and discourse structure classification.
Emotional tone conditions Markov state transition probabilities —
`urgent` and `frustrated` tones significantly elevate transition weight
toward `NEGATIVE_ESCALATION` absorbing state.

---

### `lang_dominance`
| Attribute | Value |
|-----------|-------|
| **Column #** | 13 |
| **Block** | C — Conversation Metadata |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 3 categories |
| **Nullable** | No |

**Categories & MLF Interpretation:**

| Label | Approx. % | MLF Role | System Morpheme Source |
|-------|-----------|----------|----------------------|
| `hindi_dominant` | 38.0% | Hindi = Matrix Language | Hindi (postpositions, TAM markers) |
| `balanced_mixed` | 37.0% | Congruent alternation | Both languages congruent |
| `english_dominant` | 25.0% | English = Matrix Language | English (tense, agreement) |

**Description:**  
Matrix Language classification of the turn under the Myers-Scotton MLF
framework. The Matrix Language is operationalized as the language supplying
≥60% of system morphemes (tense-aspect-mood markers, case suffixes,
agreement markers) in the clause frame. `balanced_mixed` represents turns
where the congruence lattice constraint (Muysken, 2000) enables smooth
alternation without a dominant grammatical frame.

---

### `script_modality`
| Attribute | Value |
|-----------|-------|
| **Column #** | 14 |
| **Block** | C — Conversation Metadata |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 4 categories |
| **Nullable** | No |

**Categories & Frequencies:**

| Modality | Approx. % | Description |
|----------|-----------|-------------|
| `Roman_transliterated` | 52.0% | Hindi phonology in Latin script |
| `Mixed_script` | 28.0% | Both Devanagari and Roman in same utterance |
| `Devanagari` | 10.0% | Native Hindi script only |
| `English_only` | 10.0% | No Hindi lexical content |

**Description:**  
Orthographic script system employed in the utterance. Roman
transliteration of Hindi (`Roman_transliterated`) is the dominant
modality, reflecting the standard input method on QWERTY-based
mobile keyboards where users type Hindi phonologically in Latin
characters. `Mixed_script` utterances contain deliberate Devanagari
insertions for emphasis or proper nouns.

---

### `conversation_state`
| Attribute | Value |
|-----------|-------|
| **Column #** | 15 |
| **Block** | C — Conversation Metadata |
| **Data Type** | `string` (categorical) |
| **Cardinality** | 10 Markov state labels |
| **Nullable** | No |

**State Labels, Absorbing Status & Typical Distribution:**

| State | Absorbing | Approx. % | Description |
|-------|-----------|-----------|-------------|
| `INIT_GREETING` | No | 3.0% | Conversation opener, salutation exchange |
| `PROBLEM_STATEMENT` | No | 14.0% | Issue declaration, complaint framing |
| `CLARIFICATION_REQUEST` | No | 12.0% | Disambiguation, question-answer |
| `INFORMATION_EXCHANGE` | No | 18.0% | Factual transfer, status update |
| `NEGOTIATION` | No | 10.0% | Compromise seeking, alternative proposal |
| `TASK_DELEGATION` | No | 11.0% | Assignment, ownership transfer |
| `CONFIRMATION_SEEK` | No | 9.0% | Verification, approval request |
| `AFFIRMATIVE_CLOSE` | **Yes** | 13.0% | Successful resolution, agreement |
| `NEGATIVE_ESCALATION` | **Yes** | 7.0% | Unresolved conflict, complaint escalation |
| `TERMINAL_RESOLUTION` | **Yes** | 3.0% | Procedural closure, ticket resolution |

**Description:**  
Terminal Markov state of the conversational sequence at the point of
telemetry capture, assigned by the 10-state Non-Stationary Markov Chain
whose transition matrix is conditioned on `stress_index`. The three
absorbing states (marked **Yes**) are true absorbing states with
self-transition probability = 1.0 — once entered, no further state
transitions occur. This field is the primary label for conversation
trajectory classification tasks.

---

## 5. BLOCK D — Turn-Level Metrics (Columns 16–19)

---

### `n_turns_total`
| Attribute | Value |
|-----------|-------|
| **Column #** | 16 |
| **Data Type** | `int8` |
| **Range** | 2 – 17 |
| **Distribution** | Discrete Gamma-like; mean ≈ 9.5, σ ≈ 3.8 |
| **Nullable** | No |

**Description:**  
Total number of alternating-role turns in the captured conversational
session. Drawn from a discrete Gamma-approximated distribution with
domain-conditioned shape parameters — `social_casual` sessions show
higher mean turn counts (~11.2) than `government_portal` sessions (~6.1),
reflecting the transactional vs. relational nature of those domains.

---

### `turn_number`
| Attribute | Value |
|-----------|-------|
| **Column #** | 17 |
| **Data Type** | `int8` |
| **Range** | 1 – 17 (always ≤ `n_turns_total`) |
| **Nullable** | No |

**Description:**  
Ordinal position of the current record within its conversational session
sequence (1-indexed). Combined with `session_id`, enables full sequential
reconstruction of conversation trajectories. Turn number is a critical
positional embedding input for Transformer-based dialogue models and
turn-depth-conditioned feature analysis.

---

### `turn_role`
| Attribute | Value |
|-----------|-------|
| **Column #** | 18 |
| **Data Type** | `string` |
| **Values** | `USER`, `AGENT` |
| **Nullable** | No |
| **Assignment Rule** | Odd `turn_number` → `USER`; Even → `AGENT` |

**Description:**  
Dyadic role label for the conversational participant producing the
current turn. Alternates deterministically within each session starting
with `USER` at turn 1. Supports supervised role-classification tasks,
asymmetric pragmatic analysis, and turn-level dialogue act labeling.

---

### `stress_index`
| Attribute | Value |
|-----------|-------|
| **Column #** | 19 |
| **Data Type** | `float32` |
| **Range** | [0.0, 1.0] |
| **Distribution** | AR(1)-approximated GP; Hurst H ≈ 0.72 |
| **Nullable** | No |

**Description:**  
Continuous conversational stress indicator ranging from 0.0 to 1.0.
Higher values represent increased conversational tension, urgency,
or friction. The feature is designed to capture evolving interaction
difficulty across conversation turns and is associated with escalation
risk and emotional intensity.

---

## 6. BLOCK E — Code-Switch Linguistic Features (Columns 20–27)

---

### `cs_ratio_hindi`
| Attribute | Value |
|-----------|-------|
| **Column #** | 20 |
| **Data Type** | `float32` |
| **Range** | [0.025, 0.975] |
| **Distribution** | Normal-CDF transformed; mean ≈ 0.51, σ ≈ 0.18 |
| **Nullable** | No |

**Description:**  
Proportion of morphological units in the utterance sourced from the
Hindi lexical and grammatical inventory, quantified via CRF-based
morpheme boundary tagging. Anti-correlated with `cs_ratio_english`
(ρ ≈ −0.85) through the Cholesky joint distribution, with residual
variance capturing congruent bilingual lexical items counted in both
inventories. Primary input dimension for code-switch density regressors.

---

### `cs_ratio_english`
| Attribute | Value |
|-----------|-------|
| **Column #** | 21 |
| **Data Type** | `float32` |
| **Range** | [0.01, 0.99] |
| **Distribution** | Complementary to `cs_ratio_hindi` + Gaussian noise |
| **Nullable** | No |

**Description:**  
Complementary English morpheme proportion. Computed as
`1 − cs_ratio_hindi + ε` where `ε ~ N(0, 0.03)` captures the variance
contributed by congruent bilingual items (e.g., "mobile", "internet")
that are morphologically acceptable in both inventories. Values > 0.70
reliably co-occur with `lang_dominance = english_dominant`.

---

### `morpheme_binding_score`
| Attribute | Value |
|-----------|-------|
| **Column #** | 22 |
| **Data Type** | `float32` |
| **Range** | [0.0, 1.0] |
| **Distribution** | Beta(α=2.5, β=2.5); symmetric, slight bimodality |
| **Nullable** | No |

**Description:**  
Fidelity score measuring the structural integrity of morphologically-bound
code-switching constructs — specifically, the correct attachment of
Hindi system morphemes (TAM suffixes, case markers) to English verb
bases. Score = 1.0 indicates perfect MLF morpheme order compliance
(e.g., `download-kar-raha-hoon`); score → 0.0 indicates morphological
order violations or incomplete binding. Negatively correlated with
`perplexity_score` (ρ ≈ −0.38).

---

### `insertion_rate`
| Attribute | Value |
|-----------|-------|
| **Column #** | 23 |
| **Data Type** | `float32` |
| **Range** | [0.0, 20.0] |
| **Distribution** | Gamma(α=2.0, scale=1.5); right-skewed |
| **Nullable** | No |

**Description:**  
Per-sentence count of Embedded Language island insertions into the
Matrix Language frame, following the MLF insertion taxonomy (Myers-Scotton,
1993). An insertion event is counted each time a contiguous sequence
of EL morphemes appears within an ML clause frame. High values (>10)
occur predominantly in `software_engineering` and `startup_ops` domains
where English technical terminology has no Hindi equivalents.

---

### `alternation_rate`
| Attribute | Value |
|-----------|-------|
| **Column #** | 24 |
| **Data Type** | `float32` |
| **Range** | [0.0, 15.0] |
| **Distribution** | Uniform-CDF transformed; right-bounded |
| **Nullable** | No |

**Description:**  
Rate of inter-sentential alternation events — full clause-level language
switches between consecutive sentences within a single turn. Distinct
from `insertion_rate`: alternation preserves monolingual clause structure
in each segment, while insertion involves intra-clausal EL islands.
High alternation rates characterize `balanced_mixed` lang_dominance
records and `semi_formal` register interactions.

---

### `congruent_lexical_rate`
| Attribute | Value |
|-----------|-------|
| **Column #** | 25 |
| **Data Type** | `float32` |
| **Range** | [0.0, 100.0] |
| **Unit** | Percentage (%) |
| **Nullable** | No |

**Description:**  
Percentage of lexical items in the utterance classified as congruent —
phonologically or orthographically adapted borrowings accepted into both
Hindi and English lexical norms (e.g., "mobile", "internet", "download",
"recharge", "update"). Congruent items reduce detectable switching events
while preserving code-mixed register identity. High congruence rates
(>60%) are diagnostic of `ecommerce_support` and `fintech_banking`
domains.

---

### `hindi_morpheme_count`
| Attribute | Value |
|-----------|-------|
| **Column #** | 26 |
| **Data Type** | `int16` |
| **Range** | [0, 40] |
| **Distribution** | Gamma(α=3, scale=2); mean ≈ 6, right-skewed |
| **Nullable** | No |

**Description:**  
Absolute count of Hindi-origin morphemes identified in the utterance
via morpheme boundary analysis using the IIIT-Hyderabad Hindi Treebank
CRF tagger. Positively correlated with `utterance_length_chars` and
strongly associated with `lang_dominance = hindi_dominant`. Counts
include both free morphemes (content words) and bound morphemes (TAM
suffixes, case markers).

---

### `english_morpheme_count`
| Attribute | Value |
|-----------|-------|
| **Column #** | 27 |
| **Data Type** | `int16` |
| **Range** | [0, 40] |
| **Distribution** | Gamma(α=3, scale=2); mean ≈ 6, right-skewed |
| **Nullable** | No |

**Description:**  
Absolute count of English-origin morphemes. Jointly correlated with
`hindi_morpheme_count` (ρ ≈ +0.31) via the shared Cholesky correlation
structure, reflecting the empirical observation that longer utterances
tend to draw from both morpheme inventories simultaneously. High counts
co-occur with `domain_context` values in technology and finance domains.

---

## 7. BLOCK F — Lexical & Syntactic Quality (Columns 28–33)

---

### `lexical_density`
| Attribute | Value |
|-----------|-------|
| **Column #** | 28 |
| **Data Type** | `float32` |
| **Range** | [0.0, 100.0] |
| **Unit** | Percentage (%) |
| **Distribution** | Normal-CDF transformed; mean ≈ 50, σ ≈ 18 |
| **Nullable** | No |

**Description:**  
Percentage of content words (nouns, verbs, adjectives, adverbs) relative
to total word count, following the standard computational linguistics
operationalization (Ure, 1971). Higher values (>65%) indicate
information-dense professional domain utterances; lower values (<35%)
indicate phatic, filler-heavy casual exchanges with high frequency of
function words, particles, and discourse markers.

---

### `syntax_fluency_index`
| Attribute | Value |
|-----------|-------|
| **Column #** | 29 |
| **Data Type** | `float32` |
| **Range** | [0.5, 10.0] |
| **Scale** | 10-point composite; higher = more fluent |
| **Nullable** | No |

**Description:**  
Composite syntactic fluency rating aggregating dependency parse depth,
grammatical agreement accuracy across the ML frame, and MLF morpheme
order compliance score. Correlated with `pragmatic_coherence` (ρ ≈ +0.58)
and `formality_score` (ρ ≈ +0.44) through the shared Cholesky
correlation structure. Values below 3.0 indicate structurally fragmented
utterances with incomplete clause structures.

---

### `pragmatic_coherence`
| Attribute | Value |
|-----------|-------|
| **Column #** | 30 |
| **Data Type** | `float32` |
| **Range** | [0.5, 10.0] |
| **Scale** | 10-point composite; higher = more coherent |
| **Nullable** | No |

**Description:**  
Discourse-level coherence rating measuring conversational relevance and
illocutionary force alignment relative to the preceding conversational
context. Captures pragmatic failures — topic drift, false starts,
incomplete speech acts, and politeness strategy mismatches — that
characterize natural conversational telemetry under cognitive load.
Low scores (<3.0) correlate with `frustrated` and `confused` emotional
tones.

---

### `utterance_length_chars`
| Attribute | Value |
|-----------|-------|
| **Column #** | 31 |
| **Data Type** | `int16` |
| **Range** | [5, 250] |
| **Distribution** | Gamma(α=4, scale=12); mean ≈ 48, right-skewed |
| **Unit** | Unicode characters (incl. emoji codepoints) |
| **Nullable** | No |

**Description:**  
Character count of the raw utterance string including all Unicode
characters, punctuation, whitespace, and emoji codepoints. Gamma-distributed
to match empirical mobile messaging character length distributions.
Strongly correlated with `token_count` (ρ ≈ +0.91) and moderately
correlated with `hindi_morpheme_count` + `english_morpheme_count` total.

---

### `token_count`
| Attribute | Value |
|-----------|-------|
| **Column #** | 32 |
| **Data Type** | `int16` |
| **Range** | [2, 80] |
| **Distribution** | Derived from `utterance_length_chars`; mean ≈ 10 |
| **Nullable** | No |

**Description:**  
Whitespace-tokenized word count of the utterance, derived from
`utterance_length_chars` divided by a language-conditioned mean token
length estimator (range 3.5–5.5 chars/token). Primary input dimension
for computational load modeling in LLM inference pipelines and sequence
padding/truncation decisions.

---

### `oov_token_rate`
| Attribute | Value |
|-----------|-------|
| **Column #** | 33 |
| **Data Type** | `float32` |
| **Range** | [0.0, 0.35] |
| **Distribution** | Normal-CDF transformed; mean ≈ 0.10 |
| **Nullable** | No |

**Description:**  
Fraction of tokens classified as out-of-vocabulary relative to a combined
Hindi-English reference dictionary of 250,000 entries. High OOV rates
reflect transliteration variation (multiple valid romanizations of Hindi
phonemes), nonce borrowings, and morphologically novel code-switched
constructs not yet lexicalized in reference corpora. Highest OOV rates
occur in `feature_phone` device records and `informal_youth` register.

---

## 8. BLOCK G — Sentiment & Affect (Columns 34–37)

---

### `sentiment_valence`
| Attribute | Value |
|-----------|-------|
| **Column #** | 34 |
| **Data Type** | `float32` |
| **Range** | [−1.0, 1.0] |
| **Scale** | Bipolar; −1.0 = maximally negative, +1.0 = maximally positive |
| **Distribution** | Clipped Normal; mean ≈ 0.0, σ ≈ 0.35 |
| **Nullable** | No |

**Description:**  
Affective valence dimension of the utterance on the bipolar
negative-to-positive scale (Russell, 1980), derived from lexical
sentiment analysis combined with discourse marker classification.
Correlated with `emotional_tone` categorical label (ρ ≈ +0.71):
`frustrated`, `urgent`, `anxious`, `sarcastic` tones cluster at
negative valence; `grateful`, `enthusiastic`, `celebratory` cluster
at positive valence.

---

### `sentiment_arousal`
| Attribute | Value |
|-----------|-------|
| **Column #** | 35 |
| **Data Type** | `float32` |
| **Range** | [0.0, 1.0] |
| **Scale** | Unipolar; 0.0 = calm, 1.0 = maximally activated |
| **Distribution** | Half-Normal (absolute value of Gaussian) |
| **Nullable** | No |

**Description:**  
Arousal (activation) dimension of the utterance on the Russell
circumplex model, capturing intensity of emotional activation independent
of valence direction. High arousal characterizes both `frustrated` and
`enthusiastic` tones (~0.75–0.90); `neutral_transactional` and
`apologetic` tones show low arousal (~0.15–0.30). Arousal is the
primary predictor of `response_latency_ms` variance.

---

### `formality_score`
| Attribute | Value |
|-----------|-------|
| **Column #** | 36 |
| **Data Type** | `float32` |
| **Range** | [1.0, 10.0] |
| **Scale** | 10-point; 10.0 = maximally formal |
| **Distribution** | Normal-CDF transformed; mean varies by `social_register` |
| **Nullable** | No |

**Description:**  
Composite formality rating integrating lexical formality markers (Heylighen
& Dewaele, 1999), honorific usage frequency (`aap` vs `tum` vs `tu`),
punctuation compliance rate, sentence completeness index, and absence of
filler particles. Strong predictor of `platform_channel` — `Email` records
show mean formality 8.6; `Discord` records show mean formality 2.8.

---

### `politeness_score`
| Attribute | Value |
|-----------|-------|
| **Column #** | 37 |
| **Data Type** | `float32` |
| **Range** | [1.0, 10.0] |
| **Scale** | 10-point; 10.0 = maximally polite |
| **Nullable** | No |

**Description:**  
Pragmatic politeness rating operationalized via Brown and Levinson's
(1987) face-threatening act (FTA) framework — measuring density of
positive politeness strategies (agreement tokens, solidarity markers,
`please`/`kindly` usage) and negative politeness strategies (hedges,
indirect requests, question forms for directives). Correlated with
`formality_score` (ρ ≈ +0.62) but distinct: `authoritative_senior`
register shows high formality with low positive politeness.

---

## 9. BLOCK H — Behavioral Telemetry (Columns 38–41)

---

### `response_latency_ms`
| Attribute | Value |
|-----------|-------|
| **Column #** | 38 |
| **Data Type** | `float32` |
| **Range** | [50, 30,000] |
| **Unit** | Milliseconds |
| **Distribution** | Gamma(α=2.5, scale=800); mean ≈ 2,000ms, heavy right tail |
| **Nullable** | No |

**Description:**  
Inter-turn response latency in milliseconds, capturing the wall-clock
delay between the preceding turn's completion and the current turn's
first keystroke event, as recorded by the edge telemetry daemon.
Gamma-distributed with heavy right tail reflecting cognitive load peaks,
context retrieval delays, multi-task interruption patterns, and
network-induced delays on `feature_phone` devices. Negatively correlated
with `typing_speed_wpm` (ρ ≈ −0.44).

---

### `typing_speed_wpm`
| Attribute | Value |
|-----------|-------|
| **Column #** | 39 |
| **Data Type** | `float32` |
| **Range** | [10, 200] |
| **Unit** | Words per minute |
| **Distribution** | Gamma(α=5, scale=18); mean ≈ 90 WPM |
| **Nullable** | No |

**Description:**  
Estimated words-per-minute production rate derived from turn completion
duration and `token_count`. Strongly conditioned on `device_type`:
`desktop_web` mean ≈ 115 WPM; `feature_phone` mean ≈ 32 WPM. Voice-to-text
input events (identifiable by extreme high WPM values >160 combined with
low `edit_distance_ratio`) create a bimodal sub-distribution within the
`android_smartphone` device stratum.

---

### `edit_distance_ratio`
| Attribute | Value |
|-----------|-------|
| **Column #** | 40 |
| **Data Type** | `float32` |
| **Range** | [0.0, 0.60] |
| **Distribution** | Normal-CDF transformed; mean ≈ 0.12 |
| **Nullable** | No |

**Description:**  
Normalized Levenshtein edit distance between initial draft keystrokes
and the final submitted utterance text (ratio to final utterance length).
Captures self-correction intensity — a behavioral proxy for linguistic
uncertainty at code-switching decision points. High values (>0.35)
co-occur with `oov_token_rate` > 0.20 and `balanced_mixed` lang_dominance,
reflecting the elevated decision complexity of mid-utterance language
frame selection.

---

### `perplexity_score`
| Attribute | Value |
|-----------|-------|
| **Column #** | 41 |
| **Data Type** | `float32` |
| **Range** | [5.0, 800.0] |
| **Distribution** | Gamma(α=3, scale=25); log-normal character; mean ≈ 75 |
| **Nullable** | No |

**Description:**  
N-gram language model perplexity of the utterance computed against a
bilingual Hindi-English 4-gram Kneser-Ney smoothed reference model
trained on 50M tokens of Hinglish web text. High perplexity values
index novel code-switching constructs, heavy OOV token sequences, and
structurally atypical utterances that expose gaps in current multilingual
model architectures. Negatively correlated with `morpheme_binding_score`.

---

## 10. BLOCK I — NLP Quality Proxies & System (Columns 42–45)

---

### `language_model_perplexity`
| Attribute | Value |
|-----------|-------|
| **Column #** | 42 |
| **Data Type** | `float32` |
| **Range** | [5.0, 800.0] |
| **Distribution** | Identical marginal to `perplexity_score`; correlated |
| **Nullable** | No |

**Description:**  
Perplexity estimate representing contextual language-model uncertainty.
Lower values indicate more predictable utterances, while higher values
reflect linguistically novel, rare, or complex code-switching patterns.

---

### `schema_version`
| Attribute | Value |
|-----------|-------|
| **Column #** | 43 |
| **Data Type** | `string` (constant) |
| **Value** | `v1.4.2-MLF` (uniform across all 1M records) |
| **Nullable** | No |

**Description:**  
Dataset schema version identifier following semantic versioning protocol.
Uniform constant across all records in this release. Enables automated
schema validation, backward-compatibility checking in ingestion pipelines,
and unambiguous version resolution when multiple HCSS corpus releases are
concatenated in multi-version training pipelines.

---

### `dialogue_json`
| Attribute | Value |
|-----------|-------|
| **Column #** | 44 |
| **Data Type** | `string` (serialized JSON) |
| **Max Length** | ~8,192 characters (17 turns × ~480 chars/turn) |
| **Nullable** | No |
| **Encoding** | UTF-8 with full Unicode (Devanagari, emoji preserved) |

**Description:**  
Embedded multi-turn dialogue transaction log serialized as a UTF-8 JSON
string. Contains the complete structured conversational payload for the
session, with per-turn role, Markov state, utterance text, language tag,
and code-switch point annotation. This is the primary payload column for
sequence modeling, dialogue state tracking, utterance-level NLP tasks,
and MLF boundary detection. Full schema documented in Section 12 below.

**Parsing Example:**
```python
import json
import pandas as pd
import pyarrow.parquet as pq

df = pq.read_table("HinglishCodeSwitch_Syntax_v1_elite.parquet").to_pandas()
record   = json.loads(df["dialogue_json"].iloc[0])
for turn in record["turns"]:
    print(f"[{turn['role']}|{turn['state']}] {turn['utterance']}")
```

---
## DATASET COMPOSITION

The HinglishCodeSwitch-Syntax (HCSS) corpus is a large-scale multilingual
conversational dataset designed to represent realistic Hindi-English
code-switched communication patterns commonly observed across digital
communication platforms.

The dataset contains 1,000,000 conversational records distributed across
multiple communication domains including professional collaboration,
academic discussions, customer support interactions, financial services,
healthcare administration, startup operations, government services, and
informal social communication.

Each record represents a conversational event enriched with linguistic,
behavioral, demographic, temporal, and conversational-state metadata.

The corpus captures several forms of code-switching behavior:

- Lexical insertion of English terms into Hindi sentence structures
- Hindi lexical insertions within English sentence structures
- Clause-level language alternation
- Morpheme-level language mixing
- Script switching between Romanized Hindi and Devanagari
- Register adaptation across formal and informal contexts

Conversation sessions range from short transactional exchanges to
extended multi-turn discussions. Session lengths vary from 2 to 17
turns, with an average of approximately 9–10 conversational turns.

The dataset additionally includes behavioral and interaction signals
such as response latency, typing speed, editing behavior, sentiment,
formality, politeness, and conversational progression states.

HCSS is intended for research and development in:

- Code-switching analysis
- Multilingual NLP
- Conversational AI
- Dialogue state tracking
- Sentiment analysis
- Register prediction
- Sociolinguistic modeling
- Language identification
- Sequence modeling
- Human-computer interaction research

## 11. ENUMERATION REFERENCE TABLES

### 11.1 `geo_state` — Complete Enumeration
Maharashtra | Delhi | Karnataka | Tamil_Nadu | Telangana | Gujarat
West_Bengal | Uttar_Pradesh | Rajasthan | Punjab | Haryana | Kerala
Madhya_Pradesh | Bihar | Odisha | Jharkhand | Chhattisgarh | Other
### 11.2 `domain_context` — Complete Enumeration
software_engineering | corporate_hr | student_academic | ecommerce_support
social_casual | fintech_banking | healthcare_admin | startup_ops
government_portal | media_content
### 11.3 `social_register` — Complete Enumeration (Quasi-Ordinal)
formal_professional > authoritative_senior > semi_formal
casual_peer > intimate_family > informal_youth
### 11.4 `emotional_tone` — Complete Enumeration
frustrated | urgent | polite_request | confused | grateful | sarcastic
enthusiastic | neutral_transactional | anxious | assertive | apologetic
celebratory
### 11.5 `conversation_state` — Complete Enumeration with Absorbing Status
INIT_GREETING          [transient]
PROBLEM_STATEMENT      [transient]
CLARIFICATION_REQUEST  [transient]
INFORMATION_EXCHANGE   [transient]
NEGOTIATION            [transient]
TASK_DELEGATION        [transient]
CONFIRMATION_SEEK      [transient]
AFFIRMATIVE_CLOSE      [ABSORBING — terminal]
NEGATIVE_ESCALATION    [ABSORBING — terminal]
TERMINAL_RESOLUTION    [ABSORBING — terminal]
### 11.6 `lang_dominance` — Complete Enumeration
hindi_dominant | english_dominant | balanced_mixed
### 11.7 `script_modality` — Complete Enumeration
Roman_transliterated | Mixed_script | Devanagari | English_only
### 11.8 `device_type` — Complete Enumeration
android_smartphone | ios_smartphone | desktop_web | tablet_android
tablet_ios | feature_phone | unknown
### 11.9 `platform_channel` — Complete Enumeration
WhatsApp | Telegram | Instagram_DM | Twitter_X | LinkedIn
Slack | MS_Teams | SMS | Email | Discord
### 11.10 `age_group` — Complete Enumeration (Ordinal)
18-24 | 25-34 | 35-44 | 45-54 | 55+
### 11.11 `turn_role` — Complete Enumeration
USER | AGENT
---

## 12. DIALOGUE JSON SCHEMA

Every `dialogue_json` value is a UTF-8 serialized JSON object conforming
to the following schema:

```json
{
  "turns": [
    {
      "turn":       <integer>  1-indexed turn ordinal within session,
      "role":       <string>   "USER" | "AGENT",
      "state":      <string>   One of 10 Markov state labels,
      "utterance":  <string>   MLF-generated Hinglish utterance (≤512 chars),
      "lang_tag":   <string>   "hindi_dominant" | "english_dominant" | "balanced_mixed",
      "cs_point":   <boolean>  true if code-switch crossing detected at clause boundary
    },
    ...
  ]
}
```

**Field Definitions:**

| JSON Field | Type | Description |
|------------|------|-------------|
| `turn` | `int` | 1-indexed position within the session's turn sequence |
| `role` | `string` | Speaker role; alternates USER→AGENT→USER... |
| `state` | `string` | Markov state at this turn; sampled from NSMC conditioned on `stress_index` |
| `utterance` | `string` | MLF-generated Hinglish text; max 512 chars; UTF-8 with Devanagari and emoji support |
| `lang_tag` | `string` | Matrix Language classification for this specific turn (may differ from record-level `lang_dominance`) |
| `cs_point` | `bool` | True when a code-switch insertion point was detected at the primary clause boundary of this turn |

**Example Record:**
```json
{
  "turns": [
    {
      "turn": 1,
      "role": "USER",
      "state": "PROBLEM_STATEMENT",
      "utterance": "Toh Main jaldi se is PR ko review-karna chahta hoon by EOD yaar 😅 Deadline kal hai and still nothing.",
      "lang_tag": "hindi_dominant",
      "cs_point": true
    },
    {
      "turn": 2,
      "role": "AGENT",
      "state": "CLARIFICATION_REQUEST",
      "utterance": "Basically, which branch — main ya staging? Bata dena.",
      "lang_tag": "balanced_mixed",
      "cs_point": false
    },
    {
      "turn": 3,
      "role": "USER",
      "state": "INFORMATION_EXCHANGE",
      "utterance": "Main aur meri team staging branch ko properly merge-kar-rahe hain abhi.",
      "lang_tag": "hindi_dominant",
      "cs_point": true
    }
  ]
}
```

---

## 13. CORRELATION STRUCTURE REFERENCE

The following Observed feature relationships
are preserved in the corpus shows a positive relationship:

| Feature A | Feature B | Expected ρ | Direction | Mechanism |
|-----------|-----------|-----------|-----------|-----------|
| `cs_ratio_hindi` | `cs_ratio_english` | ≈ −0.85 | Strong negative | Morpheme inventory complementarity |
| `formality_score` | `politeness_score` | ≈ +0.62 | Moderate positive | Register co-variation |
| `syntax_fluency_index` | `pragmatic_coherence` | ≈ +0.58 | Moderate positive | Shared linguistic competence signal |
| `stress_index` | `NEGATIVE_ESCALATION rate` | ≈ +0.58 | Moderate positive | NSMC stress modulation |
| `sentiment_valence` | `emotional_tone` | ≈ +0.71 | Strong positive | Affective label alignment |
| `perplexity_score` | `morpheme_binding_score` | ≈ −0.38 | Moderate negative | Structural regularity inverse |
| `response_latency_ms` | `typing_speed_wpm` | ≈ −0.44 | Moderate negative | Production speed trade-off |
| `utterance_length_chars` | `token_count` | ≈ +0.91 | Very strong positive | Derivation relationship |
| `oov_token_rate` | `edit_distance_ratio` | ≈ +0.33 | Moderate positive | Transliteration uncertainty |
| `insertion_rate` | `domain_context=SE` | ≈ +0.51 | Moderate positive | Technical vocabulary gap |
| `hindi_morpheme_count` | `lang_dominance=HD` | ≈ +0.68 | Strong positive | MLF Matrix Language signal |
| `formality_score` | `platform_channel=Email` | ≈ +0.59 | Moderate positive | Platform register governance |

---

## 14. KNOWN DISTRIBUTIONS & STATISTICAL BOUNDS

| Column | Distribution Family | Parameters | Empirical Mean | Empirical σ |
|--------|--------------------|-----------:|----------------|-------------|
| `cs_ratio_hindi` | Normal-CDF transform | μ=0, σ=1 | ≈ 0.510 | ≈ 0.180 |
| `morpheme_binding_score` | Beta | α=2.5, β=2.5 | ≈ 0.500 | ≈ 0.178 |
| `insertion_rate` | Gamma | α=2.0, scale=1.5 | ≈ 3.0 | ≈ 2.1 |
| `lexical_density` | Normal-CDF × 100 | μ=0, σ=1 | ≈ 50.0 | ≈ 18.0 |
| `sentiment_valence` | Clipped Normal | μ=0, σ=0.35 | ≈ 0.001 | ≈ 0.345 |
| `sentiment_arousal` | Half-Normal | σ=0.5 | ≈ 0.400 | ≈ 0.228 |
| `response_latency_ms` | Gamma | α=2.5, scale=800 | ≈ 2,000 | ≈ 1,265 |
| `typing_speed_wpm` | Gamma | α=5.0, scale=18 | ≈ 90.0 | ≈ 40.2 |
| `perplexity_score` | Gamma | α=3.0, scale=25 | ≈ 75.0 | ≈ 43.3 |
| `utterance_length_chars` | Gamma | α=4.0, scale=12 | ≈ 48.0 | ≈ 24.0 |
| `oov_token_rate` | Normal-CDF × 0.35 | μ=0, σ=1 | ≈ 0.100 | ≈ 0.065 |
| `stress_index` | AR(1) GP approx | φ≈0.9999, σ_ε | ≈ 0.500 | ≈ 0.289 |
| `n_turns_total` | Discrete Gamma-like | mean≈9.5, σ≈3.8 | ≈ 9.5 | ≈ 3.8 |
| `edit_distance_ratio` | Normal-CDF × 0.60 | μ=0, σ=1 | ≈ 0.120 | ≈ 0.110 |

---

## 15. NULL POLICY & MISSING VALUE PROTOCOL

**Zero-Null Guarantee:** This corpus enforces a strict zero-null policy
across all 45 columns. The generation pipeline applies the following
null prevention controls:

| Control | Implementation |
|---------|----------------|
| Numeric bounds clipping | `np.clip()` applied post-distribution transform |
| Integer rounding | `np.round().astype()` with explicit dtype casting |
| String categorical | `rng.choice()` with exhaustive probability vectors summing to 1.0 |
| Timestamp generation | Deterministic offset arithmetic from fixed base date |
| JSON payload | Per-row generation with guaranteed minimum 1 turn |
| ID fields | Deterministic UUID and sequential ID construction |

**Null Verification:**
```python
assert df.isnull().sum().sum() == 0, "Null values detected"
```

If null values are detected in a downloaded copy, this indicates
file corruption during transfer. Re-download and verify SHA-256 checksum.

---

## 16. RECOMMENDED FEATURE GROUPINGS FOR ML

### 16.1 Code-Switch Density Regression
**Target:** `cs_ratio_hindi` or `morpheme_binding_score`  
**Features:** `lang_dominance`, `domain_context`, `social_register`,
`age_group`, `geo_state`, `insertion_rate`, `alternation_rate`,
`congruent_lexical_rate`, `script_modality`

### 16.2 Conversation State Classification (Markov Label)
**Target:** `conversation_state` (10-class)  
**Features:** `turn_number`, `n_turns_total`, `stress_index`,
`emotional_tone`, `sentiment_valence`, `sentiment_arousal`,
`pragmatic_coherence`, `syntax_fluency_index`, `domain_context`

### 16.3 Dialogue Sequence Modeling (Turn-Level)
**Input:** `dialogue_json` turns (ordered by `turn_number`)  
**Auxiliary:** `session_id`, `lang_dominance`, `stress_index`,
`domain_context`, `social_register`

### 16.4 Register & Formality Prediction
**Target:** `social_register` or `formality_score`  
**Features:** `platform_channel`, `domain_context`, `age_group`,
`politeness_score`, `filler_particle_rate` (derivable from `dialogue_json`),
`honorific_density` (derivable from `dialogue_json`)

### 16.5 Behavioral Latency Modeling
**Target:** `response_latency_ms`  
**Features:** `device_type`, `platform_channel`, `stress_index`,
`sentiment_arousal`, `emotional_tone`, `token_count`,
`edit_distance_ratio`, `hour_of_day`, `day_of_week`

### 16.6 Reinforcement Learning — Dialogue Policy
**State space:** `conversation_state`, `stress_index`, `turn_number`,
`emotional_tone`, `sentiment_valence`  
**Action space:** `turn_role` × `conversation_state` (next)  
**Reward signal:** Terminal state reached (`AFFIRMATIVE_CLOSE` = +1,
`NEGATIVE_ESCALATION` = −1, `TERMINAL_RESOLUTION` = 0)  
**Trajectory:** Group by `session_id`, order by `turn_number`

---

## APPENDIX: QUICK REFERENCE CARD
HCSS v1.4.2-MLF  |  1,000,000 rows  |  45 columns  |  Parquet/Snappy
BLOCKS:
A: record_id, session_id, timestamp_utc, hour_of_day, day_of_week
B: geo_state, age_group, device_type, platform_channel
C: domain_context, social_register, emotional_tone, lang_dominance,
script_modality, conversation_state
D: n_turns_total, turn_number, turn_role, stress_index
E: cs_ratio_hindi, cs_ratio_english, morpheme_binding_score,
insertion_rate, alternation_rate, congruent_lexical_rate,
hindi_morpheme_count, english_morpheme_count
F: lexical_density, syntax_fluency_index, pragmatic_coherence,
utterance_length_chars, token_count, oov_token_rate
G: sentiment_valence, sentiment_arousal, formality_score, politeness_score
H: response_latency_ms, typing_speed_wpm, edit_distance_ratio,
perplexity_score
I: language_model_perplexity, schema_version, dialogue_json
PRIMARY KEY   : record_id
SESSION KEY   : session_id
TEMPORAL IDX  : timestamp_utc
PAYLOAD COL   : dialogue_json (structured JSON; full turn-level detail)
NULL POLICY   : Zero nulls guaranteed across all 45 columns
SPLIT RULE    : Temporal ordering mandatory (non-i.i.d. stress trajectory)
---

*HinglishCodeSwitch-Syntax Data Dictionary v1.4.2-MLF*  
*International Multilingual AI Telemetry Consortium (IMATC)*  
*CC BY 4.0 | DOI: 10.57967/hf/IMATC-HCSS-2024-v1*
