---
note_type: definition
tags:
  - unity
sticker: lucide//star
---

## What is it
- `Object Pooling` là pattern tái sử dụng object bằng cách giữ chúng trong một pool thay vì liên tục `Instantiate` và `Destroy`, giúp giảm allocation trên heap và giảm Garbage Collection pressure.
- Theo tài liệu Unity `6.3`, Unity cung cấp sẵn `ObjectPool<T>` trong namespace `UnityEngine.Pool` từ Unity 2021 trở đi, hỗ trợ pool cho cả `GameObject` và các kiểu C# thông thường.

## Related notes
- [[Object Pooling]]
- [[ObjectPool API]]
- [[Pool Best Practices]]
- [[Summary]]
