# Commentable HTML

A Claude Code agent skill that makes any HTML document Claude generates commentable — reviewers can highlight text, drop pins anywhere, reply in threads, and save annotations back into the same file.

## What it does

When this skill is installed, Claude will automatically embed an annotation runtime into any standalone HTML document it generates for human review — PRDs, design docs, reports, dashboards, write-ups, previews. The reviewer opens the file in a browser and gets:

- **Highlight to comment** — select any text and a "Comment" pill appears.
- **Anywhere-pins** — click the floating pin button, then click anywhere to drop a free-floating comment.
- **Threaded replies** — each highlight or pin opens a sticky note with the full thread (`⌘+Enter` posts).
- **Save to disk** — `⌘S` writes the annotated HTML back to the same file. Annotations live inside the file as a JSON `<script>` block, so the file *is* the database.

## Install

Drop the skill into your Claude skills directory:

```bash
git clone https://github.com/<you>/commentable-html ~/.claude/skills/commentable-html
```

Claude will pick it up automatically and invoke it whenever it generates HTML meant for a human to read.

## Files

- `SKILL.md` — the skill definition Claude loads.
- `template.html` — the working template the skill copies from.
- `sample.html` — a rendered example you can open in a browser to try the commenting flow.

## Browser support

- Chrome / Edge: full support, including in-place save via the File System Access API.
- Firefox / Safari: comments work; Save falls back to a download.

## License

MIT.
