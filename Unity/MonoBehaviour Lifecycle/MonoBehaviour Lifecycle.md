---
note_type: section
tags:
  - unity
---

## Scope
Folder này chứa kiến thức về `MonoBehaviour Lifecycle` trong Unity.
Tập trung vào Unity `6.4`, giải thích callback nào chạy khi object khởi tạo, bật/tắt, update, xử lý physics, render, và bị hủy.
Mục tiêu là nhớ đúng nơi đặt logic: `Awake` cho self-setup, `Start` cho cross-object setup, `FixedUpdate` cho physics, `Update` cho input/game logic, `LateUpdate` cho logic sau movement, và `OnDisable` cho cleanup.

## Note map
- [[Definition]]: Gốc.
- [[Decommissioning]]: Chi tiết.
- [[Execution Order]]: Chi tiết.
- [[Initialization]]: Chi tiết.
- [[Update Loop]]: Chi tiết.
- [[Summary]]: Review.

## Related notes
- [[../Unity|Unity]]
