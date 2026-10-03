---
name: eli-x-explainer
description: Produce an explanation of any topic in a chosen output format — (1) a self-contained HTML page with an interactive "ELI-X" slider across five complexity levels (ELI5 → ELI12 → General → ASD-STE100 → Expert), (2) a diagram, (3) an animated walkthrough, or (4) an explainer video. Use when the user asks to "explain with an ELI-X slider", "make an ELI-X explanation", "explain at multiple levels", wants a complexity slider, a diagram/animation of a concept, or an explainer video. The facts stay identical across levels/formats; only vocabulary, depth, and presentation change. ASD-STE100 (Simplified Technical English) is always one of the HTML slider levels.
version: 1.0.0
tags: [skill, explainer, eli-x, html, documentation, asd-ste100]
---

# ELI-X Explainer

## Overview
This skill renders ONE explanation at FIVE complexity levels inside a single, self-contained HTML page. A range slider ("ELI-X") lets the reader move from a child-simple version up to a full expert version. The underlying facts (numbers, names, identifiers) MUST be identical at every level — only the vocabulary, sentence length, and depth change.

Output is a single `.html` file (inline CSS + JS, zero dependencies, no server required), opened via a `file://` URL.

## Usage
Use this skill when the user asks to:
- "Explain X with an ELI-X slider" / "make an ELI-X explanation of X"
- "Explain this at multiple levels" or wants a complexity/difficulty slider
- Re-render a previous answer as an interactive multi-level page
- Produce an explanation that includes an ASD-STE100 (Simplified Technical English) variant

## Output formats
Offer the user a choice up front (or infer it from their words). EVERY format obeys the same hard rules below: facts invariant, no fabrication, self-contained + offline, and a source footer.

| # | Format | What it is | How to produce | Notes |
|---|--------|-----------|----------------|-------|
| 1 | **HTML ELI-X slider** (default) | Self-contained HTML; a slider rewrites the explanation across the 5 complexity levels, **including ASD-STE100** | The template in this skill | The signature format; ASD-STE100 is always a level |
| 2 | **Diagram** | A visual: flowchart / sequence / architecture / state | Inline **SVG** for simple diagrams (true offline); **Mermaid.js** for complex ones (vendor the lib locally, or accept a CDN and note the offline tradeoff) | Optionally pair with a short ELI-X caption |
| 3 | **Animation** | A step-through animated walkthrough (animated SVG or CSS/JS) with Play / Next controls that reveal the explanation in stages | Self-contained HTML + CSS/JS; optional in-browser voice-over via the Web Speech API (`speechSynthesis`) | Good for processes and data flow |
| 4 | **Explainer video** | Video-like output | See the honesty note below | True MP4 needs external tooling |

### Explainer video — be honest about what is actually possible
- **In-tool, no extra dependencies (recommended default for "video"):** a self-contained HTML page that **auto-plays** through narrated steps with timing + animation, optionally with **browser speech-synthesis** voice-over. It looks like a video and ships as one file.
- **Script / storyboard:** produce a scene-by-scene narration script + visual directions that a human or a tool can record.
- **True MP4:** requires an external renderer — e.g. **Remotion** (React→MP4), **manim** (math/code animations), or render HTML frames + **ffmpeg**. This skill does NOT produce MP4 inline. Only go this route if the tool is installed AND the user approves (it runs a real build/render step). Do not claim an MP4 was produced unless one actually was.

## The five levels (fixed order)
| Index | Level | Audience & rules |
|------|-------|------------------|
| 0 | **ELI5** | A young child. Everyday words only, short concrete sentences, friendly analogies. No jargon, no identifiers unless unavoidable. |
| 1 | **ELI12** | A young teen. Plain language, one new idea at a time, minimal jargon (define any term inline). |
| 2 | **General** | A non-technical professional. Clear prose, correct terms used but not deep internals. This is the DEFAULT slider position. |
| 3 | **ASD-STE100** | Simplified Technical English. Short sentences: **≤20 words for instructions, ≤25 for descriptive**. One instruction per sentence. Active voice (passive only in descriptions when the agent is unknown). Approved verb forms only (infinitive, imperative, simple present/past/future, past participle as adjective); no auxiliary complex tenses. No multi-word noun longer than 3 words. Keep subject/verb/article (do not drop to shorten). Consistent terminology (no synonyms). Technical Names (exact identifiers like `cghdId`) allowed verbatim. **Full rule list: `references/ste-rules.md`.** |
| 4 | **Expert** | A domain engineer. Full depth: exact queries/commands, data sources, edge cases, pitfalls, correctness caveats, identities/IDs. |

## Hard rules (do NOT violate)
1. **Facts are invariant.** The same numbers, dates, IDs, and conclusions appear at every level. If a number is 393 at Expert, it is 393 at ELI5. Never simplify by changing a fact — only by changing words.
2. **No fabrication.** Only include facts you have verified or that the user supplied. If a level would need detail you don't have, keep it at the honest level of certainty — do not invent internals to fill the Expert tier.
3. **Self-contained + offline.** One `.html` file. Inline `<style>` and `<script>`. No external CDNs, fonts, or network calls.
4. **Source/meta footer.** Always include a small footer stating the source of the facts (log group / query / doc / account+region, or "user-provided"), so the page is auditable.
5. **ASD-STE100 tier really follows STE.** Rewrite to STE — do not just shorten the General text: short sentences (**≤20 words instructions / ≤25 descriptive**), one instruction per sentence, active voice, approved verb forms, no multi-word noun longer than 3 words. See `references/ste-rules.md` for the full rule list and examples.
6. **Accessibility.** The slider has an `aria-label`; level name is shown as text (not color only); the active tick is marked with text weight, not just color.

## Steps

### 1. Choose the output format, then gather the canonical facts
First pick the output format (see **Output formats**): HTML ELI-X slider (default), diagram, animation, or explainer video. Ask if it's unclear. For "explainer video", confirm whether they want the in-browser auto-play version (no extra deps) or a true MP4 (needs external tooling + their go-ahead).

Then identify what to explain and the concrete facts (numbers, names, dates, IDs, source). If the facts came from a query/log/doc earlier in the session, reuse them verbatim. If anything is missing or unverified, ask or mark it as unverified — never guess to pad a level or a scene.

### 2. Write the five variants
Draft each level from the SAME fact set. Check that every fact present in one level that is also relevant to another is stated consistently. Apply the per-level rules in the table above. Enforce real ASD-STE100 for level 3.

### 3. Produce the artifact for the chosen format
- **HTML ELI-X slider:** fill the template below. Put each level's body as an HTML string in the `LEVELS` array (`name` + `html`). Keep facts in the meta footer.
- **Diagram:** emit inline SVG (simple) or a Mermaid-rendered HTML page (complex); keep it self-contained and add the source footer.
- **Animation:** build a self-contained HTML page with Play/Next controls that reveal stages; optional `speechSynthesis` voice-over; source footer.
- **Explainer video:** default to the in-browser auto-play HTML (per the honesty note); only produce a true MP4 if an external renderer is installed and the user approved.

Keep facts identical across every level/scene regardless of format.

### 4. Save and hand off a URL
Write the file (ask for a path, or default to a sensible location such as the current working directory or `/Volumes/workplace/health/<topic>-eli-x.html`). Give the `file://` URL. Optionally offer a local `http://` server (`python3 -m http.server`) if they want to view it from another device.

### 5. Verify
Confirm the file written is valid HTML, the slider has `min=0 max=4`, there are exactly five `LEVELS` entries in the fixed order, and the facts match across levels. Mention that reloading the browser tab picks up changes.

## Template
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{TITLE}} — ELI-X</title>
  <style>
    body { font-family: Arial, Helvetica, sans-serif; max-width: 820px; margin: 2rem auto; padding: 0 1rem; line-height: 1.55; color: #222; }
    h1 { border-bottom: 2px solid #444; padding-bottom: .3rem; }
    .controls { background: #f6f8fa; border: 1px solid #d0d7de; border-radius: 8px; padding: 1rem 1.2rem; margin: 1.2rem 0; }
    .controls label { font-weight: bold; display: block; margin-bottom: .6rem; }
    input[type="range"] { width: 100%; }
    .ticks { display: flex; justify-content: space-between; font-size: .72rem; color: #555; margin-top: .3rem; }
    .ticks span { flex: 1; text-align: center; }
    .ticks span.active { color: #0a558c; font-weight: bold; }
    #level-name { color: #0a558c; }
    #content { margin-top: 1.4rem; }
    #content h2 { color: #0a558c; margin-top: 1.4rem; }
    code { background: #eef1f4; padding: 1px 5px; border-radius: 3px; font-size: .9em; }
    pre { background: #0d1117; color: #e6edf3; padding: .8rem; border-radius: 6px; overflow-x: auto; }
    pre code { background: none; color: inherit; }
    .meta { font-size: .8rem; color: #666; border-top: 1px solid #eee; margin-top: 2rem; padding-top: .6rem; }
  </style>
</head>
<body>
  <h1>{{TITLE}}</h1>
  <p>Current level: <strong id="level-name">General</strong></p>
  <div class="controls">
    <label for="eli">ELI-X — slide to change the explanation level</label>
    <input type="range" id="eli" min="0" max="4" step="1" value="2" aria-label="Explanation level">
    <div class="ticks">
      <span data-i="0">ELI5</span><span data-i="1">ELI12</span><span data-i="2">General</span><span data-i="3">ASD-STE100</span><span data-i="4">Expert</span>
    </div>
  </div>
  <div id="content"></div>
  <p class="meta">{{SOURCE_FOOTER}}</p>
  <script>
    const LEVELS = [
      { name: "ELI5",       html: `{{ELI5_HTML}}` },
      { name: "ELI12",      html: `{{ELI12_HTML}}` },
      { name: "General",    html: `{{GENERAL_HTML}}` },
      { name: "ASD-STE100", html: `{{STE_HTML}}` },
      { name: "Expert",     html: `{{EXPERT_HTML}}` }
    ];
    const slider = document.getElementById('eli');
    const content = document.getElementById('content');
    const levelName = document.getElementById('level-name');
    const ticks = document.querySelectorAll('.ticks span');
    function render(i) {
      content.innerHTML = LEVELS[i].html;
      levelName.textContent = LEVELS[i].name;
      ticks.forEach(t => t.classList.toggle('active', Number(t.dataset.i) === i));
    }
    slider.addEventListener('input', e => render(Number(e.target.value)));
    render(Number(slider.value));
  </script>
</body>
</html>
```

## Examples

These show the two things that go wrong most: facts drifting between levels, and the ASD-STE100 tier being fake STE. Facts below are illustrative.

### Example 1 — one fact, five levels (facts stay invariant)
Fact set: *393 patients consented; a deletion removes database records but not log lines.*

| Level | Rendering |
|-------|-----------|
| ELI5 | "393 people said yes. When we erase someone, we erase their card but keep the note that says they once said yes." |
| ELI12 | "393 users turned on consent. Deleting a user clears the database but not the logs." |
| General | "393 distinct patients consented. Deletion removes database records but not log lines." |
| ASD-STE100 | "393 patients gave consent. A deletion removes the database records. A deletion does not remove the log lines." |
| Expert | "`count_distinct(cghdId)=393`. Deletion drops the DDB mapping; CloudWatch log lines persist, so log-based counts still include deleted patients." |

**Why it's correct:** `393` is identical at every level — only vocabulary and depth change. The deletion caveat survives all five levels; no level softens or drops it.

### Example 2 — ASD-STE100 (canonical rules, from the STE dictionary)
Standard STE illustrations (adapted from Wikipedia, CC BY-SA 4.0; full list + attribution in `references/ste-rules.md`). They show the "one word, one part of speech" and approved-word rules:

| Non-STE (before) | STE (after) | Rule shown |
|---|---|---|
| "Before **acceptance** of unit, do the specified test procedure." | "Before you **accept** the unit, do the specified test procedure." | approved part of speech (verb *accept*, not noun *acceptance*) |
| "**Rotate** the cover until the jacks marked + and − are **accessible**." | "**Turn** the cover until you can **get access to** the jacks that have + and − marks." | approved word (*turn*, not *rotate*); *accessible* → *get access* |
| "do not go **close** to the landing gear" | "do not go **near** the landing gear" | *close* (adj) is not approved → *near* |

(The ASD-STE100 manual writes procedures in UPPERCASE; for software docs we keep normal case.)

### Example 2b — the same rules applied to software/agent output (original)
| Before | After | Rule |
|---|---|---|
| "The service will have completed the sync and may then purge stale entries." | "The service completes the sync. Then the service removes the stale entries." | simple tenses; one instruction per sentence; active voice |
| "Ensure accessibility of the config prior to initialization." | "Make sure that you can get access to the config before you start the service." | approved parts of speech; no nominalization |
| "The outbound-request-retry-backoff-policy applies." | "The retry policy for outbound requests applies." | no multi-word noun longer than 3 words |

**Do not over-correct / do not invent:** keep a genuine hedge like "may" when the uncertainty is real (do not delete real uncertainty just to satisfy the simple-tense rule), and never add a cause, mechanism, or frequency the source did not state — adding new facts is no longer a rewrite.

### Example 3 — anti-patterns (wrong → right)
| ✗ Wrong | ✓ Right | Rule broken |
|--------|---------|-------------|
| ELI5 says "about 400 people" while Expert says 393 | Say **393** at every level | Facts invariant |
| Expert tier adds "scanned 78.9M records" when no query was run | Only include values you actually measured; otherwise omit | No fabrication |
| "Here's your explainer video: `output.mp4`" when no renderer ran | Ship the in-browser auto-play HTML (or a script), and state no MP4 was rendered | Honest video claim |
| ASD-STE100 tier is just the General text with commas removed | Rewrite to real STE: short active sentences, one idea each | STE tier must be real STE |

## Common mistakes
| Mistake | Fix |
|---|---|
| Changing a number/fact between levels to "simplify" | Keep facts identical; simplify only words |
| ASD-STE100 tier is just shortened prose | Rewrite to real STE (short active sentences, one idea each) |
| Padding the Expert tier with invented internals | Only include verified detail; stop at honest certainty |
| External CSS/JS/CDN links | Inline everything; the file must work offline |
| No source footer | Always state where the facts came from (auditable) |
| Backticks inside a level's HTML break the JS template literal | Escape or avoid backticks inside the `html` strings; use `<code>` tags |

## Reference & attribution
- **ASD-STE100** = ASD Simplified Technical English Specification — a controlled language (restricted grammar + ~900 approved words). Current edition: Issue 9, Jan 2025 (53 rules). The standard is **© ASD** and a registered trademark; do **not** reproduce its dictionary.
- The STE **rules and canonical examples** in this skill are adapted from **Wikipedia, "Simplified Technical English"** (https://en.wikipedia.org/wiki/Simplified_Technical_English), licensed **CC BY-SA 4.0**. The attributed extract lives in `references/ste-rules.md`; the share-alike terms apply to that STE-derived content.
- The before→after *example format* was inspired by the public `danyuchn/asd-ste100-skill` repo (verify its license before redistributing).
