# Style Profile (en)

- schema_version: 2
- language: `en`
- generated_at_utc: `2026-02-22T12:15:06Z`

## Coverage
- samples: 44867 (retweets excluded)
- tokens: 912025
- sentences (heuristic): 124192

## High-signal traits (stable)
- Short, punchy sentences on average (mean 7.599 tokens/sentence).
- Heavy use of emphatic punctuation (exclamation-share 22.372%; question-share 2.982%).
- Emphasis via ALL CAPS appears regularly (approx 12.84 samples per 1k tokens include >=1 ALL-CAPS word of len>=3).

## Dimensions
### Lexis / lexical richness (`lexis`)
- stability: stable
- notes: Word normalization lowercases and strips edge punctuation for lexical counts.
- Type-token ratio (overall): 0.054854.
- Hapax rate: 0.652455 (share of unique types seen once).

### Syntax & complexity (`syntax`)
- stability: stable
- notes: Sentence splitting is heuristic on .!? boundaries; commas and parentheticals are character-count proxies.
- Sentence length median: 6.0 tokens/sentence.
- Commas per sentence: 0.3188.

### Cohesion (`cohesion`)
- stability: stable
- notes: Discourse markers lexicon: however, therefore, meanwhile, also, because, so, but, then, now (simple presence-per-sample count).
- Discourse-marker presence: 9.323 per 1k tokens.

### Stance / modality (`stance`)
- stability: stable
- notes: Hedges/boosters/modals use lightweight English lexicons; treat as proxies (not full semantic parsing).
- Hedge markers: 1.569 per 1k tokens.
- Booster markers: 7.771 per 1k tokens.

### Perspective & audience design (`perspective`)
- stability: stable
- notes: Pronoun counts are token regexes on measurement text.
- Pronouns per 1k tokens: I=19.359, we=13.426, you=17.292.

### Register / tone (`register`)
- stability: stable
- notes: Contractions are tokens with apostrophes; emoji count is a rough Unicode-block heuristic.
- Contractions: 8.728 per 1k tokens.
- Emoji: 0.498 per 1k tokens (rough).

### Rhetorical devices (`rhetoric`)
- stability: stable
- notes: Contrast and listing measures are simple surface detectors.
- Contrast frames: 3.093 per 1k tokens.
- List-line share: 0.0222% of lines start with list markers.

### Structure / discourse organization (`structure`)
- stability: stable
- notes: Opening types are heuristic based on first sentence; closing CTA uses simple phrase matches or terminal question.
- Openings distribution (heuristic): {'claim': 91.8, 'greeting': 3.14, 'question': 4.33, 'attention': 0.72}.
- Closing CTA markers: 6.902 per 1k tokens.

## Do / Avoid (generation constraints)
### Do
- Use short, assertive sentences; allow occasional longer run-ons for emphasis.
- Use emphatic punctuation (especially `!`) and occasional rhetorical questions.
- Use capitalization for emphasis sparingly but noticeably (ALL CAPS keywords).
- Use direct address (`you`) and collective framing (`we/our`) when making calls-to-action.

### Avoid
- Avoid long multi-paragraph exposition; keep items compact and tweet-like.
- Avoid Markdown list formatting unless intentionally breaking voice (lists are rare).

## Numeric targets (stable)
- sentence_length_tokens_per_sentence IQR target: {'min': 1.0, 'max': 11.0, 'unit': 'tokens_per_sentence'}
- question_share_percent (approx 95% band): {'min': 2.89, 'max': 3.08, 'unit': 'percent'}
- list_line_share_percent: {'min': 0.0, 'max': 1.0, 'unit': 'percent'}

## Limitations
- Sentence splitting and several detectors are heuristic; treat as interpretable proxies.
- Non-English/ambiguous items were excluded as `unknown` (samples=1315, tokens=4493).
