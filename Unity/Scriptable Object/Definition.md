---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## Definition
- `ScriptableObject` là kiểu serializable trong Unity kế thừa từ `UnityEngine.Object`, cho phép tạo các asset chứa data độc lập với `GameObject` và scene.
- Theo tài liệu Unity `6.4 (6000.4)`, giá trị chính của `ScriptableObject` là data store, dùng làm container cho shared data giữa nhiều object lúc runtime, giúp giảm memory do tránh duplicate giá trị.

## Related notes
- [[Scriptable Object]]
- [[CreateAssetMenu]]
- [[Lifecycle]]
- [[Data Persistence]]
- [[Event Channel]]
- [[Runtime Set]]
- [[Summary]]
