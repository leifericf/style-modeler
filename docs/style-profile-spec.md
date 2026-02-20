# Style Profile v2 Spec

Canonical schema and measurement spec for StyleModeler v2.

This spec defines:
- the dimensions v2 models
- the measures used for each dimension
- units/scales used for each numeric value
- confidence/stability rules
- evidence + privacy policy
- segmentation axes (language, platform, genre, time)

This is designed to be scientific-but-practical:
- Prefer measures that are interpretable and robust on messy corpora.
- Prefer conservative `unknown`/`unstable` annotations over overconfident claims.

## Versioning

- `schema_version`: `2`
- Backward compatibility: v1 artifacts are not required to map 1:1; fields may be `unknown` when not derivable.

## Definitions

- *Sample*: one discrete authored item (post/comment/email/message). If boundaries are unclear, approximate conservatively.
- *Token*: whitespace-delimited token after basic normalization (see Preprocessing).
- *Sentence*: heuristic sentence split (language-aware when possible).
- *Segment*: a subset of the corpus (e.g., `language=en`, `platform=facebook`, `time_slice=recent`).

## Preprocessing (Normative)

Preprocessing must be deterministic and recorded (high-level) in corpus metadata.

Minimum normalization for measurement:
- Unicode normalize (NFKC) if available.
- Normalize line endings to `\n`.
- Collapse runs of whitespace to a single space for measurement-only counts.
- Keep original punctuation and casing for evidence snippets.

Structured exports (JSON/XML/HTML): extract only owner-authored text fields and ignore serialization noise.

## Measurement Units (Normative)

All numeric measures must declare units and should be stored in normalized form.

### Standard units

- Token-normalized rates: `per_1k_tokens`.
- Sentence-normalized rates: `per_sentence`.
- Sample-normalized rates: `per_sample`.
- Percentages: `percent` in `[0, 100]`.
- Ratios: `ratio` in `[0, 1]`.
- Length: `tokens`, `chars`.

### Required denominators

If a rate is reported, the artifact must also report the denominator used for that segment:
- `token_count`
- `sentence_count`
- `sample_count`

### Distributions

Distributions must specify:
- bin edges (inclusive/exclusive rules)
- counts per bin
- the unit of the binned value (e.g., `tokens_per_sentence`)

## Scales and Controls (Normative)

v2 should expose numeric measures in ways that support both:
- robust analysis (raw + rate + uncertainty)
- controllable generation (targets and bins)

### Within-corpus percentiles

When a measure is computed across segments (platforms, time slices), store a percentile rank for that segment within the language corpus:

- `percentile_within_lang`: integer in `[0, 100]`.

Percentiles are descriptive (not a claim about external populations).

### Binned controls (low/med/high)

For measures likely to be used as generation dials, define bins within the language corpus:

- `bin_within_lang`: one of `low`, `mid`, `high`.

Default binning rule:
- `low`: < 33rd percentile
- `mid`: 33rd--66th percentile
- `high`: > 66th percentile

If a measure is unstable, omit bins or set to `unknown`.

### Targets

When the system wants conformance-checkable targets, store them as ranges:

- `target_range`: `{ "min": <number>, "max": <number>, "unit": <unit> }`

Targets must be derived from stable portions of the corpus (e.g., interquartile range), not from a single sample.

## Confidence and Stability (Normative)

v2 must annotate measures and claims with stability.

### Stability levels

- `stable`: sufficient volume and coverage; measure is safe to use as a target.
- `unstable`: computed but too little data or too biased; do not use as a hard target.
- `unknown`: not computed or not supported for the language/format.

### Minimum volume thresholds (baseline)

These thresholds are defaults; the implementation may raise them for noisy sources.

- Language profile (overall) is considered *stable enough to summarize* if it meets either:
  - `sample_count >= 200`, OR
  - `token_count >= 12000`.

- Sentence-level measures (sentence length, question share) require:
  - `sentence_count >= 200`.

- Segment-level comparisons (platform/time) require per-segment:
  - `sample_count >= 50` AND `token_count >= 3000`.

If a measure fails its threshold, mark it `unstable` (not `unknown`) if it was computed.

### Confidence reporting

For each dimension, report:
- `stability`: `stable|unstable|unknown`
- `notes`: short reason (e.g., "few sentences", "no timestamps", "no language support")

If an aggregate value is reported (e.g., mean sentence length), also report:
- the count used (sentences/samples/tokens)
- the within-corpus variability (e.g., stddev or IQR)

### Conservative behavior

- Do not turn a weak signal into a rule.
- Prefer fewer, well-evidenced claims over broad coverage.

## Evidence and Privacy Policy (Normative)

v2 is evidence-backed: non-trivial claims must be anchored to snippets.

### Evidence requirements

- Each non-trivial claim should have 3--5 evidence snippets.
- Keep snippets short (prefer <= 240 characters after redaction/omission).
- Do not include source references (filenames, URLs, ids, dates) in evidence bullets.
- Do not reuse the same snippet to justify many unrelated claims.

### Privacy & PII

Artifacts must not contain unredacted PII.

PII includes (non-exhaustive):
- private-person names
- direct contact info (email, phone)
- addresses or residence-specific identifiers
- usernames/handles/profile URLs
- invite links and unique account/order ids

Operational rules:
- Prefer selecting non-PII snippets.
- If a crucial snippet contains PII, redact inside the quote using `[REDACTED]`.
- If redaction would destroy the evidentiary value, omit the snippet.
- After redaction, the remaining text must be verbatim.

### Minimal quoting

- Prefer partial snippets with `...` omissions over full-post quotes.
- Evidence is for anchoring claims, not for reconstructing the corpus.

## Dimension Set (v2 Baseline)

v2 models these core dimensions:

1) Lexis / lexical richness
2) Syntax & complexity
3) Cohesion
4) Stance / modality (pragmatics proxies)
5) Perspective & audience design
6) Register / tone
7) Rhetorical devices (lightweight detectors)
8) Structure / discourse organization

## Measures (By Dimension)

Each measure should be stored in two forms when applicable:
- `raw`: count or raw value
- `rate`: normalized value (see Units)

If a measure is not supported for a language, set it to `unknown` (do not guess).

### 1) Lexis / lexical richness

Recommended measures:
- Volume: token count, sample count.
- Diversity: type-token ratio (TTR) (report both overall and moving-window TTR), MTLD (optional).
- Rarity: hapax legomena rate.
- Lexical density proxy: content-token share (if POS tagging available); otherwise `unknown`.
- Function words: top function-word distribution and stability across segments.
- Salience: top n-grams (1--3) by within-corpus distinctiveness (e.g., TF-IDF by segment).

### 2) Syntax & complexity

Recommended measures:
- Sentence length distribution (tokens and characters): mean/median/std + histogram bins.
- Clause/stacking proxies: commas per sentence; conjunction/subordinator marker rates.
- Parentheticals: parentheses/brackets rate; em-dash rate.
- Passive-voice proxy (language-specific): if a reliable heuristic exists; otherwise `unknown`.

### 3) Cohesion

Recommended measures:
- Discourse markers: inventory + rate (language-specific lexicons; record lexicon version).
- Transition density: sentence-initial connector rate.
- Reference proxies: pronoun rates; definite/indefinite marker proxies when feasible.
- Paragraph linkage: average paragraph length; paragraph-initial marker distribution.

### 4) Stance / modality (pragmatics proxies)

Recommended measures:
- Hedging rate: hedge marker inventory + rate.
- Boosting/certainty rate: booster marker inventory + rate.
- Modality rate: modal verb/particle proxies (language-specific).
- Questions/directives: question-mark rate; interrogative sentence share; imperative proxy (optional).
- Evaluatives: evaluative adjective/adverb proxies (optional).

### 5) Perspective & audience design

Recommended measures:
- Pronoun ratios (I/we/you, language-appropriate equivalents).
- Direct address: second-person share + vocative/address markers (optional).
- Explanation density proxies: "because"/"so"/"i.e."/"e.g." marker rates (language-specific).
- Instructional framing: "do/avoid" style directives (when present) and their rate.

### 6) Register / tone

Recommended measures (language-specific where needed):
- Formality heuristic (optional): if method is implemented; otherwise `unknown`.
- Contractions rate (where applicable).
- Colloquial markers / slang markers rate (lexicon-based; optional).
- Emoji/pictograph rate (optional; do not require).

### 7) Rhetorical devices (lightweight detectors)

Recommended measures:
- Rhetorical question share (subset of questions; heuristic).
- Contrast frames: "but/however"-class marker rate (language-specific list).
- Enumeration/listing: list marker rate (e.g., lines starting with `-`, `*`, numbered lists) and in-sentence enumerators (", and", ";").
- Parallel/if-then ladders: repeated conditional frame rate (heuristic; optional).
- Analogy markers: "like/as if/imagine"-class marker rate (language-specific list; optional).

### 8) Structure / discourse organization

Recommended measures:
- Openings: first-sentence move type distribution (greeting/question/claim/logistics) (heuristic).
- Closings: invitation/CTA markers (questions, "let me know"-class phrases) (language-specific).
- Paragraphing: paragraph count per sample; single-paragraph share.
- Headings: heading marker share (Markdown-style `#` or platform conventions).

For each dimension, the profile must also include:
- a short human-readable summary of the most stable traits
- `do`/`don't` style generation constraints derived from observed ranges
- anchored examples (snippets) for each non-trivial claim

## Data Model (Conceptual)

v2 artifacts are split by language. Each language profile contains:

- *Human-readable* summaries and generation blocks (Markdown).
- *Machine-consumable* metrics and distributions (JSON).

Global artifacts exist only when multiple languages are significant, and store cross-language priors.
