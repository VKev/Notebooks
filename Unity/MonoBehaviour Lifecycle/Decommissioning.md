---
aliases:
  - OnDisable OnDestroy OnApplicationQuit
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Decommissioning` là phần cleanup của lifecycle: `OnDisable` dọn kết nối tạm thời, `OnDestroy` dọn object sắp bị hủy, `OnApplicationPause/Focus/Quit` xử lý app mất focus hoặc đóng.

## Key points
- `OnDisable` chạy khi component bị disable, GameObject/parent bị deactivate, object bị destroy, scene unload, hoặc domain reload.
- `OnDisable` đối xứng với `OnEnable`; đây là nơi chính để unsubscribe event, dừng listener, và trả state tạm.
- `OnDestroy` chạy khi GameObject/component sắp bị destroy, khi scene unload, hoặc khi thoát Play Mode/runtime. Unity chỉ gọi nó cho GameObject đã từng active.
- Trên mobile, hệ điều hành có thể kill app sau khi suspend, nên `OnDestroy`/`OnApplicationQuit` không đáng tin để save dữ liệu quan trọng.
- `OnApplicationQuit` gửi tới active GameObject trước khi app quit; iOS thường suspend thay vì quit.
- Dữ liệu quan trọng nên save khi `OnApplicationPause(true)` hoặc `OnApplicationFocus(false)`, không đợi quit.

## Decision rules
- Subscribe trong `OnEnable`, unsubscribe trong `OnDisable`.
- Trả object về pool bằng API pool, không chỉ `SetActive(false)` rời rạc nếu pool cần bookkeeping.
- Dùng `OnDestroy` cho cleanup cuối: dispose unmanaged resource, remove khỏi static registry, hủy token.
- Save game trên pause/focus loss cho mobile/web; `OnApplicationQuit` chỉ là cơ hội cuối trên platform hỗ trợ.
- Không dựa vào thứ tự `OnDestroy` giữa nhiều object. Manager có thể đã chết trước listener.
- Viết cleanup idempotent: gọi nhiều lần vẫn an toàn.

## Example
```csharp
using UnityEngine;
public class Enemy : MonoBehaviour
{
    public static event System.Action<Enemy> OnEnemyDestroyed;
    void OnEnable()
    {
        EnemyManager.Register(this);
    }
    void OnDisable()
    {
        EnemyManager.Unregister(this);
    }
    void OnDestroy()
    {
        OnEnemyDestroyed?.Invoke(this);
    }
    void OnApplicationQuit()
    {
        PlayerPrefs.SetInt("TotalKills", GameStats.TotalKills);
        PlayerPrefs.Save();
    }
}
```

## Related notes
- [[Definition]]
- [[Execution Order]]
- [[Initialization]]
- [[Update Loop]]
- [[Summary]]
