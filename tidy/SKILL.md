---
name: tidy
description: Keeps code simple, clean and easy to read. Use only when the user explicitly asks for it (/tidy, or asks to tidy or simplify code) or when the project's CLAUDE.md says to use it; do not load it automatically otherwise. Invoke as /tidy (changed files), /tidy <file> (refactor a file) or /tidy check <file> (report only).
---

# Tidy

Write the simplest code that solves the problem. Code is read far more often than it is written:
optimize for the next reader, not for cleverness or hypothetical future needs.

## Principles

- **Simplest thing that works.** No speculative abstractions, layers, options or config (YAGNI).
- **One job per function.** Keep functions small; use early returns instead of deep nesting.
- **Descriptive names.** Say what a thing is or does; no cryptic abbreviations.
- **Remove duplication, but don't abstract too early.** Extract on the third repetition, not the second.
- **Reuse before writing.** Prefer the standard library and existing project utilities over new code.
- **Comments explain why, not what.** If the code needs a comment to explain what it does, rename or restructure it.
- **Match the codebase.** Follow the existing style, naming and idioms of the project.
- **Handle errors where it matters.** Validate at boundaries (user input, I/O, external APIs); no blanket try/catch.

## Guidelines (not hard rules)

| Metric | Aim for |
|---|---|
| Function length | ≲ 30 lines |
| Parameters | ≤ 3–4 (group the rest into an object) |
| Nesting depth | ≤ 2 levels |
| File | one clear purpose |

## AI-generated code smells

Watch for these in your own output — they are the most common sources of noise:

- Defensive checks for cases that cannot happen (`if (x !== null && x !== undefined && ...)` on a value just created).
- Comments that restate the code (`// increment counter` above `counter++`).
- Wrapper functions or classes that only forward to another call.
- Unused imports, variables, parameters or leftover debug code.
- Generic names: `data`, `result`, `info`, `item`, `handle`, `helper`, `manager`, `utils`.
- Excessive logging or print statements.
- Classes, interfaces, factories or config where a plain function would do.
- Re-implementing what the language or project already provides.

## Guardrails — when NOT to simplify

- Never change behavior or public APIs while tidying.
- Don't touch code unrelated to the current task.
- Don't trade clarity for brevity: no clever one-liners or dense chains that are harder to read.
- Keep diffs small and focused; tidying should be easy to review.
- If a "simplification" needs a long explanation, it isn't one — leave it.

## Writing mode (automatic)

Before delivering any code, run this checklist:

1. Can anything be removed without losing functionality?
2. Does any function do more than one thing?
3. Is any of the smells above present?
4. Would a newcomer understand this in 30 seconds?

Fix what you find, then deliver.

## Commands

- `/tidy` — tidy the files changed in this session (or the current `git diff` if available).
- `/tidy <file>` — tidy that file.
- `/tidy check <file>` — report issues only; do not edit.

### Process for `/tidy` and `/tidy <file>`

1. Read the target code fully.
2. List the issues found (principles, smells, guidelines).
3. Apply the fixes, preserving behavior and respecting the guardrails.
4. Run the project's tests or linter if they exist.
5. Report.

### Report format

| Issue | Fix | Why |
|---|---|---|
| `processData` does validation and saving | Split into `validateOrder` and `saveOrder` | One job per function |

End with a one-line summary (e.g. "4 issues fixed, 38 → 24 lines, behavior unchanged").
If nothing needs changing, say so in one line.

## Examples

See [references/examples.md](references/examples.md) for before/after examples of each pattern.
