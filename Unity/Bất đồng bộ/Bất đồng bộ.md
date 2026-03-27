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
- [[Definition]]: khái niệm chung của bất đồng bộ và mental model "wait without blocking".
- [[Coroutine]]: tạm dừng và tiếp tục logic theo frame, thời gian, hoặc điều kiện.
- [[Async Await|Async/Await]]: model `Task` của C# với `async`, `await`, `Task<T>`, và các pitfall chính.
- [[Comparison]]: cách chọn đúng model theo frame-based hay task-based waiting.
- [[Awaitable]]: bridge của Unity để dùng cú pháp `await` với frame, time, và `AsyncOperation`.

## Related structure notes
- [[Unity]]: quy luật note cho Unity field.
