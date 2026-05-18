---
aliases:
  - ECS Systems
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Systems` là nơi ECS thực thi logic bằng cách đọc và ghi component data của các entity phù hợp.

## Key points
- System là lớp xử lý behavior trong ECS, tách khỏi data.
- Theo `Entities 1.4.2`, bạn có thể tạo system bằng `ISystem` hoặc `SystemBase`.
- `ISystem` là unmanaged system và có các callback có thể Burst compile; còn `SystemBase` là managed system.
- Mỗi frame, system chạy trong world và xử lý các entity khớp query hoặc logic của nó.
- Tài liệu `ISystem` cho biết callback của system chạy trên main thread, và best practice là dùng `OnUpdate` để schedule job làm phần lớn công việc.
- Các system được nhóm trong system groups; thứ tự update có thể được điều khiển bởi `UpdateInGroup`, `UpdateBefore`, và `UpdateAfter`.

## Decision rules
- Giữ logic tập trung theo loại xử lý thay vì rải behavior vào từng object.
- Phù hợp với batch processing và job scheduling.
- Dễ tối ưu hơn vì query và update order rõ ràng.
- Khi bạn có rule gameplay hoặc simulation cần áp dụng cho nhiều entity có cùng data shape.
- Khi logic nên chạy theo phase hoặc group trong frame.
- Tránh cố nhồi state local phức tạp vào system nếu dữ liệu đó thực ra nên là component.
- Không cần ECS system cho những logic editor-only hoặc những flow nhỏ đang hoạt động tốt với `MonoBehaviour`.
- Update order giữa nhiều system có thể khó theo dõi nếu group và attribute bị dùng thiếu kỷ luật.
- Tất cả callback của system vẫn bắt đầu từ main thread; muốn tận dụng đa luồng thì bạn phải schedule job đúng cách.

## Example
```csharp
using Unity.Entities;
using Unity.Transforms;
using Unity.Mathematics;
public struct Speed : IComponentData
{
    public float Value;
}
public partial struct MoveSystem : ISystem
{
    public void OnUpdate(ref SystemState state)
    {
        float dt = SystemAPI.Time.DeltaTime;
        foreach (var (transform, speed) in
            SystemAPI.Query<RefRW<LocalTransform>, RefRO<Speed>>())
        {
            transform.ValueRW.Position += new float3(0f, 0f, speed.ValueRO.Value * dt);
        }
    }
}
```

## Related notes
- [[Definition]]
- [[Components]]
- [[World]]
- [[Burst Compiler]]
- [[Job System]]
- [[Summary]]
