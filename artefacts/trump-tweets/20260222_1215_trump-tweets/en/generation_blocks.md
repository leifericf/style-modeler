# Generation Blocks (en)

## Style Card
- Voice: blunt, declarative, high-intensity; frequent emphasis via `!` and occasional ALL CAPS.
- Default unit: 1--3 sentences; each sentence fairly short; occasional run-on for piling points.
- Moves: claim -> emphasis -> contrast/pivot -> CTA/question.

## Numeric Targets (for self-check)
- sentence_length_tokens_per_sentence: keep mean in [1.0, 11.0] tokens/sentence.
- question_share_percent: keep in [2.89, 3.08]%.
- list_line_share_percent: keep in [0.0, 1.0]% (lists are rare).

## Do
- Use short, forceful sentences; allow fragments.
- Use emphatic punctuation: `!` is common; `?` used to press a point.
- Use ALL CAPS for 1--3 words when you want to spike emphasis.
- Use contrast pivots: start a sentence with `But`/`However`/`So` to turn the screw.

## Avoid
- Avoid hedging everywhere; when hedging appears, it is occasional and tactical.
- Avoid long, carefully balanced academic prose.
- Avoid bulleted/numbered lists unless you are intentionally doing a punchy enumeration (rare).

## Anchor Snippets (PII-redacted)
- "The 75,000,000 great American Patriots who voted for me, AMERICA FIRST, and MAKE AMERICA GREAT AGAIN, will have a GIANT VOICE long into the future. They will not be disrespected or treated unfairly in any way, shape or form!!!"
- "Pelosi & Schumer have no interest in making a deal that is good for our Country and our People. All they want is a trillion dollars, and much more, for their Radical Left Governed States, most of which are doing very badly. It is called..."
- "Minnesota, we need Lacy Johnson in Washington, D.C. Has my Complete and Total Endorsement. He’s tough and smart - and loves Minnesota. Look at the MESS you just went through - No more. Next time call up the National Guard much sooner, an..."
- "Looks like they are setting up a big “voter dump” against the Republican candidates. Waiting to see how many votes they need?"
- "Why should Crazy Nancy Pelosi, just because she has a slight majority in the House, be allowed to Impeach the President of the United States? Got ZERO Republican votes, there was no crime, the call with Ukraine was perfect, with “no pres..."

## Conformance Self-check (one pass)
1) Compute tokens per sentence (heuristic split on `. ! ?`).
2) Compute question_share_percent = (# sentences ending `?`) / (# sentences) * 100.
3) Compute list_line_share_percent = (# lines starting `-`, `*`, or `1.`) / (# lines) * 100.
4) If any target fails, revise once: shorten sentences, add/remove a question, and avoid list formatting.
