# AGENTS.md

This file explains how an agent should read, navigate, and extend this Obsidian vault.

## Scope

- This repository is an Obsidian vault, not an application codebase.
- The primary source material is Markdown notes plus Obsidian configuration.
- Each root folder is a separate field of knowledge.
- The current field is `Unity/`, with room to grow later.

## Root Field Model

- Treat every top-level content folder in `/` as a field boundary.
- When an agent enters a field, it must first read that field's note with the same name as the folder.
- That field note is the rule entry point for everything inside the field.
- Deeper folders inside the field may define narrower topic rules, but they do not replace the field-level rules unless they explicitly narrow them.

Example:

- Entering `Unity/` means the agent must read `Unity/Unity.md` first.
- After that, if the agent works inside `Unity/Bất đồng bộ/`, it should also read `Unity/Bất đồng bộ/Bất đồng bộ.md`.

## Rule Precedence

Read rules in this order before making changes:

1. This `AGENTS.md` for repository-wide agent behavior.
2. The current root field note whose filename matches its folder name.
3. The nearest deeper father note whose filename matches its folder name.
4. The matching template note in `Templates.hide/` when creating a new note.
5. Existing neighboring notes as style examples.

Examples:

- `Unity/Unity.md` is the authoritative rule note for the Unity area and currently defines the main vault model.
- `Unity/Bất đồng bộ/Bất đồng bộ.md` defines the scope and note index for that topic folder.

## Folder Map

### `/`

- Contains repository docs, vault config folders, and top-level knowledge areas.
- Any top-level content folder here should be treated as a field.
- `README.md` is for humans.
- `AGENTS.md` is for agents and maintainers.

### `/.obsidian/`

- Obsidian app, theme, plugin, and UI configuration.
- Treat this as tooling config, not learning content.
- Read it when you need setup context such as enabled plugins, theme, or hidden-file behavior.
- Avoid relying on `workspace*.json`; it is local layout state and is ignored in git.

### `/.space/`

- make.md space metadata for navigation.
- `def.json` and `waypoints.json` are useful config.
- Do not treat `.space` files as knowledge notes.

### `/.makemd/`

- Generated make.md cache/state.
- Ignore it for authoring decisions.
- It is excluded from git.

### `/.trash/`

- Obsidian trash.
- Never use it as source-of-truth content.
- It is excluded from git.

### `/Unity/`

- Main field root for the current vault.
- Read `Unity/Unity.md` before editing or adding anything under this tree.
- `Unity/Unity.md` defines:
  - vault model
  - note types
  - naming rules
  - language rules
  - father note and child note structure
  - inbox and template workflow
- Everything inside `Unity/` inherits these rules unless a deeper folder adds narrower rules.

### `/Unity/Templates.hide/`

- System-only support area.
- Contains note templates and the template index note.
- Use these files when creating notes:
  - `Unity/Templates.hide/Structure Note Template.hide.md`
  - `Unity/Templates.hide/Permanent Note Template.hide.md`
  - `Unity/Templates.hide/Fleeting Note Template.hide.md`
- Do not store topic knowledge here.

### `/Unity/Inbox.hide/`

- System-only capture area for `fleeting notes`.
- Read `Unity/Inbox.hide/Inbox.hide.md` before changing workflow assumptions.
- Notes here may be incomplete, temporary, or disposable.
- Useful ideas should be rewritten into proper permanent notes later.

### `/Unity/Bất đồng bộ/`

- Existing topic folder for asynchronous Unity concepts.
- `Unity/Bất đồng bộ/Bất đồng bộ.md` is the father note for this folder.
- `Coroutine.md` is a child permanent note and is the best current example of a finished concept note.

## How To Read The Vault Correctly

- First identify which root field folder you are working in.
- Read that field's same-name note before doing anything else in that field.
- A folder with a Markdown file of the same name is a topic area with a father note.
- The father note is the structure note and should organize the child notes in that folder.
- Child notes live directly inside the same folder as the father note.
- Folders ending in `.hide` are support areas, not topic areas.
- Files ending in `.hide.md` are hidden support notes, templates, or workflow notes.
- Use links as the primary structure. Do not try to organize the vault mainly with tags.
- A deeper folder-specific rule note can narrow the field rules, but the field rules still apply unless clearly contradicted.

## How To Handle Raw Information

- When the user provides raw information, do not dump it directly into one note.
- First convert the raw input into structured knowledge that fits the field rules and folder structure.
- Use this pipeline:
  - `Filter`
  - `Classify`
  - `Group`
  - `Extract`
  - `Compress`

### Filter

- Keep only information that is relevant to the user's current objective.
- Remove duplicates, noise, filler, and low-value repetition.
- Normalize obvious metadata when available:
  - source
  - date
  - author
  - topic
  - document type

### Classify

- If the field already has clear categories, father-note patterns, or topic folders, classify the raw information into those existing buckets first.
- Prefer existing field taxonomy over inventing new labels.
- Example in `Unity/`: map content into the correct topic folder before writing notes.

### Group

- If the information does not fit an existing structure cleanly, group it by natural themes before writing notes.
- Split mixed raw input into separate concepts, claims, comparisons, workflows, or examples.
- If one chunk answers "what it is", it likely belongs in `Definition.md`.
- If one chunk mainly contrasts options, trade-offs, or choices, it likely belongs in `Comparison.md`.
- If one chunk is a standalone concept, make it its own permanent note.

### Extract

- Pull important information out of paragraph form into reusable knowledge units.
- Extract, when present:
  - core concept
  - claims
  - actions
  - outcomes
  - entities
  - dates
  - sources
  - important terms and key phrases
- For high-accuracy content, prefer extractive capture first, then rewrite in your own words second.
- Preserve uncertainty instead of over-claiming when the raw input is ambiguous.

### Compress

- Only after filtering and extraction, rewrite for readability.
- Put the most important conclusion first.
- Use short sections, short bullets, and plain language.
- Prefer 3 to 5 strong buckets over long undifferentiated lists.
- Use progressive disclosure: main point first, supporting detail after.

## Output Model For Raw Information

- When converting raw information into notes, aim for two layers:
  - machine-readable structure
  - human-readable explanation

### Machine-readable structure

- Preserve or infer, when useful:
  - topic
  - category
  - key entities
  - key phrases
  - source
  - date
  - importance
  - confidence

### Human-readable structure

- Prefer this shape:
  - one-sentence conclusion first
  - 3 to 5 main points
  - for each point: main idea, why it matters, supporting detail

## How To Turn Raw Information Into Vault Notes

1. Identify the root field and read its rule note.
2. Filter the raw input to the user's actual goal.
3. Classify into an existing folder if possible.
4. Group mixed content into separate note-sized units.
5. Extract facts, claims, examples, and terminology before rewriting.
6. Create or update the smallest set of notes that preserves clarity.
7. Use `Definition.md` for core meaning and `Comparison.md` for compare/contrast content when the field rules allow it.
8. Prefer multiple linked permanent notes over one overloaded summary note.
9. Update the father note index and related links after writing.

## How To Add A New Child Note

1. Identify the root field.
2. Read that field's same-name rule note.
3. Read the target folder's father note.
4. Use the field's permanent note template.
5. Create the file directly inside the target topic folder.
6. Use an English filename that names one concept or one strong claim.
7. Add `created: YYYY-MM-DD` in frontmatter for the permanent note.
8. Keep headings in English and write the body mainly in Vietnamese.
9. Add links to the father note and related notes.
10. Update the father note under `## Notes` with a short description.

## How To Add A New Topic Folder

1. Choose the correct root field folder.
2. Read the field's same-name rule note.
3. Create the new topic folder inside that field unless the user asks for a new field.
4. Create the father note inside that folder with the exact same name as the folder.
5. Use the field's structure note template.
6. Add child notes directly inside the folder, not inside extra subfolders.
7. Add the new father note link to the field root note.

## How To Add A New Fleeting Note

1. Identify the correct root field.
2. Use that field's `Inbox.hide/`.
3. Use that field's fleeting note template.
4. Use a timestamp or short capture name.
5. Rewrite or discard it later.
6. If promoted into a permanent note, move the idea into the proper topic folder instead of keeping the inbox note as the final version.

## Naming And Content Rules

- Folder names: Vietnamese or English allowed.
- Father note names: must exactly match the folder name.
- Child note filenames: English only.
- Hidden support folders: use the `.hide` suffix.
- Hidden support notes: use the `.hide.md` extension.
- Avoid fragile filename characters such as `#`, `|`, `^`, `:`, `%`, `[[`, and `]]`.
- Prefer short, specific, unique note names.
- Avoid vague names like `Basics`, `Overview`, `Misc`, or `Part 1`.

## Editing Rules For Agents

- Use UTF-8 when reading or writing notes because the vault contains Vietnamese text and folder names.
- Always determine the current root field before editing notes.
- Entering a field means reading that field's same-name rule note first.
- Permanent notes should include a `created` field in frontmatter using `YYYY-MM-DD`.
- If you touch an older permanent note that is missing `created`, add it when the correct date is reasonably known.
- When the user gives raw material, restructure it into note-sized knowledge units instead of preserving the source shape by default.
- Prefer extract-first, rewrite-second for high-accuracy material.
- Do not rename a father note without renaming its folder to match.
- Do not create separate table-of-content files when the father note already covers that role.
- Do not add knowledge notes inside `.hide` folders.
- Do not use `.trash/` as a reference when recreating content.
- Keep repository docs in the root and note content in the vault folders.

## Git Expectations

- Keep note content, templates, and stable Obsidian config tracked.
- Ignore local or generated state such as:
  - `.trash/`
  - `.makemd/`
  - `**/.space/context.mdb`
  - `.obsidian/workspace*.json`

When uncertain, follow the nearest father note and existing templates instead of inventing a new note schema.
