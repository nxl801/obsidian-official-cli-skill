---
name: obsidian-official-cli
description: Work with Obsidian 1.12+ via the official `obsidian` command line interface exposed by the desktop app. Use when the user wants to search, read, inspect backlinks/links, list tags or properties, inspect tasks, or automate a running Obsidian vault without vectorizing/importing notes. Prefer this skill over older `obsidian-cli` / NotesMD workflows when the machine already has the official Obsidian CLI enabled. Especially useful for OpenClaw / Claude Code / agent workflows that need transparent, verifiable retrieval from a live Obsidian vault.
---

# Obsidian official CLI

Use the official `obsidian` CLI for retrieval-first workflows against a running Obsidian app.

## Quick checks

Run these first:

```bash
zsh -lic 'command -v obsidian && obsidian version'
```

If `obsidian` is missing:
- confirm Obsidian Desktop is version 1.12+
- confirm **Settings → General → Command line interface** is enabled
- use a login shell (`zsh -lic`) so PATH includes `/Applications/Obsidian.app/Contents/MacOS`

If the app is closed, running a command may launch it.

## Default workflow

Prefer this sequence:

1. **Locate candidates** with `search` or `search:context`
2. **Inspect structure** with `tags`, `properties`, `outline`, `backlinks`, `links`
3. **Read only the few relevant notes** with `read`
4. **Summarize / answer** after retrieval

Do not start by scanning the whole vault file-by-file unless the user explicitly wants raw filesystem treatment.

## OpenClaw / agent guidance

Prefer the official CLI when the user asks questions like:
- “帮我在 Obsidian 里找 PLC 相关笔记”
- “看一下这篇笔记有哪些反向链接”
- “列出这个 vault 的 tags / properties”
- “不要走向量库，直接用 Obsidian 自己的索引”

For agent workflows:
- prefer `format=json` when parsing downstream
- prefer `search:context` before `read` when result sets are broad
- prefer `path=` over `file=` when note names are duplicated
- keep reads narrow; do not dump large note sets into model context without filtering

## High-value read-only commands

Use these by default:

```bash
obsidian search query="PLC" limit=10 format=json
obsidian search:context query="PLC" limit=10 format=json
obsidian read file="Note name"
obsidian outline file="Note name" format=json
obsidian backlinks file="Note name" format=json counts
obsidian links file="Note name"
obsidian tags counts format=json
obsidian properties counts format=json
obsidian unresolved total
obsidian tasks todo format=json
```

Use `file=<name>` when wikilink-style resolution is convenient; use `path=<exact/path.md>` when ambiguity matters.

## Retrieval patterns

### Topic lookup

For questions like “find my PLC notes”:

```bash
obsidian search query="PLC" limit=20 format=json
obsidian search:context query="PLC" limit=20 format=json
```

For Chinese vaults, search with the native topic wording when possible, for example:

```bash
obsidian search query="可编程逻辑控制器" limit=20 format=json
obsidian search query="工业控制" limit=20 format=json
obsidian search:context query="安全PLC" limit=20 format=json
```

Then read only the top few matching notes.

### Structure-aware expansion

When one note looks central:

```bash
obsidian backlinks file="Central note" format=json counts
obsidian links file="Central note"
```

Use this to expand to related notes without broad rescans.

### Metadata-first lookup

When the vault uses frontmatter/properties heavily:

```bash
obsidian properties counts format=json
obsidian property:read name=status file="Project note"
obsidian tags counts format=json
```

This is often cheaper and more transparent than semantic retrieval.

## Write safety

Prefer read-only commands unless the user clearly asks to modify the vault.

Commands that can change the vault include:
- `append`
- `prepend`
- `create`
- `rename`
- `move`
- `delete`
- `property:set`
- `property:remove`
- `task`
- `command`
- `eval`

Before using write commands:
- name the target vault/file clearly
- prefer the smallest reversible change
- avoid bulk edits without confirmation
- treat `command` and especially `eval` as higher-risk operations; do not use them when a first-class note command is enough

## When not to use this skill

Do not use this skill when:
- the machine only has older community `obsidian-cli` / NotesMD tooling
- the user wants Sync automation without the desktop app running; use Obsidian Headless / `ob` instead
- plain filesystem reads are enough and Obsidian-specific structure is irrelevant

## Reference

For command families and examples, read:
- `references/official-cli-commands.md`
