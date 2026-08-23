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
| `_schema/` | this contract | human edits; agent reads |
| `_templates/` | note templates | human edits; agent reads |
| `.agents/` `.docs/` `.tmp/` `.obsidian/` | local meta | ignore |

A top-level folder is a **category** only if its name has no leading `.` or `_`.

## Categories

Exactly one per content note, expressed by the folder it lives in:

- **Article** — long-form finished writing.
- **Project** — ongoing work with a goal and status.
- **Log** — a short-to-medium dated entry, chained to the one before it.

**Logs are chained**: each log's `previous` field links the log immediately before it. Logs may also wikilink any earlier log by reference.

## Front-matter

Required on every content note:

```yaml
origin: human | agent   # who authored it
created: YYYY-MM-DD     # creation date
updated: YYYY-MM-DD     # last change
tags: []                # Obsidian tags (may be empty)
```

`Logs` additionally require:

```yaml
previous: "[[Prior log]]"   # wikilink to the immediately preceding log (omit on the first)
```

Templates default `origin` to `human`; when an agent authors a draft, it sets `origin: agent`.

## Workflow (Phase 1 — write-direct)

1. Human writes directly into the matching category folder.
2. Agent authors new content into `Private/Drafts/<Category>/`.
3. Human reviews a draft and **promotes** it: move the file into the matching public category folder, refresh `updated`, and keep `origin: agent`.
4. Agent may maintain the wiki — find missing links, fix formatting, validate front-matter — but never authors into public categories directly. Bump `updated` whenever it changes a note.

## Lint checklist

Run these checks after any edit:

- [ ] Every content note has valid front-matter (`origin`, `created`, `updated`, `tags`; `previous` for logs).
- [ ] No public note links into `Private/`.
- [ ] Every log except the first has a `previous` pointing to the immediately preceding log.
- [ ] No content note lives outside the three category folders or `Private/`.
- [ ] Nothing under `Private/Personal/` was touched.
