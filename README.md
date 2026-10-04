# eli-x-explainer

A Kiro/agent **skill** that explains any topic in a chosen presentation format, with an interactive
**"ELI-X" slider** that rewrites the same explanation across five complexity levels — so one page
serves a child, a teammate, and a domain expert without changing a single fact.

> The facts stay identical across every level and format. Only the vocabulary, depth, and presentation change.

## Demo
Try the ready-made sample — **[`examples/sample-eli-x.html`](examples/sample-eli-x.html)** ("What is caching?"): download it, open in any browser, and drag the slider from **ELI5 → Expert**.

<!-- After you capture a screen recording of the slider, save it as assets/demo.gif and uncomment:
![ELI-X slider demo](assets/demo.gif)
-->

## The five levels
`ELI5 → ELI12 → General → ASD-STE100 → Expert`

- **ELI5 / ELI12** — plain, friendly, minimal jargon.
- **General** — clear professional prose (default slider position).
- **ASD-STE100** — Simplified Technical English (short active sentences, approved verb forms, one idea per sentence). See [`references/ste-rules.md`](references/ste-rules.md).
- **Expert** — full depth: queries, data sources, edge cases, caveats.

## Output formats
1. **HTML ELI-X slider** (default) — a single self-contained HTML file (inline CSS/JS, works offline).
2. **Diagram** — inline SVG (simple) or Mermaid (complex).
3. **Animated walkthrough** — auto-play steps with Play/Next controls and optional in-browser voice-over.
4. **Explainer video** — in-browser auto-play "video" by default; true MP4 only via external tooling (Remotion / manim / ffmpeg).

## Hard rules (what keeps it trustworthy)
- **Facts invariant** across levels — simplify words, never the facts.
- **No fabrication** — only include verified or user-supplied facts.
- **Self-contained + offline** — one HTML file, no external CDNs.
- **Source footer** — every artifact states where its facts came from.
- **Real ASD-STE100** — the STE level follows the actual STE rules, not just shortened prose.

## Install
> **Note:** The paths below are for the **Kiro** agent. The skill itself (a `SKILL.md` with YAML frontmatter + `references/`) is harness-agnostic — if you use a different agent harness (Claude Code, Cursor, Codex, MeshClaw, etc.), copy the skill into **that harness's skills/context directory** instead, per its own docs.

**Kiro:**
```bash
# user-wide (all workspaces)
cp -R eli-x-explainer ~/.kiro/skills/

# or workspace-local
cp -R eli-x-explainer <your-project>/.kiro/skills/
```

## Use
Ask naturally, e.g.:
- "explain &lt;topic&gt; with an ELI-X slider"
- "explain &lt;topic&gt; at multiple levels"
- "make a diagram / an animated walkthrough of &lt;topic&gt;"

The skill writes a self-contained `.html` file and gives you a `file://` URL to open.

## Repository structure
```
eli-x-explainer/
├── SKILL.md                 # the skill (workflow, rules, template, examples)
├── references/
│   └── ste-rules.md         # ASD-STE100 rules + examples (CC BY-SA 4.0)
├── examples/
│   └── sample-eli-x.html    # ready-made demo ("What is caching?")
├── README.md
└── LICENSE                  # MIT
```

## License & attribution
- **Code / skill content:** [MIT](LICENSE) © 2026 pahadiprogrammer.
- **`references/ste-rules.md`:** adapted from [Wikipedia, "Simplified Technical English"](https://en.wikipedia.org/wiki/Simplified_Technical_English), licensed **CC BY-SA 4.0** — that file carries CC BY-SA 4.0 (share-alike) and retains attribution.
- **ASD-STE100** is **© ASD** (AeroSpace and Defence Industries Association of Europe) and a registered trademark. This repo summarizes the rules in its own words and does **not** reproduce the ASD-STE100 dictionary.
