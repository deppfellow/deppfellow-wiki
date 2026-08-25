# Guidelines

Apply before returning response;

- When responding to an answer, use simple plain language. Define first any terms or technical jargon you introduce in the response with markdown quote `>`.
- When dealing with trivial task, use ASD-STE100 Simplified Technical English.
- When dealing with non-trivial task, read `/skill:pragmatic-yagni`

Project specific `AGENTS.md` start below

---

## Deppfellow Wiki

This repository define LLM wiki for humans and agents, and its supporting components.

Table below show allowlist path, for agents allowed to inspect external components outside this repository. External path that not provided in the table should be asked before inspection.

| Name            | Description                                                  | Path                                  |
| --------------- | ------------------------------------------------------------ | ------------------------------------- |
| Nix's dotfiles  | Nix dotfiles configuration used in this environment          | `~/dotfiles`                          |
| deppfellow-page | Site code repository displaying public articles of this Wiki | `~/workshop/projects/deppfellow-page` |
