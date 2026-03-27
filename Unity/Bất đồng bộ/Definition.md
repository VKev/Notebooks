---
aliases:
  - Definition
  - Bất đồng bộ
  - Asynchronous
  - Asynchronous Programming
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- Asynchronous programming là cách tổ chức code để chờ một việc hoàn thành mà không chặn `main thread`, nên game vẫn có thể `render`, nhận `input`, và chạy các hệ thống khác.

## Problem
- Nhiều việc trong game không cần CPU chạy liên tục, mà chủ yếu là chờ: chờ `network response`, chờ `file I/O`, chờ `scene loading`, hoặc chờ một mốc thời gian.
- Nếu code chờ theo kiểu đồng bộ và chặn `main thread`, game sẽ đứng hình, UI không phản hồi, và trải nghiệm bị hỏng.

## Core idea
- Ý chính của bất đồng bộ là tách `waiting` ra khỏi `blocking`.
- Công việc có thể bắt đầu ở hiện tại, nhường control lại cho engine, rồi tiếp tục khi điều kiện phù hợp đã xảy ra.
- Trong Unity, thứ bạn đang chờ thường rơi vào ba nhóm:
  - frame/time
  - task completion
  - Unity async primitive như `AsyncOperation` hoặc `Awaitable`

## Mechanism
- Một flow bất đồng bộ thường có ba pha:
  - bắt đầu công việc
  - tạm dừng tại điểm chờ
  - tiếp tục khi điều kiện hoàn tất
- Tùy công cụ, điều kiện `resume` là khác nhau:
  - `Coroutine`: resume theo frame, thời gian, hoặc điều kiện gameplay
  - `async/await`: resume khi `Task` hoàn thành
  - `Awaitable`: resume theo frame hoặc Unity async API nhưng dùng cú pháp `await`

## When to use
- Dùng khi công việc có thời gian chờ đáng kể hoặc cần flow nhiều bước mà bạn không muốn khóa `main thread`.
- Hợp cho `network`, `loading`, `timing`, `UI sequence`, `scene transition`, hoặc chờ điều kiện gameplay.

## Benefits
- Giữ game responsive trong lúc chờ.
- Flow thường rõ hơn callback hoặc state thủ công.
- Dễ tách phần "đợi cái gì" khỏi phần "làm gì tiếp theo".

## Trade-offs / Limits
- Bất đồng bộ không tự động tạo song song CPU thật.
- Nếu đoạn code nặng vẫn chạy trên `main thread`, frame vẫn có thể bị khựng.
- Khi có nhiều model bất đồng bộ cùng tồn tại, lifecycle và điểm resume có thể khó theo dõi hơn.

## Common mistakes
- Nghĩ rằng "async" luôn có nghĩa là chạy ở `background thread`.
- Dùng bất đồng bộ cho mọi thứ mà không xét xem mình đang chờ frame hay chờ task.
- Trộn `Coroutine`, `Task`, và `Awaitable` bừa bãi làm flow khó debug.

## Example
- Cùng là "đợi", nhưng mỗi model chờ theo một kiểu khác nhau.

```csharp
yield return null;                           // chờ frame sau
await Task.Delay(1000);                      // chờ task timer hoàn thành
await Awaitable.WaitForSecondsAsync(1f);     // chờ theo Unity awaitable API
```

## Related notes
- [[Bất đồng bộ]]
- [[Coroutine]]
- [[Async Await]]
- [[Awaitable]]
- [[Comparison]]
