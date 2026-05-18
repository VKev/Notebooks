---
aliases:
  - OnDisable OnDestroy OnApplicationQuit
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Decommissioning` gồm `OnDisable`, `OnDestroy`, và `OnApplicationQuit`, là các callback chạy khi object bị tắt, hủy, hoặc application đóng.

## Key points
- Unity `6.3`: `OnDisable` được gọi khi object hoặc component bị disable, dùng để unsubscribe event và dọn dẹp tạm thời.
- `OnDestroy` được gọi khi object bị hủy hoàn toàn bởi `Destroy()` hoặc khi scene unload, dùng để cleanup resource cuối cùng.
- `OnApplicationQuit` được gọi trước khi application tắt, dùng để lưu data hoặc đóng kết nối.
- `OnDisable` chạy mỗi khi `gameObject.SetActive(false)` hoặc `enabled = false`, và đối xứng với `OnEnable`.
- `OnDestroy` chạy một lần duy nhất khi object bị hủy, sau `OnDisable`.
- `OnApplicationQuit` chạy trước khi application đóng, trước `OnDestroy` của các object còn sống.
- Khi chuyển scene, object không có `DontDestroyOnLoad` sẽ nhận `OnDisable` → `OnDestroy`.
- Trong Editor, thoát Play Mode cũng kích hoạt `OnDisable` → `OnDestroy`.

## Decision rules
- Đảm bảo cleanup đối xứng với initialization: subscribe trong `OnEnable` thì unsubscribe trong `OnDisable`.
- Tránh memory leak và null reference do event listener còn tồn tại sau khi object bị hủy.
- `OnApplicationQuit` cho phép lưu trạng thái cuối cùng trước khi tắt game.
- Dùng `OnDisable` để unsubscribe event, dừng coroutine, release pooled object.
- Dùng `OnDestroy` để dispose unmanaged resource, remove khỏi static list, cleanup final.
- Dùng `OnApplicationQuit` để save game state, flush log, đóng network connection.
- Không đặt logic quan trọng chỉ trong `OnApplicationQuit`, vì trên mobile callback này không được đảm bảo chạy khi app bị kill.
- Không gọi `Destroy` trên object khác trong `OnDestroy`, vì thứ tự destroy không xác định.
- `OnApplicationQuit` không đáng tin cậy trên iOS và Android khi user swipe kill app.
- Thứ tự `OnDestroy` giữa các object là không xác định.
- `OnDisable` chạy trước `OnDestroy`, nên resource cleanup cuối cùng nên đặt trong `OnDestroy`.

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
