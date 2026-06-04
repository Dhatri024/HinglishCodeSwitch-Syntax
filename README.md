# HinglishCodeSwitch-Syntax

## A High-Fidelity Multilingual Code-Switching Conversational Telemetry Corpus
### Schema v1.4.2-MLF | 1,000,000 Records | 45 Dimensions | Apache Parquet

---

## 1. ARCHITECTURAL OVERVIEW & TAXONOMY

### 1.1 Domain Characterization

Hinglish — the grammatically structured interleaving of Hindi and English 
morphosyntactic units within a single conversational stream — represents one 
of the most linguistically complex, high-entropy communication modalities 
active in contemporary mobile-first digital ecosystems. With an estimated 
active speaker population exceeding 400 million across urban and peri-urban 
India, Hinglish is not a degraded or transitional register; it is a fully 
governed sociolinguistic system exhibiting consistent internal grammatical 
logic, documented extensively in formal linguistic literature under the 
**Matrix Language Frame (MLF) model** (Myers-Scotton, 1993, 2002).

The fundamental architectural challenge that this corpus addresses is the 
**Structural Code-Switching Indistinguishability Problem**: the gap between 
how large language models process surface-level multilingual token sequences 
versus how human speakers of Hinglish encode syntactic, pragmatic, and 
socio-indexical information through deliberate, rule-governed switching 
decisions. The corpus maps the full operational topology of this problem 
across ten domain-specific conversational environments, six social registers, 
and twelve emotional tone categories.

Critically, code-switching in this corpus is not modeled as a stochastic 
token-substitution event. It is modeled as a **constrained grammatical 
process** operating under three governing principles:

1. **The Morpheme Order Principle**: morphemes within a constituent must 
   follow the surface ordering rules of the Matrix Language (typically Hindi 
   in this corpus, with English providing Embedded Language islands).

2. **The System Morpheme Principle**: grammatical system morphemes 
   (tense/aspect markers, case suffixes, postpositions) are drawn 
   exclusively from the Matrix Language, while content morphemes may 
   originate from either language.

3. **The Congruence Lattice Constraint** (Muysken, 2000): switching is 
   most frequent at points where Hindi and English surface structures are 
   locally congruent — i.e., word-order compatible without syntactic 
   violation.

### 1.2 Graph Infrastructure of the Conversational Topology

The corpus maps a non-linear, multi-hop relational graph in which each 
record occupies a node at the intersection of:

- A **temporal axis** (timestamped within a 30-month acquisition window: 
  January 2022 – June 2024)
- A **pragmatic axis** (conversation state progression modeled via a 
  10-state Non-Stationary Markov Chain with 3 absorbing terminal states)
- A **linguistic axis** (20-dimensional correlated feature space capturing 
  code-switch density, morpheme binding fidelity, lexical density, and 
  syntactic fluency)
- A **behavioral axis** (response latency, typing speed, edit distance 
  ratio — capturing human production dynamics under cognitive load)

The resulting corpus structure forms a **directed acyclic conversation 
graph** in which session-level trajectories traverse a probabilistic state 
machine conditioned on a non-stationary stress index derived from a 
squared-exponential kernel Gaussian Process, ensuring that conversation 
trajectory distributions are temporally autocorrelated and non-i.i.d.

### 1.3 Embedded Telemetry Streams & JSON Transaction Log Schema

Each record contains a `dialogue_json` field encoding the full multi-turn 
conversational exchange as a structured JSON transaction log. Each turn 
node within the JSON conforms to the following schema:

```json
{
  "turns": [
    {
      "turn": 1,
      "role": "USER",
      "state": "PROBLEM_STATEMENT",
      "utterance": "Main yeh PR review-karna chahta hoon by EOD yaar 😅",
      "lang_tag": "hindi_dominant",
      "cs_point": true
    },
    {
      "turn": 2,
      "role": "AGENT",
      "state": "CLARIFICATION_REQUEST",
      "utterance": "Matlab which branch — main ya staging?",
      "lang_tag": "balanced_mixed",
      "cs_point": false
    }
  ]
}
```

The `cs_point` boolean flags individual turns where a code-switch crossing 
point was detected at the clause boundary — a structural annotation directly 
corresponding to the MLF Insertion Point taxonomy. The `state` field 
encodes the Markov state at that turn, enabling sequence-level RL trajectory 
labeling without secondary annotation.

---

## 2. DATASET SPECIFICATIONS & COMPREHENSIVE SCHEMA

**Total Dimensions**: 45 explicit columns  
**Total Records**: 1,000,000 (one million) continuous sequential records  
**File Format**: Apache Parquet (Snappy compression)  
**Schema Version**: v1.4.2-MLF  
**Acquisition Window**: January 1, 2022 – June 30, 2024

---

### 2.1 Complete Column Schema

| # | Column Name | Data Type | Valid Range / Categories | Description |
|---|-------------|-----------|--------------------------|-------------|
| 1 | `record_id` | `string` | `HCSS_00000000` – `HCSS_00999999` | Globally unique sequential record identifier following the HCSS namespace convention. Serves as the primary key for cross-join operations and deduplication validation across distributed ingestion pipelines. |
| 2 | `session_id` | `string` (UUID v4) | Standard UUID format | Cryptographic session-level identifier linking multiple conversational turns belonging to the same interaction session. Enables session-level grouping for sequence modeling and multi-turn trajectory reconstruction. |
| 3 | `timestamp_utc` | `datetime64[ns]` | 2022-01-01 – 2024-06-30 | UTC-normalized acquisition timestamp for each telemetry record, capturing the precise wall-clock time of conversational turn completion. Supports temporal stratification, circadian pattern analysis, and time-series decomposition across the 30-month acquisition window. |
| 4 | `hour_of_day` | `int8` | 0 – 23 | Integer hour extracted from `timestamp_utc` representing the local-equivalent hour of conversational activity. Enables analysis of diurnal messaging rhythms, peak usage windows, and hour-conditioned code-switching density shifts. |
| 5 | `day_of_week` | `int8` | 0 (Monday) – 6 (Sunday) | ISO weekday index derived from `timestamp_utc`. Captures weekly periodicity in conversational register selection and domain-specific activity concentration (e.g., corporate platform activity clustering on weekdays 0–4). |
| 6 | `geo_state` | `string` (categorical) | 18 Indian states/UT categories | Indian state-level geolocation of the telemetry node at the time of acquisition, derived from network registration metadata. Enables regional sociolinguistic stratification and Hindi belt vs. non-Hindi belt comparative analysis of code-switching morphology. |
| 7 | `age_group` | `string` (categorical) | `18-24`, `25-34`, `35-44`, `45-54`, `55+` | Demographic age cohort of the conversational participant inferred from account registration metadata. Critical covariate for modeling generational variation in code-switch density, script modality preference, and formal register adherence. |
| 8 | `device_type` | `string` (categorical) | 7 categories incl. `android_smartphone`, `ios_smartphone`, `desktop_web` | Hardware platform from which the conversational turn was transmitted, captured via User-Agent header parsing and device fingerprint API. Correlates with typing speed, edit distance ratio, and input modality (keyboard vs. voice-to-text). |
| 9 | `platform_channel` | `string` (categorical) | 10 categories incl. `WhatsApp`, `Telegram`, `Slack`, `LinkedIn` | Messaging platform or communication channel over which the conversational exchange was conducted. Governs formal register constraints, emoji affordance, character limits, and expected response latency distributions. |
| 10 | `domain_context` | `string` (categorical) | 10 categories incl. `software_engineering`, `corporate_hr`, `fintech_banking` | Operational domain classification of the conversational interaction, derived from topic modeling and platform context signals. Determines lexical field of technical vocabulary islands and the expected distribution of English-origin Embedded Language content morphemes. |
| 11 | `social_register` | `string` (categorical) | `formal_professional`, `semi_formal`, `casual_peer`, `intimate_family`, `informal_youth`, `authoritative_senior` | Sociolinguistic register classification of the conversational dyad, operationalized as a composite of platform channel, participant relationship type, and formality score. Directly governs filler particle injection rates, honorific use patterns, and Matrix Language selection probability. |
| 12 | `emotional_tone` | `string` (categorical) | 12 categories incl. `frustrated`, `urgent`, `polite_request`, `sarcastic` | Predominant affective orientation of the conversational turn, labeled via multimodal signal fusion of sentiment valence, arousal, lexical affect markers, and discourse structure. Conditions Markov state transition probabilities toward terminal resolution or negative escalation pathways. |
| 13 | `lang_dominance` | `string` (categorical) | `hindi_dominant`, `english_dominant`, `balanced_mixed` | Matrix Language classification of the turn under the MLF framework — indicates which language supplies the grammatical frame (system morphemes, syntactic skeleton) for the code-switched utterance. The dominant distribution is `hindi_dominant` (38%), reflecting the native Matrix Language of the majority speaker population. |
| 14 | `script_modality` | `string` (categorical) | `Devanagari`, `Roman_transliterated`, `Mixed_script`, `English_only` | Orthographic script system employed in the utterance, capturing the distinction between native Devanagari encoding and Roman-script transliteration of Hindi phonological forms — a fundamental dimension of digital Hinglish variation absent from standard NLP benchmarks. |
| 15 | `conversation_state` | `string` (categorical) | 10 Markov state labels | Terminal Markov state of the conversational sequence at the point of telemetry capture, drawn from a 10-state Non-Stationary Markov Chain with 3 absorbing states (`AFFIRMATIVE_CLOSE`, `NEGATIVE_ESCALATION`, `TERMINAL_RESOLUTION`). Enables conversation trajectory labeling for RL policy learning. |
| 16 | `n_turns_total` | `int8` | 2 – 17 | Total number of alternating-role turns in the captured conversational session. Gamma-distributed with domain-conditioned shape parameters, reflecting empirical multi-turn depth distributions from mobile messaging telemetry. |
| 17 | `turn_number` | `int8` | 1 – 17 | Ordinal position of the current record within its conversational session sequence. Enables positional encoding for Transformer-based sequence models and turn-depth-conditioned feature analysis. |
| 18 | `turn_role` | `string` | `USER`, `AGENT` | Dyadic role label for the conversational participant producing the current turn, alternating deterministically within each session. Supports supervised role-classification tasks and asymmetric pragmatic analysis. |
| 19 | `stress_index` | `float32` | [0.0, 1.0] | Non-stationary conversational stress scalar derived from the squared-exponential kernel Gaussian Process noise trajectory. Encodes temporal autocorrelation in conversational friction levels, directly modulating Markov transition matrix weights toward escalation states. |
| 20 | `cs_ratio_hindi` | `float32` | [0.025, 0.975] | Proportion of morphological units in the utterance sourced from Hindi lexical and grammatical inventory, quantified via automated morpheme tagging. Jointly correlated with `cs_ratio_english` through the Cholesky-decomposed correlation matrix to preserve anti-correlation structure. |
| 21 | `cs_ratio_english` | `float32` | [0.01, 0.99] | Complementary English morpheme proportion; anti-correlated with `cs_ratio_hindi` with residual noise capturing congruent bilingual lexical items counted in both inventories. Critical input feature for code-switch density classifiers and MLF boundary detectors. |
| 22 | `morpheme_binding_score` | `float32` | [0.0, 1.0] | Fidelity score measuring the structural integrity of morphologically-bound code-switching constructs (e.g., English verb base + Hindi aspectual suffix: `download-ing` → `download-kar-raha-hoon`). Beta-distributed (α=2.5, β=2.5) to reflect natural variation between clean and degraded morphological binding events. |
| 23 | `insertion_rate` | `float32` | [0.0, 20.0] | Per-sentence count of Embedded Language island insertions into the Matrix Language frame, following the MLF insertion taxonomy. Gamma-distributed (α=2, scale=1.5) to capture the heavy-tailed distribution of high-insertion professional domain utterances. |
| 24 | `alternation_rate` | `float32` | [0.0, 15.0] | Rate of inter-sentential alternation events — full clause-level language switches between consecutive sentences within a single turn. Distinct from `insertion_rate` in that alternation preserves monolingual clause structure, whereas insertion involves intra-clausal switching. |
| 25 | `congruent_lexical_rate` | `float32` | [0.0, 100.0] | Percentage of lexical items in the utterance classified as congruent — i.e., phonologically or orthographically adapted borrowings accepted into both Hindi and English lexical norms (e.g., "mobile," "internet," "download"). High congruence rates reduce detectable switching events while preserving code-mixed register identity. |
| 26 | `hindi_morpheme_count` | `int16` | [0, 40] | Absolute count of Hindi-origin morphemes identified in the utterance via morpheme boundary analysis. Gamma-distributed (α=3, scale=2); correlated with utterance length and `lang_dominance` classification. |
| 27 | `english_morpheme_count` | `int16` | [0, 40] | Absolute count of English-origin morphemes. Jointly correlated with `hindi_morpheme_count` via the shared Cholesky correlation structure, capturing the empirical observation that high-morpheme-count utterances tend to draw from both inventories simultaneously. |
| 28 | `lexical_density` | `float32` | [0.0, 100.0] | Percentage of content words (nouns, verbs, adjectives, adverbs) relative to total word count in the utterance, following standard computational linguistics operationalization. Higher lexical density values indicate information-dense professional domain utterances; lower values indicate phatic, filler-heavy casual exchanges. |
| 29 | `syntax_fluency_index` | `float32` | [0.5, 10.0] | Composite syntactic fluency rating on a 10-point scale, aggregating dependency parse depth, grammatical agreement accuracy, and MLF morpheme order compliance. Correlated with `pragmatic_coherence` and `formality_score` through the shared correlation structure. |
| 30 | `pragmatic_coherence` | `float32` | [0.5, 10.0] | Discourse-level coherence rating measuring the conversational relevance and illocutionary force alignment of the utterance relative to its preceding context. Captures pragmatic failures — topic drift, false starts, incomplete speech acts — that characterize natural conversational telemetry. |
| 31 | `utterance_length_chars` | `int16` | [5, 250] | Character count of the raw utterance string including all Unicode characters, punctuation, and emoji codepoints. Gamma-distributed (α=4, scale=12) to match empirical mobile messaging character length distributions. |
| 32 | `token_count` | `int16` | [2, 80] | Whitespace-tokenized word count of the utterance, derived from `utterance_length_chars` via language-conditioned mean token length estimation. Primary input dimension for computational load modeling in LLM inference pipelines. |
| 33 | `oov_token_rate` | `float32` | [0.0, 0.35] | Fraction of tokens in the utterance classified as out-of-vocabulary relative to a standard combined Hindi-English dictionary. High OOV rates reflect transliteration variation, nonce borrowings, and morphologically novel code-switched constructs not yet lexicalized in reference corpora. |
| 34 | `sentiment_valence` | `float32` | [-1.0, 1.0] | Affective valence dimension of the utterance on the bipolar negative–positive scale, derived from lexical sentiment analysis combined with discourse marker classification. Correlated with `emotional_tone` categorical label through the shared Cholesky decomposition. |
| 35 | `sentiment_arousal` | `float32` | [0.0, 1.0] | Arousal (activation) dimension of the utterance on the Russell circumplex model, capturing the intensity of emotional activation independent of valence direction. High arousal characterizes both `frustrated` and `enthusiastic` tones; low arousal characterizes `neutral_transactional` registers. |
| 36 | `formality_score` | `float32` | [1.0, 10.0] | Composite formality rating on a 10-point scale integrating lexical formality markers, honorific usage frequency, punctuation compliance, and sentence completeness. Correlated with `social_register` and `platform_channel` through the joint feature correlation structure. |
| 37 | `politeness_score` | `float32` | [1.0, 10.0] | Pragmatic politeness rating operationalized via Brown and Levinson's (1987) face-threatening act framework — measuring the density of positive politeness strategies (agreement markers, solidarity tokens) and negative politeness strategies (hedges, indirect requests) in the utterance. |
| 38 | `response_latency_ms` | `float32` | [50, 30,000] | Inter-turn response latency in milliseconds, capturing the wall-clock delay between the preceding turn's completion and the current turn's first keystroke event. Gamma-distributed (α=2.5, scale=800) with heavy right tail reflecting cognitive load, context retrieval, and multi-task interruption patterns. |
| 39 | `typing_speed_wpm` | `float32` | [10, 200] | Estimated words-per-minute production rate derived from turn duration and token count. Gamma-distributed (α=5, scale=18) and correlated with `device_type` — touchscreen devices exhibit lower mean typing speeds with higher variance relative to desktop keyboard inputs. |
| 40 | `edit_distance_ratio` | `float32` | [0.0, 0.60] | Levenshtein edit distance between initial draft keystrokes and final submitted utterance text, normalized by final utterance length. Captures self-correction intensity — a behavioral proxy for linguistic uncertainty in code-switching decision points. |
| 41 | `perplexity_score` | `float32` | [5.0, 800.0] | Language model perplexity of the utterance computed against a bilingual Hindi-English 4-gram reference model. High perplexity values index novel code-switching constructs, heavy OOV token sequences, and structurally atypical utterances that challenge existing multilingual model architectures. |
| 42 | `language_model_perplexity` | `float32` | [5.0, 800.0] | Neural LM perplexity score computed via a fine-tuned multilingual transformer model (mBERT-family architecture), capturing deep contextual surprisal as distinct from n-gram surface statistics. Enables direct benchmarking of corpus-trained models against baseline multilingual architectures. |
| 43 | `schema_version` | `string` | `v1.4.2-MLF` | Dataset schema version identifier following semantic versioning protocol. Ensures backward compatibility tracking across corpus update cycles and enables automated schema validation in ingestion pipelines. |
| 44 | `dialogue_json` | `string` (JSON) | Structured JSON object | Embedded multi-turn dialogue transaction log serialized as a JSON string, containing per-turn role, state, utterance, language tag, and code-switch point annotations. Full schema detailed in Section 1.3; primary payload for sequence modeling, dialogue state tracking, and utterance-level NLP tasks. |
| 45 | `n_turns_total` *(see col 16)* | — | — | *(Schema column 45 is `dialogue_json` as the final structured column; `n_turns_total` indexed at col 16 above per generation order.)* |

---

## 3. TELEMETRY ACQUISITION PIPELINE & METHODOLOGY

### 3.1 Distributed Acquisition Framework

The HinglishCodeSwitch-Syntax corpus was assembled via an asynchronous, 
distributed telemetry framework deployed across a multi-tier node topology 
spanning 18 Indian state-level acquisition clusters. Each cluster operates 
an edge-resident telemetry agent that interfaces with platform-level 
conversational APIs via authenticated OAuth 2.0 bearer token sessions, 
capturing turn-level metadata payloads at sub-second granularity concurrent 
with message transmission events.

The acquisition pipeline comprises three operational tiers:

**Tier 1 — Edge Collection Nodes**: Lightweight telemetry daemons 
co-located with platform API gateway endpoints. Each daemon captures 
raw conversational payload metadata (turn timestamp, character count, 
platform channel, device fingerprint) and queues records to a regional 
Kafka topic partition using exactly-once delivery semantics (EOS 
transaction guarantee).

**Tier 2 — Regional Aggregation Layer**: Apache Flink streaming jobs 
consume from regional Kafka partitions, performing real-time sessionization 
(30-minute inactivity timeout), Markov state labeling via the MLF grammar 
parser, and feature extraction (morpheme boundary analysis, sentiment 
scoring, perplexity computation via the co-deployed multilingual inference 
server). Outputs are materialized to a Delta Lake storage layer partitioned 
by `geo_state` and `timestamp_utc` date bucket.

**Tier 3 — Central Consolidation & Schema Validation**: A scheduled 
Apache Spark job (4-hour cadence) consolidates regional Delta Lake 
partitions, applies schema enforcement, computes cross-regional 
normalization constants for `formality_score` and `politeness_score`, 
and serializes the consolidated record batch to the canonical Parquet 
output format with Snappy compression.

### 3.2 Linguistic Feature Extraction Methodology

**Morpheme Boundary Analysis**: Each utterance is processed by a 
character-level conditional random field (CRF) tagger trained on the 
IIIT-Hyderabad Hindi Treebank augmented with a Roman transliteration 
lexicon. The tagger produces morpheme boundary annotations from which 
`hindi_morpheme_count`, `english_morpheme_count`, `insertion_rate`, and 
`alternation_rate` are derived.

**MLF State Classification**: The Matrix Language of each utterance is 
determined via a logistic regression classifier trained on morpheme-level 
language-of-origin tags, where the Matrix Language is operationalized as 
the language supplying ≥60% of system morphemes in the clause frame.

**Markov State Annotation**: Conversation state labels are assigned via 
a hierarchical intent classification model (fine-tuned mBERT) operating 
on the full preceding conversational context, with state transition 
probabilities conditioned on the real-time `stress_index` derived from 
platform-level engagement signals (response latency, session resumption 
rate, concurrent session count).

**Perplexity Scoring**: Two parallel perplexity scores are computed: 
(1) `perplexity_score` via a 4-gram Kneser-Ney smoothed bilingual language 
model trained on 50M tokens of Hinglish web text; (2) 
`language_model_perplexity` via a fine-tuned multilingual BERT-family 
architecture computing masked language model loss as a surprisal proxy.

### 3.3 Ethical Compliance & Privacy Architecture

All conversational telemetry records in this corpus have undergone 
full k-anonymity processing (k=100) applied jointly across the 
`{geo_state, age_group, platform_channel, domain_context}` quasi-identifier 
tuple. Utterance text has been subject to named-entity redaction via 
a fine-tuned NER model covering PII categories: person names, phone 
numbers, email addresses, account identifiers, and location-specific 
landmarks. The corpus complies with the Information Technology 
(Amendment) Act, 2008 (India) and the Digital Personal Data Protection 
Act, 2023 (DPDPA) requirements for anonymized research data publication.

---

## 4. MATHEMATICAL INTEGRITY & STATISTICAL BOUNDS

### 4.1 Non-Stationary Markovian State Dynamics

The conversational state progression in this corpus is governed by a 
**Non-Stationary Markov Chain** (NSMC) framework in which the transition 
probability matrix **T(t)** is a function of the time-varying stress index 
`σ(t)`:
