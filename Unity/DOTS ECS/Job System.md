---
aliases:
  - Unity Job System
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Job System` cho phép Unity chạy code đa luồng an toàn hơn để tận dụng nhiều CPU core, và là mảnh ghép quan trọng khi ECS cần scale.

## Key points
- Theo Unity Manual `6.4`, job system cho phép bạn viết multithreaded code để dùng các CPU core hiệu quả hơn.
- Job system có thể dùng độc lập, nhưng Unity khuyến nghị kết hợp với `Burst` để đạt hiệu năng tốt hơn.
- Tài liệu cũng nêu rõ bạn có thể dùng job system cùng ECS để tạo data-oriented code hiệu năng cao.
- Unity dùng worker threads để thực thi job song song với main thread.
- Job system dùng work stealing để cân bằng tải giữa các worker threads.
- Hệ thống safety giúp phát hiện race condition tiềm năng.
- Dữ liệu gửi vào job phải là blittable hoặc các kiểu thread-safe phù hợp, vì Unity copy data sang native memory khi schedule job.

## Decision rules
- Khi system ECS cần xử lý số lượng lớn entity và phần việc có thể chia song song.
- Khi bạn muốn schedule `IJobEntity`, `IJobChunk`, hoặc job song song khác từ `OnUpdate`.
- Tránh dùng cho logic cực ngắn hoặc quá phụ thuộc main thread API của Unity.
- Không phải lựa chọn thay thế cho `async/await` hay `Coroutine` trong các bài toán chờ `I/O`, frame timing, hoặc event flow.
- Nếu bài toán không đủ lớn hoặc không đủ song song, chi phí schedule có thể không đáng.
- Tăng throughput CPU cho các tác vụ có thể song song hóa.
- Giảm áp lực cho main thread khi xử lý lượng lớn entity.
- Phối hợp tốt với ECS query và data layout theo chunk.

## Example
```csharp
using Unity.Burst;
using Unity.Entities;
using Unity.Transforms;
using Unity.Mathematics;
public struct Speed : IComponentData
{
    public float Value;
}
[BurstCompile]
public partial struct MoveJob : IJobEntity
{
    public float DeltaTime;
    private void Execute(ref LocalTransform transform, in Speed speed)
    {
        transform.Position += new float3(0f, 0f, speed.Value * DeltaTime);
    }
}
[BurstCompile]
public partial struct MoveJobSystem : ISystem
{
    [BurstCompile]
    public void OnUpdate(ref SystemState state)
    {
        new MoveJob
        {
            DeltaTime = SystemAPI.Time.DeltaTime
        }.ScheduleParallel();
    }
}
```

## Related notes
- [[Definition]]
- [[Burst Compiler]]
- [[Systems]]
- [[Archetypes and Chunks]]
- [[Summary]]
