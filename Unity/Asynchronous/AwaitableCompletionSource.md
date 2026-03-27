---
aliases:
  - Unity AwaitableCompletionSource
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `AwaitableCompletionSource` là công cụ cho phép code của bạn tự điều khiển thời điểm một `Awaitable` hoàn thành.

## What is it
- `AwaitableCompletionSource` là producer side của một `Awaitable`.
- Nó cho bạn lấy ra `Awaitable` để nơi khác `await`, trong khi chính code của bạn quyết định lúc nào `SetResult`, `SetException`, hoặc `SetCanceled`.
- Nó là bản tương đương theo hướng Unity-native của ý tưởng `TaskCompletionSource`.

## How it works
- Tạo một `AwaitableCompletionSource` hoặc `AwaitableCompletionSource<T>`.
- Lấy `Awaitable` từ property `Awaitable`.
- Ở nơi khác, khi sự kiện người dùng hoặc điều kiện nào đó xảy ra, gọi `SetResult`, `SetException`, `SetCanceled`, hoặc các biến thể `TrySet...`.
- Theo tài liệu Unity `6.3`, bạn cũng có thể `Reset` để đưa completion source sang một `Awaitable` mới.

## Why use it
- Giúp bridge giữa event/callback/UI và flow `await`.
- Tránh phải tự viết state machine thủ công khi chờ input người dùng hoặc tín hiệu từ hệ thống khác.
- Là cách chính thức của Unity để tự raise completion cho `Awaitable`.

## When to use it
- Dùng khi bạn có UI prompt, popup xác nhận, hoặc event-based flow nhưng muốn tiêu thụ nó bằng `await`.
- Dùng khi bạn cần một điểm completion do user code kiểm soát thay vì do Unity API trả về sẵn.

## When to not use it
- Không cần dùng nếu Unity đã có sẵn API async phù hợp như `SceneManager.LoadSceneAsync` hoặc `Awaitable.WaitForSecondsAsync`.
- Không nên dùng để che giấu một flow quá phức tạp nếu bản thân source event và lifecycle chưa rõ ràng.

## Limitations
- Bạn phải tự chịu trách nhiệm lifecycle và bảo đảm completion được raise đúng lúc.
- Nếu quên `SetResult` hoặc `SetCanceled`, nơi `await` có thể chờ mãi.
- Nếu object phát completion bị destroy hoặc state đổi bất ngờ, flow có thể bị kẹt nếu không thiết kế cancel path.

---

## Example code
```csharp
using UnityEngine;

public class ConfirmPrompt : MonoBehaviour
{
    private AwaitableCompletionSource<bool> _source = new AwaitableCompletionSource<bool>();

    public Awaitable<bool> WaitForConfirmAsync()
    {
        return _source.Awaitable;
    }

    public void Confirm()
    {
        _source.SetResult(true);
    }
}
```

---

## Related notes
- [[Definition]]
- [[Awaitable]]
- [[AsyncAwait]]
- [[Summary]]
