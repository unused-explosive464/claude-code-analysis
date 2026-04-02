# Install: Claude Code Self-Knowledge Skill

One folder. Drop it in. Your Claude Code agent gets autonomous access to a complete map of its own internals.

## What This Does

Gives Claude Code a `/internals` skill that fetches its own reverse-engineered architectural docs from GitHub on-demand. When your agent hits context pressure, permission blocks, tool failures, or agent coordination issues — it knows exactly which document to pull and what pattern to apply.

The skill auto-triggers when relevant. No manual intervention needed — though you can also invoke `/internals compaction` or `/internals permissions` directly.

## Install (30 seconds)

### Option A: One command

```bash
mkdir -p ~/.claude/skills/internals && curl -sL \
  https://raw.githubusercontent.com/thtskaran/claude-code-analysis/master/.claude/skills/internals/SKILL.md \
  -o ~/.claude/skills/internals/SKILL.md
```

This installs it globally — works across all your projects.

### Option B: Per-project

```bash
mkdir -p .claude/skills/internals && curl -sL \
  https://raw.githubusercontent.com/thtskaran/claude-code-analysis/master/.claude/skills/internals/SKILL.md \
  -o .claude/skills/internals/SKILL.md
```

### Option C: Manual

1. Create `~/.claude/skills/internals/` (or `.claude/skills/internals/` in your project)
2. Copy [`SKILL.md`](.claude/skills/internals/SKILL.md) into that folder
3. Done

## How It Works

The `SKILL.md` contains:

- **A trigger description** — Claude auto-invokes when it detects situations where self-knowledge would help (context growing, tools blocked, agents spawning, etc.)
- **A ReAct loop** — Identify subsystem → fetch the right doc from GitHub via WebFetch → extract insight → apply it
- **A full document manifest** — 82 files mapped with one-line descriptions so the agent picks the right one
- **Critical constants** — Hardcoded thresholds (93% compaction, 64-token YOLO, etc.) available without fetching

The agent uses `WebFetch` to pull raw markdown from `raw.githubusercontent.com` on-demand. It only fetches what's relevant — not the whole repo.

## Usage

### Automatic (recommended)

Just work normally. The skill's description tells Claude when to activate:

> *Context getting long? Claude fetches the compaction doc, learns the 3-tier system and 93% threshold, restructures its approach.*
>
> *Bash command blocked? Claude fetches the parser doc, learns the 15 dangerous AST types, rewrites the command to pass.*

### Manual

```
/internals compaction
/internals permissions
/internals bash-parser
/internals agents
/internals prompts
```

Or just `/internals` with any topic — it maps to the right document.

## Verify It's Working

After installing, start a new Claude Code session and ask:

> "What do you know about your own compaction system? Check your internals."

If it fetches from `raw.githubusercontent.com/thtskaran/claude-code-analysis/...` and gives you specifics about 3-tier compaction with the 93% threshold and 9-section summary template — it's working.

## Requirements

- Claude Code (any version with skills support)
- WebFetch tool available (enabled by default)

## Uninstall

```bash
rm -rf ~/.claude/skills/internals
```
