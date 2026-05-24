# Explanation and Resources Strategy

Use this reference when turning blindspot detection into a teaching-oriented archive.

## Goal

The note should not stop at `what was weak`.

It should also explain:

- what the concept means
- how to think about it correctly
- how it applied to the current thread
- where to study it next

Aim for an approximate split:

- half of the value comes from correct blindspot identification
- half of the value comes from useful explanation and follow-up learning guidance

## Explanation Depth by Confidence

### Confirmed

Explain fully.

Each confirmed blindspot should teach:

- the core concept
- the correct mental model
- the most relevant boundary or trade-off
- how the concept maps back to the user's actual problem

This is the main teaching surface of the document.

### Likely

Explain briefly.

Each likely blindspot should get:

- a short explanation of the suspected missing concept
- one practical application sentence
- a short resource list

Do not over-teach a likely blindspot if the evidence is still weak.

### Tentative and Not Archived

Do not provide explanatory blocks by default.

If the user explicitly asks to learn from excluded items too, state that those items are not reliable blindspot conclusions before explaining them.

## Teaching Structure

For a confirmed blindspot, explain in this order:

1. What it is
2. Why it matters here
3. How the moving parts relate
4. What mistake pattern usually causes confusion
5. What to remember next time

Prefer practical explanation over textbook definition.

Good:

- explain why backend authentication success does not automatically mean browser login state exists
- explain where cookies are written and why a landing page may still be needed

Weak:

- generic definitions with no tie back to the current thread
- broad lectures that do not help the user handle the same issue next time

## Recommended Resources

When adding learning resources, prefer this order:

1. official docs
2. standards or primary specs
3. framework-maintainer docs
4. high-signal tutorials or deep dives

When web access is available:

- look up current official documentation for the relevant technology
- include direct links when they are stable and clearly relevant
- avoid low-quality roundup articles

When web access is unavailable:

- name the resource clearly
- provide a precise search query or section name

## Resource Count

Per confirmed blindspot:

- 2 to 4 resources

Per likely blindspot:

- 1 to 3 resources

Keep the list short and focused.

## Resource Selection Rules

- Prefer the exact technology in the thread over generic background material.
- If the blindspot is conceptual, pair one conceptual resource with one stack-specific resource.
- If the blindspot concerns HTTP, URL encoding, cookies, auth redirects, routing, or browser behavior, prefer standards or authoritative platform docs where practical.
- If the blindspot concerns a framework or library, prioritize that framework's official docs.

## Final Section

The document may still end with cross-cutting learning actions, but those actions should come after the blindspot explanations and resource recommendations.

Do not let `Learning Actions` replace the teaching content.
