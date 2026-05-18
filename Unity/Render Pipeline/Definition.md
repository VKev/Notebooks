---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## Definition
- `Render Pipeline` là quy trình mà Unity thực hiện để đưa scene lên màn hình, gồm ba giai đoạn chính: `Culling` loại bỏ object ngoài camera, `Rendering` vẽ object với lighting, và `Post-processing` áp dụng hiệu ứng cuối cùng.
- Theo tài liệu Unity `6.3 LTS (6000.3)`, Unity có `Built-in Render Pipeline`, `URP`, và `HDRP`; trong đó `URP` và `HDRP` là render pipeline dựng trên nền tảng `Scriptable Render Pipeline`.
- Trong Unity 6, custom rendering trong URP nên được viết bằng `Scriptable Render Pass` kết hợp `Render Graph`.

## Related notes
- [[Render Pipeline]]
- [[SRP]]
- [[Render Graph]]
- [[Scriptable Render Pass]]
- [[URP]]
- [[HDRP]]
- [[Shader Graph]]
- [[Summary]]
