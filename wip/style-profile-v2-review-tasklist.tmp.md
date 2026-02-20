# Style Profile v2 Review Fix Tasklist (Temp)

Scope: bring implementation fully in line with `wip/style-profile-v2-plan.md` and `wip/stylistics-to-nlg-conversation-summary.md`.

## 1) Fix Interactive Corpus Builder (v1 -> v2)

- [x] Update `prompts/interactive-corpus-builder.md` to stop referencing legacy v1 artifacts:
  - Remove/replace reads of `artefacts/writing-style-profile*.md` and `artefacts/writing-style-profile-revision-log*.md`.
  - Replace with v2-aware detection (examples):
    - Prefer checking for `artefacts/<lang>/profile_summary.md` and/or `artefacts/<lang>/metrics.json`.
    - Treat any missing profile as “no v2 artifacts yet”.
- [x] Update the “modeling step” instructions:
  - If v2 artifacts exist: run `prompts/update-style-profile.md`.
  - Otherwise: run `prompts/generate-style-profile.md`.
- [x] Update revision-log guidance to v2 paths:
  - Per-language: `artefacts/<lang>/revision-log.md`.
  - Optional global: `artefacts/global/revision-log.md` when multilingual is significant.
- [x] Normalize terminology inside the prompt:
  - Use “style profile” consistently; avoid “writing-style-profile”.

Acceptance checks:
- [x] `prompts/interactive-corpus-builder.md` contains no `writing-style-profile` strings.
- [x] The prompt’s end-state matches the v2 artifact layout described in `docs/style-profile-spec.md`.

## 2) Fix README v1 remnants (Interactive Builder section)

- [x] Update `README.md` “Interactive Corpus Builder” section:
  - Remove the v1 output list (`artefacts/writing-style-profile.md`, `artefacts/writing-style-profile-revision-log.md`).
  - Replace with: writes interview data to `sources/`, updates `config/sources.yml`, then runs v2 generate/update prompts producing `artefacts/<lang>/...` and `artefacts/corpus-metadata.md`.

Acceptance checks:
- [x] `README.md` contains no references to `artefacts/writing-style-profile` outputs (except in the migration section, if kept as legacy inputs).
- [x] README’s workflow sections are internally consistent with `docs/workflows.md`.

## 3) Align `include_glob` recursion behavior

- [x] Make `config/sources-example.yml` match the recursive intent in `prompts/sources-manifest-wizard.md` and `docs/style-profile-method.md`:
  - Change `include_glob` for the `sources/` dir source to `"**/*.{txt,md,json,xml,html}"`.
  - Alternatively (if you decide non-recursive is intended), update the wizard + method docs to match.

Acceptance checks:
- [x] Wizard text, method doc, and `config/sources-example.yml` all agree on whether `sources/` ingestion is recursive.

## 4) Specify recency weighting semantics (avoid ambiguous behavior)

- [x] Decide and document the policy for `recency_weight` and `update_policy.recency_weight_latest_phase`.
  - Recommended minimal policy (least disruptive):
    - Metrics/distributions remain unweighted (describe the full corpus).
    - Recency weighting influences only:
      - which snippets are preferred as evidence/anchors
      - what is emphasized in `profile_summary.md` / `generation_blocks.md`
      - optional “current voice” notes (clearly labeled as recency-biased)
- [x] Write the policy into `docs/style-profile-method.md`.
- [x] If needed, add a short clarification in `docs/style-profile-spec.md` about where weighting is allowed vs forbidden.

Acceptance checks:
- [x] `docs/style-profile-method.md` contains an explicit, testable statement of what is weighted and what is not.
- [x] The policy does not contradict stability/target rules (no targets derived from unstable slices).

## 5) Quick verification sweep (repo-level)

- [x] Search for remaining legacy blueprint/v1 references:
  - `writing-style-profile`
  - `style-blueprint`
- [x] Re-read `docs/quality-gates.md` and confirm no new contradictions were introduced.
- [x] Optional: update `docs/smoke-test.md` if any file path expectations change.
