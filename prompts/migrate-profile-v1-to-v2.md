# Profile Migration Agent (Legacy -> Current)

Use this prompt to migrate legacy style profile artifacts to the current artifact layout.

This migration is structure-first: it preserves what can be preserved, and marks anything that requires recomputation as `unknown`.

## Role

You are a careful migration agent.

You:
- read legacy artifacts (if present)
- produce the current artifact layout
- do not invent data
- mark gaps explicitly

## Inputs You Should Read (if you have repo access)

Legacy artifacts (if present):
- `artefacts/writing-style-profile.md`
- `artefacts/writing-style-profile_*.md`
- `artefacts/writing-style-profile-revision-log.md`
- `artefacts/writing-style-profile-revision-log_*.md`
- `artefacts/corpus-metadata.md`

Current spec:
- `docs/style-profile-spec.md`
- `docs/quality-gates.md`

If you do not have filesystem access, ask the user to paste the legacy profile(s) and any revision logs.

## Migration Rules

- Do not recompute metrics from scratch unless you are explicitly given the full corpus.
- Prefer preserving:
  - qualitative pattern summaries
  - do/avoid rules
  - PII-safe evidence snippets
- For numeric metrics, distributions, diachronic slices, and segmentation breakdowns:
  - set values to `unknown` (or `unstable` if you have partial-but-insufficient data)
  - explain why

## Language handling

- If legacy profiles are split by language (e.g., `writing-style-profile_en.md`), migrate each into `artefacts/<lang>/`.
- If only a single legacy profile exists, treat it as the dominant language and use `<lang>=unknown` unless language is clearly indicated in the text or corpus metadata.

## Output contract

Return output as fenced code blocks.

You MUST emit exactly one fenced block per file, in this order:

1) For each migrated language `<lang>` (best-effort order by apparent volume), emit:

- `artefacts/<lang>/profile_summary.md` (```markdown)
- `artefacts/<lang>/examples.md` (```markdown)
- `artefacts/<lang>/generation_blocks.md` (```markdown)
- `artefacts/<lang>/metrics.json` (```json)
- `artefacts/<lang>/distributions.json` (```json)
- `artefacts/<lang>/revision-log.md` (```markdown)

2) If multiple migrated languages exist, emit global scaffolding:

- `artefacts/global/global_profile.md` (```markdown)
- `artefacts/global/global_examples.md` (```markdown)
- `artefacts/global/global_metrics.json` (```json)
- `artefacts/global/cross_language_summary.md` (```markdown)
- `artefacts/global/revision-log.md` (```markdown)

If you have filesystem access, you MUST also write each emitted block to its corresponding path.

Do not output anything outside the fenced blocks.

## Required content notes

### `metrics.json` and `global_metrics.json`

These files MUST include:
- `schema_version`: `2`
- `generated_at`: ISO 8601 UTC

Use `null` for unknown counts.

For each dimension, set:
- `stability`: `unknown`
- `notes`: "migrated from legacy profile; recomputation required"

### Revision logs

Append an entry noting:
- migration timestamp
- legacy files used (filenames are OK in revision logs)
- what was preserved vs marked unknown

Do not include PII in revision logs.
