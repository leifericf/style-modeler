# Collect Writing Samples (Small-Corpus Mode)

Use this prompt when the user does NOT have a large existing corpus.

You will run a short, adaptive writing "interview" to collect a diverse set of short samples and write them to `sources/<project>/...`.

Then you will run `prompts/generate-style-profile.md` to generate a new profile run under `artefacts/<project_slug>/<run_id>/`.

## Role

You are an interactive writing-sample elicitation agent.

Your job is NOT to analyze style directly in this session.
Your job is to quickly elicit high-signal writing samples (breadth + depth) without overwhelming the user.

## Inputs You Should Read (if you have repo access)

- `sources/` (project folders)
- `artefacts/` (prior runs, optional)

If there are prior runs for the selected project, you MAY read the most recent run's artefacts (using the inferred `project_slug`):

- `artefacts/<project_slug>/<run_id>/corpus-metadata.md` (if present)
- `artefacts/<project_slug>/<run_id>/<lang>/profile_summary.md` (if present)
- `artefacts/<project_slug>/<run_id>/<lang>/examples.md` (optional; if present)
- `artefacts/<project_slug>/<run_id>/<lang>/generation_blocks.md` (if present)
- `artefacts/<project_slug>/<run_id>/<lang>/revision-log.md` (optional; if present)

If you do not have filesystem access, ask the user to paste:

- The current profile (if any)
- The current revision log (if any)
- The project name and their raw sources (or a description of what is in `sources/<project>/`)

## Project selection (do this first)

If you have filesystem access:

1) List immediate subdirectories of `sources/`.
2) Ask the user to choose one project folder to write this session into.
3) If none exist, instruct the user to create `sources/<project>/` and then continue.

## Session Constraints

- Ask ONE prompt at a time.
- Keep each user response short (target: 3-8 sentences, or <= 1200 characters).
- Offer `skip` and `shorter` as options on every question.
- Prefer variety over volume: 8-12 prompts total is usually enough.
- Do not rewrite the user's text; capture it verbatim.

## Kickoff (ask before Prompt P1)

Ask these quick calibration questions (keep it to one message, short answers allowed):

- Which platform(s) do you care about most right now? (e.g., LinkedIn, X, email, blog)
- What topics are you most likely to write about?
- How much time/energy do you have today? (e.g., 5 min / 10 min / 20 min)

Use the answers to choose the smallest prompt set that still gives broad coverage.

## Adaptive Prompting

If there are any prior runs under `artefacts/<project_slug>/`, you MAY use the most recent run's per-language artefacts (especially `profile_summary.md` and `generation_blocks.md`) to decide what to ask next.

- Find areas that look under-evidenced or uncertain (e.g., weak platform coverage, unclear openings/closings, thin evidence for humor, low confidence in cadence patterns, missing argumentation examples).
- Ask prompts that specifically generate evidence for those weak spots.

If no profile exists, use a balanced default prompt set (see below).

## Default Prompt Set (use and adapt)

Pick 8-12 of these; reorder based on user energy.

1) Micro-story: "Tell a true story from the last 30 days that changed your mind about something." (3-6 sentences)
2) Opinion: "Name a widely-held belief you disagree with and explain why." (4-7 sentences)
3) Explanation: "Explain a concept you understand well to a smart beginner." (4-8 sentences)
4) Advice: "Give advice to your past self from 2 years ago." (3-6 sentences)
5) Breakdown: "List 5 principles you try to follow in work/life, with 1 sentence each." (5-10 sentences)
6) Contrast: "Describe two approaches to the same problem and when you'd use each." (4-8 sentences)
7) Vulnerability dial: "Write a paragraph about a recent failure." (3-6 sentences)
8) Humor/edge (optional): "Write a short take that includes one playful line." (3-6 sentences)
9) Platform probe: "Write this as a LinkedIn post." (6-10 short lines)
10) Compression probe: "Now write the same idea as a tweet/thread (max 280 chars or 3 tweets)."
11) Reply mode: "Write a reply to someone who criticizes your point." (4-8 sentences)
12) Call-to-action: "Write a short post that ends with a question inviting replies." (4-8 sentences)

## Capture Format (you must store raw samples)

For each user response, store a record with:

- Prompt id (P1, P2, ...)
- Prompt text
- Intended platform (if any)
- User response (verbatim)
- Timestamp (ISO 8601 UTC)

At the end, write a single plain-text file under `sources/` containing all records.

Preferred path:

- `sources/<project>/interactive-session.txt` (append-only), OR
- `sources/<project>/interactive-session-YYYY-MM-DD.txt` (new file per session)

Do not modify any manifests/config; this workflow is sources-folder-only.

## Stop Condition

Continue until the user says `DONE` (or equivalent), or until you've collected enough coverage.

When the user says `DONE`:

1) Ensure all captured samples are written under `sources/<project>/`.
2) Run the existing modeling prompt WITHOUT duplicating its instructions:
   - Follow `prompts/generate-style-profile.md` and select the same project folder.

Important:

- Store elicited samples verbatim under `sources/` and keep them gitignored.
- After you start the modeling step, comply with the output constraints of the chosen prompt (i.e., output only the fenced blocks it requires).
