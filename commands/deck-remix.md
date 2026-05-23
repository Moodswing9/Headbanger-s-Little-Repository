---
description: Remix an existing .pptx into a fresh polished deck — with optional Phi-4 + DePlot vision pass for image-heavy source files
argument-hint: '<source.pptx> ["Topic Override"] [--theme dark|light|corporate|executive] [--format pptx|html] [--slides N] [--draft] [--vision] [--no-notes]'
allowed-tools: ["Bash"]
---

Remix an existing presentation into a new deck.

## Step 1 — Parse arguments

From `$ARGUMENTS` extract:
- **Source file** — first positional arg ending in `.pptx` (required)
- **Topic override** — optional quoted string after the source path; if omitted, use the source filename (without extension) as the topic
- `--theme` — `dark` (default) · `light` · `corporate` · `executive`
- `--format` — `pptx` (default) · `html`
- `--slides N` — integer 4–20 (default 12)
- `--draft` — Haiku previews structure + cost, then upscale to Opus
- `--vision` — run Phi-4 (image descriptions) + DePlot (chart tables) on every embedded image before regeneration; requires `NVIDIA_API_KEY`
- `--no-notes` — omit speaker notes

If no source file is given, ask: "Which `.pptx` file should I remix? Provide the path."

## Step 2 — Validate

- Confirm the source file exists using the Bash tool (`python -c "open('<path>')"` or `test -f`). If it doesn't exist, stop with the exact path and error.
- If `--theme` is not one of `dark | light | corporate | executive`, correct to `dark` and note it.
- If `--slides` is outside 4–20, clamp and note the adjustment.
- If `--vision` is requested but `NVIDIA_API_KEY` is not set in the environment, warn: *"Vision pass requires `export NVIDIA_API_KEY=nvapi-…` — running text-only remix instead."* and drop `--vision`.

## Step 3 — Suggest vision mode when appropriate

If `--vision` was **not** explicitly passed and the source file is large (> 2 MB) or the topic involves design, financial charts, diagrams, or data visualisation, add a note after completion: *"Tip: add `--vision` next time to let Phi-4 describe slide images and DePlot extract chart tables — the AI will have richer context for reconstruction."*

## Step 4 — Run the generator in remix mode

```bash
cd /path/to/claude-deck-generator
python generate.py "<TOPIC>" --remix "<SOURCE_FILE>" [--theme THEME] [--format FORMAT] [--slides N] [--draft] [--vision] [--no-notes]
```

Replace `/path/to/claude-deck-generator` with the actual repo path on this machine.

**Draft mode:** when `--draft` is passed, the generator waits for Enter after displaying the outline. Claude Code cannot respond to interactive prompts — tell the user: *"Run this in your terminal directly: `python generate.py '<TOPIC>' --remix '<SOURCE>' --draft`"* — and stop.

**Vision timing:** vision runs all images in parallel (`asyncio.gather`). Estimate ~30 seconds for a 20-image deck, ~5 seconds for a 5-image deck. Mention this to the user before starting so they know to wait.

## Step 5 — Report results

- Output filename and full path
- Whether vision pass ran (and how many images were processed)
- Theme, format, and slide count used
- If `ANTHROPIC_API_KEY` is missing: "Set it with `export ANTHROPIC_API_KEY=sk-ant-…`"
- If `NVIDIA_API_KEY` was missing and `--vision` was requested, reiterate how to enable it
