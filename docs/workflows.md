# Workflows

This doc describes the intended workflows for analyzing an author's writing and drafting new text.

## 1) Set up sources

- Create or update your local `config/sources.yml`.
- Recommended: run `prompts/sources-manifest-wizard.md`.

`config/sources.yml` is gitignored.

## 2) Generate a profile

- Run `prompts/generate-style-profile.md`.
- The agent ingests sources and writes artifacts under `artefacts/`.

## 3) Update a profile

- Add new writing samples to sources referenced by `config/sources.yml`.
- Run `prompts/update-style-profile.md`.
- The agent reprocesses the whole corpus, overwrites artifacts, and appends revision logs.

## 4) Draft new text (two options)

### A. Profile-conditioned drafting (with conformance)

- Run `prompts/profile-conditioned-drafting.md`.
- Use when you want a direct draft in your style plus a conformance report.

### B. Brain-dump then polish

- Run `prompts/profile-guided-authoring.md`.
- Use when you want to write rough notes first and then have them polished in your style.

## 5) Migrate legacy artifacts

- If you have older legacy profiles, run `prompts/migrate-profile-v1-to-v2.md`.
- Migration preserves qualitative patterns and evidence and marks metrics as `unknown`.

## References

- Schema and file contracts: `docs/style-profile-spec.md`
- Shared method: `docs/style-profile-method.md`
- Success criteria: `docs/quality-gates.md`
- Manual smoke test: `docs/smoke-test.md`
