---
title: Notes CLI — progress update
slug: notes-cli-progress
tags: [project, programming, go, cli]
---

# Notes CLI — progress update

From the design sketch at [[20251115_1997]]. Have been working on this in the evenings.

## What's done

**`notes new` command:** Creates a new note with today's date, generates a sequential ID, optionally adds frontmatter if you pass `--frontmatter`, and opens it in `$EDITOR`. This alone has reduced friction significantly.

```bash
notes new "Spiced cookie recipe"
notes new --type recipe "Spiced cookie recipe"
notes new --frontmatter "Running notes December"
```

**Directory structure:** Creates `YYYY/MM/` directories automatically. The ID is looked up from a JSON file (`id.json`), incremented, and written back atomically.

**`notes today`:** Opens or creates a daily note. If one exists for today, opens it. If not, creates it with a simple heading and today's date.

## What's in progress

**`notes search`:** Calling `rg` (ripgrep) as a subprocess. Works but the output formatting needs work. Currently shows raw ripgrep output; want context and a link to the file.

## What's next

**`notes links`:** This is the hard one. Need to find all files containing `[[YYYYMMDD_ID]]` references to a given note. Straightforward with ripgrep but need to parse the pattern correctly.

**Configuration:** `~/.notesrc` for notes directory, editor preference, default type.

## Technical notes

Using [cobra](https://github.com/spf13/cobra) for command structure. The library is very good—subcommand definition is clean, flag handling is consistent.

The ID file write uses `os.WriteFile` with a temporary file and rename, which should be atomic on most filesystems.

Total code so far: ~400 lines of Go.
