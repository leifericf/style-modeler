# Package Style Profile Bundle (Zip Or Single Markdown)

You are packaging an existing StyleModeler profile run under `artefacts/<project_slug>/<run_id>/` into a single distributable unit that downstream consumers (humans and AI agents) can use with minimal friction.

You MUST follow `docs/style-profile-bundle-spec.md`.

## What you will produce

You will produce EXACTLY ONE of the following (ask which):

- A) A zip bundle: `bundles/<profile_id>.styleprofile.zip`
- B) A single inline bundle Markdown file: `bundles/<profile_id>.inline.md`

Important: `<profile_id>` MUST include the `project_slug` so the output filename is self-identifying (e.g., `styleprofile-trump-tweets-en-20260222`).

Default recommendation: B (inline) for easiest copy/paste consumption.

## Preflight checks (required)

0) Select a profile run:
   - If you have filesystem access:
     1) List available projects under `sources/` (immediate subdirectories).
     2) Ask the user to select one project folder.
     3) Infer `project_slug` from the selected folder name using the same slug rules as `prompts/generate-style-profile.md`.
     4) List runs under `artefacts/<project_slug>/`.
   - Default to the newest run (lexicographically largest `run_id`) unless the user specifies otherwise.

1) Verify `artefacts/<project_slug>/<run_id>/` exists and contains at least one per-language folder `artefacts/<project_slug>/<run_id>/<lang>/`.
2) Identify available languages by listing `artefacts/<project_slug>/<run_id>/*/generation_blocks.md`.
3) For each selected language, verify required files exist:
   - `artefacts/<project_slug>/<run_id>/<lang>/generation_blocks.md`
   - `artefacts/<project_slug>/<run_id>/<lang>/metrics.json`
   - `artefacts/<project_slug>/<run_id>/<lang>/profile_summary.md`
4) If any required file is missing, stop and report exactly what is missing.

## Bundle artefact layout (normalize)

The selected run lives under `artefacts/<project_slug>/<run_id>/` in this repo.

Inside the bundle you create, normalize paths so consumers always see:

- `artefacts/<lang>/...` for per-language artefacts
- `artefacts/global/...` for global artefacts (if present)
- optional `artefacts/corpus-metadata.md`

Do NOT include the original `<project>/<run_id>` folders inside the bundle.

## One question to ask (then proceed)

Ask the user to choose:

1) Package type: `inline` (recommended) or `zip`
2) Language(s): default to all languages found under the selected run root (`artefacts/<project_slug>/<run_id>/`)
3) Include optional sensitive-ish artefacts? (recommended defaults)
   - Include `examples.md`: YES (helpful), but mark as calibration-only and never to be quoted
   - Include `corpus-metadata.md`: NO by default
   - Include `distributions.json`: YES if present

If the user does not answer, proceed with: inline + all languages + include examples + exclude corpus-metadata + include distributions.

## Profile identity (required)

Choose:

- `profile_id`: a short, filesystem-safe id. MUST include `project_slug`. Default: `styleprofile-<project_slug>-<default_lang>-<YYYYMMDD>`.
- `profile_name`: default: `StyleModeler Profile (<project_slug>, <default_lang>)`.
- `default_language`: if multiple, pick the highest-volume language if known; otherwise first in sorted order.

## Generate entrypoint prompt(s)

Create `prompts/entrypoint-stylize.md` content (within the package you build).

Entrypoint requirements:

- Explicit loading order: `generation_blocks.md` -> `metrics.json` -> `profile_summary.md`.
- Works in two modes:
  - Filesystem mode (zip bundle): read the files from `artefacts/`.
  - Inline mode: locate the embedded file sections by markers `BEGIN_FILE`/`END_FILE`.
- Inputs:
  - `lang` (default `default_language`)
  - `input_text`
  - `intent`: `polish` | `rewrite` (default `polish`)
  - `output_format`: `markdown` | `text` (default `markdown`)
  - `constraints` (optional)
- Safety:
  - preserve meaning; do not invent facts
  - do not output artefact contents
  - do not quote `examples.md`
- Output contract: return ONLY the stylized text.

Optionally create `prompts/entrypoint-draft.md` (same loading, but assumes the user provides instructions/notes rather than a prewritten `input_text`). Only include it if you think it materially helps the consumer.

## Build manifest.json

Create `manifest.json` with the required fields from `docs/style-profile-bundle-spec.md`.

Populate `files` with (recommended; REQUIRED for zip bundles):

- bundle-relative `path`
- `bytes`
- `sha256` (use `shasum -a 256`)
- `media_type` (e.g. `application/json`, `text/markdown`)

For inline bundles, hashes are optional; if you include them, ensure they match the embedded file contents exactly.

## Build output (exactly one)

### If inline bundle

Write `bundles/<profile_id>.inline.md` with:

1) A short consumer section: how to use (paste this file, then provide input text).
2) A `manifest` JSON block.
3) The entrypoint prompt text.
4) Embedded artefacts for each included file, using these markers:

`<!-- BEGIN_FILE: <path> -->`

followed by a fenced code block with an appropriate info string.

`<!-- END_FILE: <path> -->`

Embedded file contents MUST be verbatim.

### If zip bundle

1) Create a temp directory structure under `bundles/.tmp/<profile_id>/` containing:
   - `manifest.json`
   - `prompts/entrypoint-stylize.md`
   - optional `prompts/entrypoint-draft.md`
   - `artefacts/` (selected files, using the normalized layout above)
2) Create `bundles/<profile_id>.styleprofile.zip` using a command that does not include macOS resource forks.
   - Prefer: `zip -r -X` from the temp directory root.

## Final response

When done, output:

- The produced path (single file)
- Which languages were included
- Whether `examples.md` and `corpus-metadata.md` were included
- A 2-line instruction snippet that a downstream consumer can follow
