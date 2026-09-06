# Guidelines

## Response

- When responding to an answer, use simple plain language. Define first any terms or technical jargon you introduce in the response with markdown quote `>`.
- When dealing with trivial tasks, use ASD-STE100 Simplified Technical English.
- When dealing with non-trivial tasks, use Caveman full: terse, no filler, technical substance intact. Off only on explicit "normal mode"/"stop caveman".

## File Tools

Always use `fff-mcp` when searching files: `fffind` and `ffgrep` over shell (`fd`/`rg`/`sg` only when flags missing).

1. Path/fuzzy → `fffind`, fallback `fd`.
2. Text content (docs, logs, configs, plain code) → `ffgrep`, fallback `rg`.
3. Code semantics — real refs, structure, rewrite → `ast-grep`, fallback `rg`
4. Identifier: `ffgrep` first; if comment/string noise pollutes → redo `ast-grep -p '...' --lang <lang>`

## Workflows

- When work can, or need to, run in parallel, use `herdr` to start a new pi in a new pane.
- When working with code related tasks; DO NOT put unnecessary/narrating comments on file/func/method/logic/etc. Prefer self-explaining code. Comment only what code can't say: info needed to continue work later, or worth documenting.

Project specific `AGENTS.md` start below

---

## Deppfellow Wiki

This repository define LLM wiki for humans and agents, and its supporting components. See `_schema/AGENTS.md`.

Table below show allowlist path, for agents allowed to inspect external components outside this repository. External path that not provided in the table should be asked before inspection.

| Name | Description | Path |
| --- | --- | --- |
| Nix's dotfiles | Nix dotfiles configuration used in this environment | `~/dotfiles` |
| deppfellow-page | Site code repository displaying public articles of this Wiki | `~/workspace/projects/deppfellow-page` |
| bridgekeeper | Personal orchestration substrate: `bk` (gates) + `tdg` (shim) | `~/workspace/projects/bridgekeeper` |
| td | Go-binary, minimalist CLI for tracking tasks across AI coding sessions | `~/workspace/projects/td` |
