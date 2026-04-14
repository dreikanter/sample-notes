# Static site generator – design notes

From the backlog (see [[20251231_2025]]). Finally picking this up. Here's where I'm thinking.

## Goals

- Generate HTML from Markdown with frontmatter
- Support a template system (Go templates, probably)
- Fast: rebuild a 500-page site in under a second
- Single binary with no runtime dependencies
- Understand it completely — no black boxes

## Non-goals

- Plugin systems
- JavaScript bundling
- Hot reload (later, maybe)

## Core pipeline

```
Input: directory of .md files
  → Parse frontmatter + Markdown per file
  → Build page index (for cross-references, tags, listing pages)
  → Apply templates
  → Write output files
Output: directory of .html files
```

## Data structures

```go
type Page struct {
    Frontmatter map[string]any
    Content     string // raw markdown
    Rendered    template.HTML
    Path        string
    OutputPath  string
}

type Site struct {
    Pages  []*Page
    Config SiteConfig
}
```

## Implementation order

1. Read files, parse frontmatter (using `gopkg.in/yaml.v3`)
2. Render Markdown to HTML (using `github.com/yuin/goldmark`)
3. Apply templates
4. Write output
5. Add tag pages and index generation
6. Add file watching

## Current status

Steps 1–3 working. Step 4 is where I stalled in November. Resuming here.

goldmark docs: https://github.com/yuin/goldmark
Go templates: https://pkg.go.dev/html/template
