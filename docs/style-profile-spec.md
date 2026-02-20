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

## Data Model (Conceptual)

v2 artifacts are split by language. Each language profile contains:

- *Human-readable* summaries and generation blocks (Markdown).
- *Machine-consumable* metrics and distributions (JSON).

Global artifacts exist only when multiple languages are significant, and store cross-language priors.
