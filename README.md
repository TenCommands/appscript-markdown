# Markdown Renderer for Google Apps Script 

Render GitHub-flavored Markdown (GFM) inside **Google Apps Script** and receive a ready-to-use **`HtmlOutput`** object.

This library supports rendering:

- Raw Markdown strings
- Markdown files from GitHub URLs
- GitHub release notes by tag

It automatically resolves relative links, applies GitHub styling, and falls back to client-side rendering when needed.

---

## What This Returns

All public methods return a **Google Apps Script `HtmlOutput` object**:

```js
HtmlService.HtmlOutput
````

This means you can:

* Display it in dialogs or sidebars
* Serve it from a web app
* Modify its size, title, sandbox mode, or content
* Embed it into larger HTML workflows

---

## Features

* ✅ GitHub-Flavored Markdown (GFM)
* ✅ Server-side rendering via GitHub Markdown API
* ✅ Automatic client-side fallback
* ✅ Relative link & image rewriting
* ✅ GitHub blob, raw, and release URLs supported
* ✅ Syntax highlighting (highlight.js)
* ✅ GitHub-style CSS
* ✅ Admonition blocks (`:::note`, `:::warning`, etc.)

---

## Installation

1. Create a new **Google Apps Script** project
2. Paste the source into a `.gs` file (for example: `markdown.gs`)
3. Save the project

No additional libraries required.

---

## Public API

The public interface is exposed via the global `markdown` object.

---

### `markdown.string(mdString)`

Render a raw Markdown string.

```js
function renderMarkdownString() {
  return markdown.string('# Hello World\n\nThis is **Markdown**.');
}
```

Returns: `HtmlOutput`

---

### `markdown.fileUrl(fileUrl)`

Render a Markdown file from a URL.

Supported URLs:

* `https://github.com/{owner}/{repo}/blob/...`
* `https://raw.githubusercontent.com/...`
* Any direct Markdown file URL

```js
function renderMarkdownFile() {
  return markdown.fileUrl(
    'https://github.com/owner/repo/blob/main/README.md'
  );
}
```

Returns: `HtmlOutput`

---

### `markdown.releaseUrl(releaseUrl)`

Render the body of a GitHub release by tag.

```js
function renderReleaseNotes() {
  return markdown.releaseUrl(
    'https://github.com/owner/repo/releases/tag/v1.2.3'
  );
}
```

Returns: `HtmlOutput`

---

## Rendering the HtmlOutput

### Show in a Dialog

```js
function showDialog() {
  var html = markdown.string('# Hello from Markdown');
  html.setWidth(800).setHeight(600);

  SpreadsheetApp.getUi().showModalDialog(
    html,
    'Markdown Preview'
  );
}
```

---

### Show in a Sidebar

```js
function showSidebar() {
  var html = markdown.fileUrl(
    'https://github.com/owner/repo/blob/main/README.md'
  );

  html.setTitle('Documentation');

  SpreadsheetApp.getUi().showSidebar(html);
}
```

---

### Serve from a Web App

```js
function doGet() {
  return markdown.string('# Public Markdown Page');
}
```

Deploy as a **Web App** and the rendered Markdown becomes the HTTP response.

---

## Modifying the HtmlOutput

### Set Title, Size, and Sandbox Mode

```js
function customizedOutput() {
  var html = markdown.string('## Styled Output');

  html
    .setTitle('My Markdown Page')
    .setWidth(900)
    .setHeight(700)
    .setSandboxMode(HtmlService.SandboxMode.IFRAME);

  return html;
}
```

---

### Wrap Markdown in Additional HTML

```js
function wrappedMarkdown() {
  var content = markdown.string('# Inner Markdown').getContent();

  var html = HtmlService.createHtmlOutput(`
    <header style="padding:10px;font-weight:bold;">
      My App Header
    </header>
    ${content}
  `);

  return html;
}
```

---

---

## Admonitions

```md
> [!NOTE]
> This is a note
```
