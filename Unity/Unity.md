---
note_type: field
tags:
  - unity
---

Đây là field note của folder `Unity`.
Nó mô tả phạm vi kiến thức Unity trong vault và định nghĩa rule viết note cho các section bên trong folder này.

## Note Formats

### Feature note
````md
---
aliases:
  - Alternate note name
note_type: feature
tags:
  - unity
---

## One-line
- ...

## What is it
- ...

## How it works
- ...

## Why use it
- ...

## When to use it
- ...

## When to not use it
- ...

## Limitations
- ...

---

## Example code
```csharp
// code here
```

---

## Related notes
- [[Definition]]
- [[Summary]]
````

### Definition note
````md
---
note_type: definition
tags:
  - unity
---

## What is it
- ...

## Related notes
- [[FeatureNote]]
- [[Summary]]
````

### Summary note
````md
---
note_type: summary
tags:
  - unity
---

## Core keywords
- `Keyword`
  Short Vietnamese explanation.
  Another short supporting line.

## Another section
- `Keyword`
  Short Vietnamese explanation.
  Another short supporting line.

## Interview questions and answers
### Câu hỏi?
- Câu trả lời ngắn gọn.
````

## Notes
- Folder names and file names stay in English.
- `Definition.md`, feature notes, and `Summary.md` should be written in Vietnamese content.
- `Definition.md`, feature notes, and `Summary.md` should use minimal YAML properties at the top of the note.
- Each note type has its own icon.
- Keep the icon for feature notes, definition notes, and summary notes consistent when creating or updating notes.
- Each field folder must have a same-name note such as `Unity/Unity.md`.
- Each same-name field note and same-name section note should be a short folder description, not a table of contents.
- Each section folder must have a same-name note such as `Asynchronous/Asynchronous.md`.
- Keep the note order consistent inside each section: same-name note first, `Definition.md` next, feature notes in the middle, and `Summary.md` last.
- Validate Unity technical facts against official Unity documentation version `6.3` or newer before adding or updating notes.
- If a behavior, API, or workflow is version-specific, write the Unity version explicitly in the note.
