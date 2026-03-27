---
color: ""
tags:
  - unity
---

# Unity Notes Rules

## Vault model
- This vault uses a hybrid model: Obsidian for linking and navigation, Zettelkasten for note design.
- `Unity.md` is the root note of the whole vault.
- A topic area uses this pattern:
  - one folder
  - one father note `.md`
  - the father note file name must be exactly the same as the folder name
- Example:
  - folder: `Bất đồng bộ/`
  - father note: `Bất đồng bộ/Bất đồng bộ.md`
- The father note is the `structure note` for that folder.
- A child note is a leaf note inside the father folder.
- A child note does not get its own folder.
- Child notes are the main knowledge units of the vault.
- A separate `Tables of Content.md` is no longer needed.
- The father note itself should list and organize its child notes.

## Note types
- Father note: `structure note`
- Child note: `permanent note`
- Inbox note: `fleeting note`
- Template note: system note used to create new notes faster

## Naming language rules
- Folder names: Vietnamese or English are allowed.
- Father note names: Vietnamese or English are allowed, but they must exactly match the folder name.
- Child note file names: English only.
- Hidden support folders should use the `.hide` suffix.
- Use hidden support folder names such as `Inbox.hide/` and `Templates.hide/`.
- Hidden support notes should use the `.hide.md` extension.
- Use `.hide.md` for notes that should be easy to hide from graph or navigation, such as `Inbox.hide.md`, `Templates.hide.md`, and template notes.
- Hidden support folders should still use English names even when hidden.
- Use safe filename characters for folders and notes.
- Avoid characters that make Obsidian links or filenames fragile, such as `#`, `|`, `^`, `:`, `%`, `[[`, and `]]`.

## Child note naming rules
- One child note should cover one main concept or one strong claim.
- Each topic folder may reserve these canonical child note names:
  - `Definition.md`: the folder's core definition and mental model note
  - `Comparison.md`: the folder's comparison note for deciding between related concepts in that folder
- Prefer a sharp concept handle when the note is about a thing:
  - `Coroutine`
  - `ScriptableObject`
  - `Object Pooling`
- If the note is about a claim, rule, or comparison, prefer a complete phrase:
  - `Coroutines do not create real parallelism`
  - `Update is not a scheduler`
- `Definition.md` and `Comparison.md` are explicit exceptions to the "prefer specific names" rule because they play fixed roles inside a topic folder.
- Keep child note names short, specific, and unique.
- Avoid vague names such as `Basics`, `Overview`, `Notes`, `Misc`, `Part 1`, or `More`.
- Keep acronyms only when they are already the common term, such as `ECS` or `GC`.
- If you want Vietnamese discoverability, keep the English file name and add Obsidian `aliases` for Vietnamese or alternate English names.

## Child note language rules
- Child note section headings must be in English.
- Child note body content should be mainly Vietnamese for now.
- Technical terms, API names, class names, code identifiers, and important phrases should stay in English.
- Full dual-language writing is not required.
- Add English only when it improves clarity, searchability, or naming consistency.

## Child note frontmatter rules
- Permanent notes should include `note_type: permanent`.
- Permanent notes should include `created: YYYY-MM-DD`.
- Add `aliases` when alternate English or Vietnamese discovery would help.
- Keep frontmatter stable and minimal; do not add noisy metadata unless it serves retrieval or workflow.

## Child note color rules
- `color` is optional frontmatter and should be used as a visual workflow marker, not as the main organization system.
- `Definition.md` and `Comparison.md` may share the same `color` to mark them as the canonical orientation notes inside a topic folder.
- Current shared value for that pair in this field: `var(--mk-color-base-40)`.
- If a topic folder already uses a shared color for `Definition.md` and `Comparison.md`, preserve that color when editing those notes.
- Do not add `color` to normal permanent notes by default unless the user wants a broader color system.

## Zettelkasten rules in this vault
- Treat each child note as a `permanent note`:
  - self-contained
  - understandable later without opening the original source again
  - written mostly in your own words
- Treat each father note as a `structure note`:
  - it organizes access to a topic area
  - it should stay concise
  - it should list child notes directly
- Use links as the main relationship system.
- Folder location tells where a note starts.
- Links tell how ideas actually relate.
- A child note should link to related child notes, even across different father folders.
- Do not rely on tags as the main structure.
- Use aliases, backlinks, outgoing links, and local graph as support tools.
- If a note becomes about several unrelated ideas, split it.
- If a note is only a list of subtopics, that content belongs in the father note, not in a child note.
- Keep orphan notes low; if a note has no meaningful links, connect it or merge it.

## Father note structure
Use this structure for each father note:

````md
# Father Note

## Scope
- What this topic area covers.

## Notes
- Add child note link here: short description
- Add another child note link here: short description

## Related structure notes
- Add related father note link here
````

## Child note structure
Use this structure for each child note:

````md
---
aliases:
  - Optional Vietnamese name
  - Optional alternate English name
note_type: permanent
created: {{date:YYYY-MM-DD}}
---

## One-line answer
- Short definition in Vietnamese.
- Keep key technical terms in English.

## Problem
- What problem this concept solves.
- What goes wrong without it.

## Core idea
- Main idea in simple words.

## Mechanism
- How it works.

## When to use
- When it fits.

## Benefits
- Main advantages.

## Trade-offs / Limits
- Main drawbacks or boundaries.

## Common mistakes
- Common misunderstanding or misuse.

## Example
- Small practical example.

```csharp
// optional code
```

## Related notes
- Add father note link here.
- Add related note link here.
````

## Inbox workflow
- `Inbox.hide/` is the folder for `fleeting notes`.
- Fleeting notes are fast captures, not finished knowledge notes.
- They can be messy, incomplete, or time-based.
- Rewrite useful fleeting notes into proper child notes later.
- After rewriting, link the permanent note into the correct father note.

## Obsidian workflow rules
- Use `Templates.hide/` as the template folder for consistent note creation.
- Use `Unique note creator` if you want fast timestamp capture for fleeting notes in `Inbox.hide/`.
- Use Backlinks and Outgoing links after writing to find missing connections.
- Use local Graph view to inspect a note neighborhood, not as the main writing view.
- Father notes act as structure notes, so a separate `Tables of Content.md` is not needed.
- Use `.hide` folders and `.hide.md` notes for support areas that you want to exclude more easily from graph or navigation.

## Folder checklist for new topic notes
1. Create the folder.
2. Create the father note with the same name as the folder.
3. If needed, create `Definition.md` for the topic definition and `Comparison.md` for topic comparisons.
4. Create other child notes directly inside that folder with English file names.
5. Add the child note links into the father note under `## Notes`.
6. Add direct links from the child note to related notes.
7. Add the father note link into `Unity.md`.

## Topic areas
- [[Bất đồng bộ]]: asynchronous flow, coroutine, task, awaitable, threads, và continuation behavior.
- [[DOTS ECS]]: data-oriented architecture, entities, systems, baking, archetypes/chunks, và Job System.
- [[Draw Call]]: draw call mental model, batching, instancing, `SRP Batcher`, và workflow tối ưu render CPU overhead.
