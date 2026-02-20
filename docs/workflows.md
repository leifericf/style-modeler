# Workflows

This doc describes the intended workflows for analyzing an author's writing and drafting new text.

## 1) Set up sources

- Create or update your local `config/sources.yml`.
- Recommended: run `prompts/sources-manifest-wizard.md`.

`config/sources.yml` is gitignored.

## 2) Generate a profile

- Run `prompts/generate-style-profile.md`.
- The agent ingests sources and writes artifacts under `artefacts/`.

## 3) Re-generate after adding sources

- Add new writing samples to sources referenced by `config/sources.yml`.
- Re-run `prompts/generate-style-profile.md`.
- The agent reprocesses the whole corpus, overwrites artifacts, and appends revision logs (if present).

## 4) Draft new text (two options)

### A. Profile-conditioned drafting (with conformance)

- Run `prompts/profile-conditioned-drafting.md`.
- Use when you want a direct draft in your style plus a conformance report.

### B. Brain-dump then polish

- Run `prompts/profile-guided-authoring.md`.
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
- Manual smoke test: `docs/smoke-test.md`
