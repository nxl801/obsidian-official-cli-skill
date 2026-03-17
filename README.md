# Obsidian Official CLI Skill for OpenClaw

An OpenClaw-compatible AgentSkill for working with **Obsidian 1.12+ official CLI** (`obsidian`).

This skill is designed for retrieval-first agent workflows against a running Obsidian desktop app, with emphasis on:

- `search`
- `search:context`
- `read`
- `backlinks`
- `links`
- `tags`
- `properties`
- `tasks`
- `outline`
- `unresolved`

It is intended for **OpenClaw / Claude Code / agent** use cases where you want transparent, verifiable retrieval from a live Obsidian vault without relying on vector import as the first step.

## Repository contents

- `obsidian-official-cli/` — skill source
- `dist/obsidian-official-cli.skill` — packaged skill artifact

## Skill highlights

- Targets the **official** Obsidian CLI introduced in Obsidian 1.12+
- Keeps a **read-first** default posture
- Encourages `search -> search:context -> read -> backlinks/links/properties`
- Includes Chinese and English retrieval examples
- Documents write boundaries for higher-risk commands like `eval` and `command`

## Requirements

- Obsidian Desktop 1.12+
- Obsidian **Command line interface** enabled in:
  - `Settings -> General -> Command line interface`
- On macOS, using a login shell may help ensure PATH includes:
  - `/Applications/Obsidian.app/Contents/MacOS`

## Install / use

### Source form
Copy `obsidian-official-cli/` into your OpenClaw skills directory.

### Packaged form
Use the packaged artifact in `dist/obsidian-official-cli.skill`.

## Notes

This skill intentionally does **not** replace older community `obsidian-cli` / NotesMD workflows. It is a parallel skill for environments that already have the official `obsidian` command available.

## License

MIT
