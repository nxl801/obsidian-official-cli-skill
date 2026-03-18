# Obsidian Official CLI Skill for OpenClaw

[中文版本 / Chinese version](./README.zh-CN.md)

An OpenClaw-compatible AgentSkill for working with the **official Obsidian 1.12+ CLI** (`obsidian`).

This skill is built for **retrieval-first** agent workflows against a running Obsidian desktop app. It is especially useful for OpenClaw, Claude Code, and similar agent systems that want to query a live Obsidian vault transparently, without making vector import the default path.

## Why this skill exists

There are older community tools such as `obsidian-cli` / NotesMD, and they are still useful in some setups. But Obsidian 1.12 introduced an official CLI exposed by the desktop app itself.

This skill focuses on that official path and encourages a workflow like:

```text
search -> search:context -> read -> backlinks / links / properties
```

That makes retrieval:
- more transparent
- easier to verify
- closer to what a human sees inside Obsidian
- often cheaper in token cost than broad note dumping or first-pass vectorization

## What the skill covers

The skill is centered around these commands:

- `search`
- `search:context`
- `read`
- `outline`
- `backlinks`
- `links`
- `tags`
- `properties`
- `tasks`
- `unresolved`

It defaults to a **read-first** posture and documents safer boundaries around write-capable commands such as `append`, `delete`, `command`, and `eval`.

## Repository contents

- `obsidian-official-cli/` — skill source
- `dist/obsidian-official-cli.skill` — packaged skill artifact
- `README.zh-CN.md` — Chinese README

## Requirements

- Obsidian Desktop **1.12+**
- Obsidian **Command line interface** enabled in:
  - `Settings -> General -> Command line interface`
- A working `obsidian` command

On macOS, using a login shell may help ensure PATH includes:

```text
/Applications/Obsidian.app/Contents/MacOS
```

## Example usage patterns

### Find topic-related notes

```bash
obsidian search query="PLC" limit=10 format=json
obsidian search:context query="PLC" limit=10 format=json
```

### Read only the relevant note

```bash
obsidian read path="文档发布/2026-03-08_边缘AI推理安全与网关防护.md"
```

### Expand into note graph structure

```bash
obsidian backlinks file="Central note" format=json counts
obsidian links file="Central note"
```

### Inspect vault structure

```bash
obsidian tags counts format=json
obsidian properties counts format=json
obsidian unresolved total
```

## Installation

### Option 1: use the packaged artifact

Use the packaged file in:

```text
dist/obsidian-official-cli.skill
```

### Option 2: copy the source skill directory

Copy:

```text
obsidian-official-cli/
```

into your OpenClaw skills directory.

## Design choices

This skill intentionally does **not** replace older community `obsidian-cli` / NotesMD workflows. It is a parallel skill for environments where the official `obsidian` CLI is already available and preferred.

It also intentionally favors:
- small, explainable retrieval steps
- structured output (`format=json`) when possible
- narrow reads over whole-vault ingestion

## Status

- validated
- packaged
- tested against a real Obsidian vault using the official CLI

## License

MIT
