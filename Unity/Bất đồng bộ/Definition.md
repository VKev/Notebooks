---
aliases:
  - Definition
  - Bất đồng bộ
  - Asynchronous
  - Asynchronous Programming
note_type: permanent
created: 2026-03-27
tags:
  - unity
color: var(--mk-color-base-40)
---

## One-line answer
- Trong Unity, bất đồng bộ là cách tổ chức code để chờ hoặc offload công việc mà không chặn `main thread`, nhưng nó không chỉ có một lớp thực thi mà đi qua nhiều execution layers khác nhau.

## Problem
- Nhiều việc trong game không cần CPU chạy liên tục, mà chủ yếu là chờ: chờ `network response`, chờ `file I/O`, chờ `scene loading`, hoặc chờ một mốc thời gian.
- Nếu code chờ theo kiểu đồng bộ và chặn `main thread`, game sẽ đứng hình, UI không phản hồi, và trải nghiệm bị hỏng.
- Nhiều lỗi thiết kế xảy ra vì người ta dùng `thread`, `task`, `await`, `job`, `scheduler`, và `coroutine` như thể là cùng một thứ.

## Core idea
- Ý chính của bất đồng bộ là tách `waiting` ra khỏi `blocking`.
- Công việc có thể bắt đầu ở hiện tại, nhường control lại cho engine, rồi tiếp tục khi điều kiện phù hợp đã xảy ra.
- Trong Unity, câu hỏi đúng không chỉ là "có async hay không", mà là:
  - cái gì đang được schedule
  - scheduler nào đang điều phối nó
  - nó đang chạy trên thread nào
  - continuation sẽ quay về đâu

## Mechanism
- Unity async thực tế đi qua nhiều lớp:
  - OS scheduler: cấp CPU time slice cho `thread`
  - CLR layer: `ThreadPool`, `Task`, và scheduler của .NET
  - Unity main-thread layer: `UnitySynchronizationContext` đưa normal `Task` continuation về `main thread`
  - Unity async layer: `Awaitable` cho frame-aware async với allocation thấp hơn
  - Unity compute layer: `Job System` cho multithreaded data-oriented work
- Cũng cần nhớ rằng `Coroutine` là frame-based sequencing trên `main thread`, không phải multithreading.

## When to use
- Dùng note này như mental map khi bạn cần chọn giữa `Coroutine`, `Task`, `Awaitable`, `Task.Run`, raw `Thread`, hoặc `Job System`.
- Hợp khi bạn đang thiết kế async flow hoặc debug chuyện code resume sai thread.

## Benefits
- Giữ game responsive trong lúc chờ.
- Flow thường rõ hơn callback hoặc state thủ công.
- Dễ tách phần "đợi cái gì" khỏi phần "làm gì tiếp theo".
- Giúp chọn đúng tool cho đúng loại waiting hoặc compute.

## Trade-offs / Limits
- Bất đồng bộ không tự động tạo song song CPU thật.
- Nếu đoạn code nặng vẫn chạy trên `main thread`, frame vẫn có thể bị khựng.
- Khi có nhiều model bất đồng bộ cùng tồn tại, lifecycle và điểm resume có thể khó theo dõi hơn.

## Common mistakes
- Nghĩ rằng "async" luôn có nghĩa là chạy ở `background thread`.
- Dùng `Task`, `Coroutine`, `Awaitable`, và `Job System` như thể chúng thay thế hoàn toàn cho nhau.
- Không hỏi continuation sẽ quay về đâu trước khi gọi Unity API.

## Example
- Cùng là "không block", nhưng mỗi model đi qua execution layer khác nhau.

```csharp
yield return null;                           // Player loop / main thread
await SomeTask();                            // Task + continuation model
await Awaitable.BackgroundThreadAsync();     // switch sang ThreadPool
jobHandle = job.Schedule();                  // Unity Job System
```

## Related notes
- [[Bất đồng bộ]]
- [[Thread]]
- [[Main Thread]]
- [[ThreadPool]]
- [[Task]]
- [[UnitySynchronizationContext]]
- [[Coroutine]]
- [[Async Await]]
- [[Awaitable]]
- [[Job System]]
- [[Comparison]]
