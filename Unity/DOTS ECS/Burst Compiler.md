---
aliases:
  - Unity Burst
  - Burst
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Burst Compiler` là compiler của Unity dùng để biên dịch phần code tương thích từ IL/.NET sang native code tối ưu cao, đi cùng `Job System` và ECS để tăng hiệu năng CPU.

## Key points
- Theo Unity `6.3`, `Burst` là compiler dịch từ IL/.NET bytecode sang highly optimized native code bằng `LLVM`.
- Theo tài liệu package `com.unity.burst@1.8`, Burst chủ yếu được thiết kế để đi cùng `Job System`.
- Trong workflow DOTS, Burst là lớp tối ưu cuối cùng sau khi bạn đã có data layout phù hợp và job song song hóa hợp lý.
- Bạn đánh dấu job, system callback, hoặc function phù hợp bằng `[BurstCompile]`.
- Trong Editor Play Mode, Burst compile theo kiểu `JIT`; trong Player build, Burst compile theo kiểu `AOT`.
- Theo tài liệu Burst `1.8`, ở Play Mode Burst mặc định compile bất đồng bộ lần đầu được dùng; trong lúc đó code có thể tạm chạy bằng managed path cho tới khi Burst compile xong.

## Decision rules
- Tăng tốc CPU-bound code mạnh hơn so với chỉ schedule job mà không có Burst.
- Tận dụng tốt dữ liệu kiểu `NativeArray`, `Unity.Mathematics`, và flow xử lý theo batch của ECS.
- Là mảnh ghép rất quan trọng để DOTS phát huy đúng lợi thế data-oriented trên số lượng entity lớn.
- Dùng cho job hoặc system callback có khối lượng tính toán lớn và chạy lặp lại nhiều lần.
- Khi bạn đã có data-oriented flow tương thích với ECS hoặc `Job System`.
- Khi code của bạn nằm trong phần hot path CPU như movement, simulation, culling logic, hoặc transform math thuần dữ liệu.
- Không dùng khi code phụ thuộc nặng vào managed object, nhiều API main-thread-only của Unity, hoặc các phần không tương thích với HPC#.
- Tránh kỳ vọng Burst tự cứu performance nếu data layout, query pattern, hoặc workload decomposition còn kém.
- Không dùng Burst như giải pháp cho `I/O`, sequencing, hay flow chờ kiểu `Coroutine` hoặc `async/await`.
- Burst chỉ hỗ trợ một high-performance subset của C# (`HPC#`), nên không phải mọi C# code đều Burst compile được.
- Compile lần đầu trong Editor có thể gây khác biệt timing nếu bạn đang profile mà chưa warmup.
- Một số tùy chọn như `FloatMode.Fast` hoặc giảm safety checks có thể đổi trade-off giữa tốc độ, khả năng debug, và độ chặt chẽ của kiểm tra.

## Example
```csharp
using Unity.Burst;
using Unity.Entities;
using Unity.Mathematics;
using Unity.Transforms;
public struct Velocity : IComponentData
{
    public float3 Value;
}
[BurstCompile]
public partial struct MoveBurstJob : IJobEntity
{
    public float DeltaTime;
    private void Execute(ref LocalTransform transform, in Velocity velocity)
    {
        transform.Position += velocity.Value * DeltaTime;
    }
}
[BurstCompile]
public partial struct MoveBurstSystem : ISystem
{
    [BurstCompile]
    public void OnUpdate(ref SystemState state)
    {
        new MoveBurstJob
        {
            DeltaTime = SystemAPI.Time.DeltaTime
        }.ScheduleParallel();
    }
}
```

## Related notes
- [[Definition]]
- [[Systems]]
- [[Job System]]
- [[Summary]]
