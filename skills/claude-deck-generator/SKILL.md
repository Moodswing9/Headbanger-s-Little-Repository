# Claude Deck Generator

AI-powered presentation generator. Produces polished `.pptx` or `.html` slide decks from a single topic sentence using Claude Opus 4.6 with extended thinking.

## Trigger

Activate this skill when the user wants to:
- Generate a new presentation, deck, or slideshow
- Remix or rebuild an existing `.pptx` file
- Ask about slide themes, formats, or generator options
- Run `/deck` or `/deck-remix`

## Capabilities

| Feature | Command flag |
|---------|-------------|
| Generate `.pptx` (default) | *(default)* |
| Generate `.html` | `--format html` |
| Choose theme | `--theme dark\|light\|corporate\|executive` |
| Set slide count (4–20) | `--slides N` |
| Embed Unsplash photos | `--images` (needs `UNSPLASH_ACCESS_KEY`) |
| Remix an existing deck | `--remix path/to/file.pptx` |
| Omit speaker notes | `--no-notes` |

## Setup

```bash
cd path/to/claude-deck-generator
pip install -r requirements.txt
export ANTHROPIC_API_KEY=sk-ant-...
# Optional — enables --images flag
export UNSPLASH_ACCESS_KEY=your-unsplash-access-key
```

## Usage

```bash
python generate.py "Your Topic"
python generate.py "Q4 Business Review" --theme corporate --slides 10
python generate.py "Heavy Metal History" --format html --theme dark
python generate.py "Product Roadmap" --remix current_deck.pptx
```

Or launch the Streamlit web UI:

```bash
streamlit run app.py
```

## How it Works

1. `generate_content(topic)` calls Claude Opus 4.6 with `thinking: {type: "adaptive"}` and a strict `SLIDE_SCHEMA` — guarantees structured output every time.
2. `ingest_pptx(path)` (remix mode) uses MarkItDown to extract content from an existing deck and passes it as `<reference_deck>` context to Claude.
3. `build_pptx()` or `build_html()` renders the structured output into a file — all elements drawn manually, no PowerPoint layout placeholders.

## Themes

| Theme | Description |
|-------|-------------|
| `dark` | Dark background with light text (default) |
| `light` | Light background with dark text |
| `corporate` | Professional blue/slate palette |
| `executive` | Warm off-white with gold accent — boardroom ready |

## Key Constraints

- `ANTHROPIC_API_KEY` must be set — raises `AuthenticationError` without it.
- Slide count is clamped to 4–20.
- `--images` requires `UNSPLASH_ACCESS_KEY`.
- The project root contains pre-built pitch, objection, and Q&A decks as reference materials.
