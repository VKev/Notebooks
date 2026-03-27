---
aliases:
  - Unity Sync Context
  - SynchronizationContext trong Unity
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `UnitySynchronizationContext` là custom `SynchronizationContext` của Unity, dùng để đưa continuation của .NET `Task` về `main thread` theo mặc định khi async flow bắt đầu từ `main thread`.

## Problem
- Phần lớn Unity API không thread-safe.
- Nếu `Task` continuation resume ở thread bất kỳ, async code bình thường sẽ rất dễ đụng sai thread khi quay lại gọi Unity API.

## Core idea
- Unity thay `SynchronizationContext` mặc định bằng context riêng của nó để giữ async continuation của `Task` thân thiện với `main thread`.
- Đây là một trong những lý do vì sao `Task` trong Unity không hành xử giống hệt console app hay server app.

## Mechanism
- Nếu bạn gọi `Task`-returning API từ `main thread`:
  - `await` capture `UnitySynchronizationContext`
  - task hoàn tất
  - continuation được post về Unity context
  - code resume ở `Update` tick kế tiếp trên `main thread`
- Nếu bạn gọi từ background thread, continuation có thể hoàn tất trên `ThreadPool`.
- `SynchronizationContext` quyết định continuation resume ở đâu; nó không phải cùng thứ với OS scheduler hay Unity Job System scheduler.

## When to use
- Dùng như mental model khi debug async behavior trong Unity.
- Hữu ích khi bạn cần hiểu vì sao `await SomeTask()` quay lại `main thread`, hoặc vì sao lại có thêm một frame latency.

## Benefits
- Giúp normal `Task` flow an toàn hơn với Unity API.
- Giảm số lần bạn phải tự marshal về `main thread` trong flow bắt đầu từ `main thread`.

## Trade-offs / Limits
- Capture context có overhead.
- Resume ở `next Update` tạo thêm latency so với `Awaitable`.
- Nếu bạn block `main thread` bằng `.Wait()` hoặc `.Result`, continuation có thể không có cơ hội chạy.

## Common mistakes
- Nhầm `SynchronizationContext` với `TaskScheduler`.
- Nghĩ rằng continuation luôn quay về `main thread` bất kể flow khởi đầu ở đâu.
- Quên rằng `Task` trong Unity có thể thêm một frame latency chỉ để quay lại `main thread`.

## Example
- Mental flow của `await SomeTask()` từ `main thread`:

```csharp
// main thread
await SomeTaskReturningMethod();
// next Update tick on main thread
UseUnityApi();
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Task]]
- [[ThreadPool]]
- [[Main Thread]]
- [[Awaitable]]
- [[Comparison]]
