---
aliases:
  - Awake Start OnEnable
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Initialization` chia setup thành ba lớp: `Awake` chuẩn bị chính object, `OnEnable` bật kết nối tạm thời, `Start` nối với object khác sau khi mọi `Awake` đã xong.

## Key points
- `Awake` chạy một lần cho mỗi script instance khi Unity load/activate instance lần đầu. Component disabled vẫn có thể nhận `Awake` nếu GameObject active.
- `Awake` không có thứ tự xác định giữa các GameObject, nên không dùng để đọc state cần object khác setup trước.
- `OnEnable` chạy sau `Awake`, trước `Start`, và chạy lại mỗi lần component/GameObject active lại.
- `Start` chạy một lần, trước `Update` đầu tiên, chỉ khi component enabled.
- Unity đảm bảo mọi `Awake` trong scene xong trước khi bất kỳ `Start` nào chạy.
- Object inactive từ đầu sẽ trì hoãn `Awake`/`OnEnable`/`Start` cho đến lần active đầu tiên.
- `Awake` và `OnEnable` không thể là coroutine; `Start` có thể là coroutine.

## Decision rules
- `Awake`: Cache component, tạo collection, set invariant nội bộ.
- `OnEnable`: Subscribe event, start listen, reset state mỗi lần object được bật.
- `Start`: Lấy reference tới manager/object khác, gọi setup cần hệ thống khác đã init.
- Subscribe trong `OnEnable` thì unsubscribe trong `OnDisable`.
- Không đặt logic chỉ được chạy một lần trong `OnEnable`; callback này chạy lại nhiều lần.
- Không dùng constructor cho serialized state; dùng `Awake` vì Unity chưa đảm bảo serialized data trong constructor.

## Example
```csharp
using UnityEngine;
public class Player : MonoBehaviour
{
    private Rigidbody _rb;
    private GameManager _gm;
    void Awake()
    {
        _rb = GetComponent<Rigidbody>();
    }
    void OnEnable()
    {
        GameEvents.OnLevelStart += HandleLevelStart;
    }
    void Start()
    {
        _gm = FindFirstObjectByType<GameManager>();
        _gm.RegisterPlayer(this);
    }
    void OnDisable()
    {
        GameEvents.OnLevelStart -= HandleLevelStart;
    }
    void HandleLevelStart() => _rb.linearVelocity = Vector3.zero;
}
```

## Related notes
- [[Definition]]
- [[Execution Order]]
- [[Update Loop]]
- [[Decommissioning]]
- [[Summary]]
