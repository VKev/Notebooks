---
aliases:
  - Unity Awaitable
  - Awaitable trong Unity
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Awaitable` là primitive bất đồng bộ theo kiểu Unity cho phép bạn dùng cú pháp `await` với `frame wait`, `time wait`, `AsyncOperation`, và chuyển đổi giữa `main thread` với `background thread`.

## Problem
- Coroutine hợp cho flow theo frame nhưng không mạnh bằng `Task` trong việc compose, return value, hoặc tích hợp với model async hiện đại.
- `Task` thuần của C# lại không diễn tả trực tiếp các khái niệm rất Unity-specific như `next frame`, `end of frame`, hay chuyển về `main thread` một cách rõ ràng.

## Core idea
- `Awaitable` là cây cầu giữa cú pháp `async/await` và game loop của Unity.
- Nó giữ cảm giác "viết code như tuần tự" nhưng vẫn nói được những điểm chờ quen thuộc của Unity.

## Mechanism
- Method có thể trả về `Awaitable`.
- Unity cung cấp một số điểm chờ trực tiếp:
  - `Awaitable.NextFrameAsync()`
  - `Awaitable.WaitForSecondsAsync(...)`
  - `Awaitable.EndOfFrameAsync()`
  - `await` trên `AsyncOperation`
  - `Awaitable.BackgroundThreadAsync()`
  - `Awaitable.MainThreadAsync()`
- Với Unity API trả về `Awaitable`, continuation scheduling mặc định phụ thuộc thread gọi method:
  - gọi từ `main thread` thì resume ở `main thread`
  - gọi từ `background thread` thì resume ở `ThreadPool` thread
- `Awaitable.MainThreadAsync()` và `Awaitable.BackgroundThreadAsync()` chỉ ảnh hưởng rõ ràng tới method hiện tại, không phải một "global mode switch".
- Pattern quan trọng là chỉ dùng `Unity API` khi bạn đang ở `main thread`.

## When to use
- Hợp khi bạn muốn syntax `async/await` nhưng flow vẫn gắn chặt với `game loop`.
- Hợp cho `scene loading`, `wait next frame`, `wait seconds`, `UI flow`, hoặc các coroutine đơn giản muốn viết sạch hơn.
- Hợp khi bạn cần chuyển sang `background thread` để xử lý nặng rồi chuyển về `main thread`.

## Benefits
- Cú pháp gọn hơn nhiều coroutine dài.
- Diễn tả rõ các điểm chờ kiểu Unity mà `Task` thuần không nói lên được.
- Giúp async code thân thiện hơn với workflow của Unity.
- So với `Task`, nó thường tránh được chi phí capture `SynchronizationContext` và tránh phải chờ thêm một `Update` tick chỉ để continuation resume trên `main thread`.

## Trade-offs / Limits
- `Awaitable` không xóa bỏ các ràng buộc lifecycle của Unity.
- Vẫn phải tự quản lý chuyện hủy flow, object bị destroy, và thời điểm resume.
- `Awaitable` instance được pool để giảm allocation, nên không an toàn nếu `await` nhiều lần trên cùng một instance.
- Nếu bạn cần `WaitAll`, `WaitAny`, hoặc nhiều consumer cùng chờ một kết quả, `Task` vẫn tự nhiên hơn.
- Nếu project không dùng phiên bản Unity có hỗ trợ `Awaitable`, pattern này sẽ không khả dụng.

## Common mistakes
- Chuyển sang `background thread` rồi vẫn gọi `transform`, `gameObject`, hoặc API Unity khác.
- Nghĩ rằng `Awaitable` thay thế mọi `Task` và mọi `Coroutine`.
- Lưu một `Awaitable` vào biến rồi `await` nó hơn một lần.
- Mix `Awaitable` với nhiều model khác mà không rõ ownership của flow.

## Example
- Ví dụ một flow Unity-friendly dùng `Awaitable`.

```csharp
using UnityEngine;

public class AwaitableExample : MonoBehaviour
{
    public async Awaitable Test()
    {
        Debug.Log("Start");

        await Awaitable.WaitForSecondsAsync(2f);

        Debug.Log("Done");
    }
}
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Coroutine]]
- [[Async Await]]
- [[Comparison]]
