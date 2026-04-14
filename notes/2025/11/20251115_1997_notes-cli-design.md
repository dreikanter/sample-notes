---
title: Notes CLI — design sketch
slug: notes-cli-design
tags: [project, programming, cli, design]
description: Initial design for a personal notes command-line tool.
---

# Notes CLI — design sketch

From the Q4 goals (see [[20250925_1950]]). Starting to think through this more concretely.

## The problem I'm solving

I take notes in Markdown files, organized by date in a directory tree. Finding things requires either remembering the file or using grep. Creating new notes requires opening a terminal, navigating, creating the file, and writing frontmatter. I want the friction to be lower.

## Design goals

- Fast: creating a note should take one command and open an editor
- Searchable: full-text search across all notes with context
- Cross-reference aware: follow `[[wikilink]]` references
- Minimal: no database, no sync service, no account — just files

## Command ideas

```bash
notes new "title"           # create new note with frontmatter
notes new --type todo       # create typed note
notes search "term"         # full-text search
notes today                 # open or create today's note
notes list --tag python     # filter by tag
notes open 20251001_1956    # open by ID
notes links 20251001_1956   # show backlinks to this note
```

## Implementation approach

Go seems right for this — fast startup, single binary, good CLI libraries. Will look at [cobra](https://github.com/spf13/cobra) for the command structure.

For search: ripgrep as a library or subprocess. For backlinks: simple scan of `[[ID]]` patterns.

Configuration: a `~/.notesrc` file pointing to the notes directory.

## Similar tools

- [Obsidian](https://obsidian.md/) does most of this in a GUI with a plugin ecosystem — not what I want
- [nb](https://xwmx.github.io/nb/) is a shell script with similar goals — worth studying the design

**Next step:** Build the `new` command first.
