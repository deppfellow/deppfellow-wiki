# Agents — Deppfellow LLM Wiki

This vault is a shared memory layer for a human and their AI agents. It is an Obsidian vault tracked by Git. All content is plain Markdown using Obsidian wikilinks (`[[Note]]`) and tags (`#tag`).

## Non-negotiable rules

1. Never read or write anything under `Private/Personal/`. It is human-only.
2. Never author content directly into `Articles/`, `Projects/`, or `Logs/`. AI-authored content goes into `Private/Drafts/<Category>/` first; a human promotes it.
3. Every content note must carry valid front-matter (see **Front-matter**).
4. Public notes must never link into `Private/`.
5. Use only Obsidian wikilinks and tags — no custom link syntax.
6. Category is implied by the folder. Never store a `category` field in front-matter.

## Structure

| Path | Role | Who writes |
|------|------|------------|
| `Articles/` | public category | human directly; agent only via promotion |
| `Projects/` | public category | same |
| `Logs/` | public category (chained) | same |
| `Private/Personal/` | human-only private notes | human only |
| `Private/Drafts/<Category>/` | AI staging area | agent authors; human promotes |
| `Private/Sources/<Category>/` | captured raw text (Raw Sources), e.g. `Article`, `Reddit`, `Twitter`, `YouTube` | the Obsidian Web Clipper only; never hand-edited |
| `_schema/` | this contract | human edits; agent reads |
| `_templates/` | note templates | human edits; agent reads |
| `_assets/` | public attachments (images, files) | human and agent, alongside the notes that embed them |
| `.agents/` `.docs/` `.tmp/` `.obsidian/` | local meta | ignore |

A top-level folder is a **category** only if its name has no leading `.` or `_` **and** it is listed in `_schema/categories.md`. The list is the publication boundary: an unlisted folder is eligible but never rendered by the site.

## Categories

Exactly one per content note, expressed by the folder it lives in:

- **Article** — long-form finished writing.
- **Project** — ongoing work with a goal and status.
- **Log** — a short-to-medium dated entry, chained to the one before it.

**Logs are daily and chained**: one log per day, filename `YYYY-MM-DD.md`. Each log's `previous` field links the log with the latest earlier date; the first log omits the field entirely. Logs may also wikilink any earlier log by reference.

## Front-matter

Required on every content note:

```yaml
origin: human | agent   # who authored it
created: YYYY-MM-DD     # creation date
tags: []                # Obsidian tags (may be empty)
```

`Logs` additionally require:

```yaml
previous: "[[YYYY-MM-DD]]"  # wikilink to the log with the latest earlier date; omit the field entirely on the first log
```

Templates default `origin` to `human`; when an agent authors a draft, it sets `origin: agent`.

## Raw Sources (capture)

Raw text is brought into the vault with the **Obsidian Web Clipper**, which saves directly to `Private/Sources/<Category>/` (e.g. `Private/Sources/Article/`, `Reddit/`, `Twitter/`, `YouTube/`). A Raw Source is captured, not authored, and the Clipper sets its front-matter (typically `title`, `author`, `published`, `source`, `ContentType`, `fetched`, `processed`, `tags`).

- Never hand-edit a Raw Source; the body stays as captured.
- Never add `origin` or `category` to a Raw Source.
- `processed: false` in the Clipper front-matter marks a source not yet synthesized into a Draft; the agent flips it to `true` after producing one.
- Sources are private (they are other people's content) — only a later synthesis may be promoted to a public category.

## Workflow (Phase 1 — write-direct)

1. Human writes directly into the matching category folder.
2. Agent authors new content into `Private/Drafts/<Category>/`.
3. Human reviews a draft and **promotes** it: move the file into the matching public category folder and keep `origin: agent`.
4. Agent may maintain the wiki — find missing links, fix formatting, validate front-matter — but never authors into public categories directly.

## Lint checklist

Run these checks after any edit:

- [ ] Every content note has valid front-matter (`origin`, `created`, `tags`; `previous` for logs).
- [ ] No public note links into `Private/`.
- [ ] Every log is named `YYYY-MM-DD.md` (one per day) and, except the first, has a `previous` pointing to the log with the latest earlier date.
- [ ] No content note lives outside the listed category folders or `Private/`.
- [ ] Every top-level folder without a leading `.`/`_` is listed in `_schema/categories.md`.
- [ ] Attachments referenced by notes live in `_assets/`.
- [ ] Raw Sources live only under `Private/Sources/<Category>/`, keep their Clipper front-matter, and never carry `origin`/`category`.
- [ ] Nothing under `Private/Personal/` was touched.
