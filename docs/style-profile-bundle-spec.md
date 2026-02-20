# Style Profile Bundle Spec

This document defines how to package a StyleModeler profile into a single distributable unit for downstream consumers (humans and AI agents).

Two formats are supported:

- A) Zip bundle: one archive containing the profile artifacts plus an entrypoint prompt.
- B) Inline bundle: one single Markdown file that contains everything needed (copy/paste friendly).

The inline bundle is the simplest for consumers; the zip bundle is best for tool integrations that can unzip and read files.

## Goals

- One-step consumption for downstream agents.
- Clear, explicit loading order.
- Works with or without filesystem access.
- Does not require any repository context beyond the bundle itself.

## Bundle Schema Versioning

- `package_schema_version`: version of this bundle container spec.
- `schema_version` inside `artefacts/**`: the StyleModeler profile schema version (currently `2`).

## A) Zip Bundle

### File extension

Use `.styleprofile.zip` (a regular zip archive; extension is convention).

### Required contents

- `manifest.json`
- `artefacts/` (see below)
- `prompts/entrypoint-stylize.md`

### Optional contents

- `prompts/entrypoint-draft.md`
- `README.md`

### Required artifacts (recommended minimum)

Per language `<lang>`:

- `artefacts/<lang>/generation_blocks.md` (primary)
- `artefacts/<lang>/metrics.json` (numeric targets)
- `artefacts/<lang>/profile_summary.md` (secondary)

Optional (helpful when present):

- `artefacts/<lang>/distributions.json`
- `artefacts/<lang>/examples.md` (calibration only; never quote back)
- `artefacts/corpus-metadata.md` (ingestion notes)

### manifest.json contract (minimum)

`manifest.json` MUST include:

- `package_schema_version`: number (currently `1`)
- `profile_schema_version`: number (expected `2`)
- `profile_id`: string (stable identifier; choose something distribution-friendly)
- `profile_name`: string
- `created_at`: ISO 8601 timestamp
- `languages`: array of language ids (e.g. `["en"]`)
- `default_language`: language id
- `entrypoints`: object mapping logical names to bundle-relative paths
  - at minimum: `{ "stylize": "prompts/entrypoint-stylize.md" }`
- `artifacts_by_language`: object keyed by language id with paths to the key artifacts
- `files`: array of file descriptors for integrity and selective loading
  - each descriptor: `{ "path": "...", "bytes": <int>, "sha256": "...", "media_type": "..." }`

Consumers SHOULD:

1) unzip to a temp directory
2) read `manifest.json`
3) choose a language (`default_language` if none specified)
4) load `generation_blocks.md` + `metrics.json` (+ optional `profile_summary.md`)
5) run the `entrypoint-stylize.md` prompt with their input text

## B) Inline Bundle (Single Markdown File)

### Purpose

The inline bundle is designed for environments where the downstream agent cannot read from the filesystem. A human can paste one file into an AI chat, then provide input text.

### Format

The inline bundle is a single Markdown file containing:

- A small `manifest` block (JSON; recommended)
- The entrypoint prompt text
- Embedded artifacts as distinct, parseable sections

Use explicit file markers so both humans and tools can find sections:

```markdown
<!-- BEGIN_FILE: artefacts/en/generation_blocks.md -->
```markdown
...file contents...
```
<!-- END_FILE: artefacts/en/generation_blocks.md -->
```

The embedded content MUST be verbatim.

### Inline manifest guidance

The inline manifest is primarily for humans and light tooling.

- It SHOULD include: `package_schema_version`, `profile_schema_version`, `profile_id`, `profile_name`, `created_at`, `languages`, `default_language`, and `entrypoints`.
- It MAY include a `files` array with hashes/sizes, but hashes are OPTIONAL for inline bundles.

### Consumer guidance

The entrypoint prompt inside the inline bundle MUST instruct the downstream agent to:

- locate and use the embedded files by their markers
- follow the same loading precedence as the zip bundle
- return ONLY the stylized output (unless explicitly asked otherwise)

## Privacy guidance

Even if the artifacts are intended to be PII-safe, bundles are still derived from personal/organizational writing. Treat bundles as potentially sensitive and avoid publishing them publicly.
