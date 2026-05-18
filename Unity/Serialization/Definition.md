---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## Definition
- `Serialization` trong Unity là quá trình chuyển state của object, component, scene, prefab, hoặc asset thành dạng Unity có thể lưu lại và dựng lại sau.
- Theo tài liệu Unity `6.3 LTS (6000.3)`, Unity dùng serialization system riêng, không phải .NET serialization thông thường.
- Serialization quyết định field nào xuất hiện trong Inspector, field nào được lưu vào scene/prefab/asset, và dữ liệu nào được phục hồi sau domain reload hoặc khi mở lại project.

## Related notes
- [[Serialization]]
- [[SerializeField]]
- [[SerializeReference]]
- [[Summary]]
