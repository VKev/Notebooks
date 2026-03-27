---
aliases:
  - Unity Job System
  - Jobs
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- Unity `Job System` là hệ thống multithreading native của Unity để chạy các job data-oriented trên worker threads với dependency graph và safety system.

## Problem
- Raw thread hoặc `Task.Run(...)` cho compute work trong game dễ dẫn tới race condition, cache kém, và đụng Unity API sai thread.
- Bạn cần một mô hình an toàn hơn để song song hóa compute ngắn, đặc biệt khi dữ liệu có thể tách rõ.

## Core idea
- Hãy xem Job System là compute scheduler của Unity, không phải I/O async model.
- Bạn tạo job struct, schedule nó từ `main thread`, để Unity chạy trên worker threads, rồi `Complete()` khi thật sự cần kết quả.

## Mechanism
- Chỉ `main thread` được schedule và complete job.
- Job chạy qua worker threads của Unity và thường nên hoàn tất trong dưới một frame.
- `JobHandle` biểu diễn dependency; job phụ thuộc chỉ chạy khi dependency xong.
- `NativeContainer` như `NativeArray` cho phép chia sẻ native memory an toàn hơn giữa job và `main thread`.
- Parallel jobs chia dữ liệu thành batch và Unity dùng work stealing để cân bằng tải giữa worker threads.

## When to use
- Hợp cho CPU-heavy, short-lived, data-oriented compute.
- Hợp nhất khi đi cùng `Burst`.
- Không phải lựa chọn đầu tiên cho web request, file I/O, hoặc async flow chờ sự kiện bên ngoài.

## Benefits
- Tận dụng nhiều core CPU tốt hơn cho compute.
- Có dependency model và safety system rõ ràng hơn raw threads.
- Phù hợp với data-oriented architecture.

## Trade-offs / Limits
- Không hợp với managed object graph hoặc code đụng `UnityEngine` object trực tiếp.
- Dữ liệu phải thân thiện với blittable/native container.
- Job quá dài hoặc `Complete()` quá sớm sẽ phá parallelism và gây stall `main thread`.

## Common mistakes
- Schedule job dài rồi gọi `Complete()` ngay sau đó.
- Dùng Job System như thể nó là `Task` cho I/O.
- Truy cập data không an toàn hoặc tránh safety system bằng static mutable state.

## Example
- Flow cơ bản của một job:

```csharp
JobHandle handle = myJob.Schedule();
// do other main-thread work here
handle.Complete();
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Thread]]
- [[Main Thread]]
- [[ThreadPool]]
- [[Awaitable]]
- [[Comparison]]
