---
aliases:
  - ScriptableObject Runtime Set
  - SO Runtime Set
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Runtime Set` là pattern dùng `ScriptableObject` làm danh sách toàn cục để các object tự đăng ký và hủy đăng ký khi chúng active trong scene.

## Key points
- Theo hướng dẫn kiến trúc `ScriptableObject` của Unity, Runtime Set là `ScriptableObject` chứa danh sách reference tới các object đang tồn tại trong scene.
- Mỗi object tự thêm mình vào set khi `OnEnable` và xóa mình khi `OnDisable`, giúp bất kỳ hệ thống nào cũng có thể truy cập danh sách mà không cần `FindObjectsByType` hay singleton.
- Pattern này là alternative nhẹ hơn singleton để chia sẻ reference toàn cục giữa các hệ thống.
- Tạo một `ScriptableObject` chứa `List<T>` bên trong.
- Mỗi `MonoBehaviour` tham gia set sẽ reference tới asset đó, gọi `Add(this)` trong `OnEnable` và `Remove(this)` trong `OnDisable`.
- Hệ thống cần truy cập danh sách chỉ cần reference cùng asset và duyệt qua `Items`.
- vì `ScriptableObject` tồn tại ngoài scene, danh sách vẫn hoạt động khi chuyển scene miễn là object ở scene mới cũng đăng ký.
- `OnEnable` của asset nên clear list để tránh data cũ từ lần chạy trước tồn tại.

## Decision rules
- Tránh `FindObjectsByType` tốn kém mỗi frame.
- Tránh singleton pattern với các vấn đề khó test và tight coupling.
- Khi nhiều hệ thống cần biết danh sách các object cùng loại đang active, ví dụ tất cả enemy, tất cả collectible, tất cả waypoint.
- Khi bạn muốn thay thế singleton manager bằng cách tiếp cận data-driven.
- Không phù hợp nếu object tạo và hủy với tần suất cao, vì `Add`/`Remove` liên tục trên list có chi phí.
- Nếu object quên gọi `Remove` trong `OnDisable`, list sẽ giữ reference null và gây lỗi khi duyệt.
- Nhiều hệ thống có thể đọc cùng set mà không cần biết nhau.
- Bỏ qua nếu chỉ có một hệ thống duy nhất cần truy cập danh sách, dùng `GetComponentsInChildren` hoặc query đơn giản hơn.

## Example
```csharp
using System.Collections.Generic;
using UnityEngine;
[CreateAssetMenu(fileName = "NewRuntimeSet.asset", menuName = "Game/Runtime Set")]
public class RuntimeSet : ScriptableObject
{
    private readonly List<GameObject> items = new List<GameObject>();
    public IReadOnlyList<GameObject> Items => items;
    public void Add(GameObject obj)
    {
        if (!items.Contains(obj))
            items.Add(obj);
    }
    public void Remove(GameObject obj) => items.Remove(obj);
    void OnEnable() => items.Clear();
}
public class Enemy : MonoBehaviour
{
    public RuntimeSet enemySet;
    void OnEnable() => enemySet.Add(gameObject);
    void OnDisable() => enemySet.Remove(gameObject);
}
public class EnemyCounter : MonoBehaviour
{
    public RuntimeSet enemySet;
    void Update()
    {
        Debug.Log($"Active enemies: {enemySet.Items.Count}");
    }
}
```

## Related notes
- [[Definition]]
- [[Event Channel]]
- [[Data Persistence]]
- [[Summary]]
