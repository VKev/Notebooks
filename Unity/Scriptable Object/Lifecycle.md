---
aliases:
  - ScriptableObject Lifecycle
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `ScriptableObject` có lifecycle callbacks hạn chế so với `MonoBehaviour`, chỉ hỗ trợ `Awake`, `OnEnable`, `OnDisable`, `OnDestroy`, và `OnValidate`.

## What is it
- Theo tài liệu Unity `6.3 LTS (6000.3)`, `ScriptableObject` nhận được một số messages từ Unity engine nhưng không hỗ trợ `Update`, `Start`, `LateUpdate`, hay bất kỳ per-frame callback nào.
- Các messages được hỗ trợ gồm `Awake` khi instance được tạo, `OnEnable` khi object load, `OnDisable` khi object unload, `OnDestroy` khi object bị destroy, `OnValidate` chỉ trong Editor khi script reload hoặc Inspector thay đổi giá trị, và `Reset` để đưa về giá trị mặc định.

## How it works
- `Awake` được gọi một lần duy nhất khi instance `ScriptableObject` được tạo lần đầu.
- `OnEnable` được gọi khi object load, khi vào Play Mode, hoặc khi recompile trong Editor.
- `OnDisable` được gọi khi object ra khỏi scope hoặc khi thoát Play Mode.
- `OnDestroy` được gọi ngay trước khi object bị hủy.
- `OnValidate` chỉ chạy trong Editor, thường dùng để validate hoặc clamp data khi thay đổi trong Inspector.
- Vì không có `Update`, mọi logic per-frame cần được điều khiển từ một `MonoBehaviour` bên ngoài.

## Why use it
- Hiểu lifecycle giúp biết chính xác thời điểm init data và cleanup resource.
- `OnEnable` rất phổ biến để reset data về trạng thái ban đầu mỗi khi vào Play Mode.
- `OnValidate` hữu ích để tự động kiểm tra tính hợp lệ của data ngay trong Editor mà không cần chạy game.

## When to use it
- Dùng `OnEnable` để khởi tạo hoặc reset giá trị runtime khi object load.
- Dùng `OnValidate` để clamp giá trị hoặc enforce constraint trong Editor.
- Dùng `OnDisable` và `OnDestroy` để giải phóng resource nếu `ScriptableObject` giữ reference tới unmanaged resource.

## When to not use it
- Không cố gắng đặt logic per-frame trong `ScriptableObject`, vì `Update` không được gọi.
- Không dùng `OnValidate` cho logic runtime, vì nó chỉ chạy trong Editor.

## Limitations
- Không có `Start`, `Update`, `FixedUpdate`, hay `LateUpdate`.
- `OnValidate` là Editor-only, không tồn tại trong build.
- Thứ tự gọi `OnEnable` giữa các `ScriptableObject` không được đảm bảo.

---

## Example code
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

---

## Related notes
- [[Definition]]
- [[CreateAssetMenu]]
- [[Data Persistence]]
- [[Summary]]
