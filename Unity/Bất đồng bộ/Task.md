---
aliases:
  - Dotnet Task
  - C# Task
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Task` là abstraction của .NET đại diện cho asynchronous work và continuation; nó không phải là một thread.

## Problem
- Nếu không có `Task`, async code dễ bị vỡ thành callback chồng nhau hoặc state machine thủ công.
- Trong Unity, nhiều người hiểu sai rằng `await` tự tạo multithreading nên đặt kỳ vọng sai cho performance và thread safety.

## Core idea
- Khi gọi một `async` method, code chạy ngay trên thread hiện tại cho tới khi gặp `await` chưa hoàn tất.
- Lúc đó method suspend, trả control về caller, và phần còn lại sẽ chạy như continuation khi awaited operation hoàn tất.

## Mechanism
- `Task` và `Task<T>` là handle cho công việc đang diễn ra.
- `await` cài continuation vào `Task`.
- Nếu tại lúc suspend có `SynchronizationContext`, continuation thường resume qua context đó; nếu không có thì dựa nhiều hơn vào scheduler và `ThreadPool`.
- Trong Unity main-thread code, điều này thường có nghĩa là continuation quay về `UnitySynchronizationContext`.

## When to use
- Hợp cho `HTTP`, `file I/O`, `database`, và composition kiểu `WhenAll` hoặc `WhenAny`.
- Hợp khi bạn cần `Task<T>`, `try/catch`, và `CancellationToken`.
- Không đủ để offload CPU-heavy work nếu bạn không thật sự chuyển nó khỏi `main thread`.

## Benefits
- Flow async gọn và composable.
- Dễ trả về kết quả.
- Error handling và cancellation tự nhiên hơn coroutine.

## Trade-offs / Limits
- `Task` không tự đồng bộ với `game loop`.
- `await` không tự tạo thread mới.
- Trong Unity, nếu bắt đầu từ `main thread`, continuation của `Task` có thể quay về `main thread` ở `Update` tick kế tiếp, nên có thêm latency.

## Common mistakes
- Nghĩ rằng `async/await` tự động là multithreaded.
- Dùng `.Wait()` hoặc `.Result` trên `main thread`.
- Nghĩ rằng `await` tự nó sẽ làm CPU-heavy code nhẹ đi.

## Example
- `Task` rất hợp cho I/O-bound async:

```csharp
using System.Net.Http;

var client = new HttpClient();
string json = await client.GetStringAsync(url);
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Async Await]]
- [[Thread]]
- [[ThreadPool]]
- [[Main Thread]]
- [[UnitySynchronizationContext]]
- [[Awaitable]]
- [[Comparison]]
