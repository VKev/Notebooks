---
aliases:
  - Awake Start OnEnable
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Initialization` gồm `Awake`, `OnEnable`, và `Start`, là ba callback chạy khi `MonoBehaviour` được tạo hoặc kích hoạt lần đầu.

## Key points
- Unity `6.3`: `Awake` được gọi khi script instance load, trước bất kỳ `Start` nào, dùng để thiết lập reference nội bộ.
- `OnEnable` được gọi ngay sau `Awake` và mỗi khi object được bật lại, dùng để đăng ký event hoặc reset state.
- `Start` được gọi một lần duy nhất trước frame `Update` đầu tiên, chỉ khi script enabled, dùng để thiết lập logic phụ thuộc vào object khác đã `Awake`.
- `Awake` chạy cho tất cả object trước khi bất kỳ `Start` nào chạy, đảm bảo mọi reference nội bộ đã sẵn sàng.
- `OnEnable` chạy ngay sau `Awake` nếu object active, và chạy lại mỗi khi `gameObject.SetActive(true)` hoặc `enabled = true`.
- `Start` chạy sau khi tất cả `Awake` hoàn tất, nhưng chỉ một lần duy nhất trong vòng đời script.
- Nếu object bị disable lúc đầu, `Awake` vẫn chạy nhưng `OnEnable` và `Start` bị trì hoãn cho đến khi object active.
- Thứ tự `Awake` giữa các object không được đảm bảo, nên `Awake` chỉ nên thiết lập chính nó, không đọc từ object khác.

## Decision rules
- Tách initialization thành hai bước: `Awake` cho self-setup và `Start` cho cross-object setup giúp tránh null reference.
- `OnEnable` là nơi lý tưởng để subscribe event, đảm bảo cleanup đối xứng với `OnDisable`.
- Dùng `Awake` để cache `GetComponent`, khởi tạo collection, set giá trị mặc định cho field.
- Dùng `Start` để đọc reference từ object khác, gọi method setup phụ thuộc vào hệ thống đã `Awake`.
- Dùng `OnEnable` để subscribe event, bật coroutine, reset state mỗi khi object active.
- Không đọc reference từ object khác trong `Awake`, vì object đó có thể chưa `Awake`.
- Không đặt logic chỉ chạy một lần vào `OnEnable`, vì `OnEnable` chạy lại mỗi khi object bật.
- Thứ tự `Awake` giữa các script là không xác định trừ khi cấu hình `Script Execution Order`.
- `Start` không chạy nếu script bị disable lúc đầu, phải đợi đến khi enabled lần đầu.
- `Awake` chạy ngay cả khi component disabled, miễn là `GameObject` active.

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
