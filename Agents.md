# AGENTS.md

## Purpose
This vault is organized for structured learning and compact note-taking.

The goal is:
- keep knowledge modular
- separate broad topics from specific concepts
- make review easier
- make linking between notes consistent

---

## Core Structure

A vault is organized into **Field nodes**, **Section nodes**, and **Note nodes**.

### 1. Field node
A **Field node** is a top-level domain folder.
A field node must also contain a markdown file with the same name as the folder.
That note briefly describes what information belongs in the field folder.

Examples:
- `Unity`
- `Machine Learning`
- `Networking`
- `Database`

### 2. Section node
A **Section node** is a subfolder inside a field folder.
It groups a specific subtopic inside that field.
A section node must also contain a markdown file with the same name as the folder.
That note briefly describes what information belongs in the section folder.

Examples:
- `Unity/Asynchronous`
- `Unity/Rendering`
- `Machine Learning/Optimization`

### 3. Note node
A **Note node** is a markdown file inside a section folder.

Each section node must contain:
- one same-name note that briefly describes the folder
- `Definition.md`
- one or more feature notes
- `Summary.md`

---

## Required Folder Rule

For each field, create one folder.

When adding, updating, or deleting notes, always preserve the field > section > note structure.
- Add new notes inside the correct section folder.
- Keep each field complete with a same-name note such as `Unity/Unity.md`.
- Keep each section complete with a same-name note such as `Unity/Asynchronous/Asynchronous.md`, plus `Definition.md`, feature notes, and `Summary.md`.
- Use English for folder names and file names only.
- Write the content of `Definition.md`, feature notes, and `Summary.md` in Vietnamese.
- Write the same-name field note and same-name section note as brief folder descriptions.
- Feature notes should start with YAML properties using `---` at the top of the file.
- Keep properties minimal and atomic.
- Use `aliases` as a YAML list when alternate names help search or linking.
- When deleting or renaming notes, update related field notes and section summaries to avoid broken links.

Example:

```text
Unity/
|- Asynchronous/
|  |- Asynchronous.md
|  |- Definition.md
|  |- Coroutine.md
|  |- AsyncAwait.md
|  `- Summary.md
`- Rendering/
   |- Rendering.md
   |- Definition.md
   |- DrawCall.md
   |- TextureAtlas.md
   `- Summary.md
```
