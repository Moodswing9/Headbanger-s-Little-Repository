---
description: Generate a polished presentation from a topic using Claude AI
allowed-tools: ["Bash"]
---

Generate a presentation from the topic the user provided.

**Parse the user's arguments:**
- First positional arg (or quoted string) = topic
- `--theme dark|light|corporate|executive` (default: `dark`)
- `--format pptx|html` (default: `pptx`)
- `--slides N` (default: 12, range 4–20)
- `--images` flag = embed Unsplash photos (needs `UNSPLASH_ACCESS_KEY`)
- `--no-notes` flag = omit speaker notes
- `--output FILE` = custom output filename

If no topic is given, ask the user: "What topic should the deck cover?"

**Run the generator:**

```bash
cd path/to/claude-deck-generator
python generate.py "<TOPIC>" [--theme THEME] [--format FORMAT] [--slides N] [--images] [--no-notes] [--output FILE]
```

Replace `path/to/claude-deck-generator` with the actual repo path on this machine.

**After completion:**
- Report the output filename and full path
- State the theme, format, and slide count used
- If `--format html` was used, mention the user can open it directly in a browser
- If the command failed because `ANTHROPIC_API_KEY` is not set, tell the user to run: `export ANTHROPIC_API_KEY=sk-ant-...`
