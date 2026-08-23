---
name: pragmatic-yagni
description: YAGNI coding rule that does not sacrifice flexibility. Use this skill when implementing a task
---

# Pragmatic YAGNI

Before you write code, understand the problem first. Read the task and the code that it touches. Trace the full flow from start to end.

Then check in this order. Stop at the first check that is true:

1. Does this code need to exist at all?
2. Does this codebase already have it?
3. Does the standard library do it?
4. Does a native platform feature do it?
5. Does an already-installed dependency do it?
6. Can you write it in one line?

Write the minimum code that works only if no check is true.

A small change in the wrong place causes a second bug. Fix the root cause of a bug. Grep every caller. Fix the shared function once. If you patch only the path that the ticket names, you leave every other caller broken.

Write minimal code that you can extend later. Do not write code that blocks future changes.

Some decisions are easy to reverse. Call these two-way doors. Examples: the implementation, internal names, a function's shape. Any change that a later diff can undo cheaply is a two-way door. Make these decisions now. Keep them small. Do the rest later, when you need it.

Some decisions are hard to reverse. Call these one-way doors. Examples: the data schema, persisted formats, the public API, the wire protocol, the security model. Do not commit to these until you have enough information. If you must commit now, choose the option that leaves the door open. Prefer a change that adds over a change that renames. Prefer a narrow public surface over a wide one.

Small must not mean cornered. If the minimal version makes the next change hard, pay the small cost now that keeps the next change cheap. But never guess the shape of future needs.

Do not add abstractions that nobody requested. Do not add a new dependency if you can avoid it. Do not add boilerplate that nobody asked for. Prefer deletion over addition. Prefer boring code over clever code. Use the fewest files possible.

If you use a shortcut that has a known limit, add a comment. The comment must name the limit and the upgrade path. Examples of known limits: a global lock, an O(n²) scan, a naive heuristic.

Never remove these: your understanding of the problem, input validation at trust boundaries, error handling that prevents data loss, security, accessibility, hardware calibration, and anything explicitly requested.

Non-trivial logic must leave one runnable check: an assert-based self-check or one small test file. Use nothing heavier. Trivial one-liners need no check.
