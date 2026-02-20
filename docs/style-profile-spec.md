# Style Profile Spec

Canonical schema and measurement spec for StyleModeler.

This spec defines:
- the dimensions modeled
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

The schema should expose numeric measures in ways that support both:
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

The schema must annotate measures and claims with stability.

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

The schema is evidence-backed: non-trivial claims must be anchored to snippets.

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

## Segmentation Axes (Normative)

The system may compute metrics for segments when metadata exists or is inferable.

### Language (always)

- Always partition the corpus by language.
- Per-language artifacts must be strictly monolingual.
- Treat a language as *significant* only if it meets both:
  - share >= 10% of usable tokens, AND
  - size >= 200 samples OR >= 12000 tokens.

### Source / platform (often)

If sources have platform labels, compute per-platform segments. At minimum, store:
- per-platform volume (samples/tokens)
- key measure deltas vs the language overall

If platform segmentation is too sparse (fails stability thresholds), mark it `unstable`.

### Genre / register (optional)

If genre/register metadata exists (explicit labels or strong inference), compute per-genre segments.

If missing, do not invent genre labels; leave as `unknown`.

### Time slices (optional; requires timestamps)

If timestamps exist for a meaningful share of samples, compute time slices.

Default slicing:
- `early`, `middle`, `recent` by timestamp terciles within each language.

Recency weighting (when producing "current" summaries):
- weight `recent` slice 2x.

Recency weighting must be explicit in outputs:
- By default, numeric metrics/distributions describe the full unweighted language corpus.
- If any "current" (recency-biased) summary or target is produced, it must be clearly labeled and the weighting method recorded in `artefacts/corpus-metadata.md`.

If timestamps are absent or too sparse, time-slice segmentation must be `unknown`.

## Dimension Set (Schema Baseline)

The schema models these core dimensions:

1) Lexis / lexical richness
2) Syntax & complexity
3) Cohesion
4) Stance / modality (pragmatics proxies)
5) Perspective & audience design
6) Register / tone
7) Rhetorical devices (lightweight detectors)
8) Structure / discourse organization

This list is the baseline dimension set referenced in `wip/style-profile-v2-plan.md` Phase 1.

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

Artifacts are split by language. Each language profile contains:

- *Human-readable* summaries and generation blocks (Markdown).
- *Machine-consumable* metrics and distributions (JSON).

Global artifacts exist only when multiple languages are significant, and store cross-language priors.

## Artifact Layout and File Contracts (Normative)

Artifact root: `artefacts/`.

### Per-language artifacts

Per-language artifacts live in `artefacts/<lang>/` where `<lang>` is ISO 639-1 lowercase when possible (e.g., `en`, `no`).

Required files:

#### `artefacts/<lang>/profile_summary.md`

Human-readable summary for the language. Must include:
- language id and `schema_version`
- corpus/segment coverage notes (what is included/excluded)
- per-dimension summary bullets, with stability annotations
- a short "do/avoid" list derived from stable traits
- limitations (unknown/unstable dimensions)

#### `artefacts/<lang>/metrics.json`

Machine-consumable metrics and targets. Must include at minimum:

- `schema_version`: number (must be `2`)
- `language`: string
- `generated_at`: ISO 8601 UTC timestamp
- `counts`: `{ sample_count, token_count, sentence_count }`
- `dimensions`: object keyed by dimension id; each contains:
  - `stability`: `stable|unstable|unknown`
  - `notes`: short string
  - `measures`: numeric fields with `{ raw, rate, unit, denom }` where applicable
  - optional `targets`: target ranges used for conformance (see Scales and Controls)

If a measure is `unknown`, omit `raw/rate` and store `{ "status": "unknown", "reason": "..." }`.

#### `artefacts/<lang>/distributions.json`

Machine-consumable distributions. Must include:
- `schema_version`: `2`
- `language`
- `generated_at`
- `counts` (same contract as metrics)
- `distributions`: object of named distributions; each distribution includes:
  - `unit`
  - `bin_edges`
  - `bin_counts`
  - `notes` (optional)

#### `artefacts/<lang>/examples.md`

PII-safe evidence anchors and representative snippets. Must include:
- sections per dimension
- labeled snippets that demonstrate each non-trivial claim
- no source references (no URLs/filenames/ids/dates)

#### `artefacts/<lang>/generation_blocks.md`

Reusable prompt blocks for generation. Must include:
- a compact "style card" (derived from stable measures/targets)
- do/avoid constraints as imperative rules
- a small set of short "anchor" examples (PII-safe)
- explicit instructions for conformance self-check behavior when numeric targets exist

#### `artefacts/<lang>/diachronic.json` (optional)

Only emit when timestamps exist at sufficient coverage. Must include:
- `schema_version`: `2`
- `language`
- slice definitions (e.g., early/middle/recent) and boundaries
- per-slice counts
- per-slice key measures and their stability

### Global artifacts (multilingual only)

Emit `artefacts/global/` only when 2+ languages are significant.

#### `artefacts/global/global_profile.md`

Human-readable cross-language priors. Must include:
- `schema_version`
- list of languages included
- stable cross-language traits (high-level only)
- explicit mapping notes: what differs by language and should not be treated as global

#### `artefacts/global/global_metrics.json`

Machine-consumable priors and shared targets. Must include:
- `schema_version`: `2`
- `generated_at`
- `languages`: list
- `priors`: high-level targets/ranges that are stable across languages

Global priors must not conflict with per-language targets; per-language wins on conflict.

#### `artefacts/global/global_examples.md`

PII-safe cross-language anchor snippets (short) that demonstrate shared voice traits.

If cross-language anchoring is not PII-safe, omit examples and record the limitation.

#### `artefacts/global/cross_language_summary.md`

Comparative report across languages. Must include:
- which dimensions are stable across languages vs language-specific
- notable adaptations (e.g., tone/formality shifts)
- explicit cautions where metrics are not comparable cross-language

### Revision history (recommended)

Revision logs are written alongside artifacts so updates remain auditable without diffing large JSON blobs.

Emit one revision log per language:

#### `artefacts/<lang>/revision-log.md`

Append-only log. Each entry must include:
- `generated_at` (ISO 8601 UTC)
- `schema_version`
- sources included (ids/platform labels; no sensitive paths)
- high-level changes since previous run (added/removed sources, key metric shifts)
- notes about stability changes (e.g., a dimension moved from `unstable` to `stable`)

If multilingual global artifacts exist, optionally also emit:

#### `artefacts/global/revision-log.md`

Append-only log capturing cross-language changes (languages included, global priors changes).

Do not paste large diffs or full metric dumps into revision logs; keep them concise and comparative.
