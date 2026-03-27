# vkev

Personal Obsidian vault for long-term learning notes.

The current vault is centered on Unity, but the structure is intended for broader study over time. Notes are written mostly in Vietnamese, while technical terms, section headings, and child note filenames stay in English for consistency and searchability.

## Vault Model

- Obsidian is used for navigation, linking, and local graph views.
- The note design follows a lightweight Zettelkasten model.
- Each topic area uses one folder and one father note with the exact same name as the folder.
- The father note acts as the `structure note` for that topic.
- Child notes act as `permanent notes` and live directly inside the topic folder.
- `Inbox.hide/` stores `fleeting notes`.
- `Templates.hide/` stores note templates and other support notes.

## Current Structure

```text
.
|-- Unity/
|   |-- Unity.md
|   |-- Bất đồng bộ/
|   |   |-- Bất đồng bộ.md
|   |   `-- Coroutine.md
|   |-- Inbox.hide/
|   `-- Templates.hide/
|-- .obsidian/
|-- .space/
|-- .trash/
`-- .makemd/
```

## Writing Rules

- Folder names and father note names may be Vietnamese or English, but they must match exactly.
- Child note filenames should be English only.
- Child note headings should be in English.
- Child note body content should be mainly Vietnamese for now.
- Use links as the main structure. Tags are secondary.
- Do not create separate `Tables of Content.md` files when a father note already plays that role.

## Workflow

1. Capture rough ideas in `Unity/Inbox.hide/`.
2. Rewrite useful captures into permanent notes inside the correct topic folder.
3. Add the new child note link to the father note under `## Notes`.
4. Add links to related notes so the graph stays connected.
5. When creating a new topic folder, create its father note first and match the folder name exactly.

## Obsidian Setup

- Theme: `Minimal`
- Core plugins in use include `Templates`, `Daily notes`, `Backlinks`, `Outgoing links`, and `Graph`.
- Community plugin in use: `make-md`
- Hidden support notes use the `.hide.md` suffix and are filtered from normal browsing.

## Git Tracking

This repository tracks the vault content, templates, and useful Obsidian configuration. Local trash and generated state are ignored with `.gitignore`, including the Obsidian trash folder, make.md cache files, per-space context databases, and local workspace layout files.

For agent-facing operating rules, see `AGENTS.md`.
