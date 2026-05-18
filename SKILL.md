---
name: commentable-html
description: Embed inline commenting (text highlights, anywhere-pins, threaded replies, save-to-disk) into any standalone HTML document Claude generates. Use whenever producing HTML that a human will open in a browser to read or review — PRDs, design docs, reports, dashboards, write-ups, generated previews. Skip for HTML fragments that live inside an existing app, for emails, or for files consumed only by automated pipelines.
---

# Commentable HTML

When you generate a standalone HTML file that a human will read, embed the annotation runtime so reviewers can highlight text and pin comments, reply in threads, and save annotations back into the same file. The runtime is fully client-side — no server, no build step, no external assets.

## How to use

1. Read `template.html` from this skill directory. It is a fully working empty document containing all CSS, HTML, and JS needed.
2. Write the output file using the template, with two changes:
   - Replace the `<title>` text with the document title.
   - Replace everything **inside** `<article>…</article>` with the actual document body.
3. Do **not** modify any of these — they are load-bearing for the annotation runtime:
   - The `<style>` block.
   - `<div id="annotation-bar">`, `<div id="context-menu">`, `<div id="selection-toolbar">`, `<button id="pin-btn">`.
   - `<script type="application/json" id="annotations-data">`.
   - The trailing `<script>` block.

That's the whole protocol. The output is one self-contained `.html` file.

## What the user gets

- Highlight text → "Comment" pill appears above the selection (or right-click → Add comment).
- Click the floating pin button, then click anywhere in the article to drop a free-floating comment.
- Each highlight or pin opens a sticky note showing the full thread; **Reply** appends to the thread (⌘+Enter posts).
- **Save** (or ⌘S) writes the annotated HTML back to disk via the File System Access API.
- **Multi-page / slide deck support**: mark each page or slide with `data-commentable-page`. The runtime shows one page at a time, adds ◀/▶ navigation to the toolbar, and scopes pins and highlights to their originating page. Arrow keys also navigate pages. Elements with class `slide` or `page` are detected automatically.

## Multi-page usage

To produce a slide deck or paginated document, put each page inside `<article>` as a direct child tagged with `data-commentable-page`:

```html
<article>
  <section data-commentable-page>
    <h1>Slide 1</h1>
    <p>Content…</p>
  </section>
  <section data-commentable-page>
    <h1>Slide 2</h1>
    <p>Content…</p>
  </section>
</article>
```

The runtime hides all but the first page on load and injects prev/next buttons. All other annotation features work per-page.

## When to use vs. skip

**Use when** the deliverable is a standalone HTML document a human will read and discuss — PRDs, design write-ups, status reports, generated analyses, internal previews, anything you'd otherwise paste into a Google Doc for review.

**Skip when**:
- The HTML is a fragment to be injected into an existing page or framework.
- The HTML is templated server-side or post-processed by a pipeline.
- The output is an email body (most clients strip JS).
- The user explicitly wants a clean HTML without UI chrome (e.g., for printing or embedding).

If unsure, ask the user.

## Caveats to mention to the user when you ship

- Annotations live inside the file itself (in a JSON `<script>` block), so reviewers must **save the file back to disk** to preserve their comments — the runtime can't auto-save.
- Save uses Chrome / Edge's File System Access API. Firefox / Safari fall back to a download.

## Reference

- Template: [`template.html`](./template.html)
