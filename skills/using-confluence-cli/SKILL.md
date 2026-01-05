---
name: using-confluence-cli
description: Use when reading, searching, creating, or editing Confluence pages via CLI. Triggers on Confluence mentions, wiki operations, or page content management.
---

## Installation Guard

```bash
command -v confluence || npm install -g confluence-cli
confluence --version  # Verify v1.12.1+
```

If not configured: `confluence init`

## Robustness Warnings

- **No automatic retry** - transient API failures fail immediately
- **No rate limiting** - use `--delay-ms` for bulk operations
- **Single-attempt operations** - wrap critical workflows with retry logic

## Core Workflows

### Search & Retrieve
```bash
confluence search "query" --limit 10
confluence find "Page Title" --space KEY
confluence read <pageId> --format markdown
confluence info <pageId>
```

### Read-Edit-Update Cycle
```bash
confluence edit <pageId> --output ./page.xml   # Export
# Edit ./page.xml locally
confluence update <pageId> --file ./page.xml --format storage
```

### Page Hierarchy
```bash
confluence spaces                              # List spaces
confluence create "Title" SPACE --format storage --content "<p>...</p>"
confluence create-child "Title" <parentId>
confluence copy-tree <srcId> <destId> --dry-run
```

### Attachments
```bash
confluence attachments <pageId>
confluence attachments <pageId> --download --dest ./files
confluence export <pageId> --dest ./export --referenced-only
```

## Storage Format Reference

When creating or editing page content with `--format storage`, see [storage-format.md](./storage-format.md) for:
- Status macros (lozenges)
- Info/warning/note/tip panels
- Code blocks with syntax highlighting
- Expand/collapse sections
- Page layouts
- Tables, images, links
- Task lists (checklists)
