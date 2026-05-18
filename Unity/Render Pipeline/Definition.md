---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## Definition
- `Render Pipeline` là quy trình Unity dùng để đưa scene lên màn hình.
- Ba bước dễ nhớ: `Culling` chọn object thấy được, `Rendering` vẽ object với lighting, `Post-processing` áp dụng hiệu ứng cuối.
- Theo tài liệu Unity `6.4 (6000.4)`, Unity có `Built-in Render Pipeline`, `URP`, và `HDRP`; trong đó `URP` và `HDRP` là render pipeline dựng trên nền tảng `Scriptable Render Pipeline`.
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
