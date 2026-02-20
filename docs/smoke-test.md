# Smoke Test

This is a manual smoke test to validate the end-to-end workflow:
- sources manifest -> profile artifacts -> profile-conditioned drafting -> conformance report

## Setup

1) Copy the tracked smoke config into your local manifest:

`cp config/sources-smoke-example.yml config/sources.yml`

(`config/sources.yml` is gitignored.)

2) Ensure `fixtures/smoke-corpus/en/samples.txt` exists (tracked in the repo).

## Step 1: Generate artifacts

Run `prompts/generate-style-profile.md` in an agent that has repo access.

Expected outputs (written to disk):

- `artefacts/en/profile_summary.md`
- `artefacts/en/examples.md`
- `artefacts/en/generation_blocks.md`
- `artefacts/en/metrics.json`
- `artefacts/en/distributions.json`
- `artefacts/en/revision-log.md`
- `artefacts/corpus-metadata.md`

Accept criteria:

- Files exist at the expected paths.
- `artefacts/en/metrics.json` contains `schema_version: 2` and non-null counts.
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
