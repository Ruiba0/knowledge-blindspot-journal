---
name: knowledge-blindspot-journal
description: Use when the user wants to extract, review, archive, or summarize knowledge blindspots from the current thread after coding, debugging, environment troubleshooting, root-cause analysis, feature design, or postmortem work. This skill turns thread evidence into a markdown note that separates confirmed blindspots, likely blindspots, and items that should not be archived.
---

# Knowledge Blindspot Journal

## Overview

Use this skill to convert a working thread into a focused learning note about the user's real knowledge gaps.

Do not treat every request for help as ignorance. This skill is deliberately conservative: it only archives blindspots that are supported by thread evidence and explicitly separates weaker candidates from confirmed ones.

The final note should balance two jobs:

- identify real blindspots from thread evidence
- explain those blindspots clearly enough that the user can understand and reuse the knowledge

## When To Use

Use this skill when the user:

- explicitly invokes `$knowledge-blindspot-journal`
- asks to summarize what they did not understand in the current thread
- asks for a learning note, blindspot archive, reflection, or post-task knowledge summary
- wants to preserve debugging lessons, architecture lessons, or repeated confusion points from coding work

Common triggers in natural language:

- `knowledge blindspot`
- `blindspot summary`
- `learning archive`
- `thread review`
- `what did I not understand`
- `archive what I learned from this issue`

Do not use this skill for generic task summaries, changelogs, or solution recaps unless the user explicitly wants knowledge-gap extraction.

For Chinese-language routing and mode selection examples, see `references/zh-mode-trigger-strategy.md`.

## Core Rule

The goal is not to guess what the user knows in general.

The goal is to identify what this thread provides evidence that the user does not fully understand, or understands unstably, and to archive only those items with the correct confidence level.

Archive capability gaps, mental-model gaps, and repeated reasoning gaps. Do not archive the surface symptom alone.

Prefer:

- `unstable understanding of module resolution and build flow`
- `unstable judgment around database transaction boundaries`
- `missing decision criteria for API design trade-offs`

Avoid:

- `cannot fix this error`
- `cannot build this feature`
- `cannot configure this environment`

## Evidence Model

Start by extracting candidate blindspots from the current thread only unless the user explicitly asks for cross-thread synthesis.

### Strong signals

Treat these as strong evidence:

- The user explicitly says they do not understand something.
- The user explicitly asks why a mechanism works after seeing the fix.
- The user makes a concrete but incorrect root-cause claim that is later corrected.
- The same concept causes repeated confusion within the same thread.

### Medium signals

Treat these as supporting evidence:

- The user asks for the principle, internal flow, or trade-off behind a solution.
- The user cannot choose between multiple options even after trade-offs are explained.
- The user can describe symptoms but cannot map them to the underlying subsystem.
- The same category of issue has appeared repeatedly in the visible conversation context.

### Weak signals

Treat these as insufficient by themselves:

- The user pastes logs and asks for help.
- The user asks for a direct fix without analysis.
- The user delegates an implementation task to save time.
- The user requests a full design for a new feature.

## Exclusion Rules

Do not archive an item as a blindspot when any of these is true:

- The user already identified the root cause correctly and only wants execution help.
- The behavior is best explained by convenience, speed, or delegation rather than lack of understanding.
- The issue is transient, external, or operational noise with no stable learning value.
- The candidate is just a one-off tool command or environment step with no reusable concept behind it.
- The thread contains too little evidence to distinguish ignorance from normal collaboration.

When in doubt, downgrade the item instead of promoting it.

## Classification Rules

Classify each candidate using evidence quality, not volume.

- `confirmed`: at least one strong signal, or two independent medium signals that point to the same underlying concept
- `likely`: some evidence exists, but it is not strong enough to assert a real blindspot safely
- `tentative`: weak candidate worth mentioning only in review mode
- `exclude`: do not archive; optionally list the reason

Never promote a candidate to `confirmed` from weak signals alone.

## Mode Selection

Support two output modes.

### strict

Use strict mode when the user wants a clean archive with low false positives.

Rules:

- Output only `confirmed` and `likely`
- Keep the list short
- Prefer omission over over-attribution
- Mention excluded candidates only when that clarification is useful

### review

Use review mode when the user wants to inspect candidates before final archiving.

Rules:

- Output `confirmed`, `likely`, `tentative`, and `excluded`
- Show the evidence behind each candidate
- If precision is still materially uncertain, ask at most 3 short confirmation questions before producing the final archive

Default to `review` if the user asks for precision or seems worried about misclassification. Otherwise default to `strict`.

## Trigger Routing

Choose the mode from user intent, not from the presence of the word `blindspot` alone.

Apply these precedence rules:

1. If the user explicitly says `strict` or `review`, obey that directly.
2. If the user asks to inspect, judge, filter, confirm, or avoid misclassification before archiving, use `review`.
3. If the user asks for a final archive, summary note, or concise markdown record without asking for screening first, use `strict`.
4. If both signals appear, prefer `review`.

Heuristic mapping:

- `review` intent usually means `help me decide what counts as a blindspot`
- `strict` intent usually means `give me the final archived result`

When the user says the archive should be precise, conservative, or low-noise, that is a `review` signal unless they also explicitly ask to skip screening.

## Extraction Workflow

1. Summarize the thread goal in 1-3 sentences.
2. Extract candidate blindspots from the user's own messages, follow-up questions, and corrected misunderstandings.
3. Merge duplicates that map to the same underlying concept.
4. Rewrite each candidate from symptom-level wording into concept-level wording.
5. Apply exclusion rules before classification.
6. Classify each remaining candidate as `confirmed`, `likely`, `tentative`, or `exclude`.
7. Draft the markdown note using the template in `references/output-template.md`.

## Rewriting Guidance

Convert concrete incidents into reusable knowledge targets.

Examples:

- `Why does the import resolve locally but fail in build?`
  -> `insufficient understanding of module resolution, path aliases, and build/runtime differences`
- `Why is the entity loaded but commit still fails?`
  -> `unstable understanding of ORM entity state, transaction boundaries, and commit-time validation`
- `I know the feature goal but don't know how to split it safely`
  -> `missing a stable method for feature decomposition, dependency impact analysis, and safe verification`

Keep the abstraction one level above the surface issue. Do not drift into broad labels like `weak backend fundamentals` or `not familiar with frontend`.

## Output Requirements

Produce a markdown note in Chinese unless the user asks for another language.

Resolve the output path using the strategy in `references/output-path-strategy.md`.
Use `references/explanation-and-resources.md` for how much explanation to provide and how to recommend learning resources.

The note must contain:

- thread summary
- mode used
- confirmed blindspots
- likely blindspots
- evidence for each included item
- explanation for each confirmed blindspot
- concise explanation for each likely blindspot
- excluded or non-archived items with reasons when useful
- recommended learning resources
- learning actions or follow-up topics

Each included blindspot should include:

- `blindspot`: concept-level label
- `why_it_was_selected`: 1-2 sentences
- `evidence`: concrete thread evidence
- `next_step`: a targeted learning action

For `confirmed` blindspots, provide a full teaching block:

- `explanation`: explain the underlying concept in practical language
- `correct_mental_model`: explain how the parts relate and where responsibility belongs
- `how_to_apply`: map the concept back to the user's actual task or bug
- `recommended_resources`: prioritized links or source names, preferably official documentation first

For `likely` blindspots, provide a concise teaching block:

- `concise_explanation`: short explanation of the likely missing concept
- `recommended_resources`: 1-3 high-signal resources, official first when available

For `tentative` and `not archived` items, do not add explanatory teaching content unless the user explicitly asks for it.

Do not fabricate missing evidence. If evidence is absent, mark the item as excluded or tentative.

When recommending resources:

- prefer official documentation, standards, or primary sources first
- then add one or two high-signal secondary sources only if they materially improve learning
- if web access is available and the user did not prohibit browsing, include direct links
- if web access is unavailable, provide source names and precise search targets instead of guessing URLs

When writing the archive to disk:

- create the output directory if it does not exist
- show the resolved final path in the response
- if the user asked only for analysis and not file creation, still mention the path that would be used
- if both a path and a filename pattern are configured, obey them unless the user explicitly overrides them in the current request

## If Evidence Is Thin

If the thread is too short or too execution-focused, say so directly.

Use this fallback:

- state that the current thread does not provide enough evidence for precise blindspot extraction
- list at most 3 tentative candidates
- ask for either more thread context or permission to keep only a lightweight note

## Reference

Use `references/output-template.md` as the default output structure.
Use `references/output-path-strategy.md` for output path resolution and configuration precedence.
Use `references/explanation-and-resources.md` for explanation depth, teaching structure, and resource selection.
