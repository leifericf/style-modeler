# Check Style Profile Conformance

Use this prompt to evaluate (and optionally revise) a draft so it conforms to numeric targets from a Style Profile.

This prompt is designed to be used as a shared procedure by:
- `prompts/draft-with-style-profile.md` (required)
- `prompts/author-with-style-profile.md` (only when the user asks for strict conformance)

## Inputs (required)

You need:

- A draft text (the text to evaluate).
- Output language `<lang>` (e.g., `en`).
- A Style Profile run root: `artefacts/<project_slug>/<run_id>/`.

Revision mode:
- `revise_on_fail`: `true|false` (default `false`)
- `max_revisions`: always `1`

## Select the Style Profile run (if not provided)

If the caller did not provide `project_slug` and `run_id` and you have filesystem access:

1) List available projects under `sources/` (immediate subdirectories).
2) Ask the user to select one project folder.
3) Infer `project_slug` from the selected folder name using the same slug rules as `prompts/generate-style-profile.md`.
4) List available runs under `artefacts/<project_slug>/`.
5) Default to the newest run (lexicographically largest `run_id`) unless the user specifies otherwise.

## Files to read (if you have repo access)

- `artefacts/<project_slug>/<run_id>/<lang>/metrics.json` (targets + stability)
- `artefacts/<project_slug>/<run_id>/<lang>/generation_blocks.md` (to guide revisions)

If `metrics.json` is missing or has no usable targets, stop and report: "no conformance targets available".

## Target selection (required)

1) Collect candidate targets from `metrics.json`:
   - Prefer targets associated with `dimensions.*.targets`.
   - Use only targets from dimensions marked `stable`.
   - If a target is present but its dimension is `unstable` or `unknown`, you MAY include it as an `unknown` check, but do not treat it as a hard failure.

2) Choose a small, checkable set of targets (aim for 3--8) that you can compute from the draft alone.
   - Do not invent targets.
   - Do not try to check everything.

3) For each chosen target, record:
   - target name
   - range (min/max) or threshold
   - unit
   - whether it is `stable` (hard) vs `unstable`/`unknown` (soft)

## Computing observed values (required)

Compute only what you can compute reliably from the draft text.

Rules:
- If you cannot compute a target reliably, mark it `unknown` and explain why in one short phrase.
- Do not fabricate values.
- Use simple, transparent heuristics when needed, and label them as heuristics.

## Pass/fail decision (required)

For each checked target:

- `pass`: observed value is within the target range/threshold.
- `fail`: observed value is outside the target range/threshold AND the target is from a `stable` dimension.
- `unknown`: not computable, ambiguous, or only soft/unstable targets.

Define "key targets" as the stable targets you chose. If any key target fails:

- If `revise_on_fail` is `true`, revise the draft once.
- If `revise_on_fail` is `false`, do not revise; just report failures.

## One-revision procedure (only if enabled)

When revising:

1) Use `generation_blocks.md` to keep the revision in the same voice.
2) Make the smallest edits that move the draft toward conformance.
3) Preserve meaning; do not add new facts.
4) Recompute observed values for the targets you checked.

If the revision improves some targets but worsens others, prefer the stable targets.

## Output contract

If this prompt is run standalone, return:

1) Conformance report
2) If a revision happened: revised draft (only once), then an updated conformance report

If this prompt is invoked by another prompt, produce a conformance report block that the caller can include verbatim.

Conformance report format (required, concise):

- `project_slug`, `run_id`, `lang`
- `revision_happened`: yes/no
- Targets checked:
  - `<name>`: target `<range/unit>`, observed `<value/unit>`, status `pass|fail|unknown`

Do not include raw artefact contents in your response.
