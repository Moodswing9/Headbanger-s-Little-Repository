---
description: Generate a polished presentation from a topic using Claude AI — with smart draft-mode suggestions for large or complex decks
argument-hint: '"Your Topic" [--theme dark|light|corporate|executive] [--format pptx|html] [--slides N] [--draft] [--images] [--no-notes] [--output FILE]'
allowed-tools: ["Bash"]
---

Generate a presentation from the topic the user provided.

## Step 1 — Parse arguments

From `$ARGUMENTS` extract:
- **Topic** — first positional arg or quoted string (required)
- `--theme` — `dark` (default) · `light` · `corporate` · `executive`
- `--format` — `pptx` (default) · `html`
- `--slides N` — integer 4–20 (default 12)
- `--draft` — two-phase mode: Haiku previews slide outline + cost, press Enter to upscale to Opus
- `--images` — embed Unsplash photos (needs `UNSPLASH_ACCESS_KEY`)
- `--no-notes` — omit speaker notes
- `--output FILE` — custom output filename

If no topic is given, ask: "What topic should the deck cover?"

> **⚠ Draft mode — interactive only:** If `--draft` is present in `$ARGUMENTS`, stop immediately before running anything and tell the user:
> *"Draft mode waits for you to press Enter before upscaling to Opus — Claude Code can't respond to that prompt interactively. Run this directly in your terminal instead:*
> `python generate.py "<topic>" --draft [any other flags]`*"*
> Do not proceed past this step when `--draft` is present.

## Step 2 — Validate arguments

Before running:
- If `--theme` was supplied, confirm it is one of `dark | light | corporate | executive`. If not, correct it to `dark` and note the correction.
- If `--slides` was supplied, clamp to 4–20 and note if adjusted.
- If `--format` was supplied, confirm it is `pptx` or `html`.

## Step 3 — Suggest draft mode when appropriate

If `--draft` was **not** explicitly passed, recommend it (but still run without it unless the user asks to pause):

- Slide count ≥ 15, or
- Topic is a complex domain (technical architecture, financial analysis, legal/compliance, research overview)

In those cases, add a note after completion: *"Tip: next time add `--draft` to preview the outline and cost before the full Opus run."*

## Step 4 — Run the generator

```bash
cd /path/to/claude-deck-generator
python generate.py "<TOPIC>" [--theme THEME] [--format FORMAT] [--slides N] [--draft] [--images] [--no-notes] [--output FILE]
```

Replace `/path/to/claude-deck-generator` with the actual repo path on this machine.

**Draft mode behaviour:** when `--draft` is passed, the generator prints the slide outline and cost estimate, then waits for Enter. Claude Code cannot press Enter interactively — so tell the user: *"Draft mode requires interactive input. Run this command directly in your terminal: `python generate.py <TOPIC> --draft`"* — and stop.

## Step 5 — Report results

- Output filename and full path
- Theme, format, and slide count used
- If `--format html`: "Open `<filename>` directly in any browser — no server needed"
- If `ANTHROPIC_API_KEY` is missing: "Set it with `export ANTHROPIC_API_KEY=sk-ant-…`"
- If `--slides` or `--theme` was corrected, state what was changed and why
