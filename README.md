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

This repository contains three primary components:

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

## Output

The system produces a structured Markdown document titled `Writing Style Blueprint`.

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
-   Revision log (for iterative updates)

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

1.  Copy `config/sources-example.yml` to `config/sources.yml`.
2.  (Optional) Add or remove sources. By default, `config/sources.yml` reads from the repo-local `sources/` folder.

Note: `config/sources.yml` is ignored by git by default to avoid accidentally committing personal information.

### Step 0.5: Add Local Text Files

1.  Put plain text files in `sources/` (for example, `sources/linkedin.txt`, `sources/blog.txt`).
2.  Keep those files local. The `sources/` directory is ignored by git by default.

### Step 0.75: Keep `config/sources.yml` Updated

The prompts can emit an updated `config/sources.yml` as part of their output (timestamps, newest-sample info). Paste that YAML into your local `config/sources.yml` after each run.

### Step 1: Generate Initial Blueprint

1.  Gather writing samples (LinkedIn posts, tweets, blog posts, etc.).
2.  Paste them into the **Cross-Platform Style Blueprint Generator** prompt (`prompts/generate-style-blueprint.md`).
3.  Run the prompt.
4.  Save the generated Markdown file as:

    artefacts/writing-style-blueprint.md

### Step 2: Add New Writing Samples

When you have more writing (new LinkedIn posts, new platform, etc.):

1.  Provide:
    -   The current Markdown blueprint
    -   The new writing samples
2.  Use the **Writing Style Blueprint Update & Refinement Agent** prompt (`prompts/update-style-blueprint.md`).
3.  Replace the old blueprint with the updated version.
4.  Review the Revision Log section for changes.

### Step 3: Generate Stylized Writing

After analysis:

1.  Provide a new topic.
2.  Specify the target platform (optional).
3.  Ask for controlled imitation.
4.  The system generates a post following extracted rules.

## License
See `LICENSE` (MIT).
