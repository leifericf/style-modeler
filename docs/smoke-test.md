# Smoke Test

This is a manual smoke test to validate the end-to-end workflow:
- sources folder -> profile artefacts -> profile-conditioned drafting -> conformance report

## Setup

1) Ensure `fixtures/smoke-corpus/en/samples.txt` exists (tracked in the repo).

2) Create a smoke project under `sources/` and copy the fixture corpus into it:

`mkdir -p sources/smoke && cp fixtures/smoke-corpus/en/samples.txt sources/smoke/samples.txt`

## Step 1: Generate artefacts

Run `prompts/generate-style-profile.md` in an agent that has repo access.

When prompted to select a project folder under `sources/`, choose `smoke`.

Expected outputs (written to disk):

- `artefacts/smoke/<run_id>/en/profile_summary.md`
- `artefacts/smoke/<run_id>/en/examples.md`
- `artefacts/smoke/<run_id>/en/generation_blocks.md`
- `artefacts/smoke/<run_id>/en/metrics.json`
- `artefacts/smoke/<run_id>/en/distributions.json`
- `artefacts/smoke/<run_id>/en/revision-log.md`
- `artefacts/smoke/<run_id>/corpus-metadata.md`

Accept criteria:

- Files exist at the expected paths.
- `artefacts/smoke/<run_id>/en/metrics.json` contains `schema_version: 2` and non-null counts.
- At least a small set of numeric `targets` exist when the slice is stable.

## Step 2: Draft with conformance

Run `prompts/profile-conditioned-drafting.md` and request a short, concrete text.

Example request (edit as desired):

- language: `en`
- genre: "internal technical memo"
- audience: "engineers"
- purpose: "explain a small design change and tradeoffs"
- length: "~300 words"

Accept criteria:

- Output includes a draft and a conformance report.
- Conformance report lists targets used (with units) and observed values.
- Report includes `pass`/`fail`/`unknown` per target.
- If a key target fails, the agent revises once and reports that it revised.

## Step 3: Package for downstream consumption (optional)

Run `prompts/package-style-profile-bundle.md`.

Recommended for the smoke test: choose the inline bundle option (single Markdown file).

Accept criteria:

- One new file is produced under `bundles/` with suffix `.inline.md`.
- The file contains an embedded entrypoint prompt (`prompts/entrypoint-stylize.md`).
- The file contains embedded artefacts for at least:
  - `artefacts/smoke/<run_id>/en/generation_blocks.md`
  - `artefacts/smoke/<run_id>/en/metrics.json`
  - `artefacts/smoke/<run_id>/en/profile_summary.md`

Optional checks (nice-to-have):

- The inline bundle includes `manifest.json` metadata.
- Embedded file markers use `<!-- BEGIN_FILE: ... -->` and `<!-- END_FILE: ... -->`.
