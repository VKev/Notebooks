---
aliases:
  - Comparison
  - Coroutine vs Async/Await
  - Coroutine vs Async Await
  - Coroutine vs Task vs Awaitable vs Jobs
  - Khi nào dùng Coroutine hay Async
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Coroutine`, `Task`, `Awaitable`, `ThreadPool` work, và `Job System` là các execution model khác nhau; muốn chọn đúng thì phải hỏi cái gì đang được schedule, chạy ở đâu, và resume về đâu.

## Problem
- Nhiều API đều trông như "làm việc sau" hoặc "đợi rồi làm tiếp", nên rất dễ bị gom chung thành một nhóm mơ hồ gọi là async.
- Nếu chọn sai model, bạn sẽ gặp `main thread` stall, extra latency, race condition, hoặc code khó maintain.

## Core idea
- Hãy phân loại theo loại scheduling:
  - `Coroutine`: sequencing theo `frame/time` trên `main thread`
  - `Task`: async continuation model của .NET
  - `Awaitable`: Unity-aware async model
  - `ThreadPool` hoặc `Task.Run`: background execution cho pure C# work
  - `Job System`: multithreaded compute model của Unity
- Chúng không phải tên khác nhau của cùng một cơ chế.

## Mechanism
- So sánh ngắn theo tiêu chí thực tế:
  - `Coroutine`: scheduler là Unity player loop; chạy trên `main thread`; hợp cho frame-timed sequence
  - `Task`: scheduler là CLR async/task world; trong Unity, continuation từ `main thread` thường quay lại `UnitySynchronizationContext` ở `next Update`
  - `Awaitable`: continuation-aware kiểu Unity; main thread gọi thì resume main thread, background gọi thì resume `ThreadPool`; có API switch thread/frame rõ ràng
  - `ThreadPool` hoặc `Task.Run`: code thật sự chạy ở CLR background worker; không được chạm phần lớn Unity API
  - `Job System`: job được schedule từ `main thread`, chạy trên Unity worker threads, đồng bộ bằng `JobHandle` và `NativeContainer`
  - `Return value`: `Task<T>` và `Awaitable<T>` tự nhiên hơn coroutine; jobs trả kết quả qua data container
  - `Multi-await`: `Task` hợp hơn `Awaitable` khi cần nhiều consumer hoặc await nhiều lần

## When to use
- Dùng `Coroutine` khi:
  - flow là `gameplay timing`, `spawn loop`, `cooldown`, `animation sequence`
- Dùng `Task` hoặc `async/await` khi:
  - flow là `HTTP`, `file`, `database`, hoặc task dài không phụ thuộc frame
- Dùng `Awaitable` khi:
  - bạn muốn syntax async nhưng vẫn chờ theo `next frame`, `seconds`, `scene loading`, hoặc cần bridge giữa `background thread` và `main thread`
- Dùng `ThreadPool` hoặc `Task.Run` khi:
  - bạn có pure C# CPU work không phù hợp với Job System
- Dùng `Job System` khi:
  - bạn có short-lived, data-oriented, CPU-heavy compute và muốn scale qua nhiều core

## Benefits
- Chọn đúng model làm code rõ hơn, lifecycle sạch hơn, và bug ít hơn.
- Team dễ nhìn vào abstraction để biết đây là wait-style async, frame sequencing, background work, hay parallel compute.

## Trade-offs / Limits
- Không nên ép toàn bộ project dùng một model duy nhất.
- Mix bừa `Coroutine`, `Task`, `Awaitable`, `ThreadPool`, và `Job System` làm ownership của flow mơ hồ.
- `Task` mạnh về composition nhưng không tự thay thế frame-based flow.
- `Awaitable` Unity-friendly hơn nhưng không phải bản sao một-một của `Task`.
- `Job System` rất mạnh cho compute, nhưng không phải model cho I/O hoặc arbitrary managed object workflow.

## Common mistakes
- Dùng coroutine cho `HTTP` hoặc `file I/O` chỉ vì đang quen tay với Unity.
- Dùng `Task.Run(...)` cho logic có đụng `Unity API`.
- Dùng `.Wait()` hoặc `.Result` trên `main thread`.
- Cố rewrite toàn bộ coroutine sang async mà không xét xem flow đó vốn đang gắn với frame hay không.
- Dùng Job System cho bài toán vốn chỉ là chờ network hoặc chờ file xong.

## Example
- Một mental shortcut hữu ích:

```csharp
// frame-based sequencing
yield return null;

// task-based async
await someTask;

// Unity-friendly async
await Awaitable.NextFrameAsync();

// background C# work
await Task.Run(ComputeSomething);

// data-oriented parallel compute
JobHandle handle = job.Schedule();
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Thread]]
- [[Main Thread]]
- [[ThreadPool]]
- [[Task]]
- [[UnitySynchronizationContext]]
- [[Coroutine]]
- [[Async Await]]
- [[Awaitable]]
- [[Job System]]
