---
aliases:
  - Async/Await
  - Async Await
  - C# Async Await
  - Async Await in CSharp
  - async await trong C#
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `async/await` là model bất đồng bộ của C# cho phép bạn chờ `Task` hoàn thành mà không block thread; compiler sẽ biến flow này thành `state machine`.

## Problem
- Nếu không có `async/await`, code bất đồng bộ dễ rơi vào callback lồng nhau hoặc phải tự quản lý state để biết khi nào tiếp tục bước kế tiếp.
- Các tác vụ như `HTTP`, `file I/O`, `database`, hoặc pipeline dài thường cần flow rõ ràng và khả năng trả về kết quả.

## Core idea
- Bạn viết code theo dạng gần giống tuần tự: làm A, `await`, rồi làm B.
- Khi gặp `await`, function tạm dừng, trả control về caller hoặc engine, và chỉ resume khi `Task` đã hoàn thành.
- `async` không tự tạo thread mới; nó chỉ cho phép method sử dụng `await`.

## Mechanism
- Method bất đồng bộ thường trả về `Task` hoặc `Task<T>`.
- `await` đăng ký phần tiếp theo của method làm continuation.
- Compiler generate `state machine` để lưu trạng thái local variable, vị trí đang dừng, và điểm cần resume.
- Muốn chuyển việc nặng sang `background thread`, bạn vẫn cần API phù hợp như `Task.Run(...)`.
- Trong Unity, nếu bạn gọi một `Task` từ `main thread`, continuation thường quay lại `UnitySynchronizationContext` và resume ở `Update` tick kế tiếp của `main thread`.
- Nếu async flow bắt đầu từ background thread, continuation có thể tiếp tục ở `ThreadPool` thay vì tự quay về `main thread`.

## When to use
- Hợp cho `HTTP request`, `file I/O`, `database`, hoặc task dài mà kết quả được báo bằng `Task`.
- Hợp khi bạn cần `return value`, `try/catch`, `CancellationToken`, hoặc compose nhiều task với nhau.
- Ít phù hợp hơn cho flow gameplay thuần frame-based như chờ `next frame` hoặc `animation timing`.

## Benefits
- Viết flow task-based gọn và dễ đọc hơn callback.
- Dễ trả về dữ liệu với `Task<T>`.
- `try/catch` và `CancellationToken` dùng tự nhiên hơn coroutine.

## Trade-offs / Limits
- `async/await` không tự đồng bộ với `game loop`.
- Trong Unity, continuation của `Task` có thể quay về `main thread` ở frame kế tiếp, nên có thêm latency so với `Awaitable`.
- Nếu resume ở sai context hoặc bạn đụng Unity API từ `background thread`, bug sẽ rất khó chịu.
- `fire-and-forget` làm việc theo kiểu `_ = SomeTask()` thường khiến lỗi bị nuốt hoặc lifecycle khó kiểm soát.

## Common mistakes
- Nghĩ rằng `async` luôn chạy ở thread khác.
- Dùng `Task.Delay(...).Wait()` hoặc `.Result`, vô tình block thread.
- Đụng `UnityEngine` API bên trong `Task.Run(...)`.
- Dùng quá nhiều `fire-and-forget` mà không quản lý lỗi và hủy.
- Quên rằng `Task` là abstraction của work và continuation, không phải thread.

## Example
- Ví dụ đơn giản của task-based async trong C#.

```csharp
using System.Threading.Tasks;
using UnityEngine;

public class Loader : MonoBehaviour
{
    public async Task LoadData()
    {
        Debug.Log("Start");

        await Task.Delay(2000);

        Debug.Log("Done after 2s");
    }
}
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Task]]
- [[ThreadPool]]
- [[Main Thread]]
- [[UnitySynchronizationContext]]
- [[Coroutine]]
- [[Awaitable]]
- [[Comparison]]
