# Quality Gates

This document defines the success criteria for the current style profile system.

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

These gates apply to any profile run that emits artifacts under `artefacts/`.

### A. Contract: expected files exist

- For each significant language `<lang>`, the run emits the full artifact set for that language.
- If multilingual is significant, a global layer exists and is used only for cross-language priors.

### B. Coverage: core dimensions are handled

For each core dimension in the schema, the artifacts must contain either:
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

These gates apply to any drafting workflow that claims to be style-conditioned by artifacts.

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

The system is considered “done” when there is at least one generation workflow that:

1) Loads artifacts (global + target language).
2) Drafts a response.
3) Runs a measurable conformance self-check against declared numeric targets.
4) Revises once if the check fails.

Minimum conformance-report contract:
- Report the targets used (including units).
- Report observed values computed from the draft.
- Mark each checked target as `pass` / `fail` / `unknown`.
- State whether a revision happened.

The self-check must be conservative: if a metric cannot be computed reliably from the draft, mark it `unknown` rather than inventing a number.

## Packaging Quality Gates (Downstream Distribution)

These gates apply to any workflow that packages a profile for downstream consumers (humans and AI agents).

### A. Single-file deliverable

The packaging workflow emits EXACTLY ONE deliverable file in either format:

- Zip bundle: `*.styleprofile.zip`, OR
- Inline bundle: `*.inline.md`

### B. Entrypoint included

The deliverable includes an explicit entrypoint prompt that:

- states required inputs (`lang`, `input_text`, format/options)
- states artifact loading order
- instructs the downstream agent to return ONLY the stylized output (unless asked otherwise)

### C. Artifact completeness

For each included language `<lang>`, the package includes at minimum:

- `artefacts/<lang>/generation_blocks.md`
- `artefacts/<lang>/metrics.json`
- `artefacts/<lang>/profile_summary.md`

### D. Copy/paste robustness (inline bundles)

Inline bundles must embed artifacts verbatim and be machine-locatable via explicit file markers (e.g., `BEGIN_FILE`/`END_FILE`).

### E. Privacy posture

Packaging must not weaken privacy guarantees:

- Do not include additional raw sources.
- If including evidence snippets (`examples.md`), they remain calibration-only and must not be quoted back to end users.
