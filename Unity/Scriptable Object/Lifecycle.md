---
aliases:
  - ScriptableObject Lifecycle
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `ScriptableObject` có lifecycle callbacks hạn chế so với `MonoBehaviour`, chỉ hỗ trợ `Awake`, `OnEnable`, `OnDisable`, `OnDestroy`, và `OnValidate`.

## Key points
- Unity `6.4 (6000.4)`: `ScriptableObject` nhận được messages từ Unity engine nhưng không hỗ trợ `Update`, `Start`, `LateUpdate`, hay bất kỳ per-frame callback nào.
- Message runtime: `Awake`, `OnEnable`, `OnDisable`, và `OnDestroy`.
- Message Editor: `OnValidate` khi script reload/Inspector đổi giá trị, và `Reset` để đưa về giá trị mặc định.
- `Awake` được gọi một lần duy nhất khi instance `ScriptableObject` được tạo lần đầu.
- `OnEnable` được gọi khi object load, khi vào Play Mode, hoặc khi recompile trong Editor.
- `OnDisable` được gọi khi object ra khỏi scope hoặc khi thoát Play Mode.
- `OnDestroy` được gọi ngay trước khi object bị hủy.
- `OnValidate` chỉ chạy trong Editor, dùng để validate hoặc clamp data khi thay đổi trong Inspector.
- Vì không có `Update`, mọi logic per-frame cần được điều khiển từ một `MonoBehaviour` bên ngoài.

## Decision rules
- Dùng `OnEnable` để khởi tạo hoặc reset giá trị runtime khi object load.
- Dùng `OnValidate` để clamp giá trị hoặc enforce constraint trong Editor.
- Dùng `OnDisable` và `OnDestroy` để giải phóng resource nếu `ScriptableObject` giữ reference tới unmanaged resource.
- Không cố gắng đặt logic per-frame trong `ScriptableObject`, vì `Update` không được gọi.
- Không dùng `OnValidate` cho logic runtime, vì chỉ chạy trong Editor.
- Không có `Start`, `Update`, `FixedUpdate`, hay `LateUpdate`.
- Hiểu lifecycle giúp biết chính xác thời điểm init data và cleanup resource.
- `OnEnable` phổ biến để reset data về trạng thái ban đầu mỗi khi vào Play Mode.

## Example
```csharp
using UnityEngine;
[CreateAssetMenu(fileName = "PlayerData.asset", menuName = "Game/Player Data")]
public class PlayerData : ScriptableObject
{
    public float maxHealth = 100f;
    [HideInInspector]
    public float currentHealth;
    void OnEnable()
    {
        currentHealth = maxHealth;
    }
    void OnValidate()
    {
        if (maxHealth < 0f)
            maxHealth = 0f;
    }
}
```

## Related notes
- [[Definition]]
- [[CreateAssetMenu]]
- [[Data Persistence]]
- [[Summary]]
