# Using-Confluence-CLI Skill Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a two-file Claude skill that teaches workflows for Confluence operations with comprehensive storage format reference.

**Architecture:** Skill directory at `skills/using-confluence-cli/` with SKILL.md (main file, ~250 words) and storage-format.md (reference, ~600 words). Progressive disclosure - main skill always loaded, storage format loaded on demand when editing content.

**Tech Stack:** Claude Code skills (markdown with YAML frontmatter)

---

## Task 1: Create Skills Directory Structure

**Files:**
- Create: `skills/using-confluence-cli/SKILL.md`
- Create: `skills/using-confluence-cli/storage-format.md`

**Step 1: Create the directory**

```bash
mkdir -p skills/using-confluence-cli
```

**Step 2: Verify directory exists**

Run: `ls -la skills/`
Expected: `using-confluence-cli/` directory listed

**Step 3: Commit scaffold**

```bash
git add skills/
git commit -m "chore: scaffold skills directory for using-confluence-cli"
```

---

## Task 2: Write SKILL.md - Main Skill File

**Files:**
- Create: `skills/using-confluence-cli/SKILL.md`

**Step 1: Write the skill file**

```markdown
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
```

**Step 2: Verify file created**

Run: `cat skills/using-confluence-cli/SKILL.md | head -20`
Expected: Frontmatter with name and description visible

**Step 3: Commit**

```bash
git add skills/using-confluence-cli/SKILL.md
git commit -m "feat(skill): add using-confluence-cli main skill file"
```

---

## Task 3: Write storage-format.md - Reference File

**Files:**
- Create: `skills/using-confluence-cli/storage-format.md`

**Step 1: Write the storage format reference**

```markdown
# Confluence Storage Format Reference

Copy-paste examples for `--format storage` content.

## Status Macros (Lozenges)

```xml
<ac:structured-macro ac:name="status">
  <ac:parameter ac:name="colour">Green</ac:parameter>
  <ac:parameter ac:name="title">On track</ac:parameter>
</ac:structured-macro>
```

**Colors:** Green, Red, Yellow, Blue, Grey

## Panels

### Info/Note/Warning/Tip

```xml
<ac:structured-macro ac:name="info">
  <ac:parameter ac:name="title">Note Title</ac:parameter>
  <ac:rich-text-body><p>Content here</p></ac:rich-text-body>
</ac:structured-macro>
```

**Types:** `info`, `note`, `warning`, `tip`

### Custom Panel

```xml
<ac:structured-macro ac:name="panel">
  <ac:parameter ac:name="bgColor">#e3fcef</ac:parameter>
  <ac:parameter ac:name="title">Custom Panel</ac:parameter>
  <ac:rich-text-body><p>Content</p></ac:rich-text-body>
</ac:structured-macro>
```

## Code Blocks

```xml
<ac:structured-macro ac:name="code">
  <ac:parameter ac:name="language">python</ac:parameter>
  <ac:parameter ac:name="linenumbers">true</ac:parameter>
  <ac:parameter ac:name="theme">Midnight</ac:parameter>
  <ac:plain-text-body><![CDATA[def hello():
    print("Hello")]]></ac:plain-text-body>
</ac:structured-macro>
```

**Languages:** python, javascript, java, bash, sql, xml, json, etc.
**Themes:** Midnight, Eclipse, Emacs, etc.

## Expand/Collapse

```xml
<ac:structured-macro ac:name="expand">
  <ac:parameter ac:name="title">Click to expand</ac:parameter>
  <ac:rich-text-body><p>Hidden content</p></ac:rich-text-body>
</ac:structured-macro>
```

## Page Layouts

```xml
<ac:layout>
  <ac:layout-section ac:type="two_equal">
    <ac:layout-cell><p>Left column</p></ac:layout-cell>
    <ac:layout-cell><p>Right column</p></ac:layout-cell>
  </ac:layout-section>
</ac:layout>
```

**Types:** `single`, `two_equal`, `two_left_sidebar`, `two_right_sidebar`, `three_equal`, `three_with_sidebars`

## Tables

```xml
<table>
  <tbody>
    <tr>
      <th><p>Header 1</p></th>
      <th><p>Header 2</p></th>
    </tr>
    <tr>
      <td><p>Data 1</p></td>
      <td><p>Data 2</p></td>
    </tr>
  </tbody>
</table>
```

**Spanning:** `<td colspan="2">` or `<td rowspan="2">`

## Images

### From Attachment
```xml
<ac:image ac:width="600">
  <ri:attachment ri:filename="screenshot.png" />
</ac:image>
```

### External URL
```xml
<ac:image>
  <ri:url ri:value="https://example.com/image.png" />
</ac:image>
```

## Links

### Internal Page Link
```xml
<ac:link>
  <ri:page ri:content-title="Target Page" ri:space-key="SPACE" />
  <ac:link-body>Link text</ac:link-body>
</ac:link>
```

### Attachment Link
```xml
<ac:link>
  <ri:attachment ri:filename="document.pdf" />
  <ac:link-body>Download PDF</ac:link-body>
</ac:link>
```

### External URL
```xml
<a href="https://example.com">External link</a>
```

## Task Lists

```xml
<ac:task-list>
  <ac:task>
    <ac:task-id>1</ac:task-id>
    <ac:task-status>incomplete</ac:task-status>
    <ac:task-body>First task</ac:task-body>
  </ac:task>
  <ac:task>
    <ac:task-id>2</ac:task-id>
    <ac:task-status>complete</ac:task-status>
    <ac:task-body>Done task</ac:task-body>
  </ac:task>
</ac:task-list>
```

## Text Formatting

| Format | Markup |
|--------|--------|
| Bold | `<strong>text</strong>` |
| Italic | `<em>text</em>` |
| Underline | `<u>text</u>` |
| Strikethrough | `<s>text</s>` |
| Monospace | `<code>text</code>` |
| Superscript | `<sup>text</sup>` |
| Subscript | `<sub>text</sub>` |
| Color | `<span style="color: rgb(255,0,0);">red</span>` |
```

**Step 2: Verify file created**

Run: `wc -l skills/using-confluence-cli/storage-format.md`
Expected: ~150-180 lines

**Step 3: Commit**

```bash
git add skills/using-confluence-cli/storage-format.md
git commit -m "feat(skill): add storage-format.md reference"
```

---

## Task 4: Test Skill Locally

**Files:**
- Read: `skills/using-confluence-cli/SKILL.md`
- Read: `skills/using-confluence-cli/storage-format.md`

**Step 1: Validate skill frontmatter**

Run: `head -5 skills/using-confluence-cli/SKILL.md`
Expected: Valid YAML frontmatter with `name:` and `description:`

**Step 2: Validate storage-format examples**

Run: `grep -c "ac:structured-macro" skills/using-confluence-cli/storage-format.md`
Expected: 6+ matches (status, info, panel, code, expand, layout)

**Step 3: Check for broken markdown**

Run: `cat skills/using-confluence-cli/SKILL.md | grep -E '^\s*```' | wc -l`
Expected: Even number (balanced code blocks)

---

## Task 5: Create Pull Request

**Files:**
- Commit all changes in `skills/` directory

**Step 1: Check git status**

Run: `git status`
Expected: Clean working tree or only skill files staged

**Step 2: Push to fork**

```bash
git push -u origin main
```

**Step 3: Create PR**

```bash
gh pr create \
  --title "feat: add using-confluence-cli skill for Claude Code" \
  --body "## Summary
- Adds Claude Code skill for confluence-cli workflows
- Two-file structure: SKILL.md (main) + storage-format.md (reference)
- Covers all 15 CLI commands with robustness warnings
- Progressive disclosure for token efficiency

## Files Added
- \`skills/using-confluence-cli/SKILL.md\` (~250 words)
- \`skills/using-confluence-cli/storage-format.md\` (~600 words)

## Testing
Skill triggers on Confluence-related tasks and provides accurate CLI commands and storage format examples."
```

**Step 4: Verify PR created**

Run: `gh pr view --web`
Expected: Opens PR in browser

---

## Summary

| Task | Files | Commits |
|------|-------|---------|
| 1 | Directory scaffold | 1 |
| 2 | SKILL.md | 1 |
| 3 | storage-format.md | 1 |
| 4 | Testing (no commits) | 0 |
| 5 | Push + PR | 0 |

**Total: 3 commits, 2 files created**
