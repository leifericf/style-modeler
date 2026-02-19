# StyleModeler

Corpus-driven writing style reverse-engineering and replication.

StyleModeler (formerly "Writing Style Modeler") is an iterative, corpus-driven system for reverse-engineering and replicating your writing style across platforms.

This project allows you to:

-   Analyze your writing across LinkedIn, Facebook, Twitter/X, blogs, or any other text source
-   Detect stylistic patterns, evolution, and cross-platform differences
-   Generate a structured, living Markdown style blueprint describing your writing
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

This repository contains six primary components:

### 1. Cross-Platform Style Blueprint Generator

**Purpose:** Creates the initial:

> Writing Style Blueprint

This is used when starting from scratch with a new corpus.

### 2. Writing Style Blueprint Update & Refinement Agent

**Purpose:** Updates the existing blueprint when:

-   New writing samples are added
-   A new platform is introduced
-   Recent writing needs higher weighting
-   Previous conclusions must be revised

This ensures the model remains coherent and current.

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
-   Update the blueprint using the existing update/generate prompts

### 5. Source Manifest Wizard (Optional)

If you want to add new URLs or local paths without hand-editing YAML, use:

-   `prompts/sources-manifest-wizard.md`

It will guide you to add sources and update `config/sources.yml` (creating it if missing, and validating/normalizing YAML formatting before writing).

If the agent has network access, it can also sanity-check whether pasted URLs are reachable.

### 6. Blueprint-Guided Authoring Agent (Optional)

Use `prompts/blueprint-guided-authoring.md` when you want to draft a new text from messy notes while staying faithful to your Writing Style Blueprint. You can brain-dump in any order until you say `DONE`, then the agent will clean up spelling/grammar and structure the piece in your style (and offer a few low-deviation improvement suggestions).

## Output

The system produces a structured Markdown document titled `Writing Style Blueprint`.

If the corpus contains a significant amount of writing in multiple languages, the generator/update prompts should emit one strictly monolingual blueprint per language (e.g. `artefacts/writing-style-blueprint_en.md`, `artefacts/writing-style-blueprint_no.md`) so stylistic differences by language are captured separately. If other languages appear only in small amounts, the prompts should mention them under data limitations instead of creating separate blueprints.

The blueprint includes:

-   Vocabulary & word choice patterns
-   Sentence construction & rhythm
-   Structural patterns
-   Rhetorical devices
-   Argumentation style
-   Emotional tone & character
-   Values & subtext
-   Cross-platform differences
-   Evolution over time
-   Style vector summary
-   Mimicry blueprint

Revision history is stored separately in:

-   `artefacts/writing-style-blueprint-revision-log.md`

In multilingual mode, revision logs are also split per language (e.g. `artefacts/writing-style-blueprint-revision-log_en.md`).

The result is a deterministic style model that can be reused for imitation.

## Iterative Workflow

1.  Import corpus
2.  Generate blueprint
3.  Add new data
4.  Update blueprint
5.  Repeat

The model improves over time.

## Quick Start

### Step 0: Create Your Source Manifest

1.  Ensure you have a `config/sources.yml` (choose one):
    -   (Recommended) Run `prompts/sources-manifest-wizard.md` to create/update `config/sources.yml`.
    -   Or copy `config/sources-example.yml` to `config/sources.yml` and edit it manually.
2.  (Optional) Add or remove sources. By default, `config/sources.yml` can read from the repo-local `sources/` folder.

Note: `config/sources.yml` is ignored by git by default to avoid accidentally committing personal information.

Note: The prompts emit an updated `config/sources.yml` as part of their output (timestamps, newest-sample info). If you're using an AI agent with repo access, it should write that YAML directly to `config/sources.yml` each run.

If you give the agent new source locations (file paths / URLs), it should also add them to `config/sources.yml`.

If you're running the prompts in a chat-only interface (no filesystem access), you'll need to copy/paste the emitted YAML into `config/sources.yml` manually.

### Step 0.5: Add Local Text Files

1.  Put writing samples in `sources/` (for example, `sources/linkedin.txt`, `sources/blog.txt`). These can be plain text, or structured exports (e.g., JSON/XML/HTML) as long as they contain your original writing.
2.  Keep those files local. The `sources/` directory is ignored by git by default.

### Step 0.6 (Optional): Run the Interactive Corpus Builder

If you don't have much writing yet (or you want to fill gaps), run:

-   `prompts/interactive-corpus-builder.md`

The agent will elicit short samples, write them into `sources/`, and then generate/update:

-   `artefacts/writing-style-blueprint.md`
-   `artefacts/writing-style-blueprint-revision-log.md`
-   `config/sources.yml`

This workflow can create/update the blueprint directly, so you can skip Step 1/2 if you use it.

### Step 1: Generate Initial Blueprint

1.  Ensure `config/sources.yml` points at your writing (local files and/or URLs).
2.  Run the **Cross-Platform Style Blueprint Generator** prompt (`prompts/generate-style-blueprint.md`). The agent should read `config/sources.yml` and ingest the corpus from disk/URLs.
3.  If you're using an AI agent with repo access, it should write:

    -   `artefacts/writing-style-blueprint.md`
    -   `artefacts/writing-style-blueprint-revision-log.md`
    -   `config/sources.yml`

    If it only prints the fenced blocks, ask it to save them to those paths.

### Step 2: Add New Writing Samples

When you have more writing (new LinkedIn posts, new platform, etc.):

1.  Add the new material to disk/URLs referenced by `config/sources.yml`.
2.  Run the **Writing Style Blueprint Update & Refinement Agent** prompt (`prompts/update-style-blueprint.md`).
3.  If you mention new sources in the prompt (new file paths / URLs), the agent should add them to `config/sources.yml` automatically.
4.  If you're using an AI agent with repo access, it should update `artefacts/writing-style-blueprint.md`, `artefacts/writing-style-blueprint-revision-log.md`, and `config/sources.yml` for you.
5.  Review `artefacts/writing-style-blueprint-revision-log.md` for what changed.

### Step 3: Generate Stylized Writing

After analysis:

1.  Provide a new topic.
2.  Specify the target platform (optional).
3.  Ask for controlled imitation.
4.  The system generates a post following extracted rules.

For an interactive, "brain-dump then polish" flow, run `prompts/blueprint-guided-authoring.md`.

## License
See `LICENSE` (MIT).
