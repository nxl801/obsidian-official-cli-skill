# Official Obsidian CLI command reference

## Core commands

```bash
obsidian version
obsidian help
obsidian reload
obsidian restart
```

## Search and reading

```bash
obsidian search query="text" limit=20 format=json
obsidian search:context query="text" limit=20 format=json
obsidian read file="Note"
obsidian read path="Folder/Note.md"
obsidian outline file="Note" format=json
```

Use `search` to get candidate files. Use `search:context` when you need matching lines to judge relevance before reading.

## Links and graph structure

```bash
obsidian backlinks file="Note" format=json counts
obsidian links file="Note"
obsidian unresolved total
obsidian unresolved verbose format=json
obsidian orphans total
obsidian deadends total
```

## Tags and properties

```bash
obsidian tags counts format=json
obsidian tag name="#plc" verbose
obsidian properties counts format=json
obsidian properties file="Note" format=json
obsidian property:read name=status file="Note"
```

## Tasks

```bash
obsidian tasks todo format=json
obsidian tasks done format=json
obsidian tasks daily format=json
obsidian task ref="path/to/file.md:42"
```

## Useful output habits

Prefer structured output when downstream parsing matters:

```bash
format=json
counts
total
verbose
```

## Notes

- Use `file=<name>` for wikilink-like lookup.
- Use `path=<exact/path>` when note names are ambiguous.
- If a command appears to return nothing, try a more targeted query, `search:context`, or exact `path=`.
- On macOS, run through a login shell when PATH propagation is inconsistent:

```bash
zsh -lic 'obsidian version'
```
