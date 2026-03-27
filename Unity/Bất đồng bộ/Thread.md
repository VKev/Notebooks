---
aliases:
  - Luồng
  - System Thread
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Thread` là execution lane thật sự được OS schedule lên CPU; trong .NET, `Thread` là managed wrapper cho lane đó, chứ không phải chỉ là một `Task` hay một coroutine.

## Problem
- Nhiều người dùng lẫn lộn `thread`, `task`, `await`, `job`, và `coroutine` như cùng một thứ.
- Nếu không tách rõ khái niệm này, bạn dễ chọn sai công cụ và đụng lỗi thread-safety trong Unity.

## Core idea
- `Thread` là nơi code thực sự chạy.
- Scheduler của hệ điều hành mới là thứ cấp CPU time slice cho thread.
- Các abstraction khác như `Task`, `Awaitable`, hay `Job` đều phải cuối cùng chạy ở đâu đó: `main thread`, `ThreadPool` thread, hoặc Unity worker thread.

## Mechanism
- OS quyết định thread nào được chạy dựa trên priority và thuật toán scheduling của nó.
- Trong .NET:
  - `main thread` và thread tạo bằng `new Thread(...)` là `foreground thread` theo mặc định
  - `ThreadPool` thread là `background thread`
- `background thread` không giữ process sống nếu tất cả `foreground thread` đã kết thúc.

## When to use
- Dùng khi bạn thật sự cần thread-level control mà `Task`, `Awaitable`, hoặc Job System không giải quyết tốt.
- Trong Unity gameplay code bình thường, raw `Thread` thường không phải lựa chọn đầu tiên.

## Benefits
- Có real concurrent CPU execution.
- Cho bạn quyền kiểm soát trực tiếp hơn với lifecycle và cấu hình thread.

## Trade-offs / Limits
- Khó quản lý hơn `Task` và khó an toàn hơn Job System.
- Phần lớn `UnityEngine` API không thread-safe, nên thread riêng không được đụng tùy tiện.
- Tạo quá nhiều raw thread có thể tốn chi phí scheduling và context switching.

## Common mistakes
- Nghĩ rằng `Task` hay coroutine tự nó là thread.
- Chạm vào `GameObject`, `Transform`, hoặc scene API từ background thread.
- Dùng raw thread cho bài toán mà Job System hoặc `Task` đã đủ tốt.

## Example
- Ví dụ raw thread trong C#:

```csharp
using System.Threading;

var worker = new Thread(() =>
{
    // pure C# work only
});

worker.Start();
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Main Thread]]
- [[ThreadPool]]
- [[Task]]
- [[Job System]]
- [[Comparison]]
