---
name: explain
description: "Explain how something works as a published HTML page or diagram, grounded in the current project's own code, with every claim linked to file:line. Use when the user types /explain, or asks to understand or visualise how part of their codebase works ('how does this work', 'walk me through this flow', 'I forgot how this works', 'bu nasıl çalışıyor', 'bana anlat', 'akışı göster')."
trigger: /explain
---

# /explain

Turn "how does X work?" into a disposable visual page the user reads in a browser, instead of a wall of terminal text.

Idea: Andrej Karpathy, 2026-10-02 (https://x.com/karpathy/status/2105819303471976479). As agents do more of the work, our job shifts to understanding their output, so ask for richer output formats than prose.

## Usage

```
/explain <topic>                        # default: page
/explain page <topic>                   # interactive HTML page   (TR: sayfa)
/explain diagram <topic>                # one focused SVG diagram (TR: diyagram)
```

`<topic>` is usually a part of the current project ("checkout flow", "how widget screenshots are uploaded"). If it has no relation to the codebase, explain it generally and skip step 1.

## Steps

1. **Read the code first. Never explain from memory or from file names.**
   - Find the entry points (routes, commands, jobs, components) with grep/glob, then follow the real call path.
   - Note every claim you will make with its `path:line`. If you cannot point to a line, you do not know it: read more, or mark it as inferred.
   - Stop when you can trace the topic end to end. Do not explain the whole codebase.

2. **Load the design skills if they are listed.** `artifact-design` always; `artifact-diagramming` too for `diagram`, or when the page contains a diagram. If they are not available, still design both light and dark themes, keep the page readable at phone width, and draw diagrams as inline SVG.

3. **Write the page** to the session's scratchpad directory, or to `~/explainers/` when there is none. Never write into the project: no explainer files in repos.
   - Use the project's real names: actual class, method, route, table and variable names. A generic example the user has to map back to their code is a failure.
   - **page**: open with a 2–3 sentence summary, then a diagram of the flow, then step-by-step sections the user can expand. Interaction should help understanding (click a step → see the code and what happens), not decorate.
   - **diagram**: one diagram with a one-line title and short labels. Clicking or hovering a node may reveal its `path:line`.
   - End every page with a **Claims / Sources** section (Turkish: **İddialar / Kaynaklar**): each factual claim → `path:line`. Mark anything inferred rather than read as *inferred* (*çıkarım*). A wrong claim inside a nice visual is harder to catch than a wrong sentence; this section is how the user checks it.

4. **Publish** with the Artifact tool and give the user the link with a 1–2 sentence summary in chat. Do not repeat the page content in the terminal.
   - If the Artifact tool is not available (it depends on the account), write the page as a complete standalone HTML file (with `<!doctype html>`, `<head>`, `<body>`) to `~/explainers/<topic-slug>.html`, open it with `open` (macOS) or `xdg-open` (Linux), and give the user the path.

## Writing rules

Write in the user's language. Use a simplified technical style, loosely after ASD-STE100:
- Short sentences. One idea per sentence.
- Active voice: say who does what ("the `SubmitFeedback` job uploads the file to S3").
- One term per concept; do not switch between synonyms.
- Code names stay exactly as in code, in `monospace`, never translated.
- No filler, no marketing tone.
