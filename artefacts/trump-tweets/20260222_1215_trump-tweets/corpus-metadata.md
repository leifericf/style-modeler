# Corpus Metadata (trump-tweets)
- schema_version: 2
- project_slug: `trump-tweets`
- run_id: `20260222_1215_trump-tweets`
- generated_at_utc: `2026-02-22T12:15:06Z`
## Sources
- Ingested: `sources/trump-tweets/trump_tweets.csv`
- Used columns: `text` for writing; `datetime` (fallback `date`) for time slicing
- Excluded: rows where `is_retweet` is TRUE; additionally excluded any `text` that starts with `RT @`
## Preprocessing
- Unicode normalization: NFKC (measurement only)
- Line endings normalized to `\n`
- Measurement whitespace collapsed to single spaces; evidence snippets keep original punctuation/case (with redactions)
- Deduplication: exact match on measurement-normalized text (whitespace-collapsed)
## Volume
- total_rows: 56571
- excluded_retweets_flag: 9877
- excluded_retweets_prefix_guard: 127
- included_after_filters: 46182
- deduped_removed: 385
## Language
- en: samples=44867, tokens=912025
- unknown: samples=1315, tokens=4493

Significant languages (per spec thresholds): en

## Time
- Timestamp field coverage is assessed from `datetime` (ISO 8601) with fallback to `date`.
- Diachronic slicing uses timestamp terciles within each significant language when coverage is sufficient.

## Recency weighting
- Numeric metrics/distributions: unweighted (full usable corpus).
- Qualitative outputs (examples/anchors): prefer more recent items when selecting among many candidates (light bias), while keeping representative spread.

## Privacy / PII handling
- Evidence snippets redact: URLs -> `[REDACTED_URL]`, @handles -> `[REDACTED_HANDLE]`, emails/phones -> `[REDACTED]`.
- Snippets may be truncated with `...` to stay <= 240 characters.
