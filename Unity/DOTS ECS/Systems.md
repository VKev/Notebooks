---
aliases:
  - ECS Systems
  - System trong ECS
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `System` là nơi logic của ECS chạy; nó query các entity có component phù hợp rồi xử lý chúng theo batch thay vì gắn behavior lên từng object riêng lẻ.

## Problem
- Nếu logic nằm phân tán trên từng object, bạn khó batch work và khó tận dụng layout data của ECS.
- ECS cần nơi tập trung logic theo data shape chứ không theo object identity.

## Core idea
- System nói "hãy xử lý tất cả entity có tập component này".
- Đây là thay đổi lớn nhất trong tư duy từ OOP sang ECS.

## Mechanism
- Current Entities docs và samples ưu tiên tổ chức logic vào system rồi query bằng `SystemAPI` hoặc job.
- System thường:
  - đọc/ghi component data
  - lọc entity theo query
  - schedule job hoặc chạy foreach
  - quản lý dependency của hệ thống
- `ISystem` là một pattern hiệu năng tốt cho ECS runtime code hiện nay.

## When to use
- Dùng cho mọi logic cần chạy trên group entity có component chung.
- Hợp cho movement, combat resolution, cooldown update, spawn, cleanup, AI processing.

## Benefits
- Logic tập trung và batch-friendly.
- Dễ phối hợp với `Job System`.
- Giảm coupling giữa data và behavior.

## Trade-offs / Limits
- Debug flow ban đầu có thể lạ nếu quen `Update()` per object.
- Query design kém sẽ làm system khó đọc hoặc khó maintain.

## Common mistakes
- Viết system như thể nó chỉ xử lý một entity.
- Thực hiện structural changes bừa trong hot path thay vì dùng buffer/ECB phù hợp.
- Tách quá nhiều system nhỏ nhưng không rõ responsibility.

## Example
- System query data và xử lý hàng loạt:

```csharp
foreach (var (transform, speed) in
    SystemAPI.Query<RefRW<LocalTransform>, RefRO<Speed>>())
{
    transform.ValueRW.Position +=
        math.forward() * speed.ValueRO.Value * SystemAPI.Time.DeltaTime;
}
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Entity]]
- [[Components]]
- [[World]]
- [[Job System]]
