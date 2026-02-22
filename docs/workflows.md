# Workflows

This doc describes the intended workflows for analyzing an author's writing and drafting new text.

## 1) Set up sources

- Create a project folder under `sources/` (one folder per profile/project).
  - Example: `sources/facebook/`
  - You may create subfolders inside a project (e.g., `sources/facebook/posts/`, `sources/facebook/comments/`).
- Put all source material for that project inside that folder.
- All sources must be local files under `sources/` (no URL ingestion in this workflow).

## 2) Generate a profile

- Run `prompts/generate-style-profile.md`.
- The agent asks you to select a project folder under `sources/`.
- The agent ingests all files under `sources/<project>/` and writes a new run under `artefacts/<project_slug>/<run_id>/`.

## 3) Re-run after adding sources (no overwrite)

- Add new writing samples to `sources/<project>/`.
- Re-run `prompts/generate-style-profile.md` and select the same project.
- The agent reprocesses the whole corpus and writes a NEW timestamped run folder; prior runs remain intact.

## 4) Draft new text (two options)

### A. Profile-conditioned drafting (with conformance)

- Run `prompts/draft-with-style-profile.md`.
- Use when you want a direct draft in your style plus a conformance report.

Implementation note: conformance checking follows `prompts/check-style-profile-conformance.md`.

### B. Brain-dump then polish

- Run `prompts/author-with-style-profile.md`.
- Use when you want to write rough notes first and then have them polished in your style.

## 5) Package a profile for downstream consumers (optional)

If you want to share a profile with a downstream human or AI agent, you can package it into a single distributable file.

Two supported packaging formats:

- A) Zip bundle (`.styleprofile.zip`): copy the zip into a project and run the included entrypoint prompt.
- B) Inline bundle (`.inline.md`): copy/paste one single Markdown file into a prompt window (simplest).

Use: `prompts/package-style-profile-bundle.md`

## References

- Schema and file contracts: `docs/style-profile-spec.md`
- Bundle packaging: `docs/style-profile-bundle-spec.md`
- Shared method: `docs/style-profile-method.md`
- Success criteria: `docs/quality-gates.md`
