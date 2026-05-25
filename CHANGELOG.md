# Changelog

## [4.5.0] — 2026-05-01
### Added
- NVIDIA Palmyra-Creative-122B provider (`--provider nvidia`) via NVIDIA NIM
- Vision pass for `--remix`: Phi-4 multimodal describes embedded slide images; DePlot extracts chart data into tables
- Unsplash photo embedding (`--images`) — searches and embeds relevant photos per slide
- Streamlit web UI (`app.py`) — browser-based frontend for the generator
- GitHub Actions CI — pytest on Python 3.11 and 3.12
- `--slides N` flag to control slide count (4–20, default 12)
- `--no-notes` flag to omit speaker notes

### Changed
- `generate_content()` uses `thinking: {type: "adaptive"}` for deeper reasoning on complex topics

## [4.0.0] — 2026-04-10
### Added
- Claude Haiku 4.5 fast-draft provider (`--provider claude-haiku`) — 4,000-token ceiling, no adaptive thinking, same output schema
- `--slides` count control clamped to 4–20

## [3.0.0] — 2026-03-20
### Added
- `--remix old.pptx` — ingest existing deck via MarkItDown, rebuild with Claude while preserving structure and content

## [2.0.0] — 2026-03-10
### Added
- HTML output format (`--format html`) — self-contained single file with CSS custom properties
- 4 themes: `corporate`, `dark`, `minimal`, `vibrant`
- Speaker notes in PPTX and HTML output
- Custom slide builders: closing slide, objection handling deck, Q&A prep deck, visual direction brief

## [1.0.0] — 2026-03-01
### Added
- Claude Opus 4.6 with adaptive thinking generates structured slide content
- PPTX output via `python-pptx` (blank layouts, all elements drawn manually)
- `SLIDE_SCHEMA` enforcing consistent JSON structure: title, subtitle, bullets, notes, quote, stat slide types
- Auto-derived output filename from topic
