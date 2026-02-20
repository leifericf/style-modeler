# v2 Quality Gates

This document defines the success criteria for Style Profile v2.

It is intentionally:
- practical (measurable where possible)
- evidence-backed (claims are anchored to quotes/snippets)
- privacy-preserving (PII-safe)

## Scope

- Written text only (no speech/prosody).
- Nonfiction-first.
- Style profiling and profile-conditioned drafting.

## Locked Decisions (Phase 0)

- Artifact root directory: `artefacts/`.
- Human-readable artifacts: Markdown (`.md`).
- Machine-consumable artifacts: JSON (`.json`).
- Source manifest: YAML (`config/sources.yml`).

## Analysis Quality Gates (Profile Artifacts)

These gates apply to any v2 profile run that emits artifacts under `artefacts/`.

### A. Contract: expected files exist

- For each significant language `<lang>`, the run emits the v2 artifact set for that language.
- If multilingual is significant, a global layer exists and is used only for cross-language priors.

### B. Coverage: core dimensions are handled

For each core dimension in the v2 spec, the artifacts must contain either:
- measured values + interpretation + evidence anchors, OR
- an explicit `unknown` / `unstable` marking with a reason (e.g., insufficient sample size).

### C. Numeric hygiene

- Every numeric field declares a unit and scale.
- Raw counts are paired with a normalized rate when the metric is used for comparison.
- If a metric is derived/heuristic, the method is named and caveated.

### D. Confidence and stability

- Each dimension has a confidence/stability status.
- Sample-size thresholds are applied consistently; small/biased slices must be marked unstable.

### E. Evidence policy

- No invented quotes.
- Each non-trivial claim is anchored to representative snippets.
- Evidence snippets do not include source references (filenames/URLs/ids/dates).
- Evidence reuse is limited: the same snippet should not justify many unrelated claims.

### F. Privacy & PII

- Artifacts must not contain unredacted PII.
- If a crucial snippet contains PII, redact using `[REDACTED]` or omit with `...` while keeping the remainder verbatim.

### G. Language purity

- Per-language artifacts are strictly monolingual.
- Non-significant languages are handled as limitations rather than separate profiles.

## Generation Quality Gates (Profile-Conditioned Drafting)

These gates apply to any v2 drafting workflow that claims to be style-conditioned by v2 artifacts.

### A. Inputs are explicit

The drafting prompt/workflow collects or infers (and states) at minimum:
- language
- genre/domain
- audience
- purpose/speech act
- length constraint
- stance/tone dials (if not implied by genre)
- any must-include/must-avoid constraints

### B. Nonfiction safety

- If the user did not supply facts, the draft must avoid inventing specific factual claims.
- If uncertain, the draft uses qualified language or asks for missing facts.

### C. Conformance self-check and one revision

TBD in Phase 0.
