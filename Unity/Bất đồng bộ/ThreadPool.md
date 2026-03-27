---
aliases:
  - CLR ThreadPool
  - .NET ThreadPool
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `ThreadPool` là pool các `background thread` được .NET runtime tái sử dụng để chạy work item và phần lớn `Task` mặc định; trong Unity, `Awaitable.BackgroundThreadAsync()` cũng chuyển sang `ThreadPool`.

## Problem
- Tạo raw thread liên tục là tốn kém và khó scale.
- Bạn cần một nơi chạy background work mà không phải tự tạo, tự hủy, và tự quản lý quá nhiều thread.

## Core idea
- Thay vì tạo thread mới cho từng việc nhỏ, runtime giữ sẵn worker threads và tái sử dụng chúng.
- Đây là thế giới của CLR scheduler, không phải Unity Job System.

## Mechanism
- Theo .NET docs:
  - có một `ThreadPool` cho mỗi process
  - `ThreadPool` thread là `background thread`
  - `Task` mặc định thường schedule lên `ThreadPool`
- Unity 6.3 docs ghi rõ `Awaitable.BackgroundThreadAsync()` resume trên `ThreadPool background thread`.
- Vì thread được tái sử dụng, thread-local state hoặc dữ liệu đánh dấu kiểu `ThreadStatic` có thể còn sót lại từ lần dùng trước.

## When to use
- Hợp cho pure C# background work hoặc các flow `Task` không đụng Unity API.
- Hợp khi bạn cần offload việc nặng nhưng không phù hợp với Job System data-oriented.

## Benefits
- Tránh chi phí tạo raw thread liên tục.
- Tích hợp tự nhiên với `Task` và `Task.Run(...)`.

## Trade-offs / Limits
- Không được xem `ThreadPool` worker như Unity worker thread.
- Không nên đụng `UnityEngine` API trực tiếp trên đó.
- Ít kiểm soát hơn raw `Thread`.

## Common mistakes
- Nghĩ rằng `ThreadPool` và Job System là cùng một loại worker.
- Dựa vào thread-local state như thể thread luôn mới tinh.
- Quên switch về `main thread` trước khi gọi Unity API.

## Example
- Chuyển method hiện tại sang `ThreadPool` thread trong Unity:

```csharp
await Awaitable.BackgroundThreadAsync();
// pure C# work here
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Thread]]
- [[Main Thread]]
- [[Task]]
- [[UnitySynchronizationContext]]
- [[Awaitable]]
- [[Job System]]
- [[Comparison]]
