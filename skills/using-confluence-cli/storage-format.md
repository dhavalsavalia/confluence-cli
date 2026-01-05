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
