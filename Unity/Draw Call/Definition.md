---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## What is it
- `Draw Call` là lần CPU gửi lệnh để GPU vẽ một mesh hoặc một nhóm mesh với render state phù hợp.
- Theo tài liệu Unity `6.3`, phần tốn CPU nhất thường không phải bản thân lệnh vẽ, mà là bước chuẩn bị render state như shader, texture, buffer, và các tham số liên quan.
- Section này tập trung vào các kỹ thuật Unity dùng để giảm số draw call hoặc giảm chi phí CPU khi chuẩn bị draw call.

## Related notes
- [[Draw Call]]
- [[Render State]]
- [[Batching]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[Summary]]
