# StyleModeler Inline Bundle: StyleModeler Profile (trump-tweets, en)

This is a single-file bundle. Paste this file into an AI chat, then provide `input_text` and (optionally) `lang`, `intent`, and `output_format`.

## Manifest

```json
{
  "artefacts_by_language": {
    "en": {
      "diachronic": "artefacts/en/diachronic.json",
      "distributions": "artefacts/en/distributions.json",
      "examples": "artefacts/en/examples.md",
      "generation_blocks": "artefacts/en/generation_blocks.md",
      "metrics": "artefacts/en/metrics.json",
      "profile_summary": "artefacts/en/profile_summary.md"
    }
  },
  "created_at": "2026-02-22T12:31:05Z",
  "default_language": "en",
  "entrypoints": {
    "stylize": "prompts/entrypoint-stylize.md"
  },
  "files": [
    {
      "bytes": 2104,
      "media_type": "text/markdown",
      "path": "prompts/entrypoint-stylize.md",
      "sha256": "c1d3802335d560f46d96813e0b24f86039aac1c80fd1d1666b1a12a96a54f065"
    },
    {
      "bytes": 2708,
      "media_type": "application/json",
      "path": "artefacts/en/diachronic.json",
      "sha256": "7710af15e560b4ee30719fc0ce4cd3646164299bfd42cb7aae2621ba689b3c0d"
    },
    {
      "bytes": 1166,
      "media_type": "application/json",
      "path": "artefacts/en/distributions.json",
      "sha256": "c6f5733a31da950216756a1092b113d01ff74f343692de54a3a280b625111204"
    },
    {
      "bytes": 10400,
      "media_type": "text/markdown",
      "path": "artefacts/en/examples.md",
      "sha256": "ff37dbc80ee5082bb7c13c67e7bb1b1cd5353158ad657cda2b5180e59a6cfbce"
    },
    {
      "bytes": 2558,
      "media_type": "text/markdown",
      "path": "artefacts/en/generation_blocks.md",
      "sha256": "98e0f4d528a25b5558080211ee4f456820d82fab1f667237f8bc54c1cb70ec26"
    },
    {
      "bytes": 6678,
      "media_type": "application/json",
      "path": "artefacts/en/metrics.json",
      "sha256": "3bb7c0ec1065b64e4d4cb5b8c385eaa098d6dad7635327bc1bcb23fcdeb60ab1"
    },
    {
      "bytes": 3531,
      "media_type": "text/markdown",
      "path": "artefacts/en/profile_summary.md",
      "sha256": "2a7322f82f4e3956e2fa58635b42ad0cc53a3c590c471806146353533913f0d2"
    }
  ],
  "languages": [
    "en"
  ],
  "package_schema_version": 1,
  "profile_id": "styleprofile-trump-tweets-en-20260222",
  "profile_name": "StyleModeler Profile (trump-tweets, en)",
  "profile_schema_version": 2,
  "source_run": {
    "note": "This bundle normalizes paths to artefacts/<lang>/... and includes derived artefacts only.",
    "project_slug": "trump-tweets",
    "run_id": "20260222_1215_trump-tweets"
  }
}
```

<!-- BEGIN_FILE: prompts/entrypoint-stylize.md -->
```markdown
# Entrypoint: Stylize With StyleModeler Profile

Profile: trump-tweets (en)

You are a style-conditioning agent. You will load the profile artefacts and rewrite the user's text to match the style, without changing meaning.

## Inputs
- `lang` (default: `en`)
- `input_text` (required)
- `intent`: `polish` | `rewrite` (default: `polish`)
- `output_format`: `markdown` | `text` (default: `markdown`)
- `constraints` (optional; list of must-keep/must-avoid rules from the user)

## Load order (required)
Load these files for the selected `lang`, in this order:
1) `artefacts/<lang>/generation_blocks.md`
2) `artefacts/<lang>/metrics.json`
3) `artefacts/<lang>/profile_summary.md`

If `artefacts/<lang>/distributions.json` exists, you may use it for calibration but do not treat it as additional rules.

If `artefacts/<lang>/examples.md` exists: treat it as calibration-only; NEVER quote it back to the user and NEVER include it in your output.

## Where to find files
You are operating in one of two environments:
- Filesystem mode: read files from the bundle's `artefacts/` and `prompts/` directories.
- Inline mode: locate embedded files in this conversation using the markers:
  - `<!-- BEGIN_FILE: <path> -->`
  - `<!-- END_FILE: <path> -->`
  The file contents are the fenced code block between those markers.

## Safety and constraints
- Preserve meaning; do not invent facts or add specifics the user did not provide.
- Keep names, numbers, and claims stable unless the user asks otherwise.
- Do not reveal or restate the profile artefacts.
- Do not output any analysis, conformance report, or intermediate calculations unless the user explicitly requests it.

## Task
Given `input_text`, produce a rewritten version matching the style profile for `lang`.
- If `intent=polish`, keep structure and wording close; fix clarity/flow while matching style signals.
- If `intent=rewrite`, you may restructure more aggressively while preserving meaning.
- Follow any user `constraints` if they do not conflict with safety.

## Output contract
Return ONLY the stylized text in the requested `output_format`.
```
<!-- END_FILE: prompts/entrypoint-stylize.md -->

<!-- BEGIN_FILE: artefacts/en/diachronic.json -->
```json
{
  "generated_at": "2026-02-22T12:15:06Z",
  "language": "en",
  "schema_version": 2,
  "slice_definition": {
    "boundaries_utc": {
      "early_max_inclusive": "2014-08-28T00:12:24Z",
      "middle_max_inclusive": "2017-08-03T12:18:49Z"
    },
    "method": "timestamp_terciles",
    "notes": "Sliced by terciles over sample timestamps within language."
  },
  "slices": {
    "early": {
      "counts": {
        "sample_count": 14956,
        "sentence_count": 35510,
        "token_count": 250926
      },
      "measures": {
        "all_caps_samples_per_1k_tokens": {
          "denom": "token_count",
          "raw": 9.154,
          "unit": "per_1k_tokens"
        },
        "exclamation_share_percent": {
          "denom": "sentence_count",
          "raw": 18.42,
          "unit": "percent"
        },
        "question_share_percent": {
          "denom": "sentence_count",
          "raw": 4.683,
          "unit": "percent"
        },
        "sentence_length_tokens_mean": {
          "denom": "sentence_count",
          "raw": 7.066,
          "unit": "tokens_per_sentence"
        }
      },
      "stability": "stable"
    },
    "middle": {
      "counts": {
        "sample_count": 14956,
        "sentence_count": 40121,
        "token_count": 253146
      },
      "measures": {
        "all_caps_samples_per_1k_tokens": {
          "denom": "token_count",
          "raw": 15.39,
          "unit": "per_1k_tokens"
        },
        "exclamation_share_percent": {
          "denom": "sentence_count",
          "raw": 22.716,
          "unit": "percent"
        },
        "question_share_percent": {
          "denom": "sentence_count",
          "raw": 2.281,
          "unit": "percent"
        },
        "sentence_length_tokens_mean": {
          "denom": "sentence_count",
          "raw": 6.31,
          "unit": "tokens_per_sentence"
        }
      },
      "stability": "stable"
    },
    "recent": {
      "counts": {
        "sample_count": 14955,
        "sentence_count": 48561,
        "token_count": 407953
      },
      "measures": {
        "all_caps_samples_per_1k_tokens": {
          "denom": "token_count",
          "raw": 13.524,
          "unit": "per_1k_tokens"
        },
        "exclamation_share_percent": {
          "denom": "sentence_count",
          "raw": 24.977,
          "unit": "percent"
        },
        "question_share_percent": {
          "denom": "sentence_count",
          "raw": 2.319,
          "unit": "percent"
        },
        "sentence_length_tokens_mean": {
          "denom": "sentence_count",
          "raw": 8.401,
          "unit": "tokens_per_sentence"
        }
      },
      "stability": "stable"
    }
  }
}
```
<!-- END_FILE: artefacts/en/diachronic.json -->

<!-- BEGIN_FILE: artefacts/en/distributions.json -->
```json
{
  "counts": {
    "sample_count": 44867,
    "sentence_count": 124192,
    "token_count": 912025
  },
  "distributions": {
    "tokens_per_sample": {
      "bin_counts": [
        2868,
        4498,
        16941,
        13464,
        2006,
        5089,
        1,
        0,
        0,
        0
      ],
      "bin_edges": [
        0,
        5,
        10,
        20,
        30,
        40,
        60,
        80,
        120,
        200
      ],
      "notes": "Tweet-level length distribution.",
      "unit": "tokens"
    },
    "tokens_per_sentence": {
      "bin_counts": [
        54940,
        29137,
        19972,
        11450,
        5411,
        1883,
        1038,
        328,
        33,
        0,
        0,
        0,
        0
      ],
      "bin_edges": [
        0,
        5,
        10,
        15,
        20,
        25,
        30,
        40,
        50,
        60,
        80,
        100,
        150
      ],
      "notes": "Bins are [edge_i, edge_{i+1}) with last bin >= last_edge.",
      "unit": "tokens_per_sentence"
    }
  },
  "generated_at": "2026-02-22T12:15:06Z",
  "language": "en",
  "schema_version": 2
}
```
<!-- END_FILE: artefacts/en/distributions.json -->

<!-- BEGIN_FILE: artefacts/en/examples.md -->
```markdown
# Examples (en)

Evidence snippets are PII-redacted where needed and contain no source references.

## Register / emphasis
### Claim: Uses ALL CAPS for emphasis on key words/phrases.
- "The 75,000,000 great American Patriots who voted for me, AMERICA FIRST, and MAKE AMERICA GREAT AGAIN, will have a GIANT VOICE long into the future. They will not be disrespected or treated unfairly in any way, shape or form!!!"
- "Pelosi & Schumer have no interest in making a deal that is good for our Country and our People. All they want is a trillion dollars, and much more, for their Radical Left Governed States, most of which are doing very badly. It is called..."
- "Hard to believe, but if Nancy Pelosi had put our great Trade Deal with Mexico and Canada, USMCA, up for a vote long ago, our economy would be even better. If she doesn’t move quickly, it will collapse!"
- "Watching final hole of [REDACTED_HANDLE]. [REDACTED_HANDLE] is looking GREAT!"
- "Now that the Witch Hunt has given up on Russia and is looking at the rest of the World, they should easily be able to take it into the Mid-Term Elections where they can put some hurt on the Republican Party. Don’t worry about Dems FISA A..."

### Claim: Leans on exclamation marks for intensity and urgency.
- "The 75,000,000 great American Patriots who voted for me, AMERICA FIRST, and MAKE AMERICA GREAT AGAIN, will have a GIANT VOICE long into the future. They will not be disrespected or treated unfairly in any way, shape or form!!!"
- "Minnesota, we need Lacy Johnson in Washington, D.C. Has my Complete and Total Endorsement. He’s tough and smart - and loves Minnesota. Look at the MESS you just went through - No more. Next time call up the National Guard much sooner, an..."
- "The three year Hoax continues! [REDACTED_URL]"
- "The cost of ObamaCare is far too high for our great citizens. The deductibles, in many cases way over $7000, make it almost worthless or unusable. Good things are going to happen! [REDACTED_HANDLE]  [REDACTED_HANDLE] [REDACTED_HANDLE]  [..."
- "Today, it was my great honor to welcome the 2017 NCAA Football National Champion, Alabama Crimson Tide - to the White House. Congratulations! #RollTide[REDACTED_URL] [REDACTED_URL]"

## Stance & questions
### Claim: Uses questions (often rhetorical) to challenge or frame a point.
- "Looks like they are setting up a big “voter dump” against the Republican candidates. Waiting to see how many votes they need?"
- "Why should Crazy Nancy Pelosi, just because she has a slight majority in the House, be allowed to Impeach the President of the United States? Got ZERO Republican votes, there was no crime, the call with Ukraine was perfect, with “no pres..."
- "....terrible Gang of Angry Democrats. Look at their past, and look where they come from. The now $30,000,000 Witch Hunt continues and they’ve got nothing but ruined lives. Where is the Server? Let these terrible people go back to the Cli..."
- "After Crooked [REDACTED_HANDLE] allowed ISIS to rise, she now claims she'll defeat them? LAUGHABLE! Here's my plan: [REDACTED_URL]"
- "Shouldn’t George Will have to give a disclaimer every time he is on Fox that his wife works for Scott Walker?"

### Claim: Uses boosters/intensifiers ("very/really/absolutely/never/always") to heighten certainty.
- "The States want to redo their votes. They found out they voted on a FRAUD. Legislatures never approved. Let them do it. BE STRONG!"
- "I always stand with leaders who weren’t afraid to stand early with me. That’s why I am supporting a true conservative, [REDACTED_HANDLE] for State Treasurer of North Dakota! [REDACTED_URL]"
- "....Don't the Europeans have a lot of responsibility?” [REDACTED_HANDLE] Thank you Katie, I offered ISIS prisoners to the European countries from where they came, and was rejected on numerous occasions. They probably figured that the U.S..."
- "I never offered Pardons to Homeland Security Officials, never ordered anyone to close our Southern Border (although I have the absolute right to do so, and may if Mexico does not apprehend the illegals coming to our Border), and am not “..."
- "Harley-Davidson should stay 100% in America, with the people that got you your success. I’ve done so much for you, and then this. Other companies are coming back where they belong! We won’t forget, and neither will your customers or your..."

### Claim: Occasionally hedges with softeners ("maybe/perhaps/probably") when speculating.
- "Something how Dr. Fauci is revered by the LameStream Media as such a great professional, having done, they say, such an incredible job, yet he works for me and the Trump Administration, and I am in no way given any credit for my work. Ge..."
- "Do Nothing Democrats were busy wasting their time on the Impeachment Hoax, & anything they could do to make the Republican Party look bad, while I was busy calling early boarder & flight closings, putting us way ahead in our battle with..."
- "The E.U. and China will further lower interest rates and pump money into their systems, making it much easier for their manufacturers to sell product. In the meantime, and with very low inflation, our Fed does nothing - and probably will..."
- "The Paris Agreement isn’t working out so well for Paris. Protests and riots all over France. People do not want to pay large sums of money, much to third world countries (that are questionably run), in order to maybe protect the environm..."
- "Art Laffer just said that he doesn't know how a Democrat could vote against the big tax cut/reform bill and live with themselves!  [REDACTED_HANDLE]"

## Cohesion & framing
### Claim: Uses contrast markers ("but/however/yet") to pivot or reframe.
- "These scoundrels are only toying with the [REDACTED_HANDLE] (a great guy) vote. Just didn’t want to announce quite yet. They’ve got as many ballots as are necessary. Rigged Election!"
- "There is tremendous CoronaVirus testing capacity in Washington for the Senators returning to Capital Hill on Monday. Likewise the House, which should return but isn’t because of Crazy Nancy P. The 5 minute Abbott Test will be used. Pleas..."
- "Now the press is trying to sell the fact that I wanted a Moat stuffed with alligators and snakes, with an electrified fence and sharp spikes on top, at our Southern Border. I may be tough on Border Security, but not that tough. The press..."
- "Bernie Sanders and wife should pay the Pre-Trump Taxes on their almost $600,000 in income. He is always complaining about these big TAX CUTS, except when it benefits him. They made a fortune off of Trump, but so did everyone else - and t..."
- "Congress must pass smart, fast and reasonable Immigration Laws now. Law Enforcement at the Border is doing a great job, but the laws they are forced to work with are insane. When people, with or without children, enter our Country, they..."

### Claim: Sometimes opens with a connector ("But/So/Because/Now") to continue a thread.
- "So true. Thanks Josh! [REDACTED_URL]"
- "Now Fake News [REDACTED_HANDLE] is actually reporting that I wanted my daughter, Ivanka, to run with me as my Vice President in 2016 Election. Wrong and totally ridiculous. These people are sick!"
- "So nice to see this great honor. Thank you (but haven’t played golf in a long time)! [REDACTED_URL]"
- "So Great! [REDACTED_URL]"
- "So sorry to hear about the terrible accident involving our GREAT West Point Cadets. We mourn the loss of life and pray for the injured. God Bless them ALL!"

## Audience & calls-to-action
### Claim: Direct address to the reader ("you/your") is common in persuasive lines.
- "I am asking for everyone at the U.S. Capitol to remain peaceful. No violence! Remember, WE are the Party of Law & Order – respect the Law and our great men and women in Blue. Thank you!"
- "The Fake and Corrupt News never called Google. They said this was not true. Even in times such as these, they are not truthful. Watch for their apology, it won’t happen. More importantly, thank you to Google! [REDACTED_URL]"
- "Hans Von Spakovsky, “I haven’t seen any evidence of actual violations of the law, which is usually a basis before you start an investigation. Adam Schiff seems to be copying Joseph McCarthy in wanting to open up investigations when they..."
- "Thank you to my great supporters in Wisconsin. I heard that the crowd and enthusiasm was unreal!"
- """"[REDACTED_HANDLE]: [REDACTED_HANDLE] By going after you, Jindal also lost the LA governor race! Politicians Beware.""

### Claim: Closes with CTA-style phrasing (thanks/please/let me know) or a prompting question.
- "I am asking for everyone at the U.S. Capitol to remain peaceful. No violence! Remember, WE are the Party of Law & Order – respect the Law and our great men and women in Blue. Thank you!"
- "The great people of Montana can have no better VOICE than Senator [REDACTED_HANDLE]. He is doing an incredible job! Whoever the Democrat nominee may be, please understand that I will be working hard with Steve all the way, & last night I..."
- "Very important that OPEC increase the flow of Oil. World Markets are fragile, price of Oil getting too high. Thank you!"
- "Crimea was TAKEN by Russia during the Obama Administration. Was Obama too soft on Russia?"
- "THANK YOU Arkansas! Get out &amp, #VoteTrump on Tuesday. We will MAKE AMERICA SAFE &amp, GREAT AGAIN! [REDACTED_URL]"

## Structure
### Claim: Rarely uses explicit list formatting (when present, it is a deliberate rhetorical move).
- "7. The badly flawed Paris Climate Agreement protects the polluters, hurts Americans, and cost a fortune. NOT ON MY WATCH!8. I want crystal clean water and the cleanest and the purest air on the planet – we’ve now got that!"
- "6. The Democrats’ destructive “environmental” proposals will raise your energy bill and prices at the pump. Don't the Democrats care about fighting American poverty?"
- "4. The U.S. now leads the world in energy production...BUT... 5. Who's got the world's cleanest and safest air and water? AMERICA!"
- "1. Which country has the largest carbon emission reduction?AMERICA!2. Who has dumped the most carbon into the air?   CHINA!3. 91% of the world’s population are exposed to air pollution above the World Health Organization’s suggested leve..."
- "3.  You should tweet your pick for MVP using the celebrity’s name followed by the hashtag #CelebApprenticeMVP."

```
<!-- END_FILE: artefacts/en/examples.md -->

<!-- BEGIN_FILE: artefacts/en/generation_blocks.md -->
```markdown
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
```
<!-- END_FILE: artefacts/en/generation_blocks.md -->

<!-- BEGIN_FILE: artefacts/en/metrics.json -->
```json
{
  "counts": {
    "sample_count": 44867,
    "sentence_count": 124192,
    "token_count": 912025
  },
  "dimensions": {
    "cohesion": {
      "measures": {
        "discourse_marker_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 9.323209341849182,
          "raw": 8503,
          "unit": "per_1k_tokens"
        },
        "sentence_initial_connector_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 0.4747676872892739,
          "raw": 433,
          "unit": "per_1k_tokens"
        }
      },
      "notes": "Discourse markers lexicon: however, therefore, meanwhile, also, because, so, but, then, now (simple presence-per-sample count).",
      "stability": "stable"
    },
    "lexis": {
      "measures": {
        "hapax_legomena": {
          "denom": "unique_types",
          "raw": 32641,
          "unit": "types"
        },
        "hapax_rate": {
          "denom": "unique_types",
          "raw": 0.652455,
          "unit": "ratio"
        },
        "moving_window_ttr_mean": {
          "denom": "200_token_window",
          "raw": 0.703883,
          "unit": "ratio"
        },
        "type_token_ratio": {
          "denom": "token_count",
          "raw": 0.054854,
          "unit": "ratio"
        }
      },
      "notes": "Word normalization lowercases and strips edge punctuation for lexical counts.",
      "stability": "stable"
    },
    "perspective": {
      "measures": {
        "pronouns_i_per_1k_tokens": {
          "denom": "token_count",
          "rate": 19.35911844521806,
          "raw": 17656,
          "unit": "per_1k_tokens"
        },
        "pronouns_we_per_1k_tokens": {
          "denom": "token_count",
          "rate": 13.42616704585949,
          "raw": 12245,
          "unit": "per_1k_tokens"
        },
        "pronouns_you_per_1k_tokens": {
          "denom": "token_count",
          "rate": 17.292289136810943,
          "raw": 15771,
          "unit": "per_1k_tokens"
        }
      },
      "notes": "Pronoun counts are token regexes on measurement text.",
      "stability": "stable"
    },
    "register": {
      "measures": {
        "all_caps_word_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 12.8395603190702,
          "raw": 11710,
          "unit": "per_1k_tokens"
        },
        "contraction_tokens_per_1k_tokens": {
          "denom": "token_count",
          "rate": 8.727830925687343,
          "raw": 7960,
          "unit": "per_1k_tokens"
        },
        "emoji_chars_per_1k_tokens": {
          "denom": "token_count",
          "rate": 0.49779337189221784,
          "raw": 454,
          "unit": "per_1k_tokens"
        }
      },
      "notes": "Contractions are tokens with apostrophes; emoji count is a rough Unicode-block heuristic.",
      "stability": "stable"
    },
    "rhetoric": {
      "measures": {
        "contrast_frame_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 3.093116964995477,
          "raw": 2821,
          "unit": "per_1k_tokens"
        },
        "list_line_share_percent": {
          "denom": "line_count",
          "raw": 0.0222,
          "unit": "percent"
        }
      },
      "notes": "Contrast and listing measures are simple surface detectors.",
      "stability": "stable"
    },
    "stance": {
      "measures": {
        "booster_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 7.770620322907815,
          "raw": 7087,
          "unit": "per_1k_tokens"
        },
        "hedge_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 1.5690359365148983,
          "raw": 1431,
          "unit": "per_1k_tokens"
        },
        "modal_tokens_per_1k_tokens": {
          "denom": "token_count",
          "rate": 17.681532852717854,
          "raw": 16126,
          "unit": "per_1k_tokens"
        }
      },
      "notes": "Hedges/boosters/modals use lightweight English lexicons; treat as proxies (not full semantic parsing).",
      "stability": "stable"
    },
    "structure": {
      "measures": {
        "closing_cta_samples_per_1k_tokens": {
          "denom": "token_count",
          "rate": 6.902223075025356,
          "raw": 6295,
          "unit": "per_1k_tokens"
        },
        "openings_percent": {
          "denom": "sample_count",
          "raw": {
            "attention": 0.72,
            "claim": 91.8,
            "greeting": 3.14,
            "question": 4.33
          },
          "unit": "percent"
        }
      },
      "notes": "Opening types are heuristic based on first sentence; closing CTA uses simple phrase matches or terminal question.",
      "stability": "stable"
    },
    "syntax": {
      "measures": {
        "commas_per_sentence": {
          "denom": "sentence_count",
          "raw": 0.3188,
          "unit": "per_sentence"
        },
        "emdash_markers_per_1k_tokens": {
          "denom": "token_count",
          "rate": 2.9834708478386007,
          "raw": 2721,
          "unit": "per_1k_tokens"
        },
        "exclamation_share_percent": {
          "denom": "sentence_count",
          "raw": 22.372,
          "unit": "percent"
        },
        "parenthetical_markers_per_1k_tokens": {
          "denom": "token_count",
          "rate": 6.121542720868397,
          "raw": 5583,
          "unit": "per_1k_tokens"
        },
        "question_share_percent": {
          "denom": "sentence_count",
          "raw": 2.982,
          "unit": "percent"
        },
        "sentence_length_tokens_mean": {
          "denom": "sentence_count",
          "raw": 7.599,
          "unit": "tokens_per_sentence"
        },
        "sentence_length_tokens_median": {
          "denom": "sentence_count",
          "raw": 6.0,
          "unit": "tokens_per_sentence"
        }
      },
      "notes": "Sentence splitting is heuristic on .!? boundaries; commas and parentheticals are character-count proxies.",
      "stability": "stable",
      "targets": {
        "list_line_share_percent": {
          "target_range": {
            "max": 1.0,
            "min": 0.0,
            "unit": "percent"
          }
        },
        "question_share_percent": {
          "target_range": {
            "max": 3.08,
            "min": 2.89,
            "unit": "percent"
          }
        },
        "sentence_length_tokens_per_sentence": {
          "target_range": {
            "max": 11.0,
            "min": 1.0,
            "unit": "tokens_per_sentence"
          }
        }
      }
    }
  },
  "generated_at": "2026-02-22T12:15:06Z",
  "language": "en",
  "schema_version": 2
}
```
<!-- END_FILE: artefacts/en/metrics.json -->

<!-- BEGIN_FILE: artefacts/en/profile_summary.md -->
```markdown
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
```
<!-- END_FILE: artefacts/en/profile_summary.md -->

