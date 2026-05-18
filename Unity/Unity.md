---
note_type: field
tags:
  - unity
---

field note của folder `Unity`.
Nó mô tả phạm vi kiến thức Unity trong vault và định nghĩa rule viết note cho các section bên trong folder này.
Các section hiện tại gồm những nhóm như `Addressables`, `Asynchronous`, `DOTS ECS`, `Draw Call`, `MonoBehaviour Lifecycle`, `Object Pooling`, `Render Pipeline`, `Scriptable Object`, `Serialization`, `Shader`, `Texture`, và `UI Toolkit`.

## Format rules
- Chi tiết format chung nằm ở [[../Vault|Vault Format System]].
- Feature note mặc định dùng `## Core idea`, `## Key points`, `## Decision rules`, tùy chọn `## Example`, và `## Related notes`.
- Chỉ tách thêm heading khi note cần review theo API, workflow, setting, hoặc performance riêng.
- Với Unity API note, ưu tiên `## API surface`, `## Parameters`, `## Mechanics`, `## Example`, `## Common traps`.
- Với Unity architecture/system note, ưu tiên `## Mechanics`, `## Workflow`, `## Decision rules`, `## Tradeoffs`.
- Với Unity performance/rendering note, ưu tiên `## Performance notes`, `## Decision rules`, `## Common traps`.
- Với Unity settings note, ưu tiên `## Important settings`, `## Decision rules`, `## Common traps`.
- Definition note dùng `## Definition` và `## Related notes`.
- Summary note được phép có heading domain-specific như `## Rendering paths`, `## Lifecycle`, `## Architecture patterns`.

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
