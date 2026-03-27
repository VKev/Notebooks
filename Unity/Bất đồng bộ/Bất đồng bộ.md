---
note_type: structure
tags:
  - unity
---

# Bất đồng bộ

## Scope
- Nhóm note này chứa các concept về asynchronous flow trong Unity.
- Father note này đóng vai trò `structure note` cho toàn bộ folder.

## Notes
- [[Definition]]: mental model tổng quát của async trong Unity và các execution layers liên quan.
- [[Thread]]: thread là execution lane thật sự được OS schedule, không phải chỉ là `Task`.
- [[Main Thread]]: thread scripting chính của Unity và quy tắc main-thread-only của engine API.
- [[ThreadPool]]: pool các CLR background worker mà `Task` và `Awaitable.BackgroundThreadAsync()` dùng.
- [[Task]]: abstraction của .NET cho async work và continuation, không phải thread.
- [[UnitySynchronizationContext]]: cách Unity đưa normal `Task` continuation về `main thread`.
- [[Coroutine]]: tạm dừng và tiếp tục logic theo frame, thời gian, hoặc điều kiện.
- [[Async Await|Async/Await]]: model `Task` của C# với `async`, `await`, `Task<T>`, và các pitfall chính.
- [[Awaitable]]: bridge của Unity để dùng cú pháp `await` với frame, time, và `AsyncOperation`.
- [[Comparison]]: cách chọn giữa `Coroutine`, `Task`, `Awaitable`, background thread work, và các mô hình liên quan.

## Related structure notes
- [[Unity]]: quy luật note cho Unity field.
- [[DOTS ECS]]: topic data-oriented về Entities, baking, và Job System.
