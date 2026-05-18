---
aliases:
  - Coroutine trong Unity
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Coroutine` là cách viết flow theo frame hoặc theo thời gian trong Unity mà không tạo thread mới.

## Key points
- `Coroutine` là cơ chế sequencing của Unity dựa trên `IEnumerator`.
- cho phép logic tạm dừng rồi tiếp tục ở frame sau, sau một khoảng thời gian, hoặc khi điều kiện đúng.
- Bạn viết method trả về `IEnumerator` rồi chạy bằng `StartCoroutine(...)`.
- Mỗi lần gặp `yield return`, Unity lưu trạng thái hiện tại và resume ở thời điểm phù hợp.
- `yield return null` đợi frame sau, `WaitForSeconds` đợi theo thời gian, và `WaitUntil` đợi theo điều kiện.

## Decision rules
- Dùng cho `cooldown`, `spawn loop`, `animation flow`, `timing`, và chờ điều kiện gameplay.
- Khi logic cần bám theo player loop của Unity.
- Tránh dùng cho tác vụ `CPU-heavy` vì vẫn chạy trên `main thread`.
- Không phải lựa chọn tốt khi bạn cần `return value`, `try/catch`, hoặc compose nhiều `Task`.
- Không phù hợp nếu bạn thật sự cần background work.
- Giúp viết flow tuần tự kiểu làm A -> chờ -> làm B dễ đọc hơn.
- Giảm việc phải tự quản lý `timer`, `state`, và cờ trong `Update()`.
- Hợp với các hành vi gameplay phụ thuộc frame hoặc thời gian.

## Example
```csharp
private IEnumerator SpawnLoop()
{
    while (true)
    {
        SpawnEnemy();
        yield return new WaitForSeconds(1f);
    }
}
```

## Related notes
- [[Definition]]
- [[AsyncAwait]]
- [[Awaitable]]
- [[AsyncOperation]]
- [[Summary]]
