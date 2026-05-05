---
description: Remix an existing .pptx file into a new polished deck using Claude AI
allowed-tools: ["Bash"]
---

Remix an existing presentation file into a new deck.

**Parse the user's arguments:**
- First positional arg = path to the source `.pptx` file
- Optional topic override (quoted string after the path)
- `--theme dark|light|corporate|executive` (default: `dark`)
- `--format pptx|html` (default: `pptx`)
- `--slides N` (default: 12, range 4–20)
- `--no-notes` flag = omit speaker notes

If no file path is given, ask: "Which .pptx file should I remix? Provide the path."

**Run the generator in remix mode:**

```bash
cd path/to/claude-deck-generator
python generate.py "<TOPIC_OR_ORIGINAL_TITLE>" --remix "<SOURCE_FILE>" [--theme THEME] [--format FORMAT] [--slides N] [--no-notes]
```

Replace `path/to/claude-deck-generator` with the actual repo path on this machine.
If no topic override is given, use the source filename (without extension) as the topic.

**After completion:**
- Report the output filename and full path
- Briefly describe what changed (Claude extracts the source content and rebuilds it fresh)
- If the source file was not found, show the exact error and ask the user to check the path
