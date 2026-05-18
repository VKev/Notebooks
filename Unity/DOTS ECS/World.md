---
aliases:
  - ECS World
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `World` là container của ECS, nơi chứa entity, system, và cách chúng được bootstrap và update.

## What is it
- Theo `Entities 1.4.2`, world là collection của entities.
- ID của entity chỉ unique bên trong world của nó.
- World sở hữu `EntityManager` và một tập system, đồng thời là ngữ cảnh tổ chức archetype và memory layout.

## How it works
- Khi vào Play mode, Unity mặc định tạo một default world và thêm system vào world đó.
- Nếu cần kiểm soát nhiều hơn, bạn có thể bootstrap thủ công bằng `ICustomBootstrap`.
- Nếu tự tắt automatic bootstrap, code của bạn phải tự tạo world, system, và chèn update của world vào `PlayerLoop`.

## Why use it
- World giúp ECS chia bối cảnh chạy rõ ràng, đặc biệt khi cần runtime world, editor world, hoặc world chuyên biệt.
- Là lớp tổ chức quan trọng để hiểu entity sống ở đâu và system nào xử lý chúng.

## When to use it
- Dùng như mental model khi học ECS, vì mọi entity và system đều gắn với một world.
- Dùng khi bạn cần nhiều world cho test, mô phỏng riêng, hoặc bootstrap đặc biệt.

## When to not use it
- Không nên tự quản lý nhiều world nếu bạn chưa thật sự cần, vì độ phức tạp tăng nhanh.
- Không cần bootstrap thủ công cho những project ECS đơn giản dùng default world là đủ.

## Limitations
- Nhiều world làm việc debug và quản lý update order khó hơn.
- Entity không chuyển tự do giữa world như thể cùng một không gian ID.

---

## Example code
```csharp
using Unity.Entities;

public partial struct WorldInfoSystem : ISystem
{
    public void OnCreate(ref SystemState state)
    {
        var entityManager = state.EntityManager;
        var worldName = state.WorldUnmanaged.Name;
    }
}
```

---

## Related notes
- [[Definition]]
- [[Entity]]
- [[Systems]]
- [[Archetypes and Chunks]]
- [[Summary]]
