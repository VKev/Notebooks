---
aliases:
  - Coroutine trong Unity
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Coroutine` là cách viết flow theo frame hoặc theo thời gian trong Unity mà không tạo thread mới.

## What is it
- `Coroutine` là cơ chế sequencing của Unity dựa trên `IEnumerator`.
- Nó cho phép logic tạm dừng rồi tiếp tục ở frame sau, sau một khoảng thời gian, hoặc khi điều kiện đúng.

## How it works
- Bạn viết method trả về `IEnumerator` rồi chạy bằng `StartCoroutine(...)`.
- Mỗi lần gặp `yield return`, Unity lưu trạng thái hiện tại và resume ở thời điểm phù hợp.
- `yield return null` đợi frame sau, `WaitForSeconds` đợi theo thời gian, và `WaitUntil` đợi theo điều kiện.

## Why use it
- Giúp viết flow tuần tự kiểu làm A -> chờ -> làm B dễ đọc hơn.
- Giảm việc phải tự quản lý `timer`, `state`, và cờ trong `Update()`.
- Hợp với các hành vi gameplay phụ thuộc frame hoặc thời gian.

## When to use it
- Dùng cho `cooldown`, `spawn loop`, `animation flow`, `timing`, và chờ điều kiện gameplay.
- Dùng khi logic cần bám theo player loop của Unity.

## When to not use it
- Không nên dùng cho tác vụ `CPU-heavy` vì nó vẫn chạy trên `main thread`.
- Không phải lựa chọn tốt khi bạn cần `return value`, `try/catch`, hoặc compose nhiều `Task`.
- Không phù hợp nếu bạn thật sự cần background work.

## Limitations
- `Coroutine` không tạo parallelism thật.
- Đoạn code giữa hai lần `yield` vẫn có thể làm tụt frame nếu quá nặng.
- Việc stop, restart, hoặc quản lý lifecycle có thể rối nếu object bị disable hoặc destroy.

---

## Example code
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

---

## Related notes
- [[Definition]]
- [[AsyncAwait]]
- [[Awaitable]]
- [[AsyncOperation]]
- [[Summary]]
