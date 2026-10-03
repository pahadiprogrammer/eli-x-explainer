# ASD-STE100 — writing-rules reference (for the ELI-X "ASD-STE100" level)

> **Attribution / license.** The rules and canonical examples below are adapted from
> **Wikipedia, "Simplified Technical English"** — https://en.wikipedia.org/wiki/Simplified_Technical_English —
> licensed **CC BY-SA 4.0**. This file (the STE-derived content) therefore carries **CC BY-SA 4.0**.
> **ASD-STE100** itself is **© ASD** (AeroSpace and Defence Industries Association of Europe) and a
> registered trademark; do **not** reproduce the ASD-STE100 dictionary. Use this summary + your own words.
>
> Current standard: **Issue 9 (January 2025) — 53 writing rules + ~900 approved words.**

## Applicable writing rules (summary)
Use these when generating the **ASD-STE100** level of an ELI-X explanation:

- Use approved words **only as the part of speech and meaning** given in the dictionary ("one word, one part of speech, one meaning").
- **Make instructions clear and specific.**
- **Write one instruction per sentence.**
- **Short sentences:** ≤ **20 words** for instructions (procedures); ≤ **25 words** for descriptive text.
- **Approved verb forms only:** infinitive, imperative, simple present, simple past, simple future, and the past participle **only as an adjective**.
- **Do not use auxiliary verbs** to build complex verb constructions (no perfect/continuous stacks).
- Use the **active voice**. In descriptive writing, use the passive **only when the agent is unknown**.
- Use the **"-ing" form only** as a technical noun or as a modifier in a technical noun.
- **Do not write multi-word nouns longer than 3 words.**
- **Do not omit** the subject, verb, or article to make a sentence shorter.
- Use **vertical lists** for complex text.
- **One topic per paragraph; no more than 6 sentences per paragraph.**
- **Start safety instructions with the command or the condition.**

## Canonical before → after examples (illustrations via Wikipedia)
| Non-STE (before) | STE (after) | Rule shown |
|---|---|---|
| Before **acceptance** of unit, do the specified test procedure. | Before you **accept** the unit, do the specified test procedure. | approved part of speech (verb *accept*, not noun *acceptance*) |
| **Rotate** the cover until the jacks marked + and − are **accessible**. | **Turn** the cover until you can **get access to** the jacks that have + and − marks. | approved word (*turn*, not *rotate*); *accessible* → *get access* |
| do not go **close** to the landing gear | do not go **near** the landing gear | *close* (adj) is not approved → *near* |

(The ASD-STE100 manual writes procedures in UPPERCASE; for software docs we keep normal case.)

## Applying STE to software / agent output (original examples)
| Before | After | Rule |
|---|---|---|
| The service will have completed the sync and may then purge stale entries. | The service completes the sync. Then the service removes the stale entries. | simple tenses; one instruction per sentence; active voice |
| Ensure accessibility of the config prior to initialization. | Make sure that you can get access to the config before you start the service. | approved parts of speech; no nominalization |
| The outbound-request-retry-backoff-policy applies. | The retry policy for outbound requests applies. | no multi-word noun longer than 3 words |

## Do-not-over-correct guardrails
- Keep a genuine hedge (e.g., "may") when the uncertainty is **real** — do not delete real uncertainty just to satisfy the simple-tense rule.
- Never add a cause, mechanism, or frequency the source did not state. Adding new facts is no longer a rewrite.
- Keep exact **Technical Names / identifiers** (e.g., `cghdId`, a log-group name) verbatim — STE permits approved Technical Names.

## Source
- Wikipedia, "Simplified Technical English" (CC BY-SA 4.0): https://en.wikipedia.org/wiki/Simplified_Technical_English
- Official standard (© ASD): https://asd-ste100.org/
