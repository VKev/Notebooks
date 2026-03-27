---
aliases:
  - Unity Awaitable
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Awaitable` là kiểu async gốc của Unity dùng được với `await` và thường phù hợp hơn `Task` cho nhiều luồng bất đồng bộ trong Unity.

## What is it
- `Awaitable` là custom type của Unity có thể `await` và cũng có thể làm kiểu trả về cho method `async`.
- Trong Unity `6.3`, nhiều API async của Unity hỗ trợ model này, gồm frame-based await, chuyển thread, và các kiểu kế thừa `AsyncOperation`.

## How it works
- `Awaitable` tích hợp với cú pháp `async/await` của C# nhưng được thiết kế để hiểu main thread và update loop của Unity.
- Unity cung cấp sẵn các static method như `NextFrameAsync`, `WaitForSecondsAsync`, `FixedUpdateAsync`, `EndOfFrameAsync`, `MainThreadAsync`, `BackgroundThreadAsync`, và `FromAsyncOperation`.
- Theo tài liệu Unity `6.3`, `Awaitable` được pool để giảm allocation, nên cùng một instance không an toàn nếu `await` nhiều lần.
- Continuation của `Awaitable` thường resume ngay theo context hiện tại thay vì luôn chờ sang `Update` tick kế tiếp như `Task`.

## Why use it
- Giảm allocation so với nhiều trường hợp dùng `.NET Task`.
- Có built-in support cho main thread, background thread, `Update`, `FixedUpdate`, và các Unity async operation.
- Hợp khi bạn muốn viết flow async kiểu hiện đại nhưng vẫn bám sát cách Unity chạy frame và thread.

## When to use it
- Dùng khi bạn cần chờ frame tiếp theo, đợi một khoảng thời gian, hoặc chờ Unity operation hoàn thành bằng cú pháp `await`.
- Dùng khi bạn muốn chuyển sang background thread rồi quay lại main thread theo API chính thức của Unity.
- Dùng khi bạn viết Unity-native async flow và không cần nhiều consumer cùng `await` một instance.

## When to not use it
- Không nên dùng khi bạn cần `await` cùng một async result nhiều lần hoặc từ nhiều consumer khác nhau.
- Không phải lựa chọn tối ưu nếu code của bạn chủ yếu xoay quanh API đã trả về `Task` và cần ecosystem `.NET Task` đầy đủ như `WhenAll`.
- Không nên xem `Awaitable` như công cụ thay thế Job System cho compute ngắn và song song CPU.

## Limitations
- `Awaitable` instance được pool nên không an toàn để `await` nhiều lần trên cùng một instance.
- Không có built-in `WaitAll` hoặc `WaitAny` như `Task`.
- Nếu lạm dụng `NextFrameAsync` trên rất nhiều object chạy đồng thời, hiệu năng vẫn có thể thành vấn đề.

---

## Example code
```csharp
using UnityEngine;

public class AwaitableExample : MonoBehaviour
{
    private async Awaitable Start()
    {
        await Awaitable.NextFrameAsync();
        await Awaitable.WaitForSecondsAsync(1f);
        Debug.Log("Done");
    }
}
```

---

## Related notes
- [[Definition]]
- [[AsyncAwait]]
- [[AsyncOperation]]
- [[AwaitableCompletionSource]]
- [[Summary]]
