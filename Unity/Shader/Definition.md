---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## Definition
- `Shader` là chương trình chạy trên GPU để quyết định cách mesh, sprite, UI, hoặc full-screen effect được vẽ ra màn hình.
- Theo Unity `6.4 (6000.4)`, `Shader` object là container Unity-specific cho GPU program.
- Một shader có thể chứa `Properties`, `SubShader`, `Pass`, tag, fallback, và metadata để Unity gắn shader với material.
- Trong Unity hiện đại, shader viết tay dùng `ShaderLab` để mô tả cấu trúc và `HLSL` để viết code chạy trên GPU.

## Related notes
- [[Shader]]
- [[ShaderLab]]
- [[HLSL Shader Code]]
- [[Shader Pragma]]
- [[Shader Render State]]
- [[Summary]]
