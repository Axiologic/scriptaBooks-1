# ScriptaBooks 1

Content-only ScriptaHub repository. The Ploinky ScriptaHub agent owns book pages, readers, navigation and contribution management. This repository contains no application server, workflows or generated landing pages.

## Layout

```text
collection.json                  Version 2 index: book IDs, metadata paths and SHA-256
books/<stable-book-id>/book.json  Display metadata, edition history and legacy URL aliases
content/<sha256>.html            Canonical book and summary documents, stored once
assets/shared/<sha256>.<ext>      Shared images, fonts, PDFs and film content, stored once
```

The original title routes and book IDs remain in each book's `legacyDirectory`. Each `resources` entry maps a legacy book-relative filename to its one physical content/asset file. The agent renders `en/book.html` and other language landing pages from `metadata`; it serves `manifest.json` and `editions.json` from the same record. Every known legacy raw-content path resolves through `resources`, including historical editions. For example, `/books/metacult/bk-96a7b4595ce6486c/en/book.html?lang=en` remains a valid agent URL.

Readers receive HTML at its original public URL so relative images, fonts and chapter links continue to work. Repeated images and current/historical files point to the same shared object when their bytes are identical. `provenance.originalSha256` records the input hashes and the source commit; all ten books' raw files retain their original bytes. Existing translation availability is preserved: Metacult currently has an English reader; migration does not invent a Romanian edition.

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

Imported from Axiologic/ScriptaHub commit `9fade75c2948d9a77a559d156134f76ee0b5d051`, preserving authored text, artwork, PDFs, films, credits and release history. The previous layout used 489 files and 42,494,850 bytes for these book trees. The new shared payload uses 91 physical files and 22,580,365 bytes, plus metadata; 88 generated presentation pages have moved to the renderer.

## Publishing

Write each changed raw object once using its SHA-256 filename, update the book's resource aliases and metadata, then update its metadata checksum in `collection.json` in the same commit. Do not overwrite historical release records or assign a new ID to an existing book. The agent loads the index over HTTP from `main` or `master`, checks book metadata hashes, and fetches content on demand. No clone is needed by the agent.

Configuration in ScriptaHubAgent `sources.json`:

```json
{
  "id": "scripta-books-1",
  "url": "https://github.com/Axiologic/scriptaBooks-1.git",
  "ref": "main-or-master"
}
```

The publishing and migration tools live in the ScriptaHub application repository (`tools/content_repository.py`); they are intentionally not copied into this content repository.
