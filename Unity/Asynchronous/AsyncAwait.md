---
aliases:
  - Async/Await
  - Async Await
  - async await trong Unity
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `async/await` là model bất đồng bộ của C# giúp chờ `Task` hoàn thành mà không block thread hiện tại.

## Key points
- `async/await` là cách viết flow bất đồng bộ theo kiểu gần giống code tuần tự.
- Nó đi cùng `Task` hoặc `Task<T>` để biểu diễn công việc và kết quả trả về.
- Khi gặp `await`, method tạm dừng và đăng ký phần còn lại làm continuation.
- Khi `Task` hoàn thành, continuation sẽ resume ở context phù hợp.
- `async` không tự tạo thread mới. Nếu cần background work thì vẫn phải dùng API phù hợp như `Task.Run(...)`.

## Decision rules
- Giúp flow `I/O` hoặc pipeline nhiều bước dễ đọc hơn callback lồng nhau.
- Hợp khi bạn cần `return value`, `try/catch`, hoặc quản lý lỗi rõ ràng.
- Dễ compose nhiều `Task` hơn so với flow thuần frame-based.
- Dùng cho `HTTP request`, `file I/O`, `database`, hoặc các API trả về `Task`.
- Khi bạn cần chờ kết quả rồi tiếp tục xử lý theo logic rõ ràng.
- Không phải lựa chọn tối ưu cho flow gameplay chỉ cần chờ frame hoặc chờ thời gian đơn giản.
- Tránh dùng rồi đụng Unity API từ background thread.
- Tránh xem `async/await` như cách thay thế mọi loại sequencing trong Unity.
- `async` không đồng nghĩa với multithreading.
- Continuation có thể resume ở context khác nếu bạn không kiểm soát rõ nơi bắt đầu flow.
- Nếu lạm dụng `fire-and-forget`, lỗi và lifecycle rất khó theo dõi.

## Example
```csharp
public async Task LoadDataAsync()
{
    await Task.Delay(1000);
    Debug.Log("Loaded");
}
```

## Related notes
- [[Definition]]
- [[Coroutine]]
- [[Awaitable]]
- [[AwaitableCompletionSource]]
- [[Summary]]
