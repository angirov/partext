# partext
Browser tools for assembling foldable, multi-language parallel texts.

## Quick start (browser editor)
- Serve the folder locally (required so the editor can fetch `template.html`): `python -m http.server 8000` then open `http://localhost:8000/index.html`.
- Use the top bar to name the section, add language codes, and insert batches of sentences (+5/+10/+50). Mark a sentence as a root text when needed.
- Load an existing file with **Load JSON** (prompts before discarding unsaved work).
- Export with **Download JSON** (saves the editor state) or **Download HTML** (fills `template.html`, optionally inlines `styles.css`). The HTML download will fail if `template.html` is missing or cannot be fetched.

## JSON shape accepted by the tools
- Preferred: an array of sections, each `{ "sectionTitle": string, "sentences": [...] }`.
- Sentences support: `original` (string), `translations` (object keyed by language), optional `root` (boolean).
- Alternate shorthand accepted by `generate_parallel_html.py`: a flat array of sentences, with optional `section` to group items and `translation` (string or object) if you only have one language; `--default-section` and `--default-lang` fill the gaps.
- Example:
```json
[
  {
    "sectionTitle": "Sample",
    "sentences": [
      {
        "original": "First line.",
        "translations": { "en": "First line.", "ru": "Первая строка." },
        "root": true
      }
    ]
  }
]
```

## CLI generation
- Convert JSON to a self-contained HTML page using the template placeholder `__DATA__`:
  ```bash
  python generate_parallel_html.py vimsika_15-22.json template.html output.html
  ```
- Options: `--default-section "Text"` for flat lists, `--default-lang en` for single-language `translation` fields.

## Template expectations
- `template.html` must contain the string `__DATA__` where the JSON array should be inserted (for example: `const data = __DATA__;` inside a `<script>` tag) and usually links `styles.css` so the viewer matches the editor.
- The provided `vimsika_15-22.html` shows the rendered result for `vimsika_15-22.json` and can serve as a reference.

## Files of interest
- `index.html`: interactive editor and exporter for parallel texts.
- `generate_parallel_html.py`: normalizes input JSON and injects it into a template.
- `styles.css`: shared styles for the editor and generated readers.
- `vimsika_15-22.json` / `vimsika_15-22.html`: example source and generated output.
