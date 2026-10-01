# tidy — Claude Code skill

A [Claude Code](https://claude.com/claude-code) skill that keeps code simple, clean and easy to read.

It guides Claude to write the simplest code that works, avoid common AI-generated code smells
(over-defensive checks, redundant comments, needless abstractions, generic names) and refactor
existing code without changing its behavior.

## What's inside

```
tidy/
├── SKILL.md              # principles, guardrails, commands and report format
└── references/
    └── examples.md       # before/after examples (JavaScript and Python)
```

## Install

Clone the repo and copy the skill into your personal skills folder:

```bash
git clone https://github.com/DavidVanegas720/simplificadorCodigo.git
mkdir -p ~/.claude/skills
cp -R simplificadorCodigo/tidy ~/.claude/skills/tidy
```

To get updates with `git pull`, use a symlink instead of copying:

```bash
ln -s "$(pwd)/simplificadorCodigo/tidy" ~/.claude/skills/tidy
```

Start a new Claude Code session; `/tidy` should appear when typing `/`.

## Enable it in a project

The skill only runs automatically in projects that ask for it.
Add this rule to the project's `CLAUDE.md` (create it in the project root if it doesn't exist):

```markdown
## Code style
- Whenever you write or modify code, load and follow the `tidy` skill.
- Before finishing a coding task, run `/tidy check` on the files you changed and fix what it reports.
```

In other projects you can still use it manually.

## Commands

| Command | What it does |
|---|---|
| `/tidy` | Tidy the files changed in the session |
| `/tidy <file>` | Tidy that file |
| `/tidy check <file>` | Report issues only, no edits |

## Principles

- Simplest thing that works — no speculative abstractions.
- Small, single-purpose functions with early returns.
- Descriptive names.
- Reuse the standard library and existing project code.
- Comments explain *why*, not *what*.
- Never change behavior while tidying; keep diffs small.

See [`tidy/SKILL.md`](tidy/SKILL.md) for the full rules.
