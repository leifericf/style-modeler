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

## Artifacts

This repository contains primary components:

### 1. Cross-Platform Style Profile Generator

**Purpose:** Creates the initial style profile artifacts from a corpus.

Use: `prompts/generate-style-profile.md`

### 2. Style Profile Update & Refinement Agent

**Purpose:** Updates the existing profile when:

-   New writing samples are added
-   A new platform is introduced
-   Recent writing needs higher weighting
-   Previous conclusions must be revised

This ensures the model remains coherent and current.

Use: `prompts/update-style-profile.md`

### 3. Source Manifest Template (Optional)

A lightweight YAML file for tracking:

-   Source platforms
-   File paths or URLs
-   Recency weighting
-   Update policy

Use this if your corpus becomes large or multi-source.

### 4. Interactive Corpus Builder (Optional)

If you don't have a large corpus yet, use the interactive prompt to generate a diverse set of short, high-signal writing samples quickly.

It will:

-   Ask you a sequence of short writing prompts (one at a time)
-   Save your answers into `sources/`
-   Update the profile using the existing update/generate prompts

### 5. Source Manifest Wizard (Optional)

If you want to add new URLs or local paths without hand-editing YAML, use:

-   `prompts/sources-manifest-wizard.md`

It will guide you to add sources and update `config/sources.yml` (creating it if missing, and validating/normalizing YAML formatting before writing).

If the agent has network access, it can also sanity-check whether pasted URLs are reachable.

### 6. Profile-Guided Authoring Agent (Optional)

Use `prompts/profile-guided-authoring.md` when you want to draft a new text from messy notes while staying faithful to your style artifacts. You can brain-dump in any order until you say `DONE`, then the agent will clean up spelling/grammar and structure the piece in your style (and offer a few low-deviation improvement suggestions).

### 7. Profile-Conditioned Drafting Agent (Optional)

Use `prompts/profile-conditioned-drafting.md` when you want a fresh draft written in your style with a measurable conformance self-check and a single revision pass.

### 8. Profile Bundle Packager (Optional)

Use `prompts/package-style-profile-bundle.md` when you want to distribute a profile to downstream consumers (humans or other AI agents) as either:

- a single zip bundle (`.styleprofile.zip`) that includes artifacts + an entrypoint prompt, or
- a single inline Markdown file (`.inline.md`) that can be copy/pasted into a prompt window.

## Output

The system produces a set of artifacts under `artefacts/`.

Per-language artifacts live in `artefacts/<lang>/` (e.g., `artefacts/en/`, `artefacts/no/`):

- `artefacts/<lang>/profile_summary.md`
- `artefacts/<lang>/examples.md`
- `artefacts/<lang>/generation_blocks.md`
- `artefacts/<lang>/metrics.json`
- `artefacts/<lang>/distributions.json`
- `artefacts/<lang>/revision-log.md`
- `artefacts/<lang>/diachronic.json` (only when timestamps support it)

If multiple languages are significant, a global layer may be emitted in `artefacts/global/`:

- `artefacts/global/global_profile.md`
- `artefacts/global/global_examples.md`
- `artefacts/global/global_metrics.json`
- `artefacts/global/cross_language_summary.md`
- `artefacts/global/revision-log.md`

Corpus metadata is written to `artefacts/corpus-metadata.md`.

Note: `artefacts/` is ignored by git by default (it may contain personal writing or derived snippets).

## Iterative Workflow

1.  Import corpus
2.  Generate profile
3.  Add new data
4.  Update profile
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

1. Create `config/sources.yml` (recommended: run `prompts/sources-manifest-wizard.md`).
2. Generate artifacts: run `prompts/generate-style-profile.md`.
3. Draft with the profile: run `prompts/profile-conditioned-drafting.md` (or `prompts/profile-guided-authoring.md`).
4. (Optional) Package the profile for sharing: run `prompts/package-style-profile-bundle.md`.

Notes: `config/sources.yml` and `sources/` are gitignored by default.

For the full setup/update/drafting flow, see `docs/workflows.md`.

## License
See `LICENSE` (MIT).
