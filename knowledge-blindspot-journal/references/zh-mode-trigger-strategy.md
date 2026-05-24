# Chinese Trigger and Mode Strategy

Use this reference when the user speaks Chinese and the mode is not explicitly specified.

## Goal

Map Chinese intent to the correct mode:

- `strict`: produce the final archive directly
- `review`: show candidates first, then help decide what should be archived

This routing is semantic, not keyword-only. Do not rely on a single word such as `summary` or `archive`.

## Precedence

1. Explicit mode beats trigger wording.
2. `review` beats `strict` when both signal types appear.
3. Precision and caution language imply `review`.
4. Pure archiving language with no caution signal implies `strict`.

## Review Intent Buckets

Use `review` when the Chinese request means any of these:

- judge first
- filter first
- separate true blindspots from noise
- confirm before writing markdown
- avoid misclassification
- show candidates before final archiving

Typical Chinese phrasing often carries meanings like:

- first help me judge
- first do not archive directly
- first list candidates
- help me distinguish what I truly do not understand
- help me see what should be recorded and what should be skipped

These all imply `decide before archive`.

## Strict Intent Buckets

Use `strict` when the Chinese request means any of these:

- archive directly
- write the final markdown note
- keep only confirmed blindspots
- create a formal record
- summarize into a concise learning note

Typical Chinese phrasing often carries meanings like:

- directly turn this into markdown
- archive the real blindspots from this thread
- output the final version
- record only the confirmed items
- make a formal study note

These all imply `archive now`.

## Ambiguous Chinese Requests

These meanings are not enough by themselves:

- summarize this
- review this thread
- organize this
- record this
- look at this problem

For ambiguous Chinese prompts, inspect surrounding intent:

- if the user asks what truly counts, use `review`
- if the user asks for the final note, use `strict`
- if the user mentions precision, omissions, or overcounting risk, use `review`
- otherwise use `strict`

## Feature-Development Threads

In feature work, Chinese requests often imply blindspot discovery indirectly.

Use `review` when the meaning is:

- which parts of this feature exposed my blindspots
- which parts of my design show a knowledge gap
- help me decide what is worth archiving

Use `strict` when the meaning is:

- archive the blindspots exposed during this feature work
- generate a final blindspot note from this requirement thread

## Output Reminder

Regardless of mode:

- do not equate asking for help with lack of understanding
- extract concept-level blindspots, not symptom-level labels
- prefer omission over over-attribution
