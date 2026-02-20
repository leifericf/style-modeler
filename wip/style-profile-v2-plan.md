# Style Profile v2 Redesign Plan (Stylistics -> NLG)

This file is intended to persist across sessions.

Defaults accepted (per discussion):
- Keep `artefacts/` spelling (avoid churn).
- Use Markdown for human-readable artifacts + JSON for machine-consumable metrics.
- Always split analysis by language; only create `artefacts/global/` when multiple languages are significant.
- Prefer a pragmatic, evidence-based stylistics approach: measurable features + interpretable summaries + anchored examples.

Conventions for updating this plan:
- Use `[x]` when complete.
- Keep tasks small enough to land in incremental commits.
- If scope changes, add a new task under the relevant phase.

---

## Phase 0 - Lock Decisions + Success Criteria

- [x] Document v2 success criteria (analysis + generation) in a single place (recommended path: `docs/quality-gates.md`).
- [x] Confirm artifact root remains `artefacts/` (default: keep).
- [x] Confirm output baseline remains Markdown + JSON (default: keep).
- [x] Define what "done" means for v2 (recommended): generation prompt loads v2 artifacts and performs a measurable conformance self-check.

## Phase 1 - Define a Scientific-but-Practical Profile Schema (v2)

- [x] Create `docs/style-profile-spec.md` as the canonical schema/spec:
- [x] Define core dimensions and their measures.
- [x] Define measurement units (e.g., per 1k tokens, % sentences, chars/sentence, etc.).
- [x] Define scales/controls (percentiles within corpus; low/med/high bins; optional 0-100 normalized controls).
- [x] Define confidence + stability rules (sample size thresholds; mark unstable/unknown).
- [x] Define evidence policy (PII-safe snippets; no source refs; minimal quotes; redaction rules).
- [x] Define segmentation axes the system may compute when metadata exists:
- [x] Language (always).
- [x] Source/platform (often).
- [x] Genre/register (optional).
- [x] Time slices (optional; requires timestamps).

Core dimension set (initial recommended v2 baseline):
- [x] Lexis / lexical richness (e.g., MTLD/TTR + lexical density proxy + salient n-grams).
- [x] Syntax & complexity (sentence length distribution + clause density proxies + passive/active proxy + punctuation complexity).
- [x] Cohesion (discourse marker inventory + rates + transition strategies + reference proxies).
- [x] Stance/modality/pragmatics proxies (hedges/boosters + modality + evaluatives + questions/directives).
- [x] Perspective & audience design (I/we/you ratios + direct address + explanation density proxies).
- [x] Register/tone (formality heuristic + contraction rate + colloquial markers + sentiment/emotion category proxies if feasible).
- [x] Rhetorical devices (light detectors + examples: rhetorical questions, contrast frames, repetition, parallel/listing).
- [x] Structure/discourse organization (opening/conclusion move types + paragraph/list usage distributions).

## Phase 2 - Redesign Artifact Outputs + Directory Layout (Less Duplication)

- [x] Define v2 artifact layout and exact file contracts (paths + required fields + formats):
- [x] `artefacts/<lang>/profile_summary.md`
- [x] `artefacts/<lang>/metrics.json`
- [x] `artefacts/<lang>/distributions.json`
- [x] `artefacts/<lang>/examples.md`
- [x] `artefacts/<lang>/generation_blocks.md`
- [x] `artefacts/<lang>/diachronic.json` (only when timestamps exist)
- [x] `artefacts/global/` (only when multilingual is significant)
- [x] `artefacts/global/global_profile.md`
- [x] `artefacts/global/global_metrics.json`
- [x] `artefacts/global/global_examples.md`
- [x] `artefacts/global/cross_language_summary.md`

- [x] Decide revision history strategy (default recommendation):
- [x] Keep `artefacts/<lang>/revision-log.md` (or `artefacts/global/revision-log.md` when applicable).
- [x] Log schema version + sources included + high-level metric shifts (no huge prose dumps).

## Phase 3 - Update the Analysis Prompts (Generate + Update) Around the v2 Schema

- [x] Reduce prompt duplication by introducing a shared method spec:
- [x] Add `prompts/style-profile-method.md` (ingestion, preprocessing, measures, artifact writing rules).
- [x] Refactor `prompts/generate-style-profile.md` to reference the method spec and emit v2 artifacts.
- [x] Refactor `prompts/update-style-profile.md` to reference the method spec and recompute metrics/distributions.

- [x] Update output contracts:
- [x] Generate prompt emits one fenced block per artifact file (Markdown blocks for .md, JSON blocks for .json, YAML for `config/sources.yml` if still emitted).
- [x] Update prompt emits updated artifacts + a change log entry (or run metadata).

- [x] Add explicit confidence + stability behavior:
- [x] Minimum sample thresholds per measure/dimension.
- [x] Mark measures as `unknown` or `unstable` rather than asserting.
- [x] Enforce evidence anchoring (snippets per claim; avoid reusing the same snippet across unrelated claims).

- [x] Multilingual alignment:
- [x] Always split by language.
- [x] Create `artefacts/global/` only when 2+ languages are significant.
- [x] Keep per-language artifacts strictly monolingual.

## Phase 4 - Update Source Manifest Expectations (So We Can Do Better Science)

- [x] Extend `config/sources-example.yml` and corresponding prompt text to optionally support:
- [x] `platform` label (explicit).
- [x] `default_language` (if known).
- [x] Optional `genre/register` hints.
- [x] Timestamp extraction hints for structured exports (where relevant).

- [ ] Update `prompts/sources-manifest-wizard.md` to request minimal useful metadata:
- [ ] Platform.
- [ ] Approx date range (if known).
- [ ] Language (optional).

## Phase 5 - Redesign the "Use Profile to Write Text" Prompt(s)

- [ ] Split authoring workflows:
- [ ] Create `prompts/profile-conditioned-drafting.md` (profile-conditioned NLG):
- [ ] Load artifacts (global + language folder).
- [ ] Collect generation-time parameters: language, genre, audience, purpose, length, stance dial, constraints.
- [ ] Draft.
- [ ] Run a conformance self-check against numeric targets and revise once.

- [ ] Keep/adjust `prompts/profile-guided-authoring.md` as "brain-dump then polish" flow:
- [ ] Load `generation_blocks.md` + a small set of examples.
- [ ] Do not require full metrics unless doing conformance.

- [ ] Add a compact "style card" assembly step (turn JSON targets into an instruction block).
- [ ] Add nonfiction safety constraint: if user didn’t supply facts, avoid inventing specifics (independent of style).

## Phase 6 - Migration + Backward Compatibility

- [ ] Add a migration prompt: `prompts/migrate-profile-v1-to-v2.md`:
- [ ] Read v1 profile artifacts (if present).
- [ ] Produce v2 artifacts with placeholders where recomputation is required.
- [ ] Clearly mark fields as `unknown` if they cannot be derived.

- [ ] Add `schema_version` to `metrics.json` (and to global metrics).
- [ ] Update `README.md` to describe v2 workflows + artifact layout.

## Phase 7 - Verification & Quality Gates

- [ ] Create `docs/quality-gates.md` and define checks:
- [ ] Coverage: each core dimension either has evidence or is marked uncertain.
- [ ] Units/scales present for each numeric field.
- [ ] Confidence annotations are present.
- [ ] PII policy adhered to (redaction/omission).
- [ ] Generation conformance self-check covers top targets and reports pass/fail.

- [ ] Run a smoke test on a known corpus and verify:
- [ ] Artifacts are created with expected paths.
- [ ] Generation prompt loads and uses them.
- [ ] Conformance report is meaningful.

## Phase 8 - Cleanup / Tidying

- [ ] Ensure only templates/specs are tracked; keep real outputs ignored (already mostly true via `.gitignore`).
- [ ] Consolidate methodology duplication into `docs/` (schema + method + workflow).
- [ ] Normalize terminology across docs/prompts: always "style profile" (avoid synonyms unless explicitly defined).
