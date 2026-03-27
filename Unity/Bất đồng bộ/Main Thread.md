---
aliases:
  - Unity Main Thread
  - Main thread trong Unity
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Main thread` là thread scripting chính của Unity, nơi phần lớn gameplay code và hầu hết `UnityEngine` API phải chạy để đảm bảo đúng hành vi.

## Problem
- Trong Unity, lỗi thread-safety thường không phải lỗi cú pháp mà là lỗi correctness.
- Nếu bạn gọi nhầm API main-thread-only từ background thread, dev build có thể báo lỗi, còn build thường có thể crash hoặc hành vi khó đoán.

## Core idea
- Với góc nhìn scripting, Unity là core single-threaded.
- Điều này không có nghĩa Unity chỉ có một thread, mà có nghĩa là hầu hết API bạn dùng hằng ngày chỉ an toàn trên `main thread`.

## Mechanism
- `GameObject`, `Transform`, `Component`, scene API, và đa số `UnityEngine` API nên được dùng trên `main thread`.
- Unity còn có vài tình huống đặc biệt mà code của bạn có thể bị gọi ngoài `main thread`:
  - constructors và field initializers trong lúc load scene có thể chạy trên loading thread
  - `OnValidate` có thể bị gọi từ thread khác ngoài `main thread`
  - `Application.logMessageReceivedThreaded` có thể được gọi từ nhiều thread và song song
- `UnitySynchronizationContext` tồn tại để đưa continuation của `Task` về `main thread` theo mặc định khi bạn bắt đầu từ `main thread`.

## When to use
- Bất cứ khi nào bạn đụng tới Unity scene object, asset, `Transform`, `Component`, hoặc engine state.
- Bất cứ khi nào bạn cần chắc chắn code resume ở môi trường Unity-safe trước khi gọi engine API.

## Benefits
- Giữ được correctness với Unity API.
- Giảm rủi ro race condition và crash khó debug.

## Trade-offs / Limits
- `Main thread` là nơi dễ nghẽn frame nhất.
- CPU-heavy work đặt ở đây vẫn gây giật hình dù có dùng `await` hay coroutine.

## Common mistakes
- Nghĩ rằng mọi callback của Unity đều luôn chạy trên `main thread`.
- Dùng `.Wait()` hoặc `.Result` trên `main thread`.
- Resume từ background thread rồi đụng thẳng vào `transform` hoặc `SceneManager`.

## Example
- Nếu bạn vừa xử lý nặng ở background và cần quay về Unity API:

```csharp
await Awaitable.MainThreadAsync();
transform.position = targetPosition;
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Thread]]
- [[ThreadPool]]
- [[Task]]
- [[UnitySynchronizationContext]]
- [[Awaitable]]
- [[Job System]]
- [[Comparison]]
