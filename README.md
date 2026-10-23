# ScriptaBooks 1

Ten ScriptaHub books, organized for people to browse and edit. Each book has a directory named after its title, with its text, summaries, PDF and artwork next to its metadata. The Ploinky ScriptaHub agent renders the presentation pages and provides the reader, navigation and contribution forms.

## Layout

```text
collection.json
books/
  metacult/
    book.json
    en/
      full.html
      summary.html
      book.pdf
      Metacult.assets/
        page-0001-image-01.png
    assets/
      cover.webp
      cover-source.png
      thumbnail.webp
    animation/
      metacult-introduction.shf
  affective-calculus/
    book.json
    en/
      full.html
      summary.html
      book.pdf
    ro/
      full.html
      summary.html
    assets/
      ...
assets/
  shared/                       Resources reused across different books, when needed
README.md
LICENSE
```

`collection.json` lists each book by readable title and metadata path, for example `books/metacult/book.json`. Stable book IDs and SHA-256 checksums remain inside metadata for compatibility and verification; they are not directory or content filenames.

## Inside a book

- `book.json`: multilingual display metadata, edition history, source provenance and legacy resource mappings. JSON files are formatted for manual reading.
- `en/full.html`, `ro/full.html`, etc.: the complete text in each available language.
- `<language>/summary.html`: the corresponding summary, when available.
- `<language>/book.pdf`: an available PDF edition.
- `assets/`: this book's artwork and other common resources, stored once even when several languages or editions use them.
- `animation/`: film content, when available.
- `editions/`: distinct historical files, when needed. Identical current and historical files share one physical file through the resource mappings.

Only available translations and formats are present. Metacult has an English reader; this migration does not invent a Romanian edition. Original authored document HTML, images and PDFs retain their bytes.

## Legacy URLs and deduplication

Each `book.json` keeps its stable `id` and original `legacyDirectory`. The `resources` object maps each original filename to a readable repository path. For example, Metacult's old `en/full_content.html` maps to `books/metacult/en/full.html`, and all language copies of the same cover map to `books/metacult/assets/cover.webp`.

The agent serves the original URLs, including `/books/metacult/bk-96a7b4595ce6486c/en/book.html?lang=en`. It renders the landing page from metadata and returns raw content at the legacy URLs so relative links still resolve. Historical aliases and edition metadata are preserved. Application code and generated landing pages are not stored here.

Resources identical across different books belong in `assets/shared/` with descriptive names. The current ten books have no such cross-book files; their repeated assets are shared within each book.

## Included books

- Metacult
- Capitalism Against Itself
- Anti-Smoothing: Clients, Markets, and Business Model
- Affective Calculus
- Enthusiasm: The Missing Energy of a Successful Civilization
- Incomplete Tribes
- A Mind Does Not Fit in a Feed
- Two Ways to Build a World
- The Messiah Factory
- The New Social Contract in the Age of Artificial Intelligence

Imported from Axiologic/ScriptaHub commit `9fade75c2948d9a77a559d156134f76ee0b5d051`. All ten books retain their original text, artwork, PDFs, films, credits and release history. The previous layout used 489 files and 42,494,850 bytes for these book trees. The deduplicated payload uses 91 physical files and 22,580,365 bytes, plus metadata. The original ScriptaHub repository remains unchanged.

## Publishing

1. Edit or add content in the appropriate book directory, keeping distinct historical editions intact.
2. Update display metadata and resource mappings in `book.json`. Preserve the book ID and legacy aliases.
3. Update `resourceChecksums` for changed raw files, then the `book.json` checksum in `collection.json`.
4. Validate with ScriptaHubAgent's `python3 tools/content_repository.py check --root /path/to/scriptaBooks-1` and push content and metadata together.

The migration tools belong to ScriptaHubAgent. Its `migrate` command creates this readable structure from the original published books; `organize` converts the earlier hash-named layout without changing raw bytes. They are intentionally not copied into this content repository.

Agent configuration:

```json
{
  "schemaVersion": 1,
  "syncIntervalSeconds": 60,
  "repositories": ["https://raw.githubusercontent.com/Axiologic/scriptaBooks-1/master/collection.json"]
}
```

The agent reads the configured JSON URL over HTTP, verifies book metadata and fetches content on demand. It does not clone this repository.
