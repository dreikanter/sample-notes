# sample-notes

A small sample notes store for testing
[notes-cli](https://github.com/dreikanter/notes-cli),
[notes-view](https://github.com/dreikanter/notes-view), and
[notes-pub](https://github.com/dreikanter/notes-pub).

## Layout

```
notes/
├── id.json                                    # last allocated numeric ID
├── 2023/09/20230915_1001_iceland.md           # public, full frontmatter
├── 2024/11/20241122_1002.md                   # no frontmatter, journal-style
└── 2026/03/20260308_1003_spring-projects.todo.md  # .todo type
```

Filename format expected by `notes-cli`:
`YYYYMMDD_ID[_slug][.TYPE].md`, where `TYPE` is one of
`todo`, `backlog`, `weekly`.

## Content

The notes contain original, neutral text written for this sample store —
short reading notes, a journal entry, and a todo list. Dates are spread
across a three-year window ending in early 2026. About half the notes
carry YAML frontmatter (title, tags, optional `public: true`), the rest
are plain markdown. Some notes include external links (Wikipedia, etc.)
and cross-references to other notes in the store via `[[YYYYMMDD_ID]]`
wiki-style links.

## License

All content is released under
[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)
(public domain dedication). See `LICENSE`. Use it for anything, modify
freely, no attribution required.

## Suggested content sources for larger stores

If you want to expand this store with more realistic text, these
sources permit publication and modification:

- **Project Gutenberg** (<https://www.gutenberg.org/>) — public domain
  literature; excerpts are freely reusable.
- **Wikisource** (<https://wikisource.org/>) — public domain source
  documents.
- **NASA and other US federal agency publications** — works of the US
  government are public domain in the US.
- **Openverse / Wikimedia Commons CC0 collections** — explicitly CC0
  text and media.
- **Wikipedia** (<https://www.wikipedia.org/>) — rich and realistic,
  but CC BY-SA 4.0 requires attribution and share-alike, so it is
  not compatible with a no-attribution requirement. Fine to link to.
- **Synthetic/original text** — what this repo uses; no licensing
  constraints.
