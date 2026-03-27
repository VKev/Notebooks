---
aliases:
  - Comparison
  - Coroutine vs Async/Await
  - Coroutine vs Async Await
  - Khi nào dùng Coroutine hay Async
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Coroutine` là frame-based async của Unity, còn `async/await` là task-based async của C#; chọn đúng công cụ phụ thuộc vào thứ bạn đang chờ và nơi code được phép resume.

## Problem
- Cả hai đều trông giống "đợi rồi làm tiếp", nên rất dễ bị dùng thay thế lẫn nhau một cách máy móc.
- Nếu chọn sai model, code sẽ khó debug, không khớp lifecycle, hoặc đụng phải giới hạn `main thread`.

## Core idea
- Hãy phân loại theo waiting model:
  - `Coroutine`: chờ theo `frame`, `time`, hoặc condition của game loop
  - `async/await`: chờ theo `task completion`
  - `Awaitable`: cầu nối Unity-friendly giữa hai phía
- Đây không phải ba tên gọi khác nhau của cùng một thứ; chúng phục vụ các kiểu flow khác nhau.

## Mechanism
- So sánh ngắn theo tiêu chí thực tế:
  - `Thread`: coroutine chạy trên `main thread`; `Task` trong Unity thường resume lại ở `main thread` nếu được await từ `main thread`, nhưng từ background thì continuation có thể ở `ThreadPool`; `Awaitable` cho phép switch thread rõ hơn theo API Unity
  - `Timing`: coroutine bám theo frame; `async/await` bám theo lúc task xong; `Awaitable` cho phép chờ frame/time bằng cú pháp `await`
  - `Return value`: coroutine không tự nhiên với giá trị trả về; `Task<T>` mạnh hơn nhiều
  - `Cancellation`: coroutine stop được nhưng không thống nhất; `CancellationToken` của task rõ hơn
  - `Error handling`: `try/catch` trong async tự nhiên hơn; coroutine debug lỗi thường kém rõ ràng hơn
  - `Multi-await`: `Task` an toàn hơn khi cần nhiều consumer hoặc await nhiều lần; `Awaitable` không an toàn cho kiểu dùng đó vì object được pool

## When to use
- Dùng `Coroutine` khi:
  - flow là `gameplay timing`, `spawn loop`, `cooldown`, `animation sequence`
- Dùng `async/await` khi:
  - flow là `HTTP`, `file`, `database`, hoặc task dài không phụ thuộc frame
- Dùng `Awaitable` khi:
  - bạn muốn syntax async nhưng vẫn chờ theo `next frame`, `seconds`, `scene loading`, hoặc cần bridge giữa `background thread` và `main thread`

## Benefits
- Chọn đúng model làm code rõ hơn, lifecycle sạch hơn, và bug ít hơn.
- Team cũng dễ đọc được ý định: đây là flow gameplay, flow task, hay flow Unity async bridge.

## Trade-offs / Limits
- Không nên ép toàn bộ project dùng một model duy nhất.
- Mix cả ba bừa bãi làm ownership của flow mơ hồ, nhất là khi object bị disable hoặc destroy.
- `async/await` mạnh hơn về task composition, nhưng không tự thay thế flow frame-based của Unity.
- `Awaitable` có lợi thế Unity-specific, nhưng không phải bản sao một-một của `Task`.

## Common mistakes
- Dùng coroutine cho `HTTP` hoặc `file I/O` chỉ vì đang quen tay với Unity.
- Dùng `Task.Run(...)` cho logic có đụng `Unity API`.
- Cố rewrite toàn bộ coroutine sang async mà không xét xem flow đó vốn đang gắn với frame hay không.

## Example
- Một mental shortcut hữu ích:

```csharp
// frame-based
yield return null;

// task-based
await someTask;

// Unity-friendly async
await Awaitable.NextFrameAsync();
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Coroutine]]
- [[Async Await]]
- [[Awaitable]]
