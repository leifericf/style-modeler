# StyleModeler

Corpus-driven writing style reverse-engineering and replication.

StyleModeler (formerly "Writing Style Modeler") is an iterative, corpus-driven system for reverse-engineering and replicating your writing style across platforms.

This project allows you to:

-   Analyze your writing across LinkedIn, Facebook, Twitter/X, blogs, or any other text source
-   Detect stylistic patterns, evolution, and cross-platform differences
-   Generate a structured, living Markdown style profile describing your writing
-   Iteratively refine the model as new writing samples are added
-   Produce high-fidelity stylistic imitations on new topics

## Design Principles

-   Corpus-driven (no guesswork)
-   No invented quotes
-   Explicit recency weighting
-   Cross-platform awareness
-   Deterministic mimicry rules
-   Structured Markdown output
-   Iterative refinement

## Why This Exists

Most AI "style prompts" are vague and unstable.

This system treats writing style as a structured, analyzable, reproducible model, not a mystical aesthetic.

It is designed for creators, researchers, founders, and builders who want precision, not fluff.

## Potential Use Cases

-   Auto-draft content in your own style and tone (emails, articles, social posts, etc.), then quickly review and edit before sending
-   Keep a consistent voice across platforms while adapting to each format
-   Speed up writing workflows by reusing a stable, documented style model
-   Track stylistic drift over time (and intentionally steer it)
-   Build a shareable reference for collaborators, editors, or future you

## Goals

The system is designed to:

1.  Reverse-engineer your writing style from real examples.
2.  Extract reproducible stylistic rules.
3.  Detect stylistic drift over time.
4.  Identify cross-platform adaptations.
5.  Maintain a living style model that improves as new data is added.
6.  Enable controlled, high-fidelity imitation.

This is not generic writing advice.
It is a structured linguistic modeling system.

## Artefacts

This repository contains primary components:

### 1. Cross-Platform Style Profile Generator

**Purpose:** Creates the initial style profile artefacts from a corpus.

Use: `prompts/generate-style-profile.md`

### 2. Project Sources Folder (Required)

Each profile is built from a single project folder under `sources/`:

- Create `sources/<project>/` and put all source material for that project inside it.
- You may use subfolders (e.g., `sources/facebook/posts.json`, `sources/facebook/comments.json`).

Each profile run writes to a new timestamped folder under `artefacts/` so previous runs are never overwritten:

- `artefacts/<project_slug>/<run_id>/...`

### 3. Interactive Corpus Builder (Optional)

If you don't have a large corpus yet, use the interactive prompt to generate a diverse set of short, high-signal writing samples quickly.

It will:

-   Ask you a sequence of short writing prompts (one at a time)
-   Save your answers into `sources/<project>/`
-   Generate a style profile from the collected samples

### 4. Profile-Guided Authoring Agent (Optional)

Use `prompts/profile-guided-authoring.md` when you want to draft a new text from messy notes while staying faithful to your style artefacts. You can brain-dump in any order until you say `DONE`, then the agent will clean up spelling/grammar and structure the piece in your style (and offer a few low-deviation improvement suggestions).

### 5. Profile-Conditioned Drafting Agent (Optional)

Use `prompts/profile-conditioned-drafting.md` when you want a fresh draft written in your style with a measurable conformance self-check and a single revision pass.

### 6. Profile Bundle Packager (Optional)

Use `prompts/package-style-profile-bundle.md` when you want to distribute a profile to downstream consumers (humans or other AI agents) as either:

- a single zip bundle (`.styleprofile.zip`) that includes artefacts + an entrypoint prompt, or
- a single inline Markdown file (`.inline.md`) that can be copy/pasted into a prompt window.

## Output

The system produces a new, timestamped run folder under `artefacts/<project_slug>/<run_id>/` on every generation.

Per-language artefacts live in `artefacts/<project_slug>/<run_id>/<lang>/` (e.g., `artefacts/facebook/20260220_1215_facebook/en/`):

- `profile_summary.md`
- `examples.md`
- `generation_blocks.md`
- `metrics.json`
- `distributions.json`
- `revision-log.md`
- `diachronic.json` (only when timestamps support it)

If multiple languages are significant, a global layer may be emitted in `artefacts/<project_slug>/<run_id>/global/`:

- `global_profile.md`
- `global_examples.md`
- `global_metrics.json`
- `cross_language_summary.md`
- `revision-log.md`

Corpus metadata is written to `artefacts/<project_slug>/<run_id>/corpus-metadata.md`.

Note: `artefacts/` and `sources/` are ignored by git by default.

## Iterative Workflow

1.  Import corpus
2.  Generate profile
3.  Add new data
4.  Re-run profile generation
5.  Repeat

The model improves over time.

## Documentation

Canonical docs live in `docs/`.

- Start here: `docs/workflows.md`
- Schema + file contracts: `docs/style-profile-spec.md`
- Bundle packaging: `docs/style-profile-bundle-spec.md`
- Shared method: `docs/style-profile-method.md`
- Success criteria: `docs/quality-gates.md`
- Manual end-to-end check: `docs/smoke-test.md`

## Quick Start

1. Create a project folder under `sources/` (e.g., `sources/facebook/`) and add your files.
2. Generate a profile run: run `prompts/generate-style-profile.md` and select the project.
3. Draft with the profile: run `prompts/profile-conditioned-drafting.md` (or `prompts/profile-guided-authoring.md`).
4. (Optional) Package the profile for sharing: run `prompts/package-style-profile-bundle.md`.

For the full setup/regeneration/drafting flow, see `docs/workflows.md`.

## License
See `LICENSE` (MIT).
