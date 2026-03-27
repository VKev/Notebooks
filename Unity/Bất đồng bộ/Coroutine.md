---
aliases:
  - Unity Coroutine
  - Coroutine trong Unity
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- Coroutine là hàm cho phép tạm dừng và tiếp tục chạy ở frame sau hoặc sau một khoảng thời gian hay điều kiện; nó không tạo thread mới.

## Problem
- Dùng khi logic có dạng làm A -> chờ -> làm B, hoặc khi muốn trải công việc nhẹ qua nhiều frame thay vì nhét hết vào một frame.
- Nếu không dùng coroutine, bạn thường phải tự giữ timer, state, hoặc bool trong `Update()`, khiến flow khó đọc hơn.

## Core idea
- Ý chính là viết logic tuần tự theo thời gian, còn Unity giữ trạng thái tạm dừng và tiếp tục của hàm cho bạn.

## Mechanism
- Viết hàm `IEnumerator`, gọi bằng `StartCoroutine(...)`, rồi dùng `yield return` để nhường quyền chạy tại một điểm xác định.
- `yield return null` đợi tới frame sau, `WaitForSeconds(t)` đợi theo `scaled time`, còn `WaitUntil(cond)` đợi tới khi điều kiện đúng.

## When to use
- Hợp cho `timing`, `sequence`, `cooldown`, `spawn loop`, `animation flow`, hoặc chờ một điều kiện gameplay/UI.
- Mỗi đoạn giữa hai lần `yield` phải đủ nhẹ vì toàn bộ logic vẫn chạy trên `main thread`.

## Benefits
- Code thường dễ đọc hơn so với việc nhét timer và `state machine` thủ công vào `Update()`.
- Nó diễn tả hành vi theo đúng thứ tự thời gian của hệ thống, nên phù hợp cho flow có chờ đợi.

## Trade-offs / Limits
- Coroutine không tạo song song CPU thật; nếu bên trong có đoạn nặng thì vẫn chặn `main thread`.
- Thời điểm `resume` phụ thuộc frame, và `WaitForSeconds` còn phụ thuộc `Time.timeScale`.

## Common mistakes
- Dùng coroutine cho tác vụ `CPU-heavy` rồi kỳ vọng nó hoạt động như `background thread`.
- Viết loop dài nhưng quên `yield`, làm frame bị khựng.
- Giấu quá nhiều trạng thái trong coroutine khiến flow khó `stop`, `restart`, hoặc `debug`.

## Example
- Ví dụ đơn giản: spawn quái mỗi 1 giây thay vì tự giữ timer trong `Update()`.

```csharp
using System.Collections;
using UnityEngine;

public class Spawner : MonoBehaviour
{
    [SerializeField] private GameObject enemyPrefab;
    [SerializeField] private Transform spawnPoint;

    private void Start()
    {
        StartCoroutine(SpawnLoop());
    }

    private IEnumerator SpawnLoop()
    {
        while (true)
        {
            Instantiate(enemyPrefab, spawnPoint.position, Quaternion.identity);
            yield return new WaitForSeconds(1f);
        }
    }
}
```

## Related notes
- [[Bất đồng bộ]]
- [[Definition]]
- [[Main Thread]]
- [[Awaitable]]
- [[Comparison]]
